---
name: probox-hapl-core-rest
description: >-
  Nền tảng REST ProBox v4 tenant https://hapl.s4.probox.one: Bearer token,
  OPTIONS discovery (/v4 + module bookmarks), và tra Contact (filter_Name).
  Use when gọi API HAPL/s4 khởi tạo phiên làm việc, khám phá module, hoặc tìm
  mã khách/NCC trong CRM — trước khi chứng từ B2B, kho, kế toán.
---

# HAPL s4 — core (auth & discovery)

Repo: **`hapl-probox-one-ai-agent`** · đường dẫn skill: `.cursor/skills/probox-hapl-core-rest/`.

## Xác thực

- Header: **`Authorization: Bearer <access_token>`**
- Giữ token ngoài git (vd. file local); **đừng** commit hay dán token vào chat.

## Base

- **`https://hapl.s4.probox.one`**

## Discovery

- **`OPTIONS https://hapl.s4.probox.one/v4`** — danh mục module + `href` OPTIONS tiếp.
- Theo từng resource: OPTIONS trên URI bookmark; **`?optionDocs=json`** và header **`Content-Type: application/docs+json`** (RFC) hoặc legacy **`docs/json`**.

## Contacts (CRM)

- **`GET …/v4/contact/contacts`** — lọc tên ví dụ **`filter_Name=Từ_khóa`**; **`select`** có thể gây lỗi cột JOIN trên một số tenant (**đừng** dựa vào **`filter_search`** nếu chưa thử OPTIONS).
- Một ví dụ NCC VLXD: **`DAYRANANHTHU`** — *NCC Ánh Thu dây ràn*.

## Mẫu tenant (đối chiếu khi tự động hóa)

- Page B2B buyer: **`77777032252`** — *VLXD Hoàng Anh Phú Lộc B2B*
- **`PurchaseObject`**: đã có luồng dùng **`1190316711`** — luôn xác nhận đúng site trước khi tái lập trong môi trường khác.

## Skill chủ đề (tách riêng)

- **`probox-hapl-b2b-purchase`** — đơn đặt mua kênh B2B Center
- **`probox-hapl-warehouse-inbound`** — nhập kho GRN / inventory receive
- **`probox-hapl-purchase-erp`** — phiếu đặt ERP + phiếu mua PURCHASE
- **`probox-hapl-accounting-payment`** — phiếu chi tiền
- **`probox-hapl-sales-master-price`** — bảng giá chủ (master-price)

Điều hướng nhanh: skill **`probox-hapl-s4-rest`** (mục lục, không trùng nội dung dài).
