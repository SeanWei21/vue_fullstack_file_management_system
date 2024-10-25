<template>
    <div class="col-lg">

        <h3>Add New File</h3>
        <p></p>
        <input class="form-control" type="file" ref="fileInput" @change="onFileChange" required/>
        <button type="button" class="btn btn-primary mb-3" @click="upload">Upload</button>

        <div v-if="confirmationMessage" class="alert alert-success mt-3">{{ confirmationMessage }}</div>
        <div v-if="errorMessage" class="alert alert-danger mt-3">{{ errorMessage }}</div>
    </div>
</template>

<script>
import axios from 'axios'

export default {

    data() {
        return {
            file: null,
            confirmationMessage: '',    // For success message
            errorMessage: ''            // For error message
        }
    },

    methods: {

        onFileChange(event) {
            this.file = event.target.files[0]
        },

        upload() {
            const formData = new FormData()
            formData.append('file', this.file)

            axios.post('http://localhost:3000/upload', formData)
                .then(response => {
                    // Handle the response Successful
                    console.log(response.data)
                    console.log(this.file)

                    // Success message
                    this.confirmationMessage = this.file.name + ' uploaded successfully!'
                    this.errorMessage = ''

                    // reset state
                    this.file = null   
                    this.$refs.fileInput.value = '' // Clear the file input field
                })
                .catch(error => {
                    console.error(error)

                    // failure message
                    this.confirmationMessage = ''
                    this.errorMessage = 'Error uploading file. Please try again.'
                })
        }

    }
}
</script>

<style scoped>
.btn{
    margin: 10px;
}
</style>