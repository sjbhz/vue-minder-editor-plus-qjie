<template lang="html">
  <div class="menu-container">
    <expand/>
    <selection />
    <insert-box :isHandleFlag="isHandleFlag"/>
    <move-box :move-enable="moveEnable"/>
    <edit-del :isHandleFlag="isHandleFlag"/>
    <sequence-box
      v-if="sequenceEnable"
      :prefix="priorityPrefix"
      :priority-count="priorityCount"
      :priority-disable-check="priorityDisableCheck"
      :start-with-zero="priorityStartWithZero"
    />
    <progress-box v-if="progressEnable"/>
    <tag-box
      v-if="tagEnable"
      :tags="tags"
      :tag-disable-check="tagDisableCheck"
      :tag-edit-check="tagEditCheck"
      :editResourceOuter="editResourceOuter"
      :distinct-tags="distinctTags"
    />
  </div>
</template>

<script>
import insertBox from "./insertBox";
import moveBox from "./moveBox";
import editDel from "./editDel";
import sequenceBox from "./sequenceBox";
import progressBox from "./progressBox";
import expand from "./expand";
import selection from "./selection";
import TagBox from "./tagBox";
import { editMenuProps, priorityProps, tagProps } from "../../props";

export default {
  name: "editMenu",
  components: {
    TagBox,
    insertBox,
    moveBox,
    editDel,
    sequenceBox,
    progressBox,
    expand,
    selection
  },
  props: {
    ...editMenuProps,
    ...priorityProps,
    ...tagProps
  }
};
</script>
