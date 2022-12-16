<template>
  <h1>Edit Maturity Matrix</h1>
  <div class="example-wrapper">
    <TreeList
      :dataItems="controls"
      :style="{
        maxHeight: '510px',
        overflow: 'auto',
      }"
      :editField="editField"
      :expandField="expandField"
      :subItemsField="subItemsField"
      @expandchange="onExpandChange"
      :columns="columns"
    >
      <template v-slot:myTemplate="{ props }">
        <CommandCell
          :data-item="props.dataItem"
          @edit="edit"
          @remove="remove"
        />
      </template>
    </TreeList>
    <DialogContainer
      v-if="itemInEdit"
      :data-item="itemInEdit"
      @change="onItemChange"
      @save="save"
      @cancel="cancel"
    >
    </DialogContainer>
  </div>
</template>
<script>
import {
  TreeList,
  mapTree,
  extendDataItem,
  removeItems,
} from '@progress/kendo-vue-treelist';
import samplecontrols from './data';
import DialogContainer from './DialogContainer.vue';
import CommandCell from './CommandCell.vue';
import { Button } from '@progress/kendo-vue-buttons';

export default {
  components: {
    TreeList,
    DialogContainer,
    CommandCell,
    KButton: Button,
  },
  computed: {
    controls() {
      return mapTree(this.dataItems, this.subItemsField, (item) =>
        extendDataItem(item, this.subItemsField, {
          [this.expandField]: this.expanded.includes(item.id),
        })
      );
    },
  },
  data: function () {
    return {
      subItemsField: 'subcontrols',
      expandField: 'expanded',
      editField: 'inEdit',
      dataItems: [...samplecontrols],
      itemInEdit: undefined,
      expanded: [1, 2, 32],
      columns: [
        {
          field: 'id',
          title: 'ID',
          width: '30px',
          expandable: true,
        },
        {
          field: 'title',
          title: 'Title',
          width: '280px',
          editor: 'text',
          expandable: true,
        },
        {
          field: 'maturityAtual',
          title: 'Maturity Atual',
          width: '30px',
          expandable: true,
        },
        {
          field: 'maturityExpected',
          title: 'Maturity Expected',
          width: '30px',
          editor: 'text',
          expandable: true,
        },
        {
          title: 'Edit',
          cell: 'myTemplate',
          width: '210px',
        },
      ],
    };
  },
  methods: {
    edit(dataItem) {
      this.itemInEdit = extendDataItem(dataItem, this.subItemsField);
    },
    remove(dataItem) {
      this.dataItems = removeItems(
        this.dataItems,
        this.subItemsField,
        (i) => i.id === dataItem.id
      );
    },
    save(dataitem) {
      this.dataItems = mapTree(this.dataItems, this.subItemsField, (item) =>
        dataitem.id === item.id ? dataitem : item
      );

      this.itemInEdit = undefined;
    },
    cancel() {
      this.itemInEdit = undefined;
    },
    onItemChange(itemInEdit) {
      this.itemInEdit = itemInEdit;
    },
    onExpandChange(e) {
      this.expanded = e.value
        ? this.expanded.filter((id) => id !== e.dataItem.id)
        : [...this.expanded, e.dataItem.id];
    },
  },
};
</script>
