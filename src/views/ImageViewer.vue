<template>
  <div>
  <b-row>
    <!-- Content -->
    <b-col>
      <b-row v-show="working" class="position-fixed bg-dark overlay">
        <b-col class="text-center">
          <b-spinner class="mb-3" variant="success" style="width: 4rem; height: 4rem;"/>
          <h2 class="text-light">Working...</h2>
        </b-col>
      </b-row>

      <b-row>
        <b-col>
          <b-card class="shadow-sm" header-class="pl-2 pr-2 pt-1 pb-1" header="Upload Image" no-body bg-variant="dark" header-bg-variant="primary" header-text-variant="white">
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
          </b-card>
        </b-col>
      </b-row>

      <b-row class="mb-3">
        <b-col>
          <b-row class="mb-3">
            <b-col>
              <b-card class="shadow-sm" body-class="p-1" header-class="pl-2 pr-2 pt-1 pb-1" header="Pipeline" border-variant="success" bg-variant="dark" header-bg-variant="success" header-text-variant="white">
                <b-row>
                  <b-col v-for="(file, index) in row1 " :key="index">
                    <image-frame variant="success" :src="file.image.src + '?' + file.image.bump" :caption="file.caption" :obj="file" @select="show(file.image.src)"/>
                    <b-row>
                      <b-col sm="8">
                        <b-form-input class="m-1" v-model="filterValues[file.filter.key]" @mouseup="process({name: openFile.name})" type="range" :min="file.filter.min" :max="file.filter.max" :step="file.filter.steps"/>
                      </b-col>
                      <b-col sm="2">
                        <span class="badge badge-success">{{filterValues[file.filter.key]}}</span>
                      </b-col>
                    </b-row>
                  </b-col>
                </b-row>
              </b-card>
            </b-col>
          </b-row>

          <b-row>
            <b-col>
              <b-card class="shadow-sm" body-class="p-1" header-class="pl-2 pr-2 pt-1 pb-1" header="Segmentation" border-variant="info" bg-variant="dark" header-bg-variant="info" header-text-variant="white">
                <b-row>
                  <b-col v-for="(file, index) in row2" :key="index">
                    <image-frame variant="info" :src="file.src" caption="Filtered Non-leaf" :obj="file" @select="show(file.src)"/>
                  </b-col>
                </b-row>
              </b-card>
            </b-col>
          </b-row>
        </b-col>
      </b-row>

      <b-row>
        <b-col>
          <b-card class="shadow-sm" body-class="p-1" header-class="pl-2 pr-2 pt-1 pb-1" header="Histograms" border-variant="danger" bg-variant="dark" header-bg-variant="danger" header-text-variant="white">
            <b-row>
              <b-col v-for="(h, index) in hist" :key="index">
                <image-frame variant="danger" :src="h.src" caption="Histogram Red" :obj="h" @select="show(h.src)"/>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
      </b-row>
    </b-col>
    <!-- Content end -->

    <!-- Sidebar -->
    <b-col cols="2">
      <b-card class="shadow-sm" header-class="p-2" header="Available images" body-class="p-1" bg-variant="dark" header-bg-variant="primary" border-variant="primary" header-text-variant="white">
        <div>
          <image-frame
              variant="primary"
              :class="{'mt-1' : index > 0}"
              v-for="(image, index) in images" :key="image.name"
              :src="image.path"
              :destroy-button="true"
              :obj="image"
              v-on:destroy="destroy"
              v-on:select="process"/>
        </div>
      </b-card>
    </b-col>
    <!-- Sidebar end -->

  </b-row>
    <b-row>
      <b-col>
        <b-card class="mt-3 shadow-sm" body-class="p-1" header-class="pl-2 pr-2 pt-1 pb-1" header="Whole Pipeline (one image)" border-variant="secondary" bg-variant="dark" header-bg-variant="secondary" header-text-variant="white">
          <b-row>
            <b-col>
              <b-img fluid-grow :src="files.pipeline.src + '?' + files.pipeline.bump"/>
            </b-col>
          </b-row>
        </b-card>
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
      openFile: {name},
      files: {
        "pipeline": {src: "", bump: ""},
        "original": {src: "", bump: ""},
        "hist-red": {src: "", bump: ""},
        "hist-green": {src: "", bump: ""},
        "hist-blue": {src: "", bump: ""},
        "pass": {src: "", bump: ""},
        "leaf": {src: "", bump: ""},
        "tip": {src: "", bump: ""},
        "red": {src: "", bump: ""},
        "green": {src: "", bump: ""},
        "edge": {src: "", bump: ""},
        "earth": {src: "", bump: ""},
        "blur": {src: "", bump: ""},
      },
      filterValues: {
        thetaTheshold: 0.83,
        whiteThreshold: 25,
        blurRounds: 40,
        scanlineInterpolationWidth: 0,
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
        {image: this.files.edge, caption: "Sobel Operator", filter: {min: 0.0, max: 1.0, steps: 0.1, title: "Edge-Theta", "key": "thetaTheshold"}},
        {image: this.files.blur, caption: "Vertical Blur", filter: {min: 0, max: 200, steps: 1, title: "V-Blur", "key": "blurRounds"}},
        {image: this.files.pass, caption: "High Pass", filter: {min: 0, max: 255, steps: 1, title: "High pass filter", "key": "whiteThreshold"}},
        {image: this.files.tip, caption: "Top Location", filter: {min: 0, max: 50, steps: 1, title: "Tolerance Window", "key": "scanlineInterpolationWidth"}},
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
      });
    },
  },
  methods: {
    show(url) {
      this.view = url;
      this.showModal = true;
    },
    process(file) {
      this.working = true;
      this.openFile = file;
      this.api.put("/images", {
        file: file.name,
        thetaTheshold: Number(this.filterValues.thetaTheshold),
        whiteThreshold: Number(this.filterValues.whiteThreshold),
        blurRounds: Number(this.filterValues.blurRounds),
        scanlineInterpolationWidth: Number(this.filterValues.scanlineInterpolationWidth),
      }).then(response => {
        this.loadFiles(response.data.files);
        this.working = false;
      }).catch(error => {
        alert(error.response.data.errors.join("\n"));
        this.working = false;
      });
    },
    destroy(file) {
      this.working = true;
      this.api.delete("/images/" + file.name)
        .then(() => {
          const image = this.images.filter(image => image.name === file.name)[0];
          this.images.splice(this.images.indexOf(image), 1);
          if (this.files.original.src.indexOf(file.name) !== -1) {
            this.reset();
          }
          this.working = false;
        });
    },
    loadFiles(files) {
      files.forEach(file => {
        this.files[file.element] = {src: API_URL + file.path, bump: new Date().getTime() };
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
          });
          this.working = false;
        }).catch(err => console.log(err));
    },
    reset() {
      Object.keys(this.files)
        .forEach(file => {
          this.files[file].src = API_URL + "/static/leaf.png";
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