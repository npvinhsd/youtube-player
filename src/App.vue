<template>
    <div>
        <YoutubePlayer ref="player" youtube-id="jaPBmq0sUsU"/>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import YoutubePlayer from './components/YoutubePlayer.vue';

    @Component({
        components: {
            YoutubePlayer,
        },
    })
    export default class App extends Vue {
        created() {
            setInterval(() => {
                const player: YoutubePlayer = this.$refs.player as YoutubePlayer;
                player.seekTo(player.getCurrentTime() + 20, true);
            }, 5000);
        }
    }
</script>

<style lang="stylus">

</style>
