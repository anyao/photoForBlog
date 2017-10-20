---
title: php设计模式
date: 2017-10-12 11:37:24
tags:
	- php
	- 设计模式
---
##### 工厂模式 #####

假如一个对象在很多文件中都进行 new 的一个操作。如果这个时候，对象的名称或者参数发生变化，则会造成麻烦。

另外，还隐藏了创建过程的复杂度。

	```
	class Factory{
		static function createDatebase(){
			$db = new Database;
			return $db;
		}
	}

	$db = Factory::createDatabase();
	```
<!--more-->

##### 单例模式 #####

节约资源。

	```
	class Database{
		private $db;
		private function __construct(){
		
		}

		static function getInstance(){
			if(isset(self::$db))
				self::$db = new  self();
			return self::$db;				
		}
	}

	$db = Database::getInstance();
	```

##### 注册树模式 #####

将对象注册到全局，用来被任何地方直接访问。
	
	```
	class Register{
		protected static $objects;
		static function set($alias, $object){
			self::$objects[$alias] = $object;
		}

		static  function get($name){
			return self::$objects[$alias];
		}

		function _unset($alias){
			unset(self::$objects[$alias]);
		}
	}
	
	class Factory{
		static function createDatabase(){
			$db = Database::getInstance();
			Register::set('db1', $db);
			return $db;
		}
	}

	$db = Register::get('db1');
	```

##### 适配器模式 #####

可以将截然不同的函数接口封装成统一的 API。

	```
	interface IDatabase{
		function connect($host, $user, $passwd, $dbname);
		function query($sql);
		function close();
	}

	class MySQL implements IDatabase{
		protected $conn;
		function connect($host, $user, $passwd, $dbname){
			$conn = mysql_connect($host, $user, $passwd);
			mysql_select_db($dbname, $conn);
			$this->conn = $conn;
		}

		function query($sql){
			$res = mysql_query($sql, $this->conn) or die('fail');
			$row = mysql_fetch_array($res, MYSQL_ASSOC);
			return $row;
		}

		function close(){
			mysql_close($this->conn);
		}
	}

	class MySQLi implements IDatabase{
		protected $conn;
		function connect($host, $user, $passwd, $dbname){
			$conn = mysqli_connect($host, $user, $passwd, $dbname);
			$this->conn = $conn;
		}

		function query($sql){
			return mysqli_query($sql, $this->conn);
		}

		function close(){
			mysqli_close($this->conn);
		}
	}

	class PDO implements IDatabase{
		protected $conn;
		function connect($host, $user, $passwd, $dbname){
			$conn = new PDO("mysql:host=$host;dbname=$dbname", $user, $passwd);
			$this->conn = $conn;
		}

		function query($sql){
			return $this->conn->query($sql, $this->conn);
		}

		function close(){
			unset($this->conn);
		}
	}

	$db = new IDatabase\PDO();
	```

##### 策略模式 #####

将一组特定的行为和算法封装成类，以适应某些特定的上次文环境。依赖注入：硬编码->解耦。
	
	```
	interface UserStrategy(){
		function showAd();
		function showCategory();
	}

	class FemaleStrategy implements UserStrategy{
		function showAd(){
			echo "female showAd";
		}

		function showCategory(){
			echo "female showCategory";
		}
	}

	class MaleStrategy implements UserStrategy{
		function showAd(){
			echo "Male showAd";
		}

		function showCategory(){
			echo "Male showCategory";
		}
	}

	class Page{
		protected $strategy;
		function index(){
			$this->strategy->showAd();
			$this->strategy->showCategory();
		}
		
		function setStrategy(UserStrategy $strategy){
			$this->strategy = $strategy;
		}
	}

	$page = new Page;
	$strategy = isset($_GET['strategy']) ? new FemaleStrategy() : new MaleStrategy();
	$page->setStrategy($strategy);
	$page->index();
	```

##### 数据对象映射模式 #####

将对象和数据存储映射起来，对一个对象的操作映射为对数据存储的操作。

	```
	class User{
		public $id, $name, $mobile, $regtime;
		function __construct($id){
			$this->db = new Database\MySQLi();
			$this->db->connect('127.0.0.1', 'root', 'ann', 'test');
			$res = $this->db->query('select * from user limit 1');
			$data = $res->fetch_assoc();

			$this->id = $data['id'];
			$this->name = $data['name'];
			$this->mobile = $data['regtime'];
		}

		function __destruct(){
			$this->db->query(
				"update user set 
				name = '{$this->name}',
				mobile = '{$this->mobile}',
				regtime = '{$this->regtime}'
				where id = {$this->id} limit 1");
		}		

	}

	$user = new User();
	$user->mobile = '17703100951';
	$user->name = 'ann';
	$user->regtime = date('Y-m-d H:i:s');
	```


	```
		class Factory{
			static function getUser($id){
				$key = 'user_'.$id;
				$user = Register::get($key);
				if(!$user){
					$user = new User($id);
					Register::set($key, $user);
				}
				return $user;
			}
		}

		$user = Factory::getUser(1);
	```

