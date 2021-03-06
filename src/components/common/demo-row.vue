<template>
  <div class="demo-row">
    <span class="demo-row-subtitle">{{ title }}</span>
    <span v-if="description" class="demo-row-description"
      >{{ description }}。</span
    >

    <div class="demo-row-content">
      <div>
        <slot></slot>
      </div>

      <fat-button class="btn open-btn" type="text" @click.stop="handleClick">
        <fat-icon name="code" />
        <span class="c-size-m">&nbsp;示例代码</span>
      </fat-button>
      <div v-if="UI.isOpen && code" class="demo-row-example" v-highlight>
        <div v-html="compiledCode"></div>
      </div>
    </div>
  </div>
</template>

<script>
import { StringToCode } from "./utils";

export default {
  props: {
    title: { type: String, default: "" },
    description: { type: String, default: "" },
    code: { type: String, default: "" }
  },
  inject: ["DemoBlock"],
  data() {
    return {
      UI: {
        isOpen: false
      }
    };
  },
  computed: {
    compiledCode: function() {
      const code = StringToCode(this.code);
      return this.$marked(code, { sanitize: true });
    }
  },
  methods: {
    handleClick() {
      this.UI.isOpen = !this.UI.isOpen;
    }
  }
};
</script>
<style lang="scss">
.demo-row {
  margin-top: 14px;
  display: block;
  min-width: 420px;
  max-width: max-content;
  padding: 16px 16px;
  border: 1px solid #ebedf0;
  border-radius: 2px;
  box-shadow: 2px 2px 5px #ccc;
  transition: all .5s;

  &:last-child {
    margin-bottom: 14px;
  }

  &:hover {
    box-shadow: 4px 4px 5px #bbb;
  }

  .demo-row-subtitle:first-child {
    margin-bottom: 16px;
    display: block;
    font-size: 16px;
    font-weight: 600;
    color: #1f2f3d;
    line-height: 1.5em;

    & + span {
      font-size: 14px;
      color: #5e6d82;
      line-height: 1.5em;
    }
  }

  .demo-row-description {
    display: inline-block;
    margin-bottom: 16px;
  }

  .demo-row-content {
    .demo-row-example,
    .open-btn {
      margin-top: 16px;
    }
    .hljs {
      font-size: 0.8em;
      letter-spacing: -1px;
      line-height: 14px;
      background: #fafafa;
    }
  }

  .row-item {
    display: flex;
    font-size: 14px;

    .row-item-subtitle:first-child {
      margin: 8px 8px 8px 0;
    }
  }

  .input,
  .select {
    width: 250px;
    &:not(:first-child) {
      margin-top: 16px;
    }
  }
}
</style>
