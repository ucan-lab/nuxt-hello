<template>
  <section>
    <h1>Hello Nuxt!!</h1>
    <ul>
      <li><nuxt-link to="/vue-good-table">vue-good-table sample 1</nuxt-link></li>
      <li><nuxt-link to="/vue-good-table2">vue-good-table sample 2</nuxt-link></li>
    </ul>
  </section>
</template>

<style src="~/assets/sample.scss"></style>

<script>
export default {
  layout: "article_layout",
  middleware: ["visit"]
}
</script>
