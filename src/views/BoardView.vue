<template>
  <v-container class="grey lighten-5" style="margin-top: 40px;">
    <v-row no-gutters style="margin-bottom: 10px; text-align: center; font-weight: 600;">
      <v-col cols="2" sm="6" md="2">
        <v-card class="pa-2" outlined tile>제목</v-card>
      </v-col>
      <v-col cols="8" md="6">
        <v-card class="pa-2" outlined tile>내용</v-card>
      </v-col>
      <v-col cols="2" sm="6" md="2">
        <v-card class="pa-2" outlined tile>작성자</v-card>
      </v-col>
      <v-col cols="2" sm="6" md="2">
        <v-card class="pa-2" outlined tile>시간</v-card>
      </v-col>
    </v-row>
    <v-row no-gutters v-for="(item, key) in boardList" :key="key">
      <v-col cols="2" sm="6" md="2">
        <v-card class="pa-2" outlined tile>{{item.title}}</v-card>
      </v-col>
      <v-col cols="8" md="6">
        <v-card class="pa-2" outlined tile>{{item.content}}</v-card>
      </v-col>
      <v-col cols="2" sm="6" md="2">
        <v-card class="pa-2" outlined tile>{{item.writer}}</v-card>
      </v-col>
      <v-col cols="2" sm="6" md="2">
        <v-card class="pa-2" outlined tile>{{item.time}}</v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import axios from 'axios';
  export default {
    data: () => ({
      boardList: [
        {
          title: '제목1',
          content: '내용1',
          writer: '작성자',
          time: '시간',
        },
        {
          title: '제목2',
          content: '내용2',
          writer: '작성자',
          time: '시간',
        },
        {
          title: '제목3',
          content: '내용3',
          writer: '작성자',
          time: '시간',
        },
        {
          title: '제목4',
          content: '내용4',
          writer: '작성자',
          time: '시간',
        },
      ]
    }),
    mounted() {
      this.init();
    },
    methods: {
      async init() {
        const response = await axios.get('/api/board');
        this.boardList = response.data.documents;
      }
    }
  }
</script>