<template>
  <div class="h-screen mt-16 bg-white w-full items-center flex flex-col">
    <section class="text-gray-700 body-font w-5/6 bg-white">
      <div class="px-5 py-12 flex flex-col items-center mx-auto">
        <div class="flex flex-col text-center w-full mb-12">
          <p class="text-2xl md:text-5xl font-light text-blue-600">
            Leadership
          </p>
          <p class="lg:w-3/5 mx-auto leading-relaxed text-base">
            {{ leadership.heading }}
          </p>
          <div class="w-32 mx-auto mt-6 h-1 bg-yellow-500"></div>
          <div class="w-full" v-if="isAdmin">
            <button v-if="hasChanged" @click="submitLeadershipOrder" class="mr-64 text-sm text-black">
              Save Leadership Order
            </button>
            <button @click="addLeadership" class="text-sm text-black">
              Add Leadership
            </button>
          </div>
        </div>

        <div class="flex flex-wrap w-full -m-4">
          <div
            class="lg:w-1/4 md:w-1/2 p-4 w-full mb-8"
            v-for="(leader, ind) in leadership.arr"
            :key="ind"
          >
            <a class="block relative overflow-hidden">
              <div class="flex justify-center items-center w-full h-full">
                <img
                  :alt="leader.name"
                  class="object-cover mx-auto object-center block h-auto"
                  :class="clicky ? ['rounded-full', 'w-4/5'] : ['w-full']"
                  :src="leader.img"
                />
              </div>
              <div
                v-if="isAdmin"
                class="absolute z-20 bg-white flex items-center top-0 left-0 px-3 py-2 m-2 rounded"
              >
                <button class=" text-blue-500 hover:text-blue-700" @click="swapLeadership(ind, ind-1)">Left</button>
                <div class="flex flex-col">
                    <button class=" text-blue-500 hover:text-blue-700" @click="ind>=4 ? swapLeadership(ind, ind-4) : ''">Up</button>
                    <button class=" text-blue-500 hover:text-blue-700" @click="ind<(leadership.arr.length-5) ? swapLeadership(ind, ind+4) : ''">Down</button>
                </div>
                <button class=" text-blue-500 hover:text-blue-700" @click="swapLeadership(ind, ind+1)">Right</button>
              </div>
              <button
                v-if="isAdmin"
                @click="editLeadership(ind)"
                class="absolute z-20 bg-white flex items-center top-0 right-0 px-3 py-2 m-2 rounded text-blue-500 hover:text-blue-700"
              >
                Edit
              </button>
            </a>
            <div class="mt-4">
              <p
                class="text-center font-bold text-gray-900 leading-relaxed text-base"
              >
                {{ leader.name }}
              </p>

              <p
                class="text-center font-normal text-sm text-gray-600 leading-relaxed uppercase"
              >
                {{ leader.title }}
              </p>

              <div class="flex justify-around mt-2 w-full">
                <a
                  :href="leader.linkedin"
                  target="_blank"
                  v-if="
                    leader.linkedin != '' &&
                    Object.keys(leadership).includes('linkedin')
                  "
                  class="w-6 h-6 relative"
                >
                  <svg
                    version="1.1"
                    class="w-full h-full fill-current text-blue opacity-50 hover:opacity-100"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    x="0px"
                    y="0px"
                    viewBox="0 0 382 382"
                    style="enable-background: new 0 0 382 382"
                    xml:space="preserve"
                  >
                    <path
                      style="fill: #0077b7"
                      d="M347.445,0H34.555C15.471,0,0,15.471,0,34.555v312.889C0,366.529,15.471,382,34.555,382h312.889
                                            C366.529,382,382,366.529,382,347.444V34.555C382,15.471,366.529,0,347.445,0z M118.207,329.844c0,5.554-4.502,10.056-10.056,10.056
                                            H65.345c-5.554,0-10.056-4.502-10.056-10.056V150.403c0-5.554,4.502-10.056,10.056-10.056h42.806
                                            c5.554,0,10.056,4.502,10.056,10.056V329.844z M86.748,123.432c-22.459,0-40.666-18.207-40.666-40.666S64.289,42.1,86.748,42.1
                                            s40.666,18.207,40.666,40.666S109.208,123.432,86.748,123.432z M341.91,330.654c0,5.106-4.14,9.246-9.246,9.246H286.73
                                            c-5.106,0-9.246-4.14-9.246-9.246v-84.168c0-12.556,3.683-55.021-32.813-55.021c-28.309,0-34.051,29.066-35.204,42.11v97.079
                                            c0,5.106-4.139,9.246-9.246,9.246h-44.426c-5.106,0-9.246-4.14-9.246-9.246V149.593c0-5.106,4.14-9.246,9.246-9.246h44.426
                                            c5.106,0,9.246,4.14,9.246,9.246v15.655c10.497-15.753,26.097-27.912,59.312-27.912c73.552,0,73.131,68.716,73.131,106.472
                                            L341.91,330.654L341.91,330.654z"
                    />
                    <g></g>
                  </svg>
                </a>

                <a
                  :href="'mailto:' + leader.email"
                  v-if="
                    leader.email != '' &&
                    Object.keys(leadership).includes('email')
                  "
                  class="w-6 h-6 relative"
                >
                  <div
                    class="absolute w-full h-full bg-white opacity-50 hover:opacity-0 cursor-pointer"
                    style="pointer-events: none"
                  ></div>

                  <svg
                    version="1.1"
                    class="w-full h-full fill-current text-blue"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    x="0px"
                    y="0px"
                    viewBox="0 0 512 512"
                    style="enable-background: new 0 0 512 512"
                    xml:space="preserve"
                  >
                    <path
                      style="fill: #0077b7"
                      d="M496,112.011H272c-8.832,0-16,7.168-16,16s7.168,16,16,16h177.376l-98.304,76.448l-70.496-44.832
                                            l-17.152,27.008l80,50.88c2.592,1.664,5.6,2.496,8.576,2.496c3.456,0,6.944-1.12,9.824-3.36L480,160.715v207.296H272
                                            c-8.832,0-16,7.168-16,16s7.168,16,16,16h224c8.832,0,16-7.168,16-16v-256C512,119.179,504.832,112.011,496,112.011z"
                    />
                    <path
                      style="fill: #0077b7"
                      d="M282.208,19.691c-3.648-3.04-8.544-4.352-13.152-3.392l-256,48C5.472,65.707,0,72.299,0,80.011v352
                                            c0,7.68,5.472,14.304,13.056,15.712l256,48c0.96,0.192,1.952,0.288,2.944,0.288c3.712,0,7.328-1.28,10.208-3.68
                                            c3.68-3.04,5.792-7.584,5.792-12.32v-448C288,27.243,285.888,22.731,282.208,19.691z"
                    />
                    <path
                      style="fill: #fafafa"
                      d="M144,368.011c-44.096,0-80-43.072-80-96s35.904-96,80-96s80,43.072,80,96
                                            S188.096,368.011,144,368.011z M144,208.011c-26.464,0-48,28.704-48,64s21.536,64,48,64s48-28.704,48-64
                                            S170.464,208.011,144,208.011z"
                    />
                    <g></g>
                  </svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <section
      :class="leadershipEditor || leadershipCreator ? 'block' : 'hidden'"
      ref="editor"
      class="text-gray-700 body-font w-full"
    >
      <div
        class="container mx-auto w-full flex px-5 py-16 border-b border-t border-gray-200 md:flex-row flex-col items-center relative"
      >
        <div class="md:w-1/4 w-5/6 mb-10 md:mb-0 z-10">
          <div
            v-if="leadershipEditorInfo.url == ''"
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
              :src="leadershipEditorInfo.img"
            />
          </div>
        </div>
        <div
          class="md:flex-1 lg:pl-24 md:pl-16 flex flex-col md:items-start md:text-left items-center text-center"
        >
          <p class="mb-1 leading-relaxed text-gray-700">Name:</p>
          <input
            type="text"
            class="w-full border border-gray-200 hover:border-gray-300 focus:border-blue-400 px-2 py-1 outline-none rounded"
            v-model="leadershipEditorInfo.name"
          />
          <p class="mb-1 leading-relaxed text-gray-700">Title:</p>
          <textarea
            type="text"
            class="w-full border h-16 border-gray-200 hover:border-gray-300 focus:border-blue-400 px-2 py-1 outline-none rounded"
            v-model="leadershipEditorInfo.title"
          />
          <p class="mb-1 leading-relaxed text-gray-700">Email:</p>
          <input
            type="text"
            class="w-full border border-gray-200 hover:border-gray-300 focus:border-blue-400 px-2 py-1 outline-none rounded"
            v-model="leadershipEditorInfo.email"
          />
          <p class="mt-1 leading-relaxed text-gray-700">LinkedIn:</p>
          <input
            type="text"
            class="w-full border border-gray-200 hover:border-gray-300 focus:border-blue-400 px-2 py-1 outline-none rounded"
            v-model="leadershipEditorInfo.linkedin"
          />
        </div>
        <button
          v-if="isAdmin && leadershipEditor"
          @click="removeLeadership"
          class="absolute flex items-center bottom-0 left-0 px-3 py-2 m-2 rounded text-white bg-red-500"
        >
          Remove
        </button>
        <button
          v-if="isAdmin && leadershipCreator"
          @click="submitAddLeadership"
          class="absolute flex items-center bottom-0 right-0 px-3 py-2 m-2 rounded text-white bg-blue-500"
        >
          Add
        </button>
      </div>
    </section>
    <div class="w-full">
      <footer class="text-gray-700 body-font">
        <div class="container px-5 mx-auto"></div>
        <div class="bg-teal-500">
          <div
            class="container px-5 py-6 mx-auto flex items-center sm:flex-row flex-col"
          >
            <img src="../assets/group11.png" class="w-48 h-auto" alt="" />

            <button
              @click="flipAdmin"
              v-if="isAdmin"
              class="text-sm text-white sm:ml-6 text-green sm:mt-0 mt-4"
            >
              Admin Mode: ON
            </button>
            <button
              @click="flipAdmin"
              v-if="!isAdmin"
              class="text-sm text-white sm:ml-6 sm:mt-0 mt-4"
            >
              Admin Mode: OFF
            </button>
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
  </div>
