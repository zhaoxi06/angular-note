1、使用submit按钮提交表单
```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>form表单提交方式</title>
</head>
<body>
<form id="form" action="test.php" method="post">
    First name:<br>
    <input type="text" name="firstname" value="Mickey"><br>
    Last name:<br>
    <input type="text" name="lastname" value="Mouse"><br><br>
    <input type="submit" value="提交">
</form>
<script>
    $("form").submit(function () {
        /**
        可以在这里做表单提交前的校验
        return false来阻止表单提交
        */
        var first = $("input[name='firstname']").val();
        var last = $("input[name='lastname']").val();

        if (first == "" || first == null || first == undefined) {
            alert("first");
            return false;/*阻止表单提交*/
        } else if (last == "" || last == null || last == undefined) {
            alert("last");
            return false;/*阻止表单提交*/
        } else {
            alert("提交");
            return true;
        }
    })
</script>
</body>
</html>
```
2、使用button按钮提交表单
```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>form表单提交方式</title>
</head>
<body>
<form id="form" action="test.php" method="post">
    First name:<br>
    <input type="text" name="firstname" value="Mickey"><br>
    Last name:<br>
    <input type="text" name="lastname" value="Mouse"><br><br>
    <button>登录</button>
</form>
<script>
    $("button").click(function () {
        /**
        可以在这里做表单提交前的校验
        return false来阻止表单提交
        */
        var first = $("input[name='firstname']").val();
        var last = $("input[name='lastname']").val();

        if (first == "" || first == null || first == undefined) {
            alert("first");
            return false;/*阻止表单提交*/
        } else if (last == "" || last == null || last == undefined) {
            alert("last");
            return false;/*阻止表单提交*/
        } else {
            alert("提交");
            $("#form").submit();
        }
    })
</script>
</body>
</html>
```
3、利用js进行表单提交
```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>form表单提交方式</title>
</head>
<body>
<form id="form" action="test.php" method="post">
    First name:<br>
    <input type="text" name="firstname" value="Mickey"><br>
    Last name:<br>
    <input type="text" name="lastname" value="Mouse"><br><br>
    <input type="submit" value="提交">
</form>
<div id="tijiao">点这里也可以提交</div>
<script>
    $("#tijiao").click(function () {
        /**
        可以在这里做表单提交前的校验
        return false来阻止表单提交
        */
        $("#form").submit();
    })
</script>
</body>
</html>
```
4、使用ajax实现form提交（有跨域问题）
```
<html>
<head>
    <title>login test</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta http-equiv="pragma" content="no-cache">
    <meta http-equiv="cache-control" content="no-cache">
    <meta http-equiv="expires" content="0">
    <meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
    <meta http-equiv="description" content="ajax方式">
    <script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
    <script type="text/javascript">
        function login() {
            $.ajax({
            //几个参数需要注意一下
                type: "POST",//方法类型
                dataType: "json",//预期服务器返回的数据类型
                url: "/users/login" ,//url
                data: $('#form1').serialize(),
                success: function (result) {
                    console.log(result);//打印服务端返回的数据(调试用)
                    if (result.resultCode == 200) {
                        alert("SUCCESS");
                    }
                    ;
                },
                error : function() {
                    alert("异常！");
                }
            });
        }
    </script>
</head>
<body>
<div id="form-div">
    <form id="form1" onsubmit="return false" action="##" method="post">
        <p>用户名：<input name="userName" type="text" id="txtUserName" tabindex="1" size="15" value=""/></p>
        <p>密　码：<input name="password" type="password" id="TextBox2" tabindex="2" size="16" value=""/></p>
        <p><input type="button" value="登录" onclick="login()">&nbsp;<input type="reset" value="重置"></p>
    </form>
</div>
</body>
</html>
```

事实上是[form表单提交没有跨域问题，但ajax提交存在跨域问题](https://www.cnblogs.com/tangjiao/p/9951477.html)