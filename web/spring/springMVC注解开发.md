# springMvc常用注解说明 #

在SpringMVC中，控制器Controller负责处理由DispatcherServlet分发的请求，它把用户请求的数据经过业务处理层处理之后封装成一个Model，然后再把该Model返回给对应的View进行展示。

@Controller注解和@RequestMapping注解

@Controller 用于标记在一个类上，使用它标记的类就是一个SpringMVC Controller对象,分发处理器将会扫描使用了该注解的类的方法，并检测该方法是否使用了@RequestMapping注解。@Controller 只是定义了一个控制器类，而使用@RequestMapping注解的方法才是真正处理请求的处理器

@RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上，用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

	@Controller
	@RequestMapping(value="/book")
	public class BookController {
	    @RequestMapping(value="/title")
	    public String getTitle(){
	        return "title";
	    }           
	    
	    @RequestMapping(value="/content")
	    public String getContent(){
	        return "content";
	    } 
	}

RequestMapping注解有六个属性分别如下所示：

- value：指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）；
- method：指定请求的method类型， GET、POST、PUT、DELETE等；
- params：指定request中必须包含某些参数值是，才让该方法处理。
- consumes：指定处理请求的提交内容类型（Content-Type），例如application/json, text/html
- produces:指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；
- headers：指定request中必须包含某些指定的header值，才能让该方法处理请求。

RequestMapping属性讲解：

> value属性

普通的具体值。

	如前面的value="/book"。
含某变量的一类值。

	@RequestMapping(value="/get/{bookId}")
	public String getBookById(@PathVariable String bookId,Model model){
	    model.addAttribute("bookId", bookId);
	    return "book";
	} 

	路径中的bookId可以当变量，@PathVariable注解即提取路径中的变量值。

ant风格

	@RequestMapping(value="/get/id?")：可匹配“/get/id1”或“/get/ida”，但不匹配“/get/id”或“/get/idaa”;
	@RequestMapping(value="/get/id*")：可匹配“/get/idabc”或“/get/id”，但不匹配“/get/idabc/abc”;
	@RequestMapping(value="/get/id/*")：可匹配“/get/id/abc”，但不匹配“/get/idabc”;
	@RequestMapping(value="/get/id/**/{id}")：可匹配“/get/id/abc/abc/123”或“/get/id/123”，也就是Ant风格和URI模板变量风格可混用。

含正则表达式的一类值
	
	@RequestMapping(value="/get/{idPre:\\d+}-{idNum:\\d+}")：可以匹配“/get/123-1”，但不能匹配“/get/abc-1”，这样可以设计更加严格的规则，可以通过@PathVariable 注解提取路径中的变量(idPre,idNum)

或关系
	
	@RequestMapping(value={"/get","/fetch"} )即 /get或/fetch都会映射到该方法上。

> method：指定请求的method类型， GET、POST、PUT、DELETE等

	@RequestMapping(value="/get/{bookid}",method={RequestMethod.GET,RequestMethod.POST})

> params：指定request中必须包含某些参数值是，才让该方法处理。

	@RequestMapping(params="action=del")，
	请求参数包含“action=del”,如：http://localhost:8080/book?action=del
	
> headers：指定request中必须包含某些指定的header值，才能让该方法处理请求。

	@RequestMapping(value="/header/id", headers="Accept=application/json")：
	表示请求的URL必须为“/header/id 且请求头中必须有“Accept=application/json”参数即可匹配。

	@Controller
	@RequestMapping("/owners/{ownerId}")
	public class RelativePathUriTemplateController {
	
	@RequestMapping(value = "/pets", method = RequestMethod.GET, headers="Referer=http://www.ifeng.com/")
	  public void findPet(@PathVariable String ownerId, @PathVariable String petId, Model model) {    
	    // implementation omitted
	  }
	}
	仅处理request的header中包含了指定“Refer”请求头和对应值为“http://www.ifeng.com/”的请求。
 	
> consumes：指定处理请求的提交内容类型（Content-Type），例如application/json, text/html。

	@Controller
	@RequestMapping(value = "/pets", method = RequestMethod.POST, consumes="application/json")
	public void addPet(@RequestBody Pet pet, Model model) {    
	    // implementation omitted
	}
	方法仅处理request Content-Type为“application/json”类型的请求。

> produces: 指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回。 

	@Controller
	@RequestMapping(value = "/pets/{petId}", method = RequestMethod.GET, produces="application/json")
	@ResponseBody
	public Pet getPet(@PathVariable String petId, Model model) {    
	    // implementation omitted
	}
	方法仅处理request请求中Accept头中包含了"application/json"的请求，同时暗示了返回的内容类型为application/json;

@RequestParam用于将请求参数区数据映射到功能处理方法的参数上

	public String requestparam1(@RequestParam String username)
	请求中包含username参数（如/requestparam1?username=zhang），则自动传入。

@RequestParam有以下三个参数：

