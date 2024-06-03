<template>
  <el-drawer @close="active=0" v-model="drawer" title="Connect Database" size="50%">
    <div style="padding-bottom: 20px">
      <el-steps :active="active" finish-status="success" align-center>
        <el-step title="Select Database"/>
        <el-step title="Connect Database"/>
      </el-steps>
    </div>
    <el-form
      label-position="top"
      label-width="auto"
      v-model="data"
    >
      <div v-if="active==0">
        <el-form-item class="mb-2 flex items-center text-sm"
                      label="Please select your database to connect">
          <div class="mb-2 flex items-center text-sm">
            <el-radio-group class="ml-4" v-model="data.config.dbKind">
              <div class="segment-title">Relational</div>
              <el-radio value="mysql" border>MySQL</el-radio>
              <el-radio value="mariadb" border>MariaDB</el-radio>
              <el-radio value="oracle" border>Oracle</el-radio>
              <el-radio value="sqlserver" border>SQL Server</el-radio>
              <el-radio value="postgresql" border>PostgreSQL</el-radio>
              <el-radio value="db2" border>DB2</el-radio>
              <el-radio value="sqlite" border>SQLite</el-radio>
              <el-radio value="gbase" border>GBase</el-radio>
              <el-radio value="dm" border>DM8</el-radio>
              <el-radio value="tidb" border>TiDB</el-radio>
              <div class="segment-title">Document</div>
              <el-radio value="mongodb" border>MongoDB</el-radio>
              <div class="segment-title">Other</div>
              <el-radio value="graphql" border disabled>GraphQL</el-radio>
            </el-radio-group>
          </div>
        </el-form-item>
      </div>
      <div v-if="active==1">
        <el-form-item required label="Connection name">
          <el-input v-model="data.name"/>
        </el-form-item>
        <el-form-item required label="Database URL">
          <el-input v-model="data.url" placeholder="e.g. jdbc:mysql://xxxx"/>
        </el-form-item>
        <el-form-item required label="Username">
          <el-input v-model="data.username"/>
        </el-form-item>
        <el-form-item label="Password">
          <el-input v-model="data.password"/>
        </el-form-item>
      </div>
    </el-form>

    <template #footer>
      <el-button type="primary" style="margin-top: 12px" @click="next">Connect Database</el-button>
    </template>
  </el-drawer>
</template>
<script setup lang="ts">
import {reactive, ref, watchEffect} from "vue";

const props = defineProps(['visible']);
const emits = defineEmits(['confirm', 'cancel']);
const drawer = ref(false);
watchEffect(() => {
  if (props.visible) {
    drawer.value = props.visible;
  }
});
const data = reactive<any>({config: {dbKind: 'mysql'}});

const active = ref(0);

const next = () => {
  if (active.value++ > 2) active.value = 0
}
</script>
<style scoped>
.ep-radio {
  margin: 5px;
}

.ep-radio-group {
  display: flex;
  align-items: center;
}

.segment-title {
  width: 100%;
  font-weight: 600;
  font-size: 15px;
  padding-bottom: 10px;
  padding-top: 10px;
}
</style>