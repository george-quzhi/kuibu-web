<template>
  <view class="padding-20">
    <uni-forms ref="form" :modelValue="formData" :rules="rules">
      <uni-forms-item label="待办事项" name="name">
        <input
          class="input-outer"
          v-model="formData.name"
          type="text"
          placeholder="请输入待办事项"
          @input="binddata('name', $event.detail.value)"
        />
      </uni-forms-item>
      <uni-forms-item label="事项日期" name="timestamp">
        <uni-datetime-picker
          type="date"
          returnType="timestamp"
          :value="formData.timestamp"
          :start="now"
        />
      </uni-forms-item>
      <uni-forms-item label="紧急性" name="urgency">
        <switch
          class="float-right"
          @change="binddata('urgency', $event.detail.value)"
          :checked="formData.urgency"
        />
      </uni-forms-item>
      <uni-forms-item label="重要性" name="importantce">
        <switch
          class="float-right"
          @change="binddata('importantce', $event.detail.value)"
          :checked="formData.importantce"
        />
      </uni-forms-item>
      <uni-forms-item label="子任务" name="subTask">
        <button class="float-right" size="mini" type="primary" @click="add">
          添加
        </button>
      </uni-forms-item>
    </uni-forms>
    <view class="task-list">
      <view v-for="(task, i) in taskList" :key="i">
        <uni-row class="task-item">
          <uni-col :span="2">
            <span class="index-no"> {{ i + 1 }} </span>
          </uni-col>
          <uni-col :span="16">
            <input
              class="input-outer"
              v-model="task.name"
              type="text"
              placeholder="请输入子任务"
              @input="binddata('name', $event.detail.value)"
            />
          </uni-col>
          <uni-col :span="4" :offset="2">
            <button
              class="delete-btn"
              size="mini"
              type="warn"
              @click="deleteItem(i)"
            >
              删除
            </button>
          </uni-col>
        </uni-row>
      </view>
    </view>
    <view class="bottom-btn">
      <button type="primary" @click="submit">提交</button>
    </view>
  </view>
</template>
<script>
import {
  uniForms,
  uniFormsItem,
  uniDatetimePicker,
  uniIcons,
  uniRow,
  uniCol,
} from "@dcloudio/uni-ui";
import moment from "moment";
export default {
  components: {
    uniForms,
    uniFormsItem,
    uniDatetimePicker,
    uniIcons,
    uniRow,
    uniCol,
  },
  data() {
    return {
      param: null,
      now: null,
      taskList: [],
      // 表单数据
      formData: {
        name: "",
        urgency: false,
        importantce: false,
        timestamp: null,
      },
      rules: {
        // 对name字段进行必填验证
        name: {
          rules: [
            {
              required: true,
              errorMessage: "请输入待办事项",
            },
            {
              minLength: 1,
              maxLength: 128,
              errorMessage: "长度在 {minLength} 到 {maxLength} 个字符",
            },
          ],
        },
        timestamp: {
          rules: [
            {
              required: true,
              errorMessage: "请选择日期",
            },
          ],
        },
        urgency: {
          rules: [
            {
              required: true,
              errorMessage: "请选择紧急性",
            },
          ],
        },
        importantce: {
          rules: [
            {
              required: true,
              errorMessage: "请选择重要性",
            },
          ],
        },
      },
    };
  },
  onLoad(option) {
    this.param = JSON.parse(decodeURIComponent(option.param));
    this.now = new Date(this.param.date).getTime()
    this.formData.timestamp = new Date(this.param.date).getTime()
  },
  methods: {
    deleteItem(index) {
      this.taskList.splice(index, 1);
    },
    add() {
      this.taskList.push({ name: "" });
    },
    binddata(name, value) {
      this.$refs.form.setValue(name, value);
    },
    // 触发提交表单
    submit() {
      this.$refs.form
        .validate()
        .then((res) => {
          console.log("表单数据信息：", res);
          uni.request({
            method: "POST",
            url: "http://localhost:3000/events", //仅为示例，并非真实接口地址。
            data: res,
            // header: {
            //   "custom-header": "hello", //自定义请求头信息
            // },
            success: (res) => {
              console.log(res.data);
              if (res.statusCode !== 400) {
                uni.navigateBack();
              } else {
                console.log("参数错误");
              }
            },
            fail: (error) => {
              console.log(error);
            },
          });
        })
        .catch((err) => {
          console.log("表单错误信息：", err);
        });
    },
  },
};
</script>

<style>
.float-right {
  margin-top: 4px;
  float: right;
}
.padding-20 {
  padding: 20px;
}
.input-outer {
  height: 40px;
  border: 1px solid #dcdfe6;
  padding: 0 10px;
  border-radius: 4px;
}
.bottom-btn {
  position: fixed;
  bottom: 0;
  right: 0;
  left: 0;
  padding: 0 20px 20px 20px;
  background: white;
}
.index-no {
  font-size: 14px;
  color: #333;
  margin-top: 12px;
  display: inline-block;
}
.task-list {
  padding-bottom: 70px;
}
.task-item {
  padding-bottom: 12px;
}
.delete-btn {
  margin-top: 6px;
}
</style>
