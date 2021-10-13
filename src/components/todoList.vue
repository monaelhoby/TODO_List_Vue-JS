/* eslint-disable prettier/prettier */
<template>
  <div class="container">
    <h1 class="text-left">Vue Todo List</h1>
    <div class="submit-form">
      <input type="text" placeholder="Enter Course" ref="course" />
      <button @click="submitForm" type="submit">Submit</button>
    </div>
    <div class="tasks-table">
      <table>
        <tr>
          <th>#</th>
          <th>Name</th>
          <th>Status</th>
          <th>operation</th>
        </tr>
        <tr v-for="(course, index) in courses" :key="index">
          <td>{{ index + 1 }}</td>
          <td>
            <span :class="{ done: course.status === 'done' }">{{
              course.name
            }}</span>
          </td>
          <td>
            <span
              class="pointer"
              @click="changeStatus(index)"
              :class="{
                'text-danger': course.status === 'to-do',
                'text-warning': course.status === 'in-progress',
                'text-success': course.status === 'done',
              }"
              >{{ course.status }}
            </span>
          </td>
          <td>
            <button @click="editCourse(index)">Edit</button>
            <button @click="deleteCourse(index)">Delete</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "todoList",
  data() {
    return {
      courses: [],
      editTask: null,
      avaliableStatus: ["to-do", "in-progress", "done"],
    };
  },
  methods: {
    submitForm() {
      if (this.$refs.course.value.length === 0) return;
      if (this.editTask === null) {
        this.courses.push({ name: this.$refs.course.value, status: "to-do" });
      } else {
        this.courses[this.editTask].name = this.$refs.course.value;
        this.editTask = null;
      }
      this.$refs.course.value = "";
    },
    deleteCourse(i) {
      this.courses.splice(i, 1);
    },
    editCourse(i) {
      this.editTask = i;
      this.$refs.course.value = this.courses[i].name;
      this.editTask == null;
    },
    changeStatus(i) {
      let newStatus = this.avaliableStatus.indexOf(this.courses[i].status);
      if (++newStatus > 2) newStatus = 0;
      this.courses[i].status = this.avaliableStatus[newStatus];
    },
  },
};
</script>
<style scoped>
.container {
  max-width: 800px;
  margin: auto;
  text-align: center;
}
.submit-form {
  display: flex;
  margin-top: 60px;
}
.submit-form input {
  width: 70%;
  height: 35px;
}
.submit-form button {
  width: 30%;
  height: 35px;
  background: chocolate;
  color: #fff;
  border: 1px solid chocolate;
  transition: all 0.3s ease-in-out;
  padding: 15px 0 25px 0;
}
.submit-form button:hover {
  background: #fff;
  color: chocolate;
}
.pointer {
  cursor: pointer;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}
tr:nth-child(even) {
  background-color: #dddddd;
}
tr button:last-of-type {
  margin-left: 15px;
}
.done {
  text-decoration: line-through;
}
.text-danger{
    color:crimson
}
.text-warning{
    color:darkgoldenrod
}
.text-success{
    color:darkgreen
}
</style>
