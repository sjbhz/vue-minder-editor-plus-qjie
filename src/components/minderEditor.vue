<template>
  <div class="main-container">
    <header-menu
      :sequence-enable="sequenceEnable"
      :tag-enable="tagEnable"
      :progress-enable="progressEnable"
      :priority-count="priorityCount"
      :priority-prefix="priorityPrefix"
      :priority-start-with-zero="priorityStartWithZero"
      :tags="tags"
      :move-enable="moveEnable"
      :tag-edit-check="tagEditCheck"
      :isHandleFlag="isHandleFlag"
      :tag-disable-check="tagDisableCheck"
      :priority-disable-check="priorityDisableCheck"
      :distinct-tags="distinctTags"
      :default-mold="defaultMold"
      @moldChange="handleMoldChange"
    />
    <main-editor
      :disabled="disabled"
      :sequence-enable="sequenceEnable"
      :tag-enable="tagEnable"
      :move-enable="moveEnable"
      :progress-enable="progressEnable"
      :import-json="importJson"
      :height="height"
      :isHandleFlag="isHandleFlag"
      :priority-count="priorityCount"
      :priority-prefix="priorityPrefix"
      :priority-start-with-zero="priorityStartWithZero"
      @afterMount="$emit('afterMount')"
      @save="save"
    />
  </div>
</template>

<script>
import headerMenu from "./main/header";
import mainEditor from "./main/mainEditor";
import {
  editMenuProps,
  mainEditorProps,
  moleProps,
  priorityProps,
  tagProps
} from "./props";

export default {
  name: "minderEditor",
  components: {
    headerMenu,
    mainEditor
  },
  data() {
    return {
      minder: {}
    };
  },
  mounted() {
    console.log("minderEditor===this.isHandleFlag", this.isHandleFlag);
  },
  methods: {
    handleMoldChange(data) {
      this.$emit("moldChange", data);
    },
    save(data) {
      this.$emit("save", data);
    },
    // zll添加用于撤销
    handleEnforceChange() {
      minder.fire("contentchange");
    },
  },
  props: {
    ...editMenuProps,
    ...priorityProps,
    ...tagProps,
    ...moleProps,
    ...mainEditorProps
  }
};
</script>

<style scoped>
</style>
