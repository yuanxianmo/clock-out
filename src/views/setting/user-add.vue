<template>
  <div>
    <el-button type="primary" icon="el-icon-back" class="m-b-20" @click.native.prevent="returnPage">Return</el-button>
    <el-row>
      <el-col :span="12">
        <el-form :label-position="labelPosition" label-width="160px" :model="user">
          <el-form-item label="Project Name">
            <project-select :value.sync="user.projectId"/>
          </el-form-item>
          <el-form-item label="Username" required>
            <el-input type="text" v-model="user.username" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="Password" required>
            <el-input type="password" v-model="user.password" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="Nick Name">
            <el-input v-model="user.nickname" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="Role">
            <el-input v-model="user.role" autocomplete="off"></el-input>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="onSubmit">Save</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>

</template>

<script>
  import ProjectSelect from '../../components/project-select'
  import MD5 from 'md5'

  export default {
    name: 'user-add',
    components: {ProjectSelect},
    data() {
      return {
        labelPosition: 'right',
        user: {
          username: '',
          password: '',
        },
      }
    },
    methods: {
      returnPage() {
        this.$router.back()
      },
      onSubmit() {
        let {username, password, nickname, role, projectId} = this.user
        let obj = {
          projectId: projectId,
          password: password ? MD5(password) : null,
          username: username,
          nickname: nickname,
          role: role
        }
        this.$http.post('/addUser', obj).then(res => {
          if (res.code === 0) {
            this.$message.success('Add success !')
            this.user = {}
          } else {
            this.$message.error(res.sqlMessage)
          }
        })
      },
    }
  }
</script>

<style scoped>

</style>
