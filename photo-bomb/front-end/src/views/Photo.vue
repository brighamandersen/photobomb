<template>
<div class="photo" v-if="photo.user">
  <img :src="photo.path" class="pic"/>
  <h1>{{ photo.title }}</h1>
  <p>{{ photo.description }}</p>
  <h2>Uploaded {{ formatDate(photo.created )}} by <i>{{ photo.user.firstName }} {{ photo.user.lastName }}</i> </h2>
</div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';

export default {
  name: 'Photo',
  data() {
    return {
      photo: {},
      error: '',
    }
  },
  created() {
    this.getPhoto();
  },
  methods: {
    async getPhoto() {
      try {
        let response = await axios.get("/api/photos/" + this.$route.params.id);
        this.photo = response.data;
      } catch (error) {
        this.error = error.response.data.message;
      }
    },
    formatDate(date) {
      if (moment(date).diff(Date.now(), 'days') < 15)
        return moment(date).fromNow();
      else
        return moment(date).format('d MMMM YYYY');
    }
  }

}
</script>

<style scoped>
.photo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pic {
  max-width: 500px;
  max-height: 60vh;
  padding: 2rem;
}

h1 {
  color: black;
}

.error {
  color: red;
}
</style>