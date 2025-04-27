# UV 활용 정리

**날짜**: 2025-04-27  
**출처**: [YOUTUBE - UV 소개 영상](https://www.youtube.com/watch?v=1kZ-touiEQ8)  
**공식문서**: [UV Documentation[](#)](https://docs.astral.sh/uv/)

<br>

## UV란?

UV는 Python 생태계에서 요즘 뜨겁게 부상하고 있는 **만능(all-in-one) 패키지 관리 도구(package management tool)**

### 기존 방식:
```bash
pip install openai  # 패키지 설치
python -m venv venv  # 가상환경 만들기
py -3.11  # 파이썬 버전 고르기
```

### UV 방식:
이 모든 걸 한방에 처리

<br>

## UV의 핵심 장점 3가지

| 장점 | 설명 |
| --- | --- |
| **빠름** | 설치 속도 체감 10배 이상! |
| **편함**   | 가상환경 만들기 + 활성화 + 패키지 설치를 명령어 몇 줄로 해결 |
| **표준화** | Python 커뮤니티에서 점점 표준으로 받아들여짐 |

<br>

## 기존 pip 방식 vs UV 방식

### pip로 작업할 때

#### 1. 패키지 설치 및 환경 구축 (처음 만들 때)
```bash
python -m venv venv         # 1. 가상환경 만들기
venv/Scripts/activate       # 2. 가상환경 활성화 (윈도우 기준)
pip install openai          # 3. 패키지 설치
python main.py              # 4. 프로그램 실행
pip freeze > requirements.txt  # 5. 패키지 목록 저장
```

#### 2. 프로젝트 공유받고 실행할 때
```bash
python -m venv venv
venv/Scripts/activate
pip install -r requirements.txt
python main.py
```

### UV로 작업할 때

#### 1. 패키지 설치 및 환경 구축 (처음 만들 때)
```bash
uv init  # 1. 프로젝트 초기화 (가상환경까지 자동 생성)
uv add openai  # 2. openai 패키지 설치 (자동으로 venv 생성 + 활성화!)
uv run main.py  # 3. 프로그램 실행
```

> `uv add` 하면 자동으로 가상환경 만들고, 바로 그 안에 설치. 별도로 venv 명령어를 쓸 필요 X

#### 2. 프로젝트 공유받고 실행할 때
```bash
uv run main.py
```

> requirements.txt 따로 설치할 필요 없이, UV가 필요한 패키지를 자동으로 맞춰줌

<br>

## 왜 UV가 혁신인가?

| 항목 | 기존 pip 방식 | UV 방식 |
| --- | --- | --- |
| **가상환경 생성** | 수동으로 `python -m venv venv`    | 자동 생성                   |
| **가상환경 활성화** | `venv/Scripts/activate`          | 자동 활성화                 |
| **패키지 설치**   | `pip install`                    | `uv add`                    |
| **프로젝트 실행** | 수동 `python main.py`            | `uv run main.py`            |
| **속도**          | 느림 (특히 대규모 설치시)        | 빠름 (네이티브 Rust 기반!)  |
| **사용 편의성**   | 복잡함                           | 극강 심플                   |

<br>

## UV가 앞으로 어떻게 될까?

- Python 커뮤니티에서도 UV를 점점 pip 대체제로 쓰자는 움직임
- Astral이라는 곳에서 관리하고 있고, Rust로 짜여서 성능도 탄탄

> **pip의 시대가 가고, UV의 시대가 올지도**

<br>

## 요약

- **UV** = 파이썬의 패키지 설치 + 가상환경 + 버전 관리를 한 번에 해주는 신세계 도구  
- **장점**: 빠름, 편함, 표준화되는 중  
- 기존 pip 대비 명령어도 줄고, 속도도 빨라지고, 실수도 줄어듬  
- **파이썬 하는 사람이라면, 반드시 써봐야 할 도구**