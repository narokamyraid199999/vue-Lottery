<script setup>
import { ref } from "vue";
import confetti from "canvas-confetti";
import Swal from "sweetalert2";

const items = ref([]);

let userInput = ref("");
const activeItemId = ref(null);
const selectedItemId = ref(null);
const file = ref(null);
const content = ref(null);
const doc = ref(null);

const addItem = () => {
  if (userInput.value.length > 3) {
    items.value.push({
      id: Math.floor(Math.random() * 1000000),
      data: userInput.value,
    });
    userInput.value = "";
  }
};

const clearItems = () => {
  selectedItemId.value = null;
  items.value = [];
};

const delItem = (itemId) => {
  items.value = items.value.filter((item) => item.id != itemId);
};

const shuffle = () => {
  if (items.value.length >= 2) {
    selectedItemId.value = null;

    let counter = 100;
    items.value.forEach((item) => {
      setTimeout(() => {
        selectedItemId.value =
          items.value[Math.floor(Math.random() * items.value.length)].id;
      }, counter);
      counter += 100;
    });

    counter += 100;

    setTimeout(() => {
      selectedItemId.value = items.value[items.value.length - 1].id;
    }, counter);

    counter += 100;

    setTimeout(() => {
      confetti.reset();

      // confetti({
      //   particleCount: 300,
      //   startVelocity: 30,
      //   spread: 360,
      //   origin: {
      //     x: Math.random(),
      //     // since they fall down, start a bit higher than random
      //     y: Math.random() - 0.2,
      //   },
      // });
      // confetti({
      //   particleCount: 300,
      //   startVelocity: 30,
      //   spread: 360,
      //   origin: {
      //     x: Math.random(),
      //     // since they fall down, start a bit higher than random
      //     y: Math.random() - 0.2,
      //   },
      // });
      confetti({
        particleCount: 2000,
        spread: 360,
      });
    }, counter);

    counter += 100;

    setTimeout(() => {
      Swal.fire({
        showConfirmButton: false,
        showDenyButton: true,
        denyButtonText: "Close",
        title: "Congratulation",
        text: items.value.filter((item) => item.id == selectedItemId.value)[0]
          .data,
        imageUrl: "https://lotterystring.com/assets/img/win_badge.png",
        imageWidth: 150,
        imageHeight: 150,
        imageAlt: "Custom image",
      });
    }, counter);
  }
};

const readFile = () => {
  const file = doc.value.files[0];
  if (file && file.name.includes(".txt")) {
    const reader = new FileReader();
    reader.onload = (res) => {
      content.value = res.target.result.split("\n").map((line) => {
        line = line.replace("\r", "");
        return line != null && line != "" ? line : null;
      });

      content.value.forEach((item) => {
        item != null
          ? items.value.push({
              id: Math.floor(Math.random() * 1000000),
              data: item,
            })
          : undefined;
      });
    };
    reader.onerror = (err) => {
      message.value = "Error reading the file";
      console.log(err);
    };
    reader.readAsText(file);
  } else {
    message.value = "Please upload a valid .txt file.";
    content.value = [];
  }
};
</script>

