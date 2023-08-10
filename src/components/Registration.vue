<template>
    <div>
        <div v-if="isSigned" class="main">
            <h1>Registration Form</h1>
            <form class="form" @submit.prevent="handleSubmit">
                <div class="form-group">
                    <label for="name">Name:</label>
                    <input v-model="formData.name" type="text" id="name" name="name" required />
                </div>
                <div class="form-group">
                    <label for="email">Email:</label>
                    <input v-model="formData.email" type="email" id="email" name="email" required />
                </div>
                <div class="form-group">
                    <label for="password">Password:</label>
                    <input v-model="formData.password" type="password" id="password" name="password" required />
                </div>
                <button type="submit">Submit</button>
            </form>
        </div>
        <div v-else class="main">
            <h1>Submit your OTP to verify your email</h1>
            <form class="form" @submit.prevent="handleSubmit1">
                <div class="form-group">
                    <label for="otp">OTP:</label>
                    <input v-model="formData.otp" type="text" id="otp" name="otp" required />
                </div>
                <button type="submit">Submit</button>
            </form>
        </div>
    </div>
</template>

<style>

.container {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  /* margin-top: 20px; */
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}
.main{
    display: flex;
    flex-direction: column;
    height: 100vh;
    justify-content: center;
    align-items: center;
}
.form {display: flex;
  flex-direction: column;
  width: 380px;
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
}

label {
  font-size: 16px;
  margin-bottom: 5px;
}

input,
textarea {
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 15px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #0056b3;
}

</style>

<script setup>
import { ref } from 'vue';
import { Auth } from 'aws-amplify';
import { useRouter } from 'vue-router';

const formData = ref({
    name: '',
    email: '',
    password: '',
    otp: '',
});
const router = useRouter();
const isSigned = ref(true);
const email = ref("");

const handleChange = (e) => {
    const { name, value } = e.target;
    formData[name] = value;
};

const handleSubmit = async (e) => {
    e.preventDefault();
    console.log(formData.value);
    // Perform form validation here
    try {
        const user = await Auth.signUp({
            username: formData.value.email,
            password: formData.value.password,
            attributes: {
                name: formData.name,
            },
        });
        //   setisSigned(!isSigned);
        if (user) {
            window.alert("Account created Successfully");
            console.log("registration", user)
            console.log(isSigned.value);
            email.value = formData.value.email
            isSigned.value = false;
        }
    } catch (error) {
        const err = {
            status: true,
            message: error,
        };
        //   setErrorMsg(err);
        console.log("error", error)
    }
};

const handleSubmit1 = async (e) => {
    e.preventDefault();
    console.log(formData);
    try {
        const user = await Auth.confirmSignUp(email.value, formData.value.otp);
        if (user) {
            window.alert("Account verified Successfully");
            router.push("/sign-in");
            console.log("sucuessfully logged in", user);
        }
    } catch (error) {
        const obj = {
            status: true,
            message: error,
        };
        console.log("error", error);
        // setErrorMsg(obj);
    }
};
</script>
