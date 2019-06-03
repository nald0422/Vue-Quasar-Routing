<template>
  <div class="q-pa-md" style="max-width: 400px">
    <q-form class="q-gutter-md">
      <q-input
        type="text"
        v-model="input.username"
        label="Username"
        hint="Registered User ID"
        stack-label
        lazy-rules
      />

      <q-input
        type="password"
        v-model="input.password"
        label="Your Password"
        stack-label
        lazy-rules
      />

      <q-toggle v-model="accept" label="I accept the license and terms"/>

      <div>
        <q-btn label="Submit" type="submit" color="primary" @click="prompt = true"/>
        <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm"/>
      </div>
    </q-form>
    <div class="col-6">
      <q-dialog v-model="prompt" persistent>
        <q-card style="min-width: 400px" class="q-gutter-md">
          <form @submit.prevent="simulateSubmit" class="q-pa-md">
            <q-card-section>
              <q-field label="Username" stack-label>
                <template v-slot:control>
                  <div class="self-center full-width no-outline" tabindex="0">{{ input.username }}</div>
                </template>
              </q-field>
            </q-card-section>
            <q-card-section>
              <q-field label="Password" stack-label>
                <template v-slot:control>
                  <div class="self-center full-width no-outline" tabindex="0">{{ input.password }}</div>
                </template>
              </q-field>
            </q-card-section>
            <q-card-actions align="right" class="text-primary">
              <q-btn
                type="button"
                class="glossy"
                color="primary"
                label="Confirm"
                @click="$router.replace('/home')"
              ></q-btn>
            </q-card-actions>
            <!-- <p>
              <router-link to="/home">Proceed</router-link>
            </p>-->
          </form>
        </q-card>
      </q-dialog>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      accept: "",
      prompt: false,
      dense: false,

      input: {
        username: "",
        password: ""
      }
    };
  },

  method: {
    onDisplay() {
      console.log("test");
    },

    onClear() {}
  }
};
</script>

<style>
</style>
