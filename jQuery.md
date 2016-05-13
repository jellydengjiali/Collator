- $(document).ready 的意思是等页面的文档（document）中的节点都加载完毕后，在执行后续的代码，因为我们在执行代码的时候，可能会依赖页面的某一个元素，我们要确保这个元素真正的的被加载完毕后才能正确的使用。

####如何把jQuery对象转成DOM对象？###

利用数组下标的方式读取到jQuery中的DOM对象

HTML代码

		<div>元素一</div>
		<div>元素二</div>
		<div>元素三</div>
JavaScript代码

	var $div = $('div') //jQuery对象
	var div = $div[0] //转化成DOM对象
	div.style.color = 'red' //操作dom对象的属性
用jQuery找到所有的div元素（3个），因为jQuery 对象也是一个数组结构，可以通过数组下标索引找到第一个div元素，通过返回的div对象然后调用它style属性然修改第一个div元素的颜色。这里需要注意的一点是，数组的索引是从0开始的，也就是第一个元素下标是0

####通过jQuery自带的get()方法####

jQuery对象自身提供一个.get() 方法允许我们直接访问jQuery对象中相关的DOM节点，get方法中提供一个元素的索引：

	var $div = $('div') //jQuery对象
	var div = $div.get(0) //通过get方法，转化成DOM对象
	div.style.color = 'red' //操作dom对象的属性
其实我们翻开源码，看看就知道了，get方法就是利用的第一种方式处理的，只是包装成一个get让开发者更直接方便的使用。

####DOM对象转化成jQuery对象####
相比较jQuery转化成DOM，开发中更多的情况是把一个dom对象加工成jQuery对象。$(参数)是一个多功能的方法，通过传递不同的参数而产生不同的作用。

如果传递给$(DOM)函数的参数是一个DOM对象，jQuery方法会把这个DOM对象给包装成一个新的jQuery对象
通过$(dom)方法将普通的dom对象加工成jQuery对象之后，我们就可以调用jQuery的方法了

HTML代码

	<div>元素一</div>
	<div>元素二</div>
	<div>元素三</div>
JavaScript代码

	var div = document.getElementsByTagName('div'); //dom对象
	var $div = $(div); //jQuery对象
	var $first = $div.first(); //找到第一个div元素
	$first.css('color', 'red'); //给第一个元素设置颜色
通过getElementsByTagName获取到所有div节点的元素，结果是一个dom合集对象，不过这个对象是一个数组合集(3个div元素)。通过$(div)方法转化成jQuery对象，通过调用jQuery对象中的first与css方法查找第一个元素并且改变其颜色。

####id选择器：一个用来查找的ID，即元素的id属性####

`$( "#id" )`
>id是唯一的，每个id值在一个页面中只能使用一次。如果多个元素分配了相同的id，将只匹配该id选择集合的第一个DOM元素

id选择器也是基本的选择器，jQuery内部使用JavaScript函数document.getElementById()来处理ID的获取。原生语法的支持总是非常高效的，所以在操作DOM的获取上，如果能采用id的话尽然考虑用这个选择器

###值得注意：###

id是唯一的，每个id值在一个页面中只能使用一次。如果多个元素分配了相同的id，将只匹配该id选择集合的第一个DOM元素。但这种行为不应该发生;有超过一个元素的页面使用相同的id是无效的

#####元素选择器#####
元素选择器：根据给定（html）标记名称选择所有的元素

描述：

`$( "element" )`
搜索指定元素标签名的所有节点，这个是一个合集的操作。同样的也有原生方法`getElementsByTagName()`函数支持

在CSS中，经常会在第一行写下这样一段样式

	* {padding: 0; margin: 0;}
通配符*意味着给所有的元素设置默认的边距。jQuery中我们也可以通过传递*选择器来选中文档页面中的元素

描述：

	$( "*" )
抛开jQuery，如果要获取文档中所有的元素，通过`document.getElementsByTagName()`中传递"*"同样可以获取到

