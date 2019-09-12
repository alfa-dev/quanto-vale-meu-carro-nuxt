<template>
  <div class="container">
    <h1 class="title"></h1>
    <h2>{{ result.modelo }}</h2>

    <dl>
      <dt>Marca</dt>
      <dd>{{ result.marca }}</dd>
      <dt>ano</dt>
      <dd>{{ result.ano }}</dd>
      <dt>Combustível</dt>
      <dd>{{ result.combustivel }}</dd>
    </dl>

    <p>
      <strong>Preço</strong> <br />
      <span class="subtitle">
        {{ result.preco }}
      </span>
    </p>

    <a href="/brands" disabled>Consultar novamente</a>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      result: {}
    }
  },
  created() {
    const brandId = this.$route.query.brand_id
    const modelId = this.$route.query.model_id
    const yearId = this.$route.query.year_id

    axios
      .get(
        `http://api.fipeapi.com.br/v1/carros/${brandId}/${modelId}/${yearId}?9c86d2c2155102bfffbab7d069036e11`
      )
      .then((response) => {
        this.result = response.data

        setTimeout(() => {
          this.makeToast()
        }, 3000)
      })
  },
  methods: {
    onSubmit(e) {
      e.preventDefault()

      const brandId = this.$route.query.brand_id
      const modelId = this.$route.query.model_id

      if (this.options.includes(this.brand)) {
        const selectedModel = this.brands.filter(
          (brand) => brand.name === this.brand
        )[0]

        this.$router.push(
          `result?brand_id=${brandId}&model_id=${modelId}&year_id=${selectedModel.id_modelo}`
        )
      } else {
        this.invalid = true
      }
    },
    onReset(e) {
      this.invalid = false
    },
    makeToast(append = false) {
      this.$bvToast.toast(
        `Você receveu uma oferta exclusiva, que acaba em 5 minutos, clique para saber mais.`,
        {
          title: 'Oferta imperdível',
          autoHideDelay: 5000,
          appendToast: append,
          href: '#'
        }
      )
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: var(--green);
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
