<template>
  <header>
    <div class="header__brand">
      <a href="https://crypton.studio/" class="brand__link">
        <svg
          width="46"
          height="46"
          viewBox="0 0 46 46"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M9.82023 0C4.39667 0 0 4.39666 0 9.82022V36.1798C0 41.6033 4.39667 46 9.82023 46H36.1798C41.6033 46 46 41.6033 46 36.1798V9.82022C46 4.39666 41.6033 0 36.1798 0H9.82023ZM8.05873 17.7722C7.20292 19.3127 6.77501 21.0719 6.77501 23.0497C6.77501 25.0276 7.20292 26.7868 8.05873 28.3273C8.91455 29.8677 10.1032 31.0754 11.6246 31.9502C13.1651 32.806 14.9052 33.2339 16.8451 33.2339C19.2224 33.2339 21.2573 32.6158 22.9499 31.3797C24.6425 30.1245 25.7741 28.4129 26.3446 26.2448H20.9815C20.5821 27.0816 20.0116 27.7187 19.2699 28.1561C18.5472 28.5935 17.7199 28.8122 16.788 28.8122C15.2856 28.8122 14.0685 28.2892 13.1366 27.2432C12.2047 26.1972 11.7387 24.7994 11.7387 23.0497C11.7387 21.3001 12.2047 19.9023 13.1366 18.8563C14.0685 17.8103 15.2856 17.2873 16.788 17.2873C17.7199 17.2873 18.5472 17.506 19.2699 17.9434C20.0116 18.3808 20.5821 19.0179 20.9815 19.8547H26.3446C25.7741 17.6866 24.6425 15.975 22.9499 14.7198C21.2573 13.4646 19.2224 12.837 16.8451 12.837C14.9052 12.837 13.1651 13.2745 11.6246 14.1493C10.1032 15.0051 8.91455 16.2127 8.05873 17.7722ZM36.3496 17.7722C35.4938 18.2667 34.7806 18.9513 34.2101 19.8262V17.1732H29.3319V33.0913H34.2101V25.7028C34.2101 24.3905 34.5144 23.4777 35.123 22.9642C35.7315 22.4317 36.6444 22.1654 37.8616 22.1654H39.2023V17.002C38.1563 17.002 37.2054 17.2587 36.3496 17.7722Z"
            fill="white"
          />
        </svg>
      </a>
    </div>
    <div class="option-wrap">
      <div class="options">
        <div class="options__lang-toggle">
          <div class="select" @click="isActive = !isActive">
            <!-- <div class="select__header"> -->
            <span class="select__current">{{ selected }}</span>
            <!-- <div class="select__icon"> -->
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M6.29297 10.7072L7.70718 9.29297L12.0001 13.5859L16.293 9.29297L17.7072 10.7072L12.0001 16.4143L6.29297 10.7072Z"
                fill="white"
              />
            </svg>
            <!-- </div> -->
            <!-- :class="{ active: isActive }" -->
            <div v-if="isActive" class="select__body">
              <div
                v-for="option in options"
                :key="option.value"
                @click="selectOption(option)"
                class="select__item"
              >
                {{ option.name }}
              </div>
            </div>
            <!-- </div> -->
          </div>
        </div>
        <button class="options__theme">
          <svg
            width="17"
            height="17"
            viewBox="0 0 17 17"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M16.9981 9.57093C17.0257 9.35722 16.7445 9.25565 16.6046 9.41952C15.4306 10.7943 13.6846 11.6659 11.735 11.6659C8.19984 11.6659 5.33407 8.80015 5.33407 5.26503C5.33407 3.31544 6.20568 1.56943 7.58053 0.395401C7.7444 0.255462 7.64283 -0.0257423 7.42912 0.00189767C3.23783 0.543959 0 4.1266 0 8.46548C0 13.179 3.82097 17 8.53452 17C12.8735 17 16.4561 13.7622 16.9981 9.57093Z"
              fill="black"
            />
          </svg>
        </button>
        <button class="connect-wallet">Connect wallet</button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  props: {
    options: {
      type: Array,
      default() {
        return []
      },
    },
    selected: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      isActive: false,
    }
  },
  methods: {
    selectOption(option) {
      this.$emit('select', option)
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  font-family: 'Montserrat', sans-serif;
}
header {
  background-color: var(--main-bg-color);
  display: flex;
  align-items: center;
  justify-content: space-around;
  height: 85px;
}
.connect-wallet {
  background: linear-gradient(253.94deg, #09f2c3 8.37%, #15dce4 69.71%);
  border-radius: 14px;
  border: 1px solid var(--secondary-bg-color);
}
.options {
  display: grid;
  grid-gap: 20px;
  grid-auto-flow: column;
}

.select {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100px;
  height: 50px;
  padding: 0 18px 0 15px;
  font-family: Roboto, sans-serif;
  color: #fff;
  background: var(--main-bg-color);
  border-radius: 14px;
  cursor: pointer;
  position: relative;
  border: 1px solid var(--secondary-bg-color);

  /* -------------------------------- */

  /* margin: 0 auto;
  position: relative;
  width: 100px;
  height: 50px;
  background: var(--main-bg-color); */

  /* Black / 800 */

  /* border: 1px solid var(--secondary-bg-color);
  box-sizing: border-box;
  border-radius: 14px; */
}
.select__header {
  cursor: pointer;
  display: flex;
  align-items: center;
  margin-right: auto;
}
.select__icon {
  width: 40px;
  height: 40px;
  align-items: center;
  display: flex;
  flex-shrink: 0;
  justify-content: center;
  text-align: center;
  margin-left: auto;
}
.select__current {
  font-size: 18px;
  line-height: 22px;
  color: #fff;
}
.select:hover {
  background: var(--secondary-bg-color);
}
.select__body {
  /* display: none; */
  z-index: 100;
  position: absolute;
  left: 0;
  top: calc(100% + 16px);
  width: 100%;
  background: #fff;
  box-shadow: 0 10px 20px rgb(0 0 0 / 30%);
  border-radius: 6px;
  background-color: var(--main-bg-color);
}
.active {
  display: block;
}
.select__item {
  cursor: pointer;
  font-size: 18px;
  line-height: 22px;
  color: #ababab;
  padding: 10px 15px;
}
.select__item:hover {
  color: #fff;
}
.options__theme {
  width: 50px;
  height: 50px;
  left: 20px;
  top: 20px;

  /* Black/900 */

  background: #fff;

  /* Black / 800 */

  border: 1px solid var(--secondary-bg-color);
  box-sizing: border-box;
  border-radius: 14px;
  color: #fff;
}
</style>
