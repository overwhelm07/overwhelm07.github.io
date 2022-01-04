---
title: "[Markdown] Syntax"
excerpt: "Markdown으로 블로그를 작성하기 위해 Markdown의 기본 문법들을 정리했다. "

categories:
  - Blog

tags:
  - [Blog, jekyll, Markdown]

toc: true
toc_sticky: true
 
last_modified_at: 2022-01-02


date: 2022-01-01

---

Github blog를 set-up하는 방법은 위 jekyll 테마 중 하나인 mmistakes URL에서 상세히 확인할 수 있다. 

`mmistakes 테마 설치`) : <https://mmistakes.github.io/minimal-mistakes/docs>  



# Markdown Syntax
[1단계] 헤더(Header) : 제목, 문단별 제목을 쓰고 싶을 때 글의 구조(개요) 및 큰 틀을 잡을 때 사용한다.
## 제목 2단계  
### 제목 3단계
#### 제목 4단계
##### 제목 5단계
###### 제목 6단계 
---

## 목록(List): 요소를 나열할 때 
1. 첫번째
1. 두번째
1. 세번째
  
+ 순서없음
    - 홍길동
      * 중대장
        + 프로실망러

---
## 강조: 문장 내 강조하고 싶은 단어를 눈에 띄게
__볼드(진하게)__  
_이탤릭체(기울여서)_    
~~취소선~~  
<u>밑줄</u>  
__볼드로 진하게 만들다가*이탤릭으로 기울이고*다시 볼드로__(중복 활용도 가능하다.)


---
## 인용구: 인용할 경우 또는 분위기 전환시에도 사용
> 위키백과?
>> 중대장 드립 검색
>>> "오늘 중대장은 너희에게 실망했다"

---
## 링크(Link): 클릭하면 다른 페이지, 다른 부분으로 이동 가능 
유형1(`설명어`를 클릭하면 URL로 이동) : [TheoryDB 블로그](https://theorydb.github.io "마우스를 올려놓으면 말풍선이 나옵니다.")  
유형2(URL 보여주고 `자동연결`) : <https://theorydb.github.io>  
유형3(동일 파일 내 `문단 이동`) : [동일파일 내 문단 이동](#markdown의-반드시-알아야-하는-문법)  


---
## 이미지(Image): 이미지 보여주기 
유형1(`이미지` 삽입) :  
![이미지](https://theorydb.github.io/assets/img/think/2019-06-25-think-future-ai-1.png "인공지능")
  
유형2(`사이즈를 조절`하고 싶은 경우 HTML 태그로 처리) :   
<img src="https://theorydb.github.io/assets/img/think/2019-06-25-think-future-ai-1.png" width="300" height="200"> 

유형3(이미지 삽입 후, `링크 걸기`)
[![이미지](https://theorydb.github.io/assets/img/think/2019-06-25-think-future-ai-1.png)](https://theorydb.github.io/think/2019/06/25/think-future-ai/)


---
## 표(Table): 표 그리기

|                  | 수학                        | 평가              |  
|:--- | ---: | :---: |  
| 철수             | 90            | 참잘했어요. |  
| 영희           | 50            | 분발하세요. |

---
## 수식: 수학, 논문분석 등에 사용
$$ f(x) = if x < x_{min} : (x/x_{min})^a $$  
$$ otherwise : 0 $$  
$$P(w)=U(x/2)(7/5)/Z$$  
$$p_{\theta}(x) = \int p_{\theta}(2z)p_{\theta}(y\mid k)dz$$  
$$x = argmax_k((x_t-x_u+x_v)^T*x_m)/(||x_b-x_k+x_l||)$$  

---
## 코드 블록(Code Block): 소스코드, 외부 인용자료 블록처리 등에 사용 

```python
py_vector = one_hot_encoding("파이",word2index)
py_vector.dot(torch_vector)
>>> 0.0

py_vector = one_hot_encoding("파이",word2index)
py_vector.dot(torch_vector)
>>> 0.0
```

```java
class Test{
  public void main(){
    System.out.println("Hello, wolrd!");
  }
}
```

---
## UML 다이어그램: 순서도, 흐름도 등을 표현할 때 유용하다. 

```mermaid
graph LR
A(입력)-->B[연산]
B-->C(출력)
```

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```
