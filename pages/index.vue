<template>
  <div id="container">
    <h1 class="mont title">{{title}}</h1>
    <h2 class="mont sub-title">A web app by Jonathan Knoll</h2>
    <intro></intro>
    <img class="dots" src="/dots.svg"/>
    <img class="triangle" src="/triangles.svg"/>
  </div>
</template>

<script>
import Intro from '~/components/Intro.vue'

export default {
  components: {
    Intro
  },

  data() {
      return {
        title: 'Change Blindness'
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
       { rel: 'stylesheet', href: '/global.css' },
       { rel: 'icon', type: 'image/x-icon', href: '/favi.png' }
      ],
    }
  }
}
</script>

<style>
.mont {
  font-family: 'Montserrat', sans-serif;
}
.nun {
  font-family: 'Nunito', sans-serif;
}
:root {
  --color-blue:#3D6FB1;
  --color-pink:#CB8BB3;
}

#container {
  min-height:100vh;
  min-width:100vw;
  background:#E5E5E5;
}


.dots {
  position:fixed;
  top:0;
  right:0;
}
.triangle {
  position:fixed;
  bottom:1rem;
  left:1rem;
}

.title {
  padding-top:1rem;
  margin-left:1rem;
}
.sub-title {
  margin-left:1rem;
  font-size:1em;
  font-weight:600;
  font-variant: small-caps;
}


</style>
