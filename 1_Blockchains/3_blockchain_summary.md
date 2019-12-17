# **1. 암호화 기법**
* 키 운영방식에 따라, 대칭키 암호화방식과 비대칭키 암호화방식으로 분류
## **1.1. 대칭키 암호화**
* 유일키, 비밀키 = 관용 암호화
* 암호화 키와 복호화 키가 동일
* 단점 : 키 분배 및 관리 문제
## **1.2. 비대칭키 암호화**
* 대칭키 암호화의 단점을 개선하기 위해 개발된 암호화 방식
* 암호화 키와 복호화 키가 상이함
    - ~~유일키~~ → 개인키 + 공개키
    - ~~비밀키~~ → 공개키는 외부에 '공개'

* **기밀키 보장 방법**
    - (송신측) '수신자의 공개키'를 암호화 
    - (수신측) '수신자의 개인키'로 복호화
    
* **인증/원본증명/부인방지 보장 방법**
    - (송신측) '송신자의 개인키'로 암호화
    - (수신측) '송신자의 공개키'로 암호화


# **2. 비트코인**
## **2.1. 탄생 배경**
* 서브프라임 모기지 사태 및 월스트리트 점령 시위
* 무정부주의 및 사이버펑크 운동
* 컴퓨터 공학과 암호학 발전

* 사토시 나카모토의 비트코인 백서 (2018.11.01)
## **2.2. 정의, 개념**
* 중앙기관 없이 네트워크 참여자간 P2P 방식으로 발행, 유통되는 전자화폐
* 2140년까지 2,100만개로 한정된 발행량을 지닌 디플레이션 화폐
* 모든 비트코인 거래 내역은 "블록체인"이라 불리는 공개분산장부에 기록

* 위변조 불가능한 공개분산장부를 통해, 신뢰하지 않은 주체간 안전하고 효율적인 거래가 가능
## **구현 시 고려사항**
* 악의적인 참가자에 의한 부정/위변조
   > * 누구나 참가할 수 있는 네트워크에서는 의도적으로 부정한 거래를 만들거나, 거래 결과를 자신에게 유리하게 변경하는 악의적 참가자가 존재할 수 있다.
   >
   > * 해결책
   >   * 참가자의 본인 인증과 부인 방지 (참가자의 개인키를 이용한 전자서명)
   >   * 악의적인 참가자에 의한 부정 방지 (네트워크 참가자 전원이 모든 거래이력을 기록한 장부를 공유)
   >   * 거래내역 위변조 방지 (거래 내역을 담은 블록을 생성할 때, 이전 블록의 해쉬를 포함)
   >
   >   * 블록 전체에 대한 위변조 방지 (새로운 블록을 만들 때 계산량이 큰 문제를 풀도록 함)

* 정보 전달의 지연으로 인한 불일치
  > * 실행된 거래의 결과가 늦게 전달, 공유되어 '이중 지불'과 같은 불일치 상태가 발생할 수 있다.
  >
  > * 해결책
  >   * 이중 지불(사용) 여부 보증 및 확인 ==> 네트워크 참여자 전원이 모든 거래이력을 기록한 장부를 공유
  >
  >   * P2P네트워크에서 블록체인 분기 문제 해결 ==> 가장 긴 블록체인 채택

* 네트워크를 자율적으로 유지 및 운영하기 위한 전략
  > * 누구나 참가할 수 있는 네트워크에 책임있는 관리자가 없으면, 시스템 품질(복원성, 가용성)을 유지하면서 운영하지 못할 수 있다.

# **3. 블록체인**
# **3.1. 개요**
## **3.1.1. 개념**
* (기술) 공개적으로 열람 가능한 분산원장을 유지하는 백엔드 데이터베이스
* (비즈니스) 중개자 없이도 개인(Peer)간의 거래, 가치, 자산 등을 이동시킬 수 있는 교환 네트워크(exchange network)

* (법) 종전의 신뢰 보증 기관을 대체하는 수단

## **3.1.2. 장점**
### **보안성 향상 (무결성)**
> * 중앙 데이터베이스 한곳에 모든 자료를 저장하는 것보다 데이터 손실에 대한 위험이 낮음
> * 중앙 집중화된 시스템 관리가 필요하지 않으므로, 내부자에 의한 조작 또는 정보유출 위험이 크게 감소
> * 암호화된 데이터와 암호화된 키 값으로만 거래가 이루어지므로, 보안성을 높일 수 있음
> * 새로운 블록은 기존의 블록과 연결되므로, 전체 블록 안의 데이터 변조와 탈취가 불가능
>
> * 각 참여 노드의 분산화로 해킹이 불가능
### **안정성 향상 (가용성)**
> * 일부 시스템에 오류 또는 성능저하가 발생하더라도, 전체 시스템과 네트워크에 영향을 미치지 않음
### **오프라인 대비 거래속도 향상**
> * 거래의 인증, 증명 과정에서 제3자를 배제시키는 실시간 거래가 이루어짐
>
> * 거래 기록의 신뢰성 확보와 동시에 거래의 효율성/속도가 향상됨
### **비용 감소**
> * 업계 전문가 마다 의견이 다름.
> * 중앙 집중화된 시스템이 필요없어, 비용이 줄어듬
>
> * 참여자간 직접 거래로 중개 수수료를 절감

