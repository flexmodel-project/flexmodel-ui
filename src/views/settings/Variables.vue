<template>
  <el-card>
    <template #header>
      <el-row>
        <el-col :span="12">Variables</el-col>
        <el-col :span="6">
        </el-col>
        <el-col :span="6">
          <el-input placeholder="Search variables" v-model="filterKeyword" clearable/>
        </el-col>
      </el-row>
    </template>

    <el-scrollbar height="540px">
      <el-row>
        <el-col>
          <div class="font-bold pt7 pb7">Environment variables</div>
          <el-table :data="environmentVariables" stripe style="width: 100%">
            <el-table-column prop="key" label="Variable Name" width="500px"/>
            <el-table-column prop="value" label="Variable value">
              <template #default="{row}">
                <HidePassword :text="row.value"/>
              </template>
            </el-table-column>
          </el-table>
        </el-col>
        <el-col>
          <div class="font-bold pt7 pb7">System variables</div>
          <el-table :data="systemVariables" stripe style="width: 100%">
            <el-table-column prop="key" label="Variable Name" width="500px"/>
            <el-table-column prop="value" label="Variable value">
              <template #default="{row}">
                <HidePassword :text="row.value"/>
              </template>
            </el-table-column>
          </el-table>
        </el-col>
      </el-row>
    </el-scrollbar>
  </el-card>
</template>
<script setup lang="ts">

import HidePassword from "~/components/HidePassword.vue";
import {computed, onMounted, ref} from "vue";
import {getVariables} from "~/api/environment";

const variables = ref<any>({});
const filterKeyword = ref<string>('');

const environmentVariables = computed(() => {
  const list: any[] = [];
  const keys = Object.keys(variables.value['environment'] || {});
  keys.filter(k => k.includes(filterKeyword.value)).forEach(key => list.push({
    key: key,
    value: variables.value['environment'][key]
  }));
  return list;
});
const systemVariables = computed(() => {
  const list: any[] = [];
  const keys = Object.keys(variables.value['system'] || {});
  keys?.filter(k => k.includes(filterKeyword.value)).forEach(key => list.push({
    key: key,
    value: variables.value['system'][key]
  }));
  return list;
});

const reqVariables = async () => {
  variables.value = await getVariables()
}

onMounted(() => {
  reqVariables();
});
</script>

<style scoped>

</style>
