<template>
  <li>
    <a
        href="#"
        :class="{bold: isFolder}"
        @click="toggle"
        @dblclick="changeType">
      {{model.name}}
      <span v-if="isFolder">[{{open ? '-' : '+'}}]</span>
    </a>
    <ul v-show="open" v-if="isFolder">
      <item
          class="item"
          v-for="(model, key) in model.children" :key="key"
          :model="model">
      </item>
      <li class="add" @click="addChild">+</li>
    </ul>
  </li>
</template>

<script>
import Vue from "vue";

export default {
name: "item",
  props: ["model"],
  data: function () {
    return {
      departments: [
        {
          id: 1,
          name: 'Sales',
          show: true,
          subDep:[],
        },
        {
          id: 2,
          name: 'Product',
          show: true,
          subDep:[
            {id: 4,
              name: 'SubProduct',
              show: true,
              subDep:[],
            }
          ],
        },
      ],
      departmentsByLevel: [],
      level: 1,
      open: false,
    }
  },
  computed: {
    isFolder: function () {
      return this.model.children &&
          this.model.children.length
    }
  },
  methods: {
    findLevelOfDepartment: function (deps) {
      return deps.map(d => {
        this.departmentsByLevel.push({...d, level: this.level})
        if (d.subDep.length > 0) {
          this.level++;
          this.findLevelOfDepartment(d.subDep);
          this.level--;
        }
      })
    },
    showSubDeps: function (subDep) {
      console.log(subDep,'1')
      if(subDep.subDep.length > 0) {
        subDep.subDep.forEach(dep => {
          dep.show  = !dep.show
        })
      }
    },
    toggle: function () {
      if (this.isFolder) {
        this.open = !this.open
      }
    },
    changeType: function () {
      if (!this.isFolder) {
        Vue.set(this.model, 'children', [])
        this.addChild()
        this.open = true
      }
    },
    addChild: function () {
      this.model.children.push({
        name: 'new stuff'
      })
    }
  }
}
</script>

<style scoped>

</style>
