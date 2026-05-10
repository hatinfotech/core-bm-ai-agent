---
name: probox-hapl-warehouse-inbound
description: >-
  Nhập kho trên https://hapl.s4.probox.one: goods-receipt-notes (GRN),
  inventory-receive-vouchers, chi tiết Serial/access numbers, phân biệt với
  inventory-adjust. Use when nhập hàng, phiếu nhập kho, GRN, nhận stock, tồn kho
  sau đơn B2B hoặc ERP mua hàng.
---

# HAPL s4 — nhập hàng / nhập kho (`warehouse`)

Token & base URL: **`probox-hapl-core-rest`**.

## Khám phá module

**`OPTIONS https://hapl.s4.probox.one/v4/warehouse`**

## Phiếu nhập kho GRN

**`GET/POST/PUT/DELETE`** `…/v4/warehouse/goods-receipt-notes`

Mô tả resource (tenant): nhận **stock** và chi phí liên quan.

## Chứng nhận nhập kho (inventory)

**`…/v4/warehouse/inventory-receive-vouchers`**

Path dùng **`receive`**, không phải “**receiving**”.

## Chi tiết Serial / mã truy cập

- **`…/v4/warehouse/goods-receipt-note-detail-access-numbers`**
- **Đối chiếu xuất kho** (không dùng để nhập): **`goods-delivery-notes`**.

## Điều chỉnh kiểm kê — không thay phiếu nhập mua

- **`…/v4/warehouse/inventory-adjust-notes`** (và chi tiết con theo bookmark **`OPTIONS …/v4/warehouse`** — thường pattern **`…/inventory-adjust-note/{id}/details/…`**)

## Quy ước khi tự động hóa

1. **`OPTIONS …/{resource}?optionDocs=json`** + **`Content-Type: application/docs+json`** → **`PostRequiredFields`**, **`Schema`**, FK tới đơn mua nếu có.
2. **Hoàn thành đơn B2B** (**`probox-hapl-b2b-purchase`**) **không** đồng nghĩa đã ghi nhận tồn cho đến khi có **GRN/receive voucher** hoặc nghiệp vụ **`IsAutoBookEntry`** của kênh mua chứng nhận rõ trong hệ thống.
