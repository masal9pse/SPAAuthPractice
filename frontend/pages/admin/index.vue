<template>
  <v-layout column justify-center>
    <v-card v-if="adminUsers">
      <v-card-title>
        管理者一覧
        <v-spacer />
        <v-text-field v-model="searchText" append-icon="mdi-magnify" label="検索" sigle-line />
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="adminUsers"
        :items-per-page="5"
        :search="searchText"
        sort-by="id"
        :sort-desc="true"
        class="elevation-1"
      >
        <!-- 追加 -->
        <template v-slot:top>
          <v-dialog v-model="isShowDialog" max-width="500px">
            <v-card>
              <v-card-title>
                <span class="headline">管理者編集</span>
              </v-card-title>
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="6">
                      <v-text-field v-model="dialogAdminUser.email" label="メールアドレス" />
                    </v-col>
                    <v-col cols="6">
                      <v-text-field v-model="dialogAdminUser.name" label="名前" />
                    </v-col>
                    <v-col cols="12">
                      <v-text-field
                        type="password"
                        v-model="dialogAdminUser.password"
                        label="パスワード"
                      />
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn @click="closeDialog">閉じる</v-btn>
                <v-btn class="primary" @click="onClickUpdateBtn">更新する</v-btn>
                <v-spacer />
              </v-card-actions>
            </v-card>
          </v-dialog>
        </template>
        <template v-slot:item.edit-action="{ item }">
          <v-icon small @click="onClickEditIcon(item)">mdi-pencil</v-icon>
        </template>
      </v-data-table>
    </v-card>
  </v-layout>
</template>

<script>
export default {
  middleware: "not_logined_admin_user",
  layout: "admin",
  async fetch({ store }) {
    const adminUsers = await store.dispatch("adminUsers/fetchList");
    store.commit("adminUsers/setList", adminUsers);
  },
  data() {
    return {
      dialogAdminUser: {},
      isShowDialog: false,
      searchText: ""
    };
  },
  computed: {
    adminUsers() {
      return this.$store.getters["adminUsers/list"];
    },
    headers() {
      return [
        { text: "ID", value: "id" },
        { text: "メールアドレス", value: "email" },
        { text: "名前", value: "name" },
        { text: "", value: "edit-action" },
        { text: "", value: "delete-action" }
      ];
    }
  },
  methods: {
    //追加
    closeDialog() {
      this.dialogAdminUser = {};
      this.isShowDialog = false;
    },
    onClickEditIcon(adminUser) {
      this.dialogAdminUser = Object.assign({}, adminUser);
      this.isShowDialog = true;
    },
    //追加
    async onClickUpdateBtn() {
      await this.$store.dispatch("adminUsers/update", this.dialogAdminUser);
      this.closeDialog();
    }
  }
};
</script>
