<template>
  <div>
    <b-row v-show="working" class="position-fixed bg-dark overlay">
      <b-col class="text-center">
        <b-spinner class="mb-3" variant="success" style="width: 4rem; height: 4rem;"/>
        <h2 class="text-light">Working...</h2>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <b-form-group>
          <b-form-file
              id="file-large"
              :state="Boolean(file)"
              v-model="file"
              accept="image/jpeg, image/png"
              placeholder="Choose a file or drop it here..."
              drop-placeholder="Drop file here..."
              size="md"/>
        </b-form-group>
      </b-col>
    </b-row>
    <b-row>
      <b-col cols="6">
        <b-row>
          <b-col>
            <b-row class="mb-3">
              <b-col v-for="(h, index) in hist" :key="index">
                <image-frame :src="h" caption="Histogram Red" :obj="h" @select="show(h)"/>
              </b-col>
            </b-row>
            <b-row class="mb-3">
              <b-col v-for="(file, index) in row1 " :key="index">
                <image-frame :src="file.name" :caption="file.caption" :obj="file.name" @select="show(file.name)"/>
              </b-col>
            </b-row>
          </b-col>
        </b-row>
        <b-row>
          <b-col v-for="(file, index) in row2" :key="index">
            <image-frame :src="file" caption="Filtered Non-leaf" :obj="file" @select="show(file)"/>
          </b-col>
        </b-row>
      </b-col>
      <b-col cols="4">
        <image-frame :src="files.original" img-style="max-height:300px; width:auto;" caption="Original" :obj="files.original"/>
      </b-col>
      <b-col>
        <div class="p-0" style="max-height: 1000px; overflow-x: hidden; overflow-y: scroll">
          <image-frame
              class="mb-2"
              v-for="image in images" :key="image.name"
              :src="image.path"
              :destroy-button="true"
              :obj="image.name"
              v-on:destroy="destroy"
              v-on:select="process"/>
        </div>
      </b-col>
    </b-row>

    <b-modal v-model="showModal" title="View Image" ok-only header-bg-variant="primary" header-text-variant="white">
      <img :src="view"/>
    </b-modal>
  </div>
</template>

<script>
import axios from "axios";
import ImageFrame from "../components/ImageFrame";

const API_URL = process.env.VUE_APP_API_URL;
const MAX_UPLOAD_SIZE = 5.0;

export default {
  name: "ImageViewer",
  components: {ImageFrame},
  data() {
    return {
      view: "",
      showModal: false,
      working: true,
      files: {
        "original": "",
        "hist-red": "",
        "hist-green": "",
        "hist-blue": "",
        "pass": "",
        "leaf": "",
        "tip": "",
        "red": "",
        "green": "",
        "edge": "",
        "earth": "",
        "blur": "",
      },
      file: null,
      images: [],
    };
  },
  computed: {
    hist() {
      return [
        this.files["hist-red"],
        this.files["hist-green"],
        this.files["hist-blue"],
      ];
    },
    row1() {
      return [
        {name: this.files.edge, caption: "Sobel Operator"},
        {name: this.files.blur, caption: "Vertical Blur"},
        {name: this.files.pass, caption: "High Pass"},
        {name: this.files.tip, caption: "Top Location"},
      ];
    },
    row2() {
      return [
        this.files.red,
        this.files.earth,
        this.files.green,
        this.files.leaf,
      ];
    },
  },
  watch: {
    file(val) {
      const fileInMB = val.size / 1024 / 1024;
      if (fileInMB > MAX_UPLOAD_SIZE) {
        alert(`Maxium file file is ${MAX_UPLOAD_SIZE}MB, yours is ${fileInMB}MB`);
        return;
      }
      this.working = true;
      const formData = new FormData();
      formData.append("file", val);
      this.api.post("/images", formData, {
        headers: {
          "Content-Type": "multipart/form-data",
        },
      }).then(res => {
        this.thumblist();
        this.loadFiles(res.data);
        this.working = false;
      }).catch(error => {
        alert(error.response.data.title);
        this.working = false;
      })
    },
  },
  methods: {
    show(url) {
      this.view = url;
      this.showModal = true;
    },
    process(name) {
      this.working = true;
      this.api.put("/images/" + name)
        .then(response => {
          this.loadFiles(response.data);
          this.working = false;
        });
    },
    destroy(name) {
      this.working = true;
      this.api.delete("/images/" + name)
        .then(() => {
          const image = this.images.filter(image => image.name === name)[0];
          this.images.splice(this.images.indexOf(image), 1);
          if (this.files.original.indexOf(name) !== -1) {
            this.reset();
          }
          this.working = false;
        });
    },
    loadFiles(files) {
      files.forEach(file => {
        this.files[file.element] = API_URL + file.path;
      });
    },
    thumblist() {
      this.working = true;
      while (this.images.length > 0) {
        this.images.pop();
      }
      this.api.get("/images")
        .then(response => {
          response.data.forEach(file => {
            file.path = API_URL + file.path;
            this.images.push(file);
          })
          this.working = false;
        }).catch(err => console.log(err));
    },
    reset() {
      Object.keys(this.files)
        .forEach(file => {
          this.files[file] = API_URL + "/static/leaf.png";
        });
    },
  },
  created() {
    this.reset();
    this.api = axios.create({baseURL: API_URL + "/api"});
    this.thumblist();
  },
};
</script>

<style scoped>
.overlay {
  z-index: 1000;
  right: 0;
  left: 0;
  margin-right: auto;
  margin-left: auto;
  opacity: 0.95;
  height: 100%;
  padding-top: 15%;
  vertical-align: middle;
}
</style>