</template>

<script>
export default {
  methods: {
    addLeadership() {
      this.clearLeadershipEditor();
      this.leadershipCreator = true;
      setTimeout(() => {
        window.scrollTo(0, this.$refs.editor.getBoundingClientRect().top);
      }, 100);
    },
    uploadImage(event) {
      this.img = event.target.files[0];
    },
    clearLeadershipEditor() {
      this.leadershipEditorInfo = {
        url: "",
      };
    },
    editLeadership(ind) {
      this.leadershipEditorInd = ind;
      this.leadershipEditorInfo = this.leadership.arr[ind];
      this.leadershipEditor = true;
    },
    submitAddLeadership() {
      if (this.leadershipEditorInfo.img == "") {
        return;
      }
      this.$store
        .dispatch("addLeadership", {
          img: this.img,
          leadershipInfo: this.leadershipEditorInfo,
        })
        .then(() => {
          this.clearLeadershipEditor();
          this.img = "";
          this.leadershipEditor = false;
        });
    },
    removeLeadership() {
      this.$store
        .dispatch("removeLeadership", {
          ind: this.leadershipEditorInd,
        })
        .then(() => {
          this.clearLeadershipEditor();
          this.leadershipEditor = false;
        });
    },
    clickHandler() {
      if (this.isAdmin) this.clicky = !this.clicky;
    },
    flipAdmin() {
      this.$store.commit("flipAdmin");
    },
    swapLeadership(ind1, ind2) {
        this.hasChanged = true
      this.$store.commit("changeLeadershipOrder", { ind1, ind2 });
    },
    submitLeadershipOrder() {
      this.$store.dispatch("submitLeadershipOrder").then(() => {
          this.hasChanged = false;
      });
    },
  },
  computed: {
    isAdmin() {
      return this.$store.getters.getIsAdmin;
    },
    leadership() {
      return this.$store.getters.getLeadership;
    },
  },
  data() {
    return {
      clicky: false,
      leadershipEditor: false,
      leadershipEditorInfo: {
        url: "",
        email: "",
        linkedin: "",
        name: "",
        title: "",
      },
      leadershipEditorInd: -1,
      leadershipCreator: false,
      hasChanged: false,
    };
  },
  watch: {
    leadershipEditor() {
      if (this.leadershipCreator) this.leadershipCreator = false;
      if (this.leadershipEditor) {
        this.leadershipEditorInfo = {
          url: this.leadership.arr[this.leadershipEditorInd].url,
        };
      }
    },
    leadershipCreator() {
      if (this.leadershipEditor) this.leadershipEditor = false;
      if (this.leadershipCreator) {
        this.clearLeadershipEditor();
      }
    },
  },
};
</script>

<style>
</style>