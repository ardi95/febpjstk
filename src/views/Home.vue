<template>
  <div class="customP">
    <v-container>
      <v-row>
        <v-col cols="12">
          <p class="font-24 font-weight-bold">Personal Details</p>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="6">
          <v-text-field
            v-model="job"
            name="job"
            label="Wanted Job Title"
            outlined
            clearable
          ></v-text-field>

          <v-text-field
            v-model="first_name"
            name="first_name"
            label="First Name"
            outlined
            clearable
          ></v-text-field>

          <v-text-field
            v-model="email"
            name="email"
            label="Email"
            outlined
            clearable
          ></v-text-field>

          <v-select
            v-model="country"
            :items="listCountry"
            item-text="name"
            item-value="id"
            label="Country"
            outlined
            clearable
            @change="changeCountry"
          >
            <!--class="width-auto d-inline-block"-->
          </v-select>

          <v-text-field
            v-model="address"
            name="address"
            label="Address"
            outlined
            clearable
          ></v-text-field>

          <v-text-field
            v-model="no_driving"
            name="no_driving"
            label="Driving License"
            outlined
            clearable
          ></v-text-field>

          <v-text-field
            v-model="place_of_birth"
            name="place_of_birth"
            label="Place of Birth"
            outlined
            clearable
          ></v-text-field>
        </v-col>

        <v-col cols="6">
          <div class="d-flex align-start">
            <input type="file" accept="image/png" ref="image" style="display: none" @change="onFilePicked" />
            <div class="d-inline-block uploadCustom mr-4">
              <div
                class="exImg radius-8"
                @click="pickFile"
                :style="{
                  backgroundImage: `url(${imageUrl})`,
                }"
                style="width: 56px; height: 56px"
              >
                <div class="overlay radius-8">
                  <v-icon dark class="exIcon">mdi-pencil</v-icon>
                </div>
              </div>
            </div>

            <div class="d-inline-block">
              <p class="font-18 font-weight-bold">Image</p>
              <p>Image extension .png</p>
            </div>
          </div>

          <v-text-field
            v-model="last_name"
            name="last_name"
            label="Last Name"
            outlined
            clearable
            class="mt-30"
          ></v-text-field>

          <v-text-field
            v-model="phone"
            name="phone"
            label="Phone"
            outlined
            clearable
          ></v-text-field>

          <v-select
            v-model="city"
            :items="listCity"
            item-text="name"
            item-value="id"
            label="City"
            outlined
            clearable
            :disabled="listCity === null"
          >
            <!--class="width-auto d-inline-block"-->
          </v-select>

          <v-text-field
            v-model="postal_code"
            name="postal_code"
            label="Postal Code"
            outlined
            clearable
          ></v-text-field>

          <v-text-field
            v-model="nationality"
            name="nationality"
            label="Nationality"
            outlined
            clearable
          ></v-text-field>

          <v-menu
            ref="menu"
            v-model="menu"
            :close-on-content-click="false"
            :return-value.sync="date_of_birth"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="date_of_birth"
                label="Date of Birth"
                prepend-inner-icon="mdi-calendar"
                outlined
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date_of_birth" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu = false"> Cancel </v-btn>
              <v-btn
                text
                color="primary"
                @click="$refs.menu.save(date_of_birth)"
              >
                OK
              </v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>
      </v-row>

      <v-row class="mt-6">
        <v-col cols="12">
          <p class="font-24 font-weight-bold">Professional Summary</p>

          <vue-editor v-model="summary"></vue-editor>
        </v-col>
      </v-row>

      <v-row class="mt-6">
        <v-col cols="12">
          <p class="font-24 font-weight-bold">Employment History</p>

          <template v-if="employment.length">
            <div
              class="radius-5 border-solid border-grey pa-4 mt-4"
              v-for="(dt, indexKey) in employment"
              :key="indexKey"
            >
              <p class="font-weight-bold">
                {{ dt.job_title }} at {{ dt.employer }}
              </p>

              <p class="color-info">{{ dt.start_date }} - {{ dt.end_date }}</p>
            </div>
          </template>

          <v-row v-if="statusFormEmploye" class="mt-4">
            <v-col cols="6">
              <v-text-field
                v-model="formEmployment.job_title"
                name="job_title"
                label="Job title"
                outlined
                clearable
              ></v-text-field>
            </v-col>

            <v-col cols="6">
              <v-text-field
                v-model="formEmployment.employer"
                name="employer"
                label="Employer"
                outlined
                clearable
              ></v-text-field>
            </v-col>

            <v-col cols="6">
              <div class="flex-container flex-container--start">
                <div class="table-handler-flex mr-4">
                  <v-menu
                    ref="menu2"
                    v-model="menu2"
                    :close-on-content-click="false"
                    :return-value.sync="formEmployment.start_date"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="formEmployment.start_date"
                        label="Start date"
                        prepend-inner-icon="mdi-calendar"
                        outlined
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="formEmployment.start_date"
                      type="month"
                      no-title
                      scrollable
                    >
                      <v-spacer></v-spacer>
                      <v-btn text color="primary" @click="menu2 = false">
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.menu2.save(formEmployment.start_date)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-menu>
                </div>
                <div class="table-handler-flex mr-4">
                  <v-menu
                    ref="menu3"
                    v-model="menu3"
                    :close-on-content-click="false"
                    :return-value.sync="formEmployment.end_date"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="formEmployment.end_date"
                        label="End date"
                        prepend-inner-icon="mdi-calendar"
                        outlined
                        readonly
                        v-bind="attrs"
                        v-on="on"
                        style="width: 40%"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="formEmployment.end_date"
                      type="month"
                      no-title
                      scrollable
                    >
                      <v-spacer></v-spacer>
                      <v-btn text color="primary" @click="menu3 = false">
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.menu3.save(formEmployment.end_date)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-menu>
                </div>
              </div>
            </v-col>

            <v-col cols="6">
              <v-text-field
                v-model="formEmployment.city"
                name="city"
                label="City"
                outlined
                clearable
              ></v-text-field>
            </v-col>

            <v-col cols="12">
              <p>Description:</p>

              <vue-editor v-model="formEmployment.description"></vue-editor>
            </v-col>
          </v-row>
        </v-col>

        <v-row class="mt-4">
          <v-col cols="12">
            <v-btn
              text
              color="primary"
              v-if="!statusFormEmploye"
              @click="statusFormEmploye = true"
            >
              <v-icon>mdi-plus</v-icon> Add Employment
            </v-btn>
            <v-btn
              color="primary"
              v-if="statusFormEmploye"
              @click="addEmployment"
              >submit</v-btn
            >
          </v-col>
        </v-row>
      </v-row>

      <v-row class="mt-6">
        <v-col cols="12">
          <p class="font-24 font-weight-bold">Skills</p>
        </v-col>
      </v-row>

      <v-row class="mt-4">
        <v-col cols="12">
          <v-btn
            v-for="(dt2, indexKey2) in listSkill"
            :key="indexKey2"
            :color="dt2.select ? 'primary' : 'default'"
            class="mr-4"
            @click="clickSkill(indexKey2)"
          >
            {{ dt2.name }}
            <v-icon v-if="!dt2.select">mdi-plus</v-icon>
            <v-icon v-if="dt2.select">mdi-check</v-icon></v-btn
          >
          <!--<v-btn color="primary" class="mr-4"
            >Java <v-icon>mdi-check</v-icon></v-btn
          >-->
        </v-col>
      </v-row>

      <v-row class="mt-4">
        <v-col cols="12">
          <v-expansion-panels v-if="skillAdd.length" accordion multiple>
            <v-expansion-panel
              v-for="(dt3, indexKey3) in skillAdd"
              :key="indexKey3"
            >
              <v-expansion-panel-header>
                <div>
                  <p class="font-weight-bold">{{ dt3.name }}</p>
                  <p class="color-info mt-4">
                    {{
                      skillAdd[indexKey3].level === 0
                        ? 'Bad'
                        : skillAdd[indexKey3].level === 1
                        ? 'Less'
                        : skillAdd[indexKey3].level === 2
                        ? 'Skillful'
                        : skillAdd[indexKey3].level === 3
                        ? 'Experience'
                        : 'Expert'
                    }}
                  </p>
                </div>
              </v-expansion-panel-header>
              <v-expansion-panel-content>
                <v-row>
                  <v-col cols="6">
                    <v-text-field
                      v-model="skillAdd[indexKey3].name"
                      name="name_skill"
                      label="Skill"
                      outlined
                      readonly
                    ></v-text-field>
                  </v-col>

                  <v-col cols="6">
                    <p>
                      Level -
                      <span
                        :style="{
                          color:
                            skillAdd[indexKey3].level === 0
                              ? 'grey'
                              : skillAdd[indexKey3].level === 1
                              ? 'red'
                              : skillAdd[indexKey3].level === 2
                              ? 'orange'
                              : skillAdd[indexKey3].level === 3
                              ? 'green'
                              : 'purple',
                        }"
                        >{{
                          skillAdd[indexKey3].level === 0
                            ? 'Bad'
                            : skillAdd[indexKey3].level === 1
                            ? 'Less'
                            : skillAdd[indexKey3].level === 2
                            ? 'Skillful'
                            : skillAdd[indexKey3].level === 3
                            ? 'Experience'
                            : 'Expert'
                        }}</span
                      >
                    </p>

                    <v-slider
                      v-model="skillAdd[indexKey3].level"
                      :tick-labels="ticksLevel"
                      :max="4"
                      step="1"
                      ticks="always"
                      tick-size="5"
                      :thumb-color="
                        skillAdd[indexKey3].level === 0
                          ? 'grey'
                          : skillAdd[indexKey3].level === 1
                          ? 'red'
                          : skillAdd[indexKey3].level === 2
                          ? 'orange'
                          : skillAdd[indexKey3].level === 3
                          ? 'green'
                          : 'purple'
                      "
                      :track-color="
                        skillAdd[indexKey3].level === 0
                          ? 'grey'
                          : skillAdd[indexKey3].level === 1
                          ? 'red'
                          : skillAdd[indexKey3].level === 2
                          ? 'orange'
                          : skillAdd[indexKey3].level === 3
                          ? 'green'
                          : 'purple'
                      "
                      :color="
                        skillAdd[indexKey3].level === 0
                          ? 'grey'
                          : skillAdd[indexKey3].level === 1
                          ? 'red'
                          : skillAdd[indexKey3].level === 2
                          ? 'orange'
                          : skillAdd[indexKey3].level === 3
                          ? 'green'
                          : 'purple'
                      "
                    ></v-slider>

                    <!--<div class="mt-4 d-inline-block color-orange-bg width-full" style="height: 56px;">
                      <div class="d-inline-block cursor-pointer" style="width: 20%;height: 56px;">
                      </div>

                      <div class="d-inline-block cursor-pointer" style="width: 20%;height: 56px;">
                      </div>
                      
                      <div class="d-inline-block cursor-pointer" style="width: 20%;height: 56px;">
                      </div>
                      
                      <div class="d-inline-block cursor-pointer" style="width: 20%;height: 56px;">
                      </div>
                      
                      <div class="d-inline-block cursor-pointer" style="width: 20%;height: 56px;">
                      </div>
                    </div>-->
                  </v-col>
                </v-row>
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>
<!--<template>
  <div>
    <router-view></router-view>
  </div>
