<template>
  <div class="dashboard-container">
    <!-- <div class="title">网络入侵检测系统介绍</div> -->
    <div class="invade">
        
        <div class="banner">
        <div class="banner-text wrapper">
          <div>制作人：张思源</div>
          <h2>网络入侵检测系统</h2>
          <p>Intrusion Detection</p>
          <div class='banner-btn'>
            <router-link to="/form/index" class="template_apply-btn">开始训练模型</router-link>
          </div>
        </div>
        <div class="banner-outline">
          <div class="wrapper">
            <ul class="pull-left intrusion-business clearfix">
              <li class="pull-left">
                <div>
                  7 models</div>
                <span>多模型训练</span>
              </li>
              <li class="pull-left">
                <div>
                9 kinds</div>
                <span>多分类识别</span>
              </li>
              <li class="pull-left">
                <div>
                  speed+</div>
                <span>快速训练</span>
              </li>
            </ul>
            <img class="detect-bg" src="https://download.qingteng.cn/frontendcdn/www/assets/images/6306e9c1371b9af75acf2aba5b7e2d40.png" alt="入侵检测">
          </div>
        </div>

      </div>
      <div style="border-radius: 15px; margin: 20px 15px; background-color: #fff; box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2); padding: 10px 15px;">
        <div style="display: flex; align-items: center; ">
          <div class="vertical-linee"></div>
          <div style="font-weight: bold; font-size: 25px; margin: 15px 0 15px 0;">模型介绍</div>
        </div>
        <div class="vertical-lineee"></div>
      <div class="carousel-container" @mouseenter="pause" @mouseleave="play"style="margin: 10px 0 0 0;" >
        <!-- style="background-color: #F7F9FA;" -->
          <transition-group name="slide" tag="div" class="carousel-slide" style="width: 100%;height: 300px;">
            <div v-for="(module, index) in modules" :key="module.id" v-show="index === activeIndex" class="invade-module-tr clearfix loading-model-item">
              <div class="invade-module-left pull-left">
                <h5 class="invade-module-h5">{{ module.title }}</h5>
                <span class="invade-module-span">
                  {{ module.description }}
                </span>
              </div>
              <div>
                <div v-for="image in module.images" :class="['invade-module-img', image]" :style="getBackgroundStyle(image)"></div>
              </div>
            </div>
          </transition-group>
          <button class="arrow prev" @click="prev">←</button>
          <button class="arrow next" @click="next">→</button>
        </div>
      </div>
      
      <div class="invade-highlight">
        <div style="height: 650px; border-radius: 15px; margin: 20px 15px; background-color: #fff; box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2); padding: 10px 15px;">
        <div style="display: flex; align-items: center; ">
          <div class="vertical-linee"></div>
          <div style="font-weight: bold; font-size: 25px; margin: 15px 0 15px 0;">数据集特征介绍</div>
        </div>
        <div class="vertical-lineee"></div>
        <div style="display: flex; align-items: center; justify-content: center; ">
          <div style="display: flex; align-items: center; justify-content: center; height: 100%; width: 50%;">
            <div class="waikuangg">
              <!-- 显示当前激活的waikuang的信息 -->
              <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
                
                <img style="width: 35px; height: 38px;" :src="waikuangs[currentIndex].imgSrc">
              </div>
              <div class="sublogo-text">
                <h3 class="artistic-font" style="font-weight: bold; font-size: 20px; margin-left: 25px;">{{ waikuangs[currentIndex].title }}</h3>
                <div class="artistic-font" style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 300px; margin-bottom: 22px;">{{ waikuangs[currentIndex].description }}</div>
              </div>

            </div>
            <div class="waikuangs">
              
            </div>
            <!-- <div class="waikuangg"> 
              <div  class="text-style-1lines heading-style-h5" style="text-align: center;">你好</div>
            </div> -->
            <!-- @click="selectWaikuang(index)" 点击效果没做出来 -->
          </div>
          <div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; width: 50%;">
            <div 
                v-for="(waikuang, index) in waikuangs" 
                :key="index" 
                class="waikuang" 
                @mouseover="pauseCarousel(index)" 
                @mouseleave="resumeCarousel"
                
                :class="{ 'active': currentIndex === index }"
              >
              <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
                <img style="width: 35px; height: 38px;" :src="waikuang.imgSrc">
              </div>
              <div class="sublogo-text">
                <div class="artistic-font" style="font-weight: bold; font-size: 20px; margin-left: 25px;">{{ waikuang.title }}</div>
                <div class="artistic-fontt text-style-1lines heading-style-h5" style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px; margin-bottom: 22px;">{{ waikuang.description }}</div>
              </div>

             </div>
            <!-- <div class="waikuang">
              <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
                <img style="width: 35px; height: 38px;" src="../../../public/test/模型列表.png">
              </div>
              <div class="sublogo-text">
                <div style="font-weight: bold; font-size: 20px; margin-left: 25px;">多样性</div>
                <div  class="text-style-1lines heading-style-h5" style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px; margin-bottom: 22px;">UNSW-NB15数据集包含了广泛的攻击类型，如DoS攻击、扫描、Botnet活动、Fuzzers等，以及正常的网络流量。这种多样性使得该数据集非常适合用于测试和评估入侵检测系统的性能，因为它能够模拟真实世界中多变的网络威胁景观。</div>
              </div>
            </div>

            <div class="waikuang">
              <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
                <img style="width: 35px; height: 38px;" src="../../../public/test/模型列表.png">
              </div>
              <div class="sublogo-text">
                <div style="font-weight: bold; font-size: 20px; margin-left: 25px;">现代性</div>
                <div class="text-style-1lines heading-style-h5" style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px; margin-bottom: 22px;">与较早的网络安全数据集（如KDD Cup 99）相比，UNSW-NB15数据集包括了更现代的攻击场景和技术。这是因为它被设计来反映当前的网络威胁环境，包括最近的攻击技术和策略，使其成为一个时代感更强的测试平台。</div>
              </div>
            </div>

            <div class="waikuang">
              <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
                <img style="width: 35px; height: 38px;" src="../../../public/test/模型列表.png">
              </div>
              <div class="sublogo-text">
                <div style="font-weight: bold; font-size: 20px; margin-left: 25px;">详细的特征集</div>
                <div class="text-style-1lines heading-style-h5" style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px; margin-bottom: 22px;">该数据集提供了49种不同的特征，包括基于时间的特征、基于流的特征、内容特征等。这些特征的广泛覆盖使得研究人员可以深入分析和理解不同类型的网络流量和攻击行为，提高了入侵检测模型的准确度和鲁棒性。</div>
              </div>
            </div>

            <div class="waikuang">
              <div style="display: flex; justify-content: center; align-items: center; border-radius: 50%; width: 65px; height: 65px; background-color: rgba(0,0,0,0.2);">
                <img style="width: 35px; height: 38px;" src="../../../public/test/模型列表.png">
              </div>
              <div class="sublogo-text">
                <div style="font-weight: bold; font-size: 20px; margin-left: 25px;">实用性与适用性</div>
                <div class="text-style-1lines heading-style-h5" style="margin-top: 6px;  font-size: 15px; margin-left: 25px; width: 180px; margin-bottom: 22px;">UNSW-NB15数据集不仅被学术界广泛采用，而且其设计初衷是作为网络安全从业者和研究人员的实用工具。数据集的结构和格式支持直接应用于多种机器学习和数据分析工具，使得开发和测试入侵检测系统更为方便快捷。</div>
              </div>
            </div> -->
          </div>
        </div> 

      <div class="process slideInUp-small animated zzmpc"></div>
      <!-- <div class="process slideInUp-small animated zzmmb"></div> -->
    </div>
    <div style="border-radius: 15px; margin: 20px 15px; background-color: #fff; box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2); padding: 10px 15px;">
        <div style="display: flex; align-items: center; ">
          <div class="vertical-linee"></div>
          <div style="font-weight: bold; font-size: 25px; margin: 15px 0 15px 0;">数据结果介绍</div>
        </div>
          <div class="vertical-lineee"></div>
          <div class="chart-containerr" style="margin-top: 15px; display: flex;" >
            <!--  -->
            <div class="carousel-containerr" @mouseenter="pause" @mouseleave="play" >
              <transition-group name="slide" tag="div" class="carousel-slide" style="height: 300px!important;">
                <div v-for="(module, index) in datatexts" :key="module.id" v-show="index === activeIndex" class="invade-module-tr clearfix loading-model-item">
                  <div class="invade-module-left pull-left">
                    <h5 class="invade-module-h5">{{ module.title }}</h5>
                    <span class="invade-module-spann">
                      {{ module.description }}
                    </span>
                  </div>
                </div>
              </transition-group>
              <button class="arroww prev" @click="prev">←</button>
              <button class="arroww next" @click="next">→</button>
            </div>

            <div ref="chart" style="width: 100%;height:500px;"></div>
            <!-- <chart1_1 ref="chart11" style="flex: 1;width: 100%;" ></chart1_21> -->
            <!-- <div ref="chartt" style="width: 100%;height:500px;"></div> -->
            <!-- <div id="heatmap" style="width: 600px;height:400px;"></div> -->
          </div>
    </div>
      <div style="border-radius: 15px; margin: 20px 15px; background-color: #fff; box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2); padding: 10px 15px;">
        <div style="display: flex; align-items: center; ">
          <div class="vertical-linee"></div>
          <div style="font-weight: bold; font-size: 25px; margin: 15px 0 15px 0;">数据相关性分析</div>
        </div>
      <div class="vertical-lineee"></div>
      <div class="chart-containerrr" style="display: flex; margin: 10px 0 0 0 ;">
        
        <div class="carousel-containerrr" @mouseenter="pause" @mouseleave="play">
          <transition-group name="slide" tag="div" class="carousel-slide">
            <div v-for="(module, index) in corrtexts" :key="module.id" v-show="index === activeIndex" class="invade-module-tr clearfix loading-model-item">
              <div class="invade-module-left pull-left">
                <h5 class="invade-module-h5">各类字段介绍</h5>
                <span class="invade-module-spannn pre-wrap">
                  {{ module.description }}
                </span>
              </div>
            </div>
          </transition-group>
          <button class="arrow prev" @click="prev">←</button>
          <button class="arrow next" @click="next">→</button>
        </div>
        <!-- <div id="heatmap" style="width: 600px;height:400px;"></div> -->
        <div>
          <img :src='imageUrl' alt="Image">
          <img :src='mulimageurl' alt="Image">
        </div>
      </div>
      </div>
      
      </div>
      
    </div>

    <!-- 省略其他代码 -->
    <div class="upload-drag-area"
         @dragover.prevent="handleDragOver"
         @dragleave.prevent="handleDragLeave"
         @drop.prevent="handleFileDrop">
      <p>拖拽文件到此处上传</p>
      <input type="file"
             style="display: none;"
             ref="fileInput"
             accept=".csv"
             @change="handleFileChange" />
    </div>
    <!-- 省略其他代码 -->
    <div class="card">
      <el-row :gutter="40" class="panel-group">
        <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col" @click.native="handleApriori()">
          <div class="card-panel">
            <div class="card-panel-icon-wrapper icon-money">
              <svg-icon icon-class="link" style="width: 3rem; height: 3rem" class-tagName="card-panel-icon" />
            </div>
            <div class="card-panel-description">
              <div class="card-panel-text">数据集介绍</div>
            </div>
          </div>
        </el-col>
        <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col" @click.native="handleClustering()">
          <div class="card-panel">
            <div class="card-panel-icon-wrapper icon-view">
              <svg-icon icon-class="clipboard" style="width: 4em; height: 4em" class-tagName="card-panel-icon" />
            </div>
            <div class="card-panel-description">
              <div class="card-panel-text">聚类分析</div>
            </div>
          </div>
        </el-col>
        <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col" @click.native="handleClassification()">
          <div class="card-panel">
            <div class="card-panel-icon-wrapper icon-people">
              <svg-icon icon-class="table" style="width: 4em; height: 4em" class-tagName="card-panel-icon" />
            </div>
            <div class="card-panel-description">
              <div class="card-panel-text">分类分析</div>
            </div>
          </div>
        </el-col>
        <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col" @click.native="handleRegression()">
          <div class="card-panel">
            <div class="card-panel-icon-wrapper icon-message">
              <svg-icon icon-class="search" style="width: 4em; height: 4em" class-tagName="card-panel-icon" />
            </div>
            <div class="card-panel-description">
              <div class="card-panel-text">回归分析</div>
            </div>
          </div>          
        </el-col>
      </el-row>
  </div>
    <div class="rightlist" id="rightlistid">
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import img1 from '../../../public/test/blue.png'
import img2 from '../../../public/test/red.png'
import img3 from '../../../public/test/white.png'
import pic1 from '../../../public/test/decesion.png'
import pic2 from '../../../public/test/KNN.png'
import pic3 from '../../../public/test/linear.png'
import pic4 from '../../../public/test/logic.png'
import pic5 from '../../../public/test/svm.png'
import pic7 from '../../../public/test/randomtree.png'
import pic6 from '../../../public/test/sjwl.png'
import picurl from '../../../public/test/二分类.png'
import pic11 from '../../../public/test/多样性.png'
import pic22 from '../../../public/test/现代性2.png'
import pic33 from '../../../public/test/实用性.png'
import pic44 from '../../../public/test/现代性.png'

