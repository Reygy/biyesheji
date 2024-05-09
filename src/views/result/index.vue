<template>
    <div class="result-container">
      <div class="bg">
        <img style="border-radius: 25%; width: 85px; height: 85px;" src="../../../public/test/logo.jpg">
        <div class="logo-text">
          <div style="font-weight: bold; font-size: 25px; margin-left: 25px; width: 100%;">网络入侵模型检测</div>
          <div style="margin-top: 6px;  font-size: 18px; margin-left: 25px; width: 180px;">Network intrusion model detection</div>
        </div>

        <div class="vertical-line"></div>

        <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
          <img style="width: 35px; height: 38px;" src="../../../public/test/模型列表.png">
        </div>
        <div class="sublogo-text">
          <div style="font-weight: bold; font-size: 20px; margin-left: 25px;">模型类型</div>
          <div style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px;">{{resultData.mtype}}</div>
        </div>

        <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
          <img style="width: 35px; height: 35px;" src="../../../public/test/时间.png">
        </div>
        <div class="sublogo-text">
          <div style="font-weight: bold; font-size: 20px; margin-left: 25px;">训练时长</div>
          <div style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px;">{{resultData.data.time.toFixed(2)}}s</div>
        </div>

        <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
          <img style="width: 38px; height: 38px;" src="../../../public/test/上传时间.png">
        </div>
        <div class="sublogo-text">
          <div style="font-weight: bold; font-size: 20px; margin-left: 25px;">上传时间</div>
          <div style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px;">{{ formattedDate(resultData.startTime) }}</div>
        </div>
      </div>
      
      <div style="border-radius: 15px; margin: 20px 15px; background-color: #fff; box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2); padding: 10px 15px;">
        <div style="display: flex; align-items: center; ">
          <div class="vertical-linee"></div>
          <div style="font-weight: bold; font-size: 25px; margin: 15px 0 15px 0;">训练结果</div>
        </div>
        <div class="vertical-lineee"></div>
        <div v-if="resultData">
          <div class="result"style=" display: flex; margin:45px 165px 50px 165px; justify-content: space-between;">
            <div class="resultdata" style=" display: flex; justify-content: center; align-items: center; flex-direction: column;">
              <div class="title">准确率(Accuracy)</div> 
              <div class="content">{{ resultData.data.accuracy.toFixed(2)}}</div>
            </div>
            <div class="resultdata" style=" display: flex; justify-content: center; align-items: center; flex-direction: column;">
              <div class="title">平均绝对误差(MAE)</div> 
              <div class="content">{{ resultData.data.mean_absolute_error.toFixed(4)}}</div>
            </div>
            <div class="resultdata" style=" display: flex; justify-content: center; align-items: center; flex-direction: column;">
              <div class="title">均方误差(MSE)</div> 
              <div class="content">{{ resultData.data.mean_squared_error.toFixed(4)}}</div>
            </div>
            <div class="resultdata" style=" display: flex; justify-content: center; align-items: center; flex-direction: column;">
              <div class="title">R2 分数(R2_Score)</div> 
              <div class="content">{{ resultData.data.r2_score.toFixed(4)}}</div>
            </div>
          </div>

          <!-- <div>平均绝对误差</div> {{ resultData.data.mean_absolute_error}}</div>
          <div>均方误差</div> {{ resultData.data.mean_squared_error}}</div>
          <div>均方根误差</div> {{ resultData.data.root_mean_squared_error}}</div>
          <div>R2 分数</div> {{ resultData.data.r2_score}}</div> -->
        </div>
        <div style="font-weight: bold; font-size: 25px; margin: 25px 0 0 10px;">拟合结果(预测值与实际值)</div>
        <div style="display: flex;">
          <div>
          <img :src="picurl" alt="Image">
        </div>
        <div>
          <img :src="corpicurl" alt="Image">
        </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios'; // 引入 axios

  export default {
    data() {
      return {
        picurl: '' ,// 初始化 picurl 属性为空字符串
        corpicurl:'',
        resultData: null // 初始化 resultData 属性
        //如果上面有调用要注意使用的是this.什么什么
        //props: ['resultData'], // 接收传递的数据
      };
    },

    mounted() {
      this.getImage();
      this.getcorpic();
    },

    created() {
      // 当组件创建时，检查是否有从上一页传递过来的数据

      this.resultData = JSON.parse(this.$route.query.resultData);
      if (this.resultData) {
        console.log('返回的数据内容:', this.resultData);
      } else {
        // 如果没有传递数据，你可以根据需要处理
        console.error('未找到结果数据。');
      };
      console.log(this.resultData.data)
    },
    methods:{
      formattedDate(timestamp) {
        const date = new Date(timestamp); // 使用传入的参数
        const year = date.getFullYear();
        const month = date.getMonth() + 1; // 月份是从0开始的，所以要加1
        const day = date.getDate();
        return `${year}年${month}月${day}日`;
      },
      getImage() {
        var that = this;
        axios({
              method: "get",
              url: "http://127.0.0.1:5000/getPic",
              responseType: "arraybuffer", // 最为关键
            }).then(function (response) {
              that.picurl =
                "data:image/jpeg;base64," + that.arrayBufferToBase64(response.data);
            });
          },
      getcorpic() {
        var that = this;
        axios({
              method: "get",
              url: "http://127.0.0.1:5000/corpic",
              responseType: "arraybuffer", // 最为关键
            }).then(function (response) {
              that.corpicurl =
                "data:image/jpeg;base64," + that.arrayBufferToBase64(response.data);
        });
      },
           // ArrayBuffer转为base64字符串
      arrayBufferToBase64(buffer) {
        //第一步，将ArrayBuffer转为二进制字符串
        var binary = "";
        var bytes = new Uint8Array(buffer);
        var len = bytes.byteLength;
        for (var i = 0; i < len; i++) {
          binary += String.fromCharCode(bytes[i]);
        }
        //将二进制字符串转为base64字符串
        return window.btoa(binary);
      }
    }
  }
  </script>
  
  <style scoped>
  .result-container{
    background-color: #F5F6F8;
  }
  .bg {
    background-image: linear-gradient(to right, #606DEC, #9E86D9);
    padding: 50px 40px;
    display: flex;
    /* margin: 22px 20px; */
    color: #fff;
    align-items: center;
    .vertical-line {
      width: 2px; /* 竖线的宽度 */
      height: 75px; /* 竖线的高度，可以根据需要调整 */
      background-color: white; /* 竖线的颜色 */
      margin: 0 65px; /* 竖线左右的间距 */
      opacity: 0.3;
    }
    
  }
  .vertical-linee{
      width: 5px; /* 竖线的宽度 */
      height: 20px; /* 竖线的高度，可以根据需要调整 */
      background-color: #4A71FF; /* 竖线的颜色 */
      margin: 0 15px 0 2px;
      /* opacity: 0.3; */
  }

  .vertical-lineee{
    width: 100%; /* 竖线的宽度 */
    height: 0.6px; /* 竖线的高度，可以根据需要调整 */
    background-color: rgba(0, 0, 0, 0.1); /* 竖线的颜色 */
    margin: 0 15px 0 2px;
    /* opacity: 0.1; */
  }

  .title{
    font-size: 14px;
    margin-bottom: 10px;
  }

  .content{
    font-weight: bold;
    font-size: 23px;
  }
  </style>
  