# Junit 学习笔记

> 测试环境：Eclipse 4.3（自带 Junit 插件） | Junit 4.11

## 传统测试

* 写很多包含 main 方法的类。自己观察运行结果。

## 使用建议

* 测试代码和被测试代码分离，测试代码放在单独的包中。

* 测试代码包名和被测试代码的包名保持对应。

```c

// 测试代码目录示例:
// Eclipse 中 test 目录的创建方式：File -> New -> Other -> Source Folder
--src
----com.hgy.tool
--test
----com.hgy.tool
```

* 测试方法命名： test+<被测试方法名（首字母大写）>

* 测试类命名：<被测试类名>+Test

* 测试方法必须用 @Test 修饰

* 测试方法必须用public void 修饰，不能带任何参数。

* 测试方法之间不能有任何依赖。测试方法应该可以独立运行。

## 使用技巧

* Eclipse 可以批量生成测试方法
* 测试套件可以批量运行测试类

```java
// 测试套件示例
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class) // 修改测试运行器
@Suite.SuiteClasses({ATest.class, BTest.class}) // 添加测试类
public class SuiteTest {
    //测试套件类不能包含方法
    //这个测试套件类测试了ATest 和 BTest
}
```

* 使用参数化测试可以对一个方法多组参数验证测试

```java
// 参数化测试示例
import static org.junit.Assert.*;

import java.util.Arrays;
import java.util.Collection;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.Parameters;

// 更改测试运行器
@RunWith(Parameterized.class)
public class ParameterTest {
    // 声明变量存放预期值和输入值
	int expected = 0;
	int arg1 = 0;
	int arg2 = 0;
	
    // 声明公共静态方法
	@Parameters
	public static Collection<Object[]> t(){
		return Arrays.asList(new Object[][]{
				{3,2,1},
				{4,2,2}
		});
	}
	
    // 声明构造函数
	public ParameterTest(int expected, int arg1, int arg2){
		this.expected = expected;
		this.arg1 = arg1;
		this.arg2 = arg2;
	}
	
	@Test
	public void testAdd(){
		assertEquals(expected, new Calcu().add(arg1, arg2));
	}

}

```

## Junit 的反馈类型

* Failure 方法返回结果和预期的不一样。

* Error 代码运行出现了异常。

## Junit 常用注释

* `@Test` 将一个普通方法修饰成为一个测试方法
  * @Test 可以传入参数:
    * expected 参数，用于捕获异常
    * timeout 参数，用于限定函数执行时间 
* `@BeforeClass` 被修饰的方法会在所有其他方法之前执行
* `@AfterClass` 被修饰的方法会在所有其他方法都执行完之后执行
* `@Before` 被修饰的方法会在每个测试方法运行之前执行
* `@After` 被修饰的方法会在每个测试方法运行之后执行

* `@Ignore` 被修饰的方法不会被执行
* `@RunWith` 可以修改测试运行器

```c
// junit 注释方法 执行顺序示例

@BeforeClass

@Before
@Test
@After

@Before
@Test
@After

...

@AfterClass
```



## 参考资料
[1]. [《JUnit—Java单元测试必备工具》](https://www.imooc.com/coursescore/356). 慕课网








