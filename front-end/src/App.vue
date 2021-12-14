<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <p>Home </p>
        <p>     </p>
      </div>
    </router-link>
    <router-link to="/admin">
      <div class="logo">
        <p>   Admin</p>


      </div>
    </router-link>
    <div class="title">
      <h1>Basketball Players</h1>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <router-link to="/admin">Admin</router-link>
    <br>
    <a href ="https://github.com/joeparis6/project4.git">https://github.com/joeparis6/project4.git</a>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
background: #2222ed;
  text-align: center;
  color: white;
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: #FFA500;
  color: #1C454F;
}

.title {
  margin-top: 5px;
}

.title h1 {
  text-align: center;
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

.logo {
  margin: 10px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
