<template>
  <div class="menus">
    <b-breadcrumb :items="breadcrumb" />
    <div class="text-right mb-3">
      <b-button variant="success" to="/conta/menus/new">
        <b-icon-plus /> Cadastrar
      </b-button>
    </div>
    <div v-if="menus">
      <b-table v-if="menus.length" :fields="table" :items="menus" responsive="sm">
        <template v-slot:cell(page)="data">
          <template v-if="data.value">
            <n-link :to="'/' + data.value.slug">{{ '/' + data.value.slug }}</n-link>
          </template>
          <template v-else>
            <a v-if="data.item.url.startsWith('http')" :href="data.item.url" target="_blank">{{ data.item.url }}</a>
            <n-link v-else :to="data.item.url">{{ data.item.url }}</n-link>
          </template>
        </template>
        <template v-slot:cell(actions)="data">
          <n-link class="btn btn-info btn-sm" :to="'/conta/menus/' + data.item.id + '/edit'">
            <b-icon-pencil />
          </n-link>
          <b-button variant="danger" size="sm" @click="remove(data.item)">
            <b-icon-trash />
          </b-button>
        </template>
      </b-table>
      <b-alert v-else show variant="dark" class="text-center">Nenhum Menu encontrado</b-alert>
    </div>
    <div v-else class="text-center">
      <b-spinner small label="Carregando..." />
    </div>
  </div>
</template>

<script>

export default {
  layout: 'conta',

  data () {
    return {
      menus: null,
      breadcrumb: [
        { text: 'Painel', to: '/conta' },
        { text: 'Menus', active: true }
      ],
      table: [
        { key: 'name', label: 'Nome' },
        { key: 'page', label: 'Página' },
        { key: 'actions', label: '', class: 'text-right' }
      ]
    }
  },
  created () {
    this.list()
  },
  methods: {
    async list () {
      this.menus = await this.$axios.$get('/api/menus', { params: { populate: 'page' } })
    },
    remove (menu) {
      this.$bvModal.msgBoxConfirm('Tem certeza que deseja excluír este item?').then(async confirmed => {
        if (confirmed) {
          await this.$axios.delete('/api/menus/' + menu._id).then(() => {
            this.list()
            this.$toast.success('Menu removido com sucesso!')
          })
        }
      })
    }
  }
}
</script>
