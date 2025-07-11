<template>
  <div class="personal-website">
    <!-- 头部问候和时间模块 -->
    <header class="welcome-section">
      <div class="time-display">
        <h1 class="greeting">{{ greeting }}！</h1>
        <div class="time-info">
          <div class="current-time">{{ formattedTime }}</div>
          <div class="current-date">{{ formattedDate }}</div>
        </div>
      </div>
    </header>

    <!-- 主要内容区域 -->
    <main class="main-content">
      <!-- 左侧：常用链接模块 -->
      <section class="links-section">
        <h2 class="section-title">常用网址</h2>
        <div class="links-container">
          <div
            v-for="(links, category) in linksByCategory"
            :key="category"
            class="category-group"
          >
            <h3 class="category-title">{{ category }}</h3>
            <div class="links-grid">
              <a
                v-for="link in links"
                :key="link.name"
                :href="link.url"
                target="_blank"
                rel="noopener noreferrer"
                class="link-card"
              >
                <div class="link-icon">{{ link.icon }}</div>
                <div class="link-info">
                  <h4 class="link-name">{{ link.name }}</h4>
                  <p class="link-description">{{ link.description }}</p>
                </div>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- 右侧：音乐播放模块 -->
      <section class="music-section">
        <h2 class="section-title">音乐播放器</h2>
        <div class="music-player">
          <div class="song-info">
            <div class="album-art">🎵</div>
            <div class="song-details">
              <h3 class="song-title">
                {{ currentSongInfo?.title || "未选择歌曲" }}
              </h3>
              <p class="song-artist">{{ currentSongInfo?.artist || "" }}</p>
            </div>
          </div>

          <div class="player-controls">
            <button
              @click="prevSong"
              class="control-btn"
            >
              ⏮️
            </button>
            <button
              @click="togglePlay"
              class="play-btn"
            >
              {{ isPlaying ? "⏸️" : "▶️" }}
            </button>
            <button
              @click="nextSong"
              class="control-btn"
            >
              ⏭️
            </button>
          </div>

          <div class="progress-section">
            <span class="time-current">
              {{
                currentSongInfo?.type === "radio"
                  ? "LIVE"
                  : formatTime(progress)
              }}
            </span>
            <div
              class="progress-bar"
              @click="setProgress"
              :class="{ disabled: currentSongInfo?.type === 'radio' }"
            >
              <div
                class="progress-fill"
                :style="{
                  width:
                    currentSongInfo?.type === 'radio'
                      ? '100%'
                      : duration
                        ? (progress / duration) * 100 + '%'
                        : '0%',
                }"
                :class="{ live: currentSongInfo?.type === 'radio' }"
              ></div>
            </div>
            <span class="time-total">
              {{
                currentSongInfo?.type === "radio"
                  ? "LIVE"
                  : formatTime(duration)
              }}
            </span>
          </div>

          <div class="volume-section">
            <span class="volume-icon">🔊</span>
            <input
              type="range"
              min="0"
              max="100"
              :value="volume"
              @input="setVolume"
              class="volume-slider"
            />
            <span class="volume-value">{{ volume }}%</span>
          </div>

          <div class="playlist">
            <h4 class="playlist-title">播放列表</h4>
            <div class="playlist-items">
              <div
                v-for="(song, index) in playlist"
                :key="index"
                @click="currentSongIndex = index"
                class="playlist-item"
                :class="{ active: index === currentSongIndex }"
              >
                <span class="song-number">{{ index + 1 }}</span>
                <div class="playlist-song-info">
                  <span class="playlist-song-title">
                    {{ song.title }}
                    <span
                      v-if="song.type === 'radio'"
                      class="live-badge"
                    >
                      LIVE
                    </span>
                  </span>
                  <span class="playlist-song-artist">{{ song.artist }}</span>
                </div>
                <span class="song-duration">{{ song.duration }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "MyWebsite",
  data() {
    return {
      currentTime: new Date(),
      greeting: "",
      timeTimer: null,
      isPlaying: false,
      currentSong: null,
      volume: 50,
      progress: 0,
      duration: 0,
      currentSongIndex: 0,
      favoriteLinks: [
        {
          name: "GitHub",
          url: "https://github.com",
          description: "代码托管平台",
          icon: "🐙",
          category: "开发",
        },
        {
          name: "Stack Overflow",
          url: "https://stackoverflow.com",
          description: "程序员问答社区",
          icon: "📚",
          category: "开发",
        },
        {
          name: "Bing",
          url: "https://bing.com",
          description: "微软搜索引擎",
          icon: "🌐",
          category: "工具",
        },
        {
          name: "IT-Tools",
          url: "https://it-tools.counhopig.top",
          description: "IT工具集合",
          icon: "🛠️",
          category: "工具",
        },
        {
          name: "PDF Tools",
          url: "https://pdf.counhopig.top",
          description: "PDF工具集合",
          icon: "📄",
          category: "工具",
        },
        {
          name: "YouTube",
          url: "https://youtube.com",
          description: "视频分享平台",
          icon: "📺",
          category: "娱乐",
        },
        {
          name: "Spotify",
          url: "https://spotify.com",
          description: "音乐流媒体",
          icon: "🎵",
          category: "音乐",
        },
        {
          name: "Bilibili",
          url: "https://bilibili.com",
          description: "弹幕视频网站",
          icon: "📹",
          category: "娱乐",
        },
      ],
      playlist: [
        // 环境音乐
        {
          title: "Drone Zone",
          artist: "SomaFM",
          duration: "Live",
          url: "https://somafm.com/dronezone/",
          type: "radio",
          streamUrl: "https://ice1.somafm.com/dronezone-128-mp3",
        },
        {
          title: "Lush",
          artist: "SomaFM",
          duration: "Live",
          url: "https://somafm.com/lush/",
          type: "radio",
          streamUrl: "https://ice1.somafm.com/lush-128-mp3",
        },
        {
          title: "Deep Space One",
          artist: "SomaFM",
          duration: "Live",
          url: "https://somafm.com/deepspaceone/",
          type: "radio",
          streamUrl: "https://ice1.somafm.com/deepspaceone-128-mp3",
        },
        // 爵士乐
        {
          title: "Groove Salad",
          artist: "SomaFM",
          duration: "Live",
          url: "https://somafm.com/groovesalad/",
          type: "radio",
          streamUrl: "https://ice1.somafm.com/groovesalad-128-mp3",
        },
        // 古典音乐
        {
          title: "WQXR Classical",
          artist: "WQXR",
          duration: "Live",
          url: "https://www.wqxr.org/",
          type: "radio",
          streamUrl: "https://stream.wqxr.org/wqxr",
        },
        // 轻松电子乐
        {
          title: "Beat Blender",
          artist: "SomaFM",
          duration: "Live",
          url: "https://somafm.com/beatblender/",
          type: "radio",
          streamUrl: "https://ice1.somafm.com/beatblender-128-mp3",
        },
      ],
    };
  },
  computed: {
    formattedTime() {
      return this.currentTime.toLocaleTimeString("zh-CN", {
        hour12: false,
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
      });
    },
    formattedDate() {
      return this.currentTime.toLocaleDateString("zh-CN", {
        year: "numeric",
        month: "long",
        day: "numeric",
        weekday: "long",
      });
    },
    linksByCategory() {
      const categories = {};
      this.favoriteLinks.forEach((link) => {
        if (!categories[link.category]) {
          categories[link.category] = [];
        }
        categories[link.category].push(link);
      });
      return categories;
    },
    currentSongInfo() {
      return this.playlist[this.currentSongIndex] || null;
    },
  },
  mounted() {
    this.updateTime();
    this.updateGreeting();
    this.timeTimer = setInterval(() => {
      this.updateTime();
      this.updateGreeting();
    }, 1000);
  },
  beforeUnmount() {
    if (this.timeTimer) {
      clearInterval(this.timeTimer);
    }
    if (this.currentSong) {
      this.currentSong.pause();
    }
  },
  methods: {
    updateTime() {
      this.currentTime = new Date();
    },
    updateGreeting() {
      const hour = this.currentTime.getHours();
      if (hour < 6) {
        this.greeting = "深夜好";
      } else if (hour < 12) {
        this.greeting = "早上好";
      } else if (hour < 18) {
        this.greeting = "下午好";
      } else {
        this.greeting = "晚上好";
      }
    },
    togglePlay() {
      if (this.isPlaying) {
        this.pauseMusic();
      } else {
        this.playMusic();
      }
    },
    playMusic() {
      const currentSong = this.currentSongInfo;
      if (!currentSong) return;

      // 停止当前播放的音频
      if (this.currentSong) {
        this.currentSong.pause();
        this.currentSong = null;
      }

      // 创建新的音频实例
      if (currentSong.type === "radio") {
        // 对于电台，使用流媒体URL
        this.currentSong = new Audio();
        this.currentSong.src = currentSong.streamUrl;
        this.currentSong.crossOrigin = "anonymous";

        // 电台是直播流，没有固定时长
        this.duration = 0;
        this.progress = 0;
      } else {
        // 对于普通音频文件
        this.currentSong = new Audio(currentSong.url);
      }

      // 设置音量
      this.currentSong.volume = this.volume / 100;

      // 添加事件监听器
      this.currentSong.addEventListener("loadedmetadata", () => {
        if (currentSong.type !== "radio") {
          this.duration = this.currentSong.duration;
        }
      });

      this.currentSong.addEventListener("timeupdate", () => {
        if (currentSong.type !== "radio") {
          this.progress = this.currentSong.currentTime;
        }
      });

      this.currentSong.addEventListener("ended", () => {
        if (currentSong.type !== "radio") {
          this.nextSong();
        }
      });

      this.currentSong.addEventListener("error", (e) => {
        console.error("音频播放错误:", e);
        this.isPlaying = false;
      });

      // 开始播放
      this.currentSong
        .play()
        .then(() => {
          this.isPlaying = true;
          console.log(`播放: ${currentSong.title}`);
        })
        .catch((error) => {
          console.error("播放失败:", error);
          this.isPlaying = false;
        });
    },
    pauseMusic() {
      if (this.currentSong) {
        this.currentSong.pause();
      }
      this.isPlaying = false;
      console.log("暂停播放");
    },
    nextSong() {
      this.currentSongIndex =
        (this.currentSongIndex + 1) % this.playlist.length;
      if (this.isPlaying) {
        this.playMusic();
      }
    },
    prevSong() {
      this.currentSongIndex =
        this.currentSongIndex === 0
          ? this.playlist.length - 1
          : this.currentSongIndex - 1;
      if (this.isPlaying) {
        this.playMusic();
      }
    },
    formatTime(seconds) {
      if (!seconds || seconds === 0) return "0:00";
      const mins = Math.floor(seconds / 60);
      const secs = Math.floor(seconds % 60);
      return `${mins}:${secs.toString().padStart(2, "0")}`;
    },
    setVolume(event) {
      this.volume = event.target.value;
      if (this.currentSong) {
        this.currentSong.volume = this.volume / 100;
      }
    },
    setProgress(event) {
      if (!this.currentSong || this.currentSongInfo?.type === "radio") {
        return; // 电台不支持进度调整
      }

      const rect = event.target.getBoundingClientRect();
      const percent = (event.clientX - rect.left) / rect.width;
      const newTime = percent * this.duration;

      this.currentSong.currentTime = newTime;
      this.progress = newTime;
    },
  },
};
</script>

