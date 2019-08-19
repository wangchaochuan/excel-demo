<template>
  <div class="hello">
    <div>
      <el-button type="primary" id="import" class="clearfix" @click="selectFile">导入</el-button>
      <el-button type="primary" id="export" class="clearfix" @click="dialogFormVisible=true">导出</el-button>
    </div>
    <el-table :data="tableData" border style="width: 100%" id="table">
      <template v-if="title.length>0">
        <el-table-column
          :prop="item"
          :label="item"
          v-for="item in title"
          :key="item"
          align="center"
        ></el-table-column>
      </template>
      <template v-else-if="title.length == 0">
        <el-table-column prop="date" label="日期" align="center"></el-table-column>
        <el-table-column label="支出明细" align="center">
          <el-table-column prop="prop1" label="支出项" align="center"></el-table-column>
          <el-table-column prop="prop2" label="金额" align="center"></el-table-column>
        </el-table-column>
        <el-table-column label="收入" align="center">
          <el-table-column prop="prop3" label="收入项" align="center"></el-table-column>
          <el-table-column prop="prop4" label="金额" align="center"></el-table-column>
        </el-table-column>
        <el-table-column prop="prop5" label="每日支出金额" align="center"></el-table-column>
      </template>
    </el-table>
    <el-dialog title="导出参数" :visible.sync="dialogFormVisible" width="30%">
      <el-form :model="form" :rules="rules">
        <el-form-item label="文件名称" :label-width="formLabelWidth" prop="name">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="后缀名" :label-width="formLabelWidth" prop="suffix">
          <el-select v-model="form.suffix" placeholder="请选择文件后缀名">
            <el-option label="xlsx" value="xlsx"></el-option>
            <el-option label="csv" value="csv"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="exportExcel">确 定</el-button>
      </div>
    </el-dialog>
    <input type="file" id="file" @change="importFile" />
  </div>
</template>

<script>
import untils from "../untils/excel";
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      title: [],
      tableData: [
        {
          date: "4-1",
          prop1: "吃饭",
          prop2: 28,
          prop3: "",
          prop4: "",
          prop5: 28
        },
        {
          date: "4-2",
          prop1: "吃饭",
          prop2: 28,
          prop3: "",
          prop4: "",
          prop5: 28
        },
        {
          date: "4-3",
          prop1: "吃饭",
          prop2: 28,
          prop3: "",
          prop4: "",
          prop5: 28
        },
        {
          date: "4-4",
          prop1: "吃饭",
          prop2: 28,
          prop3: "",
          prop4: "",
          prop5: 28
        }
      ],
      dialogFormVisible: false,
      form: {
        name: "",
        suffix: "csv"
      },
      rules: {
        name: [{ required: true, message: "请输入文件名称", trigger: "blur" }]
      },
      formLabelWidth: "100px"
    };
  },
  methods: {
    exportExcel() {
      if (this.form.name == "") {
        this.$message.error("请先输入文件名");
        return false;
      }
      /* const title = [["日期", "支出明细",null, "收入",null, "金额"],[null,"支出项","金额","收入项","金额",null]];
      const body = [];
      this.tableData.map((item, index) => {
        body[index] = [item.date, item.prop1, item.prop2, item.prop3,item.prop4,item.prop5];
      });
      untils.exportFromArray({
        title,
        body,
        name: this.form.name,
        suffix: this.form.suffix,
        merges: [
          { s: { r: 0, c: 0 }, e: { r: 1, c: 0 } },
          { s: { r: 0, c: 1 }, e: { r: 0, c: 2 } },
          { s: { r: 0, c: 3 }, e: { r: 0, c: 4 } },
          { s: { r: 0, c: 5 }, e: { r: 1, c: 5 } }
        ]
      }); */
      untils.exportFromTable({
        id: "table",
        name: this.form.name,
        suffix: this.form.suffix,
        merges: [
          { s: { r: 0, c: 0 }, e: { r: 1, c: 0 } },
          { s: { r: 0, c: 1 }, e: { r: 0, c: 2 } },
          { s: { r: 0, c: 3 }, e: { r: 0, c: 4 } },
          { s: { r: 0, c: 5 }, e: { r: 1, c: 5 } }
        ]
      });
      this.form = {
        name: "",
        suffix: "csv"
      };
      this.dialogFormVisible = false;
    },
    selectFile() {
      const el = document.getElementById("file");
      el.click();
    },
    async importFile() {
      const el = document.getElementById("file");
      const file = el.files[0];
      var data = await untils.importFromLocal(file);
      this.title = data.title;
      this.tableData = data.body;
      el.value = "";
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#export {
  float: right;
  margin: 12px 0;
}
#import {
  float: left;
  margin: 12px 0;
}
#file {
  display: none;
}
.el-select {
  width: 100%;
}
</style>
