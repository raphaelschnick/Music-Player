<template>
  <nav>
    <div>
      <div ref="sideNav" class="bm-menu">
        <nav class="bm-item-list">
          <a href="/upload" class="fas fa-upload"></a>
          <a class="fas fa-home"></a>
          <a class="fas fa-list-music"></a>
          <a class="fas fa-album"></a>
          <a class="fas fa-male"></a>
          <a class="fas fa-cogs"></a>
        </nav>
        <span
          class="bm-cross-button cross-style"
          @click="closeMenu"
          :class="{ hidden: !crossIcon }"
        >
          <span
            v-for="(x, index) in 2"
            :key="x"
            class="bm-cross"
            :style="{
              position: 'absolute',
              width: '3px',
              height: '14px',
              transform: index === 1 ? 'rotate(45deg)' : 'rotate(-45deg)'
            }"
          >
          </span>
        </span>
      </div>

      <div
        ref="bmBurgerButton"
        class="bm-burger-button"
        @click="openMenu"
        :class="{ hidden: !burgerIcon }"
      >
        <i class="material-icons">menu</i>
      </div>
    </div>
    <div class="center">
      <h6>Playlist</h6>
    </div>
    <div class="right">
      <i class="material-icons search">search</i>
      <i class="material-icons">queue_music</i>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'navbar',
  data() {
    return {
      isSideBarOpen: false
    }
  },
  props: {
    isOpen: {
      type: Boolean,
      required: false
    },
    right: {
      type: Boolean,
      required: false
    },
    width: {
      type: [String],
      required: false,
      default: '160'
    },
    disableEsc: {
      type: Boolean,
      required: false
    },
    noOverlay: {
      type: Boolean,
      required: false
    },
    onStateChange: {
      type: Function,
      required: false
    },
    burgerIcon: {
      type: Boolean,
      required: false,
      default: true
    },
    crossIcon: {
      type: Boolean,
      required: false,
      default: true
    },
    disableOutsideClick: {
      type: Boolean,
      required: false,
      default: false
    },
    closeOnNavigation: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  methods: {
    openMenu() {
      this.$emit('openMenu')
      this.isSideBarOpen = true
      if (!this.noOverlay) {
        document.body.classList.add('bm-overlay')
      }
      if (this.right) {
        this.$refs.sideNav.style.left = 'auto'
        this.$refs.sideNav.style.right = '0px'
      }
      this.$nextTick(function() {
        this.$refs.sideNav.style.width = this.width
          ? this.width + 'px'
          : '300px'
      })
    },
    closeMenu() {
      this.$emit('closeMenu')
      this.isSideBarOpen = false
      document.body.classList.remove('bm-overlay')
      this.$refs.sideNav.style.width = '0px'
    },
    closeMenuOnEsc(e) {
      e = e || window.event
      if (e.key === 'Escape' || e.keyCode === 27) {
        this.closeMenu()
      }
    },
    documentClick(e) {
      let element = this.$refs.bmBurgerButton
      let target = null
      if (e && e.target) {
        target = e.target
      }
      if (
        element &&
        element !== target &&
        !element.contains(target) &&
        !this.hasClass(target, 'bm-menu') &&
        this.isSideBarOpen &&
        !this.disableOutsideClick
      ) {
        this.closeMenu()
      } else if (
        element &&
        this.hasClass(target, 'bm-menu') &&
        this.isSideBarOpen &&
        this.closeOnNavigation
      ) {
        this.closeMenu()
      }
    },
    hasClass(element, className) {
      do {
        if (element.classList && element.classList.contains(className)) {
          return true
        }
        element = element.parentNode
      } while (element)
      return false
    }
  },
  mounted() {
    if (!this.disableEsc) {
      document.addEventListener('keyup', this.closeMenuOnEsc)
    }
  },
  created: function() {
    document.addEventListener('click', this.documentClick)
  },
  destroyed: function() {
    document.removeEventListener('keyup', this.closeMenuOnEsc)
    document.removeEventListener('click', this.documentClick)
  },
  watch: {
    isOpen: {
      deep: true,
      immediate: true,
      handler(newValue, oldValue) {
        this.$nextTick(() => {
          if (!oldValue && newValue) {
            this.openMenu()
          }
          if (oldValue && !newValue) {
            this.closeMenu()
          }
        })
      }
    },
    right: {
      deep: true,
      immediate: true,
      handler(oldValue, newValue) {
        if (oldValue) {
          this.$nextTick(() => {
            this.$refs.bmBurgerButton.style.left = 'auto'
            this.$refs.bmBurgerButton.style.right = '36px'
            this.$refs.sideNav.style.left = 'auto'
            this.$refs.sideNav.style.right = '0px'
            document.querySelector('.bm-burger-button').style.left = 'auto'
            document.querySelector('.bm-burger-button').style.right = '36px'
            document.querySelector('.bm-menu').style.left = 'auto'
            document.querySelector('.bm-menu').style.right = '0px'
            document.querySelector('.cross-style').style.right = '250px'
          })
        }
        if (newValue) {
          if (this.$refs.bmBurgerButton.hasAttribute('style')) {
            this.$refs.bmBurgerButton.removeAttribute('style')
            this.$refs.sideNav.style.right = 'auto'
            document.querySelector('.bm-burger-button').removeAttribute('style')
            document.getElementById('sideNav').style.right = 'auto'
            document.querySelector('.cross-style').style.right = '0px'
          }
        }
      }
    }
  }
}
</script>

<style>
nav {
  position: relative;
  z-index: 3;
  min-height: 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0px 15px;
}
nav .left {
  display: flex;
}
nav .left i {
  cursor: pointer;
  color: black;
}
nav .left h6 {
  margin: 0;
  padding-left: 20px;
  color: black;
  font-size: 18px;
  line-height: 1.38;
}
nav .center {
  padding-left: 37px;
}
nav .right {
  display: flex;
}
nav .right i {
  cursor: pointer;
  color: black;
}
nav .right .search {
  padding-right: 10px;
}
.bm-burger-button {
  width: 36px;
  height: 30px;
  cursor: pointer;
}
.bm-burger-button.hidden {
  display: none;
}
.bm-burger-bars {
  background-color: #373a47;
}
.line-style {
  position: absolute;
  height: 20%;
  left: 0;
  right: 0;
}
.cross-style {
  position: absolute;
  top: 12px;
  right: 2px;
  cursor: pointer;
}
.bm-cross {
  background: #bdc3c7;
}
.bm-cross-button {
  height: 24px;
  width: 24px;
}
.bm-cross-button.hidden {
  display: none;
}
.bm-menu {
  height: 100%;
  width: 0;
  position: fixed;
  top: 0;
  left: 0;
  background-color: #1a1b1e;
  overflow-x: hidden;
  padding-top: 60px;
  transition: 0.5s;
}
.bm-overlay {
  background: rgba(0, 0, 0, 0.3);
}
nav .bm-item-list a {
  color: white;
  margin-left: 30%;
  font-size: 40px;
}
</style>
