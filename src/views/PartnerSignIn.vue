<template>
  <!-- Sign in-->
  <div class="vh-100" id="partner-signin">
    <div class="container-fluid">
      <div style="background-color: #f8f9fa" class="row">
        <div class="col-sm-6 px-0 d-none d-sm-block">
          <img
            src="../assets/img/register2.jpg"
            alt="Login image"
            class="w-100 vh-100 img-fluid"
            style="object-fit: cover; object-position: center"
          />
        </div>

        <div class="col-sm-6 text-black">
          <div
            class="d-flex align-items-center h-custom-2 px-5 ms-xl-4 mt-5 pt-5 pt-xl-0 mt-xl-n5"
          >
            <form class="align-items-center" style="width: 23rem">
              <div class="heading my-4">
                <h4 class="fw-bold">
                  Sign in as an<span style="color: #0d6efd"> iCargo</span>
                  <span style="color: #ffc800"> Partner</span>
                </h4>
                <h5 style="font-size: 15px" class="mt-3">
                  Enter your details below
                </h5>
                <p v-if="errMsg">{{ errMsg }}</p>
              </div>

              <div class="row mb-2">
                <div class="col">
                  <div class="form-outline mb-4">
                    <label class="form-label" for="form3Example3">Email</label>
                    <input
                      style="width: 20rem"
                      type="email"
                      v-model="email"
                      id="form3Example3"
                      class="form-control form-control-sm"
                    />
                  </div>

                  <div class="form-outline mb-4">
                    <label class="form-label" for="formTextExample2"
                      >Password</label
                    >
                    <input
                      style="width: 20rem"
                      type="password"
                      v-model="password"
                      id="form3Example3"
                      class="form-control form-control-sm"
                    />
                  </div>

                  <div class="pt-1 mb-4 d-flex">
                    <a class="text-body text-right"
                      ><router-link
                        to="/forgot-password"
                        class="text-decoration-none"
                        >Forgot Password?</router-link
                      ></a
                    >
                  </div>

                  <div class="pt-1 mb-4 d-flex justify-content-center">
                    <div class="btn-group me-2 top-0">
                      <a
                        style="background-color: white"
                        class="btn btn-primary btn-sm btn-block text-dark shadow rounded border border-2"
                        role="button"
                        ><router-link to="/signin" class="text-decoration-none"
                          >Back</router-link
                        ></a
                      >
                    </div>

                    <div class="btn-group me-2 top-0">
                      <a
                        @click="signIn"
                        class="btn btn-primary btn-sm btn-block shadow rounded border border-2 text-decoration-none text-white"
                        role="button"
                        >Sign in</a
                      >
                    </div>
                  </div>

                  <div class="mt-4 me-3">
                    <div class="pt-1 mb-4 d-flex justify-content-center">
                      <h6 class="text-body fw-light mb-2 fs-6">Login with:</h6>
                    </div>

                    <div class="pt-1 mb-4 d-flex justify-content-center">
                      <div class="btn-group me-4 top-0">
                        <a
                          class="btn btn-secondary btn-sm btn-block shadow rounded border border-2"
                          role="button"
                          ><i class="fa-brands fa-facebook me-2"></i>Facbook</a
                        >
                      </div>

                      <div class="btn-group me- top-0">
                        <a
                          @click="signInWithGoogle"
                          class="btn btn-secondary btn-sm btn-block shadow rounded border border-2"
                          role="button"
                          ><i class="fa-brands fa-google me-2"></i>Google</a
                        >
                      </div>
                    </div>
                  </div>
                  <div class="pt-1 mb-4 d-flex justify-content-center">
                    <p class="me-2">Don't have an account yet?</p>
                    <router-link
                      class="nav text-warning fw-bold text-decoration-none"
                      to="/partner-signup"
                      @click.prevent="register"
                      >Sign up</router-link
                    >
                  </div>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import {
  getAuth,
  signInWithEmailAndPassword,
  GoogleAuthProvider,
  signInWithPopup
} from 'firebase/auth';
import { useRouter } from 'vue-router';
const email = ref('');
const password = ref('');
const errMsg = ref();

const router = useRouter();
const signIn = () => {
  const auth = getAuth();
  signInWithEmailAndPassword(getAuth(), email.value, password.value)
    .then((data) => {
      alert('successfully registered');
      console.log(auth.currentUser);
      router.push('/dashboard');
    })
    .catch((error) => {
      console.log(error.code);
      switch (error.code) {
        case 'auth/invalid-email':
          errMsg.value = 'Invalid email';
          break;
        case 'auth/user-not-found':
          errMsg.value = 'No account with that email was found';
          break;
        case 'auth/wrong-password':
          errMsg.value = 'Incorrect password';
          break;
        case 'auth/internal-error':
          errMsg.value = 'Incorrect password';
          break;
      }
    });
};
const signInWithGoogle = () => {
  const provider = new GoogleAuthProvider();
  signInWithPopup(getAuth(), provider)
    .then((result) => {
      console.log(result.user);
      router.push('/dashboard');
    })
    .catch((error) => {
      // handle error
    });
};
</script>

<style>
#partner-signin .form-outline .btn {
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  width: 250px;
  border-radius: 20px;
}

#partner-signin .pt-1 .link-warning {
  text-decoration: none;
}

#partner-signin .pt-1 .text-body {
  text-decoration: none;
}

@media (min-width: 576px) {
  .container-sm,
  .container {
    max-width: 540px;
  }
}
@media (min-width: 768px) {
  .container-md,
  .container-sm,
  .container {
    max-width: 720px;
  }
}
@media (min-width: 992px) {
  .container-lg,
  .container-md,
  .container-sm,
  .container {
    max-width: 960px;
  }
}
@media (min-width: 1200px) {
  .container-xl,
  .container-lg,
  .container-md,
  .container-sm,
  .container {
    max-width: 1140px;
  }
}
@media (min-width: 1400px) {
  .container-xxl,
  .container-xl,
  .container-lg,
  .container-md,
  .container-sm,
  .container {
    max-width: 1320px;
  }
}
</style>
