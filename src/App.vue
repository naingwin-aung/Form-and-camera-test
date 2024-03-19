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
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- end cropper modal  -->

  <form @submit.prevent="submitHandler" class="upload-user-form">
    <div class="container">
      <div class="photo-form-first gap-form-5">
        <!-- start upload photo -->
        <div class="upload-photo cursor" @click="openCropper = true">
          <label v-if="!previewCropPhoto" for="file" class="previewPhoto">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="44"
              height="44"
              viewBox="0 0 44 44"
              fill="none"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M33.9167 2.2915C34.2813 2.2915 34.6311 2.43637 34.8889 2.69423C35.1468 2.95209 35.2917 3.30183 35.2917 3.6665V8.70817H40.3333C40.698 8.70817 41.0477 8.85304 41.3056 9.1109C41.5635 9.36876 41.7083 9.7185 41.7083 10.0832C41.7083 10.4478 41.5635 10.7976 41.3056 11.0554C41.0477 11.3133 40.698 11.4582 40.3333 11.4582H35.2917V16.4998C35.2917 16.8645 35.1468 17.2142 34.8889 17.4721C34.6311 17.73 34.2813 17.8748 33.9167 17.8748C33.552 17.8748 33.2023 17.73 32.9444 17.4721C32.6865 17.2142 32.5417 16.8645 32.5417 16.4998V11.4582H27.5C27.1353 11.4582 26.7856 11.3133 26.5277 11.0554C26.2699 10.7976 26.125 10.4478 26.125 10.0832C26.125 9.7185 26.2699 9.36876 26.5277 9.1109C26.7856 8.85304 27.1353 8.70817 27.5 8.70817H32.5417V3.6665C32.5417 3.30183 32.6865 2.95209 32.9444 2.69423C33.2023 2.43637 33.552 2.2915 33.9167 2.2915Z"
                fill="#80BFFF"
              />
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M22 2.2915H21.8955C17.6623 2.2915 14.3458 2.2915 11.7571 2.63984C9.10979 2.9955 7.01979 3.73984 5.37896 5.37884C3.73813 7.01967 2.99563 9.10967 2.63996 11.7588C2.29163 14.3457 2.29163 17.6622 2.29163 21.8953V22.1043C2.29163 26.3375 2.29163 29.654 2.63996 32.2427C2.99563 34.89 3.73996 36.98 5.37896 38.6208C7.01979 40.2617 9.10979 41.0042 11.759 41.3598C14.3458 41.7082 17.6623 41.7082 21.8955 41.7082H22.1045C26.3376 41.7082 29.6541 41.7082 32.2428 41.3598C34.8901 41.0042 36.9801 40.2598 38.621 38.6208C40.2618 36.98 41.0043 34.89 41.36 32.2408C41.7083 29.654 41.7083 26.3375 41.7083 22.1043V21.9998C41.7083 21.6352 41.5634 21.2854 41.3056 21.0276C41.0477 20.7697 40.698 20.6248 40.3333 20.6248C39.9686 20.6248 39.6189 20.7697 39.361 21.0276C39.1032 21.2854 38.9583 21.6352 38.9583 21.9998C38.9583 26.3595 38.9546 29.4908 38.6356 31.8742L38.5953 32.1528L33.5096 27.575C32.3526 26.534 30.8767 25.9164 29.3231 25.8231C27.7696 25.7299 26.2303 26.1665 24.9571 27.0617L24.4108 27.4467C23.9693 27.7563 23.4327 27.9003 22.8955 27.8533C22.3583 27.8064 21.8549 27.5715 21.4738 27.19L13.6088 19.325C12.6055 18.3221 11.2586 17.7381 9.84082 17.6911C8.42305 17.6441 7.04041 18.1377 5.97296 19.072L5.04346 19.8842C5.05263 16.5988 5.09846 14.1037 5.36429 12.1255C5.67963 9.78067 6.28096 8.36717 7.32413 7.32217C8.36913 6.279 9.78079 5.6795 12.1256 5.36417C14.509 5.04517 17.6403 5.0415 22 5.0415C22.3646 5.0415 22.7144 4.89664 22.9722 4.63878C23.2301 4.38091 23.375 4.03118 23.375 3.6665C23.375 3.30183 23.2301 2.95209 22.9722 2.69423C22.7144 2.43637 22.3646 2.2915 22 2.2915ZM5.36429 31.8742C5.67963 34.219 6.28096 35.6325 7.32413 36.6775C8.36913 37.7207 9.78079 38.3202 12.1256 38.6355C14.509 38.9545 17.6403 38.9582 22 38.9582C26.3596 38.9582 29.491 38.9545 31.8743 38.6355C34.2191 38.3202 35.6326 37.7188 36.6776 36.6757C37.1363 36.2156 37.5111 35.6789 37.785 35.0898C37.712 35.0456 37.6432 34.9946 37.5796 34.9377L31.6708 29.621C30.9769 28.9954 30.0912 28.6239 29.1586 28.5673C28.2261 28.5107 27.3019 28.7723 26.5375 29.3093L25.993 29.6943C25.022 30.3769 23.8414 30.6951 22.6589 30.5928C21.4764 30.4905 20.3679 29.9744 19.5286 29.1352L11.6636 21.2702C11.1537 20.7604 10.469 20.4635 9.74838 20.4396C9.02772 20.4158 8.32494 20.6668 7.78246 21.1418L5.04163 23.5398C5.04713 27.1112 5.08379 29.7823 5.36429 31.8742Z"
                fill="#80BFFF"
              />
            </svg>
            <div class="previewPhoto-text">
              <div>写真をアップする</div>
              <div>Upload Photo</div>
            </div>
          </label>
          <img v-if="previewCropPhoto" :src="previewCropPhoto" alt="" />
        </div>
        <!-- end upload photo  -->

        <!-- start candidate form -->
        <div class="candidate-form">
          <div class="candidate-form-control">
            <label for="name">Name<sup>*</sup></label>
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
            <label for="contact">Contact No<sup>*</sup></label>
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
            <label for="dob">Date of Birth<sup>*</sup></label>
            <input v-model="form.dob" type="text" id="dob" placeholder="Text" />
          </div>
          <div class="candidate-form-control">
            <label for="salary">Expected Salary<sup>*</sup></label>
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
      <h4 class="mb-50">
        <div>資格・スキル</div>
        <div>Qualification/Skills</div>
      </h4>
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
      <h4 class="mb-50">Responsibilities:</h4>
      <section class="mb-10">
        <div class="even-columns justify-conent-space align-center">
          <div class="responsibilities">
            <div>Work Experience</div>
            <div>実務経験</div>
          </div>
          <div class="even-columns gap-small response-select-box align-center">
            <div>
              <select v-model="form.experience" @change="experienceFormHandler">
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
              </select>
            </div>
            <div>追加</div>
          </div>
        </div>

        <div class="even-columns justify-center mt-30">
          <div class="entry-click even-columns align-center me-10">
            Click Here if you are entry Level
            <div class="entry even-columns align-center">
              <input
                type="checkbox"
                id="entry"
                v-model="form.isEntry"
                :checked="form.isEntry"
              />
              <label for="entry"
                >エントリーレベルの方はここをクリックしてください</label
              >
            </div>
          </div>
        </div>
      </section>
      <!-- end responsibilities -->

      <!-- start work experience -->
      <section
        v-for="index in experienceYear"
        :key="index"
        class="exp-work-form"
      >
        <h4 class="mb-50">職歴 ({{ index }}) Work experience ({{ index }})</h4>
        <section>
          <div class="even-grid-row mb-20 align-center">
            <div class="even-grid-row-work">
              <div>役職 Job Title</div>
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
              <div>会社 Company</div>
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
              <div>期間 Period</div>
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
              <div>&nbsp; &nbsp; &nbsp; &nbsp; Description</div>
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
      <button>Submit 提出する</button>
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
  isEntry: false,
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
  
  // Stop video tracks
  if (video.value && video.value.srcObject) {
    const stream = video.value.srcObject;
    const tracks = stream.getTracks();
    tracks.forEach(track => track.stop());
    video.value.srcObject = null;
  }

  // Set video reference to null
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
  resetComponent();
};

const convertBlobToUrl = async () => {
  const blob = await (await fetch(photoData.value)).blob();
  file.value = new File([blob], "NEW_PHOTO.png", { type: blob.type });
  uploadedImage.value = URL.createObjectURL(file.value);
  previewCropPhoto.value = URL.createObjectURL(file.value);
  form.value.image = file.value;
  isOpenCamera.value = false;
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
    form.value.expForm = [
      ...form.value.expForm,
      {
        title: null,
        company: null,
        periodFrom: null,
        periodTo: null,
        description: null,
      },
    ];
  }
};

onBeforeMount(() => {
  addFormWorkData();
});
</script>

<style lang="scss" scoped></style>
