<template>
  <div id="app" class="container">
    <div class="row">
          <div class="col-md-3"></div>
          <div class="col-md-6">
            <img src="https://www.iconfinder.com/data/icons/weather-flat-14/64/weather02-512.png" alt="" width="250">
          </div>
          <div class="col-md-3"></div>
      </div>
    <div class="row">
      
      <div class="col-md-10">
        <b-input v-model="text" placeholder="เชียงใหม่" size="lg"></b-input>
      </div>
      <b-button size="lg" @click="getitem">OK</b-button>
    </div>

    <div class="row">
      <div>
        <p>°C</p>
      </div>
      <div class="ml-2">
        <b-form-checkbox v-model="checked" name="check-button" switch />
      </div>
      <div>
        <p>°F</p>
      </div>
    </div>

    <div class="row mt-5">
      <div class="col-md-2"></div>
      <div class="col-md-8" v-if="tmp != null" >
        <b-card :img-src="Img " img-left class="mb-3" img-width="500px" img-height="auto">
          <b-card-text>
            <h3>{{$text}}</h3>
            <br />

            <h3 v-if="checked==false">{{c}} °C</h3>
            <h3 v-if="checked==true">{{f}} °F</h3>

            <br />

            <h3>{{$cond}}</h3>
          </b-card-text>
        </b-card>
      </div>
      <div class="col-md-2"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      checked: false,
      $text: "",
      c: "",
      f: "",
      tmp: null,
      text: "",
      condN: null,
      $cond: "",
      cond: [
        "ท้องฟ้าแจ่มใส",
        "มีเมฆบางส่วน",
        "เมฆเป็นส่วนมาก",
        "มีเมฆมาก",
        "ฝนตกเล็กน้อย",
        "ฝนปานกลาง",
        "ฝนตกหนัก",
        "ฝนฟ้าคะนอง",
        "อากาศหนาวจัด",
        "อากาศหนาว",
        "อากาศเย็น",
        "อากาศร้อนจัด",
      ],
      condimg: [
        "https://pbs.twimg.com/media/D8mJeNcUIAIkHqz.jpg",
        "https://www.scholarship.in.th/wp-content/uploads/2016/02/cloudy_d-e618500.jpg",
        "https://cache.desktopnexus.com/thumbseg/441/441199-bigthumbnail.jpg",
        "https://p0.pikist.com/photos/792/871/cloudscape-clouds-sky-atmosphere-cloudy-sky.jpg",
        "https://isarapost.com/wp-content/uploads/2020/07/2-11.jpg",
        "https://fbi.dek-d.com/26/1129146/113354783",
        "https://obs.line-scdn.net/0hZZOJPu-1BXVyMy325DV6IkhlBhpBXxZ2FgVUdi5dW0ELV0crHVFKQF5mDhFYCkIrHAVDFFI1HkQNAENxTVBK/w644",
        "https://hilight.kapook.com/img_cms2/user/surattana/shutterstock_123174094%20%281%29.jpg",
        "https://f.ptcdn.info/470/065/000/pw5r7lwkaxCS7KtxO96-o.jpg",
        "https://hilight.kapook.com/img_cms2/user/surauch/Test/de_2.jpg",
        "https://cms.kapook.com/uploads/tag/6/ID_5595_57bbf5f75f7eb.jpg,",
        "https://mpics.mgronline.com/pics/Images/562000004025401.JPEG",
      ],
      Img: "",
    };
  },
  methods: {
    getitem() {
      var config = {
        headers: {
          accept: "application/json",
          authorization:
            "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6Ijc1YjhkN2I3NDNlYmQ1ODQxYmZhNWFmZTU2YWU4ZTZlN2E3YjRlNjYyNzY2MWM3MDViNWNhYzFkMWI0ZjBmMTA2ZTk4MWIxMzlhNWJkNDM1In0.eyJhdWQiOiIyIiwianRpIjoiNzViOGQ3Yjc0M2ViZDU4NDFiZmE1YWZlNTZhZThlNmU3YTdiNGU2NjI3NjYxYzcwNWI1Y2FjMWQxYjRmMGYxMDZlOTgxYjEzOWE1YmQ0MzUiLCJpYXQiOjE2MDA3NTU0ODQsIm5iZiI6MTYwMDc1NTQ4NCwiZXhwIjoxNjMyMjkxNDg0LCJzdWIiOiI5ODIiLCJzY29wZXMiOltdfQ.kMWhYFQtybu2Z4X6DXli0IxvOojW2-cyD9uMaRoVMOjml3CQ6uUHqYhR1r-c1gYj2uoIXBNwn3BIjxcz06nTA-v9m36VboeHPkYuKbc5t408QxMoZMREcT43TLQkawdu4uiFlYOqT2M4cmR-ccPsdGVkTgvJoEwX8VjH3FSbKCCsRlQrwM1dWzXrM-avg9b_rrd_0xRSsU_M9g0plarOIFzZgAX4rREBvkyPX9Uq2_sjTtii2zjybdwfRGBKLpmDKjZTgfFs4FvJszoTdSp1KhnNbyP-cNoa74kjQ4ebTGqI0m4s5gZAE-1de9ij_Uu_4-sOrSvPZvQhQjC8Iga_mWmpkSpc510KcQCvvc8HY8JXQealFK9MWjICnR0C6mPumhmZpqzgW8kAhx57915QoJNeA2Hj9UjsYAqdrS5wl74S4OzUCpURL49NQyn3ggtt5UY_xLnkfjFQbq7rH8519qPVxkzTymZjhctPbQuqV5UEifj_vec9UG01d9pGs4kdELNZqCURw-VNn571DwCL53UGjA2MyTh9wqwPYrDm6G7fHlekuD_PRwM8pranjFSWKvOhZzeq-NfPVZe1hKv3aMmiF9AJ3LZHaamPbKi1xN6KTVQ1FBQo84Z8i6vSv9CLNhRcpvxQB-rmXT8e1yZr_8MRZ4zE_CcVkyvxe8nUrBA",
        },
      };
      axios
        .get(
          "https://data.tmd.go.th/nwpapi/v1/forecast/location/daily/place?province=" +
            this.text,
          config
        )
        .then((res) => {
          this.tmp = res.data.WeatherForecasts[0].forecasts[0].data;
          this.c = this.tmp.tc;
          this.f = this.tmp.rh;
          this.condN = this.tmp.cond - 1;
          this.$cond = this.cond[this.condN];
          this.Img = this.condimg[this.condN];
          this.$text = this.text;
          console.log(this.tmp);
        })
        .catch((err) => {
          console.error(err);
        });
    },
  },
};
</script>

<style>
.app{
 font-family: 'Prompt', sans-serif;
}
</style>