####层级选择器####
![](http://i.imgur.com/RSBdYfc.png)

仔细观察层级选择器之间还是有很多相似与不同点

- 层级选择器都有一个参考节点
- 后代选择器包含子选择器的选择的内容
- 一般兄弟选择器包含相邻兄弟选择的内容
- 相邻兄弟选择器和一般兄弟选择器所选择到的元素，必须在同一个父元素下
	
		$( "parent > child" ) 
		子选择器：选择所有指定“parent”元素中指定的"child"的直接子元素。
		
		$("ancestor descendant")	
		后代选择器：选择给定的祖先元素的所有后代元素, 一个元素的后代可能是该元素的一个孩子，孙子，曾孙等
		
		$("prev + next")
		相邻兄弟选择器：选择所有紧接在“prev”元素后的“next”元素
		
		$("prev ~ siblings")
		一般兄弟选择器：匹配“prev”元素之后的所有 兄弟元素。具有相同的父元素，并匹配过滤“siblings”选择器

#####基本筛选选择器#####
![](http://i.imgur.com/YCd0jEu.png)

#####内容筛选选择器#####
基本筛选选择器针对的都是元素DOM节点，如果我们要通过内容来过滤，jQuery也提供了一组内容筛选选择器，当然其规则也会体现在它所包含的子元素或者文本内容上

内容过滤器描述如下表：
![](http://i.imgur.com/mRK8oHh.png)

注意事项：

- :contains与:has都有查找的意思，但是contains查找包含“指定文本”的元素，has查找包含“指定元素”的元素
- 如果:contains匹配的文本包含在元素的子元素中，同样认为是符合条件的。
- :parent与:empty是相反的，两者所涉及的子元素，包括文本节点

#####可见性筛选选择器######
元素有显示状态与隐藏状态，jQuery根据元素的状态扩展了可见性筛选选择器`:visible与:hidden`
![](http://i.imgur.com/yRsItPl.png)
`:hidden选择器，不仅仅包含样式是display="none"的元素，还包括隐藏表单、visibility等等`

#####我们有几种方式可以隐藏一个元素：

- CSS display的值是none
- type="hidden"的表单元素
- 宽度和高度都显式设置为0
- 一个祖先元素是隐藏的，该元素是不会在页面上显示
- CSS visibility的值是hidden
- CSS opacity的指是0

如果元素中占据文档中一定的空间,元素被认为是可见的。
可见元素的宽度或高度，是大于零。
元素的`visibility: hidden` 或 `opacity: 0`被认为是可见的，因为他们仍然占用空间布局。

    <script type="text/javascript">
    	//查找id = div1的DOM元素,是否可见
    	show($('#div1:visible'));
    </script>

    <script type="text/javascript">
    	//查找id = div2的DOM元素,是否可见
    	show( $('#div2:visible')  );
    </script>

    <script type="text/javascript">
    	//查找id = div3的DOM元素,是否可见
    	show(  $('#div3:visible')  );
    </script>

    <script type="text/javascript">
    	//查找id = div1的DOM元素,是否隐藏
    	show( $('#div1:hidden') );
    </script>

    <script type="text/javascript">
    	//查找id = div2的DOM元素,是否隐藏
    	show($('#div2:hidden'));
    </script>

    <script type="text/javascript">
    	//查找id = div3的DOM元素,是否隐藏
    	show($('#div3:hidden'));
    </script>

#####属性筛选选择器
![](http://i.imgur.com/vXGkUIv.png)
#####子元素筛选选择器
![](http://i.imgur.com/2GS0l3d.png)
#####表单元素选择器
![](http://i.imgur.com/DzYECEf.png)
#####表单对象属性筛选选择器
![](http://i.imgur.com/4hARqOV.png)
注意事项：

- 选择器适用于复选框和单选框，对于下拉框元素, 使用 `:selected `选择器
- 在某些浏览器中，选择器:checked可能会错误选取到`<option>`元素，所以保险起见换用选择器`input:checked`，确保只会选取`<input>`元素

>even:选择所引值为偶数的元素，从 0 开始.odd: 选择所引值为奇数的元素，从 0 开始计数

#####特殊选择器this

- this，表示当前的上下文对象是一个html对象，可以调用html对象所拥有的属性和方法。
- $(this),代表的上下文对象是一个jquery的上下文对象，可以调用jQuery的方法和属性值。

#####.attr()与.removeAttr()

attr()有4个表达式

 - attr(传入属性名)：获取属性的值
- attr(属性名, 属性值)：设置属性的值
- attr(属性名,函数值)：设置属性的函数值
- attr(attributes)：给指定元素设置多个属性值，即：{属性名一: “属性值一” , 属性名二: “属性值二” , … … }

--------------
removeAttr()删除方法

- .removeAttr( attributeName ) : 为匹配的元素集合中的每个元素中移除一个属性（attribute）

优点：

attr、removeAttr都是jQuery为了属性操作封装的，直接在一个 jQuery 对象上调用该方法，很容易对属性进行操作，也不需要去特意的理解浏览器的属性名不同的问题
>获取Attribute就需要用attr，获取Property就需要用prop

		 <script type="text/javascript">
    	//找到第三个input，通过使用一个函数来设置属性
    	//可以根据该元素上的其它属性值返回最终所需的属性值
    	//例如，我们可以把新的值与现有的值联系在一起：
    	$("input:eq(2)").attr('value',function(i, val){
    		return '通过function设置' + val
    	})
   	 </script>

#####html()及.text()

>.html()方法 

获取集合中第一个匹配元素的HTML内容 或 设置每一个匹配元素的html内容，具体有3种用法：

- .html() 不传入值，就是获取集合中第一个匹配元素的HTML内容
- .html( htmlString )  设置每一个匹配元素的html内容
- .html( function(index, oldhtml) ) 用来返回设置HTML内容的一个函数

注意事项：

-.htm()方法内部使用的是DOM的innerHTML属性来处理的，所以在设置与获取上需要注意的一个最重要的问题，这个操作是针对整个HTML内容（不仅仅只是文本内容）

>.text()方法

得到匹配元素集合中每个元素的文本内容结合，包括他们的后代，或设置匹配元素集合中每个元素的文本内容为指定的文本内容。，具体有3种用法：

- .text() 得到匹配元素集合中每个元素的合并文本，包括他们的后代
- .text( textString ) 用于设置匹配元素内容的文本
- .text( function(index, text) ) 用来返回设置文本内容的一个函数
注意事项：

- .text()结果返回一个字符串，包含所有匹配元素的合并文本
######.html与.text的异同:

- .html与.text的方法操作是一样，只是在具体针对处理对象不同
- .html处理的是元素内容，.text处理的是文本内容
- .html只能使用在HTML文档中，.text 在XML 和 HTML 文档中都能使用
- 如果处理的对象只有一个子文本节点，那么html处理的结果与text是一样的
- 火狐不支持innerText属性，用了类似的textContent属性，.text()方法综合了2个属性的支持，所以可以兼容所有浏览器

#####.val()方法

- .val()无参数，获取匹配的元素集合中第一个元素的当前值
- .val( value )，设置匹配的元素集合中每个元素的值
- .val( function ) ，一个用来返回设置值的函数

> 注意事项：

- 通过.val()处理select元素， 当没有选择项被选中，它返回null
- .val()方法多用来设置表单的字段的值
- 如果select元素有multiple（多选）属性，并且至少一个选择项被选中， .val()方法返回一个数组，这个数组包含每个选中选择项的值

#####切换样式.toggleClass()
>.toggleClass( )方法：在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性。即：如果存在（不存在）就删除（添加）一个类

- .toggleClass( className )：在匹配的元素集合中的每个元素上用来切换的一个或多个（用空格隔开）样式类名
- .toggleClass( className, switch )：一个布尔值，用于判断样式是否应该被添加或移除
- .toggleClass( [switch ] )：一个用来判断样式类添加还是移除的 布尔值
- .toggleClass( function(index, class, switch) [, switch ] )：用来返回在匹配的元素集合中的每个元素上用来切换的样式类名的一个函数。接收元素的索引位置和元素旧的样式类作为参数

注意事项：

- toggleClass是一个互斥的逻辑，也就是通过判断对应的元素上是否存在指定的Class名，如果有就删除，如果没有就增加
- toggleClass会保留原有的Class名后新增，通过空格隔开

>.addClass与.css方法各有利弊，一般是静态的结构，都确定了布局的规则，可以用addClass的方法，增加统一的类规则
如果是动态的HTML结构，在不确定规则，或者经常变化的情况下，一般多考虑.css()方式

#####jQuery提供的存储接口

		jQuery.data( element, key, value )   //静态接口,存数据
		jQuery.data( element, key )  //静态接口,取数据   
		.data( key, value ) //实例接口,存数据
		.data( key ) //实例接口,存数据
2个方法在使用上存取都是通一个接口，传递元素，键值数据。在jQuery的官方文档中，建议用.data()方法来代替。

我们把DOM可以看作一个对象，那么我们往对象上是可以存在基本类型，引用类型的数据的，但是这里会引发一个问题，可能会存在循环引用的内存泄漏风险

通过jQuery提供的数据接口，就很好的处理了这个问题了，我们不需要关心它底层是如何实现，只需要按照对应的data方法使用就行了

同样的也提供2个对应的删除接口，使用上与data方法其实是一致的，只不过是一个是增加一个是删除罢了

		jQuery.removeData( element [, name ] )
		.removeData( [name ] )
