<template>
  <div class="h-screen mt-16 bg-white w-full items-center flex flex-col">
    <section class="text-gray-700 body-font w-full sm:w-3/4">
      <div class="w-full py-12 flex flex-col items-center mx-auto">
        <div class="flex flex-col text-center w-full mb-12">
          <div class="w-full h-32 md:64 lg:h-84 overflow-hidden relative">
            <img
              class="min-w-full"
              style="top: 50%; transform: translateY(-15%)"
              src="https://images.unsplash.com/photo-1558402529-d2638a7023e9?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1050&q=80"
              alt=""
            />
            <div
              class="absolute top-0 flex flex-col justify-center h-full w-full z-30"
            >
              <p class="text-3xl md:text-5xl font-bold text-white">Startups</p>
            </div>
            <div class="absolute top-0 w-full h-full bg-black opacity-50"></div>
          </div>

          <p
            class="lg:w-3/5 mx-auto mt-6 leading-relaxed text-gray-800 text-base"
          >
            {{ startUps.heading }}
          </p>
          <p
            class="lg:w-2/3 mx-auto leading-relaxed text-gray-800 mt-4 text-sm"
          >
            {{ startUps.contactHeading }}
            <span
              class="text-blue-500 ml-1s cursor-pointer hover:underline"
              v-scroll-to="'#contact'"
              >Click here</span
            >
          </p>

          <div class="w-32 mx-auto mt-6 h-1 bg-yellow-500"></div>

          <div class="ml-auto" v-if="isAdmin">
            <button @click="startupCreator = true" class="text-sm text-black">
              Add Startup
            </button>
          </div>
        </div>
        <div class="flex flex-wrap w-full -m-4 mb-12">
          <div
            class="lg:w-1/4 md:w-1/2 p-4 w-full"
            v-for="(company, ind) in startUps.arr"
            :key="ind"
          >
            <a class="block relative h-32 rounded overflow-hidden">
              <div
                class="flex justify-center items-center w-full h-full relative"
              >
                <img
                  v-if="company.site"
                  alt="ecommerce"
                  class="object-cover mx-auto object-center block"
                  style="height: 75px; width: 75px"
                  :src="'//logo.clearbit.com/' + company.site"
                />
                <img
                  v-else
                  alt="ecommerce"
                  class="object-cover mx-auto object-center block h-auto"
                  style="height: 75px"
                  :src="company.url"
                />
                <div
                  class="hover:opacity-0 cursor-pointer absolute top-0 left-0 bg-white h-full w-full opacity-25"
                ></div>
                <button
                  v-if="isAdmin"
                  @click="editStartup(ind)"
                  class="absolute flex items-center top-0 right-0 px-3 py-2 m-2 rounded text-blue-500 hover:text-blue-700"
                >
                  Remove
                </button>
              </div>
            </a>
            <div class="mt-4"></div>
          </div>
        </div>
      </div>
    </section>

    <section
      :class="startupEditor || startupCreator ? 'block' : 'hidden'"
      ref="editor"
      class="text-gray-700 body-font w-full"
    >
      <div
        class="container mx-auto w-full flex justify-center px-5 py-16 border-b border-t border-gray-200 md:flex-row flex-col items-center relative"
      >
        <div class="md:w-1/4 w-5/6 mb-10 md:mb-0 z-10">
          <div
            v-if="startupEditorInfo.url == ''"
            class="object-cover object-center bg-gray-200 hover:bg-gray-300 cursor-pointer rounded mx-auto h-64 flex items-center justify-center"
          >
            <input
              type="file"
              class="w-3/4"
              accept="image/*"
              @change="uploadImage($event)"
              id="file-input"
            />
          </div>
          <div v-else class="flex flex-col items-center">
            <img
              class="object-cover object-center rounded mx-auto"
              alt="hero"
              :src="startupEditorInfo.url"
            />
            <input
              type="file"
              class="w-3/4"
              accept="image/*"
              @change="uploadImage($event)"
              id="file-input"
            />
          </div>
        </div>
        <button
          v-if="isAdmin && startupEditor"
          @click="removeStartup"
          class="absolute flex items-center bottom-0 left-0 px-3 py-2 m-2 rounded text-white bg-red-500"
        >
          Remove
        </button>
        <button
          v-if="isAdmin && startupCreator"
          @click="submitAddStartup"
          class="absolute flex items-center bottom-0 right-0 px-3 py-2 m-2 rounded text-white bg-blue-500"
        >
          Add
        </button>
      </div>
    </section>

    <div
      class="relative flex content-center items-center justify-center w-full"
      style="min-height: 75vh"
    >
      <div
        class="absolute block top-0 w-full h-full bg-center bg-cover bg-image-1"
      >
        <span
          id="blackOverlay"
          class="left-0 w-full h-full absolute opacity-50 bg-black"
        >
        </span>
      </div>

      <div
        class="absolute sm:hidden block top-0 w-full h-full bg-center bg-cover bg-image-1"
      >
        <span
          id="blackOverlay"
          class="left-0 w-full h-full absolute opacity-50 bg-black"
        >
        </span>
      </div>

      <Contact :from="'Startup'" :title="'Contact Us'" :description="'Looking to add your startup to this page?'" />
    </div>

    <footer class="text-gray-700 body-font w-full">
      <div class="container px-5 mx-auto"></div>
      <div class="bg-teal-500">
        <div
          class="container px-5 py-6 mx-auto flex items-center sm:flex-row flex-col"
        >
          <img src="../assets/group11.png" class="w-48 h-auto" alt="" />

          <!-- <button @click="flipAdmin" v-if="isAdmin" class="text-sm text-white sm:ml-6 text-green sm:mt-0 mt-4">Admin Mode: ON
                </button>
                <button @click="flipAdmin" v-if="!isAdmin" class="text-sm text-white sm:ml-6 sm:mt-0 mt-4">Admin Mode: OFF
                </button> -->
          <span
            class="inline-flex sm:ml-auto sm:mt-0 mt-4 justify-center sm:justify-start"
          >
            <a class="text-gray-200">
              <svg
                fill="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                class="w-5 h-5"
                viewBox="0 0 24 24"
              >
                <path
                  d="M18 2h-3a5 5 0 00-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 011-1h3z"
                ></path>
              </svg>
            </a>
            <a class="ml-3 text-gray-200">
              <svg
                fill="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                class="w-5 h-5"
                viewBox="0 0 24 24"
              >
                <path
                  d="M23 3a10.9 10.9 0 01-3.14 1.53 4.48 4.48 0 00-7.86 3v1A10.66 10.66 0 013 4s-4 9 5 13a11.64 11.64 0 01-7 2c9 5 20 0 20-11.5a4.5 4.5 0 00-.08-.83A7.72 7.72 0 0023 3z"
                ></path>
              </svg>
            </a>
            <a class="ml-3 text-gray-200">
              <svg
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                class="w-5 h-5"
                viewBox="0 0 24 24"
              >
                <rect width="20" height="20" x="2" y="2" rx="5" ry="5"></rect>
                <path
                  d="M16 11.37A4 4 0 1112.63 8 4 4 0 0116 11.37zm1.5-4.87h.01"
                ></path>
              </svg>
            </a>
            <a class="ml-3 text-gray-200">
              <svg
                fill="currentColor"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="0"
                class="w-5 h-5"
                viewBox="0 0 24 24"
              >
                <path
                  stroke="none"
                  d="M16 8a6 6 0 016 6v7h-4v-7a2 2 0 00-2-2 2 2 0 00-2 2v7h-4v-7a6 6 0 016-6zM2 9h4v12H2z"
                ></path>
                <circle cx="4" cy="4" r="2" stroke="none"></circle>
              </svg>
            </a>
          </span>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import Contact from '../components/Contact'

