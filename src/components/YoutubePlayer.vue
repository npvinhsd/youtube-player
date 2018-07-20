<template>
    <div>
        <div id="player"></div>
    </div>
</template>

<script lang='ts'>
    import Vue from 'vue';
    import {Component, Prop} from 'vue-property-decorator';

    export enum YoutubePlayerState {
        UNSTARTED = -1,
        ENDED = 0,
        PLAYING = 1,
        PAUSED = 2,
        BUFFERING = 3,
        VIDEO_CUED = 5,
    }

    export enum YoutubePlayerError {
        INVALID_PARAMEMTER = 2,
        HTML5_ERROR = 5,
        VIDEO_NOT_FOUND = 100,
        EMMBED_VIDEO_NOT_ALLOWED = 101,
        EMMBED_VIDEO_NOT_ALLOWED_2 = 150,
    }

    export enum YoutubePlaybackQuantity {
        HIGHRES = 'highres',
        HD1080 = 'hd1080',
        HD720 = 'hd720',
        LARGE = 'large',
        MEDIUM = 'medium',
        SMALL = 'small',
    }

    @Component({})
    export default class YoutubePlayer extends Vue {
        @Prop({
            required: true,
            type: String,
        }) youtubeId!: string;
        private player: any = null;

        // ------Life cycle -----
        public created() {
            // This code loads the IFrame Player API code asynchronously.
            const tag = document.createElement('script') as HTMLScriptElement;
            tag.src = 'https://www.youtube.com/iframe_api';

            const firstScriptTag = document.getElementsByTagName('script')[0];

            firstScriptTag.parentNode!.insertBefore(tag, firstScriptTag);

            (window as any).onYouTubeIframeAPIReady = () => {
                this.player = new (window as any).YT.Player('player', {
                    videoId: this.youtubeId,
                    events: {
                        onReady: this.onPlayerReady,
                        onStateChange: this.onPlayerStateChange,
                        onPlaybackQualityChange: this.onPlaybackQualityChange,
                        onPlaybackRateChange: this.onPlaybackRateChange,
                        onError: this.onError,
                    },
                });
            };
        }

        // ------ End Section -----

        // ----- Playback Event
        public onPlayerReady(event: any) {
            this.$emit('onPlayerReady');
        }

        public onPlayerStateChange(event: any) {
            this.$emit('onPlayerStateChange', event.data);
        }

        public onPlaybackQualityChange(event: any) {
            this.$emit('onPlaybackQualityChange', event.data);
        }

        public onPlaybackRateChange(event: any) {
            this.$emit('onPlaybackRateChange', event.data);
        }

        public onError(event: any) {
            this.$emit('onError', event);
        }

        // ------ End Section -----

        // ------ Playback Control --
        public playVideo() {
            this.player.playVideo();
        }

        public stopVideo() {
            this.player.stopVideo();
        }

        public pauseVideo() {
            this.player.pauseVideo();
        }

        public seekTo(seconds: number, play: boolean = true) {
            this.player.seekTo(seconds, true);
            if (play) {
                this.playVideo();
            } else {
                this.pauseVideo();
            }
        }

        public getCurrentTime(): number {
            return this.player.getCurrentTime();
        }

        // ------ End Section -----
    }
</script>

<style lang='stylus'>

</style>