<template>
  <div class="container">
        <ul v-for="(model, modelIndex) in departments" :key="modelIndex">
          <item
              class="item"
              :model="model">
          </item>
        </ul>
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
        id: 1,
        name: 'Sales',
        show: true,
        children:[],
      },
      {
        id: 2,
        name: 'Product',
        show: true,
        children:[
          {id: 4,
            name: 'SubProduct',
            show: true,
            children:[],
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
<style scoped>
body {
  font-family: Menlo, Consolas, monospace;
  color: #444;
}
.item {
  cursor: pointer;
}
.bold {
  font-weight: bold;
}
::v-deep ul {
  margin: 0 0 0 1rem;
  padding: 0;
  list-style: none;
  position: relative;
}
ul ul {
  margin-left: 0.5em;
}
ul:before {
  content: "";
  display: block;
  width: 0;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  border-left: 1px solid;
}
ul li:before {
  content: "";
  display: block;
  width: 10px;
  height: 0;
  border-top: 1px solid;
  margin-top: -1px;
  position: absolute;
  top: 1em;
  left: 0;
}
ul li:last-child:before {
  background: #fff;
  height: auto;
  top: 1em;
  bottom: 0;
}
li {
  margin: 0;
  padding: 0 1em;
  line-height: 2em;
  color: #369;
  font-weight: 700;
  position: relative;
}
li .expand {
  display: block;
}
li .collapse {
  display: none;
}
li a {
  text-decoration: none;
  color: #369;
}
li button {
  text-decoration: none;
  color: #369;
  border: none;
  background: transparent;
  margin: 0px 0px 0px 0px;
  padding: 0px 0px 0px 0px;
  outline: 0;
}
li button:active {
  text-decoration: none;
  color: #369;
  border: none;
  background: transparent;
  margin: 0px 0px 0px 0px;
  padding: 0px 0px 0px 0px;
  outline: 0;
}
li button:focus {
  text-decoration: none;
  color: #369;
  border: none;
  background: transparent;
  margin: 0px 0px 0px 0px;
  padding: 0px 0px 0px 0px;
  outline: 0;
}
</style>
