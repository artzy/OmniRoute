# 실패 GitHub Actions 정리 (2026-07-29)

## 배경
포크 `artzy/OmniRoute`에서 반복 실패하는 Actions를 조사한 뒤, PR/릴리스에 필수가 아닌 것만 비활성화했다.

## 최근 실패 요약 (최근 ~80 runs)

| Workflow | Success | Failure | 필수 여부 |
|---|---:|---:|---|
| Quality Gates | 0 | 7 | 필수 (PR 품질) — 유지 |
| Release-Green (continuous) | 3 | 7 | 비필수 (NON-BLOCKING drift) |
| Nightly Node Compat | 0 | 3 | 비필수 (nightly compat) |
| Nightly Resilience | 0 | 2 | 비필수 (nightly a11y 등) |
| CI | 0 | 2 | 필수 — 유지 |
| OpenSSF Scorecard | 1 | 1 | 보안 신호 — 유지 |
| Publish to Docker Hub | 0 | 1 | 배포 — 유지 |

## 조치
`gh workflow disable`로 아래 3개 비활성화 (파일은 유지 — upstream sync 호환):

1. **Nightly Node Compat** (`nightly-compat.yml`) — Node 24/26 unit shard 실패 반복
2. **Release-Green (continuous)** (`nightly-release-green.yml`) — 문서상 NON-BLOCKING
3. **Nightly Resilience** (`nightly-resilience.yml`) — A11y axe job 실패

## 유지한 실패 워크플로
- **CI**, **Quality Gates**: PR/머지 게이트 — 수정 대상이지 삭제 대상 아님

## 재활성화
```powershell
gh workflow enable "Nightly Node Compat"
gh workflow enable "Release-Green (continuous)"
gh workflow enable "Nightly Resilience"
```

## 참고
워크플로 YAML 파일 자체는 삭제하지 않았다. 포크에서 `gh workflow disable`이 upstream pull과 충돌하지 않는 방식이다.
