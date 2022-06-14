<template>
  <div id="test">
    <!-- :set="effect='art:incognito'" -->
    <div>
      <span>Effect value: {{ effect }}</span>
    </div>
    <div>
      <input type="file" @change="upload" />
      <cld-image :public-id="public_id" id="imgPic">
        <cld-transformation :effect="effect"   />
      </cld-image>
    </div>
    <div>
      图片效果：
      <select @change="changeStyle" name="selectChoices" id="choices">
        <option value="art:incognito">黑白</option>
        <option value="art:athena">模糊</option>
        <option value="art:zorro">复古</option>
      </select>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      public_id: "",
      effect: "",
    };
  },
  methods: {
    upload: function (e) {
      var _this = this;
      // 获取当前上传的文件
      var files = e.target.files[0];
      // console.log(files);
      if (!e || !window.FileReader) return; // 判断是否支持FileReader
      let reader = new FileReader();
      reader.readAsDataURL(files); // 文件转换
      reader.onloadend = function () {
        const url = "https://api.cloudinary.com/v1_1/demo/image/upload";
        const formData = new FormData();
        formData.append("file", files);
        formData.append("upload_preset", "docs_upload_example_us_preset");
        fetch(url, {
          method: "POST",
          body: formData,
        })
          .then((response) => {
            return response.text();
          })
          .then((data) => {
            data = JSON.parse(data);
            _this.public_id = data.public_id;
            _this.effect = this.effect;
            // console.log(data);
          });
      };
    },
    changeStyle: function (event) {
      this.effect = event.target.value;
      var cloudinary = document.getElementById("imgPic");
      console.log("this.effect", this.effect);
      console.log("cloud", cloudinary);
    },
  },
};
</script>
<style>
#test div {
  margin: 20px;
}

</style>