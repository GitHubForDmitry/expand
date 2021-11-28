<template>
  <b-list-group-item>
    <div
        @click="toggle"
        class="d-flex"
        style="position:relative;z-index: 2"
    >
      <span class="mx-2 d-flex align-items-start icon"  >
        <csharp style="height: 16px; width: 16px; margin: 3px 5px 0 0;" v-if="model.name === 'Csharp'" />
        <java  style="height: 16px; width: 16px; margin: 3px 5px 0 0;" v-if="model.name === 'Java'" />
        <javascript  style="height: 16px; width: 16px; margin: 3px 5px 0 0;" v-if="model.name === 'JavaScript'" />
        <react  style="height: 16px; width: 16px; margin: 3px 5px 0 0;" v-if="model.name === 'React'" />
        <angular  style="height: 16px; width: 16px; margin: 3px 5px 0 0;" v-if="model.name === 'Angular'" />
        <vue  style="height: 16px; width: 16px; margin: 3px 5px 0 0;" v-if="model.name === 'Vue'" />
        {{model.name}}
      </span>

      <div v-if="isFolder" style="position: relative; z-index: 2;">
        <div style="cursor: pointer;"
             :class="{open: open}"
        >
          <b-icon class="my-1" :icon="open ? 'folder-minus' : 'folder-plus'" aria-hidden="true">
          </b-icon>
        </div>
      </div>

    </div>
    <div v-if="model.children.length === 0">
      <multiselect
          v-model="value"
          label="title"
          track-by="title"
          :options="options"
          :show-labels="false"
          :show-pointer="false"
          @select="chooseLevel"
      >
        <template slot="singleLabel" slot-scope="{ option }">
          <div class="d-flex align-items-center">
            <div :class="'custom-bar ' + option.title">
              <div class="custom-bar__option"></div>
              <div class="custom-bar__option"></div>
              <div class="custom-bar__option"></div>
            </div>
            <strong>{{ option.title }}</strong>
          </div>
        </template>

        <template slot="option" slot-scope="props">
          <div class="option__desc"><span class="option__title">{{ props.option.title }}</span></div>
        </template>
      </multiselect>
    </div>
    <b-list-group v-show="open" v-if="isFolder">
      <item
          class="item d-flex"
          v-for="(model, key) in model.children" :key="key"
          :model="model">
      </item>
    </b-list-group>
  </b-list-group-item>
</template>

<script>
import Vue from "vue";
import Multiselect from "vue-multiselect"
import react from "@/components/icons/react";
import javascript from "@/components/icons/javascript";
import java from "@/components/icons/java";
import angular from "@/components/icons/angular";
import csharp from "@/components/icons/csharp";
import vue from "@/components/icons/vue";
export default {
name: "item",
  props: ["model"],
  data: function () {
    return {
      departmentsByLevel: [],
      level: 1,
      open: false,
      value: { title: 'Select level'},
      options: [
        { title: 'Expert', active: false, },
        { title: 'Advanced', active: false, },
        { title: 'Intermediate', active: false, },
      ],
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
    },
    chooseLevel: function (e) {
      console.log(e)
      if (e.title === 'Expert') {
        e.active = true;
      }
    }

  },

  components: {
    csharp,
    Multiselect,
    java,
    javascript,
    react,
    angular,
    vue,
  }
}
</script>

<style scoped>
.custom-bar {
  display: flex;
  align-items: flex-end;
  margin-right: 5px;
}

.custom-bar .custom-bar__option:nth-child(1) {
  width: 4px;
  height: 5px;
  border: 1px solid #333;
  border-radius: 4px;
  background-color: #fff;
  margin-right: 2px;
}

.custom-bar .custom-bar__option:nth-child(2) {
  width: 4px;
  height: 9px;
  border: 1px solid #333;
  border-radius: 4px;
  background-color: #fff;
  margin-right: 2px;
}

.custom-bar .custom-bar__option:nth-child(3) {
  width: 4px;
  height: 14px;
  border: 1px solid #333;
  border-radius: 4px;
  background-color: #fff;
}

.custom-bar.Expert .custom-bar__option {
  background-color: #1ba8c5;
  border: 1px solid #1ba8c5;
}
.custom-bar.Advanced .custom-bar__option:nth-child(1),
.custom-bar.Advanced .custom-bar__option:nth-child(2) {
  background-color: #1ba8c5;
    border: 1px solid #1ba8c5;
}
.custom-bar.Intermediate .custom-bar__option:nth-child(1) {
  background-color: #1ba8c5;
    border: 1px solid #1ba8c5;
}

/*.icon::before {*/
/*  content: '';*/
/*  position: absolute;*/
/*  right: calc(100% - 4px);*/
/*  background-color: #333;*/
/*  width: 6px;*/
/*  height: 1px;*/
/*  top: 10px;*/
/*}*/
/*.icon::after {*/
/*  content: '';*/
/*  position: absolute;*/
/*  left: -2px;*/
/*  background-color: #333;*/
/*  width: 1px;*/
/*  height: 37px;*/
/*  top: 0;*/
/*  z-index: 3;*/
/*}*/
</style>
