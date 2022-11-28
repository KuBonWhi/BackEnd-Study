# HTTP 메서드

## HTTP API 예시

- 회원 정보 관리 API

### 요구사항

- 회원 목록 조회
- 회원 조회
- 회원 등록
- 회원 수정
- 회원 삭제

### API URI 설계

- 회원 목록 조회
  - /read-member-list
- 회원 조회
  - /read-member-by-id
- 회원 등록
  - /create-member
- 회원 수정
  - /update-member
- 회원 삭제
  - /delete-member
- 이런 API URI 설계는 가독성이 좋을지 모르지만 좋은 URI 설계는 아니다.
- 설계에서 가장 중요한 것은 리소스 식별
