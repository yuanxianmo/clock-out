<template>
  <el-select v-model="projectId" filterable clearable placeholder="Project" @change="selectOption">
    <el-option
      v-for="item in list"
      :key="item.id"
      :label="item.name"
      :value="item.id">
    </el-option>
  </el-select>
</template>

<script>
  export default {
    name: 'project-select',
    data () {
      return {
        list: [],
        projectId: ''
      }
    },
    props: {
      value: null
    },
    watch: {
      value (newValue) {
        this.projectId = newValue
      }
    },
    created () {
      this.projectId = this.value
      this.getList()
    },

    methods: {
      selectOption () {
        this.$emit('update:value', this.projectId)
        this.$emit('change')
      },
      //获取project list
      getList () {
        this.$http.get('getProjectList').then(res => {
          if (res.code === 0) {
            this.list = res.data
          }
        })
      },
    }
  }
</script>

<style scoped>

</style>