##### 观察者模式 #####

一个时间发生后，要执行一连串更新操作。传统的变成方式就是在事件的代码之后直接加入处理逻辑。当更新的逻辑增多之后，代码变得难以维护。这种方式是耦合的，侵入式的，增加新的逻辑需要修改事件主体的代码。

观察者模式：当一个对象状态发生改变时，依赖它的对象全部会收到通知，并自动更新。其实现了低耦合，非侵入式的通知与更新机制。

	```
	abstract class EventGenerator(){
		private $observers = [];
		function addObserver(Observer $observer){
			$this->observers[] = $observer;
		}
		function notify(){
			foreach($this->observers as $observer){
				$observer->update();
			}
		}
	}
	
	interface Observer{
		function update($event_info = null);
	}
	
	class Event extends EventGenerator{
		function trigger(){
			echo "change";
			$this->notify();
		}
	}

	class Observer1 implements Observer{
		function update($event_info = null){
			echo "update <br/>";
		}
	}

	$event = new Event;
	$event->addObserver(new Observer1);
	$event->trigger();
	```

##### 原型模式 #####

与工厂模式作用类似，都是用来创建对象。

与工厂模式的实现不同，原型模式是先创建好一个原型对象，然后通过 `clone` 原型对象来创建新的对象。这样就免去了类创建时重复的初始化操作。

原型模式适用于大对象的创建。创建一个大对象需要很大的开销，如果每次 `new` 就会开销很大。原型模式仅需内存拷贝即可。

	```
	$prototype = new Canvas();
	$prototype->init();
	$prototype->setColor();
		
	$canvas1 = clone $prototype;
	$canvas2 = clone $prototype;
	``` 

##### 装饰器模式 #####

装饰器模式（Decorator），可以动态地添加修改类的功能。

一个类提供了一项功能，如果要修改并添加额外的功能，传统的编程模式，需要写一个子类继承它，并重新实现类的方法。

使用装饰器模式，仅需在运行时添加一个装饰器对象即可实现，可以实现最大的灵活性。

	```
	interface DrawDecorator{
		function beforeDraw();
		function afterDraw();
	}

	class Canvas{
		protected $decorators = [];
		function draw(){
			$this->beforeDraw();
			...
			$this->afterDraw();
		}

		function addDecorator(DrawDecorator $decorator){
			$this->decorator[] = $decorator;
		}

		function beforeDraw(){
			foreach($this->decorators as $decorator){
				$decorator->beforeDraw();
			}
		}

		function afterDraw(){
			$decorators = array_reverse($this->decorators);
			foreach($decorators as $decorator){
				$decorator->afterDraw();
			}
			
		}
	}

	class ColorDrawDecorator implements DrawDecorator{
		protected $color;
		function __construct($color = 'red'){
			$this->color = $color;
		}
		function beforeDraw(){
			echo "<div style='color:{$this->color}'>";
		}
		function afterDraw(){
			echo "</div>";
		}
	}

	$canvas = new Canvas();
	$canvas->init();
	$canvas->addDecorator(new ColorDrawDecorator('blue'));
	$canvas->addDecorator(...);
	$canvas->rect(3, 6, 4, 12);
	$canvas->draw();
	```

##### 迭代器模式 #####

在不需要了解内部实现的前提下，遍历一个聚合对象的内部元素。相比于传统的编程模式，迭代器模式可以隐藏遍历元素所需的操作。

	```
	class AllUser implements Iterator{
		protected $ids;
		protected $data = [];
		protected $index;
		function __construct(){
			$db = Factory:getDatabase();
			$result = $db->query("SELECT id FROM user");
			$this->ids = $result->fetch_all(MYSQLI_ASSOC);
			
		}

		function current(){
			$id = $this->ids[$this->index]['id'];
			return	Factory::getUser($id);
		}

		function next(){
			$this->index ++;
		}

		function valid(){
			return $this->index < count($this->ids);
		}
		
		function rewind(){
			$this->index = 0;
		}
		
		function key(){
			return $this->index
		}
	}

	$users = new AllUser();
	foreach($users as $user){
		var_dump($user);
	}
	```

##### 代理模式 #####

在客户端与实体之间建立一个代理对象（proxy），客户端对实体进行的操作全部委派给代理对象，隐藏实体的具体实现细节。

proxy还可以与业务代码分离，部署到另外的服务器。业务代码通过 RPC 来委派任务。

	```
		interface IUserProxy{
			function getUserName($id);
			function setUserName($id, $name);
		}
		
		class Proxy implements IUserProxy{
			function getUserName($id){
				$db = Factory::getDatabase('slave');
				$db->query("select name from user where id = $id limit 1");
			}
			
			function setUserName($id, $name){
				$db = Factory::getDatabase('master');
				$db->query("update user set name='$name' where id = $id limit 1");
			}
		}
	```








		