---
title: Spring Boot 배운 사항
categories: Spring
tags: spring java springboot
typora-root-url: ../

---

```written by c4fiber```





## DAO를 이용한 DB참조

- 참고: http://yoonbumtae.com/?p=658 



@EnableJpaRepositories 는 @SpringBootApplication 어노테이션 안에 이미 등록되어 있다



하나의 결과가 예상될 경우

- jdbcTemplate: jt
- DataAccessUtils.singleResult를 이용한이유: jt.queryForObject 메소드가 있지만 결과가 0줄일 경우 exception이 발생한다. result가 없으면 null을 반환하도록 해당 메소드 사용함

	public User login(String id, String password){
		
		return DataAccessUtils.singleResult(jt.query("select id, name, password FROM user where id = ? and password = ?", 
				(rs, rowNum) -> new User(rs.getString("id"),rs.getString("name")),
					id, password)
				);
	}

  

여러 결과가 예상될 경우

- 이건 배워야 한다. 차후에 아주 유용하게 사용할 수 있겠다
- 특히나 -> 파트를 잘 알아봐야겠다 이걸 뭐라고 부르는지, 어떻게 이용되는지도 이해가 필요함.

	public List<Map<String, ?>> selectAll() {
		
		return jt.query("select * from `table`", (rs, rowNum) -> {
			Map<String, Object> mss = new HashMap<>();
			mss.put("oid", rs.getInt(1));
			mss.put("number", rs.getInt(2));
			mss.put("places", rs.getInt(3));
			return mss;
		});
	}

  

## Spring Anotation 정리

- 참조: https://jeong-pro.tistory.com/151

@RestController를 사용하지 못한 부분이 아쉽다.

  

@Autowired는 명확한 목적을 모르겠다. spring에서 어노테이션 확인 후 bean으로 자동 매핑하여 생성해준다고 했는데 이에 따른 이득이 명확하게 다가오지 않는다. spring container가 생성하는 객체가 된다는데 음... 더 자세한 이유가 필요하다.

  

## @ComponentScan(basePackages={"path"})

DAO를 MainController와 다른 패키지로 설정했는데 DAO를 못찾는 경우가 발생했다.

@SpringBootApplication 어노테이션이 있는 파일을 basePackage로 설정하고 검색하는 문제가 발생해서다

  

 @ComponentScan 어노테이션을 추가하여 basePackage를 상위 패키지로 지정한다면 해당 패키지의 하위 모든 컴포넌트를 스캔하므로 com.main / com.dao 이렇게 분리된 패키지의 컴포넌트도 사용할 수 있다.

- 특히 @Autowired에 유용함







