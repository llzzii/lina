<template>
  <GenericListPage :table-config="tableConfig" :header-actions="headerActions" />
</template>

<script>
import { GenericListPage } from '@/layout/components'

export default {
  components: {
    GenericListPage
  },
  data() {
    return {
      tableConfig: {
        url: '/api/v1/applications/database-apps/',
        columns: [
          'name', 'get_type_display', 'host', 'port', 'database', 'comment', 'actions'
        ],
        columnsMeta: {
          get_type_display: {
            label: this.$t('applications.type'),
            width: '120px'
          },
          host: {
            width: '140px'
          },
          port: {
            width: '60px'
          },
          database: {
            showOverflowTooltip: true
          }
        }
      },
      headerActions: {
        hasCreate: false,
        hasBulkDelete: false,
        createRoute: 'DatabaseAppCreate',
        moreActionsTitle: this.$t('common.Create'),
        moreActionsType: 'primary',
        extraMoreActions: [
          {
            name: 'MySQL',
            title: 'MySQL',
            type: 'primary',
            can: true,
            callback: this.createMysql.bind(this)
          },
          {
            name: 'PostgreSQL',
            title: 'PostgreSQL',
            type: 'primary',
            can: true,
            callback: this.createPostgreSQL.bind(this)
          },
          {
            name: 'MariaDB',
            title: 'MariaDB',
            type: 'primary',
            can: true,
            callback: this.createMariaDB.bind(this)
          },
          {
            name: 'Oracle',
            title: 'Oracle',
            type: 'primary',
            can: true,
            callback: this.createOracle.bind(this)
          }
        ]
      }
    }
  },
  methods: {
    createMysql() {
      this.$router.push({ name: 'DatabaseAppCreate', query: { type: 'mysql' }})
    },
    createPostgreSQL() {
      this.$router.push({ name: 'DatabaseAppCreate', query: { type: 'postgresql' }})
    },
    createMariaDB() {
      this.$router.push({ name: 'DatabaseAppCreate', query: { type: 'mariadb' }})
    },
    createOracle() {
      this.$router.push({ name: 'DatabaseAppCreate', query: { type: 'oracle' }})
    }
  }
}
</script>

<style>

</style>