<template>
  <div
    class="app animated-background bg-[#E7F0D3] min-h-screen max-h-screen overflow-auto"
  >
    <div class="container mx-auto px-[10%]">
      <div class="flex flex-col items-center justify-center pt-16">
        <!-- <span class="material-icons">keyboard_double_arrow_right</span> -->
        <img
          class="w-40"
          src="https://cdn-icons-png.freepik.com/512/2038/2038515.png"
          alt=""
        />
        <p class="text-4xl font-bold my-4 text-[#262D5B] capitalize">
          winner winner chicken on dinner
        </p>
        <!-- <p class="text-center text-xl my-4 leading-tight text-[#3E444D]">
          An online lottery tool that let you add items or text by entering them
          in the input box, once<br />
          you're added at least two items, simply press the shuffle button, and
          the app will randomly<br />
          select a winner
        </p> -->
        <div class="mt-2 flex items-center gap-x-3">
          <!-- <button>+</button> -->
          <button
            @click="clearItems"
            class="bg-red-500 px-4 py-2 flex items-center justify-center rounded-full text-gray-100"
          >
            <span class="material-icons"> delete </span>
          </button>
          <div class="relative">
            <div class="absolute right-1 top-1.5">
              <button
                @click="addItem"
                class="bg-[#262D59] px-4 py-1.5 flex items-center justify-center rounded-full text-gray-100"
              >
                <span class="material-icons"> add </span>
              </button>
            </div>

            <input
              v-model="userInput"
              class="border-slate-800 border py-2 text-[#262D5B] rounded-full pl-4 pr-16 w-[27rem] text-xl capitalize font-bold outline-teal-700 outline-8"
              type="text"
              placeholder="Write to add items"
            />
          </div>
          <button
            @click="shuffle"
            class="bg-[#262D5B] text-lg px-5 py-2 flex items-center justify-center rounded-full text-gray-100"
          >
            <span class="material-icons mr-1">sync</span>Shuffle Items
          </button>
          <div>
            <!-- <div class="upload-btn-wrapper">
              <button class="btn flex items-center gap-x-2">
                <span class="material-icons">cloud_upload</span>
                Upload file
              </button>
              <input type="file" ref="doc" @change="readFile" />
            </div> -->
            <input type="file" ref="doc" @change="readFile" />
          </div>
        </div>
      </div>

      <!-- items -->
      <div class="overflow-auto mt-8 max-h-[26rem] scrool-panel">
        <div
          class="flex flex-auto item-parent justify-center gap-3 max-h-[30rem] flex-wrap px-[15%]"
        >
          <div
            @mouseover="activeItemId = item.id"
            @mouseleave="activeItemId = null"
            class="relative"
            v-for="item in items"
          >
            <button
              v-if="activeItemId == item.id"
              @click="delItem(item.id)"
              class="bg-red-500 absolute top-1 right-1 px-1 py-1 flex items-center justify-center rounded-full text-gray-100"
            >
              <span class="material-icons"> close </span>
            </button>
            <p
              :class="{
                ' rounded-3xl hover:shadow-md transition-all duration-200 shadow-sm flex justify-center items-center font-bold capitalize  border text-lg border-gray-400 px-4 py-1': true,
                'bg-white text-slate-500':
                  selectedItemId == null || selectedItemId != item.id,
                'bg-red-500 text-white':
                  selectedItemId != null && selectedItemId == item.id,
              }"
            >
              <span class="item-text">{{ item.data }}</span>
            </p>
          </div>
        </div>
      </div>

      <!-- <div>{{ content }}</div> -->
    </div>
  </div>
</template>

<style scoped>
.flex-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  font-size: 30px;
  text-align: center;
}
.flex-item {
  padding: 10px;
  flex: 0 1 30%; /* to all flex-items */
}

.animated-background {
  background: linear-gradient(to right, #fbf2db, #eff7de, #ebc973);
  background-size: 400% 400%;
  animation: animate-background 10s infinite ease-in-out;
}

@keyframes animate-background {
  0% {
    background-position: 0 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0 50%;
  }
}

.item-text {
  opacity: 100%;
}

.scrool-panel::-webkit-scrollbar {
  width: 3px;
  height: 3px;
}

/* Track */
.scrool-panel::-webkit-scrollbar-track {
  background: #e2e2e3;
}

/* Handle */
.scrool-panel::-webkit-scrollbar-thumb {
  background: #cd3535;
}
input[type="file"]::file-selector-button {
  margin-right: 20px;
  border: none;
  background: #262d5b;
  padding: 10px 20px;
  border-radius: 10px;
  color: #fff;
  cursor: pointer;
  border-radius: 5rem;
}
</style>
