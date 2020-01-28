<template>
  <svg xmlns="http://www.w3.org/2000/svg"
    :width="width"
    :height="height"
    :view-box="viewBox"
    :aria-labelledby="iconName"
    role="presentation"
    class="icon"
  >
    <title
      :id="iconName"
      lang="en"
    >
      {{ iconName }} icon
    </title>
    <slot />
  </svg>
</template>

<script>
export default {
  props: {
    iconName: {
      type: String,
      default: 'box',
    },
    width: {
      type: [Number, String],
      default: 16,
    },
    height: {
      type: [Number, String],
      default: 16,
    },
    viewBox: {
      type: String,
      default: '0 0 16 16',
    },
  },
};
</script>
