<template>
  <section>
    <div class="fullPage text-white backgroundColor">
      <div class="pageContent pageWidth q-py-md">
        <template>
          <q-card dark  v-for="post in posts" v-bind:key="post.id" class=" cardBackground">
            <q-card-section>
              <div class="text-h5 q-mt-sm q-mb-xs">{{ post.title }}</div>
              <div class="text-caption text-grey">
                {{ post.body }}
              </div>
            </q-card-section>

            <q-slide-transition>
              <div v-show="expanded">
                <q-separator />
                <q-card-section class="text-subitle2">
                  {{ lorem }}
                </q-card-section>
              </div>
            </q-slide-transition>

            <q-card-actions>
              <q-btn round flat color="grey-6" icon="arrow_drop_up"/>
              <div>{{ post.totalLikes.length }}</div>
              <q-btn round flat color="grey-6" icon="arrow_drop_down"/>

              <q-space />

              <q-btn
                color="grey"
                round
                flat
                dense
                :icon="expanded ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
                @click="expanded = !expanded"
              />
            </q-card-actions>

            
          </q-card>
        </template>

        <q-footer elevated>
          <q-toolbar>
            <q-form @submit="sendMessage" class="pageWidth">
              <q-input dark filled bordered v-model="newPost" dense autogrow placeholder="Share your dreams..." >
                <q-btn 
                round 
                dense 
                flat 
                icon="send" 
                color="white"
                type="submit"/>
              </q-input>
            </q-form>
          </q-toolbar>
        </q-footer>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Home',
  data () {
        return {
          newPost: '',
          expanded: false,
          lorem: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.',
          posts: [
            {
              id: 1,
              title: 'title',
              body: `Luckily friends do ashamed to do suppose. Tried meant mr smile so. Exquisite behaviour as to middleton perfectly. Chicken no wishing waiting am. Say concerns dwelling graceful six humoured. Whether mr up savings talking an. Active mutual nor father mother exeter change six did all. 
                Bed sincerity yet therefore forfeited his certainty neglected questions. Pursuit chamber as elderly amongst on. Distant however warrant farther to of. My justice wishing prudent waiting in be. Comparison age not pianoforte increasing delightful now. Insipidity sufficient dispatched any reasonably led ask. Announcing if attachment resolution sentiments admiration me on diminution. 
                Supported neglected met she therefore unwilling discovery remainder. Way sentiments two indulgence uncommonly own. Diminution to frequently sentiments he connection continuing indulgence. An my exquisite conveying up defective. Shameless see the tolerably how continued. She enable men twenty elinor points appear. Whose merry ten yet was men seven ought balls. `,
              totalLikes: [
                {
                userId: 1
                },
                {
                userId: 2
                }
              ],
              liked: 'true'
            },
            {
              id: 2,
              title: 'gaaaaaaah',
              body: `On it differed repeated wandered required in. Then girl neat why yet knew rose spot. Moreover property we he kindness greatest be oh striking laughter. In me he at collecting affronting principles apartments. Has visitor law attacks pretend you calling own excited painted. Contented attending smallness it oh ye unwilling. Turned favour man two but lovers. Suffer should if waited common person little oh. Improved civility graceful sex few smallest screened settling. Likely active her warmly has. 
                His followed carriage proposal entrance directly had elegance. Greater for cottage gay parties natural. Remaining he furniture on he discourse suspected perpetual. Power dried her taken place day ought the. Four and our ham west miss. Education shameless who middleton agreement how. We in found world chief is at means weeks smile. 
                Marianne or husbands if at stronger ye. Considered is as middletons uncommonly. Promotion perfectly ye consisted so. His chatty dining for effect ladies active. Equally journey wishing not several behaved chapter she two sir. Deficient procuring favourite extensive you two. Yet diminution she impossible understood age.`,
              totalLikes: [
                {
                userId: 1
                },
                {
                userId: 2
                },
                {
                userId: 3
                }
              ],
              liked: 'false'
            },
            {
              id: 3,
              title: 'something stupid',
              body: `With my them if up many. Lain week nay she them her she. Extremity so attending objection as engrossed gentleman something. Instantly gentleman contained belonging exquisite now direction she ham. West room at sent if year. Numerous indulged distance old law you. Total state as merit court green decay he. Steepest sex bachelor the may delicate its yourself. As he instantly on discovery concluded to. Open draw far pure miss felt say yet few sigh. 
                Am if number no up period regard sudden better. Decisively surrounded all admiration and not you. Out particular sympathize not favourable introduced insipidity but ham. Rather number can and set praise. Distrusts an it contented perceived attending oh. Thoroughly estimating introduced stimulated why but motionless. 
                He difficult contented we determine ourselves me am earnestly. Hour no find it park. Eat welcomed any husbands moderate. Led was misery played waited almost cousin living. Of intention contained is by middleton am. Principles fat stimulated uncommonly considered set especially prosperous. Sons at park mr meet as fact like. `,
              totalLikes: [
                {
                userId: 1
                },
                {
                userId: 2
                },
                {
                userId: 3
                },
                {
                userId: 4
                }
              ],
              liked: ''
            }
          ]
        }
  },
  methods: {
    like() {
      this.posts.liked = "true";
      console.log(this.posts.liked);
    }
  }
}
</script>

<style lang="scss">

</style>
