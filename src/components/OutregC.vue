<template>
  <v-container class="grey lighten-5">
    <v-form ref="form_reg" v-model="valid" lazy-validation>
      <v-card class="mx-auto" max-width="99%" min-width="100%">
        <v-img
          class="white--text"
          height="60px"
          :src="require('../assets/outreg.jpg')"
        >
          <v-card-title class="align-end fill-height">门诊挂号</v-card-title>
        </v-img>
        <v-card-text>
          <v-layout row wrap>
            &emsp;&emsp;
            <v-flex d-flex>
              <v-text-field
                v-model="out_reg.ex_pid"
                label="条码号(可以为空)"
                @input="expidChanged($event)"
              >
              </v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.patient_type"
                label="患者类别"
                required
                :items="patient_types"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '患者类别不能为空']"
                hide-details
                prepend-icon="map"
                @input="patientTypeChanged($event)"
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.patient_name"
                label="患者姓名"
                required
                :counter="6"
                :rules="nameRules"
                :disabled="isdisabled_patient_name"
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.idcard"
                label="身份证号(读卡或扫码获取)"
                disabled
              ></v-text-field
              >&emsp;&emsp;
            </v-flex>
          </v-layout>
          <v-layout row wrap>
            &emsp;&emsp;
            <v-flex d-flex>
              <v-text-field
                v-model="out_reg.pid"
                label="门诊号(确认挂号生成)"
                disabled
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.gender"
                :items="genders"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '患者性别不能为空']"
                label="性别"
                hide-details
                prepend-icon="info"
                required
              ></v-select>
            </v-flex>
            <v-flex>
              <v-layout row wrap>
                <v-flex d-flex>
                  &emsp;&emsp;
                  <v-text-field
                    v-model="out_reg.age_y"
                    label="年龄(岁)"
                  ></v-text-field>
                </v-flex>
                <v-flex d-flex>
                  &emsp;&emsp;
                  <v-text-field
                    v-model="out_reg.age_m"
                    label="年龄(月)"
                  ></v-text-field>
                </v-flex>
                <v-flex d-flex>
                  &emsp;&emsp;
                  <v-text-field
                    v-model="out_reg.age_d"
                    label="年龄(天)"
                  ></v-text-field
                  >&emsp;&emsp;
                </v-flex>
              </v-layout>
            </v-flex>
          </v-layout>
          <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.reg_type"
                label="挂号类别"
                required
                :items="reg_types"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '挂号类别不能为空']"
                hide-details
                prepend-icon="donut_small"
                @input="reg_typeChanged($event)"
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.dept_code"
                :items="dept_codes"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '就诊科室不能为空']"
                label="就诊科室"
                hide-details
                prepend-icon="group_work"
                required
                @input="dept_codeChanged($event)"
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.doctor_code"
                :items="doctor_codes"
                item-text="item-text"
                item-value="item-value"
                label="专家医师"
                hide-details
                prepend-icon="account_box"
                single-line
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-switch
                v-model="out_reg.visit_priority"
                label="就诊优先"
              ></v-switch>
            </v-flex>
          </v-layout>
          <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addr_prov"
                :items="addr_provs"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '住址省份不能为空']"
                label="住址(省份)"
                hide-details
                prepend-icon="business"
                required
                @input="prov_Changed"
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addr_city"
                :items="addr_citys"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '住址-市不能为空']"
                label="住址(市)"
                hide-details
                prepend-icon="business"
                required
                @input="city_Changed"
              ></v-select>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addr_county"
                :items="addr_countys"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '住址-区县不能为空']"
                label="住址(区县)"
                hide-details
                prepend-icon="business"
                required
                @input="county_Changed"
              ></v-select
              >&emsp;&emsp;
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-select
                v-model="out_reg.addr_township"
                :items="addr_townships"
                item-text="item-text"
                item-value="item-value"
                :rules="[v => !!v || '住址-街道社区不能为空']"
                label="住址(街道社区)"
                hide-details
                prepend-icon="business"
                required
              ></v-select
              >&emsp;&emsp;
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.addr_house_nmb"
                label="住址(详细地址)"
              ></v-text-field>
            </v-flex>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-text-field
                v-model="out_reg.company"
                label="工作单位"
              ></v-text-field>
              &emsp;&emsp;
            </v-flex>
          </v-layout>
          <!-- <v-spacer></v-spacer>           -->
          <v-layout row wrap>
            <v-flex d-flex>
              &emsp;&emsp;
              <v-card class="pa-4" tile elevation="18" width="95%">
                &emsp;&emsp;&emsp;&emsp;
                <div class="title regfee">
                  挂号费合计:&nbsp;&nbsp;{{
                    out_reg.reg_price + out_reg.check_price
                  }}&emsp;&emsp;&emsp;&emsp;
                </div>
                <div class="subtitle-1 regfee">
                  &emsp;&emsp;其中挂号费:&nbsp;&nbsp;{{ out_reg.reg_pPrice }}
                </div>
                <div class="subtitle-1 regfee">
                  &emsp;&emsp;其中诊察费:&nbsp;&nbsp;{{ out_reg.check_price }}
                </div>
              </v-card>
            </v-flex>
          </v-layout>

          <v-layout row wrap>
            <v-flex d-flex
              >&emsp;&emsp;
              <v-card class="pa-4 m-4" elevation="18" title>
                <div>
                  <video
                    ref="video"
                    id="video"
                    width="640"
                    height="480"
                    autoplay
                  ></video>
                </div>
              </v-card>
            </v-flex>
            <v-flex d-flex>
              <v-card class="pa-4" elevation="18" tile width="100%">
                <v-list dense>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title mb-1"
                        >患者医保信息</v-list-item-title
                      >
                      <v-list-item-subtitle
                        >医保读卡获取信息</v-list-item-subtitle
                      >
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="subtitle-1"
                        >医保个人编号:{{ out_reg.micard }}</v-list-item-title
                      >
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="subtitle-1"
                        >医保卡余额:{{
                          out_reg.mi_pacc_left
                        }}</v-list-item-title
                      >
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="subtitle-1"
                        >医保类别:{{ out_reg.mi_type }}</v-list-item-title
                      >
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="subtitle-1"
                        >参保单位:{{ out_reg.mi_company }}</v-list-item-title
                      >
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="subtitle-1"
                        >读卡信息:{{ out_reg.mi_str }}</v-list-item-title
                      >
                    </v-list-item-content>
                  </v-list-item>
                </v-list> </v-card
              >&emsp;&emsp;
            </v-flex>
          </v-layout>
        </v-card-text>

        <v-card-actions class="justify-center">
          <v-row no-gutters>
            <v-col><v-spacer></v-spacer></v-col>
            <v-col cols="3">
              <v-radio-group row v-model="out_reg.pay_type">
                <v-radio
                  key="cash"
                  label="现金"
                  color="success"
                  value="cash"
                ></v-radio>
                <v-radio key="wechat" label="微信" value="wechat"></v-radio>
                <v-radio key="alipay" label="支付宝" value="alipay"></v-radio>
              </v-radio-group>
            </v-col>
            <v-col cols="8">
              <v-btn
                id="snap"
                :disabled="!valid"
                color="success"
                @click="capture"
                >&emsp;&emsp;拍&emsp;照&emsp;&emsp;</v-btn
              >
              <v-btn
                color="success"
                :style="display_btn_readhealth"
                @click="validate"
                >读 健 康 卡</v-btn
              >
              <v-btn
                :style="display_btn_readmi"
                color="success"
                v-on:click="readcardClicked($event)"
                >读 医 保 卡</v-btn
              >
              <v-btn
                :disabled="!valid"
                color="success"
                @click="outregcashClicked($event)"
                >确 认 挂 号</v-btn
              >
              <v-btn
                color="warning"
                :disabled="!valid"
                @click="prnClicked($event)"
                >打印挂号单</v-btn
              >
              <v-btn
                :disabled="!valid"
                color="success"
                @click="schweixinClicked($event)"
                >查询支付订单
              </v-btn>
            </v-col>
          </v-row>
          <!-- <v-btn color="warning" @click="reset">下一位</v-btn> -->
        </v-card-actions>
      </v-card>
    </v-form>
    <v-divider></v-divider>
    <v-spacer>&emsp;</v-spacer>

    <canvas ref="canvas" id="canvas" width="640" height="480" hidden></canvas>
    <v-row no-gutters>
      <v-col cols="12" sm="12">
        <v-row no-gutters>
          <v-col><img v-bind:src="this.out_reg_pic.pic1" height="240"/></v-col>
          <v-col><img v-bind:src="this.out_reg_pic.pic2" height="240"/></v-col>
          <v-col><img v-bind:src="this.out_reg_pic.pic3" height="240"/></v-col>
        </v-row>
        <v-row no-gutters>
          <v-col><img v-bind:src="this.out_reg_pic.pic4" height="240"/></v-col>
          <v-col><img v-bind:src="this.out_reg_pic.pic5" height="240"/></v-col>
          <v-col><img v-bind:src="this.out_reg_pic.pic6" height="240"/></v-col>
        </v-row>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col cols="12" sm="12">
        <v-parallax
          height="700"
          dark
          :src="require('../assets/img/blank_cash.jpg')"
        >
          <div id="print_reg">
            <v-card
              class="pa-2"
              outlined
              style="background-color: lightgrey;"
              tile
            >
              挂号单打印样式
            </v-card>
          </div>
        </v-parallax>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import {
  getpatient_type,
  get_regopcode,
  getgender,
  getid_type,
  getreg_type,
  getdept_codes,
  getdoctor_codes,
  getprovs,
  getcitys,
  getcountys,
  getstreets,
  getpatient,
  readcard_mi,
  outreg_cash,
  // outreg_weixin,
  outreg_pic,
  sch_weixin,
  getregprice
} from "../scripts/outreg.js";

export default {
  data: () => ({
    valid: true,
    nameRules: [
      v => !!v || "姓名不能为空",
      v => (v && v.length >= 2) || "姓名长度不能少于2个汉字"
    ],
    //barcodeRules: [
    //  v => !!v || "条形码不能为空",
    //  v => (v && v.length >= 13) || "条形码应该为13位数字"
    //],
    patient_types: [], //患者类别列表
    genders: [], //性别列表
    idcard_types: [], //身份证件类型列表
    dept_codes: [], //就诊科室列表
    doctor_codes: [], //专家号专家列表
    reg_types: [], //挂号类别
    addr_provs: [], //单位或住址(省份)
    addr_citys: [], //单位或住址(市)
    addr_countys: [], //单位或住址(区县)
    addr_townships: [], //单位或住址(街道)
    out_reg: {
      hsp_code: process.env.VUE_APP_HSP_CODE,
      pid: "", //门诊号
      pid_type: "O", //患者标识类别
      ex_pid: "", //条形码
      patient_name: "", //患者姓名
      gender: "", //性别代码
      age_y: 0, // 年龄
      age_m: 0, //  年龄
      age_d: 0, //  年龄
      patient_type: "", //患者类型
      reg_type: "pz", // 挂号类别
      reg_price: 0.0, //挂号费
      check_price: 0.0, //诊察费
      visit_priority: "1", //就诊优先标志
      dept_code: "", //就诊科室
      doctor_code: "", //门诊接诊医生
      idcard: "", //患者身份证号码
      idcard_type: "jmsfz", //患者身份证件类别
      addr_prov: process.env.VUE_APP_HSP_PROV, //地址
      addr_city: process.env.VUE_APP_HSP_CITY,
      addr_county: process.env.VUE_APP_HSP_COUNTY,
      addr_township: "",
      addr_street: "",
      addr_house_nmb: "",
      company: "",
      mi_pacc_left: 0.0,
      mi_company: "",
      mi_str: "",
      mi_type: "",
      micard: "",
      pay_type: "cash", //支付方式选择
      reg_opcode: "", //挂号员
      pay_cash: 0.0, //现金支付金额
      pay_pacc: 0.0, //医保(农合)个人帐户支付金额
      pay_fund: 0.0, //医保(农合)统筹支付金额
      pay_nfc: 0.0, //移动支付金额
      invoice_nmb: "", //挂号单收据流水号
      flow_nmb: "", //挂号单操作员流水号
      mch_ip: "", //本机局域网IP地址
      ver: process.env.VUE_APP_VERSION //版本号
    },
    video: {},
    canvas: {},
    //captures: [],
    capture_num: 0,
    out_reg_pic: {
      hsp_code: process.env.VUE_APP_HSP_CODE,
      pid: "",
      ex_pid: "",
      patient_name: "",
      idcard: "",
      health_id: "",
      micard: "",
      capture_time: "",
      capture_opid: "",
      mch_ip: "",
      pic1: "",
      pic2: "",
      pic3: "",
      pic4: "",
      pic5: "",
      pic6: ""
    },
    display_btn_readhealth: "display:none",
    display_btn_readmi: "display:none",
    isdisabled_patient_name: false,
    tgc: ""
  }),
  created() {
    this.out_reg.reg_opcode = get_regopcode().split("|")[0];
    this.tgc = get_regopcode().split("|")[1];
    this.patient_types = getpatient_type(this.out_reg.reg_opcode, this.tgc);
    this.genders = getgender(this.out_reg.reg_opcode, this.tgc);
    this.idcard_types = getid_type(this.out_reg.reg_opcode, this.tgc);
    this.reg_types = getreg_type(this.out_reg.reg_opcode, this.tgc);
    this.dept_codes = getdept_codes(this.out_reg.reg_opcode, this.tgc);
    this.addr_provs = getprovs(this.out_reg.reg_opcode, this.tgc);
    this.addr_citys = getcitys(
      process.env.VUE_APP_HSP_PROV,
      this.out_reg.reg_opcode,
      this.tgc
    );
    this.addr_countys = getcountys(
      process.env.VUE_APP_HSP_CITY,
      this.out_reg.reg_opcode,
      this.tgc
    );
    this.addr_townships = getstreets(
      process.env.VUE_APP_HSP_COUNTY,
      this.out_reg.reg_opcode,
      this.tgc
    );
  },
  mounted() {
    this.video = this.$refs.video;
    if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
      navigator.mediaDevices.getUserMedia({ video: true }).then(stream => {
        this.video.srcObject = stream;
        this.video.play();
      });
    }
  },
  beforeDestroy() {
    // navigator.getUserMedia({video: true},
    // function(stream) {
    //      // can also use getAudioTracks() or getVideoTracks()
    //     var track = stream.getTracks()[0];  // if only one media track
    //     track.stop();
    //     console.log("关闭完成");
    // },
    // function(error){
    //     console.log('getUserMedia() error', error);
    // });
  },
   destroyed: function () {
    this.video.srcObject.getTracks()[0].stop(); //关闭摄像头
  },
  methods: {
    validate() {
      if (this.$refs.form.validate()) {
        this.snackbar = true;
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    expidChanged(e) {
      let texpid = e;
      console.log("texpid e=" + e);
      if (texpid.length == 9) {
        //输入的是患者主索引号
        getpatient(texpid, this.out_reg.reg_opcode, this.tgc).then(data => {
          this.out_reg = data;
        });
      }
    },
    //------------------医保类别医保1*读卡显示,农合2*显示读健康卡--------------
    patientTypeChanged(e) {
      let tptype = e;
      console.log("tptype e=" + tptype);
      this.display_btn_readmi = "display:none";
      this.display_btn_readhealth = "display:none";
      this.isdisabled_patient_name = false;
      if (tptype.slice(0, 1) == "1") {
        //医保
        this.display_btn_readmi = "";
        this.isdisabled_patient_name = true;
      }
      if (tptype.slice(0, 1) == "2") {
        //农合
        this.display_btn_readhealth = "";
      }
    },
    readcardClicked(e) {
      console.log("e=" + e.target.innerText);
      readcard_mi();
    },
    outregcashClicked(e) {
      console.log("确认挂号e=" + e);
      if (!this.$refs.form_reg.validate()) {
        window.alert("请选择必须填写的字段");
        return;
      }
      outreg_cash(this.out_reg, this.out_reg.reg_opcode, this.tgc).then(
        data => {
          this.out_reg.pid = data;
          this.out_reg.ex_pid = data.substr(0, 9);
          console.log(
            "outregcashClicked this.out_reg_pic.pid=" + this.out_reg_pic.pid
          );
          this.out_reg_pic.pid = this.out_reg.pid;
          this.out_reg_pic.ex_pid = this.out_reg.ex_pid;
          this.out_reg_pic.patient_name = this.out_reg.patient_name;
          this.out_reg_pic.idcard = this.out_reg.idcard;
          this.out_reg_pic.health_id = this.out_reg.health_id;
          this.out_reg_pic.micard = this.out_reg.micard;
          this.out_reg_pic.capture_opid = this.out_reg.reg_opcode;
          outreg_pic(this.out_reg_pic, this.out_reg.reg_opcode, this.tgc);
        }
      );
    },
    schweixinClicked(e) {
      console.log("e=" + e.target.innerText);
      sch_weixin();
    },
    dept_codeChanged(e) {
      let tdept_code = this.out_reg.dept_code;
      console.log("dept_codeChanged e=" + e);
      let tpost_tech = "1";
      this.doctor_codes = getdoctor_codes(
        tdept_code,
        tpost_tech,
        this.out_reg.reg_opcode,
        this.tgc
      );
    },
    reg_typeChanged(e) {
      console.log(
        "reg_typeChanged e=" + e + " |treg_type=" + this.out_reg.reg_type
      );
      let treg_type = this.out_reg.reg_type;
      getregprice(treg_type, this.out_reg.reg_opcode, this.tgc).then(data => {
        this.out_reg.reg_price = data[0];
        this.out_reg.check_price = data[1];
      });
    },
    //------------------获取指定省份的市列表---------------------------
    prov_Changed() {
      let tprovid = this.out_reg.addr_prov;
      this.addr_citys = getcitys(tprovid, this.out_reg.reg_opcode, this.tgc);
    },
    //------------------获取指定市的区县列表---------------------------
    city_Changed() {
      let tcityid = this.out_reg.addr_city;
      this.addr_countys = getcountys(
        tcityid,
        this.out_reg.reg_opcode,
        this.tgc
      );
    },
    //------------------获取指定区县的街道列表---------------------------
    county_Changed() {
      let tcountyid = this.out_reg.addr_county;
      this.addr_townships = getstreets(
        tcountyid,
        this.out_reg.reg_opcode,
        this.tgc
      );
    },
    //-------------------打印挂号单--------------------------------------------------
    prnClicked(e) {
      console.log("e=" + e.target.innerText);
      this.$htmlToPaper("print_reg");
    },
    capture() {
      this.canvas = this.$refs.canvas;
      var ctx = this.canvas.getContext("2d");
      //console.log(this.$refs.canvas.toDataURL("image/png"));
      ctx.drawImage(this.video, 0, 0, 640, 480);
      //this.captures.push(this.$refs.canvas.toDataURL("image/png"));
      switch (this.capture_num) {
        case 0:
          this.out_reg_pic.pic1 = this.$refs.canvas.toDataURL("image/png", 0.3);
          this.capture_num++;
          break;
        case 1:
          this.out_reg_pic.pic2 = this.$refs.canvas.toDataURL("image/png", 0.3);
          this.capture_num++;
          break;
        case 2:
          this.out_reg_pic.pic3 = this.$refs.canvas.toDataURL("image/png", 0.3);
          this.capture_num++;
          break;
        case 3:
          this.out_reg_pic.pic4 = this.$refs.canvas.toDataURL("image/png", 0.3);
          this.capture_num++;
          break;
        case 4:
          this.out_reg_pic.pic5 = this.$refs.canvas.toDataURL("image/png", 0.3);
          this.capture_num++;
          break;
        case 5:
          this.out_reg_pic.pic6 = this.$refs.canvas.toDataURL("image/png", 0.3);
          this.capture_num++;
          break;
        default:
          this.capture_num = 0;
          this.out_reg_pic.pic1 = this.$refs.canvas.toDataURL("image/png", 0.3);
      }
      //--
    }
    // ---------------------end methods----------------
  }
};
</script>

<style>
.regfee {
  display: inline;
}
</style>
