Reverse TCP
===========

# 1. 개요
1. 목적
	*

2. 동작

# 2. 함수 설명
1. generate
	* 목적
		* 
	* 흐름
		* 설정(conf) 변수에 port 번호는 datastore['LPORT']로, host는 datastore['LHOST']로, retry_count는 datastore['ReverseConnectRetries']로 설정하고 reliable은 false로 설정
		* 만약 여분 공간이 있다면, conf의 exitfunk를 datastore['EXITFUNC']로 설정하고 reliable을 true로 변경
		* 설정에 따라 generate_reverse_tcp(conf)를 통해 reverse tcp를 생성

2. generate_reverse_tcp(opts={})
 
