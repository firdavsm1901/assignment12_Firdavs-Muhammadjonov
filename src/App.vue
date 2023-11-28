  <template>
    <div class="user-profile">

      <div class="user-details">
        <h2>Welcome, {{ user.name }}!</h2>
        <img :src="user.profilePicture" alt="Profile Picture" @click="changeProfilePicture" />
        <p>Email: {{ user.email }}</p>

        <p v-if="user.isAdmin">Hello Admin!</p>
        <p v-else>Hello User!</p>

        <form @submit.prevent="updateProfile">
          <input type="text" v-model="user.name" />
          <input type="email" v-model="user.email" />
          <input type="date" v-model="user.birthdate" />
          <button type="submit">Update Profile</button>
        </form>

        <p v-if="user.birthdate">Age: {{ userAge }}</p>
      </div>
    </div>
  </template>

  <script>
  export default {
    data() {
      return {
        user: {
          name: 'John Doe',
          email: 'john@example.com',
          profilePicture: 'https://cdn-icons-png.flaticon.com/512/4139/4139981.png',
          birthdate: '', 
          isAdmin: true 
        }
      };
    },
    computed: {
      userAge() {
        if (this.user.birthdate) {
          const today = new Date();
          const dob = new Date(this.user.birthdate);
          const ageDate = new Date(today - dob);
          return Math.abs(ageDate.getUTCFullYear() - 1970);
        }
        return null;
      }
    },
    methods: {

      changeProfilePicture() {
      },

      updateProfile() {
      }
    },
    watch: {

      user: {
        handler(newValue, oldValue) {
          console.log('User details modified:', newValue);
        },
        deep: true 
      }
    },
    mounted() {
    }
  };
  </script>

  <style>


  body {
    background: url("https://s3.ap-southeast-1.amazonaws.com/arrowhitech.com/wp-content/uploads/2020/08/04080334/hero-1.png");
  }
  .user-profile {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    background-color: #f9f9f9;
  }

  .user-details {
    text-align: center;
    padding: 20px;
  }

  h2 {
    font-size: 24px;
    margin-bottom: 10px;
    word-wrap: break-word;
  }

  img {
    width: 100px;
    height: 100px;
    /* border-radius: 50%; */
    cursor: pointer;
  }

  form {
    margin-top: 20px;
  }

  input[type="text"],
  input[type="email"],
  input[type="date"],
  button {
    display: block;
    width: calc(100% - 20px);
    margin: 10px auto;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  button {
    background-color: #3498db;
    color: #fff;
    cursor: pointer;
  }

  button:hover {
    background-color: #2980b9;
  }

  p {
    margin-bottom: 10px;
  }

  </style>
  