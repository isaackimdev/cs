# 게시판 작업 순서

### Spring-mybatis Project 개발 순서
1. mybatis : DB, SQL
2. dao : db connection, mybatis request
3. service - model
4. controller
5. view

### Spring Legacy Project
1. Spring Legacy project 프로젝트 파일 생성 (legacy maven web)
2. Maven update project
3. server remove
4. 파셋 version 1.8
5. pom.xml 버전 수정
  - 디펜던시 추가   step18 pom.xml 참고
6. web.xml 부터 작업한다.
  - url-pattern : *.do
7. mvc-config.xml 작업
  - 기존 resolver 삭제
  - 폴더 등록
   > mvc-resources image, script
  - annotation 인식 명령 추가 : context:component-scan
    - board.controller
    - member.controller 

10. project Mybatis 추가하기
  > Mapper 파일 생성하기
    - board-mapping.xml
    - member-mapping.xml
  > mybatis-config.xml 추가

11. src/main/resources
 - db.properties
 - board-mapping.xml
 - member-mapping.xml
 - mybatis-config.xml

  1) 해당 폴더에 mybatis 폴더를 만든다.
  - mpping 2개와
  - config 1개를
  > mybatis 폴더에 옮긴다.

  2) db.properties는 spring        폴더로 옮긴다.

----------------------------
12. 마이바티스 작업을 DAO 보면서 진행한다.

 memberDAO의 JDBC 연결에 필요한 변수 값을,

db.properties 에 값을 [입력/수정] 한다.

13. member-mapping.xml 작업 
MemberDAO 파일을 보고, mapping.xml에 기능에 맞는 sql문으로 수정한다.
  - namespace : 수정
  - parameterType :수정
memberDTO를 타입으로 쓸 예정이며, 원래는 클래스 풀네임으로 줘야 하지만,
후에,
클래스 별명설정으로 변수이름 형태로 parameterType 값을 준다.

 -insert
 -update
 -select

ResultMap 타입 삭제
delete 삭제

 <필요한 기능 mapping
mybatis에 추가

14. board-mapping.xml 작업
BoarDAO 파일을 보고,
mapping.xml에 기능에 맞는 sql문으로 수정한다.
 - namespace : 수정
 - parameterType : 수정
BoardDTO 클래스 풀네임으로 리절트타입을 줘야 하지만, 클래스 [별명설정]을 함으로,
boardDTO라는 변수 이름으로 준다.
 * crud먼저 작업후 
 - select문 작업
   >>

15. mybatis-config.xml
 *mybatis
	별명짓기
	맵핑작업한 것
	등록하기

  사용하는 것
  - typeAliases
  - mappers

여기까지가 mybatis 설정

-------------------------------
16. application-config.xml
현재 DAO를 쓰기 위해서는
bean객체를 쭉 등록해줘야 한다.

어노테이션이 등록된 것들을 쫙 읽어올 명령어 설정

패키지 등록

<context:component-scan....
member.*
board.*

db.properties 등록
- dataSource 등록
db. 앞에 변수값 복사+붙여넣기
로 property 변수 세팅한다.
 >> DataSource 설정

이걸 가지고 SqlSession을 만들어야 함..

그것들 담고 있는 것이 SqlSessionFactoryBean
설정 파일이기 때문에,

그것을 사용하기 위한 것
SqlSessionTemplate

 -dataSource
 -sqlSessionFactory
 -sqlSession
 -----------------------------

17. memberDAO 수정
JDBC연결값, 세팅 모두 마이바티스쪽으로 들어가 있기 때문에
DAO에서의 변수 세팅 값, 설정값 모두 지운다.

각 기능 함수에도 SQL 기능을 mybatis-mapping을 통해 사용할 수 있기 때문에, sqlSession을 이용하여 마이바티스 맵핑값을 참조하여 사용한다.

18. boardDAO 수정
JDBC연결값 세팅값 모두 mybatis쪽으로 세팅되었기 때문에 DAO에서는 기존의 변수값, 세팅/설정값을 지운다.

각 기능 함수에도 SQL기능을 mybatis를 통해 사용할 것이기 때문에 기존내용을 지우고,
sqlSession을 이용해 mybatis-mapping 값을 참고하여 사용한다.


-------------------------
Controller 패키지 폴더 만든 후 작업 : member.controller

19. [member]
Service 작업
 - Dao 목록

@Service
ServiceImpl 작업
   @Autowired
 - Dao 오버라이드

[member]
MemberService 인터페이스
MemberServiceImpl 클래스
DAO 함수 이름을 그대로 사용

@Controller
MemberController 작업
- 각 기능별 Controller 작업
return [vew].jsp;
return ModelAndView
 > 데이터 공유 및 페이지 이동

참고 함수
HttpSession session = request.getSession(); 을 통해 세션 얻기 
-----------------------------
Controller 패키지 폴더 만든 후 작업 : board.controller
 
20. [board]
service 작업
 - Dao 목록

@Service
ServiceImpl 작업
   @Autowired
 - Dao 오버라이드

@Controller
BoardController 작업
- 각 기능별 Controller 작업
return [vew].jsp;
return ModelAndView
 > 데이터 공유 및 페이지 이동