- value：参数名字，即入参的请求参数名字，如username表示请求的参数区中的名字为username的参数的值将传入；
- required：是否必须，默认是true，表示请求中一定要有相应的参数，否则将抛出异常；
- defaultValue：默认值，表示如果请求中没有同名参数时的默认值，设置该参数时，自动将required设为false。

	public String requestparam4(@RequestParam(value="username",required=false) String username)

表示请求中可以没有名字为username的参数，如果没有默认为null，此处需要注意如下几点：

基本类型：必须有值，否则抛出异常，如果允许空值请使用包装类代替，Boolean包装类型：默认Boolean.FALSE，其他引用类型默认为null。
	
如果请求中有多个同名的应该如何接收呢？如给用户授权时，可能授予多个权限，首先看下如下代码：
	
	public String requestparam7(@RequestParam(value="role") String roleList)
	如果请求参数类似于url?role=admin&rule=user，则实际roleList参数入参的数据为“admin,user”，即多个数据之间使用“，”分割；我们应该使用如下方式来接收多个请求参数：
	
	public String requestparam7(@RequestParam(value="role") String[] roleList)
	或者
	public String requestparam8(@RequestParam(value="list") List<String> list) 
 
@PathVariable用于将请求URL中的模板变量映射到功能处理方法的参数上。

	@RequestMapping(value="/users/{userId}/topics/{topicId}")
	public String test(@PathVariable(value="userId") int userId, @PathVariable(value="topicId") int topicId)   

 如请求的URL为“控制器URL/users/123/topics/456”，则自动将URL中模板变量{userId}和{topicId}绑定到通过@PathVariable注解的同名参数上，即入参后userId=123、topicId=456。

@ModelAttribute可以应用在方法参数上或方法上，他的作用主要是当注解在方法参数上时会将注解的参数对象添加到Model中；当注解在请求处理方法Action上时会将该方法变成一个非请求处理的方法，但其它Action被调用时会首先调用该方法。

@ModelAttribute注释一个方法

被@ModelAttribute注释的方法表示这个方法的目的是增加一个或多个模型(model)属性。这个方法和被@RequestMapping注释的方法一样也支持@RequestParam参数，但是它不能直接被请求映射。实际上，控制器中的@ModelAttribute方法是在同一控制器中的@RequestMapping方法被调用之前调用的。

被@ModelAttribute注释的方法用于填充model属性，例如，为下拉菜单填充内容，或检索一个command对象（如，Account），用它来表示一个HTML表单中的数据。
一个控制器可以有任意数量的@ModelAttribute方法。所有这些方法都在@RequestMapping方法被调用之前调用。
有两种类型的@ModelAttribute方法。一种是：只加入一个属性，用方法的返回类型隐含表示。另一种是：方法接受一个Model类型的参数，这个model可以加入任意多个model属性。

(1)@ModelAttribute注释void返回值的方法

	复制代码
	@Controller
	@RequestMapping(value="/test")
	public class TestController {
	    
	    /**
	     * 1.@ModelAttribute注释void返回值的方法
	     * @param abc
	     * @param model
	     */
	    @ModelAttribute
	    public void populateModel(@RequestParam String abc, Model model) {
	        model.addAttribute("attributeName", abc);
	    }
	    
	    @RequestMapping(value = "/helloWorld")
	    public String helloWorld() {
	       return "test/helloWorld";
	    }
	}

这个例子，在获得请求/helloWorld 后，populateModel方法在helloWorld方法之前先被调用，它把请求参数（/helloWorld?abc=text）加入到一个名为attributeName的model属性中，在它执行后helloWorld被调用，返回视图名helloWorld和model已由@ModelAttribute方法生产好了。这个例子中model属性名称和model属性对象由model.addAttribute()实现，不过前提是要在方法中加入一个Model类型的参数。

(2)@ModelAttribute注释返回具体类的方法

	/**
	 * 2.@ModelAttribute注释返回具体类的方法
	 * @param id
	 * @return
	 */
	@ModelAttribute
	public User getUserInfo(String id){
	    if(id!=null && !id.equals("")){
	        return userService.getUserInfo(id);
	    }
	    return null;
	}

这种情况，model属性的名称没有指定，它由返回类型隐含表示，如这个方法返回User类型，那么这个model属性的名称是user。这个例子中model属性名称有返回对象类型隐含表示，model属性对象就是方法的返回值。它无须要特定的参数。

(3)@ModelAttribute(value="")注释返回具体类的方法

	@Controller
	@RequestMapping(value="/test")
	public class TestController {
	    
	    /**
	     * 3.@ModelAttribute(value="")注释返回具体类的方法
	     * @param abc
	     * @return
	    */
	    @ModelAttribute("str")
	    public String getParam(@RequestParam String param) {
	        return param;
	    }
	    
	    @RequestMapping(value = "/helloWorld")
	    public String helloWorld() {
	       return "test/helloWorld";
	    }
	}

这个例子中使用@ModelAttribute注释的value属性，来指定model属性的名称。model属性对象就是方法的返回值。它无须要特定的参数。


