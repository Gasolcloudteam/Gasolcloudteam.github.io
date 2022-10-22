<template>
  <div class="tw-w-44 tw-h-72  tw-absolute tw-rounded-xl tw-overflow-hidden tw-border tw-border-gray-400 tw-select-none" style="z-index:9999;backdrop-filter: blur(35px);box-shadow: 0 0 10px rgba(16,16,16,.3)" @click="fullclicked($event)" :style="{'left':context_menu_x+'px', 'top':context_menu_y +'px'}">
    <div class="tw-absolute tw-w-full tw-h-full tw-py-2 tw-flex tw-flex-col" style="background-color:rgba(235,235,235,.7)">
      <ContextMenuButton disabled further_menu :text="'新建'"/>
      <ContextMenuButton disabled further_menu :text="'查看'"/>
      <ContextMenuButton disabled further_menu :text="'排序方式'"/>
      <ContextMenuButton :text="'重新加载Deepin'" @click.native="reload_clicked"/>
      <ContextMenuButton disabled :text="'粘贴'"/>
      <ContextMenuButton  :text="'打开Terminal'" @click.native="terminal_clicked"/>
      <div class=" tw-w-full" style="height:1.5px;background-color:rgba(188,188,188,1)"></div>
      <ContextMenuButton disabled :text="'桌面壁纸'"/>
      <ContextMenuButton  :text="'系统设置'" @click.native="settings_clicked"/>
      <ContextMenuButton  :text="'访问我的blog'" @click.native="open_project"/>
    </div>
  </div>
</template>

<script>
import ContextMenuButton from './ContextMenuButton.vue'
export default {
  name: 'ContextMenu',
  components: {
    ContextMenuButton
  },
  data(){
    return {

    }
  },
  created(){
  },
  mounted(){
  },
  watch:{
  },
  computed:{
    has_terminal(){
      for (let item of this.$store.state.window_list) {
        if (item.type === 'terminal') {
          return true
        }
      }
      return false
    },
    has_settings(){
      for (let item of this.$store.state.window_list) {
        if (item.type === 'settings') {
          return true
        }
      }
      return false
    },
    context_menu_x(){
      return this.$store.state.context_menu_x
    },
    context_menu_y(){
      return this.$store.state.context_menu_y
    }
  },
  methods:{
    fullclicked(event){
      // this.$store.commit('hide_context_menu')
      event.stopPropagation()
    },
    settings_clicked(){
      if (this.has_settings) {
        this.$store.commit('switch_global_window_show_status', {'type':'settings'})
      } else {
        this.$store.commit('open_new_window', {'type':'settings'})
      }
      this.$store.commit('refresh_window_focus', {'type':'settings'})
      this.$store.commit('hide_context_menu')
    },
    terminal_clicked(){
      if (this.has_terminal) {
        this.$store.commit('switch_global_window_show_status', {'type':'terminal'})
      } else {
        this.$store.commit('open_new_window', {'type':'terminal'})
      }
      this.$store.commit('refresh_window_focus', {'type':'terminal'})
      this.$store.commit('hide_context_menu')
    },
    open_project(){
      window.open("https://blog.xingchencloud.top/", "_blank");
      this.$store.commit('hide_context_menu')
    },
    reload_clicked(){
      location.reload();
    }
  }
}
</script>

<style scoped>
</style>