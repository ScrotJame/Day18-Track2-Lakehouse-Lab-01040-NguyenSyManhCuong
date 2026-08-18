# Reflection — Top Lakehouse Anti-Patterns

## Anti-Pattern nguy hiểm nhất: Small Files Problem & Bỏ qua Table Maintenance (Job 3 & Job 4)

Trong các Lakehouse Anti-Patterns, anti-pattern mà team/dự án dễ vướng phải nhất là **"Thiếu các Job bảo trì bảng định kỳ (Table Maintenance) dẫn đến tích tụ Small Files"**.

### Lý do:
1. **Streaming Ingestion tự nhiên**: Trong thực tế, các pipeline ghi dữ liệu (như Kafka -> Delta/Iceberg) thường ghi theo các micro-batch nhỏ cứ mỗi vài giây. Quá trình này hoàn toàn bình thường, nhưng nếu không có cron job `OPTIMIZE` / `rewrite_data_files` dọn dẹp, số lượng file parquet nhỏ sẽ phình to gấp hàng ngàn lần.
2. **Chi phí ẩn FinOps**: Việc chỉ chạy `expire_snapshots` trên Iceberg mà không dọn file orphan (`remove_orphan_files` / `VACUUM`) khiến file rác do job crash để lại vẫn nằm nguyên trên S3, khiến hóa đơn lưu trữ cloud không hề giảm dù đã expire metadata.

---
*Họ và tên*: Nguyễn Sỹ Mạnh Cường  
*Bài nộp*: Lab 18 — Data Lakehouse Architecture
