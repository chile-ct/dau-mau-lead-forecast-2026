# Handoff — DAU/MAU/Lead Forecast 2026 theo Channel Source

**Deck:** [`index.html`](./index.html) (mở file này bằng browser để xem full deck có tab/UI, GitHub không render trực tiếp được vì repo private).
**Cập nhật lần cuối:** 2026-09-18 (v10)
**Người phụ trách trước đó:** Chi (MKT Director) + Claude
**Growth team đã được add làm collaborator** trên repo này — phụ trách tính lại forecast Digital (Paid Search) + CRM. Nếu là Growth: xem mục 2b/2c trong deck để hiểu format hiện tại trước khi thay số, và nhớ cascade số mới qua mục 1 (Tổng công ty), mục 3 (Vertical), mục 4 (Gap Tracker) — các mục này cộng dồn cả 5 nguồn nên sửa riêng 2b/2c mà không cascade sẽ làm deck bị lệch tổng.

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
- **Direct**: baseline không còn flat-hold TB 3 tháng nữa — dùng **seasonal ratio YoY 2025** (Sep-Dec/TB Jun-Aug cùng kỳ năm ngoái, query thật từ BQ) áp lên mức TB Jun-Aug 2026, cộng thêm uplift từ kế hoạch seeding social Q4 (case study thực tế: T8 seeding 220tr → 84,424 lead, hiệu suất 2,606đ/lead). Growth xác nhận sẽ lặp lại cách làm này cho **cả PTY và JOB** trong Q4. Lý do đổi: fit linear trend trực tiếp trên 8 tháng actual 2026 chỉ ra R²≈0.08 (thuần nhiễu, không dùng được) — nhưng so với cùng kỳ 2025 thì có pattern giảm thật ~8-9% từ hè xuống Oct-Dec.
- **Others** (bucket Others+Referral+Social — tránh literal channel `'Others'` vì có bug data-quality `dau_w_lead > dau`): cũng đổi sang seasonal ratio YoY 2025 áp lên baseline tháng 6/2026, không còn flat. 2025 cho thấy Sep là đáy sâu thật (~47% mức tháng 6), phục hồi dần Oct-Dec.

**Kết luận quan trọng nhất: PTY đang hụt so với demand vertical cần — cộng dồn -25.6% (Sep -22.5%, Oct -20.4%, Nov -25.9%, Dec -33.4%, Dec là điểm nóng nhất).** Con số này xấu hơn bản trước (-21.9%) vì Direct/Others giờ phản ánh seasonal thật thay vì giả định flat — **dùng số này khi trao đổi với vertical/Growth/leadership**, không dùng các số gap từng tính trước đó trong quá trình làm việc (mỗi lần đều có lỗi/thiếu sót đã được sửa dần, lịch sử đầy đủ nằm trong callout "Gap PTY lớn hơn mọi lần tính trước đó" ở tab Problem Reframe).

## 3. Việc còn tồn đọng — cần làm tiếp

1. **Direct & Others giờ đã có seasonal model (YoY 2025)** thay vì flat-hold, nhưng vẫn chỉ là seasonal-naive trên đúng 1 năm dữ liệu — chưa tách được driver cụ thể (occasion/campaign nào gây ra shape đó). Nếu có thêm data 2024 hoặc breakdown theo campaign, có thể làm chính xác hơn.
2. **File `project_dau_mau_lead_forecast_deck_scenarios.md`** (Base/Optimistic/Pessimistic scenario, lưu riêng ngoài deck) — đã cũ qua nhiều lần sửa (bao gồm cả từ trước khi đổi Direct/Others sang seasonal model), cần làm lại từ đầu.
3. **Upload lên BuilderOS đang bị lỗi tool** (không liên quan tới nội dung deck) — deck hiện đang publish tạm ở GitHub repo này thay thế.
4. Vài lưu ý mở nhỏ hơn (mapping "Onflow push" trong CRM, precedent JOB seeding lần đầu, root-cause vì sao BQ actual từng lệch 2x, vertical breakdown mục 3/4 dùng tỷ trọng lịch sử blended cho SEO/CRM/Direct/Others — GDS bị âm khi back-out phần Digital riêng, chênh lệch ~3% đã biết) — chi tiết đầy đủ nằm trong phần "Problem Reframe & Logic tính toán" của deck.

## 4. Cách làm tiếp cùng Claude

Nếu ai trong team tiếp tục làm việc này qua Claude Code: chỉ cần link repo này (hoặc file `index.html`) cho Claude đọc để nắm bối cảnh — Claude (qua Chi) đã lưu đầy đủ lịch sử, phương pháp, và các quyết định (kể cả những lần đã thử rồi bỏ) trong memory riêng dưới email `chile@chotot.vn`, nên có thể tiếp tục mạch cũ mà không cần giải thích lại từ đầu, miễn là thao tác qua đúng account/session của Chi.
