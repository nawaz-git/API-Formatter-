<template>
  <div class="main">
    <h1>
      API <span>FORMATTER</span>
    </h1>
    <div class="input">
      <v-text-field v-model="data" placeholder="Enter Fetch Response Here"></v-text-field>
    </div>
    <button v-on:click="getRequest(data)">Format</button>
    <div class="copy">
      <span class="material-symbols-outlined copybtn" v-on:click="copy()">content_copy</span>
    </div>
    <pre v-if="api !== ''" id="input">
      <code>
        <p style="color:white">{{ req_method }} - {{ api }} <br/> BODY - {{ payload }}</p>
      </code>
    </pre>
  </div>
</template>

<script setup lang="ts">

import { ref } from 'vue'

let data = ref('')
let input: any = ref(null)
let api = ref('')
let payload: any = ref('')
let req_method: any = ref('')

const getRequest = (data: string) => {

  // Get URL
  let url = data.split('("')[1].split('",')[0]
  console.log(url);

  // Get Method
  let method = data.split('"method":')[1].split('"')[1].toString()
  req_method.value = method

  console.log(method);

  let body;
  if (method == 'POST' || 'PUT' || 'UPDATE' || 'DELETE') {

    // Get Body 
    body = data.split('body":')[1].split(', "method"')[0]
    body = body.replace(/\\/g, "")
    body = body.replace(/['"]+/g, '')
    body = body.split("method")[0]
    body = body.trim()
    body = body.substring(0, body.split("method")[0].length - 1)
  }

  api.value = url
  payload.value = body

}

const copy = () => {
  let text: string = `${req_method.value} - ${api.value}\n BODY - ${payload.value} `
  navigator.clipboard.writeText(text);
}

</script>

<style scoped>
.main {
  background-color: #10181b;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.input {
  margin-right: 0.5rem;
  display: block;
  width: 50%;
  border-top-width: 0px;
  border-bottom-width: 04px;
  --tw-border-opacity: 1;
  border-bottom-color: rgb(168 85 247 / var(--tw-border-opacity));
  background-color: rgb(18 24 27 / var(--tw-bg-opacity));
  --tw-bg-opacity: 0.5;
  padding: 0.75rem;
  font-family: sofia-pro, sans-serif;
  font-size: 1.25rem;
  line-height: 1.75rem;
  --tw-text-opacity: 1;
  color: rgb(255 255 255 / var(--tw-text-opacity));
  outline: 2px solid transparent;
  outline-offset: 2px;
}

button {
  margin-top: .125rem;
  margin-bottom: .125rem;
  display: inline-flex;
  cursor: pointer;
  --tw-bg-opacity: 1;
  background-color: rgb(255 255 255 / var(--tw-bg-opacity));
  padding: .5rem 1.25rem;
  text-align: center;
  font-family: sofia-pro, sans-serif;
  font-size: .875rem;
  line-height: 1.25rem;
  font-weight: 700;
  text-transform: uppercase;
  --tw-text-opacity: 1;
  color: rgb(0 0 0 / var(--tw-text-opacity));
  -webkit-text-decoration-line: none;
  text-decoration-line: none;
  --tw-shadow: 0 4px 6px -1px rgb(0 0 0 / .1), 0 2px 4px -2px rgb(0 0 0 / .1);
  --tw-shadow-colored: 0 4px 6px -1px var(--tw-shadow-color), 0 2px 4px -2px var(--tw-shadow-color);
  box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow);
  transition-property: all;
  transition-timing-function: cubic-bezier(.4, 0, .2, 1);
  transition-duration: .15s;
}

pre {
  border-radius: 05px;
  padding: 20px;
  height: min-content;
  display: block;
  font-family: monospace;
  white-space: pre;
  margin: 1em 0;
  background-color: #222529;
  width: 600px;
}

pre,
code {
  white-space: normal;
}

h1 {
  color: white;
  font-family: Poppins;
  font-size: 40px;
  font-weight: 800;
}

span {
  color: #00b551;
}

.copy {
  width: 600px;
  position: absolute;
  top: 59%;
  left: 29%;
  display: flex;
  justify-content: end;
}

.copybtn {
  cursor: pointer;
}
</style>
