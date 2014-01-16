---
layout: post
title: "Character Encodings"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I wrote this a while back in an office blog, and then realized that a lot of people run into this problem, and would 
benefit from reading this.

----

Character Encoding is what decides how Strings which are first class citizens in most modern programming languages, get converted into byte arrays. Byte arrays are what get sent over the wire, or get written to disk. In the reverse direction, they decide how a byte array must be converted to a String.

This program takes a string in 3 different languages and shows how each language is affected by different charsets.

```groovy
import java.nio.charset.Charset
// println Charset.availableCharsets()*.key.join('\n')
void reportOnText(String text) {
  final encodings = [
      'ASCII', 'ISO-8859-1', 'windows-1251', 'UTF-8', 'UTF-16', 'UTF-32'
  ]
  println ''
  println text
  println text.replaceAll(/./,'=')
  encodings.each { enc ->
    def theBytes = text.getBytes(Charset.forName(enc))
    def reparse = new String(theBytes, enc)
    println "${enc.padRight(12)}: ${theBytes.encodeHex()} --> ${reparse}"
  }
}
reportOnText('Happy New Year!')
reportOnText('¡Feliz Año Nuevo!')
reportOnText('新年あけましておめでとうございます！')
reportOnText('KYPHON® Balloon Kyphoplasty')
```

Let's look at the output before we dig into the explanation

```
Happy New Year!
===============
ASCII       : 4861707079204e6577205965617221 --> Happy New Year!
ISO-8859-1  : 4861707079204e6577205965617221 --> Happy New Year!
windows-1251: 4861707079204e6577205965617221 --> Happy New Year!
UTF-8       : 4861707079204e6577205965617221 --> Happy New Year!
UTF-16      : feff004800610070007000790020004e00650077002000590065006100720021 --> Happy New Year!
UTF-32      : 0000004800000061000000700000007000000079000000200000004e0000006500000077000000200000005900000065000000610000007200000021 --> Happy New Year!
 
¡Feliz Año Nuevo!
=================
ASCII       : 3f46656c697a20413f6f204e7565766f21 --> ?Feliz A?o Nuevo!
ISO-8859-1  : a146656c697a2041f16f204e7565766f21 --> ¡Feliz Año Nuevo!
windows-1251: 3f46656c697a20413f6f204e7565766f21 --> ?Feliz A?o Nuevo!
UTF-8       : c2a146656c697a2041c3b16f204e7565766f21 --> ¡Feliz Año Nuevo!
UTF-16      : feff00a100460065006c0069007a0020004100f1006f0020004e007500650076006f0021 --> ¡Feliz Año Nuevo!
UTF-32      : 000000a100000046000000650000006c000000690000007a0000002000000041000000f10000006f000000200000004e0000007500000065000000760000006f00000021 --> ¡Feliz Año Nuevo!
 
新年あけましておめでとうございます！
==================
ASCII       : 3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f --> ??????????????????
ISO-8859-1  : 3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f --> ??????????????????
windows-1251: 3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f --> ??????????????????
UTF-8       : e696b0e5b9b4e38182e38191e381bee38197e381a6e3818ae38281e381a7e381a8e38186e38194e38196e38184e381bee38199efbc81 --> 新年あけましておめでとうございます！
UTF-16      : feff65b05e7430423051307e30573066304a3081306730683046305430563044307e3059ff01 --> 新年あけましておめでとうございます！
UTF-32      : 000065b000005e7400003042000030510000307e00003057000030660000304a000030810000306700003068000030460000305400003056000030440000307e000030590000ff01 --> 新年あけましておめでとうございます！
 
KYPHON® Balloon Kyphoplasty
===========================
ASCII       : 4b5950484f4e3f2042616c6c6f6f6e204b7970686f706c61737479 --> KYPHON? Balloon Kyphoplasty
ISO-8859-1  : 4b5950484f4eae2042616c6c6f6f6e204b7970686f706c61737479 --> KYPHON® Balloon Kyphoplasty
windows-1251: 4b5950484f4eae2042616c6c6f6f6e204b7970686f706c61737479 --> KYPHON® Balloon Kyphoplasty
UTF-8       : 4b5950484f4ec2ae2042616c6c6f6f6e204b7970686f706c61737479 --> KYPHON® Balloon Kyphoplasty
UTF-16      : feff004b005900500048004f004e00ae002000420061006c006c006f006f006e0020004b007900700068006f0070006c0061007300740079 --> KYPHON® Balloon Kyphoplasty
UTF-32      : 0000004b0000005900000050000000480000004f0000004e000000ae0000002000000042000000610000006c0000006c0000006f0000006f0000006e000000200000004b0000007900000070000000680000006f000000700000006c00000061000000730000007400000079 --> KYPHON® Balloon Kyphoplasty
```

As you can see, all the encodings we use do a great job with plain English text. That's because all encodings have support for the characters in the English alphabet. As we start making the alphabet more and more complex, we start seeing the difference between the Universal Encodings and the regional Encodings.

ASCII and windows-1521 have very limited support for anything other than English.

ISO-8859-1 improves support for Spanish, but Japanese is still broken.

