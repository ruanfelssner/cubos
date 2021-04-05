<template>
  <div id="app" class="home">    
      <div class="navbar">
        <h1 @click="$router.push('/')">Movies</h1>
        </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Abel&family=Lato:wght@300&display=swap');
*{
  box-sizing: border-box;
}
body{
  margin: 0;
  padding:0;
  border:0;
}
#app {
  font-family: 'Abel', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.row{
  display: flex;
  flex-wrap: wrap;
}
h1, h2, h3, h4, h5{
  margin:0;
  border:0;
  padding:0;
}

.home{
  
    max-width: 100%;
    overflow: hidden;
  .navbar{
    background:#116193;
    display: flex;
    justify-content: center;
    h1{
      color:#00e8e4;
      font-size:50px;
      padding:1rem 0;
      font-weight: initial;
    }
  }
}
</style>
