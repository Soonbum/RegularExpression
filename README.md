# 정규표현식의 역사

* UNIX 시스템의 도구에서 처음 사용하게 되었다가 후에 POSIX 표준에 편입되었다. (POSIX 정규표현식)
* POSIX 정규표현식은 둘로 나뉘게 됨
  - POSIX BRE (Basic Regular Expression)
  - POSIX ERE (Extended Regular Expression): grep -e 스위치를 사용하여 확장 정규표현식을 쓸 수 있음
* POSIX BRE로부터 파생된 정규표현식이 있다.
  - vim 정규표현식
  - Perl의 정규표현식: POSIX PCRE (이후의 많은 프로그래밍 언어들이 이 규격에서 파생되었음)

# 정규표현식 패턴 설명

|문자 클래스||
|--|--|
| \ | 특수문자가 아닌 문자 |
| . | 개행을 제외한 모든 문자 |
| \w \d \s | 문자, 숫자, 공백 |
| \W \D \S | 문자 아님, 숫자 아님, 공백 아님 |
| [abc] | a, b, c 중 아무 것 |
| [^abc] | a, b, c가 아닌 아무 것 |
| [a-g] | a와 g 사이의 문자 |

|앵커||
|--|--|
| ^abc$ | 문자열의 시작(^)과 끝($) |
| hi\b hi\B | 경계에 존재하는 단어 hi, 경계에 있지 않은 단어 hi |

|이스케이프 문자||
|--|--|
| \\. \\* \\\ | 이스케이프 특수 문자 (그 외 이스케이프 특수문자: +*?^$\\.[]{}()\|\/) |
| \t \n \r | 탭, 라인피드, 캐리지 리턴 |

