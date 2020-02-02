# Webhacking27
웹해킹kr 27번 문제 풀이 입니다.

# 27번 문제 풀이

## 소스코드
```javascript
<?php
  include "../../config.php";
  if($_GET['view_source']) view_source();
?><html>
<head>
<title>Challenge 27</title>
</head>
<body>
<h1>SQL INJECTION</h1>
<form method=get action=index.php>
<input type=text name=no><input type=submit>
</form>
<?php
  if($_GET['no']){
  $db = dbconnect();
  if(preg_match("/#|select|\(| |limit|=|0x/i",$_GET['no'])) exit("no hack");
  $r=mysqli_fetch_array(mysqli_query($db,"select id from chall27 where id='guest' and no=({$_GET['no']})")) or die("query error");
  if($r['id']=="guest") echo("guest");
  if($r['id']=="admin") solve(27); // admin's no = 2
}
?>
<br><a href=?view_source=1>view-source</a>
</body>
</html>
```

## 접근
1. 18번 문제랑 비슷하지만 다른 점은 no=() 괄호가 이미 쳐져있다는 것이다.
2.  id='guest' and no=(?)
3. 0) no=2— 괄호를 닫고 뒤부분을 주석처리하면..?
4. 안된다
5. 그러면 no=2도 괄호로 닫으면??
6. 정리해서 “0) or no=(2”를 입력하면?
7. 안된다 -> 필터링 코드에서 ‘(‘를 필터링처리함
8. ‘=‘도 필터링처리
9. 등호우회
    1. [a=b] = [a공백in공백(b)]으로됨
    2. 공백like공백b
10. 0)%09or%09no%09like%092%09--%09
    1. 주석 전 후에도 공백을 넣는다.
