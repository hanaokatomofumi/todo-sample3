<template>
  <div>
    <el-button type="success" round @click="getIssues()">issue取得</el-button>
    <el-row :gutter="12">
      <TodoItem v-for="( issue, index ) in issues" :key="issue.id" :issue="issue.title" :index="index" @closeIssue="closeIssue"/>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';
import TodoItem from '@/components/TodoItem';
const client = axios.create({
  baseURL: 'https://api.github.com/repos/diveintocode-corp/vue_seriese_api',
  headers: {
    'Accept': 'application/vnd.github.v3+json',
    'Content-Type':'application/json',
    'Authorization': `token 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`
  },
})
export default {
  name: 'IssueList',
  components: {
    TodoItem
  },
  data() {
    return {
      issues: [],
    }
  },
  methods: {
    getIssues() {
      client.get('/issues')
        .then((res) => {
          this.issues = res.data;
      })
    },
    closeIssue(index){
      const target = this.issues[index]
      client.patch(`/issues/${target.number}`,
          {
            state: 'closed'
          },
        )
        .then(() => {
          this.issues.splice(index, 1)
      })
    },
  },
  // created() {
  //   this.getIssues();
  // }
}
</script>