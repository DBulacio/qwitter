<template>
    <q-page class="relative-position">
        <q-scroll-area class="absolute fullscreen">
        <!-- input qweet -->
        <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
            <div class="col">
                <q-input
                    class="new-qweet"
                    bottom-slots
                    autogrow
                    v-model="newQweetContent"
                    placeholder="What's happening?"
                    counter
                    maxlength="280"
                >
                    <template v-slot:before>
                        <q-avatar size="xl">
                            <img src="https://cdn.quasar.dev/img/avatar5.jpg">
                        </q-avatar>
                    </template>

                    <template v-slot:append>
                        <q-icon v-if="newQweetContent !== ''" name="close" @click="newQweetContent = ''" class="cursor-pointer" />
                    </template>
                </q-input>
            </div>
            <div class="col col-shrink">
                <q-btn
                    class="q-mb-lg"
                    :disable="!newQweetContent"
                    unelevated
                    rounded
                    color="primary"
                    label="Qweet" 
                    no-caps
                    @click="addNewQweet"
                />
            </div>
        </div>

        <q-separator
            class="divider"    
            size="10px"
            color="grey-2" 
        />

        <!-- nuevos qweets -->
        <q-list separator>
            <transition-group
                appear
                enter-active-class="animated fadeIn slow"
                leave-active-class="animated fadeOut slow"
            >
                <q-item
                    v-for="qweet in qweets"
                    :key="qweet.date"
                    class="q-py-md"
                >
                    <q-item-section avatar top>
                        <q-avatar size="xl">
                            <img src="https://cdn.quasar.dev/img/avatar5.jpg">
                        </q-avatar>
                    </q-item-section>

                    <q-item-section>
                        <q-item-label class="text-subtitle1">
                            <strong>Mindy Flower</strong>
                            <!-- date-fns para que muestre la fecha relativa -->
                            <span class="text-grey-7">
                                @mindy__flower 
                                <br class="lt-md"> &bull; {{ qweet.date }}
                            </span>
                        </q-item-label>
                        <q-item-label class="qweet-content text-body1">
                            {{qweet.content}}
                        </q-item-label>

                        <!-- botones de interacción -->
                        <div class="row justify-between q-mt-sm qweet-icons">
                            <q-btn
                                flat
                                round
                                size="sm"
                                color="grey"
                                icon="far fa-comment" 
                            />
                            <q-btn
                                flat
                                round
                                size="sm"
                                color="grey"
                                icon="fas fa-retweet" 
                            />
                            <q-btn
                                flat
                                round
                                size="sm"
                                color="grey"
                                icon="far fa-heart" 
                            />
                            <q-btn
                                flat
                                round
                                size="sm"
                                color="grey"
                                icon="fas fa-trash"
                                @click="deleteQweet(qweet)"
                            />
                        </div>
                    </q-item-section>
                </q-item>
            </transition-group>
        </q-list>
        </q-scroll-area>
    </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'PageHome',
    data() {
        return {
            newQweetContent: "",
            qweets: [
                {
                    id: 1,
                    content: 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Cumque voluptatem quaerat facilis! iquam cumque ipsum odio sit laudantium. Ratione corporis itaque aliquam atque molestiae ea aperiam, nihil ut tenetur distinctio?',
                    date: '10:22'
                },
                {
                    id: 2,
                    content: 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Cumque voluptatem quaerat facilis! iquam cumque ipsum odio sit laudantium. Ratione corporis itaque aliquam atque molestiae ea aperiam, nihil ut tenetur distinctio?',
                    date: '10:20'
                }
            ]
        }
    },
    methods: {
        addNewQweet() {
            let newQweet = {
                id: this.qweets.length + 1,
                content: this.newQweetContent,
                date: '10:24'
            }
            this.qweets.unshift(newQweet)
            this.newQweetContent = ''
        },
        deleteQweet(qweet) {
            let idToDelete = qweet.id
            let index = this.qweets.findIndex(qweet => qweet.id === idToDelete)
            this.qweets.splice(index, 1)
        }
    }
})
</script>

<style lang="sass">
.new-qweet 
    textarea
        font-size: 19px
        line-height: 1.4 !important

.divider
    border-top: 1px solid
    border-bottom: 1px solid
    border-color: $grey-4

.qweet-content
    white-space: pre-line 

.qweet-icons
    margin-left: -5px
</style>