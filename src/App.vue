<template>
  <div id="app">
    <div
      v-show="isVisible"
      id="cursor"
      :style="{
        backgroundColor: cursorColor,
        width: cursorSize,
        height: cursorSize,
      }"
    ></div>
    <div
      v-show="isVisible"
      id="cursor-border"
      :style="{
        width: borderSize,
        height: borderSize,
        boxShadow: `0 0 0 1px ${cursorBorderColor}`,
      }"
    ></div>
    <div class="content">
      <h1>Welcome to Brand Book Studio</h1>
      <p>
        Hover over and click the button or links to change the cursor color.
      </p>
      <a
        v-for="link in links"
        :key="link"
        @click="changeCursorColorOnClick"
        @mouseenter="onPointerEnter"
        @mouseleave="onPointerLeave"
        class="pointer-item"
        href="#"
      >
        {{ link }}
      </a>
      <button
        @click="changeCursorColorOnClick"
        @mouseenter="onPointerEnter"
        @mouseleave="onPointerLeave"
        class="pointer-item"
      >
        Click Me
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cursorColor: "rgba(10, 45, 248, 0.842)", // Default cursor color (blue)
      cursorBorderColor: "rgb(231, 11, 11)", // Default border color (red)
      cursorSize: "10px", // Default cursor size
      borderSize: "50px", // Default border size
      isVisible: true, // Cursor visibility
      links: ["Link 1", "Link 2", "Link 3"], // Example links
    };
  },
  methods: {
    updateCursor(event) {
      const cursor = document.getElementById("cursor");
      const cursorBorder = document.getElementById("cursor-border");

      // Update cursor position
      cursor.style.top = `${event.clientY}px`;
      cursor.style.left = `${event.clientX}px`;
      cursorBorder.style.top = `${event.clientY}px`;
      cursorBorder.style.left = `${event.clientX}px`;
    },
    hideCursor() {
      this.isVisible = false;
    },
    showCursor() {
      this.isVisible = true;
    },
    changeCursorColorOnClick() {
      this.cursorColor = this.getRandomColor();
      this.cursorBorderColor = this.getRandomColor();

      setTimeout(() => {
        this.cursorColor = "rgba(10, 45, 248, 0.842)";
        this.cursorBorderColor = "rgb(231, 11, 11)";
      }, 1000);
    },
    getRandomColor() {
      const letters = "0123456789ABCDEF";
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    onPointerEnter() {
      this.cursorSize = "20px";
      this.borderSize = "50px";
      this.cursorColor = "rgba(231, 11, 11, 0.842)";
    },
    onPointerLeave() {
      this.cursorSize = "10px";
      this.borderSize = "30px";
      this.cursorColor = "rgba(10, 45, 248, 0.842)";
    },
  },
  mounted() {
    window.addEventListener("mousemove", this.updateCursor);
    window.addEventListener("mouseleave", this.hideCursor);
    window.addEventListener("mouseenter", this.showCursor);
  },
};
</script>

<style>
#cursor {
  position: fixed;
  top: 0;
  left: 0;
  border-radius: 50%;
  pointer-events: none;
  z-index: 999;
  border-style: solid;
  transition: width 0.15s ease-out, height 0.15s ease-out,
    border-width 0.15s ease-out, border-color 0.15s ease-out;
  transform: translate(-50%, -50%);
}

#cursor-border {
  position: fixed;
  border-radius: 50%;
  pointer-events: none;
  transition: top 0.15s ease-out, left 0.15s ease-out, width 0.15s ease-out,
    height 0.15s ease-out, box-shadow 0.15s ease-out;
  z-index: 999;
  transform: translate(-50%, -50%); /* Center the border on the mouse pointer */
}

* {
  cursor: none;
}

a.pointer-item,
button.pointer-item {
  color: black;
  text-decoration: none;
  margin: 10px;
  font-size: 18px;
}

button.pointer-item {
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
}
</style>
