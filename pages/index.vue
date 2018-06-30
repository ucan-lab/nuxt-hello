<template>
  <section>
    <h1>Hello Nuxt!!</h1>
    <h2>routing</h2>

    <ul>
      <li>
        <nuxt-link to="/articles">/articles</nuxt-link> => pages/articles/index.vue
        <ul>
          <li><nuxt-link to="/articles/1">/articles/1</nuxt-link> => pages/articles/_id.vue</li>
          <li><nuxt-link to="/articles/123">/articles/123</nuxt-link> => pages/articles/_id.vue</li>
        </ul>
      </li>
      <li><nuxt-link to="/ucan-lab/profile">/ucan-lab/profile</nuxt-link> => pages/_user/profile.vue</li>
      <li><nuxt-link to="/taniguchi/profile">/taniguchi/profile</nuxt-link> => pages/_user/profile.vue</li>
    </ul>

    <h2>vue-good-table plugin</h2>

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
