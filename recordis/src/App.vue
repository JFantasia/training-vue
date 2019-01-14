<template>
  <v-app id="inspire">
    <v-navigation-drawer
      :clipped="$vuetify.breakpoint.lgAndUp"
      v-model="drawer"
      fixed
      app
    >
      <v-list dense>
        <template v-for="item in items">
          <v-layout
            v-if="item.heading"
            :key="item.heading"
            row
            align-center
          >
            <v-flex xs6>
              <v-subheader v-if="item.heading">
                {{ item.heading }}
              </v-subheader>
            </v-flex>
            <v-flex xs6 class="text-xs-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>
          <v-list-group
            v-else-if="item.children"
            v-model="item.model"
            :key="item.text"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon=""
          >
            <v-list-tile slot="activator">
              <v-list-tile-content>
                <v-list-tile-title style="font-size: 12pt;">
                  {{ item.text }}
                </v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
            <v-list-tile
              v-for="(child, i) in item.children"
              :key="i"
              @click=""
            >
              <v-list-tile-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title>
                  {{ child.text }}
                </v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
          </v-list-group>
          <v-list-tile v-else :key="item.text" @click="">
            <v-list-tile-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title style="font-size: 12pt;">
                {{ item.text }}
              </v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </template>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      color="blue darken-3"
      dark
      app
      fixed
    >
      <v-toolbar-title style="width: 500px" class="ml-0 pl-3">
        <v-icon medium>apps</v-icon>
        <span style="font-size: 20pt;">Recordis.</span>
        <span style="font-size: 12pt">Una Aplicación, todas las cosas.</span>        
      </v-toolbar-title>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-text-field
        flat
        solo-inverted
        hide-details
        prepend-inner-icon="search"
        label="Search"
        class="hidden-sm-and-down"
      ></v-text-field>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>apps</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>notifications</v-icon>
      </v-btn>
      <v-btn icon large>
        <v-avatar size="32px" tile>
          <img
            src="https://cdn.vuetifyjs.com/images/logos/logo.svg"
            alt="Vuetify"
          >
        </v-avatar>
      </v-btn>
    </v-toolbar>

    <v-content>
      <v-container fluid grid-list-md>
        <v-layout row wrap>
          <v-flex d-flex xs12 sm12 md12>
            <v-card color="purple" dark>


              <div>
                <v-toolbar flat color="white">
                  <v-toolbar-title>Mis Tareas</v-toolbar-title>
                  <v-divider class="mx-2" inset vertical></v-divider>
                  <v-spacer></v-spacer>
                  <v-dialog v-model="dialog" max-width="500px">
                    <v-btn slot="activator" color="primary" dark class="mb-2">+Tarea</v-btn>
                    <v-card>
                      <v-card-title>
                        <span class="headline">{{ formTitle }}</span>
                      </v-card-title>

                      <v-card-text>
                        <v-container grid-list-md>
                          <v-layout wrap>
                            <v-flex xs12 sm6 md4>
                              <v-text-field v-model="editedItem.name" label="Dessert name"></v-text-field>
                            </v-flex>
                            <v-flex xs12 sm6 md4>
                              <v-text-field v-model="editedItem.calories" label="Calories"></v-text-field>
                            </v-flex>
                            <v-flex xs12 sm6 md4>
                              <v-text-field v-model="editedItem.fat" label="Fat (g)"></v-text-field>
                            </v-flex>
                            <v-flex xs12 sm6 md4>
                              <v-text-field v-model="editedItem.carbs" label="Carbs (g)"></v-text-field>
                            </v-flex>
                            <v-flex xs12 sm6 md4>
                              <v-text-field v-model="editedItem.protein" label="Protein (g)"></v-text-field>
                            </v-flex>
                          </v-layout>
                        </v-container>
                      </v-card-text>

                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" flat @click="close">Cancel</v-btn>
                        <v-btn color="blue darken-1" flat @click="save">Save</v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-dialog>
                </v-toolbar>
                <v-data-table
                  :headers="headers"
                  :items="desserts"
                  class="elevation-1"
                >
                  <template slot="items" slot-scope="props">
                    <td>{{ props.item.name }}</td>
                    <td class="text-xs-right">{{ props.item.calories }}</td>
                    <td class="text-xs-right">{{ props.item.fat }}</td>
                    <td class="text-xs-right">{{ props.item.carbs }}</td>
                    <td class="text-xs-right">{{ props.item.protein }}</td>
                    <td class="justify-center layout px-0">
                      <v-icon
                        small
                        class="mr-2"
                        @click="editItem(props.item)"
                      >
                        edit
                      </v-icon>
                      <v-icon
                        small
                        @click="deleteItem(props.item)"
                      >
                        delete
                      </v-icon>
                    </td>
                  </template>
                  <template slot="no-data">
                    <v-btn color="primary" @click="initialize">Reset</v-btn>
                  </template>
                </v-data-table>
              </div>


            </v-card>
          </v-flex>
        </v-layout>        
        <v-layout row wrap>
          <v-flex d-flex xs12 sm6 md4>
            <v-layout row wrap>
              <v-flex d-flex xs12 sm12 md12>
                <div>
                    <v-toolbar flat color="white">
                      <v-toolbar-title>Alarmas</v-toolbar-title>
                      <v-divider class="mx-2" inset vertical></v-divider>
                      <v-spacer></v-spacer>
                    </v-toolbar>
                    <v-data-table
                      :headers="headers2"
                      :items="desserts2"
                      class="elevation-1"
                    >
                      <template slot="items" slot-scope="props">
                        <td><v-icon style="color: red">history</v-icon></td>
                        <td>{{ props.item.name }}</td>
                      </template>
                      <template slot="no-data">
                        <v-btn color="primary" @click="initialize">Reset</v-btn>
                      </template>
                    </v-data-table>
                  </div>

              </v-flex>
              <v-flex d-flex xs12 sm12 md12>
                <v-card color="purple" dark>
                  <v-card-title primary class="title">Lorem</v-card-title>
                  <v-card-text>
                    
                    <v-card-title primary class="title">Lorem</v-card-title>
                    <v-card-text>{{ lorem }}</v-card-text>
                  

                  </v-card-text>
                </v-card>
              </v-flex>
              <v-flex d-flex xs12 sm12 md12>
                <v-card color="purple" dark>
                  <v-card-title primary class="title">Lorem</v-card-title>
                  <v-card-text>{{ lorem }}</v-card-text>
                </v-card>
              </v-flex>
          </v-layout> 
          </v-flex>
          <v-flex d-flex xs12 sm6 md4>
            <v-layout row wrap>
              <v-flex d-flex>
                <v-card color="indigo" dark>
                  <v-card-text>{{ lorem.slice(0, 70) }}</v-card-text>
                </v-card>
              </v-flex>
              <v-flex d-flex>
                <v-layout row wrap>
                  <v-flex
                    v-for="n in 2"
                    :key="n"
                    d-flex
                    xs12
                  >
                    <v-card
                      color="red lighten-2"
                      dark
                    >
                      <v-card-text>{{ lorem.slice(0, 40) }}</v-card-text>
                    </v-card>
                  </v-flex>
                </v-layout>
              </v-flex>
            </v-layout>
          </v-flex>
          <v-flex d-flex xs12 sm6 md4>
            <v-card color="blue lighten-2" dark>
              <v-card-text>{{ lorem.slice(0, 100) }}</v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      headers: [
        {
          text: 'Dessert (100g serving)',
          align: 'left',
          sortable: false,
          value: 'name'
        },
        { text: 'Calories', value: 'calories' },
        { text: 'Fat (g)', value: 'fat' },
        { text: 'Carbs (g)', value: 'carbs' },
        { text: 'Protein (g)', value: 'protein' },
        { text: 'Actions', value: 'name', sortable: false }
      ],
      headers2: [
        { text: 'Estado', value: 'name', sortable: false },
        {
          text: 'Detalle',
          align: 'left',
          sortable: false,
          value: 'name'
        }
      ],
      desserts: [],
      desserts2: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      },
      defaultItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      },
      lorem: `Lorem ipsum dolor sit amet, mel at clita quando. Te sit oratio vituperatoribus, nam ad ipsum posidonium mediocritatem, explicari dissentiunt cu mea. Repudiare disputationi vim in, mollis iriure nec cu, alienum argumentum ius ad. Pri eu justo aeque torquatos.`,
      drawer: null,
      items: [
        { icon: 'contact_phone', text: 'Contactos' },
        { icon: 'alarm', text: 'Alarmas' },
        { icon: 'assignment', text: 'Rutinas' },
        { icon: 'date_range', text: 'Calendario' },
        { icon: 'shopping_cart', text: 'Compras' },
        { icon: 'collections', text: 'Galería' },
        { icon: 'assessment', text: 'Finanzas' },
        { icon: 'toc', text: 'Tareas' },
        { icon: 'place', text: 'Lugares' },
        { icon: 'public', text: 'Viajes' }
      ]
    }),
    props: {
      source: String
    },

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
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
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7
          }
        ],
        this.desserts2 = [
          {
            name: 'Frozen Yogurt'
          },
          {
            name: 'Ice cream sandwich'
          },
          {
            name: 'Frozen Yogurt'
          },
          {
            name: 'Ice cream sandwich'
          },
          {
            name: 'Frozen Yogurt'
          },
          {
            name: 'Ice cream sandwich'
          },
          {
            name: 'Frozen Yogurt'
          },
          {
            name: 'Ice cream sandwich'
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
        confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
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