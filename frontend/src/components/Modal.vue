<template>
    <!-- Modal -->
    <div class="modal fade" id="warning_modal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
        <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Welcome</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
            <p> <strong>Note ❗ </strong> Info loading may take 2 mins because it uses FREE hosting at RENDER.com</p>
                <p><strong>Refresh if needed</strong></p>
        </div>
        </div>
    </div>
    </div>
</template>

<script>
import { Modal } from 'bootstrap'

export default {
  mounted() {
    // Initialize and show the modal after the component is mounted
    const myModal = new Modal(document.getElementById('warning_modal'))
    myModal.show()
  }
}
</script>