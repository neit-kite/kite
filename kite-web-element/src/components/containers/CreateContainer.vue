<template>
  <div>
    <div
      @click="dialogVisible = true"
      class="new"
    >
      Click here to create a container
    </div>

    <el-dialog
      title="Create New Container"
      :visible.sync="dialogVisible"
      :before-close="handleClose"
    >
      <el-container>
        <el-form
          :model="form"
          :label-width="formLabelWidth"
        >
          <el-form-item label="Container name">
            <el-input v-model="form.name" />
          </el-form-item>
          <el-form-item label="Container image">
            <el-select
              v-model="form.image"
              placeholder="select container image"
            >
              <el-option
                label="PHP"
                value="sseemayer/mini-php"
              />
            </el-select>
          </el-form-item>
        </el-form>
      </el-container>
      <span
        slot="footer"
        class="dialog-footer"
      >
        <el-button @click="handleClose">
          Cancel
        </el-button>
        <el-button
          type="primary"
          @click="handleCreateContainer"
        >
          Create Container
        </el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
      form: {
        name: "",
        image: ""
      },
      formLabelWidth: "120px"
    };
  },
  methods: {
    handleClose(done) {
      this.$confirm("Are you sure to close this dialog?")
        .then(() => {
          this.dialogVisible = false;
          done();
        })
        .catch(() => {});
    },
    async handleCreateContainer() {
      const req = await this.$jraph`
      mutation{
        msg: createContainer( userid: "${Math.floor(
          Math.random() * 1000000000
        )}-${this.form.name}" )
      }
      `;

      console.log(req);
      this.dialogVisible = false;
    }
  }
};
</script>

<style scoped>
.new {
  border: 4px dashed rgba(0,0,0,0.2);
  color: rgba(0,0,0,0.3);
  border-radius: 6px;
  width: 400px;
  height: 15vh;
  text-align: center;
  line-height: 15vh;
  cursor: pointer;
  font-family: "Roboto";
  font-weight: 400;
  transition: .175s color, .175s border;
}
.new:hover{
  border-color: rgba(0,0,0,0.3);
  color: rgba(0,0,0,0.4);
}
</style>