<template>
    <div class="page-header signup-page section-image">
        <div class="page-header-image" style="background-image: url('img/bg18.jpg')">
        </div>
        <div class="content">
            <div class="container">
                <div class="row">
                    <div class="col-md-4 ml-auto mr-auto">
                        <div class="info info-horizontal">
                            <div class="icon icon-primary">
                                <i class="now-ui-icons media-2_sound-wave"></i>
                            </div>
                            <div class="description">
                                <h5 class="info-title">Marketing</h5>
                                <p class="description">
                                    We've created the marketing campaign of the website. It was a very interesting collaboration.
                                </p>
                            </div>
                        </div>
                        <div class="info info-horizontal">
                            <div class="icon icon-primary">
                                <i class="now-ui-icons media-1_button-pause"></i>
                            </div>
                            <div class="description">
                                <h5 class="info-title">Fully Coded in HTML5</h5>
                                <p class="description">
                                    We've developed the website with HTML5 and CSS3. The client has access to the code using GitHub.
                                </p>
                            </div>
                        </div>
                        <div class="info info-horizontal">
                            <div class="icon icon-info">
                                <i class="now-ui-icons users_single-02"></i>
                            </div>
                            <div class="description">
                                <h5 class="info-title">Built Audience</h5>
                                <p class="description">
                                    There is also a Fully Customizable CMS Admin Dashboard for this product.
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-4 mr-auto">
                        <form @submit.prevent="validateBeforeSubmit" novalidate>
                            <div class="card card-signup">
                                <div class="card-body">
                                    <h4 class="card-title text-center">{{$t('auth.register')}}</h4>
                                    <div class="social text-center">
                                        <n-button round icon class="btn-twitter">
                                            <i class="fab fa-twitter"></i>
                                        </n-button>
                                        <n-button round icon class="btn-dribbble">
                                            <i class="fab fa-dribbble"></i>
                                        </n-button>
                                        <n-button round icon class="btn-facebook">
                                            <i class="fab fa-facebook"> </i>
                                        </n-button>
                                        <h5 class="card-description"> or be classical </h5>
                                    </div>
                                    <fg-input addon-left-icon="now-ui-icons users_circle-08" v-model="form.name" name="first_name" v-validate="'required'" :placeholder="$t('auth.firstName')" :class="{'has-danger': errors.has('first_name') }">
                                    </fg-input>

                                    <fg-input addon-left-icon="now-ui-icons text_caps-small" v-model="form.lastName" name="last_name" v-validate="'required'" :placeholder="$t('auth.lastName') " :class="{'has-danger': errors.has('last_name') }">
                                    </fg-input>

                                    <fg-input addon-left-icon="now-ui-icons ui-1_email-85" v-model="form.email" name="email" v-validate="'required|email'" :placeholder="$t('auth.email')" :class="{'has-danger': errors.has('email') }">
                                    </fg-input>

                                    <fg-input addon-left-icon="now-ui-icons ui-1_settings-gear-63" v-model="form.password" name="password" v-validate="'required|confirmed:password_confirmation'" type="password" :placeholder="$t('auth.password')" :class="{'has-danger': errors.has('password') }">
                                    </fg-input>

                                    <fg-input addon-left-icon="now-ui-icons ui-1_settings-gear-63" v-model="form.password_confirmation" ref="password_confirmation" name="password_confirmation" v-validate="'required'" type="password" :placeholder="$t('auth.password_confirmation')" :class="{'has-danger': errors.has('password_confirmation') }">
                                    </fg-input>

                                    <n-checkbox v-model="form.agree">
                                        I agree to the terms and
                                        <a href="#something">conditions</a>.
                                    </n-checkbox>
                                    <div class="card-footer text-center">
                                        <n-button type="primary" nativeType="submit" round size="lg"> {{$t('auth.start')}}</n-button>
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
<script>
import {
  Card,
  Button,
  FormGroupInput,
  Checkbox
} from "@/components/now-ui-kit";
export default {
  name: "signup-page",
  bodyClass: "signup-page",
  components: {
    Card,
    [Button.name]: Button,
    [Checkbox.name]: Checkbox,
    [FormGroupInput.name]: FormGroupInput
  },
  methods: {
    async register() {
      console.log("sending");
      await this.$axios.post("/auth/signup", this.form);

      this.notification(
        this.$t("auth.registerSuccess"),
        "success",
        "top",
        "center"
      );

      await this.$auth.login({
        data: {
          email: this.form.email,
          password: this.form.password
        }
      });

      this.notification(this.$t("auth.success"), "success", "top", "center");
    },
    validateBeforeSubmit() {
      this.$validator.validateAll().then(validated => {
        console.log(validated);
        if (validated) {
          this.register();
          return;
        }
        var err = this.errors.all().join("<br>");
        this.notification(err, "danger", "top", "center");
      });
    }
  },
  data() {
    return {
      form: {
        name: "",
        lastName: "",
        password: "",
        password_confirmation: "",
        email: "",
        agree: false
      }
    };
  }
};
</script>
<style>
</style>
