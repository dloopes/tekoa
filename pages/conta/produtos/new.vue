<template>
  <div class="create">
    <b-breadcrumb :items="breadcrumb" />
    <ProductForm />
  </div>
</template>
<script>
export default {
  layout: 'conta',
  data () {
    return {
      breadcrumb: [
        { text: 'Painel', to: '/conta' },
        { text: 'Loja', to: '/conta/loja' },
        { text: 'Produtos', to: '/conta/produtos' },
        { text: 'Cadastrar', active: true }
      ]
    }
  }
}
</script>
