<template>
  <!-- Modal :id="'staticBackdrop'+contact.id"-->
  <div class="modal fade" id="staticBackdrop"  data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content bg-light ">
        <div class="modal-header">
          <h5 class="modal-title fw-bolder text-secondary  " id="staticBackdropLabel"> Edit Contact </h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <img @click="editPhotoSelect" id="editSampleImg" :src="contact.photo"  width="100" height="100" class="mb-3 rounded-circle border border-2 border-secondary p-2" alt="">

          <form ref="form" class="row g-3" :class="'needs-validation'+contact.id" enctype="multipart/form-data" novalidate>
            <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
              <label for="validationTooltipEdit01" class="form-label">Name </label>
              <input name="name" type="text" :value="contact.name" class="form-control" id="validationTooltipEdit01"  required>
              <div class="valid-tooltip">
                Looks good!
              </div>
              <div class="invalid-tooltip">
                Name field is required
              </div>
            </div>

            <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
              <label for="validationTooltipUsernameEdit" class="form-label">Email</label>
              <div class="input-group has-validation">
                <span class="input-group-text" id="validationTooltipUsernamePrependEdit">@</span>
                <input name="email" type="email" :value="email" class="form-control" id="validationTooltipUsernameEdit" aria-describedby="validationTooltipUsernamePrependEdit" required>
                <div class="invalid-tooltip">
                  Please choose a unique and valid email.
                </div>
              </div>
            </div>
            <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
              <label for="validationTooltipEdit03" class="form-label">Address</label>
              <input name="address" type="text" :value="contact.address" class="form-control" id="validationTooltipEdit03" required>
              <div class="invalid-tooltip">
                Please provide a valid addres.
              </div>
            </div>
            <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
              <label for="validationTooltipEdit07" class="form-label">Phone</label>
              <input name="address" type="text" :value="contact.phone" class="form-control" id="validationTooltipEdit07" required>
              <div class="invalid-tooltip">
                Please provide a valid phone.
              </div>
            </div>

            <div class="col-md-6 mb-4 text-start fw-bold  position-relative d-none">
              <label for="validationTooltipEdit05" class="form-label">Photo</label>
              <input name="photo" @change="EditPhoto"  type="file" class="form-control" id="validationTooltipEdit05"  accept="image/*">
              <div class="invalid-tooltip">
                Please select a valid photo.
              </div>
            </div>

          </form>

        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary"  @click="updateData(contact.id)">Save Edit</button>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios';

export default {
  name: "EditModal",
  props : ['data'],
  data() {
    return {
      contact:this.data,
      email : 'ivanpho@gmai.col',
    }
  },
  methods: {
    updateData(id) {
      var forms = document.querySelectorAll('.needs-validation'+id)
      let arr = Array.prototype.slice.call(forms);

      arr.forEach(function (form){
        if (form.checkValidity()) {
          let Fdata = new FormData(form);
          Fdata.forEach(function (e,i){
            console.log(e,i,id)
          });

          var config = {
            method: 'patch',
            url: 'http://127.0.0.1:8000/api/contact/'+id,
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/x-www-form-urlencoded'
            },
            data : Fdata
          };

          axios(config)
              .then(function (response) {
                console.log(JSON.stringify(response.data));
              })
              .catch(function (error) {
                console.log(error);
              });

        }
        form.classList.add('was-validated')
      })

    },
    editPhotoSelect(){
      document.getElementById('validationTooltipEdit05').click();
    },
    EditPhoto(c){
      console.log(this.data)

      let fileReader = new FileReader();
      fileReader.readAsDataURL(c.target.files[0]);
      fileReader.onload = function (){
        document.getElementById('editSampleImg').src = fileReader.result;
      }
    }
  },

  created() {
    // console.log(this.contact)
  }


}
</script>

<style scoped>

</style>