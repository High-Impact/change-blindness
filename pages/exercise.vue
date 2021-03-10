<template>
    <div id="container" class="parent"> 
        <div class="nav">
            <h2 class="mont">Photo sets</h2>
            <ul>
                <li 
                    v-for="set in photoSet" 
                    :key="set._id"
                    @click="setUpViewer(set._id)"
                    :class="{active: id == set._id}"
                >
                    <img :src="prefixURL(set.photo_a.path)" />
                    <div v-if="!loadingTimes" class="content nun">
                        <span class="title">Average:</span>
                        <br>
                        <span v-if="createTimesArray(set._id).length > 0">
                            {{arrayAverage(createTimesArray(set._id))}} seconds
                        </span>
                        <span v-else>
                            No times yet.
                        </span>

                    </div>
                </li>
            </ul>
            <NuxtLink to="/" class="home">Go Home</NuxtLink>
        </div>
        <div class="body">
            <img class="dots" src="/dots.svg"/>
            <img class="triangle" src="/triangles.svg"/>
            <div class="viewer" v-if="!loading">
                <div class="images">
                    <img class="f" :class="{hide: image != 'f' }" src="/fill.png" />
                    <img class="a" :class="{hide: image != 'a' }" :src="prefixURL(currentSet.photo_a.path)" />
                    <img class="b" :class="{hide: image != 'b' }" :src="prefixURL(currentSet.photo_b.path)" />
                </div>
                <div class="meta">
                    <div class="timer">
                        <span class="nun timerText">Timer: </span>
                        <span v-if="timePassed">{{timePassed.toString().slice(0, -2)}}.{{timePassed.toString().slice(-2)}}</span>
                        <span v-else>0.0</span>
                        <br>
                        <br>
                        <button @click="run()">
                            <span v-if="!timerRunning">Start</span>
                            <span v-else>Stop</span>
                        </button>
                    </div>
                    <div class="toggle">
                    </div>
                    <div class="cheats">
                        <span class="cheatsText">Cheats are: </span><span v-if="cheats">ON</span> <span v-else>OFF</span>
                        <br>
                        <br>
                        <button @click="cheats = !cheats">Toggle Cheats</button>
                        
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return{
            photoSet: Array,
            photoTimes: Array,
            id: String,
            currentSet: null,
            loading:true,
            loadingTimes:true,
            image:'a',
            timerRunning:false,
            timerInterval: null,
            timePassed:false,
            cheats:false,
            title:"The Excercise"
        }   
    },  
    beforeMount: function() {
        fetch('https://highimpact.design/cockpit/api/collections/get/photoset?token=9e74f39939395db8bb25d95f563a52&sort[position]=1')
            .then(response => response.json())
            .then(data => {
                this.photoSet = data.entries,
                this.id = data.entries[0]._id,
                this.createCurrentSet();
                this.loading = false,
                console.log(this.photoSet);
                console.log(this.id);
                

            })
            .catch((error) => {
                console.error('Error:', error);
            });
        fetch('https://highimpact.design/cockpit/api/collections/get/phototimes?token=9e74f39939395db8bb25d95f563a52&sort[position]=1')
            .then(response => response.json())
            .then(data => {
                this.photoTimes = data.entries,
                this.loadingTimes = false
            })
            .catch((error) => {
                console.error('Error:', error);
            });
    },
    mounted: function () {
        this.$nextTick(function () {
            window.addEventListener('keydown', event => {
                if(event.key === ' ') {
                    event.preventDefault()
                    this.run();     
                }
            })
        })
    },
    methods: {
        prefixURL: function(url) {
            return "https://highimpact.design" + url;
        },
        setUpViewer: function(id) {
            this.id = id;
            this.createCurrentSet();
        },
        createCurrentSet: function() {
            this.currentSet = this.photoSet.find(x => x._id === this.id)
        },
        createTimesArray: function(id) {
            var times = this.photoTimes.filter(x => x.photoSet === id);
            return times;
        },
        run: function() {
            if (this.timerRunning) {
                clearInterval(this.timerInterval);
                this.timerRunning = false;
                this.image="a";
                this.photoTimes.push({"photoSet": this.id,"time": this.timePassed,})
                fetch(`https://highimpact.design/cockpit/api/collections/save/phototimes?token=9e74f39939395db8bb25d95f563a52&data[photoSet]=${this.id}&data[time]=${this.timePassed}`)
                    .then(response => response.json())
                    .then(data => (
                        console.log(data),
                        console.log('sent')
                ));
            } else {
                this.timePassed = 0;
                this.timerInterval = setInterval(() => (this.interval()), 10);
                this.timerRunning = true;
            }
        },
        interval: function() {
            if (this.cheats) {
                var intervalCount = this.timePassed / 50;
                var bonus = Math.floor(intervalCount) * 50;
                var x = this.timePassed - bonus;

                if(x <= 25) {
                    this.image = 'a';
                } else if (x <= 50) {
                    this.image = 'b';
                }   

            } else {
                var intervalCount = this.timePassed / 100;
                var bonus = Math.floor(intervalCount) * 100;
                var x = this.timePassed - bonus;

                if(x <= 25) {
                    this.image = 'a';
                } else if (x <= 55) {
                    this.image = 'f';
                } else if (x <= 80) {
                    this.image = 'b';
                } else if (x <= 110) {
                    this.image = 'f';
                }   
            }
            this.timePassed += 1;
        },
        arrayAverage: function(data) {
            var sum = 0;
            console.log(data.length)
            for (let index = 0; index < data.length; index++) {
                sum += Number(data[index].time)
            }
            console.log(sum)
            var avg = Math.floor(sum / data.length) 
            return '' + avg.toString().slice(0, -2) + '.' + avg.toString().slice(-2);

        }
    },
    head() {
        return {
        title: this.title,
        meta: [
            // hid is used as unique identifier. Do not use `vmid` for it as it will not work
            {
            hid: 'description',
            name: 'description',
            content: 'A simple web app to help illustrate a weakness in our perceived consciousness'
            }
        ],
        link: [      
            { rel: 'preconnect', href: 'https://fonts.gstatic.com' },
            { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,400;0,700;0,900;1,400;1,700;1,900&family=Nunito:ital,wght@0,400;0,700;0,900;1,400;1,700;1,900&display=swap' },
            { rel: 'stylesheet', href: '/global.css' }
        ],
        }
    }
}
</script>

