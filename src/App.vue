<template>
  <div id="app">
    <div style="width: 500px;margin: 0 auto">
      <Tabs>
        <TabPanel label="Tab1" name="T1">aaaaaaaaaaaaaaaaaa</TabPanel>
        <TabPanel label="Tab2" name="T2">bbbbbbbbbbbbbbbbbb</TabPanel>
        <TabPanel label="Tab3" name="T3">cccccccccccccccccc</TabPanel>
      </Tabs>
    </div>
    
  </div>
</template>

<script>
import Tabs from './components/Tabs'
import TabPanel from './components/TabPanel'
export default {
  name: 'app',
  components: {
    Tabs,
    TabPanel
  },
  data(){
    return {
      
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
