# FourU Shopping

Codex와 Gemini CLI에서 포유페이 개발 테스트몰의 상품을 조회하고, 위임받은 개발용 빌링키로 상품 한 개를 테스트 결제하는 플러그인입니다. 결제 결과는 개발 원장에 기록됩니다. 실제 카드 청구와 배송은 제공하지 않습니다.

## Codex에 설치

터미널에서 `dev` 마켓플레이스를 추가하고 플러그인을 설치합니다.

```sh
codex plugin marketplace add https://github.com/4U-TECH/4u-shopping-plugin.git --ref dev
codex plugin add fouru-shopping@fouru-pay
codex mcp list
```

목록에서 `fouru-shop`을 확인하고 새 채팅을 엽니다.

## Gemini CLI에 설치

Gemini CLI를 사용할 수 있는 계정 또는 API 키를 준비합니다. 터미널에서 GitHub의 `dev` 브랜치를 복제한 뒤 해당 폴더에서 확장을 설치합니다. 설치 안내에 표시되는 `fouru-shop` 서버 주소를 확인합니다.

```sh
git clone --branch dev --single-branch https://github.com/4U-TECH/4u-shopping-plugin.git
gemini extensions install ./4u-shopping-plugin
gemini extensions list
```

새 Gemini CLI 세션에서 `/mcp`를 입력해 `fouru-shop`의 도구를 확인합니다. 이 설치 명령은 터미널용입니다.

## 테스트몰 사용

1. **상품 조회** — “https://ai-billing.paas.4usoft.kr/ 에서 원두 한 봉지 가격만 알려줘.” 상품 조회에는 키가 필요하지 않습니다.
2. **테스트 결제** — 테스트몰 업종과 금액을 허용하는 본인 개발용 빌링키를 준비합니다. “[같은 테스트몰](https://ai-billing.paas.4usoft.kr/)에서 원두 1kg 한 봉지를 3만 원 이내로 테스트 결제해줘. 이번 구매에 위임하는 개발용 빌링키는 `<테스트 키>`야.” 자리표시자를 개발용 키로 바꾸고 클라이언트의 도구 실행 확인에 따릅니다. 개인 키를 저장소나 설치 명령에 넣지 마세요.
3. **결과 확인** — 승인된 경우 영수증의 주문번호, 상품명, 수량, 금액과 `APPROVED`를 확인합니다. 같은 DEV 환경의 홈페이지에서 빌링키 소유 계정으로 로그인하면 결제내역을 확인할 수 있습니다.

두 클라이언트는 같은 [개발 MCP 서버](https://api-ai-billing.paas.4usoft.kr/mcp)를 호출합니다. `get_shop_catalog`가 서버에 등록된 상품과 가격을 반환하고, `buy_shop_product`가 예산·빌링키 정책을 적용해 승인합니다. 응답을 받지 못한 구매는 같은 `requestId`와 같은 입력으로 재시도합니다.
