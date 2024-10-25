<template>
  
  <div>
    <Navbar/>
    <div class="container-fluid">
      <div class="row">
        <FileForm/>
        <FileDetail/>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import FileForm from './components/FileForm.vue'
import FileDetail from "./components/FileDetails.vue"

export default {
  components: { Navbar, FileForm, FileDetail}
}
</script>
