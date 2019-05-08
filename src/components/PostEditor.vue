<template>
  <form @submit.prevent="save">
    <div class="form-group">
      <textarea name id cols="30" rows="10" class="form-input" v-model="text"></textarea>
    </div>
    <div class="form-actions">
      <button @click.prevent="cancel" class="btn btn-ghost">Cancel</button>
      <button class="btn-blue">{{isUpdate ? 'Update' : 'Submit post'}}</button>
    </div>
  </form>
</template>

<script>
export default {
  props: {
    threadId: {
      required: false
    },
    // custom validator which ensures we get the right attributes are being passed
    //  We want both a key and text attribute which are both strings
    post: {
      type: Object,
      validator: obj => {
        const keyIsValid = typeof obj['.key'] === 'string'
        const textIsValid = typeof obj.text === 'string'
        const valid = keyIsValid && textIsValid

        if (!valid) {
          console.error('The post object must contain .key and text attributes')
        }
        return valid
      }
    }
  },

  data () {
    return {
      text: this.post ? this.post.text : ''
    }
  },

  computed: {
    isUpdate () {
      return !!this.post
    }
  },

  methods: {
    save () {
      this.persist()
        // signal the parent so it can toffle between edit and show post
        // parent component listens for a save event with @save
        .then(post => {
          this.$emit('save', {post})
        })
    },

    cancel () {
      // parent component listens for the @cancel event
      this.$emit('cancel')
    },

    create () {
      const post = {
        text: this.text,
        threadId: this.threadId
      }

      this.text = ''
      return this.$store.dispatch('createPost', post)
    },

    update () {
      const payload = {
        id: this.post['.key'],
        text: this.text
      }
      return this.$store.dispatch('updatePost', payload)
    },

    persist () {
      return this.isUpdate ? this.update() : this.create()
    }
  }
}
</script>

<style scoped>
</style>
