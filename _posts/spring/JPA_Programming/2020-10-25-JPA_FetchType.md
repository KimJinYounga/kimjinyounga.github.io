---
title: "[JPA] 즉시로딩과 지연로딩 그리고 프록시.. "
header:
  overlay_color: "#333"
categories:
- Server
tag: 
- JPA
- FetchType
- proxy
- spring
toc: true
toc_sticky: true
comments: true
---

# 지연로딩 FetchType.LAZY

```java
@Entity
@Table(name = "MEMBER_TB")
@Getter
@Setter
public class Member {
    @Id
    private String id;
 
    private String name;
 
    //== 지연로딩 로직 ==//
    @ManyToOne(fetch = FetchType.LAZY)
    private Team team;
}
 
@Entity
@Table(name = "TEAM_TB")
@Getter
@Setter
public class Team {
    @Id
    private String id;
 
    private String name;
 
    @OneToMany(mappedBy="team")
    private List<Member> members = new ArrayList<Member>();
}
```

- 지연로딩 전략 
    - fetch=FatchType.LAZY
    - 회원엔티티를 조회할 때 팀 엔티티를 즉시로딩하지 않고 지연로딩할 것.
    - 회원엔티티를 조회할 때 팀회원을 같이 데이터베이스에서 가져오지 않고, Member.getTeam() 으로 실제로 팀엔티티가 사용될 때 데베에서 해당 엔티티를 조회함.  
     

```java
psvm{
    // (1)
    Member findMember = em2.find(Member.class, "member_1");
    // (2)
    System.out.println(findMember.getTeam().getName());
}
```
- (1) 회원엔티티 조회 -> team필드엔 id=null, name=null이 들어감.  
   즉, 데베에서 조회해오지 않는 것. 
- (2) getTeam().getName() 이후 실제 팀을 조회하는 sql이 생김.  
  즉, 회원엔티티를 조회할 땐 팀엔티티를 가져오지 않고 진짜 팀엔티티가 사용될 시점에 데이터베이스에서 팀엔티티를 조회하는 것.  
  
 
**지연로딩의 핵심 프록시**   
- 프록시객체가 team필드 참조하고 있는 것.  
- JPA는 지연로딩에 프록시(대행자)전략을 이용함.  
- 팀회원을 조회할 때 team에는 실제 팀엔티티가 들어가는 것이 아니고 프록시 객체가 들어가는 것이고, 
실제 팀엔티티를 사용할때 이 프록시객체가 대행자역할을 하여 팀엔티티를 바라보고 대신 팀엔티티관련 데이터를 리턴해주는 것.  
- 하지만 만약 Team 엔티티가 이미 영속성 컨텍스트에 들어가있다면 지연로딩 설정을 해도 즉시로딩과 동일하게 영속성 컨텍스트에서 team엔티티를 가져온다.  
- 프록시 : 실제 엔티티 객체 대신에 사용되는 객체로서, 실제 엔티티 클래스와 상속 관계 및 위임 관계.  
    프록시 객체는 실제 엔티티 클래스를 상속 받아서 만들어지므로 실제 엔티티와 겉모습이 같아.  
    
    

<figure>
	<a href="/assets/images/JPA/jpa_fetch_proxy.jpg"><img src="/assets/images/JPA/jpa_fetch_proxy.jpg"></a>
	<figcaption><a href="/assets/images/JPA/jpa_fetch_proxy.jpg" title=""></a></figcaption>
</figure>
  
- 프록시 초기화
    - 프록시 객체가 참조하는 실제 엔티티가 persistence context X ->
        persistence context에 실제 엔티티 요청 -> 생성된 실제 엔티티를 프록시 객체의 참조 변쉥 할당하는 과정  
    - persistence context엔 엔티티가 없음(왜냐? 프록시:지연로딩)
    getName() -> 프록시 객체엔 실제 참조변수(target)이 있지만 영속성 컨텍스트엔 Team객체가 없기에 null을 가리킴.   
    - getName()실행을 위해선 엔티티 정보가 필요 -> db 접근 -> 엔티티 조회  
    - 영속성 컨텍스트에 객체 저장 -> 프록시가 해당 엔티티를 참조.  

그렇다면 언제 어떤 방식을 사용해야 할까? 즉시로딩? 지연로딩?   
{: .notice--info}

참고자료 : https://coding-start.tistory.com/82?category=781616