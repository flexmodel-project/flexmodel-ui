<template>
  <el-row>
    <el-col :span="4">
      <el-card shadow="never">
        <div class="datasource-wrap">
          <div
            class="ds-item"
            :class="{ 'ds-item-active': item.name === activeDs.name }"
            v-for="(item, index) in dsList"
            :key="index"
            @click="handleItemChange(item)"
          >
            {{ item.name }}
          </div>
        </div>
        <el-divider/>
        <div>
          <el-button type="primary" @click="toggleSelection()" style="width: 100%">连接数据源</el-button>
        </div>
      </el-card>
    </el-col>
    <el-col :span="20">
      <el-row>
        <el-col>
          <el-card>
            <el-row>
              <el-col :span="12">{{ activeDs.name }}</el-col>
              <el-col :span="12" style="text-align: right">
                <el-button @click="router.push(`/modeling?datasource=${activeDs.name}`)">建模</el-button>
                <el-button type="info">刷新</el-button>
                <el-button type="info">测试</el-button>
                <el-button type="info">编辑</el-button>
              </el-col>
            </el-row>
          </el-card>
        </el-col>
        <el-col>
          <el-card shadow="never">
            <el-descriptions border column="1">
              <el-descriptions-item label="连接名称">{{ activeDs.name }}</el-descriptions-item>
              <el-descriptions-item label="连接类型">{{ activeDs.type }}</el-descriptions-item>
              <el-descriptions-item label="数据库类型">{{ activeDs.config?.dbKind }}</el-descriptions-item>
              <el-descriptions-item label="连接地址">{{ activeDs.config?.url }}</el-descriptions-item>
              <el-descriptions-item label="用户名">{{ activeDs.config?.username }}</el-descriptions-item>
              <el-descriptions-item label="密码">
                <PasswordHide :text="activeDs.config?.password"/>
              </el-descriptions-item>
              <el-descriptions-item label="创建时间">{{ activeDs.createTime }}</el-descriptions-item>
            </el-descriptions>
          </el-card>
        </el-col>
      </el-row>

    </el-col>
  </el-row>
</template>
<script setup lang="ts">
import {ElMessage} from "element-plus";
import {getDatasourceList} from "~/api/datasource";
import {ref} from "vue";
import {useRouter} from "vue-router";
import PasswordHide from "~/components/PasswordHide.vue";

const router = useRouter();
const dsList = ref<Datasource[]>([]);
const activeDs = ref<Datasource>({});

const reqDatasourceList = async () => {
  try {
    dsList.value = await getDatasourceList();
    activeDs.value = dsList.value[0]
  } catch (error) {
    ElMessage.error(error as Error);
  }
};
const handleItemChange = (item: Datasource) => {
  activeDs.value = item;
}
reqDatasourceList();
</script>
<style scoped lang="scss">
.datasource-wrap {
  position: relative;
  padding: 5px;

  .ds-item {
    font-size: 14px;
    height: 32px;
    line-height: 32px;
    cursor: pointer;
    white-space: nowrap;
    display: flex;
    align-items: center;
    padding: 0 4px;

    &:hover {
      background-color: #f5f7fa;
    }

    &-active {
      background-color: #ecf5ff;
    }
  }
}
</style>