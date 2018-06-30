# .vue ファイルに追加された属性

```
<script>
export default {
  asyncData (params) {
    // コンポーネントにデータをセットする前に非同期な処理を行う
  },
  fetch () {
    // ページがレンダリングされる前にストアにデータをセットする処理を行う
  },
  layout: 'layout-name',
  transition: 'transition-name',
  scrollTop: true,
  validate () {
    // ルーティングパラメータのバリデーション
  },
  middleware: 'middleware-name'
}
</script>
```

## asyncData

asyncDataはVuexのストアを使わずに、データを取得しレンダリングの事前処理をする時に使用します。
asyncDataはページコンポーネントがロードされるたびに呼び出されます。
サーバサイドレンダリング時にも呼び出されます。
asyncData内で取得したデータはページコンポーネントのdataとマージすることができます。

asyncDataの実装方法

1. Promiseを返す
2. async/awaitを使用する
3. 第二引数としてコールバック関数を定義する

### Promiseを返す

```
<template>
  <section>
    <h1>sample</h1>
    {{ title }}
  </section>
</template>

<script>
export default {
  asyncData (params) {
    return new Promise((resolve, reject) => {
      // ここに非同期処理を書く
      resolve()
    }).then(() => {
      return {
        title: "sample_text"
      }
    })
  }
}
</script>
```

### async/awaitを使用する

```
<template>
  <section>
    <h1>sample</h1>
    {{ title }}
  </section>
</template>

<script>
async function fn() {
  return {
    title: "sample_text"
  }
}

export default {
  async asyncData (params) {
    const result = await fn();
    return result;
  }
}
</script>
```

### 第二引数としてコールバック関数を定義する

```
<template>
  <section>
    <h1>sample</h1>
    {{ title }}
  </section>
</template>

<script>
async function fn() {
  return {
    title: "sample_text"
  }
}

export default {
  asyncData({ params }, callback) {
    fn().then(res => {
      callback(null, { title: res.title })
    })
  }
}
</script>
```

## fetch

fetchはページがレンダリングされる前にデータをstoreに入れるために使用されます。
ここで定義された関数もページコンポーネントがロードされるたびに呼び出されます。
fetch関数は第一引数にコンテキストを受け取り、コンテキストを使用してデータをstoreに入れます。

```
<template>
  <section>
    <h1>sample</h1>
    {{ $store.state.title }}
  </section>
</template>

<script>
export default {
  fetch ({ store, param }) {
    return axios.get('http://example.com/api/title').then(() => {
      store.commit('setTitle', { title: res.title })
    })
  }
}
</script>
```

## head

headはページオブジェクトのレンダリング時のheadタグを設定するために使用します。
内部的にはvue-metaを使用して実装されています。

```
<script>
export default {
  data () {
    return {
      title: 'head'
    }
  },
  head () {
    return {
      title: this.title,
      meta: [
        { charset: 'utf-8' },
        { name: 'viewport', content: 'width=device-width, initial-scale=1' }
      ]
    }
  }
}
</script>
```

## layout

layout は2通りの使い方ができます。

1. 文字列でlayoutを指定
2. 関数でコンテキストオブジェクトを受け取りどう的に定義

```
<script>
export default {
  layout: 'sample-layout',
  // または
  lauout (context) {
    // コンテキストオブジェクトを受け取り動的にレイアウトを返す
    return 'sample-layout'
  }
}
</script>
```

## scrollToTop

ページを遷移した時トップまでスクロールするかどうかを設定します。
デフォルトはfalse

## validate

validate には動的なルーティングを行うコンポーネントのバリデーションメソッドを定義できます。
バリデーションが通れば該当ページコンポーネントが描画され、通らなければエラーページが表示されます。

```
<script>
export deafult {
  validate({ params, query, store }) {
    return true
  }
}
</script>
```

validateの引数は次の3つがオブジェクトで渡されます。

- params
  - 動的ルーティングのパラメータ
- query
  - URLクエリパラメータ
- store
  - Vuexのストアオブジェクト

渡されるパラメータによって柔軟にバリデーションを行えるようになります。

## middleware

middlewareにはページコンポーネントのレンダリング時に使用するミドルウェアのファイル名を文字列化文字列の配列で定義します。
middlewareディレクトリに配置したファイル名を指定します。
