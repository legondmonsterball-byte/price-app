# 최저가 앱 (price-app)

폰에서 쓰는 PWA. 생활용품 모드(쿠팡·다나와·당근·번개장터 가격 비교)와 옷 모드(플랫폼별 검색 허브). 무료 도구만 쓴다.

- 앱: `~/price-app` (index.html 한 파일 + manifest/sw/icon). GitHub Pages로 배포 예정(저장소 `price-app`).
- 서버: `~/price-worker` (Cloudflare Worker `price-hub`, 주소 https://price-hub.timetable-app.workers.dev). 비밀 키 없음. 배포는 사용자가 `npx wrangler deploy`.
- 디자인: my-design 기준 다크, 주황 포인트(운동 앱의 연두와 구분).

## 결정과 이유
- 네이버 쇼핑 검색 API는 폐지됨(프록시 호출 시 SE05 "존재하지 않는 검색 api"). 개발자센터 등록 화면에도 '검색'이 없음. 키를 받아도 못 쓴다.
- 쿠팡: k-skill 프록시(`k-skill-proxy.nomadamas.org`, 공식 쿠팡 파트너스 API)로 조회. 링크는 프록시 운영자의 제휴 링크라 앱 하단에 안내문을 둠.
- 번개장터: 공개 JSON(`api.bunjang.co.kr/api/1/find_v2.json`). 다나와: 검색 결과 HTML 파싱(데스크톱 UA 필수, 모바일 UA는 다른 화면). 당근: 검색 페이지 JSON-LD 파싱(이 맥의 node에서는 0개, 서버에서 확인 필요. 안 되면 링크 방식으로).
- 모두 비공식 표면이라 깨질 수 있음 → 사이트별로 따로 실패 처리하고 '직접 보기' 링크를 보여준다.
- 약관: 개인용, 사용자가 검색 버튼을 누를 때만 호출. 가격 알림처럼 계속 돌리는 조회는 하지 않는다(k-skill·다나와가 대량/상시 조회를 금지).
- 옷 모드는 가격을 가져오지 않고 플랫폼별 검색 URL로 보낸다(무신사·크림·4910 등은 공개 API 없음). 4910 검색 주소(`/search?keyword=`)는 응답은 200이지만 실제 검색 결과가 열리는지 미확인.

## 앞으로
- 배포 후 당근·다나와가 Cloudflare 서버에서도 되는지 확인
- 관심 상품 저장(가격 알림 없이), 가격 이력은 약관 때문에 보류
