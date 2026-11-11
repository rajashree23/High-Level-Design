# 10. Transport Layer
https://www.hellointerview.com/learn/system-design/core-concepts/networking-essentials#transport-layer-protocols

```text
Application
    ↓
Transport Layer
    ↓
 ┌─────────────┐
 │ TCP         │ → Reliable + Ordered
 │             │
 │ UDP         │ → Connectionless + Lightweight
 │             │
 │ QUIC        │ → Reliable transport over UDP
 └─────────────┘
    ↓
Network Layer
    ↓
IP + Routing
```