## **3.2.3. 단점**
### **처리 속도**
> * 시간당 거래 처리속도가 제한
### **저장 공간**
>* 모든 거래 기록을 저장해야 하므로, 저장 공간이 점점 증가

# **3.2. 분류**
## **Public Blockchain**
>  * Permissionless
>  * 네트워크 참여에 제한이 없음
>  * 누구나 블록체인 데이터를 읽고, 쓰고, 검증 가능
>  * 예시 : Bitcoin, 암호화폐

  * 단점
    * 돈 세탁, 밀수품 거래 등에 악용 가능
    * 블록체인을 유지하려면 경제적인 '인센티브'가 필요 (ex. 채굴)
## **Private Blockchain**
>  * Permissioned
>  * 단일 조직에서 운영
>  * 읽기/쓰기/합의 과정에 참여할 수 있는 참여자를 미리 지정
>  * 중앙기관 한 곳이 모든 권한을 가지며, 네트워크 참여를 위해서는 해당 중앙기관의 허가가 필요
>  * 예시 : 은행간 지급결게 네트워크

  * 장점
    * 느린 거래 속도와 네트워크 확장성 문제 해소
    * Public Blockchain의 단점 또는 위험성을 보완

## **Consortium Blockcahin**
>  * 여러 조직에서 운영
>  * 여러 기관들의 컨소시움으로 구성
>  * 미리 선정된 노드에 의해서 컨드롤
>  * 미리 선정된 노드들이 권한을 가지며, 노드간 동의가 일어나야 거래가 생성
>  * 예시 : 공급망, 엔터프라이즈 영역

  * 장점
    * 분산형 구조를 유지하면서, 제한된 참여를 통해 보안을 강화
    * 네트워크 확장이 용이, 빠른 거래속도
 
# **3.3. 블록체인 구조 (case 비트코인)**
## **3.3.1. 블록**
  * 유효한 트랜잭션 정보의 묶음
  * 비트코인 블록 하나에 포함된 트랜잭션 개수 : 평균 1,400개
  * 비트코인 블록 하나의 크기 : 평균 1.14MB
  * 블록 높이 : 제너시스 블록(0) 이후, 블록 추가될 때마다 1씩 증가

  * 블록 깊이(=confirm) : 블록(1)이 만들어진 이후, 새로운 블록이 추가될 때마다 1씩 증가
  구성 이름 설명

### **블록 구성**
* **블록헤더(60 byte)**

   > * **버전** : 데이터 구조와 버전
   > * **이전 블록 헤더 해쉬** : 블록의 체인구조에서 ***이전블록(부모블록)*** 에 대한 해쉬 참조 값
   > * **머클 루트** : 해당 블록에 포함된 거래로부터 생성된 ***머클트리의 루트*** 에 대한 해쉬 (블록에 들어있는 모든 거래의 요약본)
   > * **타임스탬프** : 블록의 생성 시간
   > * **난이도 목표** : bit 값으로 블록의 작업증명 알고리즘에 대한 난이도 목표

   > * **논스(Nonce)** : 작업증명 알고리즘에 사용되는 카운터
* **블록바디**

  > * **트랜잭션 카운트** : 블록에 포함된 트랜잭션 개수(1~9 byte)
  > * **코인베이스 트랜잭션** : 블록 생성시 발생되는 비트코인 (해당 블록을 마이닝한 miner의 수입)

  > * **트랜잭션** : 10분간 수집한 트랜잭션 확보

## **3.3.2. 블록 해쉬**
* ***블록의 식별자***
* 블록 헤더 정보를 SAH256 해쉬함수로 계산한 32 byte 길이의 숫자

* (사진)

## **3.3.3. 블록체인**
* 블록이 이어져서 만들어진 블록의 집합체
* 블록으로 이루어진 ***링크드 리스트***

* (그림)

# **3.4. 블록체인 핵심기술 (case. 비트코인)**
## **3.4.1. 트랜잭션**
* 비트코인의 거래내역
* 비트코인의 거래내역을 기록한 ***트랜잭션이 곧 비트코인***이 된다.

