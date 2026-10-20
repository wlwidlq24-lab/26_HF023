
## **💡1. 프로젝트 개요**

**1-1. 프로젝트 소개**
- 프로젝트 명 : 음성 녹음 방지 기술 개발
- 프로젝트 번호 : 26_HF023
- 프로젝트 정의 : 등록한 특정 화자의 발화를 실시간으로 판별한 뒤, 필요한 순간에만 40 kHz 초음파 기반 출력을 동작시키는 기술 및 기기 개발
- 목표 : 상시 출력을 유지하는 방식 대신, 음성 구간 감지(VAD), 화자 임베딩 유사도 비교, 이중 임계값, Gray Zone, 연속 확인(Confirm), Hangover 로직을 결합해 불필요한 동작을 줄이는 것
  

**1-2. 개발 배경 및 필요성**
- 회의, 상담, 연구·개발 등 민감한 대화가 이루어지는 공간에서는 무단 녹음과 음성 전사(STT)에 대한 우려가 있습니다. 기존의 상시 출력 방식은 필요하지 않은 상황에서도 계속 동작할 수 있고, 특정 상황에 맞춘 제어가 어렵습니다.

본 프로젝트는 보호가 필요한 화자를 사전에 등록하고, 해당 화자의 발화가 감지된 경우에만 초음파 출력을 활성화하는 구조를 제안합니다. 이를 통해 선택적 동작, 오작동 완화, 향후 소형화·다채널화 가능성을 함께 검토합니다.

**1-3. 프로젝트 특장점**
- 초음파 트랜스듀서 30개(5×6 배열) 기반
- 약 3초 분량의 음성을 5회 등록해 화자 프로필을 생성
- 입력 음성과 등록 프로필의 **cosine similarity** 기반 화자 판정
- Balanced Accuracy를 기준으로 임계값 보정 가능
- ON/OFF 이중 임계값과 **Gray Zone**으로 경계 구간의 불안정한 전환 완화
- 약 0.5초 간격의 2회 연속 Confirm과 Hangover로 오작동·출력 끊김 완화
- Raspberry Pi의 음성 판단과 Arduino Mega의 실시간 출력 제어를 분리한 구조
- 40 kHz AM 출력과 MA40S4S 초음 출력부

**1-4. 주요 기능**
- 화자 등록: 약 3초 음성 5회를 수집해 화자 프로필을 생성합니다.
- 음성 구간 감지: USB 마이크 입력에서 VAD를 통해 유효 음성 구간을 추출합니다.
- 화자 판정: 전처리된 입력 음성의 임베딩과 등록 프로필의 유사도를 비교합니다. 
- 자동 제어: 자동 판정 모드를 지원하도록 설계했습니다.
- 안정화 로직: Gray Zone, 연속 Confirm, Hangover로 상태 전이를 안정화합니다. 
- 초음파 출력 제어: Raspberry Pi의 JAM 명령을 I²C로 Arduino Mega에 전달하고 40 kHz 출력을 제어합니다.
- 출력 레벨 제어: STE(Short-Time Energy)를 활용한 적응형 출력 레벨 제어를 검토합니다.

**1-5. 기대 효과 및 활용 분야**
- 법률 회의실, 상담 공간, 연구실 등 민감한 대화 환경의 **보조 보안 수단**으로 활용 가능성 검토
- 상시 출력 대비 필요한 구간에만 동작하여 불필요한 출력 시간 감소 가능성
- PCB화와 다채널 배열 고도화를 통한 소형화·안정화·확장성 검토

**1-6. 기술 스택**

| 구분 | 사용 기술·부품 | 역할 |
| --- | --- | --- |
| 음성 입력 | USB Microphone, 48 kHz | 음성 수음 |
| Edge AI | Raspberry Pi | HPF, VAD, 리샘플링, 화자 판정 |
| 음성 처리 | Python 3.x, librosa | 오디오 전처리 및 분석 |
| 화자 임베딩 | Resemblyzer 기반 Speaker Encoder | 임베딩 생성 및 유사도 비교 |
| 제어 보드 | Arduino Mega 2560 | I²C 수신, 타이머/PWM 기반 출력 제어 |
| 통신 | I²C | Raspberry Pi–Arduino Mega 간 JAM 명령 전달 |
| 구동부 | JMOD × 5 | 초음파 트랜스듀서 구동 |
| 초음파 출력 | MA40S4S × 30 | 40 kHz AM 기반 출력 |
| 회로·PCB | KiCad | PCB 설계 및 향후 제작 검토 |
| 분석·시각화 | matplotlib | 실험 데이터 시각화 |

---

## **💡2. 팀원 소개(이름 기재X)**


| 구분 | 담당 영역 | 주요 기여 |
| :---: | --- | --- |
| 멘티 1 | 시스템·하드웨어 설계 | Raspberry Pi–Arduino Mega 연동, 출력부 및 배열 구조 설계, 화자 임베딩, VAD,유사도·임계값·상태 전이 로직 구현 |
| 멘티 2 | 3D 모델링 및 하드웨어 구현 | 시제품 3D모델링, 설계를 바탕으로 하드웨어 구현, PCB 설계 |
| 멘티 3 | 실험 설계 및 진행  | 실험 환경 구성, 거리·STT·녹음 조건 검증 및 결과 분석, 실험 진행  |
| 멘티 4 | 논문 및 연구 문서화 | 연구 방법론 및 실험 결과 정리, 논문 작성  |
| 멘토 | 기술 자문 | 시스템 구조, 음성 처리 및 하드웨어 설계 검토 |

---


