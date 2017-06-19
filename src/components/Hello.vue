<template>
  <div class="fluid container">
    <!--<div class="form-group form-group-lg panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Sortbale control</h3>
      </div>
      <div class="panel-body">
        <div class = "checkbox">
          <label><input type = "checkbox" v-model="editable">Enable drag and drop</label>      
        </div>
        <button type="button" class="btn btn-default" @click="orderList">Sort by original order</button>
      </div>
    </div>-->

    <div class="col-md-3 col-md-3-height">
      <draggable class="list-group" element="ul" v-model="list" :options="dragOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false"> 
          <transition-group type="transition" :name="'flip-list'">
            <a class="list-group-item list-group-item-dash" v-for="element in list" :key="element.order"> 
              {{element.name}}
              <span class="badge">{{element.number}}</span>
            </a> 
          </transition-group>
      </draggable>
    </div>

  </div>
</template>

<script>
import draggable from 'vuedraggable'

const message = [
  'vue.draggable', 'draggable', 'component', 'for', 'vue.js 2.0', 'based', 'on', 'Sortablejs',
]

export default {
  name: 'hello',
  components: {
    draggable,
  },
  data () {
    return {
      list: message.map((name, index) => {
        return {name, order: index+1, fixed: false, number: index+1}
      }),
      editable:true,
      isDragging: false,
      delayedDragging:false
    }
  },
  methods:{
    onMove ({relatedContext, draggedContext}) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      if (relatedElement && relatedElement.fixed) return false;

      return true;
    },
    // orderList () {
    //   this.list = this.list.sort((one,two) =>{return one.order-two.order; })
    // },
  },
  computed: {
    dragOptions () {
      return  {
        animation: 0,
        group: 'description',
        disabled: !this.editable,
        ghostClass: 'ghost'
      };
    },
    // list1: this.baseList.filter((elem) => {
    //   return elem % 3 === 1;
    // }),
  },
  watch: {
    isDragging (newValue) {
      if (newValue){
        this.delayedDragging= true
        return
      }
      this.$nextTick( () =>{
        this.delayedDragging =false
      })
    }
  }
}
</script>

<style>
.flip-list-move {
  transition: transform 0.5s;
}

.no-move {
  transition: transform 0s;
}

.ghost {
  opacity: .5;
  background: #C8EBFB;
}

.list-group {
  min-height: 150px;
}

.list-group-item {
  cursor: move;
}

.list-group-item i{
  cursor: pointer;
}

.list-group-item-dash {
  width: 250px;
  height: 250px;
  margin: 10px;
}

.list-group>span{
    display: flex;
    width: 900px;
    flex-wrap: wrap;
}

</style>
