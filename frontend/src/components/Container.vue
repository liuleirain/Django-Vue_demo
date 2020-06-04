<template>
  <div class="row">
    <div class="col-md-4">
      <div class="form-group">
        <input type="hidden" name="url" v-model="url" />
      </div>
      <div class="form-group">
        <input
          type="text"
          name="title"
          class="form-control"
          placeholder="标题"
          v-model="title"
        />
      </div>
      <div class="form-group">
        <textarea
          name="content"
          placeholder="内容"
          class="form-control"
          v-model="content"
        ></textarea>
      </div>
      <div class="form-group">
        <button class="btn btn-block btn-success" @click="saveBlog">
          保存
        </button>
      </div>
    </div>
    <div class="col-md-8">
      <table class="table table-bordered table-hover">
        <thead>
          <th class="text-center">标题</th>
          <th class="text-center">内容</th>
          <th class="text-center">编辑</th>
          <th class="text-center">删除</th>
        </thead>
        <tbody>
          <tr v-for="blog in blogs" :key="blog.url" class="text-center">
            <td>{{ blog.title }}</td>
            <td>{{ blog.content }}</td>
            <td>
              <button class="btn btn-success" @click="editBlog(blog)">
                <i class="fa fa-edit"></i>
              </button>
            </td>
            <td>
              <button class="btn btn-success" @click="deleteBlog(blog)">
                <i class="fa fa-trash"></i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Container",
  props: {},
  data() {
    return {
      base_url: "http://localhost:8000/api/blog/",
      blogs: null,
      url: "",
      title: "",
      content: "",
    };
  },
  methods: {
    getAll() {
      axios.get(this.base_url).then((res) => (this.blogs = res.data));
      this.url = "";
      this.title = "";
      this.content = "";
    },
    saveBlog() {
      if (this.url == "") {
        axios
          .post(this.base_url, { title: this.title, content: this.content })
          .then(() => this.getAll());
      } else {
        axios
          .put(this.url, { title: this.title, content: this.content })
          .then(() => this.getAll());
      }
    },
    editBlog(blog) {
      this.url = blog.url;
      this.title = blog.title;
      this.content = blog.content;
    },
    deleteBlog(blog) {
      axios.delete(blog.url).then(() => this.getAll());
    },
  },
  mounted() {
    this.getAll();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
