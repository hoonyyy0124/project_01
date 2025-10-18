# 저장소 가이드라인

## 프로젝트 구조 및 모듈 구성
이 작업 공간은 의도적으로 최소한으로 구성되어 있습니다. 새로운 모듈은 도메인별로 `src/` 하위에 그룹화하여 배치해야 합니다. 예를 들어 `src/agents/`는 에이전트 플로우용이고 `src/services/`는 통합용입니다. 공유 유틸리티는 `src/common/`에 위치합니다. 노트북이나 탐색 스크립트는 `notebooks/`에, 작은 자산은 `assets/` 하위에 버전 관리합니다. 자동화된 테스트는 `tests/`에 배치하며, 모듈 경로를 미러링합니다 (`tests/agents/test_coordinator.py`는 `src/agents/coordinator.py`를 테스트합니다).

## 빌드, 테스트 및 개발 명령어
`python -m venv .venv && source .venv/bin/activate`로 격리된 환경을 생성하세요. `requirements.txt`에 선언된 의존성을 `pip install -r requirements.txt`로 설치하세요. 메인 에이전트 하네스를 `python -m src.agents.cli`로 실행하세요. `make lint`로 포매터와 정적 분석을 집계하고, `make test`로 전체 테스트 매트릭스를 실행하세요. 새로운 도구를 추가할 때마다 `Makefile`을 업데이트하여 기여자들이 단일 진입점을 갖도록 하세요.

## 코딩 스타일 및a 명명 규칙
4칸 들여쓰기로 PEP 8을 준수하세요. 타입 힌트를 포함하고 `mypy`를 통과하도록 유지하세요. 이는 `make lint`에 연결되어 있습니다. 모듈과 패키지는 snake_case로 명명하고 (`agent_router.py`) 클래스는 PascalCase로 명명하세요 (`AgentRouter`). 관련 설정을 `src/config/`에 그룹화하고 환경 기본값을 `config.py`를 통해 노출하세요. 샘플 설정은 `.env.example`에 저장하고 실제 비밀은 절대 커밋하지 마세요.

## 테스트 가이드라인
`pytest`로 테스트를 작성하세요. 테스트 모듈은 `test_<subject>.py`로 명명하고 설명적인 테스트 함수를 선택하세요 (`test_router_handles_unknown_intents`). `pytest --cov=src`를 실행하여 최소 90% 브랜치 커버리지를 목표로 하세요. 임시 설정보다 픽스처를 선호하고, 테스트가 격리되도록 외부 서비스를 모킹이나 경량 페이크로 격리하세요.

## 커밋 및 풀 리퀘스트 가이드라인
명령형으로 커밋을 작성하세요 (`Add orchestrator health checks`)하고 제목 줄을 72자 이하로 유지하세요. 큰 변경사항을 논리적 단위로 분할하고, 이유가 명확하지 않을 때는 본문에 근거를 포함하세요. 풀 리퀘스트는 변경사항을 요약하고, 검증 단계를 설명하며, 관련 추적 이슈에 연결해야 합니다. 동작이 변경될 때는 스크린샷이나 로그를 제공하고, 검토자가 수행해야 하는 설정이나 마이그레이션 단계를 명시하세요.

## 보안 및 설정 팁
필요한 환경 변수를 `docs/configuration.md`에 문서화하고, 변경될 때마다 `.env.example`을 업데이트하세요. `requirements.txt`에서 서드파티 버전을 고정하고 알려진 CVE에 대해 분기별로 검토하세요. 비밀에는 로컬 `.env` 파일을 사용하고 노출이 의심되면 즉시 자격 증명을 회전하세요.