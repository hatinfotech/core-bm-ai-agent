---
name: probox-hapl-sales-master-price
description: >-
  Bảng giá chủ (master price) trên https://hapl.s4.probox.one: sales/master-price-tables,
  master-price-table-entries với eq_MasterPriceTable và filter_ProductName.
  Use when bảng giá chính, tra SKU đơn giá, báo giá từ bảng giá chủ.
---

# HAPL s4 — bảng giá chủ (`sales`)

Token & base URL: **`probox-hapl-core-rest`**.

## Danh mục bảng chủ

**`GET`** `…/v4/sales/master-price-tables`

Tham số ví dụ: **`limit=nolimit`** (OPTIONS cho phép các param chuẩn tenant). Trên một số tenant chỉ có một bản **chủ** đang được dùng theo luồng phê duyệt của hệ — đối chiếu **`Code`** ví dụ **`BGC201031`** trong môi trường thực tế.

## Dòng SKU / đơn giá

**`GET`** `…/v4/sales/master-price-table-entries`

Tham số thường dùng:

- **`eq_MasterPriceTable=<Code>`** — giới hạn theo một bảng giá chủ
- **`filter_ProductName=…`** — tìm theo tên hàng (biến thể marketing / tên thương mại tuỳ master)

## Gợi ý tra cứu (đã đối chiếu luồng thực tế)

- SKU **dây thung / ràn ống** (ví dụ): **`11802498708`** (“4M TốT”, mã tham chiếu **`BDT3870`**); tên trong bảng có thể khác nhau (ống 5, 4.110, …).
- **`11802498956`** — ví dụ thùng đen dày 130g (cùng nhóm hàng khi so sánh catalog).

Luôn **`GET`** lại với filter phù hợp vì master tenant có thể cập nhật mã / tên.
