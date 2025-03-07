<template>
  <BContainer fluid>
    <BRow>
      <BCol>
        <h4 class="my-3">Table without field definitions</h4>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <BTable responsive caption="List of users" :items="items" striped hover foot-clone>
          <template #cell(first_name)="data">
            <a href="#">{{ data.value }}</a>
          </template>
        </BTable>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <h4 class="my-3">Table with string field definitions</h4>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <BTable
          responsive
          caption="List of users"
          :items="items"
          :fields="stringTableDefinitions"
          striped
          hover
          foot-clone
        >
          <template #cell(first_name)="data">
            <a href="#">{{ data.value }}</a>
          </template>
        </BTable>
      </BCol>
    </BRow>

    <BRow>
      <BCol>
        <h4 class="my-3">Table with object field definitions</h4>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <BTable
          responsive
          caption="List of users"
          :items="items"
          :fields="objectTableDefinitions"
          striped
          hover
          foot-clone
        >
          <template #cell(first_name)="data">
            <a href="#">{{ data.value }}</a>
          </template>
        </BTable>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <h4 class="my-3">Selectable table</h4>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <BFormCheckbox
          id="checkbox-1"
          v-model="showSelectBox"
          name="checkbox-1"
          :value="true"
          :unchecked-value="false"
          >Show Selectable column</BFormCheckbox
        >
      </BCol>
      <BCol>
        <p>Selection mode</p>
        <BFormRadio
          v-for="(car, index) in ['single', 'multi', 'range']"
          :key="index"
          v-model="selectionMode"
          :value="car"
          >{{ car }}</BFormRadio
        >
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <BTable
          selectable
          :select-head="showSelectBox"
          caption="List of users"
          :select-mode="selectionMode"
          :items="items"
          :fields="objectTableDefinitions"
          striped
          hover
          foot-clone
          @selection="selectClick"
        >
          <template #cell(first_name)="data">
            <a href="#">{{ data.value }}</a>
          </template>
        </BTable>
      </BCol>
      <div>Selected rows: {{ selection }}</div>
    </BRow>
    <BRow>
      <BCol>
        <h4 class="my-3">Table: Adding additional rows to the header</h4>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <BTable
          responsive
          caption="List of users"
          :items="items"
          :fields="objectTableDefinitions"
          striped
          hover
          foot-clone
          dark
        >
          <template #thead-top>
            <tr class="my">
              <th colspan="2">
                <span class="sr-only">List of users</span>
              </th>
            </tr>
          </template>
          <template #thead-sub="{key, label}">
            <tr class="my">
              <th variant="danger" />
              <th variant="danger">
                <BFormSelect :placeholder="label" :options="[label, key]" label-field="label" />
              </th>
            </tr>
          </template>
          <template #cell(first_name)="data">
            <a href="#">{{ data.value }}</a>
          </template>
        </BTable>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <h4 class="my-3">Refreshable table (displays current time)</h4>
        <BButton @click="currentTimeTable?.refresh()">Refresh</BButton>
      </BCol>
    </BRow>
    <BRow>
      <BCol>
        <BTable
          ref="currentTimeTable"
          responsive
          :fields="currentTimeTableDefinitions"
          :provider="currentTimeProvider"
        />
      </BCol>
    </BRow>
  </BContainer>
</template>

<script setup lang="ts">
import {ref} from 'vue'
import type {TableField, TableItem, BTable} from 'bootstrap-vue-next'

const stringTableDefinitions = ref(['last_name', 'first_name', 'age'])
const objectTableDefinitions = ref<TableField[]>([
  {
    key: 'last_name',
    label: 'Family name',
    formatter: (value: unknown) => (typeof value === 'string' ? value.toUpperCase() : `${value}`),
  },
  {key: 'first_name', label: 'Given name'},
  {key: 'age', label: 'Age', formatter: (value: unknown) => `${value} years`},
])
const items: TableItem[] = [
  {age: 40, first_name: 'Dickerson', last_name: 'Macdonald'},
  {age: 21, first_name: 'Larsen', last_name: 'Shaw'},
  {age: 89, first_name: 'Geneva', last_name: 'Wilson'},
  {
    age: 89,
    first_name: 'Michael',
    last_name: 'Jordan',
    _rowVariant: 'primary',
    _cellVariants: {last_name: 'danger'},
  },
  {age: 38, first_name: 'Jami', last_name: 'Carney'},
]
const selection = ref<TableItem[]>([])
const showSelectBox = ref(false)
const selectionMode = ref<'single' | 'range' | 'multi'>('single')
function selectClick(selected: TableItem[]) {
  console.log(selected)
  selection.value = selected
}

const currentTimeTable = ref<typeof BTable | null>(null);
const currentTimeTableDefinitions: TableField[] = [
  {key: 'hours', label: 'Hours'},
  {key: 'minutes', label: 'Minutes'},
  {key: 'seconds', label: 'Seconds'},
  {key: 'milliseconds', label: 'Milliseconds'}
];
const currentTimeProvider = (): TableItem[] => {
  const now = new Date()

  return [
    {
      hours: now.getHours(),
      minutes: now.getMinutes(),
      seconds: now.getSeconds(),
      milliseconds: now.getMilliseconds(),
    },
  ]
}
</script>
