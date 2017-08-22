PreparedStatement 使用like 模糊查询
PreparedStatement 使用like
在使用PreparedStatement进行模糊查询的时候废了一番周折，以前一直都没有注意这个问题。一般情况下我们进行精确查询，sql语句 类似：select * from table where name =?，然后调用 PreparedStatement的setString等方法 给？指定值。那么模糊查询的时候应该怎么写呢？我首先尝试 了：select * from customer where name like ‘%?%’。
此时程序报错，因为？被包含在了单引号中，PreparedStatement并不视它为一个参数。后来上网查了相关的一些资料，发现可以这样写 select * from table where name like ？；但是在指定参数的时候把？指定为”%”+name+”%”，name是指 定的查询条件。这样就OK了。
一般情况下，我总是潜意识的认定了？就是取代所指定的参数，但是实际上我们可以对指定的参数进行了一定的包装之后再传给？，比如这里我们在参数的前后都加了一个%，然后再传给？   

String expr = "select * from  table where url like ?";
   pstmt = con.prepareStatement(expr);
   String a="a";
   pstmt.setString(1, "%"+a+"%");//自动添加单引号 （包装后的参数）
  pstmt.execute();
   System.out.println(pstmt.toString());//打印sql
   //会默认生成sql： select * from  table where url like '%http%'
