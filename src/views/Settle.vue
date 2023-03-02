<!--
订单结算地址详情页
-->
<!--<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">-->
<template>
  <div id="info">
    <div class="info_left">
      <el-form :inline="true">
        <p>Contact information</p>
        <br>
        <el-form-item>
          <el-input type="text" style="width:200%;" placeholder="Email" v-model="addform.email"></el-input>
        </el-form-item>
        <br>
        <el-form-item>
          <el-checkbox v-model="addform.emailme">Email me with news and offers</el-checkbox>
        </el-form-item>
        <br>
        <p>Shipping address</p>
        <br>
        <el-form-item>
          <el-select v-model="country" id="ctyp" placeholder="Country/Region" style="width:190%;" @change="getcty">
            <el-option v-for="option in countrys"
                       :key="option.ctyCode"
                       :value="option.cty">{{ option.cty }}
            </el-option>
          </el-select>
        </el-form-item>
        <br>
        <el-form-item>
          <el-input type="text" placeholder="First name" v-model="addform.firstname"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" placeholder="Last name" v-model="addform.lastname"></el-input>
        </el-form-item>
        <br>
        <el-form-item>
          <el-input type="text" style="width:210%;" placeholder="Company (optional)"
                    v-model="addform.company"></el-input>
        </el-form-item>
        <br>
        <el-form-item>
          <el-input type="text" style="width:210%;" placeholder="Address" v-model="addform.address"></el-input>
        </el-form-item>
        <br>
        <el-form-item>
          <el-input type="text" placeholder="City" v-model="addform.city"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" placeholder="province" v-model="addform.province"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input type="text" placeholder="Postal Code" v-model="addform.postal"></el-input>
        </el-form-item>
        <br>
        <!--      <el-form-item>-->
        <!--        <el-input type="text"  placeholder="Bank card number" v-model="addform.cardid"></el-input>-->
        <!--      </el-form-item>-->
        <!--      <el-form-item>-->
        <!--        <el-input type="text"  placeholder="cvv" v-model="addform.cvv"></el-input>-->
        <!--      </el-form-item>-->
        <!--      <br>-->
        <el-form-item>
          <el-input type="text" style="width:320%;" placeholder="Phone" v-model="addform.phone"></el-input>
        </el-form-item>
        <br>
        <el-form-item class="buttonloc">
          <!--        <router-link to="{path:'/pay',query:{set:data}}"  style="background-color: #197BBD;color: #e0e0e0">Continue to shipping</router-link>-->
          <el-button style="background-color: #197BBD;color: #e0e0e0" @click="topay">Continue to shipping</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="info_right">
      <p>Order notes (optional)</p>
      <br>
      <el-form>
        <el-form-item class="nodes">
          <el-input type="text" placeholder="Nodes about your orser" v-model="addform.nodes"></el-input>
        </el-form-item>
      </el-form>
    </div>
  </div>

</template>

<script>
export default {
  name: "info",
  data() {
    return {
      country: "UNITED STATES",
      countrys: [
        {ctyCode: 0, cty: "UNITED STATES"},
      ],
      addform: {
        "email": "",
        "emailme": "",
        "firstname": "",
        "lastname": "",
        "company": "",
        "address": "",
        "city": "",
        "province": "",
        "postal": "",
        "phone": "",
        "nodes": ""
      },
      rules: {
        emailnd: [{required: true, message: "email address is a required field.", trigger: 'blur'}]
      }
    };
  },
  methods: {
    getcty(value) {
      const cou = this.countrys.find((a) => a.ctyCode === value);
      this.country = cou.cty;
    },
    topay() {
      this.$router.push(
          {
            name: 'pay',
            params: {
              email: this.addform.email,
              emailme: this.addform.emailme,
              firstname: this.addform.firstname,
              lastname: this.addform.lastname,
              company: this.addform.company,
              address: this.addform.address,
              city: this.addform.city,
              province: this.addform.province,
              postal: this.addform.postal,
              phone: this.addform.phone,
              // cardid:this.addform.cardid,
              // cvv:this.addform.cvv,
              country: this.country,
              nodes: this.nodes
            }
          }
      )
    }
  }
}

</script>

<style scoped>
.info_left {
  float: left;
  padding-left: 10%;
  padding-right: 10%;
  padding-top: 1%;
  padding-bottom: 2%;
}

.info_left .buttonloc {
  padding-left: 70%;
}

.info_right {
  float: left;
  /*padding-left: 10%;*/
  padding-right: 10%;
  padding-top: 1%;
  padding-bottom: 2%;
}

.info_right .nodes {
  width: 200%;
}

</style>