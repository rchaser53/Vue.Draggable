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

    <!--<div  class="col-md-3">
        <draggable class="list-group" element="ul" v-model="list" :options="dragOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false"> 
          <transition-group type="transition" :name="'flip-list'">
            <li class="list-group-item" v-for="element in list" :key="element.order"> 
              <i :class="element.fixed? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin'" @click=" element.fixed=! element.fixed" aria-hidden="true"></i>
              {{element.name}}
              <span class="badge">{{element.order}}</span>
            </li> 
          </transition-group>
      </draggable>
    </div>-->

    <div class="col-md-3 col-md-3-height">
      <draggable class="list-group" element="ul" v-model="list1" :options="dragOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false"> 
          <transition-group type="transition" :name="'flip-list'">
            <a class="list-group-item nyan" v-for="element in list1" :key="element.order"> 
              {{element.name}}
              <span class="badge">{{element.number}}</span>
            </a> 
          </transition-group>
      </draggable>
    </div>

     <div class="col-md-3 col-md-3-height">
      <draggable element="span" v-model="list2" :options="dragOptions" :move="onMove"> 
          <transition-group name="no" class="list-group" tag="ul">
            <li class="list-group-item nyan" v-for="element in list2" :key="element.order"> 
              {{element.name}}
              <span class="badge">{{element.number}}</span>
            </li> 
          </transition-group>
      </draggable>
    </div>

     <div class="col-md-3 col-md-3-height">
      <draggable element="span" v-model="list3" :options="dragOptions" :move="onMove"> 
          <transition-group name="no" class="list-group" tag="ul">
            <li class="list-group-item nyan" v-for="element in list3" :key="element.order"> 
              {{element.name}}
              <span class="badge">{{element.number}}</span>
            </li> 
          </transition-group>
      </draggable>
    </div>

  </div>
</template>

<script>
import draggable from 'vuedraggable'
// const message1 = [ 'vue.draggable', 'for', 'on']
// const message2 = [ 'draggable', 'vue.js 2.0', 'Sortablejs' ]
// const message3 = [ 'component', 'based']

const message = [
  'vue.draggable', 'draggable', 'component', 'for', 'vue.js 2.0', 'based', 'on', 'Sortablejs'
]

export default {
  name: 'hello',
  components: {
    draggable,
  },
  data () {
    return {
      baseList: message.map((name, index) => {
        return {name, order: index+1, fixed: false, number: index+1}
      }),
      list1: message.map((name, index) => {
        return {name, order: index+1, fixed: false, number: index+1}
      }).filter((elem) => elem.order % 3 === 1),
      list2: message.map((name, index) => {
        return {name, order: index+1, fixed: false, number: index+1}
      }).filter((elem) => elem.order % 3 === 2),
      list3: message.map((name, index) => {
        return {name, order: index+1, fixed: false, number: index+1}
      }).filter((elem) => elem.order % 3 === 0),
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

      this.sortItem(draggedElement, relatedElement)
      return true;
    },
    sortItem (draggedElement, relatedElement) {
      // const dropedOrder = relatedElement.order;
      // const draggedOrder =draggedElement.order;

      // this.baseList[dropOrder-1] = dr

      // 上半分にドラッグされたのか下半分にドラッグされたのかが判定できないので駄目そう
      // eventtargetがそのまま欲しかった
      console.log(draggedElement, relatedElement)
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

.nyan {
  width: 250px;
  height: 250px;

}


</style>
