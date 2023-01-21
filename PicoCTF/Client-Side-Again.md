# Client-Side-Again  → 200 Pts.

1. We have a login page in front of us and just like last time we can just fuzz the sources and we can see that we have a file index file 

```jsx
<script type = "text/javascript" >
var _0x5a46 = ['0a029}', '_again_5', 'this', 'Password\x20Verified',
'Incorrect\x20password', 'getElementById', 'value', 'substring',
'picoCTF{', 'not_this'
];
(function(_0x4bd822, _0x2bd6f7) {
var _0xb4bdb3 = function(_0x1d68f6) {
while (--_0x1d68f6) {
_0x4bd822['push'](notion://www.notion.so/_0x4bd822%5B'shift'%5D());
}
};
_0xb4bdb3(++_0x2bd6f7);
}(_0x5a46, 0x1b3));
var _0x4b5b = function(_0x2d8f05, _0x4b81bb) {
_0x2d8f05 = _0x2d8f05 - 0x0;
var _0x4d74cb = _0x5a46[_0x2d8f05];
return _0x4d74cb;
};

function verify() {
checkpass = document[_0x4b5b('0x0')](notion://www.notion.so/'pass')[_0x4b5b('0x1')];
split = 0x4;
if (checkpass[_0x4b5b('0x2')](0x0, split * 0x2) == _0x4b5b('0x3')) {
if (checkpass[_0x4b5b('0x2')](0x7, 0x9) == '{n') {
if (checkpass[_0x4b5b('0x2')](split * 0x2, split * 0x2 * 0x2) == _0x4b5b(
'0x4')) {
if (checkpass[_0x4b5b('0x2')](0x3, 0x6) == 'oCT') {
if (checkpass[_0x4b5b('0x2')](split * 0x3 * 0x2, split * 0x4 * 0x2) ==
_0x4b5b('0x5')) {
if (checkpass['substring'](0x6, 0xb) == 'F{not') {
if (checkpass[_0x4b5b('0x2')](split * 0x2 * 0x2, split * 0x3 *
0x2) == _0x4b5b('0x6')) {
if (checkpass[_0x4b5b('0x2')](0xc, 0x10) == _0x4b5b('0x7')) {
alert(_0x4b5b('0x8'));
```

2. What we have here is the flag is split into parts and we don’t have any sequence manner. 

3. So I tried putting 2 things together after studying the code.

`
 'picoCTF{', 'not_this’
`

`
💡  '0a029}', '_again_5’

`
What if we can arrange them?

`picoCTF{not_this_again_50a029}

`

4. There we go our flag is here.
