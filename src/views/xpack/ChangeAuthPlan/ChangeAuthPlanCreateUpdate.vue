<template>
  <GenericCreateUpdatePage v-bind="$data" />
</template>

<script>
import { GenericCreateUpdatePage } from '@/layout/components'
import { AssetSelect } from '@/components'

export default {
  name: 'ChangeAuthPlanCreateUpdate',
  components: {
    GenericCreateUpdatePage
  },
  data() {
    return {
      url: '/api/v1/xpack/change-auth-plan/plan/',
      fields: [
        [this.$t('xpack.Basic'), ['name']],
        [this.$t('xpack.Asset'), ['username', 'assets', 'nodes']],
        [this.$t('xpack.ChangeAuthPlan.PasswordStrategy'), ['password_strategy', 'password', 'password_rules']],
        [this.$t('xpack.Timer'), ['is_periodic', 'crontab', 'interval']],
        [this.$t('xpack.Other'), ['comment']]
      ],
      initial: {
        password_strategy: 'custom',
        is_periodic: true,
        password_rules: {
          length: 20
        },
        interval: 24
      },
      fieldsMeta: {
        username: {
          helpText: '用户名为将要修改的资产上的用户的用户名。如果用户存在，则修改密码；如果用户不存在，则创建用户。'
        },
        assets: {
          type: 'assetSelect',
          component: AssetSelect,
          rules: [
            { required: false }
          ],
          label: this.$t('xpack.Asset')
        },
        password: {
          hidden: (formValue) => {
            return formValue.password_strategy !== 'custom'
          },
          rules: [
            { required: this.$route.meta.action === 'create', message: this.$t('common.fieldRequiredError'), trigger: 'blur' }
          ]
        },
        password_rules: {
          type: 'group',
          items: this.generatePasswordRulesItemsFields()
        },
        nodes: {
          label: this.$t('xpack.Node'),
          el: {
            value: [],
            ajax: {
              url: '/api/v1/assets/nodes/',
              processResults(data) {
                let results = data.results
                results = results.map((item) => {
                  return { label: item.full_value, value: item.id }
                })
                const more = !!data.next
                return { results: results, pagination: more, total: data.count }
              }
            }
          }
        },
        is_periodic: {
          type: 'switch'
        },
        crontab: {
          label: this.$t('xpack.ChangeAuthPlan.RegularlyPerform'),
          hidden: (formValue) => {
            return formValue.is_periodic === false
          },
          helpText: '' +
            'eg：每周日 03:05 执行 <5 3 * * 0>' +
            '<br/>' +
            '使用5位 Linux crontab 表达式 <分 时 日 月 星期> ' +
            '（<a href="https://tool.lu/crontab/" target="_blank">在线工具</a>）' +
            '<br/>' +
            '如果同时设置了定期执行和周期执行，优先使用定期执行' +
            ''
        },
        interval: {
          label: this.$t('xpack.ChangeAuthPlan.CyclePerform'),
          hidden: (formValue) => {
            return formValue.is_periodic === false
          },
          helpText: '（单位： 时）'
        }
      }
    }
  },
  methods: {
    generatePasswordRulesItemsFields() {
      const itemsFields = []
      const items = [
        { id: 'length', prop: 'length', label: this.$t('xpack.ChangeAuthPlan.PasswordLength') }
      ]
      items.forEach((item, index, array) => {
        itemsFields.push({
          id: item.id, prop: item.prop, el: {}, attrs: {}, type: 'input', label: item.label, required: true,
          hidden: (formValue) => { return ['random_one', 'random_all'].indexOf(formValue.password_strategy) === -1 }
        })
      })
      return itemsFields
    }
  }
}
</script>

<style scoped>

</style>