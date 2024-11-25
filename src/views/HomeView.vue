<script setup>
import { ref } from "vue";

const items = ref([]);

let userInput = ref("");
const activeItemId = ref(null);

const addItem = () => {
  items.value.push({ id: items.value.length + 1, data: userInput.value });
  userInput.value = "";
};

const clearItems = () => {
  items.value = [];
};

const delItem = (itemId) => {
  items.value = items.value.filter((item) => item.id != itemId);
};
</script>

<template>
  <div class="app animated-background bg-[#E7F0D3] min-h-screen max-h-screen">
    <div class="container mx-auto px-[10%]">
      <div class="flex flex-col items-center justify-center pt-16">
        <!-- <span class="material-icons">keyboard_double_arrow_right</span> -->
        <img
          class="w-40"
          src="https://cdn-icons-png.freepik.com/512/2038/2038515.png"
          alt=""
        />
        <p class="text-4xl font-bold mt-4 text-[#262D5B]">
          LotteryString - Random Name Picker
        </p>
        <p class="text-center text-xl my-4 leading-tight text-[#3E444D]">
          An online lottery tool that let you add items or text by entering them
          in the input box, once<br />
          you're added at least two items, simply press the shuffle button, and
          the app will randomly<br />
          select a winner
        </p>
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
            class="bg-[#262D5B] text-lg px-5 py-2 flex items-center justify-center rounded-full text-gray-100"
          >
            <span class="material-icons mr-1">sync</span>Shuffle Items
          </button>
        </div>
      </div>

      <!-- items -->
      <div
        class="flex flex-auto item-parent justify-center mt-8 gap-3 flex-wrap px-[22%]"
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
            class="bg-white rounded-3xl hover:shadow-md transition-all duration-200 shadow-sm flex justify-center items-center font-bold capitalize text-slate-500 border text-lg border-gray-400 px-4 py-1"
          >
            <span class="item-text">{{ item.data }}</span>
          </p>
        </div>
      </div>
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
  background: linear-gradient(to right, #fbf2db, #eff7de, #fbf2db);
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

.item-parent:hover .item-text {
}
</style>
