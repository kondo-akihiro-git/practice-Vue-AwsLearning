<script>
import { fetchData } from '@/utils/service';
import WordDetail from '@/components/WordDetail.vue';
import CategoryList from '@/components/CategoryList.vue';
import RequestForm from '@/components/RequestForm.vue';

export default {
    // コンポーネントを登録
    components: {
        WordDetail,
        CategoryList,
        RequestForm
    },
    // データ格納オブジェクト
    data() {
        return {
            listViewData: {},
            selectedWordData: null,
            isRequestWordVisible: false
        };
    },

    // マウント時にデータ取得
    mounted() {
        this.loadData();
    },
    methods: {
        async loadData() {
            this.listViewData = await fetchData();
        },

        // ワード詳細を表示するメソッド
        showWordDetail(selectedWordData) {
            this.selectedWordData = selectedWordData;
            this.isRequestWordVisible = false;
        },

        // 詳細を閉じるメソッド
        closeWordDetail() {
            this.selectedWordData = null;
        },

        // 新規ワード追加画面を表示するメソッド
        showRequestForm() {
            this.isRequestWordVisible = true;
            this.selectedWordData = null;
        },

        // 新規ワード追加画面を閉じるメソッド
        closeRequestForm() {
            this.isRequestWordVisible = false;
        }
    }
};
</script>

<template>
    <!-- ///////////////////////////////////////////////////////////////////////////////////////////////
    //
    // カテゴリーとワードの一覧表示とワード詳細の表示
    //
    //////////////////////////////////////////////////////////////////////////////////////////////// -->
    <div class="container">
        <div class="is-size-2">クラウドプラクティショナー</div>
        <br>

        <div class="columns">
            <!-- カテゴリとワード一覧を表示 (デフォルト表示、ワード選択時はサイドバー化) -->
            <div :class="{'column is-one-third': selectedWordData, 'column': !selectedWordData}">
                <CategoryList :listViewData="listViewData" @showWordDetailEvent="showWordDetail" v-if="!isRequestWordVisible" />
            </div>

            <!-- ワード詳細を表示 -->
            <div class="column is-two-thirds" v-if="selectedWordData">
                <WordDetail :selectedWordData="selectedWordData" @closeWordDetailEvent="closeWordDetail" />
            </div>
        </div>

        <br>

        <!-- 新規ワードの追加ボタン (RequestWord 表示時は非表示) -->
        <button v-if="!isRequestWordVisible" class="button is-primary" @click="showRequestForm">
            新規ワードの追加
        </button>
        <br>

        <!-- 新規ワードの登録フォームを表示 -->
        <div v-if="isRequestWordVisible">
            <RequestForm @closeRequestFormEvent="closeRequestForm" />
        </div>

        <br>
        <router-link to="/">ホーム画面に戻る</router-link>
    </div>
</template>
