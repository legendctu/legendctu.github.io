---
layout: post
title: How to Test a Website Hosted at an Internal Server
categories: [memo, shell]
---

## Steps (Now we're using putty to make it happen.)

### 1. Build up a ssh tunnel.

First of all, input the basic options at the Session page.

![ssh_tunnel]({{ site.baseurl }}/images/socketforward1.jpg)

As the image shown above, input a source port that is not occupied, and pick the "Dynamic" option. Then click the "Add" button.

Open the session, and login to your server. The tunnel is good to go.


### 2. Setup your browser.

![setup_browser]({{ site.baseurl }}/images/socketforward2.jpg)

You can follow the steps shown in the image. Notice that socket port is the same to the one you just enter at Step 1.


### 3. Edit your local hosts file.

Finally, you can edit your hosts file to resolve domains to the internal ip addresses.

For example:

```bash
10.13.0.23 legendctu.github.io
```

Flush the dns cache if you like. All is done now.

