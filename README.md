# What is postMessage

A concise guide to **postMessage**, covering what it is, why the same-origin policy makes it necessary, how to send and receive messages, and how it compares to direct same-origin access.

## 📄 Contents

This guide covers:

- What postMessage is
- Why we need postMessage
- Key concepts:
  - **message**
  - **targetOrigin**
  - **message event**
  - **event.origin**
- How to send a message to an iframe or a popup
- How to receive a message and verify its origin
- Same-Origin access vs postMessage
- Key takeaways

## 🔗 What is postMessage?

postMessage is a method that enables cross-origin communication between Window objects — for example, between a page and a pop-up that it spawned, or between a page and an iframe embedded within it.

Normally, scripts on different pages are allowed to access each other if and only if the pages follow the same-origin policy, meaning the pages share the same:

- **Protocol**
- **Host**
- **Port**

## 💡 postMessage Example

**1. Sending a message to an iframe:**

```javascript
const frame = document.getElementById("myFrame");

frame.contentWindow.postMessage(
  { type: "greet", text: "Hello iframe!" },
  "https://child.example.com"
);
```

**2. Sending a message to a popup:**

```javascript
// Sending to a popup window works the same way
const popup = window.open("https://child.example.com");

popup.postMessage(
  { type: "greet", text: "Hello popup!" },
  "https://child.example.com"
);
```

**3. Receiving a message, and checking its origin:**

```javascript
window.addEventListener("message", (event) => {
  if (event.origin !== "https://parent.example.com") {
    return; // ignore untrusted senders
  }

  console.log(event.data);
  // { type: "greet", text: "Hello iframe!" }
});
```

**4. Replying back to the sender:**

```javascript
window.addEventListener("message", (event) => {
  if (event.origin !== "https://parent.example.com") return;

  event.source.postMessage(
    { type: "ack", text: "Got it!" },
    event.origin
  );
});
```

> `targetOrigin` must match the receiver's origin exactly, or the browser won't deliver the message. And `event.origin` tells you who really sent the message — never trust `event.data` alone.

## 📚 Key Benefits

### Cross-Origin

Lets a page talk to an iframe or popup on a completely different origin.

### Controlled

The receiver checks the sender's origin before trusting any message.

### Two-Way

Both windows can send and receive, enabling real two-way communication.

## 🧩 Key Concepts

| Concept | Description |
| --- | --- |
| **message** | The data you want to send to the other window. |
| **targetOrigin** | The origin the receiving window must have, for safety. |
| **message event** | Fired on the receiving window when a message arrives. |
| **event.origin** | The sender's origin; always check it before trusting data. |

## 🆚 Same-Origin vs postMessage

| Scenario | Same-Origin Pages | Cross-Origin Pages |
| --- | --- | --- |
| **Direct JS access** | Allowed (`window.frame.var`) | Blocked by the browser |
| **Sharing data** | Read each other's variables | Must use postMessage |
| **Security check** | Implicit, same origin | You check `event.origin` yourself |
| **Typical use case** | Same-site iframes | 3rd-party widgets, payment iframes, OAuth popups |

## ✅ Key Takeaways

1. **Cross-origin communication**: postMessage lets Window objects talk safely across different origins.
2. **Same-origin policy**: scripts can only access each other directly when protocol, host, and port all match.
3. **Always check the origin**: verify `event.origin` on the receiving end before trusting any message.
4. **Common use cases**: iframes, popups, OAuth flows, and embedded third-party widgets.

## 📥 PDF

The complete guide is available in the PDFs below:

- **[What is postMessage (English).pdf](./What_is_PostMessage.pdf)**
- **[What is postMessage (Persian).pdf](./What_is_PostMessage_FA.pdf)**
