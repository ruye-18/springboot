# a journey to study springboot
![a journey to study springboot](https://repository-images.githubusercontent.com/185011206/4005dd00-7653-11e9-9cdb-e97ce9da0e2e)
## quickstart
### create maven project 
  *how to create a simple springboot project?*
   1. using maven project using maven-archetype-quickstart
   2. import parent project in pom file
   3. create main-function annotationed with @SpringBootApplication
   4. import spring-boot-maven-plugin plugin in pom file
   
   *project structrue*
   - src/main/java is srource folder
   - test/main/java is test folder  
   - pom.xml is maven configration  
    
## web project
   *How to design restfull interface?*
   1. import spring-boot-starter-web dependence in pom file
   2. create controller annotationed with @RestController
   3. coding restfull interface  annotationed with @RequestMapping
   4. mvn package
   5. run project using commond 'java -jar springboot-1.0-SNAP SHOT.jar'
   6. curl 'http://127.0.0.1:8080/index'
    
## junit test
   *Mock with MockMvc to initiate the http request, which is actually a class method call, then execute the controller, using Mockito's mock to mock the return of the underlying data during the execution, and finally validate the result.*
   
   *How to junit test?*
   1. import spring-boot-starter-test dependence in pom file
   2. create test class in src/test folder annotationed with @RunWith @SpringBootTest @AutoConfigureMockMvc
   3. coding test function
   
   *junit interpretation* 
   - @RunWith is used for running test
   - @SpringBootTest is used for statementing springboot test
   - @AutoConfigureMockMvc is used for autowiring beans
  
## springsecurity
### step1 jwt
   *jwt is json  web token*
   *How to integrate springsecurity and jwt?*
   1. import spring-boot-starter-security and jjwt in pom file
   2. new application.properties is using for configing jwt params
   3. new WebSecurityConfig.java using for security config,such as handler class,url without token, auth filter,generate UserDetail
   4. hanlers contains MyAuthenticationFailHandler.java MyAuthenticationSuccessHandler.java MyaccessDeniedHandler.java MyAuthenticationEntryPointHandler.java MyLogoutSuccessHandler.java
   5. new UserDetailsServiceImpl.java is using for loading auth and new SecurityUserDetails.java is using for wrapping auth
   6. JWTAuthenticationFilter.java is using for checking token as filter
   7. other custom class,BaseException.java JwtTokenUtil.java 
   8. SecuticyController.java is useing defining interface
   
   *How to test jwt?*
   1. curl -X POST 'http://127.0.0.1/security/login' -d 'username=grandhappy&password=123456
   
     {
          "data": "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJncmFuZGhhcHB5IiwianRpIjoiMiIsImF1dGhvcml0aWVzIjoiW3tcImF1dGhvcml0eVwiOlwiY29tbW9uXCJ9XSIsImV4cCI6MTU1NzMyNzI4OH0.Ekf5MmXJA9EqKd6gVTbUbtlcLZ27qvcCU9Ph01Zuhx36BK-cCabj9krIY_C0yZsWP6J-lz_UT3TV7vMdS3D8sA",
          "message": "登录成功",
          "status": 200
      }
   2. curl -X GET http://127.0.0.1/security/noauth
   
     noauth
   3. curl -X GET 'http://127.0.0.1/security/needauth' -H 'accessToken:Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJncmFuZGhhcHB5IiwianRpIjoiMiIsImF1dGhvcml0aWVzIjoiW3tcImF1dGhvcml0eVwiOlwiY29tbW9uXCJ9XSIsImV4cCI6MTU1NzMyNzI4OH0.Ekf5MmXJA9EqKd6gVTbUbtlcLZ27qvcCU9Ph01Zuhx36BK-cCabj9krIY_C0yZsWP6J-lz_UT3TV7vMdS3D8sA'
      
     needauth
   4. curl -X GET 'http://127.0.0.1/security/needauth' -H 'accessToken:Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJncmFuZGhhcHB5IiwianRpIjoiMiIsImF1dGhvcml0aWVzIjoiW3tcImF1dGhvcml0eVwiOlwiY29tbW9uXCJ9XSIsImV4cCI6MTU1NzMyNzI4OH0.Ekf5MmXJA9EqKd6gVTbUbtlcLZ27qvcCU9Ph01Zuhx36BK-cCabj9krIY_C0yZsWP6J-lz_UT3TV7vMdS3D8sA'"
   
     {
          "message": "认证失败，无法访问该资源",
          "status": 401
     }
   5. request needauth interface with wrong accessToken using commond "curl -X GET 'http://127.0.0.1/security/needauth' -H 'accessToken:xxx.xxx.xxx'"
      
     {
          "message": "token invalid",
          "status": 401
     }
### step2 permissions
   *Before the second step,we only need token to send the request.Interface permissions are ignored, so this step we need access control.Permissions are controlled according to roles.*
   *How to add permissions control?*
   1. new MyAccessDecisionManager.java is using for loading permissions
   2. new MyAccessDecisionMnaager.java is using for checking permisions by every request
   3. new MyFilterSecurityInterceptor.java is using for intercepting request as a filter
   4. coding '/security/manager' and '/securty/common' mapping interface in SecuticyController.java
   *How to test permissions?*
   1. curl -X POST 'http://127.0.0.1/security/login' -d 'username=grandhappy&password=123456'
   
    {
        "data": "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJncmFuZGhhcHB5IiwianRpIjoiMiIsImF1dGhvcml0aWVzIjoiW3tcImF1dGhvcml0eVwiOlwiY29tbW9uXCJ9XSIsImV4cCI6MTU1NzMyNzI4OH0.Ekf5MmXJA9EqKd6gVTbUbtlcLZ27qvcCU9Ph01Zuhx36BK-cCabj9krIY_C0yZsWP6J-lz_UT3TV7vMdS3D8sA",
        "message": "登录成功",
        "status": 200
    }
   2. curl -X GET 'http://127.0.0.1/security/manager' -H 'accessToken:Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImp0aSI6IjEiLCJhdXRob3JpdGllcyI6Ilt7XCJhdXRob3JpdHlcIjpcImFkbWluXCJ9LHtcImF1dGhvcml0eVwiOlwiY29tbW9uXCJ9XSIsImV4cCI6MTU1NzM5NTY4Nn0.VHAQZEkrJKcDAmIRM0-iEb8O5nKDmKy6eXAC7lvTlV99w8h5MaJfa3cV_nIRISkYMqonHfrFtDhV8SdWkH4Zng'
      
     {
        "message":"权限不足，无法访问该资源",
        "status":402
      }
   3. curl -X POST 'http://127.0.0.1/security/login' -d 'username=admin&password=123456'
          
     {
        "data": "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJncmFuZGhhcHB5IiwianRpIjoiMiIsImF1dGhvcml0aWVzIjoiW3tcImF1dGhvcml0eVwiOlwiY29tbW9uXCJ9XSIsImV4cCI6MTU1NzMyNzI4OH0.Ekf5MmXJA9EqKd6gVTbUbtlcLZ27qvcCU9Ph01Zuhx36BK-cCabj9krIY_C0yZsWP6J-lz_UT3TV7vMdS3D8sA",
        "message": "登录成功",
        "status": 200
     }
   4. curl -X GET 'http://127.0.0.1/security/manager' -H 'accessToken:Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImp0aSI6IjEiLCJhdXRob3JpdGllcyI6Ilt7XCJhdXRob3JpdHlcIjpcImFkbWluXCJ9LHtcImF1dGhvcml0eVwiOlwiY29tbW9uXCJ9XSIsImV4cCI6MTU1NzM5NTY4Nn0.VHAQZEkrJKcDAmIRM0-iEb8O5nKDmKy6eXAC7lvTlV99w8h5MaJfa3cV_nIRISkYMqonHfrFtDhV8SdWkH4Zng'
      
     security manager
### step3 bugs
   *Before the third step,request '/security/needauth' interface do not required any permissions.*   
   *How to solve this bugs???*

      Latter to resolve

## swagger2
   *Swagger is a platform for API Design and Documentation with OpenAPI*
   *How to integrate swagger?*
   1. import sspringfox-swagger2 and springfox-swagger-ui in pom file
   2. new Swagger2Config.java is using for configing swagger
   3. new SwaggerController.java using for defining swagger interface
   4. add swagger-mappings without jwt and permissions checking in application.properties
   
   *How to test swagger?*
   
   entry http://localhost/swagger-ui.html in your brower
          
## exception
   *exception handling returns the predefined exception in a uniform handling format*
   *How to integrate exception handling?*
   1. new MyExceptionAdvice.java is using for intercepting self-define exception and returning 
   2. new sefl-define exceptions,such as ParamErrorException.java,NotExistException.java and so on
   3. new ResultEnum.java for define exception types
   4. new Result.java for packaging return model
   5. new ExceptionController.java using for exception hadnling interface
   6. add '/exception/**' url-mapping without jwt and permissions checking in application.properties
   
   *How to test exception?*
   1. curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 'http://localhost:8080/exception/login?username=runtime&password=123456'
     
     {
        "code": 500,
        "message": "系统异常"
     }
   2. curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 'http://localhost:8080/exception/login?username=param&password=123456'
   
     {
        "code": 400,
        "message": "用户名错误，请输入admin"
     }
   3. curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 'http://localhost:8080/exception/login?username=notexist&password=123456'
      
     {
         "code": 404,
         "message": "用户不存在"
     }
   4. curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 'http://localhost:8080/exception/login?username=admin&password=123456'
      
     {
      "code": 200,
      "message": "登录成功",
      "data": {
              "id": 1,
              "name": "admin",
              "password": "123456",
              "status": 1,
              "roles": null
              }
      }
## devtools
   *devtools is a developer auto-compile tools,when you modify a java or html without compile you program. you do not restart your project.*
   
   *how to import devtools?*
   1. import spring-boot-devtools in  your pom file
   2. config spring.devtools.start.enable param in your application.properties
   
   *how to test?*
   1. curl http://127.0.0.1:8080/index
      
     hello world
   2. modify return result to 'hello new world' in IndexController.java
   3. after 5s, curl http://127.0.0.1/index
     
     hello new world
## lombok
   *Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
    Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.*
    
   *how to import lombok?*
   1. import spring-boot-devtools in  your pom file
   2. create domain object Lombok.java with @Getter,@Setter@Getter without writing function setter() and getter()
   3. create LombokController.java to test
   
   *how to test?*
   1. curl http://127.0.0.1:8080/lombok
   
    {
     "username": "lucy",
     "age": 13
     }
  
   *main points*
   - @Getter and @Setter
   - @NonNull  
   - @ToString
   - @EqualsAndHashCode
   - @Data
   - @Cleanup
   - @Synchronized
   - @SneakyThrows  