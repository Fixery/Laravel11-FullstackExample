<template>
    <div class="container">
        <h3 align="center" class="mt-5">Data Peserta</h3>
        <div class="row">
            <div class="col-md-2">
            </div>
            <div class="col-md-8">
            <div class="form-area">
              <form @submit.prevent="save" id="check-register-form">
                 
                    <div class="row">
                        <div class="col-md-6">
                          <label>Nama</label>
                          <v-text-field
                          v-model="employee.name"
                          label="Nama"
                          required>
                         </v-text-field>
                        </div>

                        <div class="col-md-6">
                            <label>Alamat</label>
                            <v-text-field
                            v-model="employee.address"
                            label="Alamat"
                            required>
                           </v-text-field>
                        </div>

                    </div>
                    
                    <div class="row">
                        <div class="col-md-12">
                            <label>No. Telpon</label>
                            <v-text-field
                            v-model="employee.phone"
                            label="No. Telepon"
                            required>
                           </v-text-field>
                          
                        </div>
                    </div>
                    
                    <div class="row">
                        <div class="col-md-12 mt-3">
                          <v-btn type="submit" color="success" form="check-register-form">Save</v-btn>
                        </div>
                    </div>
                </form>
            </div>
            <v-table theme="dark">
              <thead>
                <tr>
                  <th class="text-left">
                    ID Peserta
                  </th>
                  <th class="text-left">
                    Nama Peserta
                  </th>
                  <th class="text-left">
                    Alamat
                  </th>
                  <th class="text-left">
                    No. Telepon
                  </th>
                  <th class="text-left">
                    Aksi
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="employee in result"
                  :key="employee.id"
                >
                  <td>{{ employee.id }}</td>
                  <td>{{employee.name }}</td>
                  <td>{{ employee.address }}</td>
                  <td>{{ employee.phone }}</td>
                  <td>
                  <v-btn type="button" color="info"  @click="edit(employee)">Edit</v-btn>
                          
                          <v-btn type="button" color="danger"  @click="remove(employee)">Delete</v-btn>
                        </td>        
                </tr>
              </tbody>
            </v-table>
            
            </div>
        </div>
    </div>

</template>



<script>

import axios from 'redaxios';
export default {
  name: 'Employee',
  data () {
    return {
      result: {},
      employee:{
                 id: '',
                 name: '',
                 address: '',
                 phone: ''


      }
    }
  },
  created() { 
      this.EmployeeLoad();
  },
  mounted() {
        console.log("mounted() called.......");
       
    },

  methods: {
    EmployeeLoad()
         {
               var page = "http://127.0.0.1:8000/api/employee";
               axios.get(page)
                .then(
                    ({data})=>{
                      console.log(data);
                      this.result = data;
                    }
                );
         },
         
        
         save()
         {
          if(this.employee.id == '')
            {
              this.saveData();
            }
            else
            {
              this.updateData();
            }       

         },
         saveData()
         {
          axios.post("http://127.0.0.1:8000/api/employee", this.employee)
          .then(
            ({data})=>{
              this.EmployeeLoad();
              this.employee.name = '';
              this.employee.address = '',
              this.employee.phone = ''
              this.id = ''
              alert("Data berhasil disimpan!"); // Tambahkan alert di sini
            }
          )

         },
         edit(employee)
         {
          this.employee = employee;
         
         },
         updateData()
         {
            var editrecords = 'http://127.0.0.1:8000/api/employee/'+ this.employee.id;
            axios.put(editrecords, this.employee)
            .then(
              ({data})=>{
                this.employee.name = '';
                this.employee.address = '',
                this.employee.phone = ''
                this.id = ''
                alert("Update Data Berhasil");
                this.EmployeeLoad();
              }
            );

         },

         remove(employee){
           var url = `http://127.0.0.1:8000/api/employee/${employee.id}`;
           if (confirm("Apakah Anda yakin ingin menghapus data ini?")) { // Tambahkan konfirmasi di sini
             axios.delete(url)
               .then(() => {
                 alert("Hapus Data Berhasil");
                 this.EmployeeLoad();
               });
           }
         }
    }
}
</script>



<style scoped>
      .form-area{
        padding: 20px;
        margin-top: 20px;
          background-color:#0b0b0b;
          color: #fffcfc;
      }
      .bi-trash-fill{
        color:red;
        font-size: 18px;
      }
      .bi-pencil{
        color:green;
        font-size: 18px;
        margin-left: 20px;
      }
</style>