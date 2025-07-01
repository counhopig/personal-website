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
      <!-- 常用链接模块 -->
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

      <!-- 音乐播放模块 -->
      <section class="music-section">
        <h2 class="section-title">音乐播放器</h2>
        <div class="music-player">
          <div class="song-info">
            <div class="album-art">🎵</div>
            <div class="song-details">
              <h3 class="song-title">{{ currentSongInfo?.title || '未选择歌曲' }}</h3>
              <p class="song-artist">{{ currentSongInfo?.artist || '' }}</p>
            </div>
          </div>
          
          <div class="player-controls">
            <button @click="prevSong" class="control-btn">⏮️</button>
            <button @click="togglePlay" class="play-btn">
              {{ isPlaying ? '⏸️' : '▶️' }}
            </button>
            <button @click="nextSong" class="control-btn">⏭️</button>
          </div>

          <div class="progress-section">
            <span class="time-current">{{ formatTime(progress) }}</span>
            <div class="progress-bar" @click="setProgress">
              <div 
                class="progress-fill" 
                :style="{ width: duration ? (progress / duration * 100) + '%' : '0%' }"
              ></div>
            </div>
            <span class="time-total">{{ formatTime(duration) }}</span>
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
            >
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
                  <span class="playlist-song-title">{{ song.title }}</span>
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
          category: "开发"
        },
        {
          name: "Stack Overflow",
          url: "https://stackoverflow.com",
          description: "程序员问答社区",
          icon: "📚",
          category: "开发"
        },
        {
          name: "MDN Web Docs",
          url: "https://developer.mozilla.org",
          description: "Web开发文档",
          icon: "📖",
          category: "开发"
        },
        {
          name: "Vue.js",
          url: "https://vuejs.org",
          description: "渐进式JavaScript框架",
          icon: "🔧",
          category: "开发"
        },
        {
          name: "YouTube",
          url: "https://youtube.com",
          description: "视频分享平台",
          icon: "📺",
          category: "娱乐"
        },
        {
          name: "Netflix",
          url: "https://netflix.com",
          description: "在线流媒体",
          icon: "🎬",
          category: "娱乐"
        },
        {
          name: "Spotify",
          url: "https://spotify.com",
          description: "音乐流媒体",
          icon: "🎵",
          category: "音乐"
        },
        {
          name: "Bilibili",
          url: "https://bilibili.com",
          description: "弹幕视频网站",
          icon: "📹",
          category: "娱乐"
        },
        {
          name: "知乎",
          url: "https://zhihu.com",
          description: "知识问答社区",
          icon: "💡",
          category: "学习"
        }
      ],
      playlist: [
        {
          title: "Lofi Hip Hop Mix",
          artist: "Chillhop Music",
          duration: "3:24",
          url: "https://www.soundjay.com/misc/sounds-misc/bell-ringing-05.wav" // 示例音频
        },
        {
          title: "Peaceful Piano",
          artist: "Relaxing Music",
          duration: "4:12",
          url: "https://www.soundjay.com/misc/sounds-misc/bell-ringing-05.wav" // 示例音频
        },
        {
          title: "Nature Sounds",
          artist: "Ambient",
          duration: "5:30",
          url: "https://www.soundjay.com/misc/sounds-misc/bell-ringing-05.wav" // 示例音频
        }
      ]
    };
  },
  computed: {
    formattedTime() {
      return this.currentTime.toLocaleTimeString('zh-CN', {
        hour12: false,
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit'
      });
    },
    formattedDate() {
      return this.currentTime.toLocaleDateString('zh-CN', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        weekday: 'long'
      });
    },
    linksByCategory() {
      const categories = {};
      this.favoriteLinks.forEach(link => {
        if (!categories[link.category]) {
          categories[link.category] = [];
        }
        categories[link.category].push(link);
      });
      return categories;
    },
    currentSongInfo() {
      return this.playlist[this.currentSongIndex] || null;
    }
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
      // 这里是音乐播放的示例实现
      // 在实际应用中，您可能需要使用真实的音频文件
      this.isPlaying = true;
      console.log(`播放: ${this.currentSongInfo.title}`);
      
      // 模拟播放进度
      this.progress = 0;
      this.duration = 180; // 3分钟示例
      this.simulateProgress();
    },
    pauseMusic() {
      this.isPlaying = false;
      console.log("暂停播放");
    },
    nextSong() {
      this.currentSongIndex = (this.currentSongIndex + 1) % this.playlist.length;
      if (this.isPlaying) {
        this.playMusic();
      }
    },
    prevSong() {
      this.currentSongIndex = this.currentSongIndex === 0 
        ? this.playlist.length - 1 
        : this.currentSongIndex - 1;
      if (this.isPlaying) {
        this.playMusic();
      }
    },
    simulateProgress() {
      if (this.isPlaying && this.progress < this.duration) {
        setTimeout(() => {
          this.progress += 1;
          this.simulateProgress();
        }, 1000);
      } else if (this.progress >= this.duration) {
        this.nextSong();
      }
    },
    formatTime(seconds) {
      const mins = Math.floor(seconds / 60);
      const secs = seconds % 60;
      return `${mins}:${secs.toString().padStart(2, '0')}`;
    },
    setVolume(event) {
      this.volume = event.target.value;
    },
    setProgress(event) {
      const rect = event.target.getBoundingClientRect();
      const percent = (event.clientX - rect.left) / rect.width;
      this.progress = Math.floor(percent * this.duration);
    }
  }
};
</script>