export default {
  components: {
    Contact
  },
  data() {
    return {
      companies: [
        {
          name: "Hiyo",
          url:
            "https://www.anderson.ucla.edu/images/2017/sites/centers/price/knapp/2020/hiyo.jpg",
        },
        {
          name: "Wire",
          url:
            "https://www.anderson.ucla.edu/images/2017/sites/centers/price/knapp/2020/wire.jpg",
        },
        {
          name: "Vhomes",
          url:
            "https://www.anderson.ucla.edu/images/2017/sites/centers/price/knapp/2020/vhomes.jpg",
        },
        {
          name: "RealAppeal",
          url:
            "https://www.anderson.ucla.edu/images/2017/sites/centers/price/knapp/2020/real.jpg",
        },
        {
          name: "Gen Beauty",
          url:
            "https://www.anderson.ucla.edu/images/2017/sites/centers/price/knapp/2020/gen.jpg",
        },
        {
          name: "Sahara",
          url: "https://i.imgur.com/QeYfrxi.png",
        },
        {
          name: "Gucci",
          site: "Gucci.com",
        },
      ],
      startupEditor: false,
      startupEditorInfo: {
        url: "",
      },
      startupEditorInd: -1,
      startupCreator: false,
    };
  },
  methods: {
    uploadImage(event) {
      this.img = event.target.files[0];
    },
    editStartup(ind) {
      this.startupEditorInd = ind;
      this.startupEditorInfo = this.startUps.arr[ind]
      this.startupEditor = true;
    },
    clearStartupEditor() {
      this.startupEditorInfo = {
        url: "",
      };
    },
    submitAddStartup() {
      if (this.startupEditorInfo.img == "") {
        return;
      }
      this.$store
        .dispatch("addStartup", {
          img: this.img,
          startupInfo: this.startupEditorInfo
        })
        .then(() => {
          this.clearStartupEditor();
          this.img = "";
        });
    },
    removeStartup() {
      this.$store.commit("removeStartup", {
        ind: this.startupEditorInd,
      });
      this.clearStartupEditor();
      this.startupEditor = false;
    },
  },
  computed: {
    startUps() {
      return this.$store.getters.getStartUps;
    },
    isAdmin() {
      return this.$store.getters.getIsAdmin;
    },
  },
  watch: {
    startupEditor() {
      if (this.startupCreator) this.startupCreator = false;
      if (this.startupEditor) {
        this.startupEditorInfo = {
          url: this.startUps.arr[this.startupEditorInd].url,
        };
      }
    },
    startupCreator() {
      if (this.startupEditor) this.startupEditor = false;
      if (this.startupCreator) {
        this.clearStartupEditor();
      }
    },
  },
};
</script>

<style>
.bg-image-1 {
  background-image: url("../assets/business.jpg");
  left: 0px;
  background-repeat: no-repeat;
  background-attachment: fixed;
}
</style>