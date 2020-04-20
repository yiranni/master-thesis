<template>
  <div>
    <nuxt />
  </div>
</template>

<style>
html {
  font-family: "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI",
    Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

body {
  margin: 0;
}

.model {
  position: relative;
}

#intro {
  height: 100vh;
  width: 40vw;
  background: transparent;
  position: absolute;
  top: 0px;
  color: #0f91da;
  font-family: "Roboto", sans-serif;
}

#intro h1 {
  padding-top: 4rem;
  padding-left: 5rem;
  font-size: 3rem;
  font-weight: 900;
  margin-bottom: 0;
  letter-spacing: 3px;
}

#intro h2 {
  padding-left: 5rem;
  font-size: 2rem;
  margin-top: 0;
  font-weight: 900;
  letter-spacing: 3px;
}
#detail {
  color: white;
  font-weight: 300;
  padding-left: 5rem;
  line-height: 1.5rem;
  margin-top: 4rem;
}
</style>
