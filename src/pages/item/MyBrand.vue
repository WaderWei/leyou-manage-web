<template>
  <div>
    <v-card-title>
      <v-btn color="primary">新增品牌</v-btn>
      <!--空间隔离组件-->
      <v-spacer/>
      <!--搜索框，与search属性关联-->
      <v-text-field label="输入关键字搜索"  v-model="key" append-icon="search" hide-details/>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image"/></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="justify-center layout">
          <v-btn flat icon color="warning">
            <v-icon>edit</v-icon>
          </v-btn>
          <v-btn flat icon color="error">
            <v-icon>delete</v-icon>
          </v-btn>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  export default {
    name: "MyBrand",
    data() {
      return {
        totalBrands: 0,
        brands: [],
        loading: true,
        pagination: {},//分页信息
        key: "",
        headers: [
          {text: 'id', align: 'center', value: 'id'},
          {text: 'name', align: 'center', value: 'name'},
          {text: 'image', align: 'center', value: 'image'},
          {text: 'letter', align: 'center', value: 'letter'},
          {text: '操作', align: 'center', value: 'id', sortable: false}
        ],
      }
    },
    created() {
      this.getDataFromServer();
    },
    watch: {
      key() {
        this.pagination.page = 1;
        this.getDataFromServer();
      },
      pagination: {
        deep: true,
        handler() {
          this.getDataFromServer();
        }
      }
    },
    methods: {
      getDataFromServer() {
        this.$http.get("/item/brand/page", {
          params: {
            page: this.pagination.page,
            rows: this.pagination.rowsPerPage,
            sortBy: this.pagination.sortBy,
            desc: this.pagination.descending,
            key: this.key
          }
        }).then(rep => {
          this.loading=true;
            const data = rep.data;
            this.brands = data.items;
            this.totalBrands = data.total;
            this.loading = false;
        });
        // 伪造假数据
        /*const brands = [
          {
            "id": 2032,
            "name": "OPPO",
            "image": "http://img10.360buyimg.com/popshop/jfs/t2119/133/2264148064/4303/b8ab3755/56b2f385N8e4eb051.jpg",
            "letter": "O",
            "categories": null
          },
          {
            "id": 2033,
            "name": "飞利浦（PHILIPS）",
            "image": "http://img12.360buyimg.com/popshop/jfs/t18361/122/1318410299/1870/36fe70c9/5ac43a4dNa44a0ce0.jpg",
            "letter": "F",
            "categories": null
          },
          {
            "id": 2034,
            "name": "华为（HUAWEI）",
            "image": "http://img10.360buyimg.com/popshop/jfs/t5662/36/8888655583/7806/1c629c01/598033b4Nd6055897.jpg",
            "letter": "H",
            "categories": null
          },
          {
            "id": 2036,
            "name": "酷派（Coolpad）",
            "image": "http://img10.360buyimg.com/popshop/jfs/t2521/347/883897149/3732/91c917ec/5670cf96Ncffa2ae6.jpg",
            "letter": "K",
            "categories": null
          },
          {
            "id": 2037,
            "name": "魅族（MEIZU）",
            "image": "http://img13.360buyimg.com/popshop/jfs/t3511/131/31887105/4943/48f83fa9/57fdf4b8N6e95624d.jpg",
            "letter": "M",
            "categories": null
          }
        ];*/

        /*setTimeout(() => {
          this.brands = brands;
          this.totalBrands = brands.length;
          this.loading = false;
        }, 500);*/
      }
    }
  }
</script>

<style scoped>

</style>
