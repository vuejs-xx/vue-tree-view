<template>
  <li @dblclick.stop="editItem">
    <span v-if="isFloder" class="fa" :class="[caretPosition]" @click="clickCollapse"></span>
    <span :class="{inputShow:inputHide,inputHide:inputShow}">{{data.name}}</span>
    <input v-focus="true" @focus="focused=true" @blur="inputBlur" v-model="name"
           :class="{inputShow:inputShow,inputHide:inputHide}">
    <span class="fa fa-trash" @click="deleteItem"></span>
    <div v-show="showChild">
      <slot name="childTreeComponent"></slot>
    </div>
  </li>
</template>
<style>
  .inputShow {
    display: inline-block;
  }

  .inputHide {
    display: none;
  }

  span {
    line-height: 17px;
  }

  input {
    line-height: 23px;
    height: 23px;
    font-size: 14px;
  }
</style>
<script>
  import {focus} from 'vue-focus'
  export default{
    directives: {focus: focus},
    props: ['data'],
    computed: {
      isFloder: function () {
        return this.data.children.length > 0 && this.data.children
      },
      caretPosition: function () {
        if (this.showChild) {
          return 'fa-caret-down'
        } else {
          return 'fa-caret-right'
        }
      }
    },
    data () {
      return {
        inputShow: false,
        inputHide: true,
        name: '',
        showChild: false
      }
    },
    components: {},
    methods: {
      deleteItem: function () {
        console.log('delete...')
        this.$emit('delete-item', this.data)
      },
      editItem: function () {
        this.inputShow = true
        this.inputHide = false
        this.name = this.data.name
      },
      inputBlur: function () {
        this.inputShow = false
        this.inputHide = true
        console.log(this.name)
        this.data.name = this.name
      },
      clickCollapse: function () {
        this.showChild = !this.showChild
      }
    }
  }
</script>
