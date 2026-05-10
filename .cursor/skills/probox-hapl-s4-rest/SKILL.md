---
name: probox-hapl-s4-rest
description: >-
  Mục lục ProBox REST v4 tenant https://hapl.s4.probox.one (HAPL): trỏ tới các
  skill theo chủ đề — core, B2B mua hàng, kho nhập, ERP purchase, chi tiền,
  bảng giá chủ. Use when không biết đọc skill nào trước cho hapl.s4.probox.one.
---

# ProBox HAPL s4 REST — mục lục skill

Chi tiết từng luồng nằm trong skill riêng; skill này chỉ là **điều hướng**.

| Skill | Phạm vi |
|-------|--------|
| **`probox-hapl-core-rest`** | Bearer token, **`OPTIONS …/v4`**, Contacts **`filter_Name`**, ví dụ Page / **PurchaseObject** |
| **`probox-hapl-b2b-purchase`** | **`b2b-center/purchase/orders`**, NCC **`purchase/suppliers`**, **`PURCHASECOMPLETED`**, **`PAYMENTWAREHOUSE`** |
| **`probox-hapl-warehouse-inbound`** | **`goods-receipt-notes`**, **`inventory-receive-vouchers`**, serial / adjust |
| **`probox-hapl-purchase-erp`** | **`purchase/order-vouchers`**, **`purchase/vouchers`**, **`UNRECORDED`** trước **`DELETE`** |
| **`probox-hapl-accounting-payment`** | **`payment-vouchers`**, **`PAYMENTSUPPPLIER`**, 331 / 1111, **`APPROVED`** |
| **`probox-hapl-sales-master-price`** | **`master-price-tables`**, **`master-price-table-entries`** |

Repository lưu skills: **`hapl-probox-one-ai-agent`** trong **`.cursor/skills/`**.

**Nhớ:** không commit token; chỉ Bearer + base đúng kênh B2B **không** dùng ERP **`order-vouchers`**.
