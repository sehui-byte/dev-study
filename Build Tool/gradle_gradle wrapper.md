gradle에는 wrapper라고 하는 운영체제에 맞춰서 gradle build를 수행하도록 하는 batch script가 있다.

### gradle wrapper

**gradle 버전이 서로 다른 개발환경에서 기본으로 설치된 gradle 버전과 상관없이 해당 프로젝트에 종속된 gradle버전으로 사용할 수 있도록 해주는 것.**

![image-20210607095731967](C:\Users\nextree\AppData\Roaming\Typora\typora-user-images\image-20210607095731967.png)



#### gradle-wrapper.properties 파일

```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-6.8-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists

```

gradle wrapper는 `GRADLE_USER_HOME`에 찾는 gradle이 있는지 확인을 하고, 만약 존재하면 그 gradle을 사용받고 없다면 wrapper는 해당 파일을 다운받는다.



다운받아지는 gradle은 

```
C:\Users\[username\].gradle\wrapper\dists
```

위 경로에 받아진다.

![image-20210607100215105](C:\Users\nextree\AppData\Roaming\Typora\typora-user-images\image-20210607100215105.png)

![image-20210607100759865](C:\Users\nextree\AppData\Roaming\Typora\typora-user-images\image-20210607100759865.png)





---



### 참고자료

- https://oya150.tistory.com/entry/gradle-wrapper-%EB%8A%94-%EC%99%9C-%ED%95%84%EC%9A%94-%ED%95%98%EB%82%98

- https://darkstart.tistory.com/203

- [gradle wrapper설정하기](https://java.ihoney.pe.kr/336)

- [gradle 공식문서](https://docs.gradle.org/current/userguide/gradle_wrapper.html)