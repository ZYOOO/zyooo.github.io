# BigTalkDesignPattenNote


# 大话设计模式读书笔记



## 1.简单工厂模式

用一个单独的类来做创照实例的过程，这就是工厂

![image-20211008170555461](https://i.loli.net/2021/10/08/WTt8c4aLhlpxn3u.png)

``` 
public class OperationFactory{
	public static Operation createOperate(string operate){
		Operation oper = null;
		switch(operate){
			case "+":
				oper = new OperationAdd();
				break;
			case "-":
				oper = new OperationSub();
				break;
			case "*":
				oper = new OperationMul();
				break;
			case "/":
				oper = new OperationMul();
				break;
		}
		return oper;
	}
}
```

``` 
Operation oper;
oper = OperationFactory.createOperate("+");
oper.NumberA = 1;
oper.NumberB = 2;
double result = oper.GetResult();
```

### *UML类图

**UML类图图示样例**

![image-20211008170613771](https://i.loli.net/2021/10/08/geMjNRQAEsr1Hbo.png)

#### 类图

类图分为三层

第一层显示类的名称，如果是抽象类，则就用斜体显示。

第二层是类的特性，通常就是字段和属性。

第三层是类的操作，通常是方法或行为。

"+"表示public，"-"表示private ， "#"表示protected。

![image-20211008170943057](https://i.loli.net/2021/10/08/2AOx6mIhFYZ5unf.png)

#### 接口图

顶端有《interface》

第一行是接口名称

第二行是接口方法

![image-20211008171626373](https://i.loli.net/2021/10/08/swc47SWfu9odF36.png)

#### 继承

空心三角形+实线来表示

![image-20211008171717526](https://i.loli.net/2021/10/08/oYsBUELmpFWDraI.png)

#### 接口

空心三角形+虚线表示

![image-20211008171946522](https://i.loli.net/2021/10/08/hpISznZJRecABNt.png)

#### 关联

放一个类知道另一个类时，实线箭头来表示

```
class Penguin : Bird{
	private Climate climate;
}
```



![image-20211008172039252](https://i.loli.net/2021/10/08/TBVYyQGwExFLD9W.png)

#### 聚合

**"聚合表示一种弱的 '拥有' 关系，体现的是A对象可以包含B对象，但B对象不是A对象的一部分"**

用空心的菱形+实线箭头来表示

![image-20211008172407721](https://i.loli.net/2021/10/08/fc1w2oaymVUHriM.png)

#### 合成

**“合成（Composition，也有翻译成‘组合’的）是一种强的‘拥有’关系，体现了严格的部分和整体的关系，部分和整体的生命周期一样”**

用实心的菱形+实线箭头表示

![image-20211008172738136](https://i.loli.net/2021/10/08/rGONmtvPz4EVi9j.png)

#### 依赖关系

虚线箭头表示

![image-20211008172823700](https://i.loli.net/2021/10/08/1WVvkzxaEYTAtUc.png)

2021.10.08

## 2.策略模式

**策略模式**（Strategy）：它定义了 **算法家族** ，分别封装起来，让他们之间可以相互替换，此模式让算法的变化，不会影响到使用算法的客户。

 ![4935907e54f73452142b0f8cd3519a6](https://i.loli.net/2021/10/10/7kHJar8YvOnRmfD.jpg)

承担Strategy角色的一个类上定义所有支持的算法的公共接口，它的子类去实现各种不同的策略（算法）。

然后创建一个Context类，里面创建维护一个Strategy对象，（初始化需要做的判断可以放在这里，简单工厂和策略模式的结合，减轻客户端的压力）在初始化Context类的时候需要传入Strategy参数，这里传入的应该是Strategy的子类也就是各种实际的策略。在ContextInterface()中用strategy去调用Strategy中的那个公共接口，最后得到响应策略（算法）的结果，返回给客户端。

书本案例---市场促销

![a11de6923fd44929fef848cb1001cc9](https://i.loli.net/2021/10/10/cOLYq5BnjtXaA91.jpg)

```
class CashContext{
	CashSuper cs = null;
	public CashContext (string type){
		case "正常收费":
			CashNormal cs0 = new CashNormal();
			cs = cs0;
			break;
		case "满300返100":
			CashNormal cr1 = new CashReturn("300","100");
			cs = cr1;
			break;
		case "打8折":
			CashNormal cr2 = new CashNormal("0.8");
			cs = cr2;
			break;
	}
	public double GetResult(double money){
		return cs.acceptCash(money);
	}
}
```

```
//客户端窗体程序(主要部分)
doule total = 0.0d;
private void btnOk_Click(Object sender,EvenArgs e){
	CashContext csuper = new CashContext(cbxType.SelectedItem.ToString());
	double totalPrices = 0d;
	totalPrices = csuper.getResult(Conver.ToDouble(txtPrice.Text) * Convert.ToDouble(txtNum.Text));
	total = total + totalPrices;
	lbxList.Items.Add("单价" + txtPrice.Text + "数量" + txtNum.Text + " " 
				+ cbxType.SelectedItem + " 合计：" + totalPrices.ToString());
	lblResult.Text = total.ToString();
}
```



![379505ecd1757b3ad430142ec19b24c](https://i.loli.net/2021/10/10/ZwSdOQ4o5lfrJqj.jpg)

**"策略模式的Strategy类层次为Context定义了一系列的可供重用的算法或行为。继承有助于析取出这些算法中的公共功能"**

**"上例子的公共功能就是获得计算费用的结果，GetResult，这使得算法之间有了抽象的父类CashSuper"**

**策略模式另一个优点就是简化了单元测试，因为每个算法都有自己的类，可以通过自己的接口单独测试**

**策略模式就是用来封装算法的，只要在分析过程中听到需要在不同时间应用不同的业务规则，就可以考虑使用策略模式处理这种变化的可能性**



## 3.单一职责原则



"如果一个类承担的职责过多，就等于把这些职责耦合在一起，一个职责的变化可能会削弱或者抑制这个类完成其他职责的能力。这种耦合会导致脆弱的设计，当变化发生时，设计会遭受到意想不到的破坏[ASD]"

"软件设计真正要做的许多内容，就是发现职责并把那些职责相互分离。如果你能够想到多于一个的东西去改变一个类，那么这个类就具有多于一个的职责[ASD]"



## 4.开放-封闭原则

**开放-封闭原则，是说软件实体(类、模块、函数等等)应该是可拓展，但是不可修改**

**对于拓展是开放的(Open for extension)，对于更改是封闭的(Closed for modification)**

在最初编写代码时，假设变化不会发生。当变化发生时，我们就创建抽象来隔离以后发生的同类变化。例如写个简单的加法程序，开始直接在client类中完成，但是得想到后来需要增加一些其他运算的功能，于是增加一个抽象的运算类，利用一些如继承，多态等来隔离具体加法、减法与client耦合，如后续要增加乘法除法功能，就不需要再去更改client以及加法减法的类了，而是增加乘法和除法的子类就好。

**面对需求，对程序的改动是通过增加新代码进行的，而不是更改现有的代码。**



## 5.依赖倒转原则

抽象不应该依赖细节，细节应该依赖于抽象，说白了就是要对接口编程，不要对实现编程。

**A.高层模块不应该依赖低层模块。两个都应该依赖抽象。**

**B.抽象不应该依赖细节。细节应该依赖抽象。**

高层模块依赖于低层模块，如果需要用不同数据库或存储信息方式，那么连高层模块都不能使用了。

### #里氏代换原则

一个软件实体如果使用的是一个父类的话，那么一定适用于其子类，而且它察觉不出父类对象和子类对象的区别。也就是说，在软件里面，把父类都替换成它的子类，程序的行为没有变化，简单地说，子类型必须能够替换掉它们的父类型。

只有当子类可以替换掉父类，软件单位的功能不受到影响时，父类才能真正被复用，而子类也能够在父类的基础上添加新的行为，也就是因为这个才使得使用父类类型的模块在无需修改的情况下就可以扩展。

例如企鹅不能继承鸟类，虽然在生物学上企鹅是鸟类，但是在这说，企鹅不能飞，而鸟类指可以飞的，企鹅不能替换掉鸟类，所以不能继承。



## 6.装饰模式

**装饰模式，动态地给一个对象添加一些额外的职责，就增加功能来说，装饰模式比生成子类更为灵活。**

![8b8618c25fa595dfe3ef000d8dba91e](https://i.loli.net/2021/10/14/wOahjmX9GV8ZiYc.jpg)

![8125ccd8f32ad823fd1d3e5dc56d70f](https://i.loli.net/2021/10/14/pY8Gy4vnVegC7DI.jpg)

``` 
//Person类 ConcreteComponent

class Preson{
	public Person(){}
	private string name;
	public Person(string name){
		this.name = name;
	}
	public virtual void Show(){
		Console.WriteLine("装扮的{0}",name);
	}
}
```



```
//服饰类(Decorate)

class Finery : Person{
	protected Person component;
	//打扮
	public void Decorate (Person component){
		this.component = component;
	}
	public override void Show(){
		if(component != null){
			component.Show();
		}
	}
}
```



``` 
//具体服饰类 (ConcreteDecorator)

class TShirts : Finery{
	public override void Show(){
		Console.Write("大T恤");
		base.Show();
	}
}
class BigTrouser : Finery{
	public override void Show(){
		Console.Write("垮裤");
		base.Show();
	}
}
.....

```

```
static void Main(string[] args){
	Person xc = new Person("小菜");
	Console.WriteLine("\n第一种装扮:");
	
	Sneakers pqx = new Sneakers();
	BigTrouser kk = new BigTrouser();
	TShirts dtx = new TShirts();
	
	pqx.Decorate(xc);
	kk.Decorate(pqx);
	dtx.Decorate(kk);
	dtx.show;
	
	Console.WriteLine("\n第二种装扮");
	
	LeatherShoes px = new LeatherShoes();
	Tie ld = new Tie();
	Suit xz = new Suit();
	
	px.Decorate(xc);
	ld.Decorate(px);
	xz.Decorate(ld);
	xz.Show();
	
	Console.Read();
}
```

装饰模式是为已有功能动态地添加更多功能的一种方式,系统需要新的功能的时候,是向旧的类中添加新的代码,这些代码通常装饰了原有类的核心职责或主要行为,例如要添加领带ld,也保留了原来的小菜xc和皮鞋px.

**有效地把类的核心职责和装饰功能区分开了，而且可以去除相关类中重复的装饰逻辑**



## 7.代理模式

**代理模式，为其他对象提供一种代理以控制对这个对象的访问**

![f1ddfd24be4c0687ac682c58e3e708a](https://i.loli.net/2021/10/14/BIaoJKk197A6Fec.jpg)

![f1ddfd24be4c0687ac682c58e3e708a](https://i.loli.net/2021/10/14/BIaoJKk197A6Fec.jpg

![b9099e122c0362ffa3f0cb9b490ec06](https://i.loli.net/2021/10/14/mFfQbhlxtdekACN.jpg)

![7543a771966034a8c88a89123fdb878](https://i.loli.net/2021/10/14/TpjixSEchnWwYFI.jpg)

//案例

![1ec145ef1a8126aca82166d307dd0e1](https://i.loli.net/2021/10/14/YjDke6bKTJsiofg.jpg)

![d94db0e86b7312c76368e3738f245e2](https://i.loli.net/2021/10/14/3IqVQNMnY7pUltH.jpg)

![4e222ddc7cc87d471ab3464d98cdbe3](https://i.loli.net/2021/10/14/r8tADLTXngYzvGM.jpg)

**1，远程代理，也就是为了一个对象在不同的地址空间提供局部代表。这样可以隐藏一个对象存在于不同地址空间的试试。**

**2，虚拟代理，是根据需要创建开销很大的对象。通过它来存放实例化需要很长时间的真实对象**

**3，安全代理，用来控制真实对象访问时的权限**

**4，智能指引，是指当调用真实的对象时，代理处理另外一些事。**



## 8.工厂方法模式


