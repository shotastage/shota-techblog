---
title: "Swiftマクロを使ってVaporのControllerをSpringっぽい感じで書けるようにしてみた"
emoji: "📘"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["swift", "vapor"]
published: false
---

現在、[MagicApp](https://mgq.app)ではSwift Vaporを使用したバックエンドの開発にトライしています。基本はKotlin + Springをベースとしつつも技術的なチャレンジングもあると非常に先駆的な価値を生み出せると考えたからです。

## Swift Vaporを選んだ理由


## 問題: Swift Vaporの辛いところ

Swift Vaporはエコシステムこそ未熟なものの、フレームワークとしてはかなり完成されており一通りのことはできるようになっています。
一方で、Spring Frameworkなどと比較すると冗長なボイラープレートを量産しなければならない点も見受けられました。
代表的なのは下記のコードのようなRoutingの定義です。Controller毎に`func boot(routes: RoutesBuilder) throws`を作成し中にRoutingを定義します。

```swift
struct PingPongController: RouteCollection, Sendable {
    func boot(routes: RoutesBuilder) throws {
        // GET /ping → Returns "pong" endpoint
        routes.get("ping", use: ping)
        // POST /payload → Returns the JSON Payload as is
        routes.post("payload", use: echo)
    }

    // Handler that returns "pong" when accessing "ping" with a GET request
    @Sendable func ping(req: Request) async throws -> String {
        "pong"
    }

    // Handler that echoes back the Payload received in the POST request
    @Sendable func echo(req: Request) async throws -> Payload {
        let payload = try req.content.decode(Payload.self)
        return payload
    }
}
```

ただ、これはJavaのSpringであれば以下のように書けるわけです。どちらのコードも`import`や**DTO**類は省略してますがアノテーションと呼ばれるものをメソッドやクラスに付与するとフレームワークが適切な形に変換してくれるわけです。

```java
@RestController
public class PingPongController {
    // GET /ping -> "pong"
    @GetMapping("/ping")
    public String ping() {
        return "pong";
    }

    // POST /payload -> returns the JSON payload as-is
    @PostMapping("/payload")
    public Payload echo(@RequestBody Payload payload) {
        return payload;
    }
}
```



```swift
@Controller("/")
struct PingPongController: Sendable {
    // Handler that returns "pong" when accessing "ping" with a GET request
    @GET("ping")
    @Sendable func ping(req: Request) async throws -> String {
        "pong"
    }

    // Handler that echoes back the Payload received in the POST request
    @POST("payload")
    @Sendable func echo(req: Request) async throws -> Payload {
        try req.content.decode(Payload.self)
    }
}
```
