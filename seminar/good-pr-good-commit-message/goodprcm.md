# 좋은 커밋 메세지 & 좋은 풀 리퀘스트

# 좋은 커밋 메세지

## 커밋 메세지란?

모든 소프트웨어 프로젝트는 협동 프로젝트입니다.

나 혼자 개발을 하더라도 몇달 후 내가 짠 코드가 무엇을 하는지 알아내야하는 경우가 있죠.

미래의 내가 새로운 기능을 추가하거나 버그가 발생하여 고쳐야할때 코드이 맥락을 다시 파악해야 합니다.

**코드조각을 다시 파악하는 일 -> 낭비!**

이 시간을 최소화할 수 있도록 노력해야 합니다!

***커밋메시지*** 가 바로 이 역할을 합니다!

## 커밋메시지를 보면 좋은 개발지인지 아닌지 알 수 있다?

```
6개월 전 나의 커밋
"옛날꺼로 되돌림" +986 addition -975 deletion
```
"이게 뭔소리이야 뭔 짓을 한거야"

한눈에 봐서는 무엇을 어떻게 고쳤는지 알 수 가 없습니다.

코드 코멘트나 추적할 수 없는 히스토리가 없다면 앞이 깜깜할 겁니다. 

더구나 어떤 라인을 지우면 전체 프로덕트가 망가져버리지 않을까하는 걱정까지 생깁니다.

## 좋은 커밋 메세지를 작성하는 목적

1. 리뷰 프로세스를 빠르게 만듭니다.
2. 좋은 릴리즈 노트를 만들 수 있습니다.
3. 미래의 코드 메인테이너(유지보수관리자)를 도울 수 있습니다. (심지어 그게 나 자신일수도 있습니다!)

## 좋은 커밋 메세지!

다음의 세가지 질문에 답할 수 있어야 합니다.

#### 1. 왜 이코드가 필요한가요?
    - 버그수정
    - 기능 추가
    - 성능 향상
    - 신뢰성, 안정성을 위한 변경
    - 단순한 오탈자 교정

#### 2. 어떻게 이슈를 해결했나요?
    - 짧아서 명백한 부분은 생략가능
    - 깊은 수준 묘사로 어떻게 문제에 접근했는지 표기

#### 3. 패치가 어떤 영향을 미치나요?
    - 명백한 부분을 포함한 벤치마크 결과
    - 부작용 등

### 위 세가지 질문으로 알 수 있는 점

1. 코드 변경에 대한 맥락을 파악할 수 있습니다.
2. 리뷰어와 다른 개발자가 맥락을 파악하고 적절한 방법을 선택했는지 알 수 있습니다.
3. 유지보수 관리자가 배포를 결정할 수 있도록 도와줍니다.

### 세가지 질문에 답이 없는 메시지라면?
 - 페치가 어떤일을 했고, 어떻게 이슈를 해결했는지 리뷰어가 직접 찾아야하기 때문에 부담만 가중됩니다.

 - 때문에 많은 인력(리뷰어)가 이를 파악하기 위해 나서야 합니다.

 - 좋은 커밋 메시지를 작성하지 않았다는 이유 하나로 수많은 인력이 낭비가 됩니다.

 - 메인테이너가 관리 통제를 철저히 한다면 당연히 반려될 것이고, 개발자는 다시 시간을 소비해 패치를 작성해야합니다. (결과적으로 일을 두번 하게됩니다.)

좋은 커밋 메세지 하나가 이와 같은 문제들을 해결 할 수 있습니다! 

## 어떻게 해야 좋은 커밋 메세지를 작성 할 수 있을까요?

단 한가지의 이상적인 방법은 없지만, 일반적인 규칙이 있습니다.

1. 커밋은 정확히 하나의 변경사항만 포함합니다.
2. 로직변경이란, 기능을 추가하거나 버그를 수정함을 의미합니다.
3. 몇개의 단어로 변화를 표현할 수 없다면 단일 커밋만으로 부족한 상태라는 뜻입니다.
4. 변경은 가능한 스스로 이해할 수 있도록 간결해야 합니다.
5. 무엇보다 커밋 메시지만 읽어도, 다른 개발자가 비슷한 시간을 들여 같은 패치를 구현할 수 있어야 합니다.
6. git을 사용한다면 `git add -p` `git add -i`를 활용해 각각의 변경사항에 따라 단일 커밋 단위로 쪼개야 합니다.

## Git 커밋 양식

1. 첫 행은 변경에 대한 요약 (행의 최대 길이는 행 당 50자에서 70자)
2. 첫 줄을 가장 많이보고 가장 중요함
3. 첫 행 요약 다음 빈 행 입력, 그 다음 필요에 따라 상세 내역을 여러 문단으로 작성
4. 코드 설명이 아닌 **의도와 접근 방식**을 설명
5. 로그는 현재형으로 작성

## 커밋할 때 하면 안되는 것

1. 소스컨트롤 관리는 백업시스템이 아닙니다!
2. 파일 당 커밋
3. 게으른 커밋 메세지
4. 

### 커밋

- 현재 진행중인 프로젝트의 스냅샷
- 커밋 하나는 해당 프로젝트의 독립적인 버전
- 커밋마다 