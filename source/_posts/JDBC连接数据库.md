title: JDBC连接数据库简明教程
date: 2015-02-27 15:53:29
categories: [Java]
tags: [JDBC]
---

创建一个以`JDBC`连接数据库的程序，大概需要7步：

### 加载JDBC驱动程序

在连接数据库之前，首先要加载想要连接的数据库的驱动程序到**JVM**。或者通过`java.lang.Class`类的静态方法`forName(String className)`来实现。
```
try {
	/** 1. 记载 JDBC 驱动程序 */
	Class.forName("com.mysql.jdbc.Driver");
} catch (ClassNotFoundException e) {
	System.out.println("找不到驱动程序类 ，加载驱动失败！");
	e.printStackTrace();
}
```
成功加载后，会将`Driver`类的实例注册到`DriverManager`类中。
<!-- more -->
### 提供JDBC连接的URL

连接**URL**定义了连接数据库时的协议、子协议和数据源标识。格式为：*jdbc:subprotocol:subname*。
书写形式：协议：子协议：数据源标识
协议：在**JDBC**中总是以`jdbc`开始。
子协议：是桥连接的驱动程序或者数据库管理系统的名称,例如本教程中的`mysql`。
数据源标识：标记找到数据库来源的地址与连接端口，默认端口3306可以不写。
```
/** 2. 提供 JDBC 连接的 URL */
String url = "jdbc:mysql://localhost:3306/mrbs3?useUnicode=true&characterEncoding=utf-8";
String user = "root";
String password = "root";
```
`useUnicode=true`表示使用Unicode字符集。
`characterEncoding=utf-8`字符编码方式。需要与mysql数据库中的字符编码一致。

### 创建数据库的连接

要连接数据库，需要向`java.sql.DriverManager`请求，获取`java.sql.Connection`对象，该对象就代表一个数据库连接。
使用`DriverManager.getConnection(String url, String user, String password)`方法传入指定的将要连接的数据库的连接字符串以及数据库用户名和密码。
```
/** 3. 创建数据库的连接 */
Connection con = null;
try {
	con = DriverManager.getConnection(url, user, password);
} catch (SQLException e) {
	System.out.println("数据库连接失败！");  
	e.printStackTrace();
}
```

### 创建一个Statement

```
/** 4. 创建一个 Statement */
// 1、执行静态SQL语句。通常通过Statement实例实现。
Statement state = null;
// 2、执行动态SQL语句。通常通过PreparedStatement实例实现。
PreparedStatement pstmt = null;
// 3、执行数据库存储过程。通常通过CallableStatement实例实现。
CallableStatement cstmt = null;
try {
	state = con.createStatement();
	pstmt = con.prepareStatement("select * from ...");
	cstmt = con.prepareCall("{CALL demoSp(? , ?)}");
} catch (SQLException e) {
	e.printStackTrace();
}
```

### 执行SQL语句

```
/** 5. 执行 SQL */
// 1. 执行查询数据库的SQL语句，返回一个结果集（ResultSet）对象。
ResultSet rs = null;
// 2. 用于执行INSERT、UPDATE或DELETE语句以及SQL DDL语句，如：CREATE TABLE和DROP TABLE等
int row = 0;
// 3. 用于执行返回多个结果集、多个更新计数或二者组合的语句。
boolean flag = false;
try {
	rs = state.executeQuery("select * from meeting");
	row = state.executeUpdate("insert into ...");
	flag = state.execute("");
} catch (SQLException e) {
	e.printStackTrace();
}
```

### 处理数据库操作结果

```
/** 6. 处理结果 */
// 1. 执行更新返回的是本次操作影响到的结果数。
// 2. 执行查询返回的结果是一个 ResultSet 对象。
try {
	while(rs.next()){
		int id = rs.getInt(1); // 此方法比较高效，（列是从左到右编号的，并且从列1开始）
		String creator = rs.getString("creator");
		System.out.println(id + " : " + creator);
	}
} catch (SQLException e) {
	e.printStackTrace();
}
```

### 关闭JDBC对象
```
/** 7. 关闭 JDBC 对象 */
// 操作完成以后要把所有使用的JDBC对象全都关闭，以释放JDBC资源，关闭顺序和声明顺序相反：   
// 1、关闭记录集   
// 2、关闭声明   
// 3、关闭连接对象
if(rs != null){
	try {
		rs.close();
	} catch (SQLException e) {
		e.printStackTrace();
	}
}
if(state != null){
	try {
		state.close();
	} catch (SQLException e) {
		e.printStackTrace();
	}
}
if(con != null){
	try {
		con.close();
	} catch (SQLException e) {
		e.printStackTrace();
	}
}
}
```

### 结论党请看源码
```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * 
 * @author BaiNingHan</br>
 * @date 2012-7-16
 * 
 */
public class JDBC2 {
	public static void main(String[] args) {
		String driver = "com.mysql.jdbc.Driver";
		String dbName = "mrbs3";
		String passwrod = "root";
		String userName = "root";
		String url = "jdbc:mysql://localhost:3306/" + dbName;
		String sql = "select * from meeting";

		try {
			Class.forName(driver);
			Connection conn = DriverManager.getConnection(url, userName,
					passwrod);
			PreparedStatement ps = conn.prepareStatement(sql);
			ResultSet rs = ps.executeQuery();
			while (rs.next()) {
				System.out.println("id : " + rs.getInt(1) + " creator : "
						+ rs.getString("creator"));
			}

			// 关闭记录集
			if (rs != null) {
				try {
					rs.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}

			// 关闭声明
			if (ps != null) {
				try {
					ps.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}

			// 关闭链接对象
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}

		} catch (Exception e) {
			e.printStackTrace();
		}
	}

}
```

---
2015-02-27 Fancye