All the UTF encodings are great for all languages.

Among the UTF charsets, UTF-32 takes 32 bits per character. UTF-16 takes a minimum of 16 bits. UTF-8 takes a minimum of 8 bits, but adds more bits to expand the character set.

#### What if we change charsets after encoding?
This is precisely what happens when one system encodes a message in one charset and another tries to parse it using a different charset.

```groovy
import java.nio.charset.Charset
void testWrongEncoding(String text) {
  def theBytes = text.getBytes(Charset.forName('ISO-8859-1'))
  def reparse = new String(theBytes, 'UTF-8')
  println "${text}: ${theBytes.encodeHex()} --> ${reparse}"
}
println "Wrong Encoding"
println "Wrong Encoding".replaceAll(/./,'=')
testWrongEncoding('Happy New Year!')
testWrongEncoding('¡Feliz Año Nuevo!')
testWrongEncoding('新年あけましておめでとうございます！')
testWrongEncoding('KYPHON® Balloon Kyphoplasty')
```

This is the result 

```
Wrong Encoding
==============
Happy New Year!: 4861707079204e6577205965617221 --> Happy New Year!
¡Feliz Año Nuevo!: a146656c697a2041f16f204e7565766f21 --> �Feliz A�o Nuevo!
新年あけましておめでとうございます！: 3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f3f --> ??????????????????
KYPHON® Balloon Kyphoplasty: 4b5950484f4eae2042616c6c6f6f6e204b7970686f706c61737479 --> KYPHON� Balloon Kyphoplasty
```

As you can see, there will be problems in parsing the string into UTF-8. This is what happens when another system uses a different encoding and we parse it using UTF-8.

#### Why UTF-8

You might already have guessed why we use UTF. It's because UTF can support all major characters we are likely to run into. But why UTF-8 in specific?

Because UTF-8 is the most compressed for the typical inputs we receive.

Other Charsets supported by the Java Runtime

```
[Big5, Big5-HKSCS, EUC-JP, EUC-KR, GB18030, GB2312, GBK, IBM-Thai, IBM00858, IBM01140, IBM01141, IBM01142, IBM01143, IBM01144, IBM01145, IBM01146, IBM01147, IBM01148, IBM01149, IBM037, IBM1026, IBM1047, IBM273, IBM277, IBM278, IBM280, IBM284, IBM285, IBM297, IBM420, IBM424, IBM437, IBM500, IBM775, IBM850, IBM852, IBM855, IBM857, IBM860, IBM861, IBM862, IBM863, IBM864, IBM865, IBM866, IBM868, IBM869, IBM870, IBM871, IBM918, ISO-2022-CN, ISO-2022-JP, ISO-2022-JP-2, ISO-2022-KR, ISO-8859-1, ISO-8859-13, ISO-8859-15, ISO-8859-2, ISO-8859-3, ISO-8859-4, ISO-8859-5, ISO-8859-6, ISO-8859-7, ISO-8859-8, ISO-8859-9, JIS_X0201, JIS_X0212-1990, KOI8-R, KOI8-U, Shift_JIS, TIS-620, US-ASCII, UTF-16, UTF-16BE, UTF-16LE, UTF-32, UTF-32BE, UTF-32LE, UTF-8, windows-1250, windows-1251, windows-1252, windows-1253, windows-1254, windows-1255, windows-1256, windows-1257, windows-1258, windows-31j, x-Big5-HKSCS-2001, x-Big5-Solaris, x-COMPOUND_TEXT, x-euc-jp-linux, x-EUC-TW, x-eucJP-Open, x-IBM1006, x-IBM1025, x-IBM1046, x-IBM1097, x-IBM1098, x-IBM1112, x-IBM1122, x-IBM1123, x-IBM1124, x-IBM1364, x-IBM1381, x-IBM1383, x-IBM33722, x-IBM737, x-IBM833, x-IBM834, x-IBM856, x-IBM874, x-IBM875, x-IBM921, x-IBM922, x-IBM930, x-IBM933, x-IBM935, x-IBM937, x-IBM939, x-IBM942, x-IBM942C, x-IBM943, x-IBM943C, x-IBM948, x-IBM949, x-IBM949C, x-IBM950, x-IBM964, x-IBM970, x-ISCII91, x-ISO-2022-CN-CNS, x-ISO-2022-CN-GB, x-iso-8859-11, x-JIS0208, x-JISAutoDetect, x-Johab, x-MacArabic, x-MacCentralEurope, x-MacCroatian, x-MacCyrillic, x-MacDingbat, x-MacGreek, x-MacHebrew, x-MacIceland, x-MacRoman, x-MacRomania, x-MacSymbol, x-MacThai, x-MacTurkish, x-MacUkraine, x-MS932_0213, x-MS950-HKSCS, x-MS950-HKSCS-XP, x-mswin-936, x-PCK, x-SJIS_0213, x-UTF-16LE-BOM, X-UTF-32BE-BOM, X-UTF-32LE-BOM, x-windows-50220, x-windows-50221, x-windows-874, x-windows-949, x-windows-950, x-windows-iso2022jp]
```