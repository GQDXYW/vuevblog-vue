<template>
 <div>
    <el-container>
        <el-header><router-link to="/blogs">
        <img src="https://www.markerhub.com/dist/images/logo/markerhub-logo.png"
             style="height: 60%; margin-top: 10px;">
        </router-link></el-header>
        <el-main>
        <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-form-item label="用户名" prop="username">
    <el-input  v-model="ruleForm.username" autocomplete="off"></el-input>
  </el-form-item>
  <el-form-item label="密码" prop="password">
    <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
  </el-form-item>
  <el-form-item label="确认密码" prop="checkPass">
    <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
  </el-form-item>
   <el-form-item
    prop="email"
    label="邮箱"
    :rules="[
      { required: true, message: '请输入邮箱地址', trigger: 'blur' },
      { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }
    ]"
  >
    <el-input v-model="ruleForm.email"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="submitForm('ruleForm')" >注册</el-button>
    <el-button @click="resetForm('ruleForm')">重置</el-button>
  </el-form-item>
</el-form>
           </el-main>
    </el-container>

</div>
</template>
<script>
 export default {
    data() {
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.ruleForm.password) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
        ruleForm: {
          username: '',
          password: '',
          email: ''
        },
        check:{
            checkPass: ''
        },
        rules: {
            username: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          password: [
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
          const _this = this
        this.$refs[formName].validate((valid) => {
          if (valid) {
              // 提交逻辑
            this.$axios.post('http://localhost:8081/register', this.ruleForm).then((res)=>{
                 _this.open2()
                 
              _this.$router.push("/login")
            })
          } else {
              _this.open4()
            console.log('error submit!!');
            return false;
          }
        });
      },
       open2() {
        this.$message({
          message: '注册成功',
          type: 'success'
        });
      },  open4() {
        this.$message.error('注册失败');
      }
    },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
</script>