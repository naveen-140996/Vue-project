<template>
  <!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.4.1/dist/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
    </head>
    <body>
    <div :class="{ 'dark-theme': isDarkMode, 'light-theme': !isDarkMode }">
         <div class="employee_master">
            <div class="side_bar primary_bg">
                <div class="d-flex flex-column align-items-center">
                    <div class="logo text-center my-3">
                        <img src="./assets/img/softinfo.png" width="50px" />
                    </div>
                    <div class="text-center my-3">
                        <img src="./assets/img/dashboard-icon.png" width="20px" />
                        <div class="module">Dashboard</div>
                    </div>
                    <div class="text-center my-3">
                        <img src="./assets/img/product.png" width="20px" />
                        <div class="module">Product & Modules</div>
                    </div>
                    <div class="text-center my-3">
                        <img src="./assets/img/user.png" width="20px" />
                        <div class="module">Users</div>
                    </div>
                    <div class="text-center my-3">
                        <img src="./assets/img/register.png" width="20px" />
                        <div class="module">Registrations</div>
                    </div>
                    <div class="text-center my-3">
                        <img src="./assets/img/invoice.png" width="20px" />
                        <div class="module">Invoice</div>
                    </div>
                    <div class="text-center my-3">
                        <img src="./assets/img/payment.png" width="20px" />
                        <div class="module">Payment</div>
                    </div>
                    <div class="text-center my-3">
                        <img src="./assets/img/approve.png" width="20px" />
                        <div class="module">Approve</div>
                    </div>
                </div>
            </div>
            <div class="main_container">
                <div class="d-flex justify-content-between">
                    <h2>Employee Master</h2>
                    <div class="theme" >
                        Search Theme 
                        <label class="switch ml-2">
                          <input type="checkbox" @click="toggleTheme">
                          <span class="dark round"></span>
                        </label>
                    </div>
                </div>
                <div class="my-3">
                    <div class="d-flex w-100 justify-content-between flex-direction">
                        <div class="employee_card">
                            <div class="mb-3 d-flex justify-content-between font-weight-bold">
                                <h4>Employee Info</h4>
                                <div class="edit" @click="editUser(index)"> <span><img src="./assets/img/icon_pen.png" width="12px" style=vertical-align:baseline></span>EDIT</div>
                            </div>
                          <form @submit.prevent="saveUser">
                            <div>
                              <input type="text" id="name" placeholder="Display Name" class="form-control" v-model="editedUser.name" required >
                            </div>
                            <div>
                              <input type="text" id="loginId" placeholder="Login ID" class="form-control" v-model="editedUser.loginId" required>
                            </div>
                            <div>
                              <input type="password" id="password"  class="form-control"  placeholder="Password" v-model="editedUser.password" required>
                            </div>
                            <div>
                              <input type="file" id="image" accept="image/*" @change="handleImageUpload">
                              <img v-if="editedUser.image" :src="editedUser.image" alt="User Image" style="max-width: 100px;">
                            </div>
                            <div class="mt-3 d-flex justify-content-between font-weight-bold">
                                <div>
                                  <p>Status</p>
                                </div>
                                <div class="d-flex justify-content-between align-items-center">
                                  <div class="toggle-switch"> 
                                    <label class="switch pt-3">
                                      <input type="checkbox">
                                      <span class="slider round"></span>
                                    </label>
                                  </div>
                                  <div>
                                  <p class="mb-0">Active</p>
                                  </div>
                                  
                                </div>
                            </div>
                            <div class="text-center">
                              <button class="save-edit-btn"  type="submit">{{ editingIndex !== null ? 'Save Changes' : 'Save' }}</button>
                            </div>
                          </form>
                              <!-- List of users -->
                        </div>
                        <div class="employee_card">
                            <div class="mb-3 d-flex justify-content-between font-weight-bold flex-direction">
                                <h4>View Employees ({{ totalUsers }})</h4>
                                <div class="search_box">
                                  <input type="text" v-model="searchQuery" placeholder="Search by name or ID">
                                </div>
                            </div>
                             <div class="row my-5 align-items-center" v-for="(user, index) in filteredUsers" :key="user.id">
                                <div class="col-2"><span class="emp_img"><img v-if="user.image" :src="user.image" alt="User Image" style="max-width: 50px;"></span></div>
                                <div class="col-5 text-center employee_id"> {{ user.name }}</div>
                                <div class="col-5 text-center"><button @click="editUser(index)" class="view-btn">View</button></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    </body>
</html>
</template>

<script>