<style scoped>
.personal-website {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

/* 头部时间显示模块 */
.welcome-section {
  text-align: center;
  margin-bottom: 3rem;
}

.time-display {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 2rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.greeting {
  font-size: 2.5rem;
  font-weight: bold;
  color: var(--ctp-mocha-pink);
  margin-bottom: 1rem;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.current-time {
  font-size: 3rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  font-family: 'Courier New', monospace;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.current-date {
  font-size: 1.2rem;
  color: var(--ctp-mocha-subtext1);
  margin-top: 0.5rem;
}

/* 主要内容区域 */
.main-content {
  display: grid;
  grid-template-columns: 1fr;
  gap: 3rem;
}

.section-title {
  font-size: 1.8rem;
  font-weight: bold;
  color: var(--ctp-mocha-mauve);
  margin-bottom: 1.5rem;
  text-align: center;
}

/* 常用链接模块 */
.links-section {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 2rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.links-container {
  display: grid;
  gap: 2rem;
}

.category-group {
  margin-bottom: 2rem;
}

.category-title {
  font-size: 1.3rem;
  font-weight: bold;
  color: var(--ctp-mocha-blue);
  margin-bottom: 1rem;
  text-align: left;
}

.links-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1rem;
}

.link-card {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  padding: 1rem;
  text-decoration: none;
  color: inherit;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.link-card:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.link-icon {
  font-size: 2rem;
  margin-right: 1rem;
  flex-shrink: 0;
}

.link-info {
  flex: 1;
}

.link-name {
  font-size: 1.1rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  margin: 0 0 0.3rem 0;
}

.link-description {
  font-size: 0.9rem;
  color: var(--ctp-mocha-subtext0);
  margin: 0;
}

/* 音乐播放模块 */
.music-section {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 2rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.music-player {
  max-width: 500px;
  margin: 0 auto;
}

.song-info {
  display: flex;
  align-items: center;
  margin-bottom: 1.5rem;
  text-align: left;
}

.album-art {
  font-size: 3rem;
  margin-right: 1rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.song-details {
  flex: 1;
}

.song-title {
  font-size: 1.3rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  margin: 0 0 0.3rem 0;
}

.song-artist {
  color: var(--ctp-mocha-subtext1);
  margin: 0;
}

.player-controls {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.control-btn, .play-btn {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1.5rem;
}

.play-btn {
  width: 60px;
  height: 60px;
  background: var(--ctp-mocha-pink);
  font-size: 2rem;
}

.control-btn:hover, .play-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

.play-btn:hover {
  background: var(--ctp-mocha-mauve);
}

.progress-section {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
  font-size: 0.9rem;
  color: var(--ctp-mocha-subtext1);
}

.progress-bar {
  flex: 1;
  height: 6px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--ctp-mocha-pink), var(--ctp-mocha-mauve));
  border-radius: 3px;
  transition: width 0.3s ease;
}

.volume-section {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2rem;
  font-size: 0.9rem;
}

.volume-slider {
  flex: 1;
  height: 4px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 2px;
  outline: none;
  appearance: none;
}

.volume-slider::-webkit-slider-thumb {
  appearance: none;
  width: 16px;
  height: 16px;
  background: var(--ctp-mocha-pink);
  border-radius: 50%;
  cursor: pointer;
}

.volume-slider::-moz-range-thumb {
  width: 16px;
  height: 16px;
  background: var(--ctp-mocha-pink);
  border-radius: 50%;
  cursor: pointer;
  border: none;
}

.playlist {
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding-top: 1.5rem;
}

.playlist-title {
  font-size: 1.1rem;
  font-weight: bold;
  color: var(--ctp-mocha-text);
  margin-bottom: 1rem;
  text-align: left;
}

.playlist-items {
  max-height: 200px;
  overflow-y: auto;
}

.playlist-item {
  display: flex;
  align-items: center;
  padding: 0.8rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-bottom: 0.5rem;
}

.playlist-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.playlist-item.active {
  background: rgba(245, 194, 231, 0.2);
  border-left: 3px solid var(--ctp-mocha-pink);
}

.song-number {
  font-size: 0.9rem;
  color: var(--ctp-mocha-subtext0);
  margin-right: 1rem;
  width: 20px;
  text-align: center;
}

.playlist-song-info {
  flex: 1;
  text-align: left;
}

.playlist-song-title {
  display: block;
  font-weight: 500;
  color: var(--ctp-mocha-text);
  font-size: 0.95rem;
}

.playlist-song-artist {
  display: block;
  color: var(--ctp-mocha-subtext1);
  font-size: 0.8rem;
  margin-top: 0.2rem;
}

.song-duration {
  font-size: 0.8rem;
  color: var(--ctp-mocha-subtext0);
}

/* 响应式设计 */
@media (max-width: 768px) {
  .personal-website {
    padding: 1rem;
  }
  
  .greeting {
    font-size: 2rem;
  }
  
  .current-time {
    font-size: 2.5rem;
  }
  
  .links-grid {
    grid-template-columns: 1fr;
  }
  
  .link-card {
    padding: 0.8rem;
  }
  
  .time-display {
    padding: 1.5rem;
  }
}

@media (max-width: 480px) {
  .greeting {
    font-size: 1.8rem;
  }
  
  .current-time {
    font-size: 2rem;
  }
  
  .section-title {
    font-size: 1.5rem;
  }
}

/* 滚动条样式 */
.playlist-items::-webkit-scrollbar {
  width: 6px;
}

.playlist-items::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 3px;
}

.playlist-items::-webkit-scrollbar-thumb {
  background: var(--ctp-mocha-pink);
  border-radius: 3px;
}

.playlist-items::-webkit-scrollbar-thumb:hover {
  background: var(--ctp-mocha-mauve);
}
</style>
