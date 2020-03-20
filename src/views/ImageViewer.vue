<template>
  <div>
    <b-row>
      <b-col>
        <b-form-group>
          <b-form-file
              id="file-large"
              :state="Boolean(file)"
              v-model="file"
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
            <b-row>
              <b-col>
                <b-card-group class="mb-3 shadow-sm" deck>
                  <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                    <b-card-img :src="files['hist-red']" @click="show(files['hist-red'])" height="150"/>
                    <template v-slot:footer>
                      Histogram Red
                    </template>
                  </b-card>
                </b-card-group>
              </b-col>
              <b-col>
                <b-card-group class="mb-3 shadow-sm" deck>
                  <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                    <b-card-img :src="files['hist-green']" @click="show(files['hist-green'])" height="150"/>
                    <template v-slot:footer>
                      Histogram Green
                    </template>
                  </b-card>
                </b-card-group>
              </b-col>
              <b-col>
                <b-card-group class="mb-3 shadow-sm" deck>
                  <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                    <b-card-img :src="files['hist-blue']" @click="show(files['hist-blue'])" height="150"/>
                    <template v-slot:footer>
                      Histogram Blue
                    </template>
                  </b-card>
                </b-card-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                <b-card-group class="mb-3 shadow-sm" deck>
                  <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                    <b-card-img :src="files.edge" @click="show(files.edge)"/>
                    <template v-slot:footer>
                      <input type="range" min="0" max="100"/>
                    </template>
                  </b-card>
                </b-card-group>
              </b-col>
              <b-col>
                <b-card-group class="mb-3 shadow-sm" deck>
                  <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                    <b-card-img :src="files.blur" @click="show(files.blur)"/>
                    <template v-slot:footer>
                      <input type="range" min="0" max="100"/>
                    </template>
                  </b-card>
                </b-card-group>
              </b-col>
              <b-col>
                <b-card-group class="mb-3 shadow-sm" deck>
                  <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                    <b-card-img :src="files.pass" @click="show(files.pass)"/>
                    <template v-slot:footer>
                      <input type="range" min="0" max="100"/>
                    </template>
                  </b-card>
                </b-card-group>
              </b-col>
              <b-col>
                <b-card-group class="mb-3 shadow-sm" deck>
                  <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                    <b-card-img :src="files.tip" @click="show(files.tip)"/>
                    <template v-slot:footer>
                      <input type="range" min="0" max="100"/>
                    </template>
                  </b-card>
                </b-card-group>
              </b-col>
            </b-row>
          </b-col>
        </b-row>
        <b-row>
          <b-col>
            <b-card-group class="mb-3 shadow-sm" deck>
              <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                <b-card-img class="bg-white" :src="files.red" @click="show(files.red)"/>
                <template v-slot:footer>
                  <input type="range" min="0" max="100"/>
                </template>
              </b-card>
            </b-card-group>
          </b-col>
          <b-col>
            <b-card-group class="mb-3 shadow-sm" deck>
              <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                <b-card-img class="bg-white" :src="files.earth" @click="show(files.earth)"/>
                <template v-slot:footer>
                  <input type="range" min="0" max="100"/>
                </template>
              </b-card>
            </b-card-group>
          </b-col>
          <b-col>
            <b-card-group class="mb-3 shadow-sm" deck>
              <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                <b-card-img class="bg-white" :src="files.green" @click="show(files.green)"/>
                <template v-slot:footer>
                  <input type="range" min="0" max="100"/>
                </template>
              </b-card>
            </b-card-group>
          </b-col>
          <b-col>
            <b-card-group class="mb-3 shadow-sm" deck>
              <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
                <b-card-img class="bg-white" :src="files.leaf" @click="show(files.leaf)"/>
                <template v-slot:footer>
                  <input type="range" min="0" max="100"/>
                </template>
              </b-card>
            </b-card-group>
          </b-col>
        </b-row>
      </b-col>
      <b-col cols="4">
        <b-card-group class="mb-3 shadow-sm" deck>
          <b-card footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-center">
            <b-card-img :src="files.original" @click="show(files.original)"/>
            <template v-slot:footer>
              Original
            </template>
          </b-card>
        </b-card-group>
      </b-col>
      <b-col>
        <div class="p-1" style="max-height: 500px; overflow-x: hidden; overflow-y: scroll">
          <b-card-group class="mb-3 shadow-sm" deck v-for="image in images" :key="image.name">
            <b-card class="shadow-sm" footer-tag="footer" body-bg-variant="dark" footer-bg-variant="primary" border-variant="primary" body-class="p-0" footer-class="p-1 text-right">
              <b-card-img :src="image.path" @click="process(image)"/>
              <template v-slot:footer>
                <b-button size="sm" variant="danger" @click="destroy(image)">Delete</b-button>
                <b-button hidden size="sm" variant="dark" @click="process(image)" class="float-right">Process</b-button>
              </template>
            </b-card>
          </b-card-group>
        </div>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import axios from "axios";

const API_URL = process.env.VUE_APP_API_URL;

export default {
  name: "ImageViewer",
  data() {
    return {
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
  watch: {
    file(val) {
      const formData = new FormData();
      formData.append("file", val);
      this.api.post("/images", formData, {
        headers: {
          "Content-Type": "multipart/form-data",
        },
      }).then(res => {
        this.loadFiles(res.data);
      });
    },
  },
  methods: {
    show() {

    },
    process(image) {
      this.api.put("/images/" + image.name)
        .then(response => this.loadFiles(response.data));
    },
    destroy(image) {
      if (window.confirm("Delete?")) {
        this.api.delete("/images/" + image.name)
          .then(() => this.images.splice(this.images.indexOf(image), 1));
      }
    },
    loadFiles(files) {
      files.forEach(file => {
        this.files[file.element] = API_URL + file.path;
      });
    },
  },
  created() {
    Object.keys(this.files)
      .forEach(file => {
        this.files[file] = API_URL + "/static/leaf.png";
      });
    this.api = axios.create({baseURL: API_URL + "/api"});
    this.api.get("/images")
      .then(response => {
        window.console.log(response);
        response.data.forEach(file => {
          file.path = API_URL + file.path;
          this.images.push(file);
        });
        window.console.log(this.images);
      });
  },
};
</script>

<style scoped>

</style>