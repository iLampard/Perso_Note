# Batch File Snippets


### 自动连局域网网盘

```bash
net use o: \\10.180.10.0\x /user:administrator password
net use R: \\10.180.10.0\y /PERSISTENT:NO
net use p: \\10.180.10.0\oa_shares /user:administrator password
net use \\10.180.90.6\f
```

