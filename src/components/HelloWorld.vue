<template>
  <div class="hello">
   <div class="container">
     <div class="row">
       <div class="col-12 col-md-8 m-md-auto mb-md-3 m-lg-0  col-lg-7 animate__animated animate__fadeInDown  fw-bold text-secondary ">
         <div class="card bg-light shadow border border-0  ">
           <div class="card-title mt-4 ">
             <h2>Contact Create </h2>
           </div>
           <div class="card-body">
             <form id="storeForm" ref="form" class="row g-3 needs-validation" enctype="multipart/form-data" novalidate>
               <div class="">
                 <div class="  position-relative   ">
                   <label for="validationTooltip05" class="form-label  d-none">Photo</label>
                   <input name="photo" @change="Cphoto" hidden type="file" class="form-control " id="validationTooltip05" required >
                   <div class="invalid-tooltip  ">
                     Please select a valid photo.
                   </div>
                 </div>
                 <img @click="photoSelect" id="sampleImg" src="../assets/logo.png"  width="100" height="100" class="mb-3 rounded-circle border border-2 border-secondary p-2" alt="">

               </div>


               <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
                 <label for="validationTooltip01" class="form-label">Name</label>
                 <input name="name" type="text" class="form-control" id="validationTooltip01"  required>
                 <div class="valid-tooltip">
                   Looks good!
                 </div>
                 <div class="invalid-tooltip">
                   Name field is required
                 </div>
               </div>

               <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
                 <label for="validationTooltipUsername" class="form-label">Email</label>
                 <div class="input-group has-validation">
                   <span class="input-group-text" id="validationTooltipUsernamePrepend">@</span>
                   <input name="email" type="email" class="form-control" id="validationTooltipUsername" aria-describedby="validationTooltipUsernamePrepend" required>
                   <div class="invalid-tooltip">
                     Please choose valid email.
                   </div>
                 </div>
               </div>
               <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
                 <label for="validationTooltip03" class="form-label">Address</label>
                 <input name="address" type="text" class="form-control" id="validationTooltip03" required>
                 <div class="invalid-tooltip">
                    valid address.
                 </div>
               </div>

               <div class="col-md-6 mb-4 text-start fw-bold  position-relative">
                 <label for="validationTooltip06" class="form-label">Phone</label>
                 <input name="phone"  type="text" class="form-control" id="validationTooltip06" required >
                 <div class="invalid-tooltip">
                   Please fill out phone Number
                 </div>
               </div>

               <div class="col-12 text-end ">
                 <button class="btn btn-secondary text-white-50  " @click="storeData" type="button">Submit</button>
               </div>
             </form>
           </div>
         </div>
       </div>
       <div class="col-12 col-md-8 m-md-auto mb-md-3 m-lg-0  col-lg-5">
         <div class="mb-3 card card-body bg-light border-0 shadow  ">
          <div class="d-flex">
            <input type="text" class="form-control me-3" v-model="searchText" @keyup="search">
            <button class="btn btn-outline-info" @click="search">
              <i class="bi-search"></i>
            </button>
          </div>
         </div>
         <div class="card bg-light shadow border border-0  overflow-hidden mb-3   " v-for="c in LoopContacts" :key="c.id">
           <div class="card-body d-flex justify-content-md-between justify-content-around align-items-center  ">
             <img :src=" c.photo " class="rounded-circle p-2 border-1 border-secondary  border " width="50" height="50" alt="">
             <div class="d-flex justify-content-start flex-column align-items-start text-secondary">
               <p class="fw-bolder mb-1 ">Name</p>
               <p class="mb-0  small ">{{ c.name }}</p>
             </div>
             <div class="d-flex justify-content-start flex-column align-items-start text-secondary">
               <p class="fw-bolder mb-1 ">Phone</p>
               <p class="mb-0 small  ">{{ c.phone }}</p>
             </div>
             <div class="d-md-flex d-none  justify-content-start flex-column align-items-start text-secondary">
               <p class="fw-bolder mb-1 ">Email</p>
               <p class="mb-0  small ">{{ c.email }}</p>
             </div>

             <div class="btn-group position-absolute top-0" style="right: 0"  role="group" aria-label="First group">
               <button @click="edit(c)" class="btn btn-sm btn-outline-secondary " data-bs-toggle="modal" data-bs-target="#staticBackdrop" > <!-- :data-bs-target="'#staticBackdrop'+c.id" -->
                 <i class="bi-pencil-fill  "></i>
               </button>
               <button @click="del(c)" class="btn btn-outline-info  btn-sm" >
                 <i class="bi-trash"></i>
               </button>
             </div>

              <EditModal :data="c"> </EditModal> <!-- why did not i get data update when click edit btn  ? -->
           </div>
         </div>
       </div>
     </div>
   </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2/dist/sweetalert2.js'
import axios from 'axios';
import EditModal from "@/components/EditModal";
export default {
  name: 'HelloWorld',
  components: {EditModal},
  data() {
    return {
      name: 'ivan',
      email: '',
      phone: '',
      address: '',
      photo :null,
      contacts : null,
      data : undefined,
      searchText:null,
    }
  },
  computed: {
      LoopContacts(){
        return this.contacts;
      }
  },
  methods: {
    search() {
        if(this.searchText.length > 0){
          this.contacts = this.LoopContacts.filter(el => {
            let check = el.name.toLowerCase().search(this.searchText.toLowerCase().trim());
            if(check > -1){
               console.log(el)
               return el;
            }

          })

        }else {
          this.getContacts();
        }
    },
    Cphoto(c) {
      this.photo = c.target.files[0];
      let fileReader = new FileReader();
      fileReader.readAsDataURL(this.photo);
      fileReader.onload = function (){
        document.getElementById('sampleImg').src = fileReader.result;
      }

    },
    storeData() {
      var forms = document.querySelectorAll('.needs-validation')
      let arr = Array.prototype.slice.call(forms);
      console.log(this.data);

      arr.forEach(function (form){
        if (form.checkValidity()) {
          let data = new FormData(form);

          axios.post('http://127.0.0.1:8000/api/contact', data, {
          }).then((res) => {
            if(res.status === 200){
              console.log(res.data);
              //this.contacts.push(res.data);

              form.classList.remove('was-validated');
              form.reset()

            }
          })

        }

        form.classList.add('was-validated')
      })

    },
    edit(c){
      this.data  = c;
      // console.log(c)
    },
    del(data){
      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!',
        reverseButtons : true
      }).then((result) => {
        if (result.isConfirmed) {
          this.contacts = this.contacts.filter(el => el.id !== data.id);
          axios.delete('http://127.0.0.1:8000/api/contact/'+data.id, {
          }).then((res) => {
            if(res.status === 200){
              console.log(res.data);
              Swal.fire(
                  'Deleted!',
                  'Your file has been deleted.',
                  'success'
              )
            }
          })

        }
      })

    },
    photoSelect(){
      let photoFile = document.getElementById('validationTooltip05');
      photoFile.click();
    },
    getContacts(){

      axios.get('http://127.0.0.1:8000/api/contact',{})
          .then(res => res.data)
          .then(result =>this.contacts = result );

    }

  },
  mounted() {
    this.getContacts();
  }

}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
