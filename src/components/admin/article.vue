<template>
  <el-container>
    <el-main>
      <h1>文章管理</h1>
      <!--搜索功能-->
      <el-row>
        <el-col :span="4" :xs="8">
          <el-input placeholder="根据文章标题或分类搜索" v-model="search" clearable></el-input>
        </el-col>
        <el-button type="primary" icon="el-icon-search" @click="SearchArtcle(search)"></el-button>
        <el-button type="danger" icon="el-icon-delete" @click="DeleteArtcle"></el-button>
      </el-row>
      <!--文章展示-->
      <el-row>
        <el-col>
          <el-table :data="tableData" border style="width: 100%" highlight-current-row>
            <!--多选框-->
            <el-table-column type="selection" width="40"></el-table-column>
            <!--ID-->
            <el-table-column fixed prop="id" label="id" width="80"></el-table-column>
            <!--文章标题-->
            <el-table-column prop="title" label="标题" width="170" show-overflow-tooltip></el-table-column>
            <!--文章所属分类-->
            <el-table-column prop="category" label="分类" width="100" show-overflow-tooltip></el-table-column>
            <!--文章封面-->
            <el-table-column label="文章封面" show-overflow-tooltip width="130">
              <template slot-scope="scope">
                <div class="demo-image__preview">
                  <el-image
                    :src="scope.row.article_vover_url"
                    :preview-src-list="scope.row.article_vover_srcList"
                    style="width: 100px; height: 40px"
                  ></el-image>
                </div>
              </template>
            </el-table-column>
            <!--文章添加时间-->
            <el-table-column prop="add_time" label="发布时间" width="160"></el-table-column>
            <!--文章更新时间-->
            <!-- <el-table-column prop="update_time" label="修改时间" width="160"></el-table-column> -->
            <!--文章浏览量-->
            <el-table-column prop="views_number" label="浏览量" width="80"></el-table-column>
            <!--文章评论数-->
            <el-table-column prop="comments_number" label="评论数" width="80"></el-table-column>
            <!--文章推荐分数-->
            <el-table-column label="推荐分数" show-overflow-tooltip width="170">
              <template slot-scope="scope">
                <div class="block">
                  <el-rate v-model="scope.row.score" disabled show-score text-color="#ff9900"></el-rate>
                </div>
              </template>
            </el-table-column>
            <!--文章发布状态-->
            <el-table-column label="发布状态" show-overflow-tooltip width="80">
              <template slot-scope="scope">
                <el-switch v-model="scope.row.release_status" disabled active-color="#13ce66"></el-switch>
              </template>
            </el-table-column>

            <!--操作-->
            <el-table-column label="操作" show-overflow-tooltip>
              <template slot-scope="scope">
                <el-button @click="handleDelete(scope.row)" type="text" size="small">删除</el-button>
                <el-button @click="handleEdit(scope.row)" type="text" size="small">编辑</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-col>
       
      </el-row>
       <!--分页-->
        <el-row>
          <el-col>
            <div class="block">
              <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="currentPage4"
                :page-sizes="[10, 20, 30, 40]"
                :page-size="100"
                layout="total, sizes, prev, pager, next, jumper"
                :total="100"
              ></el-pagination>
            </div>
          </el-col>
        </el-row>
    </el-main>
  </el-container>
</template>

<script>
var tableData = [
  {
    id: "1000", // 文章id
    title: "上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄", // 文章标题
    category: "Vue学习", // 所属分类
    add_time: "2020-02-16 09:27:33", // 发布时间
    // update_time: "2020-05-16 16:27:33", // 修改时间
    views_number: "9999", //浏览量
    comments_number: "934", // 评论数
    release_status: true, // 发布状态
    article_vover_url:
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg", // 文章封面地址
    article_vover_srcList: [
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg" //  文章预览图地址同封面地址
    ],
    score: 3.1 // 文章推荐分数 总计5分  后端根据浏览量和评论数计算推荐分数  值必须为int类型
  },
  {
    id: "20022", // 文章id
    title: "上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄", // 文章标题
    category: "钢铁是怎样炼成的额", // 所属分类
    add_time: "2020-02-16 09:27:33", // 发布时间
    // update_time: "2020-05-16 16:27:33", // 修改时间
    views_number: "9999", //浏览量
    comments_number: "3453", // 评论数
    release_status: false, // 发布状态
    article_vover_url:
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg",
    article_vover_srcList: [
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg"
    ],
    score: 5
  },
  {
    id: "20022", // 文章id
    title: "上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄", // 文章标题
    category: "钢铁是怎样炼成的额", // 所属分类
    add_time: "2020-02-16 09:27:33", // 发布时间
    // update_time: "2020-05-16 16:27:33", // 修改时间
    views_number: "9999", //浏览量
    comments_number: "3453", // 评论数
    release_status: true, // 发布状态
    article_vover_url:
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg",
    article_vover_srcList: [
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg"
    ],
    score: 2
  },
  {
    id: "20022", // 文章id
    title: "上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄", // 文章标题
    category: "钢铁是怎样炼成的额", // 所属分类
    add_time: "2020-02-16 09:27:33", // 发布时间
    // update_time: "2020-05-16 16:27:33", // 修改时间
    views_number: "9999", //浏览量
    comments_number: "3453", // 评论数
    release_status: false, // 发布状态
    article_vover_url:
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg",
    article_vover_srcList: [
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg"
    ],
    score: 4.5
  },
  {
    id: "20022", // 文章id
    title: "上海市普陀区金沙江路 1518 弄上海市普陀区金沙江路 1518 弄", // 文章标题
    category: "钢铁是怎样炼成的额", // 所属分类
    add_time: "2020-02-16 09:27:33", // 发布时间
    // update_time: "2020-05-16 16:27:33", // 修改时间
    views_number: "9999", //浏览量
    comments_number: "3453", // 评论数
    release_status: true, // 发布状态
    article_vover_url:
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg",
    article_vover_srcList: [
      "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg"
    ],
    score: 4.7
  }
];
export default {
  name: "articleNav",
  data() {
    return {
      tableData: tableData,
      search: "",
      currentPage1: 5,
      currentPage2: 5,
      currentPage3: 5,
      currentPage4: 4
    };
  },
  methods: {
    // 编辑文章按钮
    handleEdit(index, row) {
      console.log(index, row);
    },
    // 删除文章按钮
    handleDelete(index, row) {
      this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          // 点击确认删除后向后端发起请求删除该数据
          console.log(index, row);
          this.$message({
            type: "success",
            message: "删除成功!"
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    // 模糊搜索文章
    SearchArtcle(value) {
      console.log(value);
    },
    // 批量删除
    DeleteArtcle(){

    },
    // 分页
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    }
  }
};
</script>

<style scoped>
.el-row {
  margin-bottom: 20px;
}
.el-col {
  border-radius: 4px;
}
</style>
