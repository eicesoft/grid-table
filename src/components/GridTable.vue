<template>
  <div class="grid-table-container">
    <div class="grid-table" :style="cssVars">
      <div class="grid-table-header">
        <div class="grid-table-tr">
          <div class="grid-table-th" v-for="(column, index) in columns" :key="index"> {{ column.label }}</div>
        </div>
      </div>

      <div class="grid-table-body">
        <div
          :class="{ activeRow: index == selectIndex }"
          class="grid-table-tr"
          v-for="(row, index) in rows"
          :key="index"
        >
          <div
            ref="xx"
            @click="select(index)"
            @dblclick="dblClickColumn({ rowIndex: index, colIndex: colIndex })"
            class="grid-table-td"
            v-for="(column, colIndex) in columns"
            :key="index"
          >
            {{ row[column.name] ?? '' }}</div
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
  import { Column } from '~/components/grid';
  import { PropType, computed, ref } from 'vue';

  const props = defineProps({
    columns: {
      type: Array as PropType<Column[]>,
    },
    rows: {
      type: Array as PropType<Record<string, any>[]>,
    },
    rowHeight: {
      type: Number,
      default: 24,
    },
    maxWidth: {
      type: Number,
      default: 200,
    },
  });

  const cssVars = computed(() => {
    return {
      '--rowHeight': props.rowHeight + 'px',
    };
  });

  const selectIndex = ref(-1);
  const xx = ref();

  console.log(props);

  const select = (index: number) => {
    selectIndex.value = index;
  };

  const dblClickColumn = (index: any) => {
    let column = xx.value[index.rowIndex * props.columns?.length + index.colIndex];
    console.log(column);
    column.setAttribute('contenteditable', true);
  };
</script>

<style lang="scss" scoped>
  $border: 1px solid #c7c7c7;
  $cell-padding: 0 6px;
  $cell-min-width: 80px;
  $font-size: 11px;
  $font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana,
    sans-serif;
  .grid-table-container {
    display: block;
    max-height: 200px;
    overflow: auto;
    width: 100%;
    position: relative;
    table-layout: fixed;
    border: $border;

    &::-webkit-scrollbar {
      width: 5px;
      height: 5px;
    }

    /* 滚动槽 */
    &::-webkit-scrollbar-track {
      border-radius: 10px;
      /* background-color: rgba(216, 216, 216, 0.5); */
    }

    &::-webkit-scrollbar-thumb {
      border-radius: 10px;
      background-color: rgb(226, 232, 241);
    }
  }
  .grid-table {
    display: table;
    width: 100%;
    font-size: $font-size;
    // border-collapse: collapse;
    font-family: $font-family;
    table-layout: fixed;

    .activeRow {
      .grid-table-td {
        background-color: #bee0ff71 !important;
      }
    }
    .grid-table-body {
      // max-height: 100px;
      display: table-row-group;
      position: relative;
      // top: var(--rowHeight);
      overflow-y: auto;
      border-collapse: collapse;
      // margin-top: var(--rowHeight);
    }
    .grid-table-header {
      display: table-header-group;
      position: sticky;
      z-index: 1;
      top: 0;
    }
    .grid-table-tr {
      display: table-row;

      .grid-table-th {
        user-select: none;

        border-bottom: $border;
        display: table-cell;
        height: var(--rowHeight);
        line-height: var(--rowHeight);
        padding: $cell-padding;
        min-width: $cell-min-width;
        text-align: center;
        background-color: #eeeeee;
        cursor: pointer;
        font-weight: 600;
        color: #303030;
      }

      .grid-table-td {
        user-select: auto;
        color: #7e7e7e;
        border-right: $border;
        border-bottom: $border;
        display: table-cell;
        height: var(--rowHeight);
        line-height: var(--rowHeight);
        font-weight: 500;
        padding: $cell-padding;
        min-width: $cell-min-width;
        text-align: center;
        background-color: #fefefe;
        overflow: hidden;
        text-overflow: ellipsis; //溢出用省略号显示
        white-space: nowrap;
        &:last-child {
          border-right: 1px solid #fff !important;
          border-right: 1px solid #fff !important;
        }
      }
    }
  }
</style>
