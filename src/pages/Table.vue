<template>
  <div class="q-pa-md">
    <q-table
      title="Treats"
      :data="data"
      :columns="columns"
      row-key="id"
      :filter="filter"
      :loading="loading"
    >
      <template v-slot:top>
        <span class="text-h5" style="margin-right: 12px;">Treats</span>
        <q-btn color="green" @click="createDialog = true" icon="add" size="sm"/>
        <q-space/>
        <q-input borderless dense debounce="300" color="primary" v-model="filter">
          <template v-slot:append>
            <q-icon name="search"/>
          </template>
        </q-input>
      </template>
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="userId" :props="props">{{ props.row.userId }}</q-td>
          <q-td key="id" :props="props">{{ props.row.id }}</q-td>
          <q-td key="title" class="text-ellipsis" :props="props">
            {{ props.row.title }}
            <q-popup-edit v-model="props.row.title">
              <span>{{ props.row.title }}</span>
            </q-popup-edit>
          </q-td>
          <q-td key="body" class="text-ellipsis" :props="props">
            {{ props.row.body }}
            <q-popup-edit v-model="props.row.body">
              <span>{{ props.row.body }}</span>
            </q-popup-edit>
          </q-td>
          <q-td key="action" :props="props" class="q-gutter-xs" style="min-width: 140px;">
            <q-btn
              color="primary"
              class="btn-action"
              icon="visibility"
              size="sm"
              @click="onView(props.row)"
            />
            <q-btn
              color="secondary"
              class="btn-action"
              icon="edit"
              size="sm"
              @click="onUpdate(props.row)"
            />
            <q-btn
              color="deep-orange"
              class="btn-action"
              icon="delete"
              size="sm"
              @click="onDelete(props.row)"
            />
          </q-td>
        </q-tr>
      </template>
    </q-table>
    <q-dialog v-model="createDialog" transition-show="flip-down" transition-hide="flip-up">
      <q-card style="width: 500px; max-width: 80vw;">
        <q-card-section class="bg-green">
          <div class="text-h6 text-white">Create Post</div>
        </q-card-section>
        <q-card-section style="padding-top: 16px;">
          <q-input v-model="details.userId" filled type="text" hint="User ID"/>
          <br>
          <q-input v-model="details.title" filled autogrow hint="Title"/>
          <br>
          <q-input v-model="details.body" filled autogrow hint="Body"/>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat :loading="loading" @click="executeCreate()">
            Create
            <template v-slot:loading>
              <q-spinner-facebook class="on-left"/>
            </template>
          </q-btn>
          <q-btn flat label="Close" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>

    <q-dialog v-model="viewDialog" transition-show="flip-down" transition-hide="flip-up">
      <q-card style="width: 500px; max-width: 80vw;">
        <q-card-section class="bg-primary">
          <div class="text-h6">View Post</div>
        </q-card-section>
        <q-card-section style="padding-top: 16px;">
          <q-markup-table>
            <thead>
              <tr>
                <th class="text-left">Attribute</th>
                <th>&nbsp;</th>
                <th class="text-left">Value</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>User Id</td>
                <td>:</td>
                <td>{{ details.userId }}</td>
              </tr>
              <tr>
                <td>ID</td>
                <td>:</td>
                <td>{{ details.id }}</td>
              </tr>
              <tr>
                <td>Title</td>
                <td>:</td>
                <td>{{ details.title }}</td>
              </tr>
              <tr>
                <td>Body</td>
                <td>:</td>
                <td>{{ details.body }}</td>
              </tr>
            </tbody>
          </q-markup-table>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-dialog v-model="updateDialog" transition-show="flip-down" transition-hide="flip-up">
      <q-card style="width: 500px; max-width: 80vw;">
        <q-card-section class="bg-primary">
          <div class="text-h6">Update Post</div>
        </q-card-section>
        <q-card-section style="padding-top: 16px;">
          <q-input v-model="details.userId" filled type="text" hint="User ID"/>
          <br>
          <q-input v-model="details.title" filled autogrow hint="Title"/>
          <br>
          <q-input v-model="details.body" filled autogrow hint="Body"/>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat :loading="loading" @click="executeUpdate()">
            Update
            <template v-slot:loading>
              <q-spinner-facebook class="on-left"/>
            </template>
          </q-btn>
          <q-btn flat label="Close" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-dialog v-model="deleteDialog" transition-show="flip-down" transition-hide="flip-up">
      <q-card style="width: 500px; max-width: 80vw;">
        <q-card-section class="bg-deep-orange">
          <div class="text-h6">Delete Post</div>
        </q-card-section>
        <q-card-section style="padding-top: 16px;">
          <p class="text-body2">
            Are you sure you want to delete the post "
            <span
              class="text-deep-orange"
            >{{ details.title }}</span>"?
          </p>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat :loading="loading" @click="executeDelete()">
            Delete
            <template v-slot:loading>
              <q-spinner-facebook class="on-left"/>
            </template>
          </q-btn>
          <q-btn flat label="Close" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-dialog v-model="alertSuccess">
      <q-card class="bg-teal text-white">
        <q-card-section>
          <div class="text-h6">Success</div>
        </q-card-section>

        <q-card-section>The changes were successfully saved.</q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="white" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-dialog v-model="alertFailure">
      <q-card class="bg-orange text-white">
        <q-card-section>
          <div class="text-h6">Failed</div>
        </q-card-section>

        <q-card-section>The changes were not saved.</q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="white" v-close-popup/>
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      loading: false,
      alertSuccess: false,
      alertFailure: false,
      filter: "",
      rowCount: 10,
      columns: [
        {
          name: "userId",
          align: "left",
          label: "User ID",
          field: "userId",
          sortable: true
        },
        {
          name: "id",
          align: "left",
          label: "ID",
          field: "id",
          sortable: true
        },
        {
          name: "title",
          align: "left",
          label: "Title",
          field: "title",
          sortable: true
        },
        {
          name: "body",
          align: "left",
          label: "Body",
          field: "body",
          sortable: true
        },
        { name: "action", align: "center", label: "Action" }
      ],
      details: {
        userId: "",
        id: "",
        title: "",
        body: ""
      },
      data: [],
      createDialog: false,
      viewDialog: false,
      updateDialog: false,
      deleteDialog: false
    };
  },
  methods: {
    onView(row) {
      this.viewDialog = true;
      this.details.userId = row.userId;
      this.details.id = row.id;
      this.details.title = row.title;
      this.details.body = row.body;
    },
    onUpdate(row) {
      this.updateDialog = true;
      this.details.userId = row.userId;
      this.details.id = row.id;
      this.details.title = row.title;
      this.details.body = row.body;
      var url = "https://jsonplaceholder.typicode.com/posts/" + this.details.id;
      console.log(row);
    },
    executeUpdate() {
      this.loading = true;
      var url = "https://jsonplaceholder.typicode.com/posts/" + this.details.id;
      console.log(url);
      axios
        .put(url, {
          body: JSON.stringify({
            id: this.details.id,
            title: this.details.title,
            body: this.details.body,
            userId: this.details.userId
          }),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
        .then(response => {
          console.log(response);
          this.details.id = "";
          this.details.title = "";
          this.details.body = "";
          this.details.userI = "";
          this.loading = false;
          this.updateDialog = false;
        })
        .then(response => {
          console.log(response);
          this.alertSuccess = true;
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
          this.alertFailure = true;
        });
    },
    onDelete(row) {
      this.deleteDialog = true;
      this.details.userId = row.userId;
      this.details.id = row.id;
      this.details.title = row.title;
      this.details.body = row.body;
      var url = "https://jsonplaceholder.typicode.com/posts/" + this.details.id;
      console.log(row);
    },
    executeDelete() {
      this.loading = true;
      var url = "https://jsonplaceholder.typicode.com/posts/" + this.details.id;
      console.log(url);
      axios
        .delete(url)
        .then(response => {
          console.log(response);
          this.loading = false;
          this.deleteDialog = false;
        })
        .then(response => {
          this.alertSuccess = true;
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
          this.alertFailure = true;
        });
    },
    executeCreate() {
      this.loading = true;
      var url = "https://jsonplaceholder.typicode.com/posts";
      console.log(url);
      // console.log(this.details.title)
      // console.log(this.details.body)
      // console.log(this.details.userId)
      axios
        .post(url, {
          method: "POST",
          body: JSON.stringify({
            title: this.details.title,
            body: this.details.body,
            userId: this.details.userId
          }),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
        .then(request => {
          console.logrequest;
        })
        .then(response => {
          console.log(response);
          this.loading = false;
          this.createDialog = false;
        })
        .then(response => {
          this.alertSuccess = true;
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
          this.alertFailure = true;
        });
    }
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then(response => {
        console.log(JSON.stringify(response));
        this.data = response["data"];
      })
      .catch(error => console.log(error));
  }
};
</script>
<style scoped lang="stylus">
.btn-action {
  width: 25px !important;
}

table {
  width: 100%;
}

td {
  max-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

td:nth-child(1) {
  width: 5%;
}

td:nth-child(2) {
  width: 5%;
}

td:nth-child(3) {
  width: 20%;
}

/* td:nth-child(4) {
    max-width: 30%;
} */
td:nth-child(5) {
  width: 15%;
}
</style>