import * as echarts from 'echarts';
import axios from 'axios';
import Papa from 'papaparse'; // 引入PapaParse用于解析CSV文件


export default {
  name: 'Dashboard',
  data() {
    return {
      imageUrl: require('../../../public/test/二分类.png'),
      mulimageurl:require('../../../public/test/多分类.png'),
      resultData: null ,// 初始化 resultData 属性
      Chart: null,//hot map
      echart: null,
      chart: null,
      responseData: null,
      chartInstance: null,
      judge:0,
      waikuangs: [
        { title: '多样性', description: 'UNSW-NB15数据集包含了广泛的攻击类型，如DoS攻击、扫描、Botnet活动、Fuzzers等，以及正常的网络流量。这种多样性使得该数据集非常适合用于测试和评估入侵检测系统的性能，因为它能够模拟真实世界中多变的网络威胁景观。', imgSrc: pic11 },
        { title: '现代性', description: '与较早的网络安全数据集（如KDD Cup 99）相比，UNSW-NB15数据集包括了更现代的攻击场景和技术。这是因为它被设计来反映当前的网络威胁环境，包括最近的攻击技术和策略，使其成为一个时代感更强的测试平台。', imgSrc: pic22 },
        { title: '详细的特征集', description: '该数据集提供了49种不同的特征，包括基于时间的特征、基于流的特征、内容特征等。这些特征的广泛覆盖使得研究人员可以深入分析和理解不同类型的网络流量和攻击行为，提高了入侵检测模型的准确度和鲁棒性。', imgSrc: pic33 },
        { title: '实用性与适用性', description: 'UNSW-NB15数据集不仅被学术界广泛采用，而且其设计初衷是作为网络安全从业者和研究人员的实用工具。数据集的结构和格式支持直接应用于多种机器学习和数据分析工具，使得开发和测试入侵检测系统更为方便快捷。', imgSrc: pic44 },
        // 其他waikuang定义...
      ],
      carouselInterval: null,
      chartOptions: {
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b}: {c} ({d}%)'
        },
        legend: {
          data: [
            'Normal',
            'Exploits',
            'DoS',
            'Fuzzers',
            'Reconnaissance',
            'Analysis',
            'Worms',
            'Backdoor',
            'Generic'
          ]
        },
        series: [
          {
            name: 'Access From',
            type: 'pie',
            selectedMode: 'single',
            radius: [0, '25%'],
            label: {
              position: 'inner',
              fontSize: 14
            },
            labelLine: {
              show: false
            },
            data: [
              { value: 61685, name: 'Normal' },
              { value: 19488, name: 'Abnormal' }
            ]
          },
          {
            name: 'Intrusion Type',
            type: 'pie',
            radius: ['40%', '55%'],
            labelLine: {
              length: 30
            },
            label: {
              formatter: '{a|{a}}{abg|}\n{hr|}\n  {b|{b}：}{c}  {per|{d}%}  ',
              backgroundColor: '#F6F8FC',
              borderColor: '#8C8D8E',
              borderWidth: 1,
              borderRadius: 4,
              rich: {
                a: {
                  color: '#6E7079',
                  lineHeight: 22,
                  align: 'center'
                },
                hr: {
                  borderColor: '#8C8D8E',
                  width: '100%',
                  borderWidth: 1,
                  height: 0
                },
                b: {
                  color: '#4C5058',
                  fontSize: 14,
                  fontWeight: 'bold',
                  lineHeight: 33
                },
                per: {
                  color: '#fff',
                  backgroundColor: '#4C5058',
                  padding: [3, 4],
                  borderRadius: 4
                }
              }
            },
            data: [
              { value: 39496, name: 'Normal' },
              { value: 19488, name: 'Backdoor', itemStyle: { color: '#ff69b4' } },
              { value: 16187, name: 'Fuzzers' },
              { value: 1791, name: 'Reconnaissance' },
              { value: 1731, name: 'Exploits' },
              { value: 1703, name: 'Analysis' },
              { value: 564, name: 'DoS' },
              { value: 114, name: 'Worms' },
              { value: 99, name: 'Generic' }
            ]
          }
        ]
      },
      activeIndex: 0,
      interval: null,
      currentIndex:0,
      modules: [
        {
          id: 1,
          title: '决策树（Decision Trees）',
          description: '决策树是一种机器学习算法，用于分类和回归任务。以树状图的形式模型化决策，其中每个内部节点代表一个属性上的测试，每个分支代表一个测试输出，而每个叶节点代表一种类别。决策树易于理解和解释，可以可视化，对数据的准备要求相对较少。它们通过一系列的if-then规则递归地分割数据，选择最优分割以最大化类间差异或减少不纯度（如基尼不纯度或熵）。然而，决策树容易过拟合，特别是当树很深时，可能会捕获噪声。剪枝技术如最小化成本复杂度剪枝可以用来优化决策树。',
          images: [pic1]
        },
        {
          id: 2,
          title: 'K近邻（K-Nearest Neighbors, KNN）',
          description: 'K近邻算法是一种简单、直观的分类与回归方法。基于最近邻的投票方式进行预测：对于新的数据点，算法在训练集中找到与之最近的K个数据点，然后根据这些最邻近数据点的已知响应来预测新点的分类。距离度量通常使用欧氏距离，但也可以使用其他类型的距离度量。KNN的优点是模型结构简单、理解容易。然而，它是一种惰性学习器，计算开销大，特别是当数据集很大时，因为它需要为每个预测存储整个数据集。此外，选择适当的K值和距离度量是该算法性能的关键。',
          images: [pic2]
        },
        {
          id: 3,
          title: '线性回归（Linear Regression）',
          description: '线性回归是一种预测数值输出的方法，其中输出是输入特征的加权和。它是最简单、最广泛使用的回归技术之一。算法尝试找到一条直线（在更高维度是一个平面或超平面），以最小化实际输出值和预测值之间的差异（通常使用均方误差）。线性回归模型可以扩展到多元，允许处理多个输入特征。虽然它强大且实用，但线性回归的一个主要限制是它假设数据中存在线性关系，这在许多真实情况下并不总是成立。',
          images: [pic3]
        },
        {
          id: 4,
          title: '逻辑回归（Logistic Regression）',
          description: '逻辑回归，尽管其名称中有“回归”二字，实际上是一种用于二分类问题的统计方法。该模型使用逻辑函数将特征空间映射到概率空间，其输出值被限制在0和1之间。逻辑回归是评估变量相对独立性的常用工具，广泛应用于医学、社会科学等领域。它易于实现，但如果特征空间与类标签间没有线性关系，或特征高度相关，模型的性能可能会降低。',
          images: [pic4]
        },
        {
          id: 5,
          title: '支持向量机（Support Vector Machines, SVM）',
          description: '支持向量机是一种强大的二分类模型，适用于中小型复杂数据集。其基本思想是找到一个超平面，最大化不同类之间的边界。在特征空间中，它可以通过核技巧进行非线性拓展，允许算法在高维空间中找到复杂的关系，其中数据可以被分隔。这种高维映射由所选的核函数决定，常用的有线性核、多项式核和径向基函数（RBF）核。虽然SVM通常提供高准确度，但它也更昂贵，尤其是在处理大型数据集时，因为计算需要更多的时间和内存。此外，核函数和正则化参数的选择对模型的性能有很大影响。',
          images: [pic5]
        },
        {
          id: 6,
          title: '神经网络（Neural Networks）',
          description: '神经网络，也称为人工神经网络，受人脑结构启发，由相互连接的节点或“神经元”组成。每个连接代表数据流动方向上的权重。输入数据通过网络层传递并转换，每一层都进一步抽象数据，直到最终输出层。基于简单元素的复杂网络能够捕捉数据中的非线性模式，对于图像和声音识别等复杂任务尤其有效。神经网络的一个常用变种是深度学习，它通过拥有多个隐藏层来增加模型复杂度。神经网络具有很高的柔性，但也需要大量的数据、计算资源，而且往往不易解释。',
          images: [pic6]
        },
        {
          id: 7,
          title: '随机森林（Random Forests）',
          description: '随机森林是一个集成学习方法，通过构建多个决策树并结合它们的预测来工作。在这个算法中，每棵树都在数据集的一个随机子集上独立地训练，用随机选择的特征子集来分割节点。最后，森林通过每棵树的投票或平均预测来做出决策。随机森林比单一决策树具有更好的预测性能和鲁棒性，因为它减少了过拟合并且能够处理非常高维的数据。不过，与单棵树相比，它们在界面上更难解释，并且在训练和预测时计算代价更高。',
          images: [pic7]
        }
      ],
      datatexts: [
        {
          id: 1,
          title: 'Generic（通用）',
          description: '这通常指的是普遍性的攻击，可能是自动化工具或脚本发起的攻击，不针对特定的漏洞或系统。',
          images: [pic1]
        },
        {
          id: 2,
          title: 'Normal（正常）',
          description: '指的是正常的网络流量，没有恶意活动的流量数据，可以用来与异常流量进行对比。',
          images: [pic2]
        },
        {
          id: 3,
          title: 'Exploits（利用）',
          description: '这类攻击尝试通过利用系统漏洞来控制系统或窃取信息。',
          images: [pic3]
        },
        {
          id: 4,
          title: 'DoS（拒绝服务）',
          description: 'Denial of Service (DoS) 攻击旨在使网络服务或资源不可用，通常是通过过载目标系统的服务器或网络资源实现的。',
          images: [pic4]
        },
        {
          id: 5,
          title: 'Fuzzers（模糊测试工具）',
          description: '利用模糊测试方法发起的攻击，通过发送异常或随机数据到程序中试图导致错误或漏洞。',
          images: [pic5]
        },
        {
          id: 6,
          title: 'Reconnaissance（侦查）',
          description: '表示网络上执行的信息收集活动，攻击者通常在攻击前进行侦查以收集有关目标的信息。',
          images: [pic6]
        },
        {
          id: 7,
          title: 'Analysis（分析）',
          description: '这可能是指对网络流量进行深度分析以识别潜在的威胁或异常行为的流量。',
          images: [pic7]
        },
        {
          id: 8,
          title: 'Worms（蠕虫）',
          description: '自我复制的恶意软件，能够独立传播到其他系统，无需用户干预。',
          images: [pic7]
        },
        {
          id: 9,
          title: 'Backdoor（后门）',
          description: '指的是秘密跳过正常的认证或加密的方法，常用来在系统后门中保持持续的非授权访问。',
          images: [pic7]
        }
      ],
      corrtexts: [
        {
          id: 1,
          title: 'Generic（通用）',
          description: 'dur: 会话持续时间\nspkts: 发送数据包的数量\ndpkts: 接收数据包的数量\nsbytes: 发送字节数\ndbytes: 接收字节数\nrate: 数据传输的速率',
          images: [pic1]
        },
        {
          id: 2,
          title: 'Normal（正常）',
          description: 'sttl: 源到目的地的时间存活（TTL）值\ndttl: 目的到源的时间存活（TTL）值\nsload: 源到目的负载（比如，带宽使用量）\ndload: 目的到源负载\nsloss: 源丢失的数据包数量\ndloss: 目的丢失的数据包数量',
          images: [pic2]
        },
        {
          id: 3,
          title: 'Exploits（利用）',
          description: 'sinpkt: 发送方向的数据包间隔时间\ndinpkt: 接收方向的数据包间隔时间\nsjit: 发送方向的抖动（数据包到达时间的变化量）\ndjit: 接收方向的抖动\nswin: TCP窗口大小（发送方向）\nstcpb: 发送方向TCP序列号',
          images: [pic3]
        },
        {
          id: 4,
          title: 'DoS（拒绝服务）',
          description: 'dtcpb: 接收方向TCP序列号\ndwin: TCP窗口大小（接收方向）\ntcprtt: 往返时间的和 (round-trip time)\nsynack: SYN和ACK之间的时间（连接建立时）\nackdat: ACK发送和接收之间的时间\nsmean: 发送数据包平均大小',
          images: [pic4]
        },
        {
          id: 5,
          title: 'Fuzzers（模糊测试工具）',
          description: 'dmean: 接收数据包平均大小\ntrans_depth: 传输的深度（一般用于HTTP的多部分传输）\nresponse_body_len: 响应体的长度\nct_srv_src: 连接到同一目的服务的数量\nct_state_ttl: 根据TTL值，连接过程中的状态数量',
          images: [pic5]
        },
        {
          id: 6,
          title: 'Reconnaissance（侦查）',
          description: 'ct_dst_ltm: 目的IP地址的连接数\nct_src_dport_ltm: 源IP到目的端口的连接数量\nct_dst_sport_ltm: 目的IP到源端口的连接数量\nct_dst_src_ltm: 目的IP到源IP的连接数量\nis_ftp_login: 是否为通过FTP登录的会话\nct_ftp_cmd: FTP命令的数量。',
          images: [pic6]
        },
        {
          id: 7,
          title: 'Analysis（分析）',
          description: 'ct_flw_http_mthd: HTTP流的数量（例如GET, POST请求）\nct_src_ltm: 来自同一源IP的连接数量\nct_srv_dst: 连接到同一目的IP的服务数量\nis_sm_ips_ports: 源和目的IP和端口是否相同。',
          images: [pic7]
        },
      ]
    };
  },
  mounted() {
    this.startCarousel();
    //
    this.startInterval();
    this.chartInstance = echarts.init(this.$refs.chart);
    this.chartInstance.setOption(this.chartOptions);
    //hot map
    // this.loadAndRenderHeatmap();
    // this.getData();
  },
  beforeDestroy() {
    if (this.chartInstance) {
      this.chartInstance.dispose();
    }
  },
  methods: {
    startCarousel() {
      this.carouselInterval = setInterval(() => {
        if (this.currentIndex < this.waikuangs.length - 1) {
          this.currentIndex++;
          console.log(this.currentIndex);
        } else {
          this.currentIndex = 0;
        }
      }, 3000); // 每3秒切换一个waikuang
    },
    pauseCarousel(index) {
      clearInterval(this.carouselInterval);
      this.carouselInterval = null;
      this.currentIndex = index; // 设置当前激活的waikuang
      console.log(this.currentIndex);
    },
    resumeCarousel() {
      if (!this.carouselInterval) {
        this.startCarousel();
      }
    },
    selectWaikuang(index) {
      this.currentIndex = index; // 设置当前激活的waikuang
      this.restartCarousel(); // 重启轮播
    },

    restartCarousel() {
      // 先清除现有的定时器
      this.pauseCarousel();
    },
    //
    //hotmap
    loadAndRenderHeatmap() {
        axios.get('http://127.0.0.1:5000/hotmap')
          .then(response => {
                // console.log(response.data);
                const responseData = response.data;
                console.log(responseData.x_axis);
                console.log(responseData.data);
                console.log("chenggon");
                // const heatmapData = [];
                // for (let i = 0; i < responseData.index.length; i++) {
                //   for (let j = 0; j < responseData.columns.length; j++) {
                //     heatmapData.push([
                //       j, 
                //       i, 
                //       responseData.data[i][j] !== null ? responseData.data[i][j] : '-'
                //     ]);
                //   }
                // }
                // console.log(responseData.columns);
                // this.initHeatmap(responseData.columns, responseData.index, heatmapData);
                // 基于准备好的DOM，初始化echarts实例
                this.echart = echarts.init(this.$refs.heatmap);

                // 指定图表的配置项和数据
                const option = {
                  tooltip: {
                    position: 'top'
                  },
                  grid: {
                    height: '50%',
                    top: '10%'
                  },
                  xAxis: {
                    type: 'category',
                    data: responseData.x_axis, // X轴数据
                  },
                  yAxis: {
                    type: 'category',
                    data: responseData.x_axis, // Y轴数据
                  },
                  visualMap: {
                    min: 0,
                    max: 10,
                    calculable: true,
                    orient: 'horizontal',
                    left: 'center',
                    bottom: '15%'
                  },
                  series: [{
                    name: 'Punch Card',
                    type: 'heatmap',
                    data: responseData.data, // 热力图数据
                    label: {
                      show: true
                    },
                    emphasis: {
                      itemStyle: {
                        shadowBlur: 10,
                        shadowColor: 'rgba(0, 0, 0, 0.5)'
                      }
                    }
                  }]
                };

                // 使用刚指定的配置项和数据显示图表。
                this.echart.setOption(option);
              })
              .catch(error => {
                console.error('Error fetching the heatmap data:', error);
              });
      },
      initHeatmap(columns, index, data) {
      this.chart = echarts.init(this.$refs.heatmap);
      const option = {
        tooltip: {
          position: 'top'
        },
        grid: {
          height: '50%',
          y: '10%'
        },
        xAxis: {
          type: 'category',
          data: columns,
          splitArea: {
            show: true
          }
        },
        yAxis: {
          type: 'category',
          data: index,
          splitArea: {
            show: true
          }
        },
        visualMap: {
          min: 0,
          max: 1, // 根据您的数据范围设置
          calculable: true,
          orient: 'horizontal',
          left: 'center',
          bottom: '15%'
        },
        series: [{
          name: 'Punch Card',
          type: 'heatmap',
          data: data,
          label: {
            show: true
          },
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }]
      };
      this.chart.setOption(option);
    },
    // getData(){
    //   // 发送 POST 请求到后端 Flask 服务器
    //   axios.post('http://127.0.0.1:5000/hotmap')
    //     .then(response => {
    //       // 处理请求成功的响应
    //       console.log(response.data);
    //       this.$message('训练已开始');
    //       this.responseData = response.data; // 存储返回的数据
    //       // this.responseData = JSON.stringify(this.responseData);
    //       // console.log(this.responseData);
    //       // this.showTrainCompleteDialog(); // 显示训练完成提示窗口
    //     })
    //     .catch(error => {
    //       // 处理请求失败的情况
    //       console.error('请求失败：', error);
    //       this.$message.error('请求失败，请重试');
    //     });
    // },
    getBackgroundStyle(imageUrl) {
      return {
        backgroundImage: `url(${imageUrl})`,
        backgroundSize: 'contain',
        backgroundRepeat: 'no-repeat',
        backgroundPosition: '90% center',
        // margin: '0 300px 0 0'
      };
    },
    handleDragOver(event) {
      event.target.style.borderColor = 'rgb(27, 250, 216)'
    },
    handleDragLeave(event) {
      event.target.style.borderColor = 'grey'
    },
    handleFileDrop(event) {
      const files = event.dataTransfer.files
      if (files.length > 0) {
        this.processFile(files[0])
      }
      event.target.style.borderColor = 'grey'
    },
    handleFileChange(event) {
      const files = event.target.files
      if (files.length > 0) {
        this.processFile(files[0])
      }
    },
    processFile(file) {
      if (file.type !== 'text/csv') {
        alert('请上传CSV文件')
        return
      }// 在这里处理文件...
    },
    ////////////////////////
    startInterval() {
      this.interval = setInterval(this.next, 3000); // 每3秒切换一次
    },
    pause() {
      clearInterval(this.interval);
    },
    play() {
      this.startInterval();
    },
    next() {
      this.activeIndex = (this.activeIndex + 1) % this.modules.length;
    },
    prev() {
      this.activeIndex = (this.activeIndex + this.modules.length - 1) % this.modules.length;
    },
    created() {
      this.play();
    },
    //hot map
    // getData() {
    //   axios.get('/data').then(response => {
      //   this.resultData = JSON.parse(response.data);
      //   const { data, labels } = resultData;
      //   // const { data, labels } = response.data;
      //   this.renderChart(data, labels);
      // });
       // 成功获取数据后，在这里使用 ECharts 渲染热力图
    //       const { data, labels } = response.data;
    //       this.renderChart(data, labels);
    //     })
    //     .catch(error => {
    //       // 处理错误
    //       console.error('There was an error!', error);
    //     });
    // },
    renderChart(data, labels) {
          const option = {
            // ... 其他配置项，参考之前提供的 ECharts 配置
            xAxis: {
              type: 'category',
              data: labels,
            },
            yAxis: {
              type: 'category',
              data: labels,
            },
            series: [{
              name: '热力图',
              type: 'heatmap',
              data: data,
              // ... 其他配置项
            }]
          };
          this.Chart.setOption(option);
        }
      },
      computed: {
        ...mapGetters([
          'name'
        ])
      },
      
    }
