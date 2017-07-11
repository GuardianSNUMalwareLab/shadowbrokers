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
	* BITS 64 (line 27)
		* 어셈블러에게 프로세서 대상이 64비트임을 알려줌

2. ORG
	* 용도
		* 어셈블러의 출력 코드가 실제 로딩되는 주소값을 의미
	* 형식
		* ORG <addr> 세그먼트 시작으로부터  <addr>만큼 띠어진 곳에 로드됨
	* ORG 0 (line 28)
		* 오프셋이 0임을 표시

3. DEFAULT REL
	* 용도
		* 64비트의 레지스터를 사용하지 않는 명령어가 RIP 인지 아닌지 결정
		* REL이 설정되지 않으면 절대값 사용
	* 형식
		* DEFAULT REL
	* DEFAULT REL
		* RIP 사용한다는 것을 의미

4. SECTION
	* 용도
		* 섹션의 정의
	* 예시
		* SECTION .text .text라는 섹션을 정의

5. GLOBAL
	* 용도
		* 심볼을 다른 모듈에도 알림
	* 예시
		* global payload_start: payload_start라는 심볼이 모든 모듈에게 알려짐

## 매크로 (Macro)
1. PROCESS_HASH SPOOLSV_EXE_HASH (line 35)
	* SPOOLSV_EXE_HASH로 대체

2. MAX_PID (line 36)
	* 최대 PID 값을 0x10000으로 설정

3. WINDOWS_BUILD (line 37)
	* 7601로 설정

4. USE_X86
	* USE_X86이라는 상수 정의
	* 주석에 따르면 그 의미는 x86 페이로드 사용

5. USE_X64
	* USE_X64라는 상수 정의
	* 주석에 따르면 x64 페이로드 사용


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
