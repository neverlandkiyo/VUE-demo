<template>
  <div class="list-comp">
    <h1>同学列表</h1>
    <div class="tool-bar">
      <button class="add-btn" type="primary" @click="showFlag = true">新增</button>
    </div>
    <ul>
      <li>
        <span class="No">序号</span>
        <span class="study-code">学号</span>
        <span class="name">姓名</span>
        <span class="year">年龄</span>
        <div class="operation">操作</div>
      </li>

      <li v-for="(item, index) in list" :key="item.id">
        <span class="No">{{ index + 1 }}.</span>
        <span class="study-code">
          <template v-if="editIndex === index">
            <input type="text" v-model="editStudent.id">
          </template>
          <template v-else>
            {{ item.id }}
          </template>
        </span>
        <span class="name">
          <template v-if="editIndex === index">
            <input type="text" v-model="editStudent.userName">
          </template>
          <template v-else>
            {{ item.userName }}
          </template>
        </span>
        <span class="year">
          <template v-if="editIndex === index">
            <input type="text" v-model="editStudent.age">
          </template>
          <template v-else>
            {{ item.age }}
          </template>
        </span>
        <div class="operation">
          <template v-if="editIndex === index">
            <button @click="updateStudent">保存</button>
            <button @click="cancelEdit">取消</button>
          </template>
          <template v-else>
            <button @click="deleteUser(index)">删除</button>
            <button @click="editUser(index)">编辑</button>
            <button @click="getYourName(item.id)">问名字</button>
          </template>
        </div>
      </li>
    </ul>

    <div class="pop-blank" v-if="showFlag">
        <h3>新增同学</h3>
        <div class="blank-item">
          <span>学号</span>
          <input type="text" v-model="newStudent.id">
        </div>
        <div class="blank-item">
          <span>姓名</span>
          <input type="text" v-model="newStudent.userName">
        </div>
        <div class="blank-item">
          <span>年龄</span>
          <input type="text" v-model="newStudent.age">
        </div>
        <div class="footer">
          <button type="primary" @click="showFlag = false">取消</button>
          <button type="primary" @click="addStudent">确定</button>
        </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';

const showFlag = ref(false);
const list = reactive([
  {
    id: 220812002,
    userName: '刘淑文',
    age: 18
  },
  {
    id: 220812008,
    userName: '蔡欣怡',
    age: 18
  },
  {
    id: 220812030,
    userName: '龙依',
    age: 18
  }
]);

const newStudent = reactive({
  id: '',
  userName: '',
  age: ''
});

const editStudent = reactive({
  id: '',
  userName: '',
  age: '',
  index: null
});

const editIndex = ref(null);

const deleteUser = index => {
  list.splice(index, 1);
};

const editUser = index => {
  const item = list[index];
  editStudent.id = item.id;
  editStudent.userName = item.userName;
  editStudent.age = item.age;
  editIndex.value = index;
};

const cancelEdit = () => {
  editIndex.value = null;
};

const getYourName = id => {
  const student = list.find(item => item.id === id);
  alert(student.userName);
};

const addStudent = () => {
  if (newStudent.id && newStudent.userName && newStudent.age) {
    list.push({ ...newStudent });
    newStudent.id = '';
    newStudent.userName = '';
    newStudent.age = '';
    showFlag.value = false;
  } else {
    alert('请填写完整信息');
  }
};

const updateStudent = () => {
  if (editStudent.id && editStudent.userName && editStudent.age) {
    if (editIndex.value !== null) {
      const updatedStudent = { ...editStudent };
      list.splice(editIndex.value, 1);
      list.unshift(updatedStudent);
      editIndex.value = null;
    }
  } else {
    alert('请填写完整信息');
  }
};

</script>

<style scoped>
.list-comp {
  text-align: left;
}
h1{
  text-align: center;
}
.tool-bar {
  display: flex;
  justify-content: flex-end;
  .add-btn {
    background-color: rgb(77, 6, 144);
    width: 90px;
    margin-right: 24px;
  }
}
ul {
  padding-left: 0;
  padding: 0 24px;
}
.pop-blank {
  position: absolute;
  background-color: #000000;
  border-radius: 8px;
  left: 50%;
  top: 50%;
  width: 50%;
  height: 50%;
  transform: translate(-50%, -50%);
  padding: 24px;
  border: 1px solid rgba(0, 0, 0, 0.4);
  h3 {
    text-align: center;
    font-size: 32px;
    margin-top: 0px;
  }
  .blank-item {
    height: 36px;
    font-size: 24px;
    span {
      margin-right: 12px;
    }
  }
  .footer {
    position: absolute;
    bottom:24px;
    display: flex;
    justify-content: flex-end;
    width: calc(100% - 48px);
    button {
      background-color: blueviolet;
      color:#000000;
    }
    & > button:nth-child(1) {
      margin-right: 12px;
      background-color: #000000;
      border: 1px solid blueviolet;
      color: blueviolet
    }
  }
}
 li {
  text-align: center;
  list-style: none;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  /* margin-bottom: 24px; */
  height: 64px;
  border-bottom:  1px solid rgba(255, 255, 255, 0.253);
  &:nth-child(1) {
    background-color: rgba(70, 15, 122, 0.355);
  }
  .No, .study-code, .name, .year {
  width: 18%;
  }

  .operation {
    flex-grow: 1;
    display: flex;
    /* gap: 45px; */
    justify-content: space-around;
    & > button {
      background-color: rgba(79, 21, 116, 0.696);
      width: 90px;
    }

  }
}
</style>
