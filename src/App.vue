<template>
  <!-- start cropper modal  -->
  <div v-if="openCropper">
    <div class="candidate-modal">
      <div class="candidate-overlay"></div>
      <div class="candidate-modal-view">
        <div class="candidate-modal-title" @click.self="resetComponent()">
          <div class="candidate-modal-content">
            <div class="mb-50">
              <span @click="resetComponent()" class="cursor"> Close </span>
            </div>

            <div class="btn-camera-column">
              <button
                v-if="!isOpenCamera && !uploadedImage"
                class="btn-open-camera"
                @click="startCamera()"
              >
                Open Camera
              </button>

              <button
                v-if="isOpenCamera"
                class="btn-open-camera"
                @click="takePhoto()"
              >
                Take Photo
              </button>

              <button
                v-if="uploadedImage"
                class="btn-open-camera"
                @click="cropImage()"
              >
                <span v-show="!isCropping">Crop Image</span>
                <span v-show="isCropping">Loading...</span>
              </button>

              <button class="btn-canel-camera" @click="resetComponent()">
                Cancel
              </button>
            </div>

            <div v-if="!uploadedImage && isOpenCamera" class="pb-4">
              <div class="video-wrapper">
                <video
                  autoplay
                  v-show="!isNewPhoto"
                  ref="video"
                  class="camera-video"
                  playsinline=""
                />
              </div>
              <canvas v-show="isNewPhoto" ref="canvas" class="aspect-square" />
            </div>

            <div class="cropper-wrapper">
              <Cropper
                class="cropper-image"
                ref="cropper"
                :stencil-props="{
                  movable: true,
                  resizable: true,
                  aspectRatio: 1,
                }"
                :resize-image="{
                  adjustStencil: false,
                }"
                image-restriction="stencil"
                :src="uploadedImage"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- end cropper modal  -->

  <form @submit.prevent="submitHandler" class="upload-user-form">
    <div class="container">
      <div class="even-columns gap-form-5">
        <!-- start upload photo -->
        <div class="upload-photo cursor" @click="openCropper = true">
          <label v-if="!previewCropPhoto" for="file"
            ><small>Upload Photo</small></label
          >
          <img v-if="previewCropPhoto" :src="previewCropPhoto" alt="" />
        </div>
        <!-- end upload photo  -->

        <!-- start candidate form -->
        <div class="candidate-form">
          <div class="candidate-form-control">
            <label for="name">Name</label>
            <input
              v-model="form.name"
              type="text"
              id="name"
              placeholder="Text"
            />
          </div>
          <div class="candidate-form-control">
            <label for="address">Address</label>
            <input
              v-model="form.address"
              type="text"
              id="adderss"
              placeholder="Text"
            />
          </div>
          <div class="candidate-form-control">
            <label for="contact">Contact No</label>
            <input
              v-model="form.contact"
              type="text"
              id="contact"
              placeholder="Text"
            />
          </div>
          <div class="candidate-form-control">
            <label for="email">Email Address</label>
            <input
              v-model="form.email"
              type="text"
              id="email"
              placeholder="Text"
            />
          </div>
          <div class="candidate-form-control">
            <label for="dob">Date of Birth</label>
            <input v-model="form.dob" type="text" id="dob" placeholder="Text" />
          </div>
          <div class="candidate-form-control">
            <label for="salary">Expected Salary</label>
            <input
              v-model="form.salary"
              type="text"
              id="salary"
              placeholder="Text"
            />
          </div>
          <div class="candidate-form-control">
            <label for="education">Education Degree</label>
            <input
              v-model="form.education"
              type="text"
              id="education"
              placeholder="Text"
            />
          </div>
        </div>
        <!-- end candidate form -->
      </div>

      <!-- start skills  -->
      <h4 class="mb-50">Qualification/Skills</h4>
      <section class="mb-10">
        <div class="even-grid">
          <div class="skills-list">
            <div class="skills">
              <input
                v-model="form.skills"
                type="checkbox"
                id="model"
                value="Revit Modeling"
              />
              <label for="model">Revit Modeling</label>
            </div>

            <div class="skills">
              <input
                type="checkbox"
                id="family"
                v-model="form.skills"
                value="Revit Create Family"
              />
              <label for="family">Revit Create Family</label>
            </div>

            <div class="skills">
              <input
                type="checkbox"
                id="auto-cad"
                v-model="form.skills"
                value="Auto CAD"
              />
              <label for="auto-cad">Auto CAD</label>
            </div>

            <div class="skills">
              <input
                type="checkbox"
                id="t-fas"
                v-model="form.skills"
                value="T-Fas"
              />
              <label for="t-fas">T-Fas</label>
            </div>
          </div>

          <div class="skills-list">
            <div class="skills">
              <input
                type="checkbox"
                id="load"
                v-model="form.skills"
                value="Cooling Load Calculation"
              />
              <label for="load">Cooling Load Calculation</label>
            </div>

            <div class="skills">
              <input
                type="checkbox"
                id="duct_size"
                v-model="form.skills"
                value="Duct Size, Pipe Size Selection"
              />
              <label for="duct_size">Duct Size, Pipe Size Selection</label>
            </div>

            <div class="skills">
              <input
                type="checkbox"
                id="total_electrical"
                v-model="form.skills"
                value="Total Electrical Design"
              />
              <label for="total_electrical">Total Electrical Design</label>
            </div>

            <div class="skills">
              <input
                type="checkbox"
                id="bus_duct"
                v-model="form.skills"
                value="Bus Duct Selection"
              />
              <label for="bus_duct">Bus Duct Selection</label>
            </div>
          </div>
        </div>
      </section>
      <!-- end skills  -->

      <!--start language -->
      <h4 class="mb-50">Language</h4>
      <section class="mb-10">
        <div class="skills-list">
          <div class="skills">
            <input
              type="checkbox"
              id="english"
              v-model="form.language"
              value="English"
            />
            <label for="english">English</label>
          </div>

          <div class="skills">
            <input
              type="checkbox"
              id="japanesen2"
              v-model="form.language"
              value="Japanese N2"
            />
            <label for="japanesen2">Japanese N2</label>
          </div>

          <div class="skills">
            <input
              type="checkbox"
              id="japanesen3"
              v-model="form.language"
              value="Japanese N3"
            />
            <label for="japanesen3">Japanese N3</label>
          </div>
        </div>
      </section>
      <!-- end language -->

      <!-- start responsibilities  -->
      <h4 class="mb-50">Responsibilities</h4>
      <section class="mb-10">
        <div class="even-columns justify-conent-space align-center">
          <div class="responsibilities">
            <div>Work Experience</div>
            <div>Japanese Sentence</div>
          </div>
          <div class="even-columns gap-small response-select-box">
            <div>
              <select v-model="form.experience" @change="experienceFormHandler">
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
              </select>
            </div>
            <div>Japanese Sentence</div>
          </div>
        </div>

        <div class="text-align-center mt-30">
          <div>Click Here if you are entry Level</div>
        </div>
      </section>
      <!-- end responsibilities -->

      <!-- start work experience -->
      <section v-for="index in experienceYear" :key="index">
        <h4 class="mb-50">({{ index }}) Work experience ({{ index }})</h4>
        <section>
          <div class="even-grid-row mb-20 align-center">
            <div class="even-grid-row-work">
              <div>Job Title</div>
              <div>:</div>
            </div>

            <div class="work-experience">
              <div>
                <input
                  type="text"
                  value="form.expForm[index - 1].title"
                  :id="index - 1"
                  v-model="form.expForm[index - 1].title"
                  placeholder="Text"
                />
              </div>
            </div>
          </div>

          <div class="even-grid-row mb-30 align-center">
            <div class="even-grid-row-work">
              <div>Company</div>
              <div>:</div>
            </div>

            <div class="work-experience">
              <div>
                <input
                  type="text"
                  id="company"
                  v-model="form.expForm[index - 1].company"
                  placeholder="Text"
                />
              </div>
            </div>
          </div>

          <div class="even-grid-row mb-20">
            <div class="even-grid-row-work">
              <div>Period</div>
              <div>:</div>
            </div>

            <div class="work-experience flex-experience">
              <div class="experience-date">
                <label for="from">From</label>
                <input
                  type="date"
                  id="from"
                  placeholder="Text"
                  v-model="form.expForm[index - 1].periodFrom"
                />
              </div>
              <div class="experience-date">
                <label for="to">To</label>
                <input
                  type="date"
                  id="to"
                  placeholder="Text"
                  v-model="form.expForm[index - 1].periodTo"
                />
              </div>
            </div>
          </div>

          <div class="even-grid-row mb-20">
            <div class="even-grid-row-work">
              <div>Description</div>
              <div>:</div>
            </div>

            <div class="work-experience">
              <div>
                <textarea
                  type="text"
                  id="description"
                  placeholder="Text"
                  v-model="form.expForm[index - 1].description"
                />
              </div>
            </div>
          </div>
        </section>
      </section>
      <!-- end work experience -->
    </div>
    <div class="block-submit-btn">
      <button>Submit</button>
    </div>
  </form>
