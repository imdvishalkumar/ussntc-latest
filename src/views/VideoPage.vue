<template>
  <div :class="{ 'video-page': isVideoPage }">
    <div class="video-container" :style="{
      height: videoHeight + 'px',
      marginTop: headerHeight + 'px',
      marginBottom: footerHeight + 'px',
    }">
      <!-- Loader -->
      <div v-if="loading" class="video-loader">
        Loading video, please wait...
      </div>
      <!-- Video -->
      <video ref="videoPlayer" v-show="!loading" :muted="isMuted" playsinline webkit-playsinline :controls="isIOS"
        preload="metadata" class="fullscreen-video" @loadeddata="onVideoLoaded" @loadedmetadata="onVideoLoaded"
        @canplay="onVideoCanPlay" @click="handleVideoClick" @play="onPlay" @pause="onPause" @error="onVideoError">
        <source :src="videoSrc" type="video/mp4" />
        Your browser does not support the video tag.
      </video>
      <!-- Center Play/Pause Button (hidden on iOS) -->
      <button v-show="!loading && !isPlaying && !isIOS" class="center-play-btn" @click="togglePlayPause"
        :disabled="!videoReady">
        <svg width="64" height="64" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
          <circle cx="32" cy="32" r="32" fill="white" fill-opacity="0.1" />
          <polygon points="24,18 24,46 46,32" fill="white" />
        </svg>
      </button>
      <!-- Controls overlay (hidden on iOS when native controls are shown) -->
      <div v-show="!loading && !isIOS" class="video-controls">
        <button @click="toggleMute" class="control-btn flex items-center gap-2">
          <img :src="isMuted ? muteImage : soundImage" width="24" height="24" />
        </button>
        <button @click="openFullscreen" class="control-btn">⛶</button>
      </div>
      <!-- iOS Play Button Overlay -->
      <div v-if="isIOS" class="ios-play-overlay">
        <router-link class="btn shop-btn-ios" to="/shop">
          Join the Legacy
        </router-link>
      </div>
      <!-- Shop Button (visible only when video is playing) -->
      <!-- <router-link
       v-show="isPlaying && !loading" -->
      <div v-if="!isIOS">
        <router-link class="btn shop-btn" to="/shop">
          Join the Legacy
        </router-link>
      </div>
      <!-- <button class="shop-btn" @click="$router.push('/shop')">Shop</button> -->
    </div>
  </div>
</template>
<script>
import videoFile from "../assets/video/US-Soccer.mp4";
import muteImage from "../assets/images/mute.png";
import soundImage from "../assets/images/sound.png";
export default {
  name: "VideoPage",
  data() {
    return {
      videoSrc: videoFile,
      headerHeight: 0,
      footerHeight: 0,
      videoHeight: 0,
      loading: true,
      isMuted: false,
      isPlaying: false,
      videoReady: false,
      muteImage,
      soundImage,
      userInteracted: false,
    };
  },
  computed: {
    isVideoPage() {
      return this.$route.path === "/video";
    },
    isIOS() {
      return /iphone|ipad|ipod/i.test(navigator.userAgent);
    },
    isSafari() {
      return /^((?!chrome|android).)*safari/i.test(navigator.userAgent);
    },
  },
  methods: {
    updateHeights() {
      const header = document.querySelector("header");
      const footer = document.querySelector("footer");
      this.headerHeight = header?.offsetHeight || 0;
      this.footerHeight = footer?.offsetHeight || 0;
      this.videoHeight =
        window.innerHeight - this.headerHeight - this.footerHeight;
      document.body.style.paddingTop = this.isVideoPage
        ? "0px"
        : `${this.headerHeight}px`;
    },
    onVideoLoaded() {
      this.loading = false;
      this.videoReady = true;
    },
    onVideoCanPlay() {
      this.videoReady = true;
      if (this.loading) {
        this.loading = false;
      }
    },
    onVideoError(event) {
      console.error("Video error:", event);
      this.loading = false;
    },
    onPlay() {
      this.isPlaying = true;
    },
    onPause() {
      this.isPlaying = false;
    },
    handleVideoClick() {
      if (this.isIOS) {
        // Let iOS handle video clicks natively
        return;
      }
      this.togglePlayPause();
    },
    toggleMute() {
      this.isMuted = !this.isMuted;
      const video = this.$refs.videoPlayer;
      if (video) {
        video.muted = this.isMuted;
      }
    },
    openFullscreen() {
      const video = this.$refs.videoPlayer;
      if (!video) return;
      // For iOS, use the native fullscreen
      if (this.isIOS && video.webkitEnterFullscreen) {
        video.webkitEnterFullscreen();
      }
      // For other browsers
      else if (video.requestFullscreen) {
        video.requestFullscreen();
      } else if (video.webkitRequestFullscreen) {
        video.webkitRequestFullscreen();
      } else if (video.msRequestFullscreen) {
        video.msRequestFullscreen();
      } else if (video.mozRequestFullScreen) {
        video.mozRequestFullScreen();
      }
    },
    async togglePlayPause() {
      const video = this.$refs.videoPlayer;
      if (!video || !this.videoReady) return;
      try {
        if (video.paused) {
          // iOS requires user interaction for play
          await video.play();
          this.isPlaying = true;
          this.userInteracted = true;
        } else {
          video.pause();
          this.isPlaying = false;
        }
      } catch (error) {
        console.error("Play/pause error:", error);
        // Handle autoplay policy errors
        if (error.name === "NotAllowedError") {
          console.warn("Autoplay prevented. User interaction required.");
        }
      }
    },
    setupIOSVideoHandling() {
      const video = this.$refs.videoPlayer;
      if (!video || !this.isIOS) return;
      // Handle iOS-specific events
      video.addEventListener("webkitbeginfullscreen", () => {
        console.log("iOS fullscreen started");
      });
      video.addEventListener("webkitendfullscreen", () => {
        console.log("iOS fullscreen ended");
      });
      // Ensure proper iOS video handling
      video.setAttribute("playsinline", "");
      video.setAttribute("webkit-playsinline", "");
    },
  },
  mounted() {
    this.updateHeights();
    window.addEventListener("resize", this.updateHeights);
    // Setup iOS-specific handling
    this.$nextTick(() => {
      this.setupIOSVideoHandling();
    });
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.updateHeights);
    document.body.style.paddingTop = "";
  },
  watch: {
    "$route.path"() {
      this.updateHeights();
    },
  },
};
</script>
<style scoped>
.video-container {
  width: 100%;
  overflow: hidden !important;
  position: relative;
  background-color: black;
}
 
