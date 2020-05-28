<!--suppress CssUnknownTarget -->
<template>
  <div>
    <nav>
      <nuxt-link to="/" class="logo">
        <span class="big">whois.alieris.dev</span>
        <span class="small">
          <font-awesome-icon :icon="['fas', 'home']" />
        </span>
      </nuxt-link>
      <form class="float-right" @submit.prevent="submit()">
        <div>
          <input
            id="ipInput"
            v-model.trim="ipInput"
            type="text"
            name="ipInput"
            required
            autofocus
          />
          <label for="ipInput">IP to query</label>
        </div>
      </form>
    </nav>
    <nuxt />
  </div>
</template>

<script>
export default {
  data() {
    return {
      ipInput: ''
    }
  },
  created() {
    this.ipInput = this.$route.params.ipInput
  },
  methods: {
    submit() {
      this.$router.push('/whois/' + this.ipInput)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto&display=swap');

:root {
  --text-color: #fff;
  --primary-color: #1e1e1e;
  --background-color: #121212;
  --card-background-color: #1c1c1c;
  --card-dialog-background-color: #2e2e2e;
  --default-padding: 14px;
  --navbar-logo-size: 24px;
  --light-button-padding: 4px 10px;
  --light-button-border: 1px #fff solid;
  --card-padding: 20px;
  --standard-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
  --default-border-radius: 4px;
  --input-padding: 10px;
  --blurry-shadow: 0 5px 5px -3px rgba(0, 0, 0, 0.2),
    0 8px 10px 1px rgba(0, 0, 0, 0.14), 0 3px 14px 2px rgba(0, 0, 0, 0.12);
}

html {
  min-height: 100%;
}

body {
  font-family: 'Roboto', 'Helvetica Neue', 'Helvetica', 'Arial', sans-serif;
  margin: 0;
  min-height: 100%;
  color: var(--text-color);
  background-color: var(--background-color);
}

#ipInput {
  width: 300px;
  margin-top: -5px;
}

nav a,
a:active,
a:link,
a:visited {
  color: var(--text-color);
  text-decoration: none;
}

a:hover:not(.no-decoration) {
  text-decoration: underline;
  color: var(--text-color);
}

nav {
  padding: var(--default-padding);
  background-color: var(--primary-color);
  -webkit-box-shadow: var(--standard-shadow);
  -moz-box-shadow: var(--standard-shadow);
  box-shadow: var(--standard-shadow);
}

nav .logo {
  font-size: var(--navbar-logo-size);
}

table > tr > td {
  padding: 4px 0;
}

table > tr > td:first-child {
  padding-right: 10px;
}

.float-right {
  float: right;
}

.card {
  -webkit-box-shadow: var(--standard-shadow);
  -moz-box-shadow: var(--standard-shadow);
  box-shadow: var(--standard-shadow);
  background-color: var(--card-background-color);
  -webkit-border-radius: var(--default-border-radius);
  -moz-border-radius: var(--default-border-radius);
  border-radius: var(--default-border-radius);
  padding: var(--card-padding);
  margin: 20px;
}

.pre {
  white-space: pre-line;
}

.margin {
  margin: 4px;
}

.margin {
  padding: 10px;
}

.strike-through {
  text-decoration: line-through;
}

@media (max-width: 529px) {
  nav .logo .big {
    display: none;
  }
}

@media (min-width: 530px) {
  nav .logo .small {
    display: none;
  }
}
form:not(.inline) input {
  display: block;
  width: calc(100% - 20px);
}

form > div > input {
  font-size: 14px;
  color: var(--text-color);
  font-family: unset;
  border: var(--light-button-border);
  -webkit-border-radius: var(--default-border-radius);
  -moz-border-radius: var(--default-border-radius);
  border-radius: var(--default-border-radius);
  padding: var(--input-padding);
  background: unset;
}

form {
  background-color: inherit;
}

form > div {
  position: relative;
  background-color: inherit;
}
form > div > label {
  top: var(--input-padding);
  left: var(--input-padding);
  position: absolute;
  transition: 0.1s;
  cursor: text;
}

form > div > input:focus + label,
form > div > input:valid + label,
form > div > select + label {
  font-size: 12px;
  top: -7px;
  padding: 0 5px;
  left: 5px;
  background-color: inherit;
}
</style>
