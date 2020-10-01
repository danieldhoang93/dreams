<template>
  <section>
    <div class="fullPage text-white">
      <div class="pageContent pageWidth">
        <template>
          <q-card v-for="post in posts" v-bind:key="post.id" class="backgroundColor bottomBorder">
            <q-card-section>
              <div class="text-h6 q-mb-xs">{{ post.title }}</div>
              <div class="text-caption text-grey">
                {{ post.body }}
              </div>
            </q-card-section>
            <q-card-actions>
              <q-btn round 
              flat 
              color="grey-6" 
              icon="arrow_drop_up" 
              @click="like(post)"/>
              <div>{{ post.likes }}</div>
              <q-btn round 
              flat 
              color="grey-6" 
              icon="arrow_drop_down"
              @click="unlike(post)"/>

              <q-space />
            </q-card-actions>
          </q-card>
        </template>

        
      </div>
    </div>
  </section>
</template>

<script>
import { API } from 'aws-amplify';
export default {
  name: 'Home',
  data () {
    return {
      newPost: '',
      expanded: false,
      totalLikes: 0,
      posts: []
    }
  },
  methods: {
    like(post) {
      API.put('dreamsApi', '/dreams', {
        body: {
          id: post.id,
          title: post.title,
          body: post.body,
          createdAt: post.createdAt,
          likes:post.likes + 1
        }
      }).then (result => {
        post.likes++;
        console.log(result);
      }).catch(error => {
        console.log(error);
      })
    },
    unlike(post) {
      if (post.likes > 0) {
        API.put('dreamsApi', '/dreams', {
        body: {
          id: post.id,
          title: post.title,
          body: post.body,
          createdAt: post.createdAt,
          likes:post.likes - 1
        }
        }).then (result => {
          post.likes--;
          console.log(result);
        }).catch(error => {
          console.log(error);
        })
      }
    }
  },
  created() {
      API.get('dreamsApi', '/dreams', {}).then(result => {
        this.posts = result;
      }).catch(err => {
        console.log(err);
      })
  }
}
</script>

<style lang="scss">

</style>
