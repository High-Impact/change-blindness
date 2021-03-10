<template>
  <div class="intro">
    <div class="nun" v-if="counter == 0">
      <h1 class="mont">Introduction</h1>
      <br>
      <div class="quoteMark text-blue-200"><svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="quote-left" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="svg-inline--fa fa-quote-left fa-w-16"><path fill="currentColor" d="M464 256h-80v-64c0-35.3 28.7-64 64-64h8c13.3 0 24-10.7 24-24V56c0-13.3-10.7-24-24-24h-8c-88.4 0-160 71.6-160 160v240c0 26.5 21.5 48 48 48h128c26.5 0 48-21.5 48-48V304c0-26.5-21.5-48-48-48zm-288 0H96v-64c0-35.3 28.7-64 64-64h8c13.3 0 24-10.7 24-24V56c0-13.3-10.7-24-24-24h-8C71.6 32 0 103.6 0 192v240c0 26.5 21.5 48 48 48h128c26.5 0 48-21.5 48-48V304c0-26.5-21.5-48-48-48z"></path></svg></div>
      <p class="quote">Change Blindness is a perceptual phenomenon that occurs when a change in a visual stimulus is introduced and the observer does not notice it.</p>
      <br>
      <p>Outside of academic nonsense, our brains can be kinda stupid, something can be changing right in front of us, and <i>we have no idea</i>...</p>
      <br>
      <p><strong>The reason</strong> for this is that only a small amount of our vision is actually in focus, the rest of what we see is just our brain making it's best assumption based on a very low-resolution image.  </p>
      <br>
      <p>To see how much of your vision is actually in focus, give me a thumbs up, and then reach your arm all the way out. <i>Now look at your thumbnail.</i> That’s <strong>all you see in focus.</strong></p>
      <br>
      <i>*Clearly there's a lot more to this, but that's the basic idea. Now let's head over to the next page to understand what's going to be happening.</i>
      <br><br>
    </div>
    <div class="nun" v-if="counter == 1">
      <h1 class="mont">The Exercise</h1>
      <br>
      <p><a href="https://search.bwh.harvard.edu/new/CBDatabase.html" target="blank">Harvard’s Visual Attention Lab</a> has been kind enough to prepare a bunch of images for this exact purpose. They have a set of images that have minor changes in them, <strong>the change could be an umbrella’s color, or an object removed from the photo.</strong> Your goal is to spot the change as quickly as you can.</p>
      <br>
      <p>The exploitation is that each image will only be shown for 0.25 seconds, then a mask will appear for 0.3 seconds in between each photo.</p>
      <br>
      <strong>Example:</strong>
      <img src="/example-2.png"/>
      <br><br>
      <p>Next, I’m going to walk you through how to use the app.</p>
      <br><br>
    </div> 
    <div class="nun" v-if="counter == 2">
      <h1 class="mont">Instructions</h1>
      <br>
      <p>I wanted to keep this app as simple as possible to use. On the exercise page, there is a navigation side bar where you can select different photo sets. It defaults to the first set. </p>
      <br>
      <p>To start the exercise, simply press the spacebar <img style="height:1em;" src="/spacebar.png"/>, then when you’ve found the change press the spacebar <img style="height:1em;" src="/spacebar.png"/> again, and it will log your time.</p>
      <br>
      <p>If you’ve been staring at the screen and just <strong>CANNOT</strong> find it, turn on the cheats.  What this will do is flash the images with no mask, which makes it really easy to spot the difference.</p>
      <br>
      <p>Hope you enjoy! 🤓</p>
      <br>
      <p>Cheers,<br>Jonathan</p>
      <br><br>
    </div>
    <div class="buttons">
      <button :class="{disabled: counter == 0}" @click="back">Back</button>
      <button :class="{disabled: counter == 2}" v-if="counter != 2" @click="next">Next</button>
      <NuxtLink class="start" v-if="counter == 2" to="/exercise">Let's start</NuxtLink>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      counter:0
    }
  },
  methods: {
    next: function() {
      if (this.counter <= 1) {
        this.counter++
      }
    },
    back: function() {
      if (this.counter > 0) {
        this.counter--
      }
    }
  }
}
</script>

<style>
.intro {
  background:white;
  color:var(--color-dark);
  width:650px;
  padding:2em;
  max-width:90%;
  position:absolute;
  left:50%;
  top:50%;
  transform:translate(-50%, -50%);
  border:4px solid var(--color-blue);
  box-shadow:-1em 1em 0 var(--color-pink);
}
img {
  max-width:100%;
}
.quote {
  font-size:1.2em;
  font-weight:600;
}
.quoteMark {
    width: 4em;
    margin-bottom: -3em;
    margin-left: -1em;
    opacity: .2;
    z-index: -1;
    position: relative;
}
.buttons {
}
.start {
  color:var(--color-pink);
  font-variant: all-small-caps;
  font-weight:900;
  font-size:2em;
  text-decoration: none;
  transition:300ms;
  padding: 0;
  line-height: 0;
  vertical-align: middle;
  margin-left:.5em;
}
.start:hover {
  transform:scale(1.01);
  opacity: .7;
}
.start:after {
  content:'';
  height:.5em;
  width:.5em;
  margin-left:.25em;
  background:url('/right.svg');
  background-size:cover;
  display: inline-block;
}

</style>
