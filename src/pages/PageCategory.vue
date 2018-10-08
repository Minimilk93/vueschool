<template>
  <div class="col-full">
    <div class="col-full push-top">
      <h1>{{category.name}}</h1>
      <CategoryListItem :category="category" />
    </div>
  </div>
</template>

<script>
  import CategoryListItem from '@/components/CategoryListItem'
  import sourceData from '@/data.json'

  export default {
    components: {
      CategoryListItem
    },
    props: {
      id: {
        required: true,
        type: String
      }
    },
    computed: {
      category () {
        return sourceData.categories[this.id]
      }
    }
  }
</script>

<style scoped>
</style>