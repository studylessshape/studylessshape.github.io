---
title: "使用 Actix Web 和 Vue 在开发中遇到的问题"
date: 2023-05-02
lastmod: 2023-7-20
draft: false
tags: ["Rust", "actix-web", "vue"]
categories: ["Rust"]
summary: "记录了在开发中遇到的问题"
---

> 精力有限，不再更新了。

## 前言
这篇博客会不定期记录我在使用 `actix web` 和 `vue` 时遇到的问题。两者放在一起是因为我在开发过程中，前端使用 `vue`，后端使用 `actix web`，一些地方可能两者结合起来才能说明的更加清楚。

## 遇到的问题
### 1. 类型问题
浅浅了解 `actix web` 和 类似与 `Rust` 这种强类型语言的话，很容易知道类型匹配很多时候会令人头痛。

在 `actix web` 中，有一种工具叫做提取器（[`Extractor`](https://actix.rs/docs/extractors)），提取器可以从请求的链接或者请求的数据中，提取中想要的数据，我常用的有 `Json`、`Path`、`Query`、[`actix_multipart::Multipart`](https://docs.rs/actix-multipart/latest/actix_multipart/struct.Multipart.html)。
1. `Json`：常用在 `POST` 请求中，能够从请求的数据（即 `fetch` 中的 `body`）提取出 `Json` 数据。
2. `Path`：常用于提取链接路径的参数。在 `actix web` 配置路径及对应的服务和路由时，可以通过 `{}` 来匹配对应路径的一段字符，如配置了路径 `/path/to/{extractor}`，请求的路径为 `/path/to/123`，那么通过 `Path`，就可以提取出 `{extractor}` 这一段的字符，也就是 `123`。
3. `Query`：常用于 `GET` 方法。`GET` 方法的请求数据或者说参数，经常放在路径的 `?` 号后，所以通过 `Query` 就可以提取出所有参数。
4. `actix_multipart::Multipart`：适用于用于表单传输的结构体。目前只尝试通过其读取并保存请求传输过来的文件。

在上面 4 种常用的提取器中，`Json` 和 `Query` 是非常严格的。比如，对于 `Json` 来说，得到了请求，请求的数据为 `{"user_id":"12"}`，那么在 `actix web` 这里，需要新建一个结构体才能够提取出数据。比如下面的代码，结构体中的字段和请求的数据中的字段应该相对应，这时放入请求参数中，就没有问题。
```rust
#[derive(Deserialize, Serialize)]
pub struct UserIdRequest {
    pub user_id: i32,
}

pub async fn user_id_request(user_id_re: web::Json<UserIdRequest>) -> impl Responder {
    // ....
}
```

但是上述请求会有比较蹊跷的地方，一旦尝试请求，会出现 `400` 错误码，这又是为什么呢？

仔细观察请求的 `json` 数据：
```json
{"user_id":"12"}
```

会发现 `user_id` 的数据，实际上是字符串类型，所以在请求时，由于解析无法将字符串类型解析为整型，所以出错，返回了 `400` 的错误码。这个错误出现次数非常多，多到每次排查错误排查很久，结果发现都是类型匹配错误。

#### 为何会我经常出现这种错误？
前端使用 `vue` 编写。如果有这么一个场景，我需要输入数值，然后返回给后台，那么 `vue` 代码可能是这样的：
```html
<script>
export default {
    data() {
        num: null,
    },
    method: {
        sendRequest() {
            fetch("localhost:8082/recive-number", {
                method: "POST",
                mode: "cors",
                headers: {"Content-type":"application/json"},
                body: JSON.stringify({number: num ? num : 0})
            })
        }
    }
}
</script>

<template>
    <div>
        <input v-model='num' type='text' placeholder='Please input number'/>
        <button @click='sendRequest'>submit</button>
    </div>
</template>
```

注意这里，`num` 在初始化时设定的值是 `null`，而且在下面的 `<input>` 框中，设定的 `type` 为 `text`，此时，如果直接将输入的数据发送给后台，那么就很有可能出现 `400` 的错误。因此，`body` 处的代码应该改为：
```html
<script>
export default {
    data() {
        num: null,
    },
    method: {
        sendRequest() {
            fetch("localhost:8082/recive-number", {
                method: "POST",
                mode: "cors",
                headers: {"Content-type":"application/json"},
                body: JSON.stringify({number: num ? parseInt(num) : 0})
            })
        }
    }
}
</script>
<!-- template -->
```

当然也不一定是 `parseInt`，也可以根据自己后台的数据类型决定使用哪种类型。

### 2. 数据获取问题
以前使用过 `JavaWeb`，前端可以直接获取到 Java 中的对象，前后端的数据同步基本上没有什么问题。而在我编写毕设项目时，发现 Http 中的 `GET` 请求，在页面不刷新的时候，第二次请求同一个地址，会读取缓存，无论怎样修改都会出现这种情况。由于当时对前端和后端的通信了解的比较少，所以在需要重复获取数据的时候采用了跳转页面或者修改请求地址这种比较简单的方法，甚至很多地方直接使用其他不会读取缓存的方法。

在之后的一段时间中了解到了 WebSocket，实际上就是为前端和后端建立一个 TCP 连接，这样的话想要获取需要的数据，数据可以从建立好的连接获取，就不需要考虑 `GET` 请求的缓存问题了。