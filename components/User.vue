<template>
  <div id="app">
    <v-app id="inspire">
      <v-data-table
        :headers="headers"
        :items="desserts"
        sort-by="lastName"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat color="Dark">
            <v-toolbar-title>List User</v-toolbar-title>
            <v-divider
              class="mx-4"
              inset
              vertical
            ></v-divider>
            <div class="flex-grow-1"></div>
            <v-dialog v-model="dialog" max-width="500px">
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>
                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field v-model="editedItem.name" label="Tên"></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-text-field v-model="editedItem.lastName" label="Họ"></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-menu
                          ref="menu"
                          v-model="menu"
                          :close-on-content-click="false"
                          :return-value.sync="birth"
                          transition="scale-transition"
                          offset-y
                          full-width
                          min-width="290px"
                        >
                          <template v-slot:activator="{ on }">
                            <v-text-field
                              v-model="editedItem.birth"
                              label="Ngày sinh"
                              readonly
                              v-on="on"
                            ></v-text-field>
                          </template>
                          <v-date-picker v-model="birth" no-title scrollable>
                            <div class="flex-grow-1"></div>
                            <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
                            <v-btn text color="primary" @click="$refs.menu.save(birth)">OK</v-btn>
                          </v-date-picker>
                        </v-menu>
                      </v-col>
                      <v-col cols="12">
                        <v-menu
                          ref="menu"
                          v-model="menu1"
                          :close-on-content-click="false"
                          :return-value.sync="date"
                          transition="scale-transition"
                          offset-y
                          full-width
                          min-width="290px"
                        >
                          <template v-slot:activator="{ on }">
                            <v-text-field
                              v-model="editedItem.times"
                              label="Ngày đăng ký"
                              readonly
                              v-on="on"
                            ></v-text-field>
                          </template>
                          <v-date-picker v-model="date" no-title scrollable>
                            <div class="flex-grow-1"></div>
                            <v-btn text color="primary" @click="menu1 = false">Cancel</v-btn>
                            <v-btn text color="primary" @click="$refs.menu1.save(date)">OK</v-btn>
                          </v-date-picker>
                        </v-menu>
                      </v-col>
                      <v-col cols="12">
                        <v-text-field v-model="editedItem.mail" label="Email"></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-switch v-model="editedItem.status" label="Trạng thái"></v-switch>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <div class="flex-grow-1"></div>
                  <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                  <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.action="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">edit</v-icon>
          <v-icon small @click="deleteItem(item)">delete</v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary" @click="initialize">Reset</v-btn>
        </template>
      </v-data-table>
    </v-app>
  </div>
</template>

<script>
export default {
  data: () => ({
    times: new Date().toISOString().substr(0, 10),
    birth: new Date().toISOString().substr(0, 10),
    menu: false,
    menu1: false,
    dialog: false,
    headers: [
      {
        text: 'Tên',
        align: 'left',
        sortable: false,
        value: 'name'
      },
      { text: 'Họ', value: 'lastName' },
      { text: 'Ngày sinh', value: 'date' },
      { text: 'Thời gian đăng ký', value: 'times' },
      { text: 'email', value: 'mail' },
      { text: 'Trạng thái', value: 'status' },
      { text: 'Action', value: 'action', sortable: false }
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      lastName: '',
      birth: '',
      times: '',
      mail: '',
      status: ''
    },
    defaultItem: {
      name: '',
      lastName: '',
      birth: '',
      times: '',
      mail: '',
      status: ''
    }
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New User' : 'Edit User'
    }
  },

  watch: {
    dialog (val) {
      val || this.close()
    }
  },

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.desserts = [
        {
          name: 'Frozen',
          lastName: 'Yogurt',
          date: '14/06/1996',
          times: '15/09/2019',
          mail: 'frozen@gmail.com',
          status: 'kích hoạt'
        },
        {
          name: 'Frozen',
          lastName: 'Yogurt',
          date: '14/06/1996',
          times: '15/09/2019',
          mail: 'frozen@gmail.com',
          status: 'kích hoạt'
        },
        {
          name: 'Frozen',
          lastName: 'Yogurt',
          date: '14/06/1996',
          times: '15/09/2019',
          mail: 'frozen@gmail.com',
          status: 'kích hoạt'
        },
        {
          name: 'Frozen',
          lastName: 'Yogurt',
          date: '14/06/1996',
          times: '15/09/2019',
          mail: 'frozen@gmail.com',
          status: 'kích hoạt'
        },
        {
          name: 'Frozen',
          lastName: 'Yogurt',
          date: '14/06/1996',
          times: '15/09/2019',
          mail: 'frozen@gmail.com',
          status: 'kích hoạt'
        },
        {
          name: 'Frozen',
          lastName: 'Yogurt',
          date: '14/06/1996',
          times: '15/09/2019',
          mail: 'frozen@gmail.com',
          status: 'kích hoạt'
        },
        {
          name: 'Frozen',
          lastName: 'Yogurt',
          date: '14/06/1996',
          times: '15/09/2019',
          mail: 'frozen@gmail.com',
          status: 'kích hoạt'
        }
      ]
    },

    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      const index = this.desserts.indexOf(item)
      this.desserts.splice(index, 1)
    },

    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    }
  }
}
</script>

<style scoped>

</style>
