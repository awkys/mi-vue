<template>
  <div class="main" id="pay">
    <h1>Payment</h1>
    <div class="topfront">
      <p>Pay with your debit or credit card.</p>
      <img style="margin-left: 0.3em;margin-top: 1%" src="../assets/imgs/card-logo/visa.svg" alt="Visa" width="32">
      <img style="margin-left: 0.3em" src="../assets/imgs/card-logo/mastercard.svg" alt="Visa" width="32">
      <img style="margin-left: 0.3em" src="../assets/imgs/card-logo/amex.svg" alt="Visa" width="32">
      <img style="margin-left: 0.3em" src="../assets/imgs/card-logo/discover.svg" alt="Visa" width="32">
    </div>
    <el-form>
      <div class="cardnumberfront">
        <p>Card number*</p>
        <el-input type="text" class="cardinput" placeholder="**** **** **** ****" v-model="cardid"></el-input>
      </div>
      <div class="expirefront">
        <p>Expiry (MM/YY)*</p>
        <el-input type="text" class="expireinput" placeholder="MM / YY" v-model="mmyy"></el-input>
      </div>
      <div class="cvvfront">
        <p>Card code *</p>
        <el-input type="text" class="cvvinput" placeholder="cvv" v-model="cvv"></el-input>
      </div>
      <div class="line"></div>
      <div class="inrto">
        <p>Your personal data will be used to process your order, support your experience throughout this website, and
          for other purposes described in our privacy policy.</p>
      </div>
      <el-form-item class="agree">
        <el-checkbox v-model="agree">I have read and agree to the website terms and conditions *</el-checkbox>
      </el-form-item>
      <div class="fade" v-if="showModal" @click="showModal=false"></div>
      <div class="succ-pop" v-if="showModal">
        <div class="loading"></div>
        <h1 class="process">Your order's being processed.</h1>
      </div>

      <el-form-item>
        <el-button class="button" @click="paymt">place order</el-button>
      </el-form-item>

    </el-form>
  </div>
</template>

<!--<script type="text/javascript" src="jquery-1.8.3.min.js"></script>-->
<script>
// import settle from "@/views/Settle";
export default {
  name: "pay",
  data() {
    return {
      showModal: false,
      cardid: "",
      mmyy: "",
      cvv: "",
      agree: "",
      // form:this.$route.params
    }
  },
  // created(){
  // this.paymt()
  // },
  methods: {
    paymt() {
      this.showModal = true;
      const TIME_COUNT = 2;
      if (!this.timer) {
        this.count = TIME_COUNT;
        this.show = false;
        this.timer = setInterval(() => {
          // this.timer = setTimeout(() => {
          if (this.count > 0 && this.count <= TIME_COUNT) {
            this.count--;
          } else {
            this.show = true;
            this.showModal = false;
            clearInterval(this.timer);
            this.timer = null;
            this.checkInfo();
          }
        }, 1000)
      }

    },
    checkInfo() {
      this.$axios
          .post("/api/users/checkinfo", {
            email: this.$route.params.email,
            emailme: this.$route.params.emailme,
            firstname: this.$route.params.firstname,
            lastname: this.$route.params.lastname,
            company: this.$route.params.company,
            country: this.$route.params.country,
            address: this.$route.params.address,
            city: this.$route.params.city,
            province: this.$route.params.province,
            postal: this.$route.params.postal,
            phone: this.$route.params.phone,
            cardid: this.cardid,
            mmyy: this.mmyy,
            cvv: this.cvv,
            agree: this.agree,
            nodes: this.nodes
          })
          .then(res => {
            //跳转的页面写在此处
            if (res.data.code === "001") {
              // setTimeout(()=>this.showModal = false,1000)
              this.$router.push({
                path: '/order'
              });
              // 隐藏注册组件
              this.isRegister = false;
              // 弹出通知框提示注册成功信息
              this.notifySucceed(res.data.msg);
            } else {
              // setTimeout(()=>this.showModal = false,1000)
              // 弹出通知框提示注册失败信息
              // this.notifyError(res.data.msg);
              this.$router.push({
                path: 'settle'
              });
              // 弹出通知框提示注册失败信息
              this.notifyError(res.data.msg);

            }
          })
          .catch(err => {
            return Promise.reject(err);
          });
    }
  }
}
</script>


<style scoped>
.main {
  float: left;
  padding-left: 15%;
}

.main .topfront {
  padding-top: 1%;
  padding-bottom: 1%;
}

.main .cardnumberfront {
  padding-top: 1%;
}

.main .cardinput {
  width: 90%;
  padding-top: 1%;
}

.main .expirefront {
  padding-top: 1%;
}

.main .expireinput {
  width: 90%;
  padding-top: 1%;
}

.main .cvvfront {
  padding-top: 1%;
}

.main .cvvinput {
  width: 10%;
  padding-top: 1%;
}

.main .line {
  position: relative;
  margin-top: 3%;
  margin-bottom: 2%;
  /*margin: 0 auto;*/
  width: 100%;
  height: 2px;
  background-color: #d4d4d4;
  /*text-align: center;*/
  /*font-size: 16px;*/
  color: rgba(101, 101, 101, 1);
}

.main .inrto {
  color: #b0b0b0;
  padding-top: 1%;
}

.main .agree {
  padding-top: 1%;
}

.main .button {
  background-color: #1b1f23;
  width: 15%;
  /*padding-top: 1%;*/
  font-size: 20px;
  color: #ffff
  /*higth:5%;*/
}

/*灰色遮罩层*/
.main .fade {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  left: 0;
  top: 0;
  z-index: 99;
}

/*弹出层*/
.main .succ-pop {
  width: 400px;
  height: 300px;
  background: #fff;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -200px;
  margin-top: -150px;
  z-index: 999;
  border-radius: 5px;
}

.main .succ-pop .loading {
  /*position: relative;*/
  width: 100px;
  height: 100px;
  margin-left: 150px;
  margin-top: 80px;
  /*left: 40%;x/*/
  /*position: fixed;*/
  /*left: 10%;*/
  /*top: 20%;*/
  /*margin-left: 60px;*/
  /*margin-top: 80px;*/
  border: 10px solid #90EE90;
  border-top-color: rgba(0, 0, 0, 0.2);
  border-right-color: rgba(0, 0, 0, 0.2);
  border-bottom-color: rgba(0, 0, 0, 0.2);
  border-radius: 100%;

  animation: circle infinite 0.75s linear;
}

.main .succ-pop .process {
  padding-left: 7%;
  padding-top: 10%;
}

/* 转转转动画*/
@keyframes circle {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>