</template>-->

<script>
import { VueEditor } from 'vue2-editor'

export default {
  components: {
    VueEditor,
  },

  data() {
    return {
      job: '',
      first_name: '',
      last_name: '',
      phone: '',
      email: '',
      imageUrl: '/static/img/userdefault2.png',
      imageData: '',
      listCountry: [
        {
          id: 0,
          name: 'Indonesia',
          city: [
            {
              id: 0,
              name: 'Jakarta',
            },
            {
              id: 1,
              name: 'Bandung',
            },
            {
              id: 2,
              name: 'Bekasi',
            },
          ],
        },
        {
          id: 1,
          name: 'Jepang',
          city: [
            {
              id: 0,
              name: 'Tokyo',
            },
            {
              id: 1,
              name: 'Osaka',
            },
            {
              id: 2,
              name: 'Kyoto',
            },
          ],
        },
        {
          id: 2,
          name: 'Korea Selatan',
          city: [
            {
              id: 0,
              name: 'Seoul',
            },
            {
              id: 1,
              name: 'Busan',
            },
            {
              id: 2,
              name: 'Incheon',
            },
          ],
        },
      ],
      country: null,
      listCity: null,
      city: null,
      address: '',
      no_driving: '',
      nationality: '',
      place_of_birth: '',
      postal_code: '',
      date_of_birth: new Date(
        Date.now() - new Date().getTimezoneOffset() * 60000
      )
        .toISOString()
        .substr(0, 10),
      menu: false,
      menu2: false,
      menu3: false,
      summary: '',
      formEmployment: {
        job_title: '',
        employer: '',
        start_date: new Date(
          Date.now() - new Date().getTimezoneOffset() * 60000
        )
          .toISOString()
          .substr(0, 7),
        end_date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
          .toISOString()
          .substr(0, 7),
        description: '',
      },
      statusFormEmploye: false,
      employment: [],
      listSkill: [
        {
          name: 'Java',
          select: false,
          level: 0,
        },
        {
          name: 'Javascript',
          select: false,
          level: 0,
        },
        {
          name: 'Python',
          select: false,
          level: 0,
        },
        {
          name: 'Git',
          select: false,
          level: 0,
        },
        {
          name: 'SQL',
          select: false,
          level: 0,
        },
      ],
      panel: [],
      formSkill: {
        name: '',
        level: 2,
      },
      ticksLevel: ['Bad', 'Less', 'Skillful', 'Experience', 'Expert'],
      skillAdd: [],
    }
  },

  methods: {
    pickFile() {
      this.$refs.image.click()
    },
    onFilePicked() {
      const files = this.$refs.image.files[0]
      console.log(files);
      if (files !== undefined) {
        const fr = new FileReader()
        fr.readAsDataURL(files)
        fr.addEventListener('load', () => {
          this.imageUrl = fr.result
          this.imageData = files
        })
      }
    },
    changeCountry(data) {
      console.log(data)
      this.listCity = null
      this.city = null

      const indexKey = this.listCountry.findIndex((item) => item.id === data)

      if (indexKey !== -1) {
        this.listCity = this.listCountry[indexKey].city
      }
    },

    addEmployment() {
      this.employment = [...this.employment, this.formEmployment]
      ;(this.formEmployment = {
        job_title: '',
        employer: '',
        start_date: new Date(
          Date.now() - new Date().getTimezoneOffset() * 60000
        )
          .toISOString()
          .substr(0, 7),
        end_date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
          .toISOString()
          .substr(0, 7),
        description: '',
      }),
        (this.statusFormEmploye = false)

      console.log(this.employment)
    },

    clickSkill(indexKey) {
      if (!this.listSkill[indexKey].select) {
        this.skillAdd = [...this.skillAdd, this.listSkill[indexKey]]
      } else {
        let indexSelect = this.skillAdd.findIndex(
          (item) => item.name === this.listSkill[indexKey].name
        )

        let tempArray = this.skillAdd
        tempArray.splice(indexSelect, 1)

        this.skillAdd = tempArray
      }

      this.listSkill[indexKey].select = !this.listSkill[indexKey].select
    },
  },
}
</script>