</script>

<style lang="scss" scoped>

//@import url('https://download.qingteng.cn/frontendcdn/www/css/commons-f1b90652f38bae6ba7d6.css');
//@import url('https://download.qingteng.cn/frontendcdn/www/css/intrusion-detect-f1b90652f38bae6ba7d6.css');
@import url('../../styles/textstyle.css');
@import url('../../styles/background.css');
@import url('../../styles/commons.css');
@import url('../../styles/scrollscreen.css');
.dashboard {
  &-container {
    margin: 30px;
  }

  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
//滚屏第二个数据结果
/* 确保 .carousel-container 相对定位以便箭头可以绝对定位于它 */
.carousel-containerr {
    position: relative;
    /* 其他必要的样式 */
  }
  
  /* 默认情况下隐藏箭头，但允许鼠标悬停时显示 */
  .arroww {
    opacity: 0; /* 默认透明，隐藏箭头 */
    visibility: hidden; /* 默认隐藏箭头 */
    position: absolute; /* 绝对定位 */
    height: 120px; /* 高度撑满整个容器 */
    top: 0; /* 从顶部开始 */
    border: none;
    transition: opacity 0.3s, visibility 0.3s; /* 平滑的透明度和可见性过渡 */
    cursor: pointer; /* 鼠标悬停时的指针样式 */
    background-color: #cccccc; /* 浅灰色背景 */
    z-index: 10; /* 确保箭头显示在其他内容之上 */
    display: flex; /* 使用flex布局来垂直和水平居中箭头图标 */
    align-items: center; /* 垂直居中 */
    justify-content: center; /* 水平居中 */
  }
  
  /* 鼠标悬停在 .carousel-container 上时显示箭头 */
  .carousel-containerr:hover .arroww {
    opacity: 0.5; /* 鼠标悬停时半透明 */
    visibility: visible; /* 显示箭头 */
  }
  
  /* 左箭头的特定样式 */
  .carousel-containerr .prev {
    left: 0; /* 紧贴左边界 */
    width: 50px; /* 箭头的宽度，根据需要调整 */
    /* 使用深灰色箭头图标 */
    color: #bbbbbb; /* 箭头颜色比背景深 */
  }
  
  /* 右箭头的特定样式 */
  .carousel-containerr .next {
    right: 0; /* 紧贴右边界 */
    width: 50px; /* 箭头的宽度，根据需要调整 */
    /* 使用深灰色箭头图标 */
    color: #bbbbbb; /* 箭头颜色比背景深 */
  }
  .pre-wrap {
    white-space: pre-wrap;
  }
  



  .carousel-containerrr {
    position: relative;
    width: 500px;
    height: 500px;
  }
  
  /* 默认情况下隐藏箭头，但允许鼠标悬停时显示 */
  .arrow {
    opacity: 0; /* 默认透明，隐藏箭头 */
    visibility: hidden; /* 默认隐藏箭头 */
    position: absolute; /* 绝对定位 */
    height: 100%; /* 高度撑满整个容器 */
    top: 0; /* 从顶部开始 */
    border: none;
    transition: opacity 0.3s, visibility 0.3s; /* 平滑的透明度和可见性过渡 */
    cursor: pointer; /* 鼠标悬停时的指针样式 */
    background-color: #cccccc; /* 浅灰色背景 */
    z-index: 10; /* 确保箭头显示在其他内容之上 */
    display: flex; /* 使用flex布局来垂直和水平居中箭头图标 */
    align-items: center; /* 垂直居中 */
    justify-content: center; /* 水平居中 */
  }
  
  /* 鼠标悬停在 .carousel-container 上时显示箭头 */
  .carousel-containerrr:hover .arrow {
    opacity: 0.5; /* 鼠标悬停时半透明 */
    visibility: visible; /* 显示箭头 */
  }
  
  /* 左箭头的特定样式 */
  .carousel-containerrr .prev {
    left: 0; /* 紧贴左边界 */
    width: 50px; /* 箭头的宽度，根据需要调整 */
    /* 使用深灰色箭头图标 */
    color: #bbbbbb; /* 箭头颜色比背景深 */
  }
  
  /* 右箭头的特定样式 */
  .carousel-containerrr .next {
    right: 0; /* 紧贴右边界 */
    width: 50px; /* 箭头的宽度，根据需要调整 */
    /* 使用深灰色箭头图标 */
    color: #bbbbbb; /* 箭头颜色比背景深 */
  }

.dashboard-container{
  .title {
  font-size: 50px;
  margin-left:25% ;
}
// 虚框CSS
.upload-drag-area {
  border: 2px dashed grey;
  padding: 20px;
  text-align: center;
  transition: border-color 0.3s;
  &:hover {
    border-color: rgb(27, 250, 216);
  }
}
//线条
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
  

.chart-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%; /* 或者使用具体的高度 */
  overflow: hidden;
}
.chart-containerr {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%; /* 或者使用具体的高度 */
  width: 100%;
  overflow: hidden;
}
.chart-containerrr {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%; /* 或者使用具体的高度 */
  width: 100%;
  overflow: hidden;
}

.waikuang{
  transition: transform 0.3s ease;
  display: flex;
  align-items: center;
  height: 100px;
  width: 450px;
  border:1px solid #000;
  // border-radius: 30%;
  border-radius: 15px;
  margin: 20px 15px;
  background-color: #0000; 
  box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2); 
  padding: 10px 25px;
}
.waikuang.active, .waikuang:hover {
  transform: scale(1.05);
}
.artistic-font {
  font-family: 'Pacifico', cursive; /* 使用谷歌字体或任何特殊字体 */
  font-size: 27px; /* 较大的字体大小 */
  font-weight: bold; /* 粗体 */
  color: #000000; /* 深棕色 */
  text-shadow: 2px 2px 4px #d1d0d0; /* 文字阴影 */
  letter-spacing: 3px; /* 字符间距 */
  line-height: 2; /* 行高 */
  // text-decoration: underline; /* 文字下划线 */
}
.artistic-fontt {
  font-family: 'Pacifico', cursive; /* 使用谷歌字体或任何特殊字体 */
  font-size: 27px; /* 较大的字体大小 */
  font-weight: bold; /* 粗体 */
  color: #000000; /* 深棕色 */
  text-shadow: 2px 2px 4px #d1d0d0; /* 文字阴影 */
  letter-spacing: 1px; /* 字符间距 */
  line-height: 2; /* 行高 */
  // text-decoration: underline; /* 文字下划线 */
}
.waikuangg{
  display: flex;
  align-items: center;
  height: 400px;
  width: 450px;
  border:1px solid #000;
  // border-radius: 30%;
  border-radius: 15px;
  margin: 20px 15px;
  background-color: #0000; 
  box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.2); 
  padding: 10px 25px;
}
.text-style-1lines {
	display: -webkit-box;
	overflow: hidden;
	-webkit-line-clamp: 1;
	-webkit-box-orient: vertical;
}
.heading-style-h5{
  color:var(--dark-purple-400);
  font-family:Golostext Variablefont Wght,sans-serif;
  font-size:1.5rem;
  font-weight:500;
  line-height:1.4
}

