<template>
    <div class='block'>
        <div   @click="$emit('click')" class="cell">
        <span v-if="value !== ''" class="cellvalue">{{ value }}</span>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'CellComponent',
    props: {
      value: String,
    }
  }
</script>
<style scoped>
    html{
        padding-top: 100px;
    }
  .cell {
    width: 100px;
    height: 100px;
    border: 3px solid rgba(15, 17, 46, 0.877);
    font-size: 42px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor:pointer;
    
  }
  
  .cell :hover{
  box-shadow: 2px 2px 34px -5px rgba(100, 104, 155, 0.9);  
  }
  .cellvalue{
      box-shadow: 2px 2px 34px -5px rgba(100, 104, 155, 0.4);  

  }
</style>

