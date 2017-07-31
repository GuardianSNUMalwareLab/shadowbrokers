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
	* x86/x64를 판단하여 원하는 작업을 실행

* 흐름
	* ecx = 0
	* 0x41을 binary에 끼워넣는다. 0x41은 x86에서는 inc ecx, x64에서는 rex prefix를 의미한다. inc ecx는 ecx값을 1 증가시키고, rex는 아무 작업도 하지 않는다.
	* ecx값을 1 감소시키고, 0이 아니면 x64_payload_start로 점프한다.(즉, x64인 경우 ecx=-1일 때 점프)
	* x86이면 ret한다.(아무 작업도 실행하지 않음)

## x64_payload_start
* 목적
	* x64일 때 payload를 시작한다.

* 흐름
	* 어셈블러에 64bit 프로세서임을 알린다.
	* SYSCALL_OVERWRITE가 정의되어 있으면 아래 2개의 label을 진행한다.
	* 상수 BITS를 64로 정의

## x64_syscall_overwrite
* 목적
	* handler의 주소값을 LSTAR MSR에 저장

* 흐름
	* IA32_LSTAR MSR의 주소를 ecx에 넣는다. IA32_LSTAR MSR은 syscall이 호출될 때 다음에 실행될 instruction 값을 가지고 있다.
	* IA32_LSTAR MSR 값을 읽는다.
	* rdmsr (LSTAR를 읽음) 읽은 값은 edx에 상위 4바이트가 eax에 하위 4바이트가 저장됨
	* rbx에 절대값 0xffffffffffd00ff8을 대입
	* rbx의 4바이트 뒤에 edx 값을 저장하고, rbx 주소에 eax의 값을 저장
	* x64_syscall_handler의 주소값을 rax에 읽고, rdx에 복사한 뒤, rdx를 4바이트 오른쪽으로 시프트 연산 수행
	* 위 과정을 통해 x64_syscall_handler의 상위 4바이트 주소값은 edx에, 하위 4바이트 주소값은 eax에 저장됨
	* wrmsr 명령어를 통해 LSTAR MSG에 기록하고 ret를 통해 반환

## x64_syscall_handler
* 목적
	* SYSCALL이 호출되었을 때의 처리
* 흐름
	* swapgs를 호출하여 msr 레지스터의 0xC0000102 (IA32_KERNEL_GS_BASE)의 값을 gs 레지스터와 바꿈

# 4. 참고

## 디렉티브 (directives)
* 어셈블러에게 변수의 데이터 형이 무엇인지 알려주거나 매크로를 만들 때 사용
* 소스코드의 흐름을 조절하는 명령어
