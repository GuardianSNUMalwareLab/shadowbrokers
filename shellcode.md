Shellcode 분석
==============

# 1. 개요

# 2. 상수 정의 (Constant)

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

## x64_syscall_overwrite
* 목적
	* 

* 흐름
	* IA32_LSTAR MSR의 주소를 ecx에 넣는다. IA32_LSTAR MSR은 syscall이 호출될 때 다음에 실행될 instruction 값을 가지고 있다.
	* IA32_LSTAR MSR 값을 읽는다.

# 4. 참고

1. 디렉티브 (directives)
