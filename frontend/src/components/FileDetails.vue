<template>
    <div class="col-lg">
        <h3>Change File Name?</h3>
        <p></p>
        <input class="form-control" type="text" ref="fileInput" v-model="newFileName" placeholder="Please include file type. (For example: .pdf .txt .png)" required/>

        <p></p>
        <h3>Files Saved</h3>
        <p></p>

        <div v-if="confirmationMessage" class="alert alert-success mt-3">{{ confirmationMessage }}</div>
        <div v-if="errorMessage" class="alert alert-danger mt-3">{{ errorMessage }}</div>

        <div class="card" v-for="eachFile in databaseFiles" :key="eachFile._id">
          <div class="card-body d-flex justify-content-between align-items-center">
            <div>
              <h5 class="card-title">{{eachFile.filename}}</h5>
              <p class="card-subtitle"><strong>Date:</strong> {{ formatDate(eachFile.uploadDate) }}</p>
              <p class="card-subtitle"><strong>Time:</strong> {{ formatTime(eachFile.uploadDate) }}</p>
            </div>
            <div>
              <button class="btn btn-primary btn-sm me-2" @click="download(eachFile._id, eachFile.filename)">Download</button>
              <button class="btn btn-warning btn-sm me-2" @click="update(eachFile._id)">Rename</button>
              <button class="btn btn-danger btn-sm" @click="deletion(eachFile._id)">Delete</button>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { format } from 'date-fns';

export default {
  data() {
        return {
            databaseFiles: [],
            newFileName: '',
            confirmationMessage: '',    // For success message
            errorMessage: ''            // For error message
        }
  },

  methods: {

    formatDate(dateString) {
      return format(new Date(dateString), 'MM/dd/yyyy');
    },

    formatTime(dateString) {
      return format(new Date(dateString), 'HH:mm');
    },


    onFileChange(event) {
      this.file = event.target.files[0]
    },

    getall() {
      axios.get('http://localhost:3000/files/')
      .then(response => {
        // Handle the response Successful
        console.log(response.data)

        this.databaseFiles= response.data
      })
      .catch(error => {
          console.error(error)
      })
    },

    update(id) {
      if (this.newFileName.length< 1) {

          this.confirmationMessage = ''
          this.errorMessage = 'Please enter a file name'
        
      } else {

        axios.patch('http://localhost:3000/rename/'+ id + '/' + this.newFileName)
          .then(response => {
            // Handle the response if necessary
            console.log(response.data)
            this.getall()

            this.confirmationMessage = 'File name changed to ' + this.newFileName
            this.errorMessage = ''
          })
          .catch(error => {
            console.error(error)

            this.confirmationMessage = ''
            this.errorMessage = 'Error renaming file, try again'
          })

      }
      
    },

    deletion(id) {

      axios.delete('http://localhost:3000/delete/'+ id)
        .then(response => {
          // Handle the response if necessary

          // Success message
          this.confirmationMessage = 'file deleted successfully!'
          this.errorMessage = ''

          console.log(response.data)
          this.getall()
        })
        .catch(error => {

          this.confirmationMessage = ''
          this.errorMessage = 'Error deleting file, try again'
          console.error(error)
        })
    },

    download(id, filename) {

      axios.get(`http://localhost:3000/download/`+id, {
          responseType: 'blob' // Important for handling binary data
        })
        .then(response => {

          // // Create a URL for the file
          const url = window.URL.createObjectURL(new Blob([response.data]));
          const link = document.createElement('a');
          link.href = url;
          link.setAttribute('download', filename); // Replace with the desired file name
          document.body.appendChild(link);
          link.click();
          link.remove(); // Clean up
        })
        .catch(error => {

          this.confirmationMessage = ''
          this.errorMessage = 'Error downloading file, try again'
          console.error(error)
        })
    }

  },

  mounted() {
    this.getall()
  }
}
</script>

<style scoped>
.btn{
    margin: 10px;
}
.card{
    padding: 0px;
    margin-bottom: 10px;
}
.h5 {
  color: var(--primary);
}
</style>