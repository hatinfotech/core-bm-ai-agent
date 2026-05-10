---
name: probox-hapl-b2b-purchase
description: >-
  Đặt mua và hoàn tất đơn B2B Center trên https://hapl.s4.probox.one:
  purchase/suppliers, purchase/orders, PUT State PURCHASECOMPLETED,
  PAYMENTWAREHOUSE chi tiết. Use when user nói B2B đặt mua, VLXD Hoàng Anh Phú Lộc,
  đơn mua NCC, SalesObject/PurchasePage, không dùng purchase/order-vouchers cho kênh B2B.
---

# HAPL s4 — B2B đặt mua (`b2b-center`)

Token & base URL: **`probox-hapl-core-rest`**.

## Resource đơn mua B2B

**`POST/GET/PUT`** `https://hapl.s4.probox.one/v4/b2b-center/purchase/orders`

Đây là **đúng kênh “đơn mua B2B”**. **`…/v4/purchase/order-vouchers`** là **Purchase ERP**, không thay cho B2B Center.

## Đăng ký / liên kết NCC vào cổng buyer

**`POST …/v4/b2b-center/purchase/suppliers`** — body **array**:

- **`Contact`** — ví dụ **`DAYRANANHTHU`** (CRM `Code`)
- **`Page`** — Page Code buyer B2B vd. **`77777032252`**
- **`Name`**, **`Phone`**

**`RefPage`** có thể **`null`** nếu NCC chưa có satellite page.

## POST tạo đơn (`…/purchase/orders`)

Body **array một object**:

- **`Type`:** `B2BPURCHASEORDER`
- **`B2bType`:** `B2BORDER`
- **`PurchaseType`:** `B2BPURCHASEORDER`
- **`PurchasePage`:** `{ "id": "<PageCode>", "text": "…" }`
- **`PurchaseObject`** — đơn vị mua (chuỗi ví dụ **`1190316711`** — đối chiếu tenant)
- **`SalesObject`** — NCC (vd. **`DAYRANANHTHU`**); có thể thêm **`SalesObjectName`**, **`SalesObjectPhone`**
- **`SalesPage`:** khi không có **`RefPage`**, đã dùng tạm cùng **`Page`** buyer — khi có storefront NCC gán **`RefPage`** đúng
- **`RequireInvoice`**, **`IsAutoBookEntry`** tuỳ nghiệp vụ
- **`Details`** — ví dụ:

```json
[{
  "No": 1,
  "Type": "PRODUCT",
  "PurchaseBusiness": [{"id":"PURCHASEWAREHOUSE","type":"PURCHASE"}],
  "PurchaseProduct": "<Product Code>",
  "PurchaseProductName": "…",
  "Description": "…",
  "Quantity": 100,
  "Price": 6000,
  "ToMoney": 600000,
  "PurchaseUnit": {"id":"SOI","text":"Sợi"}
}]
```

*(Đơn vị **`SOI`** / **`BOO`** tùy master.)*

## PUT — không làm mất dòng chi tiết

- **Không** dựa vào **`PUT`** chỉ có **`Details`** + **`Id`** dòng ců — có thể **xoá sạch **`Details`** trên **`GET`**.
- **An toàn:** **`PUT`** với **full envelope** như **`POST`** (đủ **`Type`**, **`B2bType`**, **`PurchasePage`**, **`PurchaseObject`**, **`SalesPage`**, **`SalesObject`**, **`Amount`**, **`Details`**), mỗi dòng **`Order`** = **`Code`** chứng tử; **bỏ **`Details[].Id`** khi làm lại khối lượng/thành tiền**.
- **`PUT`** chỉ **`[{ "Code", "State": "…" }]`**: đã xác nhận **`PURCHASECOMPLETED`** giữ được dòng chi tiết; **`GET`** sau **`PUT`** với **`includeDetails=1`** vì response **`PUT`** có thể lược **`Details`**.

## Hoàn tất phiếu B2B

**`PUT …/purchase/orders/{Code}`**

```json
[{"Code":"<Code>","State":"PURCHASECOMPLETED"}]
```

Kiểm tra: **`GET`** cùng **`Code`** + **`includeDetails=1`**.

### Quan hệ kho

**`PURCHASECOMPLETED`** = xong **khâu đặt mua kênh B2B**; **tồn kho vật lý** xem skill **`probox-hapl-warehouse-inbound`**. Nếu **`IsAutoBookEntry`:** `true`, vẫn nên đối chứng UI / truy vết phiếu nhập.