<style scoped>
/* 全局重置，确保无滚动 */
* {
  box-sizing: border-box;
}

html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden !important;
}

.personal-website {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 0.6rem;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  padding-top: 2vh;
  overflow: hidden;
  box-sizing: border-box;
  position: relative;
}

/* 头部时间显示模块 */
.welcome-section {
  text-align: center;
  margin-bottom: 1rem;
  flex-shrink: 0;
}

.time-display {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 0.8rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  box-sizing: border-box;
}

.greeting {
  font-size: 1.4rem;
  font-weight: bold;
  color: var(--ctp-mocha-pink);
  margin: 0;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.time-info {
  text-align: right;
}

.current-time {
  font-size: 1.6rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  font-family: "Courier New", monospace;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  line-height: 1;
}

.current-date {
  font-size: 0.8rem;
  color: var(--ctp-mocha-subtext1);
  margin-top: 0.1rem;
}

/* 主要内容区域 */
.main-content {
  display: grid;
  grid-template-columns: 1fr 360px;
  gap: 0.8rem;
  flex: 1;
  min-height: 0;
  overflow: hidden;
  max-height: calc(100vh - 140px);
}

.section-title {
  font-size: 1.2rem;
  font-weight: bold;
  color: var(--ctp-mocha-mauve);
  margin-bottom: 0.6rem;
  text-align: center;
}

/* 常用链接模块 */
.links-section {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 1rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
  display: flex;
  flex-direction: column;
  min-height: 0;
  overflow: hidden;
  height: 100%;
}

.links-container {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  flex: 1;
  overflow-y: auto;
  min-height: 0;
  max-height: calc(100% - 60px);
}

.category-group {
  margin-bottom: 0.6rem;
}

.category-title {
  font-size: 0.95rem;
  font-weight: bold;
  color: var(--ctp-mocha-blue);
  margin-bottom: 0.5rem;
  text-align: left;
}

.links-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 0.5rem;
}

