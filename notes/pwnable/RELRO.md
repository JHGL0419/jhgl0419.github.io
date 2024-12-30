# RELRO

## 배경
NX로 인해 code 세그먼트에는 실행 권한만 존재하고 쓰기 권한은 존재하지 않고,   
data 세그먼트에는 쓰기 권한만 존재하고 실행 권한이 없다.   

data 세그먼트에는 공격 위험이 있는 두 가지 섹션이 있다.
- .got: GOT overwrite이 가능하다.
- .init_array, .fini_array: 프로세스 초기화 및 종료 시 실행할 함수들의 주소를 저장하는 부분으로,
  공격자가 임의의 값을 쓸 수 있다면 실행 흐름을 조작할 수 있다.
   
이런 문제를 해결하고자 불필요한 data 세그먼트 일부에 쓰기 권한을 제거하는 RELRO(RELocation Read-Only) 를 개발했다.   

RELRO에는 두 가지로 구분된다.
- Partial RELRO: .init_array 와 .fini_array의 쓰기 권한이 제거된다.
- FULL RELRO: .init_array 와 .fini_array 뿐만 아니라 .got에도 쓰기 권한이 제거된다.

FULL RELRO의 경우 .got에 쓰기 권한이 없기 때문에, now binding을 통해 로딩 시점에 모두 바인딩된다.   

.data와 .bss에만 쓰기 권한이 있다.  