</template>

<script setup>
import { onBeforeMount, ref } from "vue";
import { Cropper } from "vue-advanced-cropper";
import "vue-advanced-cropper/dist/style.css";

const openCropper = ref(false);
const uploadedImage = ref(null);
const isOpenCamera = ref(false);
const isNewPhoto = ref(false);
const video = ref(null);
const canvas = ref(null);
const photoData = ref(null);
const file = ref(null);
const cropper = ref(null);
const isCropping = ref(false);
// const uploadedPhoto = ref(null);
const previewCropPhoto = ref(null);
const experienceYear = ref(1);

const form = ref({
  name: null,
  address: null,
  contact: null,
  email: null,
  dob: null,
  salary: null,
  education: null,
  image: null,
  height: null,
  width: null,
  left: null,
  top: null,
  skills: [],
  language: [],
  experience: 1,
  expForm: [],
});

const startCamera = async () => {
  isOpenCamera.value = true;

  if (navigator.mediaDevices) {
    let stream = await navigator.mediaDevices.getUserMedia({
      video: {
        width: { max: 1024 },
        height: { max: 1024 },
        aspectRatio: { ideal: 1 },
        facingMode: "user",
      },
    });
    video.value.srcObject = stream;
    video.value.play();

    video.value.style.transform = "scaleX(-1)";
  }
};

