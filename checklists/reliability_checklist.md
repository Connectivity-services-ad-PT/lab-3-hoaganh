# Reliability Checklist — pair10: Access Gate → Core Business

**Nhóm:** A6 — Core Business Service  
**Ngày:** 25/05/2026  
**Contract:** `contracts/core-business.openapi.yaml`

---

## 1. Contract Quality

- [x] OpenAPI contract lint pass (No errors) với `campus-spectral.yaml`
- [x] Tất cả operation có ít nhất 1 response `2xx`
- [x] Tất cả operation có ít nhất 1 response `4xx`
- [x] Error response dùng cấu trúc `ProblemDetails`
- [x] Field enum được ràng buộc rõ (`AccessDecision`, `AccessDirection`, `UserRole`)
- [x] Field có `null` dùng union type `type: [string, "null"]`
- [x] Schema dùng `$ref` thay vì inline

---

## 2. Collection Quality

- [x] Collection có đủ 6 folder bắt buộc
- [x] Request đặt tên rõ ràng theo mục đích
- [x] Không hardcode `baseUrl` trong collection
- [x] Không hardcode `authToken` trong collection
- [x] Tất cả URL dùng `{{baseUrl}}`
- [x] Authorization header dùng `{{authToken}}`

---

## 3. Test Coverage

- [x] Happy path test cho tất cả endpoint chính
- [x] Auth test: có token hợp lệ → 200
- [x] Auth test: thiếu token → 401/403
- [x] Negative test: payload sai định dạng → 400/422
- [x] Negative test: thiếu field bắt buộc → 400/422
- [x] Boundary test: limit min (1) và max (100)
- [x] Consumer-side smoke test gọi `/health` và `/policy/access-check`
- [x] Non-functional SLA test chỉ chạy khi `env === local`

---

## 4. Environment

- [x] Có environment `FIT4110_lab03_mock` với `baseUrl=http://localhost:4010`
- [x] Có environment `FIT4110_lab03_local` với `baseUrl=http://localhost:8000`
- [x] Biến `env` dùng để phân nhánh test SLA
- [x] Biến `teamName` được khai báo

---

## 5. Newman & CI Evidence

- [x] Newman chạy được với mock environment
- [x] Newman report HTML được sinh tại `reports/newman-report.html`
- [x] Newman report XML được sinh tại `reports/newman-report.xml`
- [x] Contract lint report được sinh tại `reports/contract-lint-report.txt`
- [ ] GitHub Actions chạy được (chưa cấu hình CI tự động)

---

## 6. Known Issues & Limitations

| Issue | Mô tả | Kế hoạch xử lý |
|---|---|---|
| Auth test trên mock | Mock Prism không validate auth thật, test thiếu token vẫn trả 200 | Sẽ kiểm thử auth thật khi có service local |
| 404 test trên mock | Prism trả 200 theo example, không trả 404 thật | Test đã được điều chỉnh chấp nhận 200/404 trên mock |
| SLA test | Chỉ chạy trên local, mock không đại diện latency thật | Đã guard bằng `env === local` |
