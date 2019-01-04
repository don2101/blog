# Maven Lifecycle

![](https://i1.wp.com/roufid.com/wp-content/uploads/2016/05/eyecatch-maven.png?resize=350%2C200)



## I. Maven Lifecycle

- Maven에서는 clean, build, site 세 가지의 Lifecycle을 제공한다.
- compile, test, package, deploy과정은 build에 속한다.
- Maven은 모든 빌드 단위에 대한 Lifecycle이 예약되어 있어 개발자가 임의로 변경할 수 없다.
- 각 Lifecycle은 순서를 갖는 phase로 구성된다.



### 1. Lifecycle의 phase

#### (1) Clean Lifecycle

- clean

#### (2) Build(Default) Lifecycle

- validate
- compile
- test
- package
- verify
- install
- deploy

#### (3) Site Lifecycle

- site



## II. Phase와 Goal

### 1. Phase

- Phase는 Build Lifecycle 각각의 단계를 의미한다.
- Phase는 특정 순서에 따라 goal이 실행되도록 구조를 제공한다.
- Phase 간에 의존 관계가 있다.
  - 예를 들어 package phase가 수행되기 위해 이전의 phase가 순서되로 수행되어야 한다.



### 2. Goal

- Goal의 Ant의 Target과 비슷한 개념으로 생각할 수 있다.
- Lifecycle에서 실행하는 플러그인이다.
  - Maven의 모든 기능은 플러그인 기반으로 동작한다.



### 3. Phase와 Goal의 관계

- Maven에서 제공하는 모든 기능은 플러그인 기반으로 동작한다.
- Maven에서 기본으로 제공하는 **Phase를 실행**하면 해당 **Phase와 연결된 플러그인의 Goal이 실행**된다.
- 각 Phase는 0개 이상의 goal과 연결되어 있다.
  - ex) package phase는 jar:jar와 연결되어 있다.

#### (1) Plugin goal

- Maven에서 플러그인을 실행할 때 '**플러그인이름:플러그인지원 goal**'의 형식으로 실행할 기능을 선택할 수 있다.
- 예를들어 **mvn** **complier:complie** 은 'complier'플러그인 에서 'complie' 기능(goal)을 실행한다는 것을 의미한다.

#### (2) 각 Phase마다 goal

1. **Phase**					-	**Goal**
2. process-resources		- 	resources:resources
3. compile					-	compiler:compile
4. process-test-resources	-	resources:testResources
5. test-compile				-	compiler:testCompile
6. test						-	surefile:test
7. package					-	jar:jar



## III. Maven default Phase와 Goal

### 1. process-resources

- resources:resources Goal이 실행된다.
- resource directory(/src/main/resources)를 \<outputDirectory>에 생성한다.

### 2. compile

- resources:resources, compile: compile Goal이 실행된다.
- 소스 코드를 컴파일해서 클래스를 \<outputDirectory>에 생성한다.

### 3. test-compile

- compiler:compile, compiler:testCompile Goal이 실행된다.
- 테스트 코드를 컴파일한다.

### 4. test

- compiler:compile, compiler:testCompile, surefire:test Goal이 실행된다.
- junit같은 테스트 코드를 실행하고, 실패하면 빌드를 멈춘다.
- targe/surefire-reports디렉토리 안에 test 리포트 파일을 생성한다.
- 단위 테스트 코드가 실패해도 빌드를 성공시키려면 maven.test.skip 속성을 true로 설정한다.

```xml
<properties>
    <maven.test.skip>true</maven.test.skip>
</properties>
```

### 5. package

- compile, test-compile, test 순으로 실행된 후 jar, war 파일이 target 디렉토리 아래에 생성된다.

### 6. install

- 로컬 리포지토리에 패키지를 배포한다.

### 7.deploy

- 원격 리포지토리에 등록하여, 다른 프로젝트에서 사용할 수 있도록 한다.

### 8.clean

- target 디렉토리의 결과물을 모두 제거한다.



## IV. Reference

[Maven Lifecycle이란](http://wiki.gurubee.net/display/SWDEV/Maven+Lifecycle)

[Maven 기초](http://javacan.tistory.com/129)

