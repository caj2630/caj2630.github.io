---
uuid: 6
title: 关于签名
---
### 新增一个utf8.js

``` bash
    function utf8(wide) {
      var c, s
      var enc = ''
      var i = 0
      while(i<wide.length) {
        c = wide.charCodeAt(i++)
        // handle UTF-16 surrogates
        if (c>=0xDC00 && c<0xE000) continue
        if (c>=0xD800 && c<0xDC00) {
        if (i>=wide.length) continue
          s= wide.charCodeAt(i++)
        if (s<0xDC00 || c>=0xDE00) continue
          c= ((c-0xD800)<<10)+(s-0xDC00)+0x10000
        }
        // output value
        if (c<0x80) enc += String.fromCharCode(c)
        else if (c<0x800) enc += String.fromCharCode(0xC0+(c>>6),0x80+(c&0x3F))
        else if (c<0x10000) enc += String.fromCharCode(0xE0+(c>>12),0x80+(c>>6&0x3F),0x80+(c&0x3F))
        else enc += String.fromCharCode(0xF0+(c>>18),0x80+(c>>12&0x3F),0x80+(c>>6&0x3F),0x80+(c&0x3F))
      }
      return enc
    }
    var hexchars = '0123456789ABCDEF'
    function toHex(n) {
      return hexchars.charAt(n>>4)+hexchars.charAt(n & 0xF)
    }
    var okURIchars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789_.-*' // 不需要编码的字符
    function encodeURIComponentNew(s) {
      var s = utf8(decodeURIComponent(s))
      var enc = ''
      for (var i= 0; i<s.length; i++) {
        if (okURIchars.indexOf(s.charAt(i)) === -1) {
          enc += '%' + toHex(s.charCodeAt(i))
        } else {
          enc += s.charAt(i)
        }
      }
      // 原型上添加方法
      String.prototype.replaceAll = function(s1,s2){ 
        return this.replace(new RegExp(s1,"gm"),s2); 
      }
      return enc.replaceAll('%20', '+') // 空格不编码
    }

    export default encodeURIComponentNew
```

### tool.js 工具文件

``` bash
    导出的encodeURIComponentNew 方法引用
    encodeURIComponentNew(params) =》 params为value值
```

#### 处理参数排序

``` bash
    function sortQueryResult (query) {
      if (!query) return null
      let queryStr = query.substring(1, query.length)
      let queryArr = queryStr.split('&')
      let keys = [], map = {}
      queryArr.forEach(item => {
        let keyValArr = item.split('=')
        if (keyValArr && keyValArr.length === 2) {
          if (map[keyValArr[0]]) {
            map[keyValArr[0]].push(encodeURIComponentNew(keyValArr[1]))
          } else {
            map[keyValArr[0]] = [encodeURIComponentNew(keyValArr[1])]
          }
        }
      })
      keys = Object.keys(map)
      keys = keys.sort()
      let rs = ''
      keys.forEach(m => {
        let ss = map[m]
        if (ss) {
          ss = ss.sort()
          ss.forEach(s => {
            rs += m + '=' + s + '&'
          })
        }
      })
      if (rs.endsWith('&')) {
        rs = rs.substring(0, rs.lastIndexOf('&'))
      }
      return rs
    }
```

### sha加密 新建sha.js 导出sha1方法

``` bash
    function sha1(s) {
      var data = new Uint8Array(encodeUTF8(s))
      var i, j, t
      var l = ((data.length + 8) >>> 6 << 4) + 16, s = new Uint8Array(l << 2)
      s.set(new Uint8Array(data.buffer)), s = new Uint32Array(s.buffer)
      for (t = new DataView(s.buffer), i = 0; i < l; i++)s[i] = t.getUint32(i << 2)
      s[data.length >> 2] |= 0x80 << (24 - (data.length & 3) * 8)
      s[l - 1] = data.length << 3
      var w = [], f = [
        function () { return m[1] & m[2] | ~m[1] & m[3] },
        function () { return m[1] ^ m[2] ^ m[3] },
        function () { return m[1] & m[2] | m[1] & m[3] | m[2] & m[3] },
        function () { return m[1] ^ m[2] ^ m[3] }
      ], rol = function (n, c) { return n << c | n >>> (32 - c) },
        k = [1518500249, 1859775393, -1894007588, -899497514],
        m = [1732584193, -271733879, null, null, -1009589776]
      m[2] = ~m[0], m[3] = ~m[1]
      for (i = 0; i < s.length; i += 16) {
        var o = m.slice(0)
        for (j = 0; j < 80; j++)
        w[j] = j < 16 ? s[i + j] : rol(w[j - 3] ^ w[j - 8] ^ w[j - 14] ^ w[j - 16], 1),
          t = rol(m[0], 5) + f[j / 20 | 0]() + m[4] + w[j] + k[j / 20 | 0] | 0,
          m[1] = rol(m[1], 30), m.pop(), m.unshift(t)
        for (j = 0; j < 5; j++)m[j] = m[j] + o[j] | 0
      }
      t = new DataView(new Uint32Array(m).buffer)
      for (var i = 0; i < 5; i++)m[i] = t.getUint32(i << 2)
      var hex = Array.prototype.map.call(new Uint8Array(new Uint32Array(m).buffer), function (e) {
          return (e < 16 ? "0" : "") + e.toString(16)
      }).join("")
      return hex
    }
    export default sha1
```

### 使用

``` bash
  1.sortQueryResult(params)
  2.按照后台规则拼接字符 signature
  3.通过sha1加密字符signature生成签名
```
