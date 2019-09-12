<template>
  <div class="container">
    <h2>Marca</h2>
    <div class="row">
      <b-form @submit="onSubmit" @reset="onReset">
        <b-form-group
          id="brand-selector-label"
          label="Selecione a marca do seu carro:"
          label-for="brand-selector"
        >
          <b-form-input
            id="brand-input"
            v-model="brand"
            :class="{ 'is-invalid': invalid }"
            autocomplete="off"
            autofocus
            list="brands-list"
          ></b-form-input>
          <b-form-datalist id="brands-list" :options="options">
          </b-form-datalist>
        </b-form-group>

        <b-button type="submit" variant="primary">OK</b-button>
        <b-button type="reset" variant="secundary">reset</b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      brand: '',
      brands: [],
      options: [],
      invalid: false
    }
  },
  created() {
    axios
      .get(
        'http://api.fipeapi.com.br/v1/carros?9c86d2c2155102bfffbab7d069036e11'
      )
      .then((response) => {
        this.brands = response.data
        this.options = response.data.map((brand) => brand.name)
      })
  },
  methods: {
    onSubmit(e) {
      e.preventDefault()

      if (this.options.includes(this.brand)) {
        const selectedBrand = this.brands.filter(
          (brand) => brand.name === this.brand
        )[0]

        this.$router.push(`model?brand_id=${selectedBrand.id}`)
      } else {
        this.invalid = true
      }
    },
    onReset(e) {
      this.invalid = false
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
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
