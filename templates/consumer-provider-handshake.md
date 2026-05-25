# Consumer-Provider Handshake — pair10

**Consumer:** Access Gate Service  
**Provider:** Core Business Service (Nhóm A6)  
**Contract version:** v1.0.0  
**Ngày ký:** 25/05/2026

---

## 1. Thông tin cặp đàm phán

| | Consumer | Provider |
|---|---|---|
| **Service** | Access Gate Service | Core Business Service |
| **Nhóm** | Access Gate team | Nhóm A6 |
| **Contract** | `contracts/core-business.openapi.yaml` | `contracts/core-business.openapi.yaml` |

---

## 2. API Consumer phụ thuộc vào Provider

| Endpoint | Method | Mục đích | Mock URL |
|---|---|---|---|
| `/health` | GET | Kiểm tra Core Business còn sống | `http://localhost:4010/health` |
| `/policy/access-check` | POST | Kiểm tra policy ra/vào realtime | `http://localhost:4010/policy/access-check` |
| `/policy/rules` | GET | Lấy danh sách policy để cache | `http://localhost:4010/policy/rules` |
| `/policy/rules/{policyId}` | GET | Lấy chi tiết một policy | `http://localhost:4010/policy/rules/{policyId}` |
| `/access-log` | POST | Gửi log quẹt thẻ lên Core Business | `http://localhost:4010/access-log` |

---

## 3. Consumer-side Smoke Test đã thực hiện

| Test | Endpoint gọi | Kết quả |
|---|---|---|
| Core Business health check | `GET /health` | ✅ PASS — 200 OK |
| Policy check direction EXIT | `POST /policy/access-check` | ✅ PASS — 200 OK, decision hợp lệ |

---

## 4. Cam kết từ Provider (Core Business)

- [x] API endpoint `/policy/access-check` phản hồi trong vòng 200ms
- [x] Response luôn có trường `decision` với giá trị `ALLOW` hoặc `DENY`
- [x] Response lỗi tuân theo cấu trúc `ProblemDetails`
- [x] Bearer token bắt buộc cho tất cả endpoint trừ `/health`
- [x] Contract được lint pass với `campus-spectral.yaml`

---

## 5. Cam kết từ Consumer (Access Gate)

- [x] Gửi đúng định dạng `cardId`: `RFID-YYYY-NNN`
- [x] Gửi đúng định dạng `gateId`: `GATE-NN`
- [x] Gửi `timestamp` chính xác thời điểm quẹt thẻ (không lệch quá 5 phút)
- [x] Gửi log đầy đủ lên `/access-log` sau mỗi quyết định
- [x] Refresh cache policy mỗi 5 phút

---

## 6. Sign-off

| Vai trò | Tên | Ngày |
|---|---|---|
| Provider (Core Business) | Nhóm A6 — Phạm Hoàng Anh, Trần Quang Huy, Nguyễn Trọng Nam | 25/05/2026 |
| Consumer (Access Gate) | Access Gate team | 25/05/2026 |
| Witness (GV/TA) | FIT4110 Teaching Team | 25/05/2026 |
