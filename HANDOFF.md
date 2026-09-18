# Handoff — DAU/MAU/Lead Forecast 2026 theo Channel Source

**Deck:** [`index.html`](./index.html) (mở file này bằng browser để xem full deck có tab/UI, GitHub không render trực tiếp được vì repo private).
**Cập nhật lần cuối:** 2026-09-18
**Người phụ trách trước đó:** Chi (MKT Director) + Claude

---

## 1. Deck này để làm gì

Forecast DAU/MAU/Lead theo 5 nguồn (SEO/Digital/CRM/Direct/Others) cho công ty, breakdown theo vertical (PTY/JOB/VEH/GDS), zoom sâu vào PTY (let/sell) để đối chiếu với demand vertical team cần. 3 tab: **Actual (Jan-Aug)**, **Forecast (Sep-Dec)**, **Problem Reframe & Logic tính toán** (toàn bộ giả định, validation, rủi ro).

## 2. Tình trạng hiện tại — đã làm gì

**Tab Actual: đã rebuild hoàn chỉnh, verify kỹ.**
- 100% dữ liệu lấy từ 1 nguồn duy nhất: BigQuery `chotot-dwh.ct_product_analytics.daumaulead_mkt_rp`.
- Channel grouping lấy đúng theo pipeline Dataform gốc (đã xin table owner xác nhận) — không tự suy đoán.
- Đã đối chiếu chéo với dashboard demand tham khảo (`chile-ct.github.io/ct-demand-marketing`) — khớp tuyệt đối DAU theo từng nguồn.
- Arithmetic verify khớp 100% (breakdown cộng đúng Tổng công ty), mọi metric, mọi tháng.

**Tab Forecast: đã rebuild, dùng số Growth cung cấp thay vì tự model.**
- **SEO**: cố định theo file kế hoạch gốc (không đổi qua toàn bộ quá trình sửa).
- **Digital & CRM**: dùng thẳng forecast bottom-up của Growth (file `FC2 H2 - Growth - FC3.pdf`, Growth gửi 2026-09-18) — **không còn dùng model tự fit của deck nữa**. Đã so sánh: model tự fit trước đó cho số cao hơn Growth ~93-100% mọi tháng — không phản ánh đúng việc CP Digital đang tăng thật (đổi cách chạy campaign Google + dịch mix Let→Sell). Số Growth đáng tin hơn nhiều.
- **Direct**: baseline TB 3 tháng gần nhất (Jun-Aug) + cộng thêm uplift từ kế hoạch seeding social Q4 (đã có case study thực tế: T8 seeding 220tr → 84,424 lead, hiệu suất 2,606đ/lead). Growth xác nhận sẽ lặp lại cách làm này cho **cả PTY và JOB** trong Q4.
- **Others**: baseline tháng 6 (trước khi có campaign spike T7-8), giữ flat — chưa có model dự báo riêng, độ tin cậy thấp hơn.

**Kết luận quan trọng nhất: PTY đang hụt so với demand vertical cần — cộng dồn -21.9% (Sep -20.4%, Oct -17.0%, Nov -20.8%, Dec -29.8%, Dec là điểm nóng nhất).** Đây là con số đáng tin nhất tính đến hiện tại (dùng thẳng forecast gốc của Growth, không qua ước tính nào) — **dùng số này khi trao đổi với vertical/Growth/leadership**, không dùng các số gap từng tính trước đó trong quá trình làm việc (mỗi lần đều có lỗi/thiếu sót đã được sửa dần).

## 3. Việc còn tồn đọng — cần làm tiếp

1. **Direct & Others chưa có model dự báo thật sự** — hiện chỉ dùng baseline trung bình đơn giản. Direct đặc biệt quan trọng vì là nguồn lớn nhất (~50% Total Lead công ty).
2. **File `project_dau_mau_lead_forecast_deck_scenarios.md`** (Base/Optimistic/Pessimistic scenario, lưu riêng ngoài deck) — đã cũ qua nhiều lần sửa, cần làm lại từ đầu.
3. **Upload lên BuilderOS đang bị lỗi tool** (không liên quan tới nội dung deck) — deck hiện đang publish tạm ở GitHub repo này thay thế.
4. Vài lưu ý mở nhỏ hơn (mapping "Onflow push" trong CRM, precedent JOB seeding lần đầu, root-cause vì sao BQ actual từng lệch 2x) — chi tiết đầy đủ nằm trong phần "Problem Reframe & Logic tính toán" của deck.

## 4. Cách làm tiếp cùng Claude

Nếu ai trong team tiếp tục làm việc này qua Claude Code: chỉ cần link repo này (hoặc file `index.html`) cho Claude đọc để nắm bối cảnh — Claude (qua Chi) đã lưu đầy đủ lịch sử, phương pháp, và các quyết định (kể cả những lần đã thử rồi bỏ) trong memory riêng dưới email `chile@chotot.vn`, nên có thể tiếp tục mạch cũ mà không cần giải thích lại từ đầu, miễn là thao tác qua đúng account/session của Chi.
