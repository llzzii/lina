<template>
  <ElDatableTable ref="table" class="el-table" v-bind="tableConfig" v-on="$listeners"></ElDatableTable>
</template>

<script>
import { default as ElDatableTable } from './compenents/el-data-table'

export default {
  name: 'DataTable',
  components: {
    ElDatableTable
  },
  props: {
    config: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    const userTableActions = this.config.tableActions || {}
    return {
      defaultConfig: {
        axiosConfig: {
          raw: 1,
          params: {
            display: 1,
            draw: 1
          }
        },
        defaultAlign: 'left',
        dataPath: 'results',
        totalPath: 'count',
        saveQuery: false, // 关闭路径保存查询参数
        persistSelection: true, // 切换页面 已勾选项不会丢失
        hasEdit: userTableActions.hasEdit !== false, // 有编辑按钮
        hasDelete: userTableActions.hasDelete !== false,
        hasNew: false,
        // editText: this.$t('action.update'), // 编辑按钮文案
        buttonSize: 'mini',
        tableAttrs: {
          stripe: true, // 斑马纹表格
          border: true, // 表格边框
          fit: true, // 宽度自适应,
          tooltipEffect: 'dark'
        },
        extraButtons: userTableActions.extraButtons,
        onEdit: (row) => {
          const defaultOnEdit = (row) => {
            const routeName = userTableActions.editRoute
            this.$router.push({ name: routeName, params: { id: row.id }})
          }
          let onEdit = userTableActions.onEdit
          if (!onEdit) {
            onEdit = defaultOnEdit
          }
          return onEdit(row)
        },
        pageCount: 5,
        paginationLayout: 'total, sizes, prev, pager, next',
        paginationSizes: [15, 30, 50, 100],
        paginationSize: 15,
        transformQuery: query => {
          if (query.page && query.size) {
            const page = query.page > 0 ? query.page : 1
            const offset = (page - 1) * query.size
            const limit = query.size
            query.offset = offset
            query.limit = limit
            delete query['page']
            delete query['size']
          }
          if (query.sort) {
            let ordering = query.direction === 'descending' ? '-' : ''
            ordering += query.sort
            query.order = ordering
            delete query['sort']
            delete query['direction']
          }
          return query
        }
      }
    }
  },
  computed: {
    tableConfig() {
      const config = Object.assign(this.defaultConfig, this.config)
      return config
    }
  },
  methods: {
    getList() {
      this.$refs.table.clearSelection()
      return this.$refs.table.getList()
    },
    search(attrs) {
      return this.$refs.table.search(attrs)
    }
  }
}
</script>

<style lang="less" scoped>

  .el-table /deep/ .el-table__row > td {
    line-height: 1.5;
    padding: 8px 0;
  }
  .el-table /deep/ .el-table__row > td> div > span {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }
  .el-table /deep/ .el-table__header > thead > tr >th {
    padding: 8px 0;
    background-color: #F5F5F6;
    font-size: 13px;
    line-height: 1.5;
  }
  .table{
    margin-top: 15px;
  }

  //分页
  .el-pagination /deep/ .el-pagination__total{
    float: left;
  }

  .el-pagination /deep/ .el-pagination__sizes{
    float: left;
  }
  //修改颜色
  // .el-button--text{
  //   color: #409EFF;
  // }
</style>