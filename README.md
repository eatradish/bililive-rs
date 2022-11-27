# bililive-rs
Bilibili live danmu websocket library

## Usage

```rust
use anyhow::Result;
use bililive::{ws_socket_object, WsStreamMessageType};
use tokio::sync::mpsc::{self, UnboundedReceiver};

#[tokio::main]
async fn main() {
   let (tx, rx) = mpsc::unbounded_channel();

   // bilibili live room id (true id): 22746343

   let ws = ws_socket_object(tx, 22746343);

   if let Err(e) = tokio::select! {v = ws => v, v = recv(rx) => v} {
       eprintln!("{}", e);
   }
}

async fn recv(mut rx: UnboundedReceiver<WsStreamMessageType>) -> Result<()> {
   while let Some(msg) = rx.recv().await {
       println!("{:?}", msg);
   }

   Ok(())
}
```
Or run `cargo run --example danmu`
