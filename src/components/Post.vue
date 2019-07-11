<template>
  <v-layout py-4 h-100>
    <v-flex row>
      <div class="caption">{{formatedDate}}
      <h3 class="color-333 headline font-weight-light">{{title}}</h3>
      <p v-line-clamp="4" class="mb-1 color-666 font-weight-light subheading">{{body}}</p>
      </div>
    </v-flex>
  </v-layout>
</template>

<script>

export default {
	name: 'Post',
	props: {
		date: {type: Date},
		title: {type: String},
		body: {type: String}
	},
  computed: {
		formatedDate() {
			return `${this.date.getFullYear()}년 ${this.date.getMonth()}월 ${this.date.getDate()}일`
    }
  }
}
</script>

<style>
  .color-666 {
    color: #666;
  }
  .color-333 {
    color: #333;
  }

  .h-100 {
    height: 100%;
  }
</style>
