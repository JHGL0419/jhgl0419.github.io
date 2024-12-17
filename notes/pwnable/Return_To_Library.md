# Return To Library

## 배경

**NX(No eXecute)** 로 인해 **stack(buffer)** 에 **shell code**를 주입 시켜 실행시킬 수 없게 되었다.
그래서 공격자들은 코드 영역에 존재하는 주소들로 **return address**를 덮는 방식을 고안했다.

## 공격에 사용되는 영역

공격에 사용되는 영역은 다음 두 가지가 있다.
- 바이너리의 코드 영역
- 라이브러리의 코드 영역

## Return To PLT

**PLT(Procedure Linkage Table)** 는 라이브러리를 동적으로 링크할 때, 라이브러리의 함수를 호출할 수 있도록 연결해주는 것이다.
만약 라이브러리의 함수를 처음 호출하면 PLT는 GOT의 주소를 가리키고 있다.
GOT에는 주소를 업데이트하는 resolve함수가 들어있는데, 주소를 가져와서 GOT에 넣어둔다.
이후 PLT 호출부터는 GOT에 함수의 진짜 주소가 들어있어 이를 실행한다.
> 참고: [PLT (Procedure Linkage Table), GOT(Global Offset Table)](https://plummmm.tistory.com/382)

만약 공격을 하고자 한다면, 함수의 PLT 주소를 호출하면 된다.
> 같은 라이브러리 안 함수들의 offset은 항상 똑같기 때문에 이를 알아내고자 한다면, GOT안의 주소를 읽어야 한다.
> `write(1, read_got, 8)`
