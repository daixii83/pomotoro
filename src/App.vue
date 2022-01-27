<template lang="pug">
#app
  b-button-group(vertical)#btnGroup
    b-button#navBtn(to='/')
      img(src='./assets/tomato.png' alt='tomato')
    b-button#navBtn(to='/settings')
      img(src='./assets/music.png' alt='music')
    b-button#navBtn(to='/list')
      img(src='./assets/chart.png' alt='chart')
  keep-alive
    router-view(v-if="$route.meta.keepAlive")
  router-view(v-if="!$route.meta.keepAlive")
</template>
