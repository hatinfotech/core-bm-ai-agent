---
name: probox-hapl-purchase-erp
description: >-
  ERP Purchase trên https://hapl.s4.probox.one: order-vouchers (đơn đặt NORMAL),
  purchase/vouchers phiếu mua chứng PURCHASE, multifunctional-purchases, xóa sau
  UNRECORDED. Use when đặt mua module Purchase không phải B2B Center, nhập ERP,
  nhập không qua vlxd Hoàng Anh Phú page.
---

# HAPL s4 — ERP Purchase (không phải `b2b-center`)

Token & base URL: **`probox-hapl-core-rest`** · **không nhầm** với **`probox-hapl-b2b-purchase`**.

## Phiếu đặt mua ERP

**`…/v4/purchase/order-vouchers`**

- **`Type`** thường **`NORMAL`**, **`Object`** = **`Code`** Contact (NCC) trong **`contact/contacts`**.
- **`POST`** body kiểu **array** chức năng tương tự chứng các module ProBox ( **`includeDetails`** trên **`GET`**).

### Xóa phiếu đã duyệt

1. **`PUT`** **`State`** → **`UNRECORDED`**
2. Sau đó **`DELETE …/purchase/order-vouchers/{Code}`**

## Phiếu mua chứng PURCHASE

**`…/v4/purchase/vouchers`**

- Theo mô tả resource tenant: chứng **`PURCHASE`**, có **nhận mua**, **IncludeInvoice**, **thread**, join **Contact/Creator** theo **`GET`** param.

Dùng khi cần **hóa đơn/ghi nhật ký chứng không chỉ có đặt**.

## Khác trong module Purchase (discovery)

- **`…/purchase/multifunctional-purchases`**
- **`…/purchase/voucher-details`** / **`order-voucher-details`**

**`OPTIONS /v4/purchase`** lấy full bookmark **`href`**.
