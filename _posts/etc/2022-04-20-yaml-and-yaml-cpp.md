---
title: yaml & yaml-cpp
author:
  name: 구름
  link: https://github.com/knabin
date: 2022-04-20 06:21:00 +0900
categories: [Etc.]
tags: [YAML]
math: true
mermaid: true
image:
  src: /common/yaml.png
  width: 600
  height: 300
---

# YAML
## YAML(Yet Another Markup Language)

.yml, .yaml

![1](/posts/220421/1.png)

![2](/posts/220421/2.png)


# 기본 문법

## 들여쓰기 (indent)

2칸(권장) 또는 4칸

```yaml
person:
    name: Nabin Kim
    job: developer
    skills:
        - docker
        - kubernetes
```


## 데이터 정의 (map)

`key: value`

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: echo
    labels:
        type: app
```


## 배열 정의 (array)

`-` 로 표시

```yaml
person:
    name: Nabin Kim
    job: Developer
    skills: 
        - docker
        - kubernetes
```


## 주석 (comment)

`#` 로 표시

```yaml
# comment
name: Nabin Kim # comment
```


## 참/거짓

`true`, `false`, `yes`, `no`

```yaml
study_hard: yes
give_up: no
hello: True
word: TRUE
manual: false
```


## 숫자 표현

```yaml
# number
version: 1.2

# string
version: "1.2"
```


## 줄 바꿈

### 마지막 줄바꿈 포함

마지막 줄바꿈을 포함하는 `|` 지시어

```yaml
newlines_sample: |
    number one line

    second line

    last line
# 이 라인 포함 (number one line\n\nsecond line\n\nlast line\n)
```

### 마지막 줄바꿈 제외

마지막 줄바꿈을 제외하는 `|-` 지시어

```yaml
newlines_sample: |-
    number one line

    second line

    last line
# 이 라인 미포함 (number one line\n\nsecond line\n\nlast line)
```

### 중간 빈줄 제외


중간에 들어간 빈줄을 제외하는 `>` 지시어

```yaml
newlines_sample: >
    number one line

    second line

    last line
# 중간 공백 미포함 (number one line\nsecond line\nlast line\n)
```

# 주의사항

## key와 value의 구분

> key와 value 사이에는 반드시 빈칸이 필요하다.
{: .prompt-danger }


```yaml
# error (not key-value, string)
key:value

# ok!
key: value
```


## 문자열 따옴표

> 대부분의 문자열을 따옴표 없이 사용해도 무방하나, key 값에 :가 들어간 경우에는 반드시 따옴표가 필요하다.
> *, - 등이 앞에 올 때도 따옴표를 사용해야 한다.
{: .prompt-danger }

```yaml
# error
windows_drive: c:

# ok!
windows_drive: "c:"
windows_drive: 'c:'
```


# yaml-cpp

[yaml-cpp](https://github.com/jbeder/yaml-cpp)

A YAML parser and emitter in C++. Contribute to jbeder/yaml-cpp development by creating an account on GitHub.


## Win32 빌드

`cmake -G "Visual Studio 16 2019" -A Win32 ..`



## yaml-cpp in Qt

yaml-cpp의 Qt 3rd party 사용 (`qtyaml.h`)

[yaml-cpp](https://gist.github.com/brcha/d392b2fe5f1e427cc8a6)

Qt Yaml support using yaml-cpp library. 

# 예제
## yaml file

```yaml
start:
    - 1
    - 3
    - 0
end:
    point: 1
    test:
        - 'Hello'
        - 'World'
```

## in cpp
```cpp
try
{
	YAML::Node node = YAML::LoadFile("C:/File/Path/test.yml");

	auto str = node["start"].as<QVector<int>>();
	auto test = node["nothing"];
	auto str2 = node["end"].as<QMap<QString, YAML::Node>>();

	if (!test.IsDefined() || test.IsNull())
	{
		// 내용이 없는 경우는 null, 아예 key 자체를 찾지 못한 경우는 undefined
		qDebug() << "Nothing.";
	}
	else
	{
		qDebug() << (YAML::NodeType::value)test.Type();
	}

	qDebug() << str << str2["point"].as<int>() << str2["test"].as<QVector<QString>>();
} 
catch (YAML::cast_exception &e)
{
	// error 처리
}
```

## output

```console
14:37:57 [DEBUG] Nothing.
14:37:57 [DEBUG] QVector(1, 3, 0) 1 QVector("Hello", "World")
```

<br>

---

# 참고 사이트


[yaml 파일이란 무엇인가요 - 인프런 질문 & 답변](https://www.inflearn.com/questions/16184)

<a href="https://subicura.com/k8s/prepare/yaml.html">YAML 문법 | 쿠버네티스 안내서</a>