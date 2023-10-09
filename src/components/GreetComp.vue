<template>
    <div>
    <h1>Hello {{name}}</h1>
    </div>
</template>

<script>
export default {
  name: 'GreetComp',
  props: {
    name: String
  }
}
</script>

<style scope>
  h1{
    color: blue;
  }
</style>