.fullscreen-video {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100%;
  height: 100%;
  object-fit: contain;
  transform: translate(-50%, -50%);
  overflow: hidden !important;
  -webkit-playsinline: true;
}
 
/* iOS specific video styles */
.fullscreen-video::-webkit-media-controls {
  display: none !important;
}
 
.fullscreen-video::-webkit-media-controls-enclosure {
  display: none !important;
}
 
/* Loader */
.video-loader {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 1.5rem;
  font-weight: bold;
  text-align: center;
}
 
/* Controls */
.video-controls {
  position: absolute;
  bottom: 20px;
  right: 20px;
  display: flex;
  gap: 0px;
  z-index: 10;
}
 
.control-btn {
  width: 45px;
  height: 45px;
  background: rgba(0, 0, 0, 0.4);
  border: none;
  border-radius: 50%;
  color: white;
  font-size: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s;
}
 
.control-btn:hover {
  background: rgba(0, 0, 0, 0.7);
}
 
/* Center Play Button */
.center-play-btn {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 60px;
  color: white;
  background: transparent;
  border: none;
  border-radius: 50%;
  width: 100px;
  height: 100px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.3s;
  z-index: 10;
}
 
.center-play-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
 
/* iOS Play Overlay */
.ios-play-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.0);
  /* transparent so native controls visible */
  z-index: 20;
  pointer-events: none;
  /* let video receive clicks */
}
 
.shop-btn-ios {
  pointer-events: auto;
  /* clickable */
  position: absolute;
  bottom: 35px;
  left: 50%;
  transform: translateX(-50%);
  cursor: pointer;
  text-decoration: none;
  z-index: 25;
}
 
.ios-play-button:hover {
  background: rgba(0, 0, 0, 0.7);
  transform: scale(1.1);
}
 
@media (max-width: 768px) {
  .fullscreen-video {
    object-fit: contain;
  }
 
  .center-play-btn {
    font-size: 40px;
    width: 80px;
    height: 80px;
  }
 
  .ios-play-button {
    padding: 15px;
  }
}
 
/* Additional iOS Safari specific fixes */
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  .fullscreen-video {
    -webkit-transform: translate(-50%, -50%);
  }
}
 
/* Shop Button */
.shop-btn {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  cursor: pointer;
  text-decoration: none;
  z-index: 20;
}
 
.shop-btn-ios {
  position: absolute;
  bottom: 35px;
  left: 50%;
  transform: translateX(-50%);
  cursor: pointer;
  text-decoration: none;
  z-index: 20;
}
</style>