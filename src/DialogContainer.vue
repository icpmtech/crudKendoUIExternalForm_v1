<template>
  <KDialog @close="cancel">
    <div class="k-form k-form-horizontal">
      <div class="k-form-field">
        <label for="Employee" class="k-form-label">Title</label>
        <div class="k-form-field-wrap">
          <KInput
            :style="{ width: '230px' }"
            :name="'Title'"
            v-model="employeeInEdit.title"
          ></KInput>
        </div>
      </div>
      <div class="k-form-field">
        <label for="Position" class="k-form-label">Maturity Atual</label>
        <div class="k-form-field-wrap">
          <KInput
            :style="{ width: '230px' }"
            :name="'Maturity Atual'"
            v-model="employeeInEdit.maturityAtual"
          ></KInput>
        </div>
      </div>
      <div class="k-form-field">
        <KLabel :editor-id="'FullTime'"> Maturity Expected </KLabel>
        <div class="k-form-field-wrap">
          <KInput
            :name="'Maturity Expected'"
            :id="'maturityExpected'"
            v-model="employeeInEdit.maturityExpected"
          />
        </div>
      </div>
    </div>
    <DialogActionsBar>
      <KButton @click="cancel"> Cancel </KButton>
      <KButton :theme-color="'primary'" @click="save"> Save </KButton>
    </DialogActionsBar>
  </KDialog>
</template>
<script>
import {
  Dialog as KDialog,
  DialogActionsBar,
} from '@progress/kendo-vue-dialogs';
import { Input as KInput, Checkbox } from '@progress/kendo-vue-inputs';
import { Label } from '@progress/kendo-vue-labels';
import { Button } from '@progress/kendo-vue-buttons';

export default {
  components: {
    KInput,
    KDialog,
    DialogActionsBar,
    KButton: Button,
    checkbox: Checkbox,
    KLabel: Label,
  },
  props: {
    dataItem: Object,
  },
  data: function () {
    return {
      employeeInEdit: {
        title: '',
        position: 0,
        fullTime: false,
      },
    };
  },
  created: function () {
    if (this.$props.dataItem) {
      this.employeeInEdit = this.$props.dataItem;
    }
  },
  methods: {
    cancel() {
      this.$emit('cancel');
    },
    save() {
      this.$emit('save', this.employeeInEdit);
    },
  },
};
</script>
