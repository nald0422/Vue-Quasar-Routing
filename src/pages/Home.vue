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
                  <q-item-section @click="$router.replace('/login')">Logout</q-item-section>
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
              title="Person"
              :data="data"
              :columns="columnsPerson"
              row-key="id"
              :filter="filter"
              :loading="loading"
            >
              <template v-slot:top>
                <q-btn
                  flat
                  dense
                  color="secondary"
                  :disable="loading"
                  label="Add Person"
                  @click="createDialog=true"
                />
                <q-btn
                  class="on-right"
                  flat
                  dense
                  color="grey-10"
                  :disable="loading"
                  label="Load"
                  @click="executeView"
                >
                  <q-icon name="refresh"></q-icon>
                </q-btn>
                <q-space/>
                <q-input borderless dense debounce="300" color="primary" v-model="filter">
                  <template v-slot:append>
                    <q-icon name="search"/>
                  </template>
                </q-input>
              </template>
              <template v-slot:body="props">
                <q-tr :props="props">
                  <q-td key="personId" :props="props">{{ props.row.personId }}</q-td>
                  <q-td key="personName" :props="props">{{ props.row.personName }}</q-td>
                  <q-td key="personAge" :props="props">{{ props.row.personAge }}</q-td>
                  <q-td key="action" :props="props" class="q-gutter-xs" style="min-width: 140px;">
                    <q-btn
                      color="info"
                      class="btn-action"
                      icon="visibility"
                      size="sm"
                      @click="onView(props.row)"
                    />
                  </q-td>
                </q-tr>
              </template>
            </q-table>
          </div>
        </q-page>
      </q-page-container>
    </q-layout>

    <q-dialog v-model="prompt" persistent>
      <q-card style="min-width: 450px" class="q-gutter-">
        <form @submit.prevent="simulateSubmit">
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
            <q-btn type="button" class="glossy" color="red-8" label="Cancel"></q-btn>
            <q-btn
              type="submit"
              :loading="submitting"
              @click="prompt = false"
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

    <q-dialog
      v-model="createChildrenDialog"
      persistent
      transition-show="flip-down"
      transition-hide="flip-up"
    >
      <q-card style="width: 500px; max-width: 80vw;">
        <q-card-section class="bg-blue">
          <div class="text-h6 text-white">Create Children</div>
        </q-card-section>
        <q-card-section style="padding-top: 16px;">
          <q-input v-model="Person.children.childId" filled type="text" hint="Child ID"/>
          <br>
          <q-input v-model="Person.children.childName" filled autogrow hint="Name"/>
          <br>
          <q-input v-model="Person.children.personAge" filled autogrow hint="Age"/>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat :loading="loading" @click="executeCreateChildren()">
            Create
            <template v-slot:loading>
              <q-spinner-facebook class="on-left"/>
            </template>
          </q-btn>
          <q-btn flat label="Close" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>

    <q-dialog
      v-model="createDialog"
      persistent
      transition-show="flip-down"
      transition-hide="flip-up"
    >
      <q-card style="width: 500px; max-width: 80vw;">
        <q-card-section class="bg-blue">
          <div class="text-h6 text-white">Create Person</div>
        </q-card-section>
        <q-card-section style="padding-top: 16px;">
          <q-input v-model="Person.personId" filled type="text" hint="Person ID"/>
          <br>
          <q-input v-model="Person.personName" filled autogrow hint="Name"/>
          <br>
          <q-input v-model="Person.personAge" filled autogrow hint="Age"/>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat :loading="loading" @click="executeCreatePerson()">
            Create
            <template v-slot:loading>
              <q-spinner-facebook class="on-left"/>
            </template>
          </q-btn>
          <q-btn flat label="Close" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>

    <q-dialog v-model="viewDialog" persistent>
      <q-card style="min-width: 1000px" class="q-gutter-md">
        <form @submit.prevent="simulateSubmit" class="q-pa-md">
          <div class="row">
            <div class="col-sm-4" style="border-right: 1px solid #333;">
              <span class="border border-right-0"></span>
              <q-card-section style="width: 250px; max-width: 40vw;">
                <q-field label="Person Id" stack-label v-model="this.Person.personId">
                  <template v-slot:control>
                    <div
                      class="self-center full-width no-outline"
                      tabindex="0"
                    >{{ Person.personId }}</div>
                  </template>
                </q-field>
                <!-- <q-card-section style="width: 250px; max-width: 40vw;"> -->
                <q-field label="Person Name" stack-label v-model="this.Person.personName">
                  <template v-slot:control>
                    <div
                      class="self-center full-width no-outline"
                      tabindex="0"
                    >{{ Person.personName }}</div>
                  </template>
                </q-field>
                <q-field label="Person Age" stack-label v-model="this.Person.personAge">
                  <template v-slot:control>
                    <div
                      class="self-center full-width no-outline"
                      tabindex="0"
                    >{{ Person.personAge }}</div>
                  </template>
                </q-field>
              </q-card-section>
            </div>

            <!-- <p>
              <router-link to="/home">Proceed</router-link>
            </p>-->
            <div class="col-sm-8">
              <template>
                <div class="q-pa-md">
                  <q-table
                    class="my-sticky-column-table"
                    title="Children"
                    :data="data"
                    :columns="columnsChild"
                    row-key="name"
                  >
                    <template v-slot:top>
                      <q-btn
                        flat
                        dense
                        color="primary"
                        :disable="loading"
                        label="Add Child"
                        @click="createChildrenDialog=true"
                      >
                        <q-icon name="add"></q-icon>
                      </q-btn>
                      <q-space/>
                      <q-input borderless dense debounce="300" color="primary" v-model="filter">
                        <template v-slot:append>
                          <q-icon name="search"/>
                        </template>
                      </q-input>
                    </template>
                  </q-table>
                </div>
              </template>
              <q-card-actions align="right" class="text-primary">
                <q-btn
                  type="button"
                  class="glossy"
                  color="positive"
                  label="Save"
                  disabled
                  @click="viewDialog=false"
                ></q-btn>
                <q-btn
                  type="button"
                  class="glossy"
                  color="primary"
                  label="Done"
                  @click="viewDialog=false"
                ></q-btn>
              </q-card-actions>
            </div>
          </div>
        </form>
      </q-card>
    </q-dialog>
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
      createDialog: false,
      createChildrenDialog: false,
      viewDialog: false,
      showing: false,
      loading: false,
      filter: "",
      rowCount: 2,
      position: "",
      dense: true,
      disable: false,

      input: {
        user: "",
        password: ""
      },

      Person: {
        personId: "",
        personName: "",
        personAge: "",
        children: [
          {
            childId: "",
            childName: "",
            childAge: ""
          }
        ]
      },

      children: {
        childId: "",
        childName: "",
        childAge: ""
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

      columnsPerson: [
        {
          name: "personId",
          align: "left",
          label: "Person Id",
          field: "personId",
          sortable: true
        },
        {
          name: "personName",
          align: "left",
          label: "Name",
          field: "personName",
          sortable: true
        },
        {
          name: "personAge",
          align: "left",
          label: "Age",
          field: "personAge",
          sortable: true
        },
        { name: "action", align: "center", label: "Action" }
      ],

      columnsChild: [
        {
          name: "childId",
          align: "left",
          label: "Child Id",
          field: "childId",
          sortable: true
        },
        {
          name: "childName",
          align: "left",
          label: "Name",
          field: "childName",
          sortable: true
        },
        {
          name: "childAge",
          align: "left",
          label: "Age",
          field: "childAge",
          sortable: true
        }
      ],

      data: []
    };
  },

  methods: {
    onView(row) {
      this.open("right");
      this.Person.personId = row.personId;
      this.Person.personName = row.personName;
      this.Person.personAge = row.personAge;
    },
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
      var url = "http://localhost:8080/Persons";
      axios
        .get(url, {
          method: "GET",
          headers: {
            "Content-type": "application/json; charset=UTF-8",
            "Access-Control-Allow-Origin": "*"
          },

          personId: this.Person.personId,
          personName: this.Person.personName,
          personAge: this.Person.personAge
        })
        .then(response => {
          console.log(JSON.stringify(response));
          this.data = response["data"];
        })
        .catch(error => console.log(error));
    },

    executeCreatePerson() {
      this.loading = true;
      var url = "http://localhost:8080/Create";
      const data = {
        personId: this.Person.personId,
        personName: this.Person.personName,
        personAge: this.Person.personAge
      };
      console.log(url);
      console.log(this.Person.personId);
      console.log(this.Person.personName);
      console.log(this.Person.personAge);
      axios
        .post(url, data)
        .then(response => {
          console.log(response);
          this.loading = false;
          this.createDialog = false;
        })
        .then(response => {
          this.alertSuccess = true;
          this.executeView();
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
          this.alertFailure = true;
        });
    },

    executeCreateChildren() {
      this.loading = true;
      var url = "http://localhost:8080/setChild/"+this.person_id;
      const data = {
        childId: this.children.childId,
        childName: this.children.childName,
        childAge: this.children.childAge
      };
      console.log(url);
      console.log(this.children.childId);
      console.log(this.children.childName);
      console.log(this.children.childAge);
      axios
        .post(url, data)
        .then(response => {
          console.log(response);
          this.loading = false;
          this.createDialog = false;
        })
        .then(response => {
          this.alertSuccess = true;
          this.executeView();
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
          this.alertFailure = true;
        });
    },

    open(position) {
      this.position = position;
      this.viewDialog = true;
    }
  },
  created() {
    this.executeView();
    console.log("created");
  }
};
</script>

<style scoped lang="stylus">
// .q-card__section {
// padding-left: 0px !important;
// }
.btn-action {
  width: 25px !important;
}

.q-card q-gutter-md {
  margin: 16px;
}

.q-field__native.row {
  padding-bottom: 0px;
  padding-top: 8px;
}

.my-sticky-column-table {
  specifying: max-width so the example can;
  highlight: the sticky column on any browser window;
}

max-width 600px {
  thead tr:first-child th:first-child {
    background-color: #fff;
    opacity: 1;
  }

  td:first-child {
    background-color: #f5f5dc;
  }

  thead tr:first-child th:first-child, td:first-child {
    position: sticky;
    left: 0;
    z-index: 1;
  }
}

.mycontent-left {
  border-right: 1px dashed #333;
}
</style>
