<template>
    <div class="container">
        <SearchBar @termChange="onTermChange"></SearchBar>
        <VideoDetail v-bind:video="selectedVideo"/>
        <VideoList @videoSelect="onVideoSelect" :videos="videosData"></VideoList>
    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';
const API_KEY = 'AIzaSyAXtAiONBR9OXtJ5uulhzRYE7VgtmWDCHc'

export default {
    name: 'App',
    components: {
        SearchBar,
        VideoList,
        VideoDetail
    },
    data() {
        return { videosData: [], selectedVideo: null };
    },
    methods: {
        onVideoSelect(video) {
            this.selectedVideo = video;
        },
        onTermChange(searchTerm) {
            axios.get('https://www.googleapis.com/youtube/v3/search', {
                params: {
                    key: API_KEY,
                    type: 'video',
                    part: 'snippet',
                    q: searchTerm
                }
            })
            .then(response => {
                this.videosData = response.data.items
        });
    }
  }
}
</script>

<style scoped>
input {
    width: 75%;
}
div {
    text-align: center;
    margin: 20px;
}
</style>