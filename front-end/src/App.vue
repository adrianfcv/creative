<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img src="/sports.svg" width="90" height="90">
      </div>
    </router-link>
    <div class="title">
      <h1>     The Greatest Athletes in History  </h1>
    </div>
  </div>
  <div class="tab">
      <router-link to="/soccer">
        <button type="button"><b>Soccer</b></button>
      </router-link>
      <router-link to="/football">
        <button type="button"><b>Football</b></button>
      </router-link>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <router-link to="/admin">Admin</router-link>
    <p><a href="https://github.com/BYU-CS-260-Winter-2020/lab-4-museum-of-ordinary-objects-adrianfcv">myGitHub</a></p>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: white;
  color: #1C454F;
  border: 4px solid black;
}

.title {
  margin-top: 5px;
  margin-left: 15px;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: white;
  font-size: 12px;
  border: 4px solid black;
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

.tab {
  overflow: hidden;
  border-right: 4px solid black;
  border-left: 4px solid black;
  border-bottom: 4px solid black;
  background-color: white;
}

/* Style the buttons that are used to open the tab content */
.tab button {
  background-color: inherit;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 10px 12px;
  transition: 0.3s;
}

/* Change background color of buttons on hover */
.tab button:hover {
  background-color: #ddd;
}

/* Create an active/current tablink class */
.tab button.active {
  background-color: #ccc;
}

</style>
