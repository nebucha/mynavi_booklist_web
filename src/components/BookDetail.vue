<template>
    <div style="background-color:transparent !important;">
        <h1>詳細編集</h1>

        <div class="panel">
            <form>
                <div class="form-group">
                    <label for="titleInput">タイトル</label>
                    <input v-model="book.title" class="form-control" id="titleInput"/>
                </div>
                <div class="form-group">
                    <label for="categoryInput">カテゴリー</label>
                    <input v-model="book.category" class="form-control" id="categoryInput"/>
                </div>
                <div class="form-group">
                    <label for="authorInput">著者</label>
                    <input v-model="book.author" class="form-control" id="authorInput"/>
                </div>
                <div class="form-group">
                    <label for="publisherInput">出版社</label>
                    <input v-model="book.publisher" class="form-control" id="publisherInput"/>
                </div>
                <div class="form-group">
                    <label for="publicationDateInput">発行日</label>
                    <input v-model="book.publicationDate" type="date" class="form-control" id="publicationDateInput"/>
                </div>
                <button type="button" class="btn btn-primary" v-on:click="saveItem()">登録</button>
                <button type="button" class="btn btn-secondary" v-on:click="cancelEdit()">キャンセル</button>
            </form>
        </div>
    </div>
</template>

<script>
    import booklistService from '../booklistService.js'

    export default {
        name: "BookDetail",
        data: function () {         // データ定義
            return {
                book:  {"id": null, "title": "", "category": "", "author": "", "publisher": "", "publicationDate": ""}  // 書籍の詳細情報
            }
        },
        created: async function () {    // 初期設定
            const id = this.$route.params.id
            if(id != null) {
                let res = await booklistService.getItem(id)
                this.book = res
            }
        },
        methods: {                      // メソッド定義
            cancelEdit() {                  // [キャンセル]ボタンの処理
                // Booklistに移動
                this.$router.push({ name: 'Booklist' })
            },
            async saveItem() {              // [登録]ボタンの処理
                // 既存アイテムなら更新、新規アイテムなら追加のリクエストを送る
                if (this.book.id != null) {
                    console.log(this.book)
                    let res = await booklistService.saveItem(this.book)
                    console.log(res)
                } else {
                    console.log(this.book)
                    let res = await booklistService.addItem(this.book)
                    console.log(res)
                }
                // Booklistに移動
                this.$router.push({ name: 'Booklist' })
            }
        }
    }
</script>

<style scoped>
    input[type="date"] {
        position: relative;
        width: 200px;
    }
</style>

