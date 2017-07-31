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
	* 아키텍처를 판별하고 64비트이면 계속 수행

* 흐름
	* ecx를 0으로 초기화
	* 0x41을 넣음. (이는 x86 아키텍처에서는 ecx를 1 증가시키는 명령어이고, x86_64에서는 rex prefix에 해당)
	* loop x64_payload_start를 통해서 아키텍처 확인. 이 명령어는 ecx를 1 감소시킨 뒤, 0이 아니면 뒤의 label로 점프한다. 만약 아키텍처가 x86이면 0이 되므로 점프하지 않으나 x86_64이면 x64_payload_start로 점프하게 된다.
	* 32비트인 경우, ret 명령어를 통해 반환된다. 즉, 쉘코드가 중지된다.
	* 64비트인 경우, x64_payload_start를 수행한다.

## x64_payload_start
* 목적
	* 상수 BITS를 64로 정의하고 그 다음 수행

* 흐름
	* 상수 BITS를 64로 정의

## x64_syscall_overwrite
* 목적
	* handler의 주소값을 LSTAR MSR에 저장

* 흐름
	* ecx에 0xc0000082를 대입 (이는 LSTAR MSR에 해당)
	* rdmsr (LSTAR를 읽음) 읽은 값은 edx에 상위 4바이트가 eax에 하위 4바이트가 저장됨
	* rbx에 절대값 0xffffffffffd00ff8을 대입
	* rbx의 4바이트 뒤에 edx 값을 저장하고, rbx 주소에 eax의 값을 저장
	* x64_syscall_handler의 주소값을 rax에 읽고, rdx에 복사한 뒤, rdx를 4바이트 오른쪽으로 시프트 연산 수행
	* 위 과정을 통해 x64_syscall_handler의 상위 4바이트 주소값은 edx에, 하위 4바이트 주소값은 eax에 저장됨
	* wrmsr 명령어를 통해 LSTAR MSG에 기록하고 ret를 통해 반환

## x64_syscall_handler
* 목적
	*

* 흐름
	* swapgs를 호출하여 msr 레지스터의 0xC0000102 (IA32_KERNEL_GS_BASE)의 값을 gs 레지스터와 바꿈

# 4. 참고

## 디렉티브 (directives)
* 어셈블러에게 변수의 데이터 형이 무엇인지 알려주거나 매크로를 만들 때 사용
* 소스코드의 흐름을 조절하는 명령어
