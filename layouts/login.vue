<template>
    <div class="float-right mt-2 mr-5">
        <form class="form-signin" netlify @submit.prevent="login">
            <h2 class="form-signin-heading"><b> Start your journey With <span class="text-orange">Edukatrip</span></b></h2>
            <!-- <a class="btn btn-primary btn-google" href="#"><span class="float-left ml-2 mt-1 fa fa-google"></span> Login with Google</a><br>
            <a class="btn btn-primary btn-facebook" href="#"><span class="float-left ml-2 mt-1 fa fa-facebook"></span> Login with Facebook</a> -->
            <br><br>
            
            <section v-if="isError">
                <div class="alert alert-danger" v-for="(error, index) in errorMsg" :key="index">
                     {{ error }}
                </div>
            </section>
            <label><b>Username</b></label><br>
            <input type="text" v-model="username" class="form-control" placeholder="Email Address" required="" autofocus="" /><br>
            <label><b>Password</b></label><br>
            <input type="password" class="form-control" v-model="password" placeholder="Password" required="" />
            <a class="forgot" href="#">Forgot Password ?</a>
            <button class="btn btn-lg btn-primary btn-block" type="submit">Login</button> <br>
            <span>Don't Have Account ?&nbsp; </span> <router-link to="/register" tag="a">Register</router-link>
        </form>
    </div>
</template>




<style scoped>
    .forgot {
        float: right;
        text-align: right;
        top: 0px;
        color: grey;
        padding-bottom: 20px;
    }

    .text-grey {
        color: grey;
    }

    .wrapper {
        float: right;
        padding: 10px;
        margin-right: 20px;
    }

    .btn-google {
        color: black;
        background: white;
        width: 320px;
        border: 1px solid rgba(0, 0, 0, 0.2);
        margin-bottom: 10px;
    }

    .btn-google:hover {
        color: white;
        background: rgb(0, 0, 0);
        width: 320px;
        border: 1px solid rgba(0, 0, 0, 0.2);
        margin-bottom: 10px;
    }

    .btn-facebook {
        color: white;
        background: rgba(0, 77, 221, 0.897);
        width: 320px;
        margin-bottom: 10px;
        transition: 0.1s ease-in-out;
    }

    .btn-facebook:hover {
        background: white;
        color: rgba(0, 77, 221, 0.897)
    }

    .form-signin {
        border-radius: 20px;
        max-width: 450px;
        padding: 60px;
        margin: 0px auto;
        background-color: #fff;
        border: 1px solid rgba(0, 0, 0, 0.1);
    }

    .form-signin-heading,
    .checkbox {
        margin-bottom: 30px;
        font-size: 25px;
    }

    .checkbox {
        font-weight: normal;
    }

    .form-control {
        width: 320px;
        position: relative;
        font-size: 14px;
        height: auto;
        padding: 5px;
    }

    input[type="text"] {
        margin-bottom: -1px;
        border-bottom-left-radius: 0;
        border-bottom-right-radius: 0;
    }

    input[type="password"] {
        margin-bottom: 20px;
        border-top-left-radius: 0;
        border-top-right-radius: 0;
    }

    .content {
        top: -280px;
        position: relative;
        z-index: 1;
    }

    .trip-section {

        margin-left: 100px;
        padding: 10px;
    }

    .rating {
        padding-top: 5%;
        padding-left: 3%;
    }

    .price {
        padding-top: 70px;
        padding-left: 30%;
    }

    .desc-tour {
        padding-right: 10px;
        font-size: 15px;
        padding-top: 20px;
        padding-left: 20px;
    }

    .preview-tour {
        border-left: 1px solid gray;
    }

    .date-from {
        margin: 12px;
        padding: 12px;
        font-size: 12px;
        border-radius: 5px;
        border: 1px solid grey;
    }

    .radio-btn {
        border: 1px solid darkblue;
    }

    /* .has-search{
   
  } */
    .has-search .form-control {
        padding-left: 2.375rem;
        margin: -5px;
        box-shadow: 3px 4px 34px 6px rgba(0, 0, 0, 0.33);

    }

    .has-search .form-control-feedback {
        position: absolute;
        z-index: 2;
        display: block;
        width: 2.375rem;
        height: 2.375rem;
        line-height: 2.375rem;
        text-align: center;
        pointer-events: none;
        color: #aaa;
    }

    .btn-tour {
        width: 150px;
    }

    .btn-show {
        margin-left: 30px;
        float: right;
        text-align: right;
        position: inherit;
        font-size: 14px;
    }

    .bg-white {
        background: whitesmoke !important;
    }

    .box-footer {
        background: #304ffe;
        height: 140px;
    }

    .price-range {
        width: 125px;
        border: 1px solid grey;
        border-radius: 20px;
        font-size: 14px;
        text-align: left;
        padding: 6px;

    }

    .date-range {
        border: 0px;
        width: 110px;
    }

    .text-white {
        color: white;
    }

    .text-orange {
        color: #ffd800;
    }
</style>

<script>
import axios from "axios";
import { mapState } from "vuex";

export default {
    data() {
        return {
            username: '',
            password: '',
            isError: false,
            errorMsg: [],
            error: ''
        }
    },
    computed: {
        ...mapState('auth',['loggedIn','user'])
    },
    methods: {
        async login() {
            var { username, password } = this
            
            try {
                const response = await this.$auth.loginWith('local',{
                    data: {
                        username, password
                    }
                })



                this.isError=false
                this.errorMsg=[]

                console.log(this.$store)

                
            } catch (error) {
                
                const { data, status } = error.response
                if ( status == 401 )
                {
                    this.isError = true
                    this.errorMsg = []
                    this.errorMsg.push(data.message)
                }
            }
        }
    },
    head: {
        bodyAttrs: {
            class: 'auth-body'
        }
    }
}
</script>