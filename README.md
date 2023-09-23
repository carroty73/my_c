도대체 main 함수가 뭐냐?
왜 함수 이름이 main이지? 이걸로 정해놨나? 바꿀수 없나?
main 함수의 표준은 무엇인가?
main 함수의 리턴값은 뭐에다 쓰나?


r1
main 실행시 인자를 받아서 출력하기
<실행파일명>.exe aaa bbb ccc
[1] argv[0]: <실행파일명>.exe
[2] argv[1]: aaa
[3] argv[2]: bbb
[4] argv[3]: ccc

r2
	main 함수의 exit_code는 OS에 돌려준다.
	이값을 쉘에서 받아서 어떤 처리를 해주는 예제

	exit_code값을 여러 다른값으로 줘보자.
	인자로 받아 줄려고 했더니, argv가 -1일때 atoi 함수가 음수 입력을 못받음

	그리고 이 exit_code를 받아서 2면 프로그램을 다 실행하고
	0이면 종료한다.


r3
	쓸데없지만 main 함수 이름 바꿔보기

#include <stdio.h>

// main 함수 이름을 exo로 바꾸었다. 당연히 에러 발생
int exo() {
    printf("Hello, World !!\n");
    printf("My name is David.\n");
    return 0;
}


exo.c 소스에서 main 함수를 빼고 exo 함수로 바꾸면 컴파일시 main 함수가 없다고 당연히 에러가 난다.
그런데 리눅스에서 아래와 같이 컴파일 옵션을 주면 build 및 실행은 가능하다.
gcc -nostartfiles -Wl,--entry=exo -o exo exo.c

그런데, 실행 종료후에 세그먼테이션 폴트 에러가 발생한다.
왜? 아래에 설명이 있다.


▼ C 언어에서 진입점 main()을 변경하는 방법
https://bytes.com/topic/c/answers/798972-how-change-entry-point-main-c-langauge


▼ Why specifying a custom entry point with -etest fail if I replace main() with test()?
https://stackoverflow.com/questions/19008659/why-specifying-a-custom-entry-point-with-etest-fail-if-i-replace-main-with-te?noredirect=1&lq=1

왜냐면 함수 'exo'가 끝에 'retq'명령을 가지고 있기 때문이다.
스택에서 반환 주소를 pop하려고 시도하지만 프로그램이 종료되어 반환 주소가 없으므로 segfault가 발생하기 대문이다.
이를 해결하려면 링크 스크립트를 작성해야 한다네. ^^

visual sutdio에서 해볼려고 했는데
링커 옵션 어딘가 있다던데?
못찾겠다 꾀꼬리

▼ /ENTRY(진입점 기호)
https://docs.microsoft.com/ko-kr/cpp/build/reference/entry-entry-point-symbol?view=msvc-160

▼ Why we always start with main() function? Can I change the name of main()?
https://www.quora.com/Why-we-always-start-with-main-function-Can-I-change-the-name-of-main	
