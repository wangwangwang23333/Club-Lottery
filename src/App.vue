<template>
  <div style="background-image: url('http://www.ahjinzhai.gov.cn/_res/images/body-bg.jpg');
  background-size: 100%;
  background-repeat: no-repeat;
  height:100vh !important;
  text-align: center;
  ">
      <el-dialog
      title="淘汰历史"
      :visible.sync="dialogVisible"
      width="30%">
      <el-table
      :data="tableData"
      stripe
      height="250"
      style="width: 100%">
      <el-table-column
        prop="index"
        label="淘汰序号"
        width="180">
      </el-table-column>
      <el-table-column
        prop="name"
        label="昵称"
        width="180">
      </el-table-column>
    </el-table>
      <span slot="footer" class="dialog-footer" style="text-align: center;">
        <el-button type="primary" @click="dialogVisible = false" style="margin-right: 40%;">确 定</el-button>
      </span>
    </el-dialog>
      <div style=" background-image: linear-gradient(135deg, #2b332b 0%, #2b332b 3%, #147bfc4a 3%);
      height: 100%;
      backdrop-filter: blur(15px);
      ">
          <el-row :gutter="20" style="padding-top: 5%;">
              <el-col :span="14" style="text-align: left;">
                  <div v-for="(item,index) in name" :key="index" style="display: inline-block; width: 10%;margin-left: 2%;
                  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;margin-top: 3%;text-align: center;">
                      
                      <div v-if="index!=curNumber"
                      >
                          <div style="position:relative">
                              <el-image :src="require('@/assets/avatar/'+(index+1)+'.png')" 
                              style="width:5vw;" >
                              </el-image>
                              <el-image :src="require('@/assets/out.png')" 
                              style="position:relative;
                              top:-20%;
                              margin-top: -200%;margin-bottom: 0;width: 5vw;" 
                              v-if="!survive[index]">
                              </el-image>
                          </div>
      
                          
                          <div style="margin-top: 5%;margin-bottom: 5%;" 
                          v-if="survive[index]">{{item}}</div>
                          <div style="margin-top: 5%;margin-bottom: 5%;text-decoration: line-through;" 
                          v-else>{{item}}</div>
                      </div>
                      <div v-else style="box-shadow: rgb(85, 91, 255) 0px 0px 0px 3px, rgb(31, 193, 27) 0px 0px 0px 6px, rgb(255, 217, 19) 0px 0px 0px 9px, rgb(255, 156, 85) 0px 0px 0px 12px, rgb(255, 85, 85) 0px 0px 0px 15px;">
                          <div style="position:relative">
                              <el-image :src="require('@/assets/avatar/'+(index+1)+'.png')" 
                              style="width:5vw;" >
                              </el-image>
                              <el-image :src="require('@/assets/out.png')" 
                              style="position:relative;
                              top:-20%;
                              margin-top: -200%;margin-bottom: 0;width: 4vw;" 
                              v-if="!survive[index]">
                              </el-image>
                          </div>
      
                          
                          <div style="margin-top: 5%;margin-bottom: 5%;" 
                          v-if="survive[index]">{{item}}</div>
                          <div style="margin-top: 5%;margin-bottom: 5%;text-decoration: line-through;" 
                          v-else>{{item}}</div>
                      </div>
                  </div>
              </el-col>
              <el-col :span="3" v-if="startLottery">
                  <div>约瑟夫数为：
                      <br>
                      <h1 style="color: white;">{{yuesefu}}</h1>
                  </div>
                  
                  <div>当前计数：
                      <br>
                      <h3 style="color: white;">{{curCount}}</h3>
                  </div>
                  <div>剩余人数：<br>
                      <el-progress type="circle" 
                      :percentage="remainPercent" :width="100"
                      :format="format"></el-progress>
                  </div>
                  <!--控制台-->
                  <!--<el-button @click="randomGoAway()">随机淘汰一个人</el-button>-->
                  <br>
                  <el-button @click="yuesefuGo()" style="margin-top: 3%;">约瑟夫淘汰</el-button>
                  <div style="margin-top: 15%;">
                    当前速度：
                    <el-input-number 
                    style="margin-top: 10%;width: 75%;"
                    v-model="curSpeed" :min="1" :max="10" label="描述文字"></el-input-number>
                  </div>
                  <el-button @click="dialogVisible=true" style="margin-top: 20%;">淘汰历史</el-button>
              </el-col>

              <el-col :span="6" v-else>
                <el-button @click="changeLottery()"
                style="margin-top: 50vh;
                margin-left:15vw;width: 10vw;height: 10vh;">开始抽奖</el-button>
              </el-col>
  
              <el-col :span="7" v-if="startLottery">
                  <div>
                      <h1>一等奖</h1>
                      <div v-if="prizes[0][0]!=-1">
                          <el-image :src="require('@/assets/avatar/'+(prizes[0][0]+1)+'.png')" 
                          style="width:100%" 
                          >
                          </el-image>
                          {{name[prizes[0][0]]}}
                      </div>
                      <div v-else>
                          <el-image :src="require('@/assets/avatar/none.png')" 
                              style="width:5vw" >
                          </el-image>
                          <br>
                          虚位以待
                      </div>
                  </div>
                  <div>
                      <h2>二等奖</h2>
                      <div v-for="(item,index) in prizes[1]" :key="index" style="width: 15%;display: inline-block;margin-left: 5%;">
                          <div v-if="prizes[1][index]!=-1">
                              <el-image :src="require('@/assets/avatar/'+(prizes[1][index]+1)+'.png')" 
                              style="width:100%"
                              >
                              </el-image>
                              {{name[prizes[1][index]]}}
                          </div>
                          <div v-else>
                              <el-image :src="require('@/assets/avatar/none.png')" 
                                  style="width:100%" >
                              </el-image>
                              <br>
                              虚位以待
                          </div>
                      </div>
                  </div>
                  <div>
                      <h3>三等奖</h3>
                      <div v-for="(item,index) in prizes[2]" :key="index" style="width: 10%;display: inline-block;margin-left: 5%;">
                          <div v-if="prizes[2][index]!=-1">
                              <el-image :src="require('@/assets/avatar/'+(prizes[2][index]+1)+'.png')" 
                              style="width:100%"
                              >
                              </el-image>
                              {{name[prizes[2][index]]}}
                          </div>
                          <div v-else>
                              <el-image :src="require('@/assets/avatar/none.png')" 
                                  style="width:100%" >
                              </el-image>
                              <br>
                              虚位以待
                          </div>
                      </div>
                  </div>
  
              </el-col>
          </el-row>
      </div>

  </div>
