Shellcode 분석
==============

# 1. 개요

# 2. 상수 정의 (Constant)
## 디렉티브 (Directive)
1. BITS
* 용도
	* 대상 프로세서의 모드를 명세해 줌
* 형식
	* BITS XX로 표기되며, XX에는 16, 32, 64가 가능

2. ORG
* 용도
	* 어셈블러의 출력 코드가 실제 로딩되는 주소값을 의미
* 형식
	* ORG <addr> 세그먼트 시작으로부터  <addr>만큼 띠어진 곳에 로드됨

3. DEFAULT REL
* 용도
	* 64비트의 레지스터를 사용하지 않는 명령어가 RIP 인지 아닌지 결정
	* REL이 설정되지 않으면 절대값 사용
* 형식
	* DEFAULT REL

4. SECTION
* 용도
	* 섹션의 정의
* 예시
	* SECTION .text .text라는 섹션을 정의

5. GLOBAL
* 용도
	* 심볼을 다른 모듈에도 알림
* 예시
	* global payload_start

## 매크로 (Macro)
1. 

# 3. 레이블 흐름 (Labels)

## payload_start
* 목적
	*

* 흐름
	*

# 4. 참고

## 디렉티브 (directives)
* 어셈블러에게 변수의 데이터 형이 무엇인지 알려주거나 매크로를 만들 때 사용
* 소스코드의 흐름을 조절하는 명령어