.slide-enter-active, .slide-leave-active {
  transition: all 0.5s ease;
}
// .slide-enter, .slide-leave-to /* .slide-leave-active for <2.1.8 */ {
//   transform: translateX(100%);
//   position: absolute;
//   opacity: 0;
// }

// .slide-move,
// .slide-enter-active,
// .slide-leave-active {
//     transition: all 0.5s ease;
// }

.slide-enter-from {
    transform: scale(0);
    opacity: 0;
}

.slide-enter-to {
    transform: scale(1);
    opacity: 1;
}

.slide-leave-to {
    transform: translateX(100%)!important;
    opacity: 0;
}

.slide-leave-active {
    position: absolute;
}

.panel-group {
  margin-top: 50px;

  .card-panel-col {
    margin-bottom: 32px;
  }

  .card-panel {
    height: 108px;
    cursor: pointer;
    font-size: 12px;
    position: relative;
    overflow: hidden;
    color: #666;
    background: #ffffff;
    box-shadow: 4px 4px 40px rgba(0, 0, 0, 0.05);
    border-color: rgba(0, 0, 0, 0.05);

    &:hover {
      .card-panel-icon-wrapper {
        color: #fff;
      }

      .icon-people {
        background: #40c9c6;
      }

      .icon-message {
        background: #36a3f7;
      }

      .icon-money {
        background: #f4516c;
      }

      .icon-view {
        background: #ffb900;
      }
    }

    .icon-people {
      color: #40c9c6;
    }

    .icon-message {
      color: #36a3f7;
    }

    .icon-money {
      color: #f4516c;
    }

    .icon-view {
      color: #ffb900;
    }

    .card-panel-icon-wrapper {
      float: left;
      margin: 14px 0 0 14px;
      padding: 16px;
      transition: all 0.38s ease-out;
      border-radius: 6px;
    }

    .card-panel-description {
      float: right;
      font-weight: bold;
      margin: 26px;
      margin-left: 0px;

      .card-panel-text {
        line-height: 18px;
        color: #909399;
        font-size: 16px;
        margin-bottom: 12px;
      }
    }
  }
}
}

</style>
