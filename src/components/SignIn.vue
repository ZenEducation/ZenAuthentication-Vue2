<template>
    <div class="container">
        <h1>Login here</h1>
        <form class="form" @submit.prevent="handleSubmit">
            <div class="form-group">
                <label for="email">Email:</label>
                <input v-model="formData.email" type="text" id="email" name="email" required />
                <label for="password">Password:</label>
                <input v-model="formData.password" type="password" id="password" name="password" required />
            </div>
            <button type="submit">Submit</button>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { Auth } from 'aws-amplify';
import awsExports from '../aws-exports';
import { useRouter } from 'vue-router';

Auth.configure(awsExports);

const formData = ref({
    email: '',
    password: '',
});
const router = useRouter();

const handleChange = (e) => {
    const { name, value } = e.target;
    formData[name] = value;
};

const handleSubmit = async () => {
    console.log(formData.value);
    try {
        const user = await Auth.signIn(formData.value.email, formData.value.password);
        if (user) {
            localStorage.setItem('authToken', user.signInUserSession.accessToken.jwtToken);
            localStorage.setItem('name', user.attributes.email);
            window.alert('Successfully logged In');
            console.log('logged in successfully', user);
            router.push('/profile');
        }
    } catch (error) {
        const obj = {
            status: true,
            message: error,
        };
        console.log('error', error);
        //  setErrorMsg(obj);
    }
};
</script>