export default {
  name: 'App',
  computed: {
     totalUsers() {
      return this.users.length;
    },
    filteredUsers() {
      const query = this.searchQuery.toLowerCase();
      return this.users.filter(user => {
        return user.name.toLowerCase().includes(query) || user.loginId.toLowerCase().includes(query);
      });
    }
  },
  data() {
    return {
      isDarkMode: false,
       users: [
        { id: 1, name: 'navin', loginId: '23', password: '1234' },
        { id: 2, name: 'vijay', loginId: '24', password: '4567' },
        { id: 3, name: 'ajith', loginId: '25', password: '7678' },
        { id: 4, name: 'sharu', loginId: '26', password: '9876' },
        { id: 5, name: 'deepi', loginId: '27', password: '98765' }
      ],
      newUser: {
        name: '',
        loginId: '',
        password: ''
      },
     editedUser: {
        id: null,
        name: '',
        loginId: '',
        password: ''
      },
      editingIndex: null,
      searchQuery: ''
    };
  },
    mounted() {
    // Load user data from local storage when the component is mounted
    const storedUsers = localStorage.getItem('users');
    if (storedUsers) {
      this.users = JSON.parse(storedUsers);
    }
  },
  methods: {
    
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode;
    },
      editUser(index) {
      this.editingIndex = index;
      this.editedUser = { ...this.users[index] };
    },
    saveUser() {
      if (this.editingIndex !== null) {
        // Editing an existing user
        this.users[this.editingIndex] = { ...this.editedUser };
      } else {
        // Adding a new user
        const newId = this.users.length > 0 ? this.users[this.users.length - 1].id + 1 : 1;
        this.users.push({ id: newId, ...this.editedUser });
      }
       localStorage.setItem('users', JSON.stringify(this.users));
       document.getElementById('image').value = null;
      this.cancelEdit();
    },
    cancelEdit() {
      this.editingIndex = null;
      this.editedUser = { id: null, name: '', loginId: '', password: '' };
    },
    deleteUser(index) {
      this.users.splice(index, 1);
      localStorage.setItem('users', JSON.stringify(this.users));
    },
    handleImageUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = () => {
          this.editedUser.image = reader.result;
        };
        reader.readAsDataURL(file);
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');
body {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.employee_master{
    width: 100%;
    overflow: hidden;

}
.primary_bg {
    background-color: #702F61;
    color: #ffff;
}
.side_bar {
    border-radius: 0 25px 25px 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 186px;
    height: 100%;
    background-color: purple;
    position: fixed;
}
.side_bar .logo {
    width: 50px;
    top: 50px;
}
.side_bar .logo img {
    max-width: 100%;
}
.side_bar .module {
    font-size: 14px;
}
.main_container {
    width: calc(100% - 186px);
    margin-left: 186px;
    padding: 25px;
    height: 100vh;
}
.main_container h2 {
    color: #4A4A4A;
    font-size: 35px;
    font-weight: 700;
}
.main_container .theme {
    font-size: 15px;
    color: #000;
    font-weight: 500;
}
.main_container .employee_card {
    border-radius: 20px;
    background-color: #fff;
    box-shadow: 0px 2px 9px 1px #00000014;
    border: 1px solid #E1E1E1;
    padding: 30px;
    width: 48%;
}
.employee_card ::placeholder {
    color: #B9B9B9;
    font-size: 14px;
}
.employee_card .form-control {
    border-color: #E9E9E9;
    margin-bottom: 20px;
}
.employee_card h4 {
    font-size: 19px;
    font-weight: 700;
    color: #4A4A4A;
}
.employee_card .edit {
    font-size: 14px;
    font-weight: 700;
    cursor:pointer;
}
.employee_card form h6 {
    font-size: 12px;
}
.employee_card form .status-btn {
    font-size: 10px;
    font-weight: 700;
}
.submit_btn {
    border-radius: 30px;
    padding: 5px 30px;
    font-size: 14px;
    font-weight: 700;
}
.view_btn {
    border-radius: 5px;
    padding: 5px 45px;
    font-size: 14px;
}
.employee_card .emp_img {
    width: 50px;
}
.employee_card .emp_img img {
    max-width: 100%;
    border-radius: 50%;
}
.employee_card .employee_id {
    font-size: 15px;
    font-weight: 700;
}

.dark-theme {
    background-color: black;
    color: white;
  }
  .dark-theme .employee_id{
    color:#fff;
  }
   .dark-theme .main_container .employee_card {
    background:#000;
    color:#fff;
   }
    .dark-theme .main_container .employee_card h4{
      color:#fff;
    }
     .dark-theme .theme {
       color:#fff;
     }
     .dark-theme .main_container h2{
       color:#fff;
     }
  .light-theme {
    background-color: white;
    color: black;
  }
  .slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
.switch {
  position: relative;
  display: inline-block;
  width:35px;
  height: 15px;
}

.switch input { 
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 3px;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
  box-shadow:0px 0px 10px #ccc;
}

.slider:before {
  position: absolute;
  content: "";
  height: 20px;
  width: 20px;
  left:-9px;
  bottom: -2px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
  box-shadow:0px 0px 10px #ccc
  }

input:checked + .slider {
  background-color: #800080;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
.dark.round {
  border-radius: 34px;
}

.dark.round:before {
  border-radius: 50%;
}
input:checked + .dark {
  background-color: #000;
}

input:focus + .dark {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .dark:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
.dark {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 3px;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
  box-shadow:0px 0px 10px #ccc;
}

.dark:before {
  position: absolute;
  content: "";
  height: 20px;
  width: 20px;
  left:-9px;
  bottom: -2px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
  box-shadow:0px 0px 10px #ccc
  }
.toggle-switch{
  position: relative;
  top: 14px;
  left: -10px;
}
.save-edit-btn{
    background: #800080;
    color: #fff;
    padding: 8px 40px;
    border-radius: 20px;
    border: none;
    outline:none;
}
.view-btn{
  background: #800080;
    color: #fff;
    padding: 8px 40px;
    border-radius: 10px;
    border: none;
    outline:none;
}
@media(max-width:768px){
  .main_container {
    height:auto;
  }
  .main_container .flex-direction{
    flex-direction:column;
  }
  .main_container .employee_card{
    width:100%;
    margin-top: 10px;
  }
}
@media(max-width:450px){
  .side_bar{
    display:none;
  }
  .main_container{
    margin-left:0px;
    width:100%;
  }
  .main_container h2{
    font-size:20px;
  }
  .employee_card h4 {
    font-size:14px;
  }
}
</style>