const resetComponent = () => {
  openCropper.value = false;
  isOpenCamera.value = false;
  isNewPhoto.value = false;
  video.value = null;
  canvas.value = null;
  uploadedImage.value = null;
};

const takePhoto = () => {
  let videoLocal = video.value;
  let canvasLocal = canvas.value;

  canvasLocal.width = videoLocal.getBoundingClientRect().width;
  canvasLocal.height = videoLocal.getBoundingClientRect().height;

  let context = canvasLocal.getContext("2d");
  context.imageSmoothingEnabled = true;
  context.imageSmoothingQuality = "high";
  context.scale(-1, 1);
  context.drawImage(
    videoLocal,
    -canvasLocal.width,
    0,
    canvasLocal.width,
    canvasLocal.height
  );
  context.setTransform(1, 0, 0, 1, 0, 0);

  isNewPhoto.value = true;
  photoData.value = canvasLocal.toDataURL();

  convertBlobToUrl();
};

const convertBlobToUrl = async () => {
  const blob = await (await fetch(photoData.value)).blob();
  file.value = new File([blob], "NEW_PHOTO.png", { type: blob.type });
  uploadedImage.value = URL.createObjectURL(file.value);
  isOpenCamera.value = false;
};

const cropImage = async () => {
  isCropping.value = true;
  const { coordinates } = cropper.value.getResult();

  // let data = new FormData();
  // data.append("image", file.value || "");
  // data.append("height", coordinates.height || "");
  // data.append("width", coordinates.width || "");
  // data.append("left", coordinates.left || "");
  // data.append("top", coordinates.top || "");

  // uploadedPhoto.value = data;
  isCropping.value = false;
  previewCropPhoto.value = URL.createObjectURL(file.value);

  form.value.image = file.value;
  form.value.height = coordinates.height;
  form.value.width = coordinates.width;
  form.value.left = coordinates.left;
  form.value.top = coordinates.top;

  resetComponent();
  // console.log(file.value);
  // console.log(form.value.image, form.value.height, form.value.width);
};

const submitHandler = () => {
  console.log(form.value);
};

const experienceFormHandler = (e) => {
  experienceYear.value = e.target.value * 1;
  addFormWorkData();
};

const addFormWorkData = () => {
  form.value.expForm = [];
  for (let i = 1; i <= experienceYear.value; i++) {
    form.value.expForm.push({
      title: null,
      company: null,
      periodFrom: null,
      periodTo: null,
      description: null,
    });
  }
};

onBeforeMount(() => {
  addFormWorkData();
});
</script>

<style lang="scss" scoped></style>
