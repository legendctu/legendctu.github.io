---
layout: post
title: How to Display Host IP in Fiddler
categories: [memo]
---

## Steps

### 1. Edit CustomRules.js

Fiddler > Ctrl+R > locate function Main()

### 2. Add one line of code

```javascript
FiddlerObject.UI.lvSessions.AddBoundColumn("ServerIP", 120, "X-HostIP");
```

Save it, and close Fiddler. Launch Fiddler again, and we're done here.

