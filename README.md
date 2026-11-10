# ANSI X12 EDI Sample Files & Retail Compliance Dataset (850, 856, 810, 855, 846, 820, 824, 997)

[![ANSI X12 004010 / 005010](https://img.shields.io/badge/ANSI_X12-004010_%7C_005010-2563eb?style=flat-square)](https://ediparse.com/reference/x12-segments)
[![Retailers](https://img.shields.io/badge/Retailers-Walmart_%7C_Target_%7C_Amazon_%7C_Home_Depot_%7C_Costco_%7C_Kroger-059669?style=flat-square)](https://ediparse.com/retailers)
[![Interactive Parser](https://img.shields.io/badge/Live_Parser-EDIParse.com-7c3aed?style=flat-square)](https://ediparse.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-slate?style=flat-square)](./LICENSE)

A production-grade, open-source repository of **12 realistic ANSI X12 EDI sample files** engineered for ERP integration developers, supply chain engineers, accounting automation teams, and 1P retail vendors.

Unlike synthetic textbook snippets, every `.edi` file in [`/samples`](./samples) is modeled after real-world **Retail Implementation Guides (MIGs)** from **Walmart (Supplier One / Retail Link)**, **Target (Partners Online)**, **Amazon Vendor Central**, **The Home Depot (IDM / CommerceHub)**, **Costco Wholesale**, and **The Kroger Co.**

> **Interactive Browser Workbench (Zero Server Upload):**
> Need to inspect segments, validate chargeback rules, or convert these raw `.edi` files into **QuickBooks Desktop `.IIF`**, **QuickBooks Online CSV**, **Shopify Orders CSV**, **Excel**, or **GS1-128 (`SSCC-18`) PDF labels**?
> Launch the client-side **[EDIParse ANSI X12 Workbench](https://ediparse.com)** or browse the **[Live GitHub Pages Technical Documentation](https://laozhang713.github.io/ansi-x12-edi-sample-files/)**.

---

## Table of Contents

1. [Complete Index of 12 ANSI X12 EDI Sample Files](#1-complete-index-of-12-ansi-x12-edi-sample-files)
2. [Segment-by-Segment Anatomy: Core Order-to-Cash Transactions](#2-segment-by-segment-anatomy-core-order-to-cash-transactions)
   - [EDI 850 Purchase Order (`walmart-850-crossdock.edi`)](#21-edi-850-purchase-order-walmart-850-crossdockedi)
   - [EDI 856 Advance Ship Notice (`target-856-asn-sotpi.edi`)](#22-edi-856-advance-ship-notice-target-856-asn-sotpiedi)
   - [EDI 810 Invoice (`amazon-810-invoice.edi`)](#23-edi-810-invoice-amazon-810-invoiceedi)
   - [EDI 997 Functional Acknowledgment & EDI 824 Application Advice](#24-edi-997-functional-acknowledgment--edi-824-application-advice)
3. [Walmart EDI Pitfalls: `SDQ` Cross-Dock Store Splits & `N2` Implied Decimals](#3-walmart-edi-pitfalls-sdq-cross-dock-store-splits--n2-implied-decimals)
4. [Target 5-Tier `S-O-T-P-I` Hierarchy & GS1-128 `SSCC-18` Modulo-10 Algorithm](#4-target-5-tier-s-o-t-p-i-hierarchy--gs1-128-sscc-18-modulo-10-algorithm)
5. [ERP & Accounting Mapping Reference (QuickBooks `.IIF`, Shopify, Xero)](#5-erp--accounting-mapping-reference-quickbooks-iif-shopify-xero)
6. [Developer Reference Directory & Online Tools](#6-developer-reference-directory--online-tools)

---

## 1. Complete Index of 12 ANSI X12 EDI Sample Files

All sample files are located in the [`/samples`](./samples) directory and use standard ANSI X12 `004010VICS` or `005010` envelopes (`ISA` / `GS` / `ST` ... `SE` / `GE` / `IEA`).

| # | Sample File | X12 Set | Retailer Spec | Key Segments & Edge Cases Tested | Live Inspector & Converter |
| :- | :--- | :---: | :--- | :--- | :--- |
| 01 | [`walmart-850-crossdock.edi`](./samples/walmart-850-crossdock.edi) | **850** | [Walmart](https://ediparse.com/retailers/walmart-edi-850-856-810-converter) | `BEG*00*SA`, `REF*DP*0018`, `SAC` (`N2` implied decimal), `PO1`, `PO4`, `SDQ` 3-store cross-dock split | [Open in EDI Viewer](https://ediparse.com/tools/edi-viewer-inspector) |
| 02 | [`target-856-asn-sotpi.edi`](./samples/target-856-asn-sotpi.edi) | **856** | [Target](https://ediparse.com/retailers/target-partners-online-edi-converter) | 5-tier `HL` (`S -> O -> T -> P -> I`), `BSN`, `TD1`, `TD5`, `PRF`, `MAN*GM` 20-digit `SSCC-18`, `LIN`, `SN1` | [Inspect 856 Hierarchy](https://ediparse.com/transactions/edi-856-advance-ship-notice) |
| 03 | [`amazon-810-invoice.edi`](./samples/amazon-810-invoice.edi) | **810** | [Amazon Vendor](https://ediparse.com/retailers/amazon-vendor-central-edi-850-856) | `BIG`, `IT1` with `BP` (ASIN) & `UP` (UPC), `TDS` (`N2` cents), `SAC*A*D240` allowance, `CAD`, `ISS` | [Convert 810 to QuickBooks](https://ediparse.com/integrations/edi-810-to-quickbooks-iif) |
| 04 | [`walmart-997-receipt-ack.edi`](./samples/walmart-997-receipt-ack.edi) | **997** | [Walmart](https://ediparse.com/transactions/edi-997-functional-ack) | `AK1*SH`, `AK2*856`, `AK5*A`, `AK9*A*2*2*2` confirming clean syntax acceptance of 2 ASNs | [Read 997 Spec](https://ediparse.com/transactions/edi-997-functional-ack) |
| 05 | [`walmart-850-violations.edi`](./samples/walmart-850-violations.edi) | **850** | [Walmart Audit](https://ediparse.com/reference/errors/edi-824-997-rejection-codes) | **Chargeback test file:** explicit decimal in `SAC05`, `SDQ` sum mismatch (`90 != 100`), wrong `SE01` count | [Test Error Validator](https://ediparse.com/reference/errors/edi-824-997-rejection-codes) |
| 06 | [`walmart-855-po-ack.edi`](./samples/walmart-855-po-ack.edi) | **855** | [Walmart](https://ediparse.com/transactions/edi-855-po-acknowledgment) | `BAK*00*AC`, line-level `ACK*IA` (Item Accepted) with `068` scheduled ship date qualifier | [Explore EDI 855](https://ediparse.com/transactions/edi-855-po-acknowledgment) |
| 07 | [`homedepot-846-inventory.edi`](./samples/homedepot-846-inventory.edi) | **846** | [Home Depot](https://ediparse.com/retailers/home-depot-edi-compliance-converter) | `BIA*00*DD`, warehouse `N1*WH`, `LIN` (`UP`/`VN`/`IN`), `QTY*33` on-hand quantity, `DTM*018` | [Explore EDI 846](https://ediparse.com/transactions/edi-846-inventory-inquiry) |
| 08 | [`walmart-820-remittance.edi`](./samples/walmart-820-remittance.edi) | **820** | [Walmart](https://ediparse.com/transactions/edi-820-payment-order) | `BPR` ACH wire details, `TRN` trace number, `ENT`, `RMR*IV` net settlement vs gross invoice & discount | [Explore EDI 820](https://ediparse.com/transactions/edi-820-payment-order) |
| 09 | [`amazon-824-application-advice.edi`](./samples/amazon-824-application-advice.edi) | **824** | [Amazon Vendor](https://ediparse.com/transactions/edi-824-application-advice) | `BGN`, `OTI*TR` (Transaction Set Rejected), `TED*848` (`TDS01` imbalance error), `NTE*ERN` resolution note | [Explore EDI 824](https://ediparse.com/transactions/edi-824-application-advice) |
| 10 | [`homedepot-850-rdc.edi`](./samples/homedepot-850-rdc.edi) | **850** | [Home Depot](https://ediparse.com/retailers/home-depot-edi-compliance-converter) | Merchandising Dept `REF*DP*028`, Vendor `REF*IA*88210`, RDC `#5086` (`N1*ST`), `SDQ` 3-store split | [Home Depot Guide](https://ediparse.com/retailers/home-depot-edi-compliance-converter) |
| 11 | [`costco-850-depot.edi`](./samples/costco-850-depot.edi) | **850** | [Costco](https://ediparse.com/retailers/costco-edi-850-to-excel-quickbooks) | Regional Depot `#270` (Sumner, WA), 7-digit Costco Item (`IN`), case UOM (`CA`), `PO4` pack rules, `SDQ` | [Costco EDI Guide](https://ediparse.com/retailers/costco-edi-850-to-excel-quickbooks) |
| 12 | [`kroger-850-peyton-dc.edi`](./samples/kroger-850-peyton-dc.edi) | **850** | [Kroger](https://ediparse.com/retailers/kroger-edi-850-856-converter) | GLN (`UL`), Peyton Northern DC `#0512`, `UK` (14-digit GTIN case code), `SAC` division promo allowance | [Kroger EDI Guide](https://ediparse.com/retailers/kroger-edi-850-856-converter) |

---

## 2. Segment-by-Segment Anatomy: Core Order-to-Cash Transactions

### 2.1 EDI 850 Purchase Order (`walmart-850-crossdock.edi`)

An [ANSI X12 850 Purchase Order](https://ediparse.com/transactions/edi-850-purchase-order) initiates the retail procurement cycle. Below is the annotated structure of [`samples/walmart-850-crossdock.edi`](./samples/walmart-850-crossdock.edi):

```edi
ISA*00*          *00*          *08*925485US00     *12*5551234567     *261109*0830*^*00501*000010850*0*P*:~
GS*PO*925485US00*5551234567*20261109*0830*10850*X*005010~
ST*850*0001~
BEG*00*SA*4500991288**20261109~
CUR*BY*USD~
REF*DP*0018~
REF*MR*0020~
REF*IA*123456789~
FOB*PP*OR*BENTONVILLE~
CSH*Y~
SAC*A*D240***2500**********NEW STORE PROMO ALLOWANCE~
ITD*01*3*2**10**30*****2/10 NET 30~
DTM*037*20261112~
DTM*038*20261118~
N1*BY*WALMART STORES INC*UL*0078742000008~
N1*ST*WALMART DC 6094G-RDC*UL*0078742035260~
N3*5801 SW REGIONAL AIRPORT BLVD~
N4*BENTONVILLE*AR*72712*US~
N1*VN*ACME HOME GOODS LLC*92*123456789~
PO1*1*100*EA*12.50*WE*UP*012345678905*IN*554433221*VN*SKU-TOWEL-BLU-L*UK*10012345678902~
PID*F*08***ORGANIC COTTON BATH TOWEL ROYAL BLUE~
PO4*10*1*EA~
SDQ*EA*92*0101*25*0102*35*0103*40~
CTT*2*160~
SE*27*0001~
```

* **[`BEG` (Beginning Segment for Purchase Order)](https://ediparse.com/reference/x12-segments/beg-beginning-segment-po):** `BEG01=00` (Original PO), `BEG02=SA` (Stand-Alone Order), `BEG03=4500991288` (Walmart 10-digit PO Number), `BEG05=20261109` (PO Date `CCYYMMDD`).
* **`REF*DP*0018` & `REF*IA*123456789`:** Walmart mandates copying the exact Department Number (`DP`) and Internal Vendor Number (`IA`) into outbound [EDI 856 ASNs](https://ediparse.com/tools/edi-850-to-856-asn-generator) and [EDI 810 Invoices](https://ediparse.com/tools/edi-850-to-810-invoice-generator).
* **[`PO1` (Baseline Item Data)](https://ediparse.com/reference/x12-segments/po1-baseline-item-data-qualifiers):** Pairs product ID qualifiers starting at `PO106`: `UP` (12-digit UPC-A `012345678905`), `IN` (Buyer Item Number `554433221`), `VN` (Vendor SKU `SKU-TOWEL-BLU-L`), and `UK` (14-digit GTIN-14 shipping case code `10012345678902`).
* **`PO4` (Item Physical Details):** `PO4*10*1*EA~` specifies 10 inner units per master shipping carton.

---

### 2.2 EDI 856 Advance Ship Notice (`target-856-asn-sotpi.edi`)

An [ANSI X12 856 Advance Ship Notice (ASN)](https://ediparse.com/transactions/edi-856-advance-ship-notice) communicates physical shipment packaging and barcode identities before the truck reaches the retailer's Distribution Center:

* **[`BSN` (Beginning Segment for Ship Notice)](https://ediparse.com/reference/x12-segments/bsn-beginning-segment-asn):** `BSN*00*SHP-20261109-01*20261109*1415*0001~` where `BSN05=0001` declares a hierarchical `Pick and Pack` (`S-O-T-P-I`) structure.
* **`TD1` & `TD5`:** Reports total carton count (`CTN25*2`), gross weight (`48.5 LB`), SCAC carrier code (`ODFL`), and routing description.
* **[`HL` (Hierarchical Level)](https://ediparse.com/reference/x12-segments/hl-hierarchical-level-sotpi):** Links parent-child nodes via `HL01` (ID), `HL02` (Parent ID), and `HL03` (Level Code: `S`, `O`, `T`, `P`, `I`).
* **[`MAN*GM` (Marks and Numbers)](https://ediparse.com/reference/x12-segments/man-marks-and-numbers-sscc18):** Carries the 20-digit GS1-128 Serial Shipping Container Code (`00` Application Identifier + 18-digit `SSCC-18`) scanned at the receiving dock door.

---

### 2.3 EDI 810 Invoice (`amazon-810-invoice.edi`)

An [ANSI X12 810 Invoice](https://ediparse.com/transactions/edi-810-invoice) requests payment via Accounts Payable three-way matching (PO 850 vs. ASN 856 vs. Invoice 810):

* **[`BIG` (Beginning Segment for Invoice)](https://ediparse.com/reference/x12-segments/big-beginning-segment-invoice):** `BIG*20261109*INV-2026-8801*20261105*AMZ-PO-77665544***DI~` binds Invoice `#INV-2026-8801` to Amazon Purchase Order `#AMZ-PO-77665544` (`DI` = Debit Invoice).
* **`IT1` (Baseline Item Data - Invoice):** Mirrors `PO1` quantities (`100 EA` @ `$12.50`, `60 EA` @ `$18.00`) and includes Amazon's `BP` (Buyer Part / ASIN `B08N5WRWNW`).
* **[`TDS` (Total Monetary Value Summary)](https://ediparse.com/reference/x12-segments/tds-total-monetary-value-implied-decimal):** Formatted strictly in **`N2` implied decimal cents** (`TDS*230500*233000*225840*4660~` = `$2,305.00` net after allowance, `$2,330.00` gross line sum, `$2,258.40` discounted amount due, `$46.60` terms discount).

---

### 2.4 EDI 997 Functional Acknowledgment & EDI 824 Application Advice

* **[EDI 997 (`walmart-997-receipt-ack.edi`)](https://ediparse.com/transactions/edi-997-functional-ack):** Acts as the Layer-1 envelope syntax receipt. `AK9*A*2*2*2~` confirms that 2 transaction sets (`ST*856`) were received and accepted (`A`). Note that a clean `997` only proves X12 grammar validity—not business rule acceptance.
* **[EDI 824 (`amazon-824-application-advice.edi`)](https://ediparse.com/transactions/edi-824-application-advice):** Acts as the Layer-2 ERP/AP business rule diagnostic. In [`samples/amazon-824-application-advice.edi`](./samples/amazon-824-application-advice.edi), `OTI*TR` flags a rejected 810 invoice, and `TED*848` pinpoints the exact mathematical imbalance in `TDS01`. See the full [EDI 824 & 997 Rejection Codes Reference](https://ediparse.com/reference/errors/edi-824-997-rejection-codes).

---

## 3. Walmart EDI Pitfalls: `SDQ` Cross-Dock Store Splits & `N2` Implied Decimals

### Pitfall A: The [`SDQ` (Destination Quantity)](https://ediparse.com/reference/x12-segments/sdq-destination-quantity-cross-dock) Cross-Dock Compression Pattern

When Walmart, Home Depot, or Costco issues a Cross-Dock Purchase Order, the `N1*ST` segment specifies a single Regional Distribution Center (e.g., `WALMART DC 6094G-RDC`), while a single `PO1` line item distributes inventory across up to 10 individual retail stores per `SDQ` segment:

```edi
PO1*1*100*EA*12.50*WE*UP*012345678905*IN*554433221*VN*SKU-TOWEL-BLU-L~
SDQ*EA*92*0101*25*0102*35*0103*40~
```

**Strict Validation Invariant:**
$$\sum_{k=1}^{n} \text{SDQ}_{2k+2} \equiv \text{PO102}$$

* Store `#0101` (`25 EA`) + Store `#0102` (`35 EA`) + Store `#0103` (`40 EA`) = **`100 EA`** (`PO102`).
* In [`samples/walmart-850-violations.edi`](./samples/walmart-850-violations.edi), Store `#0101` has `15 EA`, summing to `90 EA` against `PO102=100`. Generating an 856 ASN from an unvalidated `SDQ` split triggers immediate dock-door compliance deductions.
* To automatically flatten `SDQ` store-quantity pairs into individual fulfillment rows for 3PL warehouses, use the **[EDI to CSV / Excel Converter](https://ediparse.com/tools/edi-to-csv-excel)** or **[EDI 850 to ShipStation CSV Mapper](https://ediparse.com/integrations/edi-850-to-shipstation-csv)**.

### Pitfall B: `N2` Implied Two-Decimal Monetary Fields (`TDS` & `SAC05`)

ANSI X12 uses two distinct numeric data types for currency:
1. **`R` (Real Decimal):** Explicit decimal point allowed (e.g., `PO104` / `IT104` unit price `12.50`).
2. **`N2` (Implied 2-Decimal Integer):** Decimal point **strictly prohibited**. The parser divides the integer by $100$.

| Segment / Element | Data Type | Valid Raw X12 (`$25.00` / `$2,305.00`) | Invalid Raw X12 (Triggers `824` Reject) | Dollar Value |
| :--- | :---: | :---: | :---: | :---: |
| [`SAC05` Allowance Amount](https://ediparse.com/reference/x12-segments/sac-service-promotion-allowance-charge) | `N2` | `SAC*A*D240***2500~` | `SAC*A*D240***25.00~` | `$25.00` |
| [`TDS01` Total Invoice Net](https://ediparse.com/reference/x12-segments/tds-total-monetary-value-implied-decimal) | `N2` | `TDS*230500~` | `TDS*2305.00~` | `$2,305.00` |
| `PO104` / `IT104` Unit Price | `R` | `PO1*1*100*EA*12.50~` | `PO1*1*100*EA*1250~` | `$12.50` |

---

## 4. Target 5-Tier `S-O-T-P-I` Hierarchy & GS1-128 `SSCC-18` Modulo-10 Algorithm

### 4.1 Visualizing the 5-Tier `HL` Tree (`target-856-asn-sotpi.edi`)

Target Partners Online and Walmart Supplier One require strict hierarchical parent pointers (`HL02`) in [EDI 856 Advance Ship Notices](https://ediparse.com/reference/x12-segments/hl-hierarchical-level-sotpi):

```text
HL*1**S~                              [Level 1: Shipment]  BSN02 = SHP-20261109-01, BOL = BOL-99887766
 └── HL*2*1*O~                        [Level 2: Order]     PRF01 = TGT-PO-889900 (Dept 049)
      └── HL*3*2*T~                   [Level 3: Tare]      Pallet SSCC-18: 00106141410000099990
           ├── HL*4*3*P~              [Level 4: Pack #1]   Carton SSCC-18: 00006141410000010004
           │    └── HL*5*4*I~         [Level 5: Item #1]   LIN UP=012345678905 | SN1 = 50 EA
           └── HL*6*3*P~              [Level 4: Pack #2]   Carton SSCC-18: 00006141410000010011
                └── HL*7*6*I~         [Level 5: Item #2]   LIN UP=012345678912 | SN1 = 30 EA
```

### 4.2 GS1-128 `SSCC-18` Modulo-10 Check Digit Calculation (`MAN*GM`)

Every `MAN*GM` segment in `target-856-asn-sotpi.edi` encodes a 20-digit string consisting of the 2-digit GS1 Application Identifier (`00`) followed by an 18-digit **Serial Shipping Container Code (`SSCC-18`)**.

Let $d_1 d_2 \dots d_{17}$ be the first 17 digits of the `SSCC-18` (Extension Digit + GS1 Company Prefix + Serial Reference). Moving from right to left (starting at $d_{17}$ with weight $3$, alternating $3, 1, 3, 1 \dots$), the 18th check digit $d_{18}$ is:

$$S = \sum_{i=1}^{17} d_i \times w_i \quad \text{where } w_i = \begin{cases} 3 & \text{if } i \text{ is odd} \\ 1 & \text{if } i \text{ is even} \end{cases}$$

$$d_{18} = (10 - (S \bmod 10)) \bmod 10$$

**Worked Verification (`MAN*GM*00006141410000010004~`):**
* Strip AI `00` $\rightarrow$ 17-digit payload: `00614141000001000`
* Weighted sum: $(0\times3) + (0\times1) + (6\times3) + (1\times1) + (4\times3) + (1\times1) + (4\times3) + (1\times1) + \dots + (1\times3) + (0\times1) + (0\times3) + (0\times1) = 18 + 1 + 12 + 1 + 12 + 1 + 3 = 48$
* Check digit: $(10 - (48 \bmod 10)) \bmod 10 = 10 - 8 = \mathbf{2}$ *(or verify any custom GS1 prefix instantly with the [SSCC-18 Check Digit Calculator](https://ediparse.com/tools/sscc-18-check-digit-calculator) and print thermal 4x6 barcodes with the [GS1-128 Shipping Label Generator](https://ediparse.com/tools/gs1-128-shipping-label-generator))*.

---

## 5. ERP & Accounting Mapping Reference (QuickBooks `.IIF`, Shopify, Xero)

When integrating ANSI X12 files into accounting or e-commerce systems, use the following field mappings (or automate them one-click at **[EDIParse Integrations](https://ediparse.com/integrations)**):

| Target System | Source X12 File | Key Header Mapping | Line Item & Allowance Mapping | Direct Automation Guide |
| :--- | :---: | :--- | :--- | :--- |
| **QuickBooks Desktop (`.IIF`)** | `850` / `810` | `TRNS` (`INVOICE` / `SALESORD`), `DOCNUM` $\leftarrow$ `BIG02` / `BEG03`, `NAME` $\leftarrow$ `N1*BY` | `SPL` rows for `PO1`/`IT1` (`AMOUNT` negative in `.IIF`); `SAC` $\rightarrow$ Discount `SPL` row so `TRNS + ∑SPL = 0` | [850 to QuickBooks `.IIF`](https://ediparse.com/integrations/edi-850-to-quickbooks-desktop-iif) / [810 to `.IIF`](https://ediparse.com/integrations/edi-810-to-quickbooks-iif) |
| **QuickBooks Online (CSV)** | `850` / `810` | `InvoiceNo` $\leftarrow$ `BIG02`, `Customer` $\leftarrow$ `N1*BY`, `InvoiceDate` $\leftarrow$ `BIG01`, `DueDate` $\leftarrow$ `ITD07` | `Item(Product/Service)` $\leftarrow$ `VN` SKU, `ItemQuantity` $\leftarrow$ `IT102`, `ItemRate` $\leftarrow$ `IT104` | [EDI 850 to QuickBooks Online CSV](https://ediparse.com/integrations/edi-850-to-quickbooks-online-csv) |
| **Shopify Orders CSV** | `850` | `Name` $\leftarrow$ `BEG03`, `Shipping Name` $\leftarrow$ `N1*ST` (or `SDQ` store ID), `Tags` $\leftarrow$ `REF*DP` | `Lineitem sku` $\leftarrow$ `PO1` (`VN`), `Lineitem quantity` $\leftarrow$ `PO102` (or `SDQ` store qty), `Lineitem price` $\leftarrow$ `PO104` | [EDI 850 to Shopify CSV](https://ediparse.com/integrations/edi-850-to-shopify-csv) |
| **Xero Accounting CSV** | `810` | `*ContactName` $\leftarrow$ `N1*BY`, `*InvoiceNumber` $\leftarrow$ `BIG02`, `Reference` $\leftarrow$ `BIG04` (PO #) | `*ItemCode` $\leftarrow$ `IT1` (`VN`), `*Quantity` $\leftarrow$ `IT102`, `*UnitAmount` $\leftarrow$ `IT104`, `*AccountCode` $\leftarrow$ `200` | [EDI 810 to Xero CSV](https://ediparse.com/integrations/edi-810-to-xero-csv) |

---

## 6. Developer Reference Directory & Online Tools

All interactive converters and validators below execute **100% locally in your browser via WebAssembly/TypeScript** (zero server upload, full NDA compliance):

### Interactive X12 Converters & Generators
* **[ANSI X12 EDI Viewer & Segment Inspector](https://ediparse.com/tools/edi-viewer-inspector)** — Parse raw X12 envelopes, highlight syntax errors, and inspect loops.
* **[EDI to CSV / Excel Spreadsheet Converter](https://ediparse.com/tools/edi-to-csv-excel)** — Flatten `PO1` items and `SDQ` store allocations into clean rows.
* **[EDI to JSON / XML Developer Converter](https://ediparse.com/tools/edi-to-json-xml)** — Transform X12 hierarchical trees into structured JSON/XML payloads.
* **[EDI 850 to 856 ASN Turnaround Generator](https://ediparse.com/tools/edi-850-to-856-asn-generator)** — Auto-build compliant `S-O-T-P-I` 856 ASNs directly from an inbound 850 PO.
* **[EDI 850 to 810 Invoice Turnaround Generator](https://ediparse.com/tools/edi-850-to-810-invoice-generator)** — Turn an 850 PO into a balanced 810 Invoice with `N2` `TDS` validation.
* **[GS1-128 (`UCC-128`) Shipping Label Generator](https://ediparse.com/tools/gs1-128-shipping-label-generator)** — Render 4x6 printable carton labels synchronized with `MAN*GM`.
* **[SSCC-18 Modulo-10 Check Digit Calculator](https://ediparse.com/tools/sscc-18-check-digit-calculator)** — Compute and verify 18-digit GS1 container barcodes.

### Retailer-Specific Compliance Hubs
* [Walmart Supplier One EDI 850 / 856 / 810 Converter](https://ediparse.com/retailers/walmart-edi-850-856-810-converter)
* [Target Partners Online EDI 850 & 856 SOTPI Converter](https://ediparse.com/retailers/target-partners-online-edi-converter)
* [Amazon Vendor Central EDI 850, 856 & 810 Toolkit](https://ediparse.com/retailers/amazon-vendor-central-edi-850-856)
* [The Home Depot IDM / CommerceHub EDI Converter](https://ediparse.com/retailers/home-depot-edi-compliance-converter)
* [Costco Wholesale Depot EDI 850 to Excel & QuickBooks](https://ediparse.com/retailers/costco-edi-850-to-excel-quickbooks)
* [Lowe's Supplier EDI 850 / 856 / 810 Translator](https://ediparse.com/retailers/lowes-edi-translator)
* [Kroger EDI 850 & 856 Grocery Compliance Converter](https://ediparse.com/retailers/kroger-edi-850-856-converter)
* [SPS Commerce WebForms CSV Export Alternative](https://ediparse.com/integrations/sps-commerce-csv-alternative)

---

## License

Released under the [MIT License](./LICENSE). Maintained by the engineering team at **[EDIParse.com](https://ediparse.com)**.
