<template>
  <div id="list" :class="listState">
    <div class="wrapper">
      <div id="title" class="flex-row">
        <h1>Todo List</h1>
        <i class="material-icons"
          @click="listState ='add' ">add</i>
      </div>
      <div class="flex-column">
        <Item v-for="( item, i ) in list"
          :item="item"
          :key="i"
          :listState="listState"
          v-on:update-item="updateItem"
          v-on:delete-item="deleteItem"/>
        <div class="item-input">
          <input placeholder="Walk the dog etc..."
          ref="itemInput"
          v-model="newItem"
          @blur.prevent="resetForm"
          @keydown.enter.prevent="handleInput( newItem )"
          type="text">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Item from './Item.vue';

export default {
  name: 'list',
  components: {
    Item,
  },
  data: () => ({
    listState: '',
    newItem : '',
    list: [
      {
        title : 'butt stuff0',
        state : '',
      },
      {
        title : 'butt stuff1',
        state : '',
      },
      {
        title : 'butt stuff2',
        state : '',
      },

    ]

  }),
  watch:{

    listState( state ){

      if( state === 'add' ){
        this.$refs.itemInput.focus();
      }

    },
  },
  methods: {

    handleInput( title ){

      const newItem = { title, state : '' };

      this.list.push( newItem );
      this.resetForm();
    },

    updateItem( item ){

      const { index } = item;

      this.list[index].title = item.title;
    },

    deleteItem( value ){

      this.list.splice( value, 1 );
      this.resetForm();
    },

    resetForm(){

      this.listState = '';
      this.newItem = '';
    },

  },
};
</script>

<style lang="scss" scoped>

  @import '../globals/variables';

  #list{
    position: relative;
    width: 800px;
    margin: 100px auto;
    box-shadow: $boxShadow;

    .wrapper{
      padding: 20px 30px;
    }

    .item-input{
      transition: all .2s ease;
      height: 0;

      input{
        opacity: 0;
        height: 100%;
      }

    }


  }

  input{
    outline: none;
    border: none;
  }

  // list states
  #list.add{

    #title{

      i{
        font-size: 0px;
      }
    }

    .item-input{
      height: 45px;
      align-items: center;
      border-bottom: 1px solid $lightGray;

      input{
        opacity: 1;
        text-indent: 10px;
        font-size: 1em;
        width: 100%;
        color: $darkBlue;
      }

    }

  }

  #title{
    align-items: center;

    h1{
      text-align: center;
      flex: 3 1 0;
    }

    i.material-icons{
      transition: all .2s ease;
      cursor: pointer;
      font-size: 36px;

      &:hover{
        color: $lightGreen;
        transform: rotate(-90deg);
      }
    }
  }

</style>
