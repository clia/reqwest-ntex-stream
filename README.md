# clia-reqwest-ntex-stream

A Stream to link between Reqwest and ntex two systems.

## PayloadStream Example

```rust
async fn handle(
    body: ntex::web::types::Payload,
) {
    let mut builder = client.get(url);
    // This stream is not for send between threads, otherwise it's unsafe.
    builder = builder.body(reqwest::Body::wrap_stream(reqwest_ntex_stream::PayloadStream {
        payload: body,
    }));
    builder.send().await;
}
```

## ResponseStream Example

```rust
let res = builder.send().await;
let stream = res.bytes_stream();
let mut resp = HttpResponse::build(res.status());
// This method will use chunked Transfer-Encoding, otherwise use ntex::http::body::SizedStream
return Ok(resp.streaming(reqwest_ntex_stream::ResponseStream{ stream: stream }));
```
