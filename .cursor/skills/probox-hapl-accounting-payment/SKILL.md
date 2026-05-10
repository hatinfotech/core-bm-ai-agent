---
name: probox-hapl-accounting-payment
description: >-
  Phiếu chi tiền trên https://hapl.s4.probox.one: accounting/payment-vouchers,
  PAYMENTSUPPPLIER cho NCC, tài khoản 331/1111/1121, State APPROVED, cash-vouchers.
  Use when chi tiền, trả NCC, chi mặt, phiếu chi kế toán, duyệt phiếu payment.
---

# HAPL s4 — chi tiền / phiếu chi (`accounting`)

Token & base URL: **`probox-hapl-core-rest`**.

## Resource chính

**Phiếu chi tiền** (tiền mặt / CK trả NCC, đối tác, NV):

**`GET/POST/PUT/DELETE`** `…/v4/accounting/payment-vouchers`

Nhánh thu chi tổng quát **`cash-vouchers`** với **`Type` `PAYMENT`** có thể tồn tại trong discovery — phân nhánh chứng không trùng 100% chức như **`payment-vouchers`**.

### Docs schema

**`OPTIONS …/payment-vouchers?optionDocs=json`** · header **`Content-Type: application/docs+json`**.

### Đọc phiếu & chi tiết dòng kế toán

**`GET …/payment-vouchers/{Code}?includeDetails=1`**

**(Response của `PUT` đôi khi rút gọn **`Details`** — **`GET`** lại sau khi chỉnh / duyệt.)**

## POST lập phiếu chi

- **`Content-Type: application/json`**
- Body **array chức một Object**
- **`Object`** — **`Code`** Contact (**NCC`/KH`/NV`**), VD **`DAYRANANHTHU`**
- **`Description`**, **`DateOfVoucher`** (ISO ví dụ `2026-05-10T14:10:00.000Z`)
- **`Details`** — ít nhất **`Amount`**, **`Description`**, **`DebitAccount`**, **`CreditAccount`**, **`AccountingBusiness`**
- Server thường set **`Type` `PAYMENT`**.

## Trả NCC điển hình (tenant thực tế đã chứng nhận)

| Trường | Giá trị |
|--------|--------|
| **`AccountingBusiness`** | **`PAYMENTSUPPPLIER`** — chữ **`SUPPPLIER`** có **ba chữ P** (**S-U-P-P-P-L-I-E-R**); không gõ nhầm **`SUPPLIER`** (hai P) |
| **`DebitAccount`** | **`331`** (Phải trả người bán) |
| **`CreditAccount`** | **`1111`** (tiền mặt) hoặc **`112*`** CK — danh **`GET`** **`…/v4/accounting/accounts`** |

Phiếu mẫu khác (**`GET`** chứng có sẵn + **`includeDetails=1`** rồi copy): **`SALARYADVANCEBYCASH`** · **`PAYMENTBUSINESSCOSTBYCASH`** · **`CPBANHANGTIETMAT`** · **`CLBRTCOMMISSIONPAYMENT`**.

## Duyệt / ghi nhận

1. Phiếu mới có **`NOTJUSTAPPROVED`** (hoặc tương tự chờ duyệt)
2. **`PUT …/payment-vouchers/{Code}`** — ví dụ

```json
[{"Code":"<Code>","State":"APPROVED"}]
```

**`GET`** lại **`includeDetails=1`**.
