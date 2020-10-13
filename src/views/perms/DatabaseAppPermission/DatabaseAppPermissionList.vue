<template>
  <GenericListPage :table-config="tableConfig" :header-actions="headerActions" />
</template>

<script>
import { GenericListPage } from '@/layout/components'
import { DetailFormatter } from '@/components/ListTable/formatters'

export default {
  components: {
    GenericListPage
  },
  data() {
    return {
      tableConfig: {
        url: '/api/v1/perms/database-app-permissions/',
        columns: ['name', 'users_amount', 'user_groups_amount', 'database_apps_amount', 'system_users_amount', 'is_valid', 'actions'],
        columnsMeta: {
          name: {
            formatterArgs: {
              routeQuery: {
                activeTab: 'DatabaseAppPermissionDetail'
              }
            },
            showOverflowTooltip: true
          },
          users_amount: {
            label: this.$t('perms.User'),
            width: '110px',
            formatter: DetailFormatter,
            formatterArgs: {
              routeQuery: {
                activeTab: 'DatabaseAppPermissionUser'
              }
            }
          },
          user_groups_amount: {
            label: this.$t('perms.UserGroups'),
            width: '110px',
            formatter: DetailFormatter,
            formatterArgs: {
              routeQuery: {
                activeTab: 'DatabaseAppPermissionUser'
              }
            }
          },
          database_apps_amount: {
            label: this.$t('perms.databaseApp'),
            width: '110px',
            formatter: DetailFormatter,
            formatterArgs: {
              routeQuery: {
                activeTab: 'DatabaseAppPermissionDatabaseApp'
              }
            }
          },
          system_users_amount: {
            label: this.$t('perms.SystemUser'),
            width: '110px',
            formatter: DetailFormatter,
            formatterArgs: {
              routeQuery: {
                activeTab: 'DatabaseAppPermissionDatabaseApp'
              }
            }
          }
        }
      },
      headerActions: {
        hasBulkDelete: false,
        hasCreate: false,
        moreActionsTitle: this.$t('common.Create'),
        moreActionsType: 'primary',
        extraMoreActions: [
          {
            name: 'MySQL',
            title: 'MySQL',
            type: 'primary',
            can: true,
            callback: this.createMysqlOps.bind(this)
          },
          {
            name: 'PostgreSQL',
            title: 'PostgreSQL',
            type: 'primary',
            can: true,
            callback: this.createPostgreSQLOps.bind(this)
          },
          {
            name: 'MariaDB',
            title: 'MariaDB',
            type: 'primary',
            can: true,
            callback: this.createMariaDBOps.bind(this)
          },
          {
            name: 'Oracle',
            title: 'Oracle',
            type: 'primary',
            can: true,
            callback: this.createOracleOps.bind(this)
          }
        ]
      }
    }
  },
  methods: {
    createMysqlOps() {
      this.$router.push({ name: 'DatabaseAppPermissionCreate', query: { type: 'mysql' }})
    },
    createPostgreSQLOps() {
      this.$router.push({ name: 'DatabaseAppPermissionCreate', query: { type: 'postgresql' }})
    },
    createMariaDBOps() {
      this.$router.push({ name: 'DatabaseAppPermissionCreate', query: { type: 'mariadb' }})
    },
    createOracleOps() {
      this.$router.push({ name: 'DatabaseAppPermissionCreate', query: { type: 'oracle' }})
    }
  }
}
</script>

<style>

</style>