</template>
<script>

  export default {
    data() {
      return {
        //开始抽奖
        startLottery:false,
        //展示淘汰名单
        dialogVisible:false,
        //淘汰名单
        tableData: [],
        //约瑟夫数字
        yuesefu: 7,
        //当前计数
        curCount: 0,
        //当前数字
        curNumber: 0,
        remainPerson: 0,
        timeCount: null,
        //当前播放速度
        curSpeed:1,
        name: [
          "徐满心",
          "毛哥",
          "www",
          "归海记忆",
          "OliverShang",
          "redefinition",
          "+1",
          "Joe",
          "加油",
          "左脑",
          "??"
        ],
        survive: [

        ],
        prizes: [
          [-1],
          [-1, ],
          [-1, -1, -1]
        ],
        prizeCount: 0
      }
    },
    created() {
      for (let i = 0; i < this.name.length; ++i) {
        this.survive.push(true)
      }
      this.remainPerson = this.name.length
      for (let i = 0; i < this.prizes.length; ++i) {
        this.prizeCount += this.prizes[i].length
      }
    },
    computed: {
      remainPercent() {
        return Math.round(this.remainPerson / this.name.length * 100)
      }
    },
    methods: {
      changeLottery(){
        this.startLottery = true;
      },
      format(percentage) {
        percentage;
        return `${this.remainPerson} / ${this.name.length}`;
      },
      getPrize(index) {
        if (this.remainPerson < this.prizeCount) {
          this.prizeCount -= 1
          //把最后面的奖给它
          for (let i = this.prizes.length - 1; i >= 0; --i) {
            for (let j = 0; j < this.prizes[i].length; ++j) {
              if (this.prizes[i][j] == -1) {
                this.prizes[i][j] = index
                this.$set(this.prizes[i], j, index)
                //播放获得物品音效
                this.playHappy();
                return
              }
            }
          }

        }
        else {
          this.playFail()
        }
      },
      randomGoAway() {
        //从尚未淘汰的人里淘汰一个人
        if (!this.canGoAway()) {
          this.$message({
            message: '所有人都已经被淘汰了',
            type: 'warning'
          });
          return
        }
        let index = 0
        while (index < this.survive.length && !this.survive[index]) {
          index = Math.round(Math.random() * this.survive.length);
        }
        this.remainPerson -= 1
        this.$set(this.survive, index, false)
        this.$message({
          message: '淘汰了【' + this.name[index] + '】',
          type: 'warning'
        });
      },
      canGoAway() {
        //需要至少有一个人尚未被淘汰
        for (let i = 0; i < this.name.length; ++i) {
          if (this.survive[i]) {
            return true
          }
        }
        return false
      },
      getNextNumber() {
        let index = (this.curNumber + 1) % this.name.length
        while (!this.survive[index]) {
          index = (index + 1) % this.name.length
        }
        return index
      },
      playSE() {
        let audio = new Audio()
        audio.src = require('@/assets/go.mp3')
        audio.play();
      },
      playFail() {
        let audio = new Audio()
        audio.src = require('@/assets/die.wav')
        audio.play();
      },
      playHappy() {
        let audio = new Audio()
        audio.src = require('@/assets/yes.mp3')
        audio.play();
      },
      yuesefuGo() {
        if(this.timeCount!=null){
          this.$message({
            message: '请等待当前淘汰结束',
            type: 'warning'
          });
          return
        }
        if (!this.canGoAway()) {
          this.$message({
            message: '所有人都已经被淘汰了',
            type: 'warning'
          });
          return
        }
        //往后走一定的人数
        this.timeCount = setInterval(() => {
          this.curCount += 1
          this.curNumber = this.getNextNumber()

          if (this.curCount == this.yuesefu) {
            clearInterval(this.timeCount)
            this.timeCount = null
            this.curCount = 0
            //淘汰这个人
            this.remainPerson -= 1
            this.$set(this.survive, this.curNumber, false)
            this.$message({
              message: '淘汰了【' + this.name[this.curNumber] + '】',
              type: 'warning'
            });
            //加入淘汰历史
            this.tableData.splice(0,0,{
              index:this.remainPerson+1,
              name:this.name[this.curNumber]
            })
            //this.tableData.push()
            this.getPrize(this.curNumber)

          }
          else {
            this.playSE()
          }
        }, 300/this.curSpeed);
      }
    }
  }
</script>