---
## **💡3. 시스템 구성도**

### 서비스·하드웨어 구성도

~~~mermaid
flowchart LR
    A[USB Microphone<br/>48 kHz audio] --> B
    B[Raspberry Pi<br/>HPF · VAD · 16 kHz resampling] --> C
    P[Registered speaker profile<br/>about 3 sec × 5 recordings] --> C
    C[Speaker embedding<br/>Cosine similarity] --> D
    M[Auto / manual mode] --> D
    D[Decision logic<br/>ON/OFF thresholds · Gray Zone<br/>2× Confirm · Hangover] -->|I²C: JAM_ON / JAM_OFF| E
    E[Arduino Mega 2560<br/>Timer / PWM output control] --> F
    F[JMOD driver modules × 5] --> G[MA40S4S array × 30<br/>5 × 6, 40 kHz AM output]
~~~

### 동작 과정

1. 사용자는 약 3초 길이의 음성을 5회 등록해 화자 프로필을 생성합니다.
2. USB 마이크는 48 kHz로 음성을 수집하고, 하드웨어 HPF(설계 기준 80 Hz 이하 감쇠)와 VAD를 거쳐 음성 구간을 선별합니다.
3. 음성 구간은 16 kHz로 리샘플링되고, 화자 임베딩과 등록 프로필의 cosine similarity를 계산합니다.
4. Balanced Accuracy 기반으로 보정한 ON/OFF 임계값, Gray Zone, 2회 연속 Confirm을 통해 재밍 여부를 결정합니다.
5. 자동 모드에서는 판단 결과를, 수동 모드에서는 사용자 명령을 사용해 Raspberry Pi가 I²C로 JAM 명령을 전송합니다.
6. Arduino Mega는 명령을 수신해 JMOD 구동부와 MA40S4S 배열의 40 kHz AM 출력을 제어합니다.
7. 발화 종료 직후에는 Hangover를 적용해 즉시 OFF로 전환되지 않도록 하며, 설계서상 Fast Start는 약 1.5초·최대 판정 창은 약 3초를 목표로 합니다.

### 상태 전이 개념

~~~mermaid
stateDiagram-v2
    [*] --> Monitoring
    Monitoring --> Candidate: similarity ≥ ON threshold
    Candidate --> JamOn: Confirm count reaches 2
    Candidate --> Monitoring: Gray Zone / insufficient confirmation
    JamOn --> Hold: speech ends or similarity falls
    Hold --> JamOn: target voice resumes during Hangover
    Hold --> Monitoring: Hangover expires and OFF confirmation completes
    Monitoring --> JamOn: manual ON command
    JamOn --> Monitoring: manual OFF command
~~~

---
## **💡4. 작품 소개영상**
>
[![[한이음 공모정 영상]26 HF023: 음성 녹음 방지 기술 개발](https://img.youtube.com/vi/59oVFbw4sKc/maxresdefault.jpg)](https://youtu.be/59oVFbw4sKc)

---
## **💡5. 핵심 소스코드**

### 5-1. 화자 임베딩과 유사도 계산

-입력 음성을 전처리한 뒤 임베딩을 생성하고, 등록 프로필과의 cosine similarity를 계산합니다.

~~~python
processed = preprocess_audio(rolling_audio)

if processed is None:
    continue

embedding = encoder.embed_utterance(processed)

similarity = cosine_similarity(
    profile,
    embedding
)
~~~

### 5-2. 이중 임계값·연속 Confirm 기반 상태 갱신

-ON/OFF 임계값 사이의 Gray Zone에서는 카운터를 초기화하고, 임계값을 넘는 상태가 연속으로 확인될 때만 출력 상태를 전환합니다.

~~~python
if similarity >= on_threshold:
    positive_count += 1
    negative_count = 0
elif similarity <= off_threshold:
    negative_count += 1
    positive_count = 0
else:
    positive_count = 0
    negative_count = 0

~~~


### 5-3. 연속 판정 기반 재밍 제어 및 AM/PDM 변조

#### 1) 연속 판정 기반 재밍 제어

화자 유사도가 ON 또는 OFF 조건을 연속으로 만족한 경우에만 재밍 상태를 전환합니다. 이 로직은 순간적인 오판정으로 출력이 빈번하게 전환되는 것을 완화합니다.

~~~python
if (
    not jammer_on
    and positive_count >= ON_CONFIRM
):
    jammer_on = True
    set_jammer(True)

elif (
    jammer_on
    and negative_count >= OFF_CONFIRM
):
    jammer_on = False
    set_jammer(False)

16-bit LFSR 기반 의사 백색잡음을 생성하고, STE 기반 적응 이득과 결합해 AM 진폭을 계산합니다. 계산된 진폭은 PDM gating을 통해 출력 핀에 반영됩니다.

~~~cpp
// 16-bit LFSR pseudo-white noise
uint16_t feedback = lfsr & 1u;
lfsr >>= 1;
if (feedback) lfsr ^= 0xB400u;

int16_t noise = (int16_t)(lfsr & 0xFF) - 128;

// STE 기반 AM 진폭 계수
int16_t modulation = 256 + (noise * MOD_DEPTH_Q8) / 128;
if (modulation < 0) modulation = 0;

uint16_t amplitude = ((uint32_t)adaptiveGain * modulation) >> 8;
if (amplitude > 255) amplitude = 255;

// PDM gating
uint16_t sum = pdmAccumulator + amplitude;
if (sum >= 256) PORTG |= _BV(PG5);
else PORTG &= ~_BV(PG5);

pdmAccumulator = (uint8_t)sum;
~~~
