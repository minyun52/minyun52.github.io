# 마크다운란?

Status: Not started

마크다운은 텍스트 기반의 마크업언어로 2004년 존그루버가 만들었다.

쉽게 쓰고 읽을 수 있으며 HTML로 변환이 가능하다.

특수기호와 문자를 이용한 매우 간단한 구조의 문법을 사용한다. 그래서 웹에서도 보다 빠르게 컨텐츠를 작성하고 보다 직관적으로 인식할 수 있다.

## 장점

간결하고 별도의 도구없이 작성이 가능하다.

다양한 형태로 변환이 가능하다.

텍스트 형식으로 용량이 작다.

다양한 플랫폼에서 지원한다.

## 단점

표준이 없다. 따라서 도구에 따라 변환 방식이나 생성물이 다르다.

모든 HTML 마크업을 대신하지 못한다.

## 마크다운 문법

### 1. 헤더

```markdown
# 헤더 테스트 h1
## 헤더 테스트2 h2
### 헤더 테스트3 h3
#### 헤더 테스트4 h4
##### 헤더 테스트5 h5
###### 헤더 테스트6 h6
```

결과

# 헤더 테스트 h1

## 헤더 테스트2 h2

### 헤더 테스트3 h3

#### 헤더 테스트4 h4

##### 헤더 테스트5 h5

###### 헤더 테스트6 h6

### 2. 코드블럭

### 2.1 <pre><code> 사용

```markdown
<pre>
<code>
let test = 1;
</code>
</pre>
```

<pre>
<code>
let test = 1;
</code>
</pre>

### 2.2 탭 or 스페이스 4번

```markdown
				<pre>
				<code>
				let test = 1;
				</code>
				</pre>
```

    <pre>
    <code>
    let test = 1;
    </code>
    </pre>

## 3. 인용문

```markdown
> 첫번째 스타일 인용문
>> 두번째 스타일 인용문
>>> 세번째 스타일 인용문
```

> 첫번째 스타일 인용문
> 
> 
> > 두번째 스타일 인용문
> > 
> > 
> > > 세번째 스타일 인용문
> > > 

## 4. 글머리 기호

```markdown
+ 글머리1
	+ 글머리2
		+ 글머리3
			+ 글머리4
```

- 글머리1
    - 글머리2
        - 글머리3
            - 글머리4
    

## 5. 강조

```markdown
*single asterisk*
_single underscore_
**double asterisks**
__double underscores__
~~cancel line~~
```

*single asterisk*

_single underscore_

**double asterisks**

__double underscores__

~~cancel line~~

## 6. 기호표시

사용하고 싶은 기호 앞에 \(백슬래쉬)를 넣어준다.

```markdown
# 헤딩1
\# 헤딩1
```

# 헤딩1

\# 헤딩1

## 7. 수평 라인

```markdown
* * *
***
*****
----
- - -
```

---

---

---

---

---

## 8. 링크

```markdown
-외부 링크
[링크 키워드](링크 주소)
[네이버](https://www.naver.com/)

-자동 링크
<https://www.naver.com/>
```

[네이버](https://www.naver.com/)

<https://www.naver.com/>

## 9. 이미지

```markdown
![이미지 키워드](/이미지경로/파일명)

-스타일 주기
![이미지 키워드](/이미지경로/파일명){: width="400" height="500"}
```

## 10. 표 만들기

```markdown
| name / gender /
| -------- / ------- /
| 홍길동 | 남자 |
| 유관순 | 여자 |
```

| name / gender /
| -------- / ------- /
| 홍길동 | 남자 |
| 유관순 | 여자 |