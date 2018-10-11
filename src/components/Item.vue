<template>
  <div class="flex-row list-item"
    :class="rowState">
    <div class="flex-row main-row "
      :class="item.state">
      <div class="menu-container">
        <div class="flex-row wrapper">
          <i class="material-icons" id="edit" @click.stop="rowState = 'edit' ">edit</i>
          <i class="material-icons" id="delete" @click.stop="deleteItem">delete</i>
          <i class="material-icons" id="menu"
            @click.stop="rowState = 'active' ">more_vert</i>
        </div>
      </div>
      <p class="item-title">{{ item.title }}</p>
      <input ref="item"
        v-model="item.title"
        @blur.stop="rowState = 'active' "
        @keydown.enter.stop="updateItem( item )"
        type="text">
    </div>
    <div class="flex-row completion-state"
      :class="item.state">
      <i class="material-icons" @click.stop.self="updateItemState( item )">adjust</i>
      <i class="material-icons">panorama_fish_eye</i>
    </div>
  </div>
</template>

<script>
export default {
  name: 'item',
  props:{
    item: Object,
  },
  data: () => ({
    rowState : '',
  }),
  watch : {

    rowState( state ){

      if( state === 'edit' ){
        this.$refs.item.focus();
      }
    }

  },
  methods: {

    updateItem( item ){[]

      this.$emit( 'update-item', item );
      this.rowState = '';
    },

    updateItemState( item ){

      const state = item.state ? '' : 'complete';

      const updatedItem = Object.assign( item, { state });

      this.$emit( 'update-item-state', updatedItem );
      this.rowState = '';

    },

    deleteItem(){

      const index = this.$vnode.key;

      this.$emit( 'delete-item', index );
      this.rowState = '';
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

    .main-row{
      flex: 1 1 0;

      &.complete{
        p, i.material-icons{
          pointer-events: none;
          color: $lightGray;
        }
        p{
          text-decoration: line-through;
        }
      }
    }

    .completion-state{

      &:hover{

        i.material-icons:nth-of-type(1){
          display: block;
          color: $googleGreen;
        }
      }

      i.material-icons{
        cursor: pointer;
        transition: all .2s ease;

        &:nth-of-type(1){
          position: absolute;
          display: none;
        }
      }

      &.complete{
        color: $lightGray;

        i.material-icons:hover{
          color: black;
        }
      }
    }

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

    .completion-state{
      position: relative;

      i.material-icons{

      }
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

