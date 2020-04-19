<template>
  <q-page>
    <div class="q-pa-md">
      <div class="row q-col-gutter-md">
        <div class="col-xs-12 col-sm-12 col-md-3" v-for="(i, index) in shortcuts" :key="i.id">
          <shortcut :name="i.name" :color="i.color" :icon="i.icon" :to="i.to">
            <q-popup-proxy context-menu>
              <q-list flat separator>
                <q-item clickable v-ripple @click="edit(i)">
                  <q-item-section>Editar</q-item-section>
                </q-item>

                <q-item clickable v-ripple @click="remove(index)">
                  <q-item-section>Excluir</q-item-section>
                </q-item>
              </q-list>
            </q-popup-proxy>
          </shortcut>
        </div>
      </div>
      <q-page-sticky position="bottom-right" :offset="[18, 18]">
        <q-btn fab icon="add" color="grey-4" text-color="primary" @click="alert = true" />
      </q-page-sticky>

      <q-dialog v-model="alert">
        <q-card>
          <q-card-section>
            <div class="text-h6">Novo Atalho</div>
          </q-card-section>

          <q-card-section class="q-pt-none">
            <q-card style="min-width:480px" :style="`background:${shortcut.color}`">
              <q-card-section>
                <div class="row">
                  <div class="col-2">
                    <q-avatar square>
                      <img :src="getImgUrl(shortcut.icon)" />
                      <q-popup-proxy>
                        <q-banner>
                          <div style="max-width:400px" class="row q-col-gutter-md">
                            <div class="col-3" v-for="icon in icons" :key="icon">
                              <q-item clickable v-ripple @click="shortcut.icon = icon">
                                <q-item-section avatar>
                                  <q-avatar square class="absolute-center">
                                    <img :src="getImgUrl(icon)" />
                                  </q-avatar>
                                </q-item-section>
                              </q-item>
                            </div>
                          </div>
                        </q-banner>
                      </q-popup-proxy>
                    </q-avatar>
                  </div>
                  <div class="col">
                    <q-input
                      v-model="shortcut.name"
                      placeholder="Nome do atalho"
                      class="text-h6 text-weight-light"
                      borderless
                    />
                  </div>
                  <div class="col-12">
                    <q-input
                      v-model="shortcut.to"
                      placeholder="URL"
                      class="text-weight-light"
                      borderless
                    />
                  </div>
                </div>

                <div class="col-12">
                  <q-input v-model="shortcut.color" label="Cor" borderless>
                    <template v-slot:append>
                      <q-icon name="colorize" class="cursor-pointer">
                        <q-popup-proxy transition-show="scale" transition-hide="scale">
                          <q-color v-model="shortcut.color" />
                        </q-popup-proxy>
                      </q-icon>
                    </template>
                  </q-input>
                </div>
              </q-card-section>
            </q-card>
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat label="OK" color="primary" v-close-popup @click="persistShortcut()" />
          </q-card-actions>
        </q-card>
      </q-dialog>
    </div>
  </q-page>
  <!-- <q-page class="flex flex-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-full.svg"
    >
  </q-page>-->
</template>

<script>
import Shortcut from "../components/Shortcut";

export default {
  name: "PageIndex",
  data() {
    return {
      alert: false,
      shortcut: {
        id: null,
        name: "",
        color: "#000000",
        icon: "aws.svg",
        to: ""
      },
      shortcuts: []
    };
  },
  computed: {
    // shortcuts() {
    //   return this.$q.localStorage.getItem("shortcuts");
    // }
    icons() {
      const icons = [];
      icons.push("spdata.png");
      icons.push("jenkins.png");
      icons.push("aws.svg");
      icons.push("jelastic.svg");
      icons.push("jira.svg");
      icons.push("bb.svg");
      icons.push("gmail.svg");
      icons.push("google.svg");
      icons.push("drive.svg");
      icons.push("github.svg");
      icons.push("facebook.svg");
      icons.push("linkedin.svg");
      icons.push("whatsapp.svg");
      icons.push("mail.svg");
      icons.push("skype.svg");
      icons.push("primefaces.png");
      icons.push("quasar.svg");
      icons.push("vue.svg");
      icons.push("caixa.png");
      return icons;
    }
  },
  methods: {
    getImgUrl(icon) {
      return require("../assets/" + icon);
    },
    persistShortcut() {
      if (!this.shortcut.id) {
        this.shortcut.id = new Date().getTime();
        this.shortcuts.push(this.shortcut);
      } else {
        const element = this.shortcuts.filter((element, index) => {
          if (element.id === this.shortcut.id) {
            this.shortcuts[index] = this.shortcut;
            return element;
          }
        });
      }

      this.$q.localStorage.set("shortcuts", this.shortcuts);

      this.shortcut = {
        id: null,
        name: "",
        color: "#000000",
        icon: "aws.svg",
        to: ""
      };
    },
    edit(shortcut) {
      this.shortcut = shortcut;
      this.alert = true;
      console.log(this.shortcut);
    },
    remove(index) {
      this.shortcuts.splice(index, 1);
      this.$q.localStorage.set("shortcuts", this.shortcuts);
    }
  },
  mounted() {
    // alert(this.alert);
    this.shortcuts = this.$q.localStorage.getItem("shortcuts");
    if (this.shortcuts == null) {
      this.shortcuts = [];
      this.$q.localStorage.set("shortcuts", this.shortcuts);
    }
  },
  components: { shortcut: Shortcut }
};
</script>
