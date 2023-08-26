<template>
  <div>
    <minder-editor
      :import-json="importJson"
      :progress-enable="true"
      :tag-enable="true"
      :sequence-enable="true"
      :tags="tags"
      :move-enable="true"
      :disabled="true"
      :distinct-tags="tags"
      :height="500"
      :priority-count="4"
      :tag-edit-check="test"
      :priority-disable-check="test"
      @afterMount="afterMount()"
      :default-mold="3"
      @moldChange="handleMoldChange"
      @save="save"
    />
  </div>
</template>

<script>
import Vue from "vue";
import minderEditor from "../components/plugin";

Vue.use(minderEditor); //开发测试

export default {
  name: "dev-test",
  data() {
    return {
      importJson: {
        root: {
          data: {
            text: "test222"
            // disable: true
          },
          children: [
            {
              data: {
                text: "地图axxaaaa",
                disable: true,
                expandState: "collapse",
                // tagEnable: true,
                // allowDisabledTag: true,
                resource: ["模块1"]
              },
              children: [
                {
                  data: {
                    text: "地图axxaaaa",
                    disable: true,
                    allowDelete: true,
                    // tagEnable: true,
                    // allowDisabledTag: true,
                    resource: ["模块1"]
                  }
                }
              ]
            },
            {
              data: {
                text: "百科aa",
                expandState: "collapse",
              }
            }
          ]
        },
        template: "default"
      },
      tags: ["模块1", "用例"]
    };
  },
  mounted() {},

  methods: {
    save(data) {
      console.log(data);
    },
    test() {
      // return () => {
      return true;
      // };
    },
    handleMoldChange(a) {
      // console.log(a);
    },
    afterMount() {
      minder.on("selectionchange ", function(env) {
        console.log("selectionchange");
        console.log(env);
        // let selectNodes = env.minder.getSelectedNodes();
        // if (selectNodes) {
        //   selectNodes.forEach((node) => {
        //     markChangeNode(node);
        //   })
        // }
      });
      minder.on("contentchange", function(env) {
        console.log("contentchange");
        console.log(env);
        let selectNodes = env.minder.getSelectedNodes();
        console.log(selectNodes);
        console.log("=====");
      });
      minder.on("afterExecCommand", function(env) {
        console.log("afterExecCommand");
        console.log(env);
      });
      minder.on("preExecCommand", function(env) {
        console.log("preExecCommand");
        console.log(env);
      });
      minder.on("beforeExecCommand", function(env) {
        console.log("beforeExecCommand");
        console.log(env);
      });

      this.addEditHotBox();
      this.addHotBox();
    },
    addHotBox() {
      let hotbox = window.minder.hotbox;
      let main = hotbox.state("main");
      main.button({
        position: "ring",
        label: "Test",
        key: "/",
        action: function() {
          console.log("test");
        },
        enable: function() {
          return true;
        },
        beforeShow: function() {}
      });
    },
    addEditHotBox() {
      let runtime = window;
      let minder = window.minder;
      // var hotbox = this.hotbox;
      var fsm = this.fsm;

      let hotbox = window.minder.hotbox;
      let main = hotbox.state("main");
      var {
        isDisableNode,
        markDeleteNode,
        isDeleteDisableNode
      } = require("@/script/tool/utils");

      const buttons = [
        "前移:Alt+Up:ArrangeUp",
        "下级:Tab|Insert:AppendChildNode",
        "同级:Enter:AppendSiblingNode",
        "后移:Alt+Down:ArrangeDown",
        "删除:Delete|Backspace:RemoveNode",
        "上级:Shift+Tab|Shift+Insert:AppendParentNode"
      ];
      var AppendLock = 0;

      buttons.forEach(function(button) {
        var parts = button.split(":");
        var label = parts.shift();
        var key = parts.shift();
        var command = parts.shift();
        main.button({
          position: "ring",
          label: label,
          key: key,
          action: function() {
            if (command.indexOf("Append") === 0) {
              AppendLock++;
              minder.execCommand(command, "分支主题");

              function afterAppend() {
                if (!--AppendLock) {
                  // runtime.editText();
                }
                minder.off("layoutallfinish", afterAppend);
              }
              minder.on("layoutallfinish", afterAppend);
            } else {
              if (command.indexOf("RemoveNode") > -1) {
                markDeleteNode(minder);
              }
              minder.execCommand(command);
              //fsm.jump('normal', 'command-executed');
            }
          },
          enable: function() {
            if (command.indexOf("RemoveNode") > -1) {
              if (
                isDeleteDisableNode(minder) &&
                command.indexOf("AppendChildNode") < 0 &&
                command.indexOf("AppendSiblingNode") < 0
              ) {
                return false;
              }
            } else if (
              command.indexOf("ArrangeUp") < 0 ||
              command.indexOf("ArrangeDown") < 0
            ) {
              if (!minder.moveEnable) {
                return false;
              }
            } else if (
              command.indexOf("AppendChildNode") < 0 &&
              command.indexOf("AppendSiblingNode") < 0
            ) {
              if (isDisableNode(minder)) return false;
            }
            let node = minder.getSelectedNode();
            if (
              node &&
              node.parent === null &&
              command.indexOf("AppendSiblingNode") > -1
            ) {
              return false;
            }
            return minder.queryCommandState(command) != -1;
          }
        });
      });
    }
  }
};
</script>

<style scoped>
</style>