.link-card {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  padding: 0.6rem;
  text-decoration: none;
  color: inherit;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  min-height: 45px;
}

.link-card:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

.link-icon {
  font-size: 1.5rem;
  margin-right: 0.8rem;
  flex-shrink: 0;
}

.link-info {
  flex: 1;
  min-width: 0;
}

.link-name {
  font-size: 1rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  margin: 0 0 0.2rem 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.link-description {
  font-size: 0.8rem;
  color: var(--ctp-mocha-subtext0);
  margin: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* 音乐播放模块 */
.music-section {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 1rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
  display: flex;
  flex-direction: column;
  min-height: 0;
  overflow: hidden;
  height: 100%;
}

.music-player {
  display: flex;
  flex-direction: column;
  height: 100%;
  min-height: 0;
  max-height: calc(100% - 60px);
}

.song-info {
  display: flex;
  align-items: center;
  margin-bottom: 0.8rem;
  text-align: left;
}

.album-art {
  font-size: 1.8rem;
  margin-right: 0.7rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.song-details {
  flex: 1;
  min-width: 0;
}

.song-title {
  font-size: 1rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  margin: 0 0 0.1rem 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.song-artist {
  color: var(--ctp-mocha-subtext1);
  margin: 0;
  font-size: 0.85rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.player-controls {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.7rem;
  margin-bottom: 0.8rem;
}

.control-btn,
.play-btn {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 50%;
  width: 35px;
  height: 35px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1.1rem;
}

.play-btn {
  width: 45px;
  height: 45px;
  background: var(--ctp-mocha-pink);
  font-size: 1.4rem;
}

.control-btn:hover,
.play-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.play-btn:hover {
  background: var(--ctp-mocha-mauve);
}

.progress-section {
  display: flex;
  align-items: center;
  gap: 0.7rem;
  margin-bottom: 0.8rem;
  font-size: 0.75rem;
  color: var(--ctp-mocha-subtext1);
}

.progress-bar {
  flex: 1;
  height: 3px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 2px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.progress-bar.disabled {
  cursor: not-allowed;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(
    90deg,
    var(--ctp-mocha-pink),
    var(--ctp-mocha-mauve)
  );
  border-radius: 2px;
  transition: width 0.3s ease;
}

.progress-fill.live {
  background: linear-gradient(
    90deg,
    var(--ctp-mocha-red),
    var(--ctp-mocha-pink)
  );
  animation: live-pulse 2s ease-in-out infinite alternate;
}

@keyframes live-pulse {
  0% {
    opacity: 0.6;
  }
  100% {
    opacity: 1;
  }
}

.volume-section {
  display: flex;
  align-items: center;
  gap: 0.7rem;
  margin-bottom: 0.8rem;
  font-size: 0.75rem;
}

.volume-slider {
  flex: 1;
  height: 2px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 1px;
  outline: none;
  appearance: none;
}

.volume-slider::-webkit-slider-thumb {
  appearance: none;
  width: 10px;
  height: 10px;
  background: var(--ctp-mocha-pink);
  border-radius: 50%;
  cursor: pointer;
}

.volume-slider::-moz-range-thumb {
  width: 10px;
  height: 10px;
  background: var(--ctp-mocha-pink);
  border-radius: 50%;
  cursor: pointer;
  border: none;
}

.playlist {
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding-top: 0.8rem;
  flex: 1;
  min-height: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.playlist-title {
  font-size: 0.95rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  margin-bottom: 0.6rem;
  text-align: left;
}

.playlist-items {
  flex: 1;
  overflow-y: auto;
  min-height: 0;
}

.playlist-item {
  display: flex;
  align-items: center;
  padding: 0.5rem;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-bottom: 0.2rem;
}

.playlist-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.playlist-item.active {
  background: rgba(245, 194, 231, 0.2);
  border-left: 3px solid var(--ctp-mocha-pink);
}

.song-number {
  font-size: 0.8rem;
  color: var(--ctp-mocha-subtext0);
  margin-right: 0.8rem;
  width: 16px;
  text-align: center;
  flex-shrink: 0;
}

.playlist-song-info {
  flex: 1;
  text-align: left;
  min-width: 0;
}

.playlist-song-title {
  display: block;
  font-weight: 500;
  color: var(--ctp-mocha-text);
  font-size: 0.8rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.live-badge {
  font-size: 0.65rem;
  color: var(--ctp-mocha-red);
  margin-left: 0.3rem;
  animation: live-blink 1.5s ease-in-out infinite;
}

@keyframes live-blink {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

.playlist-song-artist {
  display: block;
  color: var(--ctp-mocha-subtext1);
  font-size: 0.7rem;
  margin-top: 0.1rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.song-duration {
  font-size: 0.7rem;
  color: var(--ctp-mocha-subtext0);
  flex-shrink: 0;
}

/* 响应式设计 */
@media (max-width: 1200px) {
  .main-content {
    grid-template-columns: 1fr;
    grid-template-rows: 1fr 400px;
    max-height: calc(100vh - 120px);
  }

  .music-section {
    min-height: 400px;
  }
}

@media (max-width: 768px) {
  .personal-website {
    padding: 0.8rem;
    padding-top: 1vh;
  }

  .main-content {
    max-height: calc(100vh - 140px);
  }

  .time-display {
    flex-direction: column;
    text-align: center;
    gap: 0.5rem;
  }

  .greeting {
    font-size: 1.5rem;
  }

  .current-time {
    font-size: 1.8rem;
  }

  .main-content {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .links-grid {
    grid-template-columns: 1fr;
  }

  .link-card {
    padding: 0.6rem;
  }

  .time-display {
    padding: 1rem;
  }
}

@media (max-width: 480px) {
  .personal-website {
    padding-top: 0.5vh;
  }

  .greeting {
    font-size: 1.3rem;
  }

  .current-time {
    font-size: 1.5rem;
  }

  .section-title {
    font-size: 1.2rem;
  }

  .time-display {
    padding: 0.8rem;
  }

  .main-content {
    max-height: calc(100vh - 120px);
  }
}

/* 垂直居中的优化样式 */
@media (min-height: 800px) {
  .personal-website {
    justify-content: center;
    padding-top: 0.6rem;
  }

  .main-content {
    max-height: calc(90vh - 140px);
  }
}

@media (min-height: 1000px) {
  .personal-website {
    justify-content: center;
    padding-top: 0.6rem;
  }

  .main-content {
    max-height: calc(85vh - 140px);
  }
}

/* 确保页面不会超出viewport高度 */
@media (min-width: 1201px) {
  .links-container::-webkit-scrollbar,
  .playlist-items::-webkit-scrollbar {
    width: 4px;
  }

  .links-container::-webkit-scrollbar-track,
  .playlist-items::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.1);
    border-radius: 2px;
  }

  .links-container::-webkit-scrollbar-thumb,
  .playlist-items::-webkit-scrollbar-thumb {
    background: var(--ctp-mocha-pink);
    border-radius: 2px;
  }

  .links-container::-webkit-scrollbar-thumb:hover,
  .playlist-items::-webkit-scrollbar-thumb:hover {
    background: var(--ctp-mocha-mauve);
  }
}
</style>
