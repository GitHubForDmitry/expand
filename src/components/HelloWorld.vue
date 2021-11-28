<template>
  <div class="container">
        <b-list-group v-for="(model, modelIndex) in departments" :key="modelIndex" class="tree">
          <item
              class="item d-flex"
              :model="model">
          </item>
        </b-list-group>
  </div>
</template>

<script>
import item from "@/components/item";
export default {
  name: 'HelloWorld',
  props: {
    model: Object
  },
  components: {
    'item': item
  },
  created() {
    this.findLevelOfDepartment(this.departments);
    console.log(this.departmentsByLevel,'1')
  },
  data: function () {
    return {
      departments: [
      {
        id: 0,
        name: 'Csharp',
        show: true,
        children:[],
      },
      {
        id: 1,
        name: 'Java',
        show: true,
        children:[],
      },
      {
        id: 2,
        name: 'JavaScript',
        show: true,
        children:[
          {
            id: 25,
            name: 'Vue',
            show: true,
            children:[
              {
                id: 5,
                name: 'Vuex',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'VueBootstrap',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'VueBootstrap',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'VueBootstrap',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'Vuex',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'Vuex',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'Vuex',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'Vuex',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'Vuex',
                show: true,
                children:[],
              },
            ],
          },
          {
            id: 4,
            name: 'React',
            show: true,
            children:[
              {
                id: 5,
                name: 'ReactHooks',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactClass',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactClass',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactHooks',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactClass',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactClass',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactHooks',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactClass',
                show: true,
                children:[],
              },
              {
                id: 5,
                name: 'ReactClass',
                show: true,
                children:[],
              },
            ],
          },
          {
            id: 4,
            name: 'Angular',
            show: true,
            children:[

              {
                id: 5,
                name: 'Angular',
                show: true,
                children:[],
              }
            ],
          }
        ],
      },
    ],
      departmentsByLevel: [],
      level: 1,
      open: false,
  }
  },

  methods: {
    findLevelOfDepartment: function (deps) {
      return deps.map(d => {
        this.departmentsByLevel.push({...d, level: this.level})
        if (d.children.length > 0) {
          this.level++;
          this.findLevelOfDepartment(d.children);
          this.level--;
        }
      })
    },
    showSubDeps: function (children) {
      console.log(children,'1')
      if(children.children.length > 0) {
        children.children.forEach(dep => {
          dep.show  = !dep.show
        })
      }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.list-group {
  position: relative;
  top: 25px;
  left: -8px;
}

.list-group-item .list-group {
  top: 25px;
  padding-bottom: 25px;
}

.list-group .list-group-item {
  border: 1px solid transparent;
  padding: 0.2rem;
}

.list-group .list-group-item {
  position: relative;
}

.list-group-item .open::after {
  content: '';
  position: absolute;
  right: 5px;
  height: calc(100% - 46px);
  width: 1px;
  top: 24px;
  z-index: 2;
  background-color: #333333;

}
.multiselect {
  max-width: 180px;
  min-height: 28px;
}
.multiselect .multiselect__tags {
  height: 29px;
  min-height: auto;
  padding: 4px 40px 0 4px;
}

.multiselect .multiselect__select {
  height: 29px;
  min-height: auto;
}
</style>
