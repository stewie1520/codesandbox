<template>
  <v-card class="post">
    <v-card-title>
      <v-list-item-avatar color="grey darken-3">
        <v-img v-if="!!avatar"
          class="elevation-6"
          :src="avatar"
        ></v-img>
        <v-avatar :color="getAvatarColor()" size="48" v-else>
          <span class="white--text subtitle-1">{{ abbrName }}</span>
        </v-avatar>
      </v-list-item-avatar>
      <v-list-item-content>
        <v-list-item-title>{{ author }}</v-list-item-title>
        <v-list-item-subtitle class="post-since">{{ postSince }}</v-list-item-subtitle>
      </v-list-item-content>
      <v-row
        align="start"
        justify="end">
        <v-icon class="mr-1">mdi-dots-horizontal</v-icon>
      </v-row>
    </v-card-title>

    <v-card-text class="headline">
      {{ content.split('').splice(0, 197).join('') + '...' }}
    </v-card-text>

    <v-card-actions>
      <v-list-item class="grow">
        <v-row
          align="center"
          justify="start">
          <v-btn icon>
            <v-icon class="mr-1">mdi-thumb-up</v-icon>
          </v-btn>
          <span class="subheading mr-2">{{ upvote }}</span>
          <span class="mr-1">·</span>
          <v-btn icon>
            <v-icon class="mr-1">mdi-thumb-down</v-icon>
          </v-btn>
          <span class="subheading mr-2">{{ downvote }}</span>
          <span class="mr-1">·</span>
          <v-btn icon>
            <v-icon class="mr-1">mdi-chat-processing</v-icon>
          </v-btn>
          <span class="subheading">{{ commentsNumber }}</span>
        </v-row>
      </v-list-item>
    </v-card-actions>
  </v-card>
</template>

<script>
import moment from '@/util/moment'

const colors = ['purple', 'teal', 'blue', 'red', 'pink', 'yellow', 'indigo', 'cyan', 'green']

export default {
  data () {
    return {
      abbrName: '',
      postSince: ''
    }
  },
  props: {
    author: {
      type: String,
      required: true
    },
    avatar: {
      type: String,
      default: ''
    },
    date: {
      type: Date,
      required: true
    },
    content: {
      type: String,
      required: true
    },
    upvote: {
      type: Number,
      required: true
    },
    downvote: {
      type: Number,
      required: true
    },
    commentsNumber: {
      type: Number,
      required: true
    }
  },
  created () {
    this.abbrName = this.getAbbrNames().join('')
    this.postSince = this.getPostSince()
  },
  methods: {
    getAvatarColor () {
      const sum = this.getAbbrNames().reduce((sum, char) => {
        sum += char.charCodeAt(0)
        return sum
      }, 0)
      return colors[sum % colors.length]
    },
    getAbbrNames () {
      const abbr = []
      const words = this.author.split(/[ ,]+/)
      words.forEach((word, i) => {
        if (i < 2) abbr.push(word[0])
      })
      return abbr
    },
    getPostSince () {
      return moment().from(this.date)
    }
  }
}
</script>
