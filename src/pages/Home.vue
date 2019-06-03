<template>
  <div class="q-pa-md">
    <q-layout view="lHh lpr lFf" container style="height: 400px" class="shadow-2 rounded-borders">
      <q-header elevated>
        <q-bar>
          <q-icon name="laptop_chromebook"/>
          <div>Google Chrome</div>

          <q-space/>

          <q-btn dense flat icon="minimize"/>
          <q-btn dense flat icon="crop_square"/>
          <q-btn dense flat icon="close"/>
        </q-bar>

        <div class="q-pa-sm q-pl-md row items-center">
          <div class="cursor-pointer non-selectable">
            File
            <q-menu>
              <q-list dense style="min-width: 100px">
                <q-item clickable v-close-popup>
                  <q-item-section @click="prompt = true">Login</q-item-section>
                </q-item>
                <q-item clickable v-close-popup>
                  <q-item-section>New</q-item-section>
                </q-item>

                <q-separator/>

                <q-item clickable>
                  <q-item-section>Preferences</q-item-section>
                  <q-item-section side>
                    <q-icon name="keyboard_arrow_right"/>
                  </q-item-section>

                  <q-menu anchor="top right" self="top left">
                    <q-list>
                      <q-item v-for="n in 3" :key="n" dense clickable>
                        <q-item-section>Submenu Label</q-item-section>
                        <q-item-section side>
                          <q-icon name="keyboard_arrow_right"/>
                        </q-item-section>
                        <q-menu auto-close anchor="top right" self="top left">
                          <q-list>
                            <q-item v-for="n in 3" :key="n" dense clickable>
                              <q-item-section>3rd level Label</q-item-section>
                            </q-item>
                          </q-list>
                        </q-menu>
                      </q-item>
                    </q-list>
                  </q-menu>
                </q-item>

                <q-separator/>

                <q-item clickable v-close-popup>
                  <q-item-section @click="$router.replace('/login')">Quit</q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </div>

          <div class="q-ml-md cursor-pointer non-selectable">
            Edit
            <q-menu auto-close>
              <q-list dense style="min-width: 100px">
                <q-item clickable>
                  <q-item-section>Cut</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Copy</q-item-section>
                </q-item>
                <q-item clickable>
                  <q-item-section>Paste</q-item-section>
                </q-item>
                <q-separator/>
                <q-item clickable>
                  <q-item-section>Select All</q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </div>
        </div>
      </q-header>
      <q-page-container>
        <q-page class="q-pa-md">
          <div class="q-gutter-sm">
            <q-table
              title="Treats"
              :data="data"
              :columns="columns"
              row-key="id"
              :filter="filter"
              :loading="loading"
            >
              <template v-slot:top>
                <q-btn
                  flat
                  dense
                  color="primary"
                  :disable="loading"
                  label="Add row"
                  @click="addRow"
                />
                <q-btn
                  class="on-right"
                  flat
                  dense
                  color="primary"
                  :disable="loading"
                  label="Remove row"
                  @click="removeRow"
                />
                <q-space/>
                <q-input borderless dense debounce="300" color="primary" v-model="filter">
                  <template v-slot:append>
                    <q-icon name="search"/>
                  </template>
                </q-input>
              </template>
            </q-table>
          </div>
        </q-page>
      </q-page-container>
    </q-layout>
    <div class="col-6">
      <q-dialog v-model="prompt" persistent>
        <q-card style="min-width: 400px" class="q-gutter-md">
          <form @submit.prevent="simulateSubmit" class="q-pa-md">
            <q-card-section>
              <q-input
              filled
              required
              v-model="input.user"
              label="User"
              :dense="dense"
              submitting: false
              @keyup.enter="prompt = false"
              />
            </q-card-section>
            <q-card-section>
              <q-input
                filled
                required
                v-model="input.password"
                label="Password"
                :dense="dense"
                @keyup.enter="prompt = false"
              />
            </q-card-section>
            <q-card-actions align="right" class="text-primary">
              <q-btn
                type="button"
                @click="executeView()"
                class="glossy"
                color="red-8"
                label="Cancel"
              ></q-btn>
              <q-btn
                type="submit"
                :loading="submitting"
                @click="executeCreate()"
                class="glossy"
                color="green-8"
                label="Login"
              >
                <template v-slot:loading>
                  <q-spinner-facebook/>
                </template>
              </q-btn>
            </q-card-actions>
          </form>
        </q-card>
      </q-dialog>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      test: "",
      alert: false,
      confirm: false,
      prompt: false,
      loading: false,
      filter: "",
      rowCount: 2,

      input: {
        user: "",
        password: ""
      },

      AuthorizationModel: {
        UserModel: {
          person_id: "",
          user_id: "",
          company_name: "",
          job_designation: ""
        },
        token: ""
      },

      text: "",
      ph: "",

      submitting: false,
      dense: false,

      columns: [
        {
          name: "desc",
          required: true,
          label: "Dessert (100g serving)",
          align: "left",
          field: row => row.name,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: "calories",
          align: "center",
          label: "Calories",
          field: "calories",
          sortable: true
        },
        { name: "fat", label: "Fat (g)", field: "fat", sortable: true },
        { name: "carbs", label: "Carbs (g)", field: "carbs" },
        { name: "protein", label: "Protein (g)", field: "protein" },
        { name: "sodium", label: "Sodium (mg)", field: "sodium" },
        {
          name: "calcium",
          label: "Calcium (%)",
          field: "calcium",
          sortable: true,
          sort: (a, b) => parseInt(a, 10) - parseInt(b, 10)
        },
        {
          name: "iron",
          label: "Iron (%)",
          field: "iron",
          sortable: true,
          sort: (a, b) => parseInt(a, 10) - parseInt(b, 10)
        }
      ],

      data: [
        {
          id: 1,
          name: "Frozen Yogurt",
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          sodium: 87,
          calcium: "14%",
          iron: "1%"
        },
        {
          id: 2,
          name: "Ice cream sandwich",
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          sodium: 129,
          calcium: "8%",
          iron: "1%"
        },
        {
          id: 3,
          name: "Eclair",
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          sodium: 337,
          calcium: "6%",
          iron: "7%"
        },
        {
          id: 4,
          name: "Cupcake",
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          sodium: 413,
          calcium: "3%",
          iron: "8%"
        },
        {
          id: 5,
          name: "Gingerbread",
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          sodium: 327,
          calcium: "7%",
          iron: "16%"
        },
        {
          id: 6,
          name: "Jelly bean",
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          sodium: 50,
          calcium: "0%",
          iron: "0%"
        },
        {
          id: 7,
          name: "Lollipop",
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          sodium: 38,
          calcium: "0%",
          iron: "2%"
        },
        {
          id: 8,
          name: "Honeycomb",
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          sodium: 562,
          calcium: "0%",
          iron: "45%"
        },
        {
          id: 9,
          name: "Donut",
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          sodium: 326,
          calcium: "2%",
          iron: "22%"
        },
        {
          id: 10,
          name: "KitKat",
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          sodium: 54,
          calcium: "12%",
          iron: "6%"
        }
      ],
      original: [
        {
          name: "Frozen Yogurt",
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          sodium: 87,
          calcium: "14%",
          iron: "1%"
        },
        {
          name: "Ice cream sandwich",
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          sodium: 129,
          calcium: "8%",
          iron: "1%"
        },
        {
          name: "Eclair",
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          sodium: 337,
          calcium: "6%",
          iron: "7%"
        },
        {
          name: "Cupcake",
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          sodium: 413,
          calcium: "3%",
          iron: "8%"
        },
        {
          name: "Gingerbread",
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          sodium: 327,
          calcium: "7%",
          iron: "16%"
        },
        {
          name: "Jelly bean",
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          sodium: 50,
          calcium: "0%",
          iron: "0%"
        },
        {
          name: "Lollipop",
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          sodium: 38,
          calcium: "0%",
          iron: "2%"
        },
        {
          name: "Honeycomb",
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          sodium: 562,
          calcium: "0%",
          iron: "45%"
        },
        {
          name: "Donut",
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          sodium: 326,
          calcium: "2%",
          iron: "22%"
        },
        {
          name: "KitKat",
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          sodium: 54,
          calcium: "12%",
          iron: "6%"
        }
      ]
    };
  },

  methods: {
    simulateSubmit() {
      this.submitting = true;

      // Simulating a delay here.
      // When we are done, we reset "submitting"
      // Boolean to false to restore the
      // initial state.
      setTimeout(() => {
        // delay simulated, we are done,
        // now restoring submit to its initial state
        this.submitting = false;
      }, 3000);
    },

    login() {},

    // emulate fetching data from server
    addRow() {
      this.loading = true;
      setTimeout(() => {
        const index = Math.floor(Math.random() * (this.data.length + 1)),
          row = this.original[Math.floor(Math.random() * this.original.length)];
        if (this.data.length === 0) {
          this.rowCount = 0;
        }
        row.id = ++this.rowCount;
        const addRow = { ...row }; // extend({}, row, { name: `${row.name} (${row.__count})` })
        this.data = [
          ...this.data.slice(0, index),
          addRow,
          ...this.data.slice(index)
        ];
        this.loading = false;
      }, 500);
    },

    removeRow() {
      this.loading = true;
      setTimeout(() => {
        const index = Math.floor(Math.random() * this.data.length);
        this.data = [
          ...this.data.slice(0, index),
          ...this.data.slice(index + 1)
        ];
        this.loading = false;
      }, 500);
    },

    executeView() {
      var url = "http://localhost:8080/Rest/webapi/staffs";
      axios
        .get(url, {
          method: "GET",
          headers: {
            "Content-type": "application/json; charset=UTF-8",
            "Access-Control-Allow-Origin": "*"
            }
        })
        .then(response => {
          console.log(JSON.stringify(response));
          // this.data = response["data"];
        })
        .catch(error => console.log(error));
    },

    executeCreate() {
      this.loading = true;
      var url = "http://192.168.2.9:3000/user/login ";
      console.log(url);
      // console.log(this.details.title)
      // console.log(this.details.body)
      // console.log(this.details.userId)
      axios
        .post(url, {
          method: "POST",
          body: JSON.stringify({
            username: "ZG9uYWxk",
            password: "Zjk3YThkZWZkMDI5N2YxNDBiNjU0N2FkNTcxNGVkZWE="
          }),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>

<style scoped>
.q-card__section {
  padding-left: 0px !important;
}
</style>
