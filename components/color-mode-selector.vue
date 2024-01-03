<template>
  <div>
    <button @click="changeMode($event)">
    <div class="tooltip">
      <img :src="`/images/${mode}`" class="dark w-10 md:w-6"  />
      <span class="tooltiptext darkMod">Switch to {{ color }} mood</span>
    </div>
    </button>
  </div>
</template>

<script setup>
  const colorMode = useColorMode()
  const color = ref('light')
  colorMode.preference = 'dark'
  const mode = ref('sun.png')


  const changeMode = (e) => {
    if (e.target.className === 'dark w-10 md:w-6') {
      colorMode.preference = 'light'
      e.target.className = 'light w-10 md:w-6'
      e.target.nextElementSibling.className = 'tooltiptext lightMod'
      mode.value = 'moon.png'
    } else if (e.target.className === 'light w-10 md:w-6') {
      colorMode.preference = 'dark'
      e.target.className = 'dark w-10 md:w-6'
      e.target.nextElementSibling.className = 'tooltiptext darkMod'
      mode.value = 'sun.png'
    }
    if (colorMode.preference === 'dark') {
    color.value = 'light'
    } else {
      color.value = 'dark'
    }
  }
</script>

<style scoped>
  img {
    display: inline-block;
  }
  .light {
    color: rgb(233, 217, 35);
    /* width: 20px;
    height: 20px; */
  }
  .dark {
    color: rgb(49, 46, 175);
    /* width: 20px;
    height: 20px; */
  }

  /* Tooltip container */
.tooltip {
  position: relative;
  display: inline-block;
}

/* Tooltip text */
.tooltip .tooltiptext {
  visibility: hidden;
  font-size: 14px;
  width: 120px;
  text-align: center;
  padding: 5px 5px;
  border-radius: 6px;
  /* Position the tooltip text */
  position: absolute;
  z-index: 1;
  top: 30px;
  right: -50px;
}

.darkMod {
  background-color: rgba(162, 159, 159, 0.627);
  color: #fff;
}

.lightMod {
  background-color: rgba(212, 211, 211, 0.627);
  color: rgb(89, 87, 87);
}

/* Show the tooltip text when you mouse over the tooltip container */
.tooltip:hover .tooltiptext {
  visibility: visible;
}
</style>