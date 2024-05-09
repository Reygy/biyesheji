<template>
  <div class="app-container">
    <div class="invade">
        <div class="banner">
        <div>
          <div style="z-index: 99;">
            <el-form style="margin-top: 72px; z-index: 99;" ref="form"  label-width="200px">
              <!-- <el-form-item label="选择数据集">
                <el-select v-model="form.region" placeholder="please select your zone">
                  <el-option label="NSL KDD" value="NSLKDD" />
                  <el-option label="UNSW NB15" value="NB15" />
                  <el-option label="ToN-loT" value="TON" />
                </el-select>
              </el-form-item> -->
              <el-form-item label="选择训练模型">
                <el-select v-model="form.model" placeholder="please select your zone">
                  <el-option label="决策树" value="decision" />
                  <el-option label="K近邻" value="knn" />
                  <el-option label="线性回归" value="liner" />
                  <el-option label="逻辑回归" value="logistic" />
                  <el-option label="支持向量机" value="lsvm" />
                  <el-option label="神经网络" value="mlp" />
                  <el-option label="随机森林" value="random" />
                </el-select>
              </el-form-item>
              <el-form-item label="选择需要检测入侵类型">
                <el-radio-group v-model="form.resource">
                  <el-radio style="color: white;" label="二分类" />
                  <el-radio style="color: white;" label="多任务辨别"/>
                </el-radio-group>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="onSubmit">开始训练</el-button>
                <el-button @click="onCancel">重置</el-button>
              </el-form-item>
            </el-form>
          </div>
          <!-- <p>Intrusion Detection</p> -->
          <img  class="detect-bgg" src="https://download.qingteng.cn/frontendcdn/www/assets/images/6306e9c1371b9af75acf2aba5b7e2d40.png" alt="入侵检测">
        </div>
      </div>
    </div>
    
    
    <!-- 训练完成提示窗口 -->
    <el-dialog
      title="训练完成"
      :visible.sync="trainCompleteDialogVisible"
      width="30%"
    >
      <p>训练已完成，您可以点击下面的按钮查看结果：</p>
      <div class="center-button">
      <el-button type="success"  @click="viewResults">查看结果</el-button>
      </div>
    </el-dialog>

  </div>
</template>

<script>
import axios from 'axios'; // 引入 axios
import router from '@/router'; // 导入 Vue Router

export default {
  data() {
    return {
      form: {
        
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: '',
        model: '' ,// 存储用户选择的训练模型
        // ...其他数据
        timer: null,
        startTime: null,
        elapsedTime: 0,
        modelt:''
      },
      trainCompleteDialogVisible: false, // 控制训练完成提示窗口的显示状态
      responseData: null // 存储从服务器返回的数据
    }
  },
  computed: {
    formattedTime() {
      const seconds = Math.floor(this.elapsedTime / 1000);
      return new Date(seconds * 1000).toISOString().substr(11, 8);
    },
  },
  methods: {
    startTimer() {
      if (this.timer) {
        clearInterval(this.timer);
      }
      this.timer = setInterval(() => {
        const currentTime = Date.now();
        this.elapsedTime = currentTime - this.startTime;
      }, 1000); // 更新时间间隔为1秒

      this.startTime = Date.now();
      this.elapsedTime = 0;
    },  
    onSubmit() {
      // startTimer();
      if (this.timer) {
        clearInterval(this.timer);
      }
      this.timer = setInterval(() => {
        const currentTime = Date.now();
        this.elapsedTime = currentTime - this.startTime;
      }, 1000); // 更新时间间隔为1秒

      this.startTime = Date.now();
      this.elapsedTime = 0;
      // 构造请求体数据
      const requestData = {
        region:"NB15",
        // region: this.form.region,
        model: this.form.model,
        resource: this.form.resource
        // 如果需要传递更多数据，可以在这里添加
      };
      console.log(this.form.region)
      console.log(this.form.model)
      console.log(this.form.resource)
      if (this.form.model =='decision') {
        this.modelt = '决策树模型';
      } else if (this.form.model =='knn') {
        this.modelt = 'K近邻模型';
      } else if (this.form.model =='logistic') {
        this.modelt = '逻辑回归模型';
      } else if (this.form.model =='lsvm') {
        this.modelt = '支持向量机模型';
      } else if (this.form.model =='mlp') {
        this.modelt = '神经网络模型';
      } else if (this.form.model =='random') {
        this.modelt = '随机森林模型';
      } else if(this.form.model =='liner' ) {
        this.modelt = '线性回归';
      }
      // 发送 POST 请求到后端 Flask 服务器
      axios.post('http://127.0.0.1:5000/train', requestData)
        .then(response => {
          // 处理请求成功的响应
          console.log(response.data);
          this.$message('训练已开始');
          // this.responseData = response.data; // 存储返回的数据
          this.showTrainCompleteDialog(); // 显示训练完成提示窗口

          // 将 startTime 和 elapsedTime 添加到 responseData
          this.responseData = {
            ...response.data, // 包含服务器响应的其他数据
            startTime: this.startTime, // 计时开始的时间
            elapsedTime: this.elapsedTime,// 从开始到现在经过的时间
            mtype: this.modelt,
            mresult:this.form.resource
          };

          
        })
        .catch(error => {
          // 处理请求失败的情况
          console.error('请求失败：', error);
          this.$message.error('请求失败，请重试');
        });

    },
    onCancel() {
      //重置表单数据
      this.form.region = '';
      this.form.model = '';
      this.form.resource = '';

      this.$message({
        message: '内容已取消',
        type: 'warning'
      })
    },
    showTrainCompleteDialog() {
      this.trainCompleteDialogVisible = true; // 显示训练完成提示窗口
    },
    viewResults() {
      console.log('函数调用',this.responseData);
      
      this.$router.push({ name: 'result', query: { resultData: JSON.stringify(this.responseData) }  }); // 假设你的路由配置中有名为 'ResultPage' 的路由
      // this.$router.push({ name: 'result', query: { a:'100','b':200 }  });
      
      //window.localStorage.setItem('name',JSON.stringify(this.responseData))
      //let result = window.localStorage.getItem('name',JSON.parse(this.response))
      //两种方法，注意这是全局变量的设置，同时还要注意质点和字符串的解析

    },
    created(){
      this.$router.push({ name: 'result', query: { a:'100','b':200 }  });
    }
  }
}
</script>

<style scoped>
.line{
  text-align: center;
}
.center-button {
  text-align: center; /* 让按钮居中显示 */
}
</style>