|그룹 & 범위||
|--|--|
| (abc) | 그룹 저장 (그룹은 선언한 순서대로 1부터 시작함) |
| (?:abc) | 저장하지 않은 그룹 |
| (?<name>ABC | 그룹 저장 (그룹을 이름으로 참조 가능함) |
| \1 | 그룹 #1에 대한 참조 |
| (?=abc) | 긍정 순방향 탐색 (abc가 뒤에 있음) |
| (?!abc) | 부정 순방향 탐색 (abc가 뒤에 없음) |
| (?<=abc) | 긍정 역방향 탐색 (abc가 앞에 있음) |
| (?<!abc) | 부정 역방향 탐색 (abc가 앞에 없음) |

|수량자||
|--|--|
| a* a+ a? | 0개 이상, 1개 이상, 0 또는 1 |
| a{5} a{2,} | 정확하게 5개, 2개 이상 |
| a{1,3} | 1~3개 사이 |
| a+? a{2,}? | 가능한 적게 일치 |
| ab\|cd | ab 또는 cd와 일치 |

|플래그||
|--|--|
| g (global) | 일치하는 다수의 결과값 (이 옵션이 없으면 처음 나오는 것만 찾음) |
| i (case insensitive) | 대소문자를 구분하지 않음 |
| m (multiline) | 모든 라인을 각각의 문장으로 간주함 (끄면 전체를 한 문장으로 간주함) |
| u (unicode) | 이 플래그를 사용하면 \x{FFFFF} 형태로 유니코드 문자를 사용할 수 있음 |
| s (single line (dotall)) | . 패턴이 가리키는 문자가 개행문자도 포함시키게 함 (개행문자도 임의의 문자로 취급함) |
| y (sticky) ||

# 정규표현식 사용법

* JavaScript의 경우: `/패턴/플래그`
* Python의 경우: re.compile(패턴, flags=re.l)
* Java/Go의 경우: (?플래그)패턴

# 간단한 예제

|패턴|설명|
|--|--|
| `^[0-9]*$` | 숫자 |
| `^[a-zA-Z]*$` | 영문자 (플래그를 써서 `/^[a-z]*$/i` 같이 쓸 수 있음) |
| `^[가-힣]*$` | 현대 한글 (유니코드를 지원하는 정규식 엔진에 한정) |
| `^[ㄱ-ㅎㅏ-ㅣ가-힣]*$` | 한글 자모 낱자를 포함한 모든 현대 한글 (굳이 유니코드 환경에서도 KS X 1001 완성형의 현대 한글 2350자만 선택하고 싶다면 완성형/한글 목록/KS X 1001 문서의 끝부분을 참고할 것) |
| `^[a-zA-Z0-9]*$` | 영문/숫자 |

|패턴|설명|
|--|--|
| `/Hi|Hello/` | Hi 또는 Hello 문자열을 찾음 |
| `/gr(e|a)y/` | Grey 또는 Gray 문자열을 찾음 |
| `/gr(?:e|a)y/` | Grey 또는 Gray 문자열을 찾음 (그룹이 지정되지 않음) |
| `/gr[aed]y/` | gray, grey, grdy 문자열을 찾음 |
| `/gr[a-f]y/` | gray, grby, ..., grfy 문자열을 찾음 |
| `/[a-zA-Z0-9]/` | 영숫자로 구성된 문자열을 찾음 |
| `/[^a-zA-Z0-9]/` | 영숫자가 아닌 문자열을 찾음 |

|패턴|설명|
|--|--|
| `/gra?y/` | gray 또는 gry 문자열을 찾음 |
| `/gra+y/` | gray, graay, graa...ay 문자열을 찾음 |
| `/gra{2}y/` | graay 문자열을 찾음 |
| `/gra{2,3}y/` | graay, graaay 문자열을 찾음 |

|패턴|설명|
|--|--|
| `/\bYa/` | 단어 앞에 나오는 Ya만 찾음 |
| `/Ya\b/` | 단어 뒤에 나오는 Ya만 찾음 |
| `/Ya\B/` | 단어 뒤에 나오지 않는 Ya만 찾음 |
| `/^Ya/` | 문장 앞에 나오는 Ya만 찾음 |
| `/Ya$/` | 문장 뒤에 나오는 Ya만 찾음 |

|패턴|설명|
|--|--|
| `/./` | 모든 문자열을 찾음 (줄바꿈 문자 제외) |
| `/\./` | 도트(.) 문자열을 찾음 (특수 문자를 찾을 경우 \를 앞에 붙일 것) |
| `/\[\]\{\}/` | 각괄호, 중괄호를 찾음 |
| `/\d/` | 숫자를 찾아냄 |
| `/\D/` | 숫자가 아닌 모든 문자를 찾아냄 |
| `/\w/` | 모든 영숫자를 찾아냄 |
| `/\W/` | 영숫자가 아닌 모든 문자를 찾아냄 (특수문자 등) |
| `/\s/` | 공백 문자를 찾아냄 |
| `/\S/` | 공백 문자가 아닌 모든 문자를 찾아냄 |

# 참고할 만한 패턴 예제

|명칭|패턴|
|--|--|
| RFC2822 이메일 검증 | `/[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/g` |
| URL 검증 | `/^(https?:\/\/)?([\da-z\.-]+\.[a-z\.]{2,6}|[\d\.]+)([\/:?=&#]{1}[\da-z\.-]+)*[\/\?]?$/igm` |
| 암호: 최소 8글자, 최소 1개의 대문자와 소문자와 숫자를 포함해야 함, 특수 문자를 포함할 수 있음 | `/^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[a-zA-Z]).{8,}$/gm` |
| 비-ASCII 문자 추출 (공백 포함) | `/[^\x00-\x7F]+\ *(?:[^\x00-\x7F]| )*/g` |
| 유효한 IPv4-주소 (0.0.0.0 - 255.255.255.255) | `/\b(?:(?:2(?:[0-4][0-9]|5[0-5])|[0-1]?[0-9]?[0-9])\.){3}(?:(?:2([0-4][0-9]|5[0-5])|[0-1]?[0-9]?[0-9]))\b/ig` |
| HEX 코드 (# 접두사는 없어도 됨) | `/#?([\da-fA-F]{2})([\da-fA-F]{2})([\da-fA-F]{2})/g` |
| HTML 태그 | `/</?\w+((\s+\w+(\s*=\s*(?:".*?"|'.*?'|[^'">\s]+))?)+\s*|\s*)/?>/g` |
| @태그와 #태그 | `/([@][A-z]+)|([#][A-z]+)/g` |

# 참고자료
  - [RegexOne](http://regexone.com/): step-by-step 방식으로 빠르게 배울 수 있는 사이트.
  - [regex101](https://regex101.com/): 간편하게 정규식을 연습하고 디버깅할 수 있는 사이트. 여러 색상으로 그룹을 표시해주기 때문에 구분이 원활하다. PCRE, 자바스크립트, 파이썬, Go를 지원한다.
  - [RegExr](http://regexr.com/): PCRE, 자바스크립트를 지원한다.
  - [Regexper](http://regexper.com/), [Debuggex](https://www.debuggex.com/), [ExtendsClass](https://extendsclass.com/regex-tester.html): 정규식을 시각화해주는 사이트. 작성한 정규식을 선로도(Railroad Diagram)로 변환하여 보여준다. 남이 만든 정규식이 도저히 읽히질 않으면 여기에 그 코드를 복붙해보자. regexper.com은 정규식을 그림 파일로 다운로드하는 기능을 제공하고 debuggex.com은 실시간 정규식 시각화 및 매치 테스트를 제공한다.
  - [Mozilla JavaScript RegExp Guide](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D): 모질라 재단에서 제공하는 개발자 문서로 자바스크립트의 정규 표현식 문법에 대해서 설명한다.
