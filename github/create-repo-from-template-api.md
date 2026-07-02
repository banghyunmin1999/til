# 템플릿 리포에서 새 리포 생성하는 API

"Use this template" 버튼을 API로 누르는 법. Jekyll Chirpy 블로그 셋업하면서 알게 됨.

```bash
curl -X POST "https://api.github.com/repos/{template_owner}/{template_repo}/generate" \
  -H "Authorization: token $GITHUB_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  -d '{"owner":"내계정","name":"새리포이름","private":false}'
```

## 왜 이게 중요했나
- 파일을 낱개로 복사해 푸시하면 **git 서브모듈(.gitmodules)이 깨짐** — chirpy-starter는 서브모듈을 썼음.
- 이 API는 버튼 클릭과 100% 동일해서 구조가 온전히 복사됨.
- fork와 다름: fork는 연결된 복제본(검색에서 기본 제외됨!), 템플릿 생성은 독립 리포.

## 덤
- 자기 계정 fork 리포는 GitHub 검색 API에서 기본으로 안 잡힘 → "분명 있었는데 검색에 없는" 미스터리의 정체였음.
- GitHub Pages를 Actions 빌드로 켜는 것도 API로 가능: `POST /repos/{o}/{r}/pages` + `{"build_type":"workflow"}`.
