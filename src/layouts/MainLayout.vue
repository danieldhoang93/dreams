<template>
  <q-layout view="lHh Lpr lFf">
    <q-header bordered class="navbar">
      <q-toolbar>
         <amplify-authenticator hidden v-if="authState !== 'signedin'"></amplify-authenticator>
        <q-toolbar-title>
          <router-link to="/" class="logoText">Dreams</router-link>
        </q-toolbar-title>

        <div class="q-pr-sm" v-if="authState === 'signedin'">Hello,  {{user.attributes.email}}
          <q-btn outline style="color: white;" label="Sign Out" @click="signOut"/>
        </div>
    
        <q-btn v-else outline style="color: white;" label="Sign In" @click="signInWindow = true"/>

        <q-dialog v-model="signInWindow" v-if="!isSignedIn" persistent>
          <q-carousel
            v-model="signInStep"
            transition-prev="slide-right"
            transition-next="slide-left"
            animated
            control-color="primary"
            class="rounded-borders backgroundColor"
          >
          <q-carousel-slide :name="1" class="text-white border no-wrap flex-center cardBackground">
            <div class="text-h6 row q-pb-md">Share Your Dreams<q-space /><q-btn icon="close" flat round dense v-close-popup /></div>
                
            <q-card class="cardBackground text-white border">
              
              <q-card-section>
                  <q-input
                    dark
                    filled
                    v-model="username"
                    label="Email Address"
                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Enter an email address']"
                  />

                  <q-input dark v-model="password" filled :type="isPwd ? 'password' : 'text'" label="Password" :rules="[ val => val && val.length > 0 || 'Enter a password']">
                    <template v-slot:append>
                      <q-icon
                        :name="isPwd ? 'visibility_off' : 'visibility'"
                        class="cursor-pointer"
                        @click="isPwd = !isPwd"
                      />
                    </template>
                  </q-input>

                  <div>
                    <q-btn label="Sign In" @click="signIn" color="blue"/>
                    <q-btn label="Create Account" @click="signUp" color="white" flat class="q-ml-sm" />
                  </div>
                  
              </q-card-section>
            </q-card>
          </q-carousel-slide>

          <q-carousel-slide :name="2" class="border column no-wrap flex-center cardBackground">
            <q-card class="cardBackground text-white">
              <q-card-section class="row items-center q-pb-none">
                <div class="text-h6">Share Your Dreams</div>
                <q-space />
                <q-btn icon="close" flat round dense v-close-popup />
              </q-card-section>

              <q-card-section>
                  <q-input
                    dark
                    filled
                    v-model="code"
                    label="Confirmation Code"
                    lazy-rules
                    :rules="[ val => val && val.length > 0 || 'Enter an email address']"
                  />

                  <div>
                    <q-btn label="Confirm" @click="confirmSignUp" color="blue"/>
                  </div>
              </q-card-section>
            </q-card>
          </q-carousel-slide>
              
          </q-carousel>
        </q-dialog>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Essential Links
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer class="navbar" bordered>
      <q-toolbar v-if="authState === 'signedin'">
        <q-btn round dense color="blue" icon="add" class="pageWidth" @click="newPost = true, title = '', body = ''"/>
      </q-toolbar>

      <q-dialog v-model="newPost" persistent>
        <q-card style="min-width: 350px" class="cardBackground bordered text-white">
          <q-card-section>
            <div class="text-h6">Share a dream</div>
          </q-card-section>

          <q-card-section class="q-pt-none">
            <q-input
                dark
                filled
                dense
                v-model="title"
                label="Title"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Enter a title']"
              />

            <q-input
                dark
                filled
                dense
                autogrow
                v-model="body"
                label="Body"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Enter a body to your post']"
              />
          </q-card-section>

          <q-card-actions align="right" class="text-white">
            <q-btn dense flat label="Cancel" v-close-popup />
            <q-btn dense color="blue" label="Post" v-close-popup @click="postDream"/>
          </q-card-actions>
        </q-card>
      </q-dialog>
    </q-footer>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'
import { Auth } from 'aws-amplify';
import { authState, onAuthUIStateChange } from '@aws-amplify/ui-components'
import { API } from 'aws-amplify';

const linksData = [
  {
    title: 'Docs',
    caption: 'quasar.dev',
    icon: 'school',
    link: 'https://quasar.dev'
  },
  {
    title: 'Github',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework'
  }
];

export default {
  name: 'MainLayout',
  components: { EssentialLink },
  data () {
    return {
      user: '',
      authState: '',
      isSignedIn: false,
      leftDrawerOpen: false,
      essentialLinks: linksData,
      signInWindow: false,
      username: '',
      password: '',
      isPwd: true,
      signInStep: 1,
      code: '',
      newPost: false,
      title: '',
      body: '',
    }
  },
  methods: {
    postDream() {
      console.log('posting');
      API.post('dreamsApi', '/dreams', {
        body: {
          title: this.title,
          body: this.body,
          likes: []
        }
      }).then(result => {
        console.log(result);
      }).catch(err => {
        console.log(err);
      })
    },
    async signUp() {
      console.log("signing up", this.username, this.password);
      try {
        const userInfo = {
          username: this.username,
          password: this.password
        }
          const { user } = await Auth.signUp({
              username: this.username,
              password: this.password
          });
          console.log(user);
          this.signInStep++;
      } catch (error) {
          console.log('error signing up:', error);
      }
    },
    async signIn() {
      try {
          const user = await Auth.signIn(this.username, this.password);
          this.isSignedIn = true;
          console.log("signed in", user);
      } catch (error) {
          console.log('error signing in', error);
      }
    },
    async signOut() {
      try {
          await Auth.signOut();
          this.isSignedIn = false;
          this.signInStep = 1;
      } catch (error) {
          console.log('error signing out: ', error);
      }
    },
    async confirmSignUp() {
      try {
        await Auth.confirmSignUp(this.username, this.code);
        //console.log('user: ', user);
        this.isSignedIn = true;
        this.signInStep++;
      } catch (error) {
          console.log('error confirming sign up', error);
      }
    }
  },
  created() {
    //authentication state management
    onAuthUIStateChange((authState, authData) => {
      this.authState = authState;
      this.user = authData;
      //console.log("authData data:", authData, " authState: ", authState)
    })
  },
  beforeDestroy() {
    return onAuthUIStateChange;
  }
}
</script>

<style lang="scss">
.logoText{
color:white;
text-decoration: none;
}
</style>