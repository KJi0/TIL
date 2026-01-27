# Java - XML/Json

# 1. 공공데이터와 XML

## 공공데이터란?

공공기관이 만들어 내는 모든 공적인 정보

각 공공기관이 보유한 데이터를 개방하여 누구나 원하는 기능에 활용 가능

[www.data.go.kr](http://www.data.go.kr) 등 회원가입 후 개별 키를 발급받아 사용

## 데이터의 형태

- 오픈API
- CSV: Comma Separated Value
- XML: 태그를 통한 데이터 형식 정의
- JSON: JavaScript Object Notation을 통해 데이터 형식 정의

![image.png](image.png)

## XML

**Extensible Markup Language**

HTML(마크업 언어)과 달리 필요에 따라 태그를 확장해서 사용 가능

정확한 문법을 지켜야 동작

### 기본 문법

문서의 시작은 <?xml version=”1.0” encoding=”UTF-8”?>

반드시 root element가 존재해야 함

나머지 태그들은 Tree 형태로 구성

시작 태그와 종료 태그는 일치해야 함

시작 태그는 k-v 구조의 속성을 가질 수 있음

속성 값은 “ “ 또는 ‘ ‘로 묶어서 표현

태그는 대소문자를 구별

### valid

xml 태그는 자유롭게 생성하기 때문에 최초 작성자의 의도대로 작성되는지 확인할 필요

→ 문서의 구조와 적절한 요소, 속성들의 개수, 순서들이 잘 지켜졌는가?

→ DTD 또는 Schema를 이용해 문서의 규칙 작성

→ 이를 잘 따른 문서를 “valid 하다”라고 함

# 2. SAX

## XML 파싱

**파싱:** 문서에서 필요한 정보를 얻기 위해 태그를 구별하고 내용을 추출하는 과정

→ 전문적인 parser 활용

**SAX parser:** 문서를 읽으면서 태그의 시작, 종료 등 이벤트 기반으로 처리하는 방식

**DOM parser:** 문서를 다 읽고 난 후 문서 구조 전체를 자료구조에 저장하며 탐색하는 방식

SAX는 빠르고 한번에 처리하기 때문에 다양한 탐색이 어려움

DOM은 다양한 탐색이 가능하지만 느리고 무거우며 큰 문서를 처리하기 어려움

---

## SAX(Simple API for XML) Parser

문서를 읽다가 발생하는 이벤트 기반으로 문서 처리

### DTO(Data Transfer Object)

계층 간 데이터 교환을 위해 사용되는 객체

필요한 데이터만 캡슐화

**작성 방법**

필요한 속성에 대해 private 선언

Getter/setter로 encapsulation

필요에 따라 toString(), hashCode(), equals() 재정의

Defualt Constructor

데이터 변환 로직 (String → Data 등)

영화 정보를 저장하기 위한 클래스 구성: DTO

---

## record 클래스

데이터를 보관하는 데 사용되는 불변 객체를 간단명료하게 정의 가능

불변의 DTO를 구현할 때 유용

### **주요 특징**

불변성: 객체의 상태는 객체 생성 시 정의되며 이후 변경 불가 → 모든 field가 final

간결성: 변수 선언 외에 필요한 코드는 컴파일 시점에 자동 생성

### 제한 사항

이미 묵시적으로 java.lang.Record를 상속받았기 때문에 추가로 다른 클래스를 상속받을 수 없음

다른 클래스가 상속받을 수도 없음

### 생성

```java
package com.ssafy.day11.dto;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;
import java.util.Date;

// TODO: 다음 클래스를 record 클래스로 작성해보세요.
//  Integer rank, String movieNm, String openDt, Integer audiAcc를 관리한다.
 public record BoxOffice(Integer rank, String movieNm, String openDt, Integer audiAcc){}
// END

package com.ssafy.day11.dto;

public class RecordTest {
	public static void main(String[] args) {
		BoxOffice office = new BoxOffice(1, "Test", "2026-01-01", 1);
		String name = office.movieNm();
		System.out.println(office + ", " + name);
		BoxOffice office2 = new BoxOffice(1, "Test", "2026-01-01", 1);
		
		System.out.println(office.equals(office2));
	}
}

/*
BoxOffice[rank=1, movieNm=Test, openDt=2026-01-01, audiAcc=1], Test
true
*/
```

### Handler 작성

```java
package com.ssafy.day11.parser;

import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

import com.ssafy.day11.dto.BoxOffice;

public class BoxOfficeSaxParser extends DefaultHandler implements BoxOfficeParser {
    // TODO: singleton 형태로 작성해보자.
	private static BoxOfficeParser parser = new BoxOfficeSaxParser();
	private BoxOfficeSaxParser() {}
    
	public static BoxOfficeParser getParser() {
		return parser;
	}
	// END

    // 파싱된 내용을 저장할 List
    private List<BoxOffice> list;
    // 지금 처리할 객체의 내용
    int rank;
    String movieNm;
    String openDt;
    int audiAcc;

    // 방금 읽은 텍스트 내용
    private String content;

    @Override
    // TODO: getBoxOffice를 재정의하여 SAXParser를 구성하고 boxoffice.xml을 파싱하시오.
    public List<BoxOffice> getBoxOffice(InputStream resource) throws SAXException, IOException, ParserConfigurationException {
    	list = new ArrayList<>();
    	SAXParserFactory factory = SAXParserFactory.newInstance();
    	SAXParser parser = factory.newSAXParser();
    	parser.parse(resource, this); // 문서 파싱 처리
    	
    	return list;
    }
    // END

    // TODO: 필요한 매서드를 재정의 하여 boxOffice.xml을 파싱하시오.
    @Override
    public void startDocument() throws SAXException {
    	System.out.println("문서 읽음 시작");
    }
    
    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
    	this.content = String.valueOf(ch, start, length);
    }
    
    @Override
    public void endElement(String uri, String localName, String qName) throws SAXException {
    	if (qName.equals("rank")) {
    		this.rank = Integer.parseInt(this.content);
    	} else if (qName.equals("movieNm")) {
    		this.movieNm = this.content;
    	} else if (qName.equals("openDt")) {
    		this.openDt = this.content;
    	} else if (qName.equals("audiAcc")) {
    		this.audiAcc = Integer.parseInt(this.content);
    	} else if (qName.equals("dailyBoxOffice")) {
    		BoxOffice office = new BoxOffice(rank, movieNm, openDt, audiAcc);
    		list.add(office);
    	}
    }
    // END
}
```

### XML 파싱 및 결과 확인

```java
package com.ssafy.day11.client;

import java.io.InputStream;
import java.util.List;
import java.util.Optional;

import com.ssafy.day11.dto.BoxOffice;
import com.ssafy.day11.parser.BoxOfficeDomParser;
import com.ssafy.day11.parser.BoxOfficeJsonParser;
import com.ssafy.day11.parser.BoxOfficeParser;
import com.ssafy.day11.parser.BoxOfficeSaxParser;

public class BoxOfficeCLI {
    private BoxOfficeParser parser = null;
    private InputStream resource = null;

    public static  enum Type{
        SAX, DOM, JSON
    }

    public List<BoxOffice> readBoxOfficeList(Type type) throws Exception {
        String file = null;
        // TODO: resource와 parser를 구성해서 정보를 가져와보자.
        switch(type) {
        	case SAX: {
        		file = "../res/boxoffice.xml";
        		parser = BoxOfficeSaxParser.getParser();
        	}
        }
        resource = BoxOfficeCLI.class.getResourceAsStream(file);
        return parser.getBoxOffice(resource);
        // END
    }

    public static void main(String[] args) {
        BoxOfficeCLI cli = new BoxOfficeCLI();
        try {
            List<BoxOffice> result = cli.readBoxOfficeList(Type.SAX);
            result.forEach(System.out::println);
        } catch (Exception e) {
            System.out.println("오류 발생!: " + e.getMessage());
        }
    }
}

/*
문서 읽음 시작
BoxOffice[rank=1, movieNm=발신제한, openDt=2021-06-23, audiAcc=499902]
BoxOffice[rank=2, movieNm=크루엘라, openDt=2021-05-26, audiAcc=1554649]
BoxOffice[rank=3, movieNm=콰이어트 플레이스 2, openDt=2021-06-16, audiAcc=684512]
BoxOffice[rank=4, movieNm=미드나이트, openDt=2021-06-30, audiAcc=37940]
BoxOffice[rank=5, movieNm=킬러의 보디가드 2, openDt=2021-06-23, audiAcc=299967]
BoxOffice[rank=6, movieNm=인 더 하이츠, openDt=2021-06-30, audiAcc=24916]
BoxOffice[rank=7, movieNm=루카, openDt=2021-06-17, audiAcc=267015]
BoxOffice[rank=8, movieNm=체르노빌 1986, openDt=2021-06-30, audiAcc=9016]
BoxOffice[rank=9, movieNm=컨저링3: 악마가 시켰다, openDt=2021-06-03, audiAcc=775008]
BoxOffice[rank=10, movieNm=괴기맨숀, openDt=2021-06-30, audiAcc=4861]
*/
```

# 3. DOM

문서를 완전히 메모리에 로딩 후 필요한 내용 찾기

**DOM Tree**

문서를 구성하는 모든 요소를 Node(태그, 속성, 값)로 구성

태그들은 root 노드를 시작으로 부모-자식의 관계 구성

```java
package com.ssafy.day11.parser;

import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

import com.ssafy.day11.dto.BoxOffice;

public class BoxOfficeDomParser implements BoxOfficeParser {

    private static BoxOfficeDomParser parser = new BoxOfficeDomParser();

    public static BoxOfficeDomParser getParser() {
        return parser;
    }

    private BoxOfficeDomParser() {
        System.out.println("DOM parser");
    }

    private List<BoxOffice> list;

    @Override
    public List<BoxOffice> getBoxOffice(InputStream resource)
            throws ParserConfigurationException, SAXException, IOException {
        list = new ArrayList<>();
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder builder = factory.newDocumentBuilder();
        // 문서 로딩 완료 --> 원하는 요소들 골라내기
        Document doc = builder.parse(resource);
        // 최 상위 element
        Element root = doc.getDocumentElement();
        System.out.println(root);
        parse(root);
        return list;
    }

    private void parse(Element root) {
        // TODO: root에서 dailyBoxOffice를 추출한 후 BoxOffice를 생성해 list에 저장하시오.
    	NodeList nodes = root.getElementsByTagName("dailyBoxOffice");
    	for (int i = 0; i < nodes.getLength(); i++) {
    		Node child = nodes.item(i);
    		list.add(getBoxOffice(child));
    	}
        // END
    }

    private BoxOffice getBoxOffice(Node node) {
        // TODO: node 정보를 이용해서 BoxOffice를 구성하고 반환하시오.
    	NodeList nodes = node.getChildNodes();
    	int rank = -1;
    	int audiAcc = -1;
    	String movieNm = null;
    	String openDt = null;
    	
    	for (int i = 0; i < nodes.getLength(); i++) {
    		Node child = nodes.item(i);
    		String name = child.getNodeName();
    		String content = child.getTextContent();
    		
    		if (name.equals("rank")) {
    			rank = Integer.parseInt(content);
    		} else if (name.equals("movieNm")) {
    			movieNm = content;
    		} else if (name.equals("openDt")) {
    			openDt = content;
    		} else if (name.equals("audiAcc")) {
    			audiAcc = Integer.parseInt(content);
    		}
    	}
    	return new BoxOffice(rank, movieNm, openDt, audiAcc);
        // END
    }
}
```

```java
package com.ssafy.day11.client;

import java.io.InputStream;
import java.util.List;
import java.util.Optional;

import com.ssafy.day11.dto.BoxOffice;
import com.ssafy.day11.parser.BoxOfficeDomParser;
import com.ssafy.day11.parser.BoxOfficeJsonParser;
import com.ssafy.day11.parser.BoxOfficeParser;
import com.ssafy.day11.parser.BoxOfficeSaxParser;

public class BoxOfficeCLI {
    private BoxOfficeParser parser = null;
    private InputStream resource = null;

    public static  enum Type{
        SAX, DOM, JSON
    }

    public List<BoxOffice> readBoxOfficeList(Type type) throws Exception {
        String file = null;
        // TODO: resource와 parser를 구성해서 정보를 가져와보자.
        switch(type) {
        	case SAX: {
        		file = "../res/boxoffice.xml";
        		parser = BoxOfficeSaxParser.getParser();
        	}
        	case DOM: {
        		file = "../res/boxoffice.xml";
        		parser = BoxOfficeDomParser.getParser();
        	}
        }
        resource = BoxOfficeCLI.class.getResourceAsStream(file);
        return parser.getBoxOffice(resource);
        // END
    }

    public static void main(String[] args) {
        BoxOfficeCLI cli = new BoxOfficeCLI();
        try {
            List<BoxOffice> result = cli.readBoxOfficeList(Type.DOM);
            result.forEach(System.out::println);
        } catch (Exception e) {
            System.out.println("오류 발생!: " + e.getMessage());
        }
    }
}

/*
DOM parser
[boxOfficeResult: null]
BoxOffice[rank=1, movieNm=발신제한, openDt=2021-06-23, audiAcc=499902]
BoxOffice[rank=2, movieNm=크루엘라, openDt=2021-05-26, audiAcc=1554649]
BoxOffice[rank=3, movieNm=콰이어트 플레이스 2, openDt=2021-06-16, audiAcc=684512]
BoxOffice[rank=4, movieNm=미드나이트, openDt=2021-06-30, audiAcc=37940]
BoxOffice[rank=5, movieNm=킬러의 보디가드 2, openDt=2021-06-23, audiAcc=299967]
BoxOffice[rank=6, movieNm=인 더 하이츠, openDt=2021-06-30, audiAcc=24916]
BoxOffice[rank=7, movieNm=루카, openDt=2021-06-17, audiAcc=267015]
BoxOffice[rank=8, movieNm=체르노빌 1986, openDt=2021-06-30, audiAcc=9016]
BoxOffice[rank=9, movieNm=컨저링3: 악마가 시켰다, openDt=2021-06-03, audiAcc=775008]
BoxOffice[rank=10, movieNm=괴기맨숀, openDt=2021-06-30, audiAcc=4861]
*/
```

![image.png](image%201.png)

# 4. JSON

JavaScri[t Object Notation

자바스크립트에서의 객체 표현법

간결한 문법, 단순한 텍스트, 적은 용량으로 대부분의 언어, 대부분의 플랫폼에서 사용 가능

이 기종 간의 데이터 교환에 광범위하게 사용됨

객체를 k-v 쌍으로 관리

```java
package com.ssafy.day11.parser;

import java.io.IOException;
import java.io.InputStream;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import com.fasterxml.jackson.core.JsonParseException;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.ssafy.day11.dto.BoxOffice;

public class BoxOfficeJsonParser implements BoxOfficeParser {

    private static BoxOfficeJsonParser parser = new BoxOfficeJsonParser();

    public static BoxOfficeJsonParser getParser() {
        return parser;
    }

    private BoxOfficeJsonParser() {
        System.out.println("json");
    }

    private List<BoxOffice> list;

    @Override
    public List<BoxOffice> getBoxOffice(InputStream resource)
            throws JsonParseException, IOException {
        // TODO: json을 파싱해서 list를 구성하시오.
    	ObjectMapper mapper = new ObjectMapper();
    	
    	// 기본적으로는 Map 구조
    	// 모든 타입을 아우르기 위해 Object
    	Map<String, Map<String, Object>> result = mapper.readValue(resource, new TypeReference<>() {});
    	
    	// 배열은 List로 처리
    	List<Map<String, Object>> boxList = (List)result.get("boxOfficeResult").get("dailyBoxOfficeList");
    	list = boxList.stream().map(item -> mapper.convertValue(item, BoxOffice.class))
    			.collect(Collectors.toList());
        // END
        return list;
    }
}
```

```java
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public record BoxOffice(Integer rank, String movieNm, String openDt, Integer audiAcc){}
```

```java
package com.ssafy.day11.client;

import java.io.InputStream;
import java.util.List;
import java.util.Optional;

import com.ssafy.day11.dto.BoxOffice;
import com.ssafy.day11.parser.BoxOfficeDomParser;
import com.ssafy.day11.parser.BoxOfficeJsonParser;
import com.ssafy.day11.parser.BoxOfficeParser;
import com.ssafy.day11.parser.BoxOfficeSaxParser;

public class BoxOfficeCLI {
    private BoxOfficeParser parser = null;
    private InputStream resource = null;

    public static  enum Type{
        SAX, DOM, JSON
    }

    public List<BoxOffice> readBoxOfficeList(Type type) throws Exception {
        String file = null;
        // TODO: resource와 parser를 구성해서 정보를 가져와보자.
        switch(type) {
        	case SAX: {
        		file = "../res/boxoffice.xml";
        		parser = BoxOfficeSaxParser.getParser();
        	}
        	case DOM: {
        		file = "../res/boxoffice.xml";
        		parser = BoxOfficeDomParser.getParser();
        	}
        	case JSON: {
        		file = "../res/boxoffice.json";
        		parser = BoxOfficeJsonParser.getParser();
        	}
        }
        resource = BoxOfficeCLI.class.getResourceAsStream(file);
        return parser.getBoxOffice(resource);
        // END
    }

    public static void main(String[] args) {
        BoxOfficeCLI cli = new BoxOfficeCLI();
        try {
            List<BoxOffice> result = cli.readBoxOfficeList(Type.JSON);
            result.forEach(System.out::println);
        } catch (Exception e) {
            System.out.println("오류 발생!: " + e.getMessage());
        }
    }
}

/*
json
BoxOffice[rank=1, movieNm=미션임파서블:고스트프로토콜, openDt=2011-12-15, audiAcc=5328435]
BoxOffice[rank=2, movieNm=마이 웨이, openDt=2011-12-21, audiAcc=1739543]
BoxOffice[rank=3, movieNm=셜록홈즈 : 그림자 게임, openDt=2011-12-21, audiAcc=1442861]
BoxOffice[rank=4, movieNm=퍼펙트 게임, openDt=2011-12-21, audiAcc=895416]
BoxOffice[rank=5, movieNm=프렌즈: 몬스터섬의비밀 , openDt=2011-12-29, audiAcc=202909]
BoxOffice[rank=6, movieNm=라이온 킹, openDt=1994-07-02, audiAcc=171285]
BoxOffice[rank=7, movieNm=오싹한 연애, openDt=2011-12-01, audiAcc=2823060]
BoxOffice[rank=8, movieNm=극장판 포켓몬스터 베스트 위시「비크티니와 백의 영웅 레시라무」, openDt=2011-12-22, audiAcc=285959]
BoxOffice[rank=9, movieNm=앨빈과 슈퍼밴드3, openDt=2011-12-15, audiAcc=516289]
BoxOffice[rank=10, movieNm=극장판 포켓몬스터 베스트 위시 「비크티니와 흑의 영웅 제크로무」, openDt=2011-12-22, audiAcc=235070]
*/
```

- boxoffice.json
    
    ```json
    {
        "boxOfficeResult": {
            "boxofficeType": "일별 박스오피스",
            "showRange": "20120101~20120101",
            "dailyBoxOfficeList": [
                {
                    "rnum": "1",
                    "rank": "1",
                    "rankInten": "0",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20112207",
                    "movieNm": "미션임파서블:고스트프로토콜",
                    "openDt": "2011-12-15",
                    "salesAmt": "2776060500",
                    "salesShare": "36.3",
                    "salesInten": "-415699000",
                    "salesChange": "-13",
                    "salesAcc": "40541108500",
                    "audiCnt": "353274",
                    "audiInten": "-60106",
                    "audiChange": "-14.5",
                    "audiAcc": "5328435",
                    "scrnCnt": "697",
                    "showCnt": "3223"
                },
                {
                    "rnum": "2",
                    "rank": "2",
                    "rankInten": "1",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20110295",
                    "movieNm": "마이 웨이",
                    "openDt": "2011-12-21",
                    "salesAmt": "1189058500",
                    "salesShare": "15.6",
                    "salesInten": "-105894500",
                    "salesChange": "-8.2",
                    "salesAcc": "13002897500",
                    "audiCnt": "153501",
                    "audiInten": "-16465",
                    "audiChange": "-9.7",
                    "audiAcc": "1739543",
                    "scrnCnt": "588",
                    "showCnt": "2321"
                },
                {
                    "rnum": "3",
                    "rank": "3",
                    "rankInten": "-1",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20112621",
                    "movieNm": "셜록홈즈 : 그림자 게임",
                    "openDt": "2011-12-21",
                    "salesAmt": "1176022500",
                    "salesShare": "15.4",
                    "salesInten": "-210328500",
                    "salesChange": "-15.2",
                    "salesAcc": "10678327500",
                    "audiCnt": "153004",
                    "audiInten": "-31283",
                    "audiChange": "-17",
                    "audiAcc": "1442861",
                    "scrnCnt": "360",
                    "showCnt": "1832"
                },
                {
                    "rnum": "4",
                    "rank": "4",
                    "rankInten": "0",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20113260",
                    "movieNm": "퍼펙트 게임",
                    "openDt": "2011-12-21",
                    "salesAmt": "644532000",
                    "salesShare": "8.4",
                    "salesInten": "-75116500",
                    "salesChange": "-10.4",
                    "salesAcc": "6640940000",
                    "audiCnt": "83644",
                    "audiInten": "-12225",
                    "audiChange": "-12.8",
                    "audiAcc": "895416",
                    "scrnCnt": "396",
                    "showCnt": "1364"
                },
                {
                    "rnum": "5",
                    "rank": "5",
                    "rankInten": "0",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20113271",
                    "movieNm": "프렌즈: 몬스터섬의비밀 ",
                    "openDt": "2011-12-29",
                    "salesAmt": "436753500",
                    "salesShare": "5.7",
                    "salesInten": "-89051000",
                    "salesChange": "-16.9",
                    "salesAcc": "1523037000",
                    "audiCnt": "55092",
                    "audiInten": "-15568",
                    "audiChange": "-22",
                    "audiAcc": "202909",
                    "scrnCnt": "290",
                    "showCnt": "838"
                },
                {
                    "rnum": "6",
                    "rank": "6",
                    "rankInten": "1",
                    "rankOldAndNew": "OLD",
                    "movieCd": "19940256",
                    "movieNm": "라이온 킹",
                    "openDt": "1994-07-02",
                    "salesAmt": "507115500",
                    "salesShare": "6.6",
                    "salesInten": "-114593500",
                    "salesChange": "-18.4",
                    "salesAcc": "1841625000",
                    "audiCnt": "45750",
                    "audiInten": "-11699",
                    "audiChange": "-20.4",
                    "audiAcc": "171285",
                    "scrnCnt": "244",
                    "showCnt": "895"
                },
                {
                    "rnum": "7",
                    "rank": "7",
                    "rankInten": "-1",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20113381",
                    "movieNm": "오싹한 연애",
                    "openDt": "2011-12-01",
                    "salesAmt": "344871000",
                    "salesShare": "4.5",
                    "salesInten": "-107005500",
                    "salesChange": "-23.7",
                    "salesAcc": "20634684500",
                    "audiCnt": "45062",
                    "audiInten": "-15926",
                    "audiChange": "-26.1",
                    "audiAcc": "2823060",
                    "scrnCnt": "243",
                    "showCnt": "839"
                },
                {
                    "rnum": "8",
                    "rank": "8",
                    "rankInten": "0",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20112709",
                    "movieNm": "극장판 포켓몬스터 베스트 위시「비크티니와 백의 영웅 레시라무」",
                    "openDt": "2011-12-22",
                    "salesAmt": "167809500",
                    "salesShare": "2.2",
                    "salesInten": "-45900500",
                    "salesChange": "-21.5",
                    "salesAcc": "1897120000",
                    "audiCnt": "24202",
                    "audiInten": "-7756",
                    "audiChange": "-24.3",
                    "audiAcc": "285959",
                    "scrnCnt": "186",
                    "showCnt": "348"
                },
                {
                    "rnum": "9",
                    "rank": "9",
                    "rankInten": "0",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20113311",
                    "movieNm": "앨빈과 슈퍼밴드3",
                    "openDt": "2011-12-15",
                    "salesAmt": "137030000",
                    "salesShare": "1.8",
                    "salesInten": "-35408000",
                    "salesChange": "-20.5",
                    "salesAcc": "3416675000",
                    "audiCnt": "19729",
                    "audiInten": "-6461",
                    "audiChange": "-24.7",
                    "audiAcc": "516289",
                    "scrnCnt": "169",
                    "showCnt": "359"
                },
                {
                    "rnum": "10",
                    "rank": "10",
                    "rankInten": "0",
                    "rankOldAndNew": "OLD",
                    "movieCd": "20112708",
                    "movieNm": "극장판 포켓몬스터 베스트 위시 「비크티니와 흑의 영웅 제크로무」",
                    "openDt": "2011-12-22",
                    "salesAmt": "125535500",
                    "salesShare": "1.6",
                    "salesInten": "-40756000",
                    "salesChange": "-24.5",
                    "salesAcc": "1595695000",
                    "audiCnt": "17817",
                    "audiInten": "-6554",
                    "audiChange": "-26.9",
                    "audiAcc": "235070",
                    "scrnCnt": "175",
                    "showCnt": "291"
                }
            ]
        }
    }
    
    ```