<style scoped>
    .triangle {
        left:21em !important;
        z-index:0;
    }
    .dots {
        z-index:0;
    }
    .parent {
        display:flex;
    }
    .nav {
        width:20em;
        background:white;
        height:100vh;
        overflow-y:scroll;
        padding:2em;
    }
    .nav ul {
        margin:0;
        padding:0;
        list-style:none;
        margin-top:1rem;
    }
    .nav ul li {
        opacity:.5;
        margin-bottom:1rem;
        transition:300ms;
        display:flex;
        flex-wrap:wrap;
        justify-content:space-between;
        cursor:pointer;
    }
    .nav ul li.active, .nav ul li:hover {
        opacity:1;
        transform: scale(1.05);
    }
    .nav ul li img {
        width:30%;
        height: auto;
        object-fit: cover;
        border-radius: .3em;
        overflow: hidden;
        box-shadow:0px 5px 9px -5px rgba(12, 23, 37, .42);
    }
    .nav ul li .content {
        width: 70%;
        padding:1em;
    }
    .nav ul li .content .title,
    .timerText,
    .cheatsText {
        text-transform:uppercase;
        font-size:.75em;
        font-weight:900;
        margin:0;
    }

    .body {
        width:calc(100vw - 20em);
        display:flex;
        justify-content: center;
        align-items:center;
    }
    .viewer {
        background:white;
        width:700px;
        padding:2em;
        max-width:90%;
        border:4px solid var(--color-blue);
        box-shadow:-1em 1em 0 var(--color-pink);
        z-index:2;
    }
    .viewer img {
        width:100%;
        display:inline-block;
        position:relative;
        display: inline-block;
        border-radius:.3em;
        box-shadow:0px 5px 9px -5px rgba(12, 23, 37, .42);
        margin-bottom:1em;
    }
    .viewer img.hide {
        display:none;
    }
    .viewer .meta {
        display:flex;
        justify-content: space-between;
        align-items:center;
    }
</style>