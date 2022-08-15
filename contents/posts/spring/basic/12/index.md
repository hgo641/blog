---
title: "OXM -  Jaxb2Marshaller"
date: 2022-08-10
tags:
  - spring
series: "moviebuddy"
---

## [미션2] 영화 메타데이터 위치 변경 기능

현재 loadMovies메소드에서 메타데이터의 위치를 지정하고 있고 해당 위치에 있는 메타데이터를 불러읽고 있다.

<br/>

메타데이터 위치를 결정하는 것과 메타데이터를 읽는 행위도 변화의 이유와 시기가 다르기 때문에 분리할 필요가 있다.

<br/>

메타데이터의 위치를 외부에서 입력받게 바꿔보자.



### 1. setter 메소드 활용

* `CsvMovieReader`클래스 내부에 `metadata` 변수를 생성한다.

```java
private String metadata;
```

<br/>

* 우클릭후, `Source` -> `Generate Getter and Setters`

* `metadata`는 필수조건이므로 `setter`를 다음과 같이 수정한다

```java
public void setMetadata(String metadata) {
		this.metadata = Objects.requireNonNull(metadata, "metadata is required value");
	}
```

<br/>

* `loadMovies`에서 url을 가져오는 부분의 코드를

  ```java
  final URI resourceUri = ClassLoader.getSystemResource("movie_metadata.csv").toURI();
  
  // 에서
  
  
  final URI resourceUri = ClassLoader.getSystemResource(getMetadata()).toURI();
  
  //로 바꿔준다.
  ```



### 2. 빈 등록

`MovieBuddyFactory`의 DataSourceModuleConfig 클래스에 다음과 같이 코드를 추가한다.

```java
@Configuration
	static class DataSourceModuleConfig {
		
		@Profile(MovieBuddyProfile.CSV_MODE)
		@Bean
		public CsvMovieReader csvMovieReader() {
			CsvMovieReader movieReader = new CsvMovieReader();
			movieReader.setMetadata("movie_metadata.csv");
			
			return movieReader;
		}
	}
```



## 메타데이터 url이 올바른지 미리 테스트

### 1. `CsvMovieReader`의 setter 메소드에서 검사

```java
public void setMetadata(String metadata) throws FileNotFoundException, URISyntaxException {
		URL metadataURL = ClassLoader.getSystemResource(metadata);
		if (Objects.isNull(metadataURL)) {
			throw new FileNotFoundException(metadata);
		}
		if (Files.isReadable(Path.of(metadataURL.toURI())) == false) {
			throw new ApplicationException(String.format("cannot read to metadata. [%s]", metadata));
		}
		
		this.metadata = Objects.requireNonNull(metadata, "metadata is required value");
		
	}
```

다음과 같이 코드를 작성해서 url이 잘 들어왔는지 (잘못 들어왔다면 Null로 들어온다.)<br/>

해당 url이 읽을 수 있는 url인지 검사하는 코드를 추가한다.





### 2. 빈 생명주기 

1️⃣  `CsvMovieReader`에 `InitializingBean` 인터페이스를 implements해준다.

2️⃣  `InitializingBean`내부에 선언된 afterPropertiesSet 클래스를 오버라이딩하고 1번에서 작성한 setter 안의 코드를 채워넣는다.

```java
@Override
	public void afterPropertiesSet() throws Exception {
		// TODO Auto-generated method stub
		URL metadataURL = ClassLoader.getSystemResource(metadata);
		if (Objects.isNull(metadataURL)) {
			throw new FileNotFoundException(metadata);
		}
		if (Files.isReadable(Path.of(metadataURL.toURI())) == false) {
			throw new ApplicationException(String.format("cannot read to metadata. [%s]", metadata));
		}
		
	}
```
