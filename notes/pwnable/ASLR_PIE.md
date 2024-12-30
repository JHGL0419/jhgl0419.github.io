# ASLR & PIE

## ASLR (PIE가 아닌 시점)
**```ASLR(Address Space Layout Ramdomization)```** 은 stack, heap, library의 주소를 랜덤하게 배치하는 것이다.
리눅스의 aslr은 3 단계가 있다.
- 0: 배치가 고정
- 1: **stack, heap**의 주소가 랜덤
- 2: **stack, heap, (Dynamic) library**(=shared object)의 주소가 랜덤

리눅스에서 aslr은 /proc/sys/kernel/randomize_va_space 를 통해 조절한다.   
```shell
// echo 2 > /proc/sys/kernel/randomize_va_space : permission denied
sudo sh -c "echo 2 > /proc/sys/kernel/randomize_va_space"
```
현재 실행 바이너리의 메모리 mapping을 보고 싶다면 **```/proc/self/maps```** 파일을 읽으면 된다.

### 참고
**static library**의 경우 컴파일 타임에 **executable file**과 링크되기 때문에, 주소가 컴파일 시점에 결정되어 aslr의 영향을 받지 않는다.

---
## PIE
기본적으로 **executable file**의 주소는 실행시점마다 바뀌지 않는다.   
**```PIE(Position Independent Executable)```**는 **executable file**의 주소 또한 실행시점마다 바뀌도록 해준다.    
Executable file 또한 shared object 타입(DYN)을 갖도록 하여 구현하기 때문에, aslr이 꺼져있다면 주소가 실행시점마다 바뀌지 않고 고정된다.   
   
**따라서, PIE 그 자체는 보호기법이 아니다.**
