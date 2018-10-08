<template>
  <div class="flex-row list-item"
    :class="itemState">
    <div class="menu-container">
      <div class="flex-row wrapper">
        <i class="material-icons" id="edit" @click="itemState = 'edit' ">edit</i>
        <i class="material-icons" id="delete" @click="deleteItem">delete</i>
        <i class="material-icons" id="menu"
          @click="itemState = 'active' ">more_vert</i>
      </div>
    </div>
    <p class="item-title">{{ item.title }}</p>
    <input ref="item"
      v-model="item.title"
      @blur.prevent="itemState = 'active' "
      @keydown.enter.prevent="updateItem( item )"
      type="text">
  </div>
</template>

<script>
export default {
  name: 'item',
  props:{
    item: Object,
  },
  data: () => ({
    itemState : '',
  }),
  watch : {

    itemState( state ){

      if( state === 'edit' ){
        this.$refs.item.focus();
      }

    }

  },
  methods: {

    updateItem( item ){

      const updatedItem = Object.assign( item, { index : this.$vnode.key });

      this.$emit( 'update-item', updatedItem );
      this.itemState = '';
    },

    deleteItem(){

      const index = this.$vnode.key;

      this.$emit( 'delete-item', index );
      this.itemState = '';
    }

  },
};
</script>

<style lang="scss" scoped>

@import '../globals/variables.scss';

  .list-item{
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid $lightGray;

    .menu-container{
      transition: all .2s ease;
      width: 35px;

      i.material-icons{
        transition: all .2s ease;
        cursor: pointer;

        &#edit, &#delete{
          font-size: 0;
        }

        &#edit{
          color: $googleYellow;
        }

        &#delete{
          color: $googleRed;
        }

      }
    }

    input{
      border: none;
      outline: none;
      opacity: 0;
      text-indent: 10px;
      font-size: 1em;
      height: 0;
      color: $darkBlue;
    }

  }

  .list-item.edit{

    .menu-container{
      width: 25px;
    }

    i.material-icons{

      &#edit{
        font-size: 24px;
      }

      &#delete, &#menu{
        font-size: 0;
      }
    }

    .item-title{
      display: none;
    }

    input{
      opacity: 1;
      height: 100%;
    }

  }

  .list-item.active{

    .menu-container{
      width: 75px;

      .wrapper{
        justify-content: space-evenly;

        i.material-icons{
          cursor: pointer;

          &#edit, &#delete{
            font-size: 24px;

            &:hover{
              opacity: .8;
            }
          }

          &#menu{
            font-size: 0;
          }
        }
      }
    }
  }


</style>

