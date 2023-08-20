<template>
  <div class="chatbox-container">
    <div
      v-for="message in messages"
      class="chatbox-message-container"
      :class="{
        chatbot: message.sender === `chatbot`,
        chatuser: message.sender === `user`,
      }"
    >
      <img class="chatbox-icon" :src="`/assets/` + message.img" />
      <div class="chatbox-message">
        <img
          v-if="message.loading"
          class="chatbox-loading-animation"
          src="/assets/loading.gif"
        />
        <div v-else>{{ message.text }}</div>
      </div>
    </div>

    <div v-if="!optionsHidden" class="chatbox-options-container">
      <div
        v-for="option in options"
        @click="clickOption(option.id)"
        class="chatbox-option"
      >
        {{ option.name }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref} from "vue";

let optionsHidden = ref(true);

const messages = ref([
  {
    sender: "chatbot",
    img: "../assets/robot2.jpg",
    loading: true,
    text: "Привет! Что я могу для Вас сделать?",
  },
]);

const options = [
  { id: 1, name: "Заказать пиццу" },
  { id: 2, name: "Установить будильник" },
  { id: 3, name: "Вывести погоду" },
];

loadMessages();

function loadMessages() {
  let count = 0;
  messages.value.forEach((message) => {
    if (message.loading) {
      count = count + 1;
      setTimeout(() => {
        message.loading = false;
      }, 1000 * count);
    }
  });

  setTimeout(() => {
    optionsHidden.value = false;
  }, 1000 * count);
}

function clickOption(id) {
  optionsHidden.value = true;
  messages.value.push(
    {
      sender: "user",
      img: "user.png",
      loading: true,
      text: `${options.find((option) => option.id === id).name}`,
    },
    {
      sender: "chatbot",
      img: "robot2.jpg",
      loading: true,
      text: `Вы выбрали опцию "${
        options.find((option) => option.id === id).name
      }". Что еще могу сделать?`,
    }
  );
  loadMessages();
}


</script>

<style scoped>
:root {
  --white: rgb(255, 255, 255);

  --yellow: rgb(248, 199, 4);
  --byellow: rgb(252, 220, 81);

  --gray: rgb(164, 180, 204);
}
.chatbox-container {
  width: 100%;
  background-color: rgb(255, 255, 255);
  border-radius: 0.7rem;
  padding: 0.45rem;

  font-size: 0.9rem;

  display: flex;
  flex-direction: column;
  row-gap: 1rem;
  overflow-y: auto;
  max-height: 100%;
}

.chatbox-container > * {
  width: 100%;
  margin: 0;
  padding: 0;
  outline: none;
  box-sizing: border-box;
  font-family: "monospace", sans-serif;
  color: rgb(0, 0, 0);
}

.chatbox-message-container {
  display: flex;
  align-items: flex-end;
  column-gap: 0.3rem;
  animation: 500ms showup;
}

@keyframes showup {
  from {
    transform: scale(0);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.chatuser {
  justify-content: end;
  flex-direction: row-reverse;
}

.chatbox-icon {
  width: 50px;
  height: 50px;
  border-radius: 50% 50% 0 50%;
  box-shadow: rgba(0, 0, 0, 0.15) 0px 0px 3px 0px;
  padding: 0.2rem;
}

.chatbot .chatbox-icon {
  background-color: rgb(248, 199, 4);
}

.chatuser .chatbox-icon {
  border-radius: 50% 50% 50% 0;
  background-color: rgb(255, 255, 255);
}

.chatbox-loading-animation {
  width: 30px;
  height: 10px;
}

.chatbox-message {
  padding: 0.8rem;
}

.chatbot .chatbox-message {
  background-color: rgb(248, 199, 4);
  border-radius: 1.1rem 1.1rem 1.1rem 0;
}

.chatuser .chatbox-message {
  box-shadow: rgba(0, 0, 0, 0.15) 0px 0px 3px 0px;
  background-color: rgb(255, 255, 255);
  border-radius: 1.1rem 1.1rem 0 1.1rem;
}

.chatbox-option {
  background-color: rgb(248, 199, 4);
  padding: 0.8rem;
  width: fit-content;
  border-radius: 1.1rem;
  margin-bottom: 0.5rem;
  user-select: none;
  cursor: pointer;
}

.chatbox-option:hover {
  filter: brightness(125%);
}
</style>
