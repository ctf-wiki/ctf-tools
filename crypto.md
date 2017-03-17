# Crypto

## 古典密码

- [CAP4](http://down.40huo.cn/crypto/CAP4.zip)

- [JPK - 406](http://down.40huo.cn/crypto/JPK_406.jar)

- [RC4 在线加解密](http://rc4.online-domain-tools.com/)

- [栅栏密码加解密工具](http://down.40huo.cn/crypto/%E6%A0%85%E6%A0%8F%E5%AF%86%E7%A0%81%E5%8A%A0%E8%A7%A3%E5%AF%861.10.rar)

- [摩斯密码在线加解密](http://www.zhongguosou.com/zonghe/moErSiCodeConverter.aspx)

- [维吉尼亚密码在线解密 1](https://www.guballa.de/vigenere-solver)

- [维吉尼亚密码在线解密 2](http://www.mygeocachingprofile.com/codebreaker.vigenerecipher.aspx)

- [厦大 ph0en1x 在线密码工具](http://tool.ph0en1x.com/)

- [密码机器](http://heartsnote.com/tools/cipher.htm)

  栅栏、凯撒、维吉尼亚、摩斯、置换等。

- [quipquip](http://quipqiup.com/)

  移位密码破解。

- [PYG 密码学综合工具](http://down.40huo.cn/crypto/pyg%E5%AF%86%E7%A0%81%E5%AD%A6%E7%BB%BC%E5%90%88%E5%B7%A5%E5%85%B7.zip)

## RSA

- [yafu 大数分解](http://down.40huo.cn/crypto/yafu-1.34.zip)

- [factordb 在线大数分解](http://factordb.com/)

- [RSATool](http://down.40huo.cn/crypto/RSATool2v17.rar_87752.rar)

- [wiener-attack](https://github.com/pablocelayes/rsa-wiener-attack)

- [rsatool](https://github.com/ius/rsatool)

  ```bash
  python rsatool.py -f PEM -o key.pem -n 13826123222358393307 -d 9793706120266356337
  python rsatool.py -f DER -o key.der -p 4184799299 -q 3303891593
  ```


## Hash

* [CRC32 碰撞脚本](https://github.com/theonlypwner/crc32/blog/master/crc32.py)

  ```bash
  crc32.py -h
  usage: crc32.py [-h] action ...

  Reverse, undo, and calculate CRC32 checksums

  positional arguments:
    action
      flip      flip the bits to convert normal(msbit-first) polynomials to
                reversed (lsbit-first) and vice versa
      reciprocal
                find the reciprocal (Koopman notation) of a reversed (lsbit-
                first) polynomial and vice versa
      table     generate a lookup table for a polynomial
      reverse   find a patch that causes the CRC32 checksum to become a desired
                value
      undo      rewind a CRC32 checksum
      calc      calculate the CRC32 checksum

  optional arguments:
    -h, --help  show this help message and exit
  ```

## 其他

- [Cisco 密码在线破解](http://www.ifm.net.nz/cookbooks/passwordcracker.html)
- [Base64 加解密](http://base64.supfree.net/)