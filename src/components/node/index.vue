<template>
    <div class="page-wrap">
        <div class="wrap lg-show">
          <Header></Header>
        </div>
        <div class="sm-show page-sm-header-wrap">
          <smHeader></smHeader>
        </div>
        <div class="sm-show sm-search-input-wrap">
          <searchInput></searchInput>
        </div>
        <div class="main-wrap">
          <div class="wrap">
            <NodeDescribe
              :title="title"
              :content="content"
              :link="link"
            ></NodeDescribe>
            <el-table
              class="list-wrap"
              :data="nodeList"
              :empty-text="$t('message.noData')"
              style="width: 100%">
              <el-table-column
                prop="ID"
                :label="$t('listHeader.nodeId')"
                min-width="120">
                <template slot-scope="scope" class="">
                  <router-link :to="{path: '/node/detail', query: { id: scope.row.ID }}">
                    <span class="table-link-color height">{{scope.row.ID}}</span>
                  </router-link>
                </template>
              </el-table-column>
              <el-table-column
                prop="Host"
                :label="$t('listHeader.host')">
              </el-table-column>
              <el-table-column
                prop="Port"
                :label="$t('listHeader.port')">
              </el-table-column>
              <el-table-column
                prop="Client"
                :label="$t('listHeader.client')">
              </el-table-column>
              <el-table-column
                prop="Caps"
                :label="$t('listHeader.caps')">
              </el-table-column>
              <el-table-column
                prop="LastSeen"
                :label="$t('listHeader.lastSeen')">
              </el-table-column>
            </el-table>
            <el-pagination
              class="el-pagination-wrap fr"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="page"
              :page-size="pageSize"
              layout="prev, pager, next"
              :total="total">
            </el-pagination>
          </div>
        </div>
        <Footer></Footer>
    </div>
</template>
<script>
import { mapActions } from 'vuex'
import Header from '../header'
import smHeader from '../sm-header'
import searchInput from '../search-input'
import NodeDescribe from '../describe'
import Footer from '../footer'

export default {
  data () {
    return {
      isDetail: false,
      title: this.$t('navs.node'),
      content: 'All nodes List.',
      link: this.$t('navs.node'),

      pageSize: 25
    }
  },
  components: {
    Header,
    smHeader,
    searchInput,
    NodeDescribe,
    Footer
  },
  mounted () {
    this.getList(1)
  },
  computed: {
    nodeList: {
      get () {
        return this.$store.state.node.nodeList
      }
    },
    page: {
      get () {
        return this.$store.state.node.page
      }
    },
    total: {
      get () {
        return this.$store.state.node.total
      }
    }
  },
  methods: {
    ...mapActions(['getNodeList']),

    handleSizeChange (val) {
      this.getList(val)
    },
    handleCurrentChange (val) {
      this.getList(val)
    },
    getList (page) {
      this.getNodeList(page)
    }
  }
}
</script>
<style lang="less">
@import "../../assets/css/page.less";
@import "../../assets/css/list.less";
</style>
