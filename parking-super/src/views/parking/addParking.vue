<template>
  <div class="addParking">
    <h1 class="addParking-title">添加停车场</h1>
    <Form ref="formValidate" :label-width="100" class="addFrom">
      <FormItem label="停车场名称：" prop="name">
        <Input v-model="parking.name" placeholder="输入停车场名称"></Input>
      </FormItem>
      <FormItem label="车位数量：" prop="number">
        <InputNumber v-model="parking.number"></InputNumber>
      </FormItem>
      <FormItem label="停车场位置：" prop="map">
        <Input v-model="parking.map" placeholder="输入地址" class="form-map"></Input>
      </FormItem>
      <FormItem label="详情：" prop="details">
        <Input v-model="parking.details" type="textarea" :autosize="{minRows: 2,maxRows: 4}"></Input>
      </FormItem>
      <FormItem>
        <Button type="primary" :style="{marginRight:'20px'}" @click="addparking">提交</Button>
        <Button @click="goback">返回</Button>
      </FormItem>
    </Form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      url: this.$store.state.url,
      parking: {
        name: "",
        number: 0,
        map:"",
        details: ""
      }
    };
  },
  methods: {
    goback() {
      this.$router.go(-1);
    },
    addparking() {
      let _this = this;
      this.axios({
        method: "post",
        url: _this.url + "garage/info",
        data: {
          garageName: _this.parking.name,
          latitudeAndLongitude: _this.parking.map,
          sumSapce: _this.parking.number,
          attribute: "地下计费",
          details: _this.parking.details
        }
      })
        .then(res => {
          _this.$Message.success("添加成功");
          _this.$router.replace("parking");
        })
        .catch(err => {
          if (err.toString().slice(-3) == 400) {
            _this.$Message.error("请填写完整");
          } else {
            _this.$Message.error("服务器出错，请稍后再试");
          }
        });
    }
  }
};
</script>

<style scoped>
.addParking-title {
  text-align: center;
}
.addFrom {
  margin: 20px auto;
  width: 800px;
}
.form-map {
  width: 30%;
  margin-right: 20px;
}
</style>