* 코인베이스 트랜잭션

  >  * 블록을 채굴한 사람에게 보상금을 지급해주는 트랜잭션
  >  * 이전 출력(지급)이 존재하지 않음
  >  * 100 confirmation 이전에는 사용할 수 없도록 제한

* 일반 트랜잭션
  >  * 코인베이스 트랜잭션을 제외한 모든 트랜잭션
  >     * 비트코인 : 송금
  >     * 이더리움 : 송금 + 계약코드의 실행에 따른 상태 변화

## **3.4.2. UTXO**
> 암호화폐를 저장하는 자료 구조 (자산을 기준으로 소유자를 명시함)
* 미사용 출력 (Unspent Transaction Output)
* 출력(지급)은 됬으나, 아직 당사자가 사용하지 않은 상태로 블록에 흩어져 있는 기록

* 당사자만 쓸 수 있도록 잠금 장치 = P2PKH(Pay-To-Public-Key-Hash)
  > A가 가진 자산 100 중 30을 B에게 송금
  > * (B에게 줄 돈)  30     ----> B의 공개키로 암호화 후, 전송 ----> B의 개인키로만 사용 가능
  >
  > * (주고 남은 돈) 70      ----> A의 공개키로 암호화 ----> A의 개인키로만 사용 가능
* UTXO의 특징
    * 다른 사람으로부터 일정량의 암호화폐를 받을 때 생성
    * 받은 금액 그대로를 UTXO로 저장
    * UTXO 내 일부 금액을 송금할 때는, 새로운 UTXO를 생성하고 기존 UTXO는 파기함

## **3.4.3. 전자서명**
> 공개키 시스템에서 송신자의 신원을 증명하는 방법
* 송신자가 *자신의 개인키*로 암호화한 메시지를, 수신자가 "송신자의 공개키"로 복호화

* 신뢰성, 무결성, 부인방지

## **3.4.4. 해쉬함수**
> *임의 크기* 의 데이터를 *고정 크기* 의 데이터로 변환하는 함수

  * **일방향성 :** H(x)=h를 만족하는 임의의 x를 찾는 것이 불가능
  
  * **충돌회피성 :** H(x) = H(y)를 만족하는 임의의 x와 y를 찾는 것이 불가능 (= 유일성)
* 해쉬함수 종류
  * SHA-256 : 미국 NIST가 개발하여 연방정보처리표준 FIPS 180-4로 표준화환 SHA-2 규격의 일부 (256비트 길이의 해쉬값)
  * RIPEMD-160 : 벨기에에서 개발한 해쉬함수 (최초 RIPEMD의 128비트에서 20바이트로 확장 및 걔량)

  * HASH160 : SHA-256에서 생성한 해쉬값을 RIPEMD-160으로 해싱한 값

## **3.4.5. Nonce**
> 블록 헤더 중 유일하게 ***변경***될 수 있는 부분
  * 0에서 시작해서 작업증명 완료될 때까지 1씩 증가

  * 오버플로우가 발생하면 새로운 트랜잭션을 추가해서 재시도

## **3.4.6. Difficulty (난이도)**
> 네트워크의 모든 노드가 동시에 블록을 만들 수 없게 하는 것
* 출력된 해쉬값이 가지는 0 배열의 개수

* 2,016개 블록 생성시마다 블록 생성 시간을 측정해 난이도를 조절 -----> 10분당 1~2개의 블록이 생성되는 것을 보장


## **3.4.7. 머클 트리**
>*머클루트만으로 트랜잭션의 유효성을 보장*
* 이진 해쉬 트리 구조
* leef 노드를 두개씩 짝지어서 해쉬값을 만들고, 이들을 반복하여 해쉬값을 생성 (leaf 노드개수가 홀수면, 마지막 leaf 노드를 복사하여 짝수로 만듦)

* 규모가 큰 데이터 집합의 완전성을 효율적으로 요약 및 검증하는데 사용

## **3.4.8. 합의(Consensus)**
> 모든 참여자의 원장이 일치하는지 확인하는 메커니즘
* **중앙집중형(신뢰 기반)** 에서의 합의 ==> *서비스 제공자가 원장 관리*
  * 빠른 서비스 제공이 가능
  * 중앙 기관의 의도 또는 악의적인 사용자의 공격을 통해 기록 조작이 가능

* **분산환경(증명 기반)** 에서의 합의
  * 비잔틴 장군 문제 해결을 통해 신뢰도 있는 서비스를 제공하는 것
  * 거래 및 거래 실행 순서에 대한 동의
  * 동일한 원장을 유지하기 위하여 검증 참여자들의 상태를 동기화
  * 악의적인 참여자 노드들은 격리