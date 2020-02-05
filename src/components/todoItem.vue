<template>
<div>
  <input type="checkbox" :checked="status">
  <h5>{{ name }}</h5>
  
</div>
</template>

<script>
export default {
  name: 'tdItem',
  props: ['name', 'status'],
  data () {
    return {
      name: '',
      status: false
    }
  }
}
</script>

<style>
input[type='checkbox'] {
  float: left;
}

</style>
