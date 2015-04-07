---
title:ssh-keygen  本地免密码验证配置
layout: post
tags:
- Linux
- ssh
---
<div> <p>[root@localhost .ssh]# ssh-keygen   –t  rsa</p><p>Generating public/private rsa key pair.</p><p>Enter file in which to save the key (/root/.ssh/id_rsa):</p><p>Enter passphrase (empty for no passphrase):                  &lt;---按enter</p>Enter same passphrase again:                              &lt;---按enter<br/><br/>[root@localhost  .ssh]# cd /root/.ssh/<br/> [root@localhost  .ssh]#more id_rsa.pub &gt;&gt; authorized_keys 这样就可以了<br/><br/> </div>