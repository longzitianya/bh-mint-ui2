<template>
  <div class="cube-slide-item">
    <slot>
      <a :href="item.url">
        <img :src="item.image">
      </a>
    </slot>
  </div>
</template>

<script type="text/ecmascript-6">
  const COMPONENT_NAME = 'cube-slide-item'
  export default {
    name: COMPONENT_NAME,
    props: {
      item: {
        type: Object,
        default() {
          /* istanbul ignore next */
          return {}
        }
      }
    }
  }
</script>

<style>
  .cube-slide-item {
    float: left;
    box-sizing: border-box;
    height: 100%;
  }
  .cube-slide-item > a {
    display: block;
    height: 100%;
    overflow: hidden;
    text-decoration: none;
  }  
  .cube-slide-item > a > img {
    display: block;
    height: 100%;
  }  
</style>