### springboot 설정파일

springboot에서도 설정에 대한 내용을 다양한 파일에 적고 읽어와 사용하는데 그중에서도 `yaml`을 이용한 설정파일이 `application.yml`이다.

- `profile`을 이용해서 여러 개의 설정을 적어두고 골라서 사용할 수 있다.

  ```yaml
  spring:
  	application:
  		name:
      profiles:
      	activate: default
      data:
      	mongodb:
      		host:
  ---
  spring:
  	profiles: default
  server:
  	address:
  
  ---
  spring:
  	profiles: local
  	data:
  		mongodb:
  			host:
  ```

  

그런데 `profiles`의 경우 springboot 2.4.0 이후 deprecated 되고 `spring.config.activate.on-profile`로 변경되었다고 한다.

```yaml
spring:
  config:
    activate:
      on-profile: local
      
---

spring:
  config:
    activate:
      on-profile: prod
      
---

spring:
  config:
    activate:
      on-profile: prod    
```





---

### 참고자료

- https://velog.io/@tenacity/Spring-Boot-%ED%95%98%EB%82%98%EC%9D%98-application.yml-%EC%97%90%EC%84%9C-%EC%97%AC%EB%9F%AC-Profile%EC%9D%84-%EC%A0%95%EC%9D%98%ED%95%B4%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95