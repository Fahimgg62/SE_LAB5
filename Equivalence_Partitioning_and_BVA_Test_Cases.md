# Improved Data Leakage Detection System
## Software Testing: Equivalence Partitioning (EP) & Boundary Value Analysis (BVA) Test Case Suite

**Project Title:** Improved Data Leakage Detection System  
**Technology Stack:** MERN Stack (MongoDB, Express.js, React.js, Node.js)  
**Document Reference:** Project Master SRS (`Complete_SRS_Document.txt`), Sequence Diagrams (`Sequence_Diagrams.txt`)  
**Lab Assignment:** Equivalence Partitioning (EP) & Boundary Value Analysis (BVA) Test Case Design  
**Note:** All standard/trivial authentication modules (*login, logout, register, account creation, password length, session timeout*) have been excluded and replaced with specialized core project features (steganographic tracking, forensic analysis, file validation, access expiration, and audit pipelines).

---

### Team Members & Allocation (4 Members, 3 Test Cases Each = 12 Total Test Cases)

| Member No. | Member Name | Student ID | Assigned Core Features / Requirements | Design Technique |
| :--- | :--- | :--- | :--- | :--- |
| **Member 1** | **Fahim Bin Zaman** | **24524203055** | 1. File Upload Format Validation `[FR-1]`<br>2. File Upload Size Limitation `[FR-1 / NFR-3]`<br>3. Target Agent Concurrent Allocation Count `[FR-2]` | • EP<br>• BVA<br>• BVA |
| **Member 2** | **Abid Ajmal Tahmid** | **24524203133** | 4. Steganographic Tracking Token (UUID v4) Format `[FR-3]`<br>5. Minimum Cover Image Dimensions for LSB Watermarking `[FR-3 / NFR-8]`<br>6. Steganographic Cover Text Payload Capacity `[FR-3]` | • EP<br>• BVA<br>• BVA |
| **Member 3** | **Hojaifa** | **24524203181** | 7. Forensic Bit Error Rate (BER) Tolerance Threshold `[FR-5]`<br>8. Leaked File Forensic Upload Format Validation `[FR-5]`<br>9. Forensic Attribution Similarity Match Threshold `[FR-5]` | • BVA<br>• EP<br>• BVA |
| **Member 4** | **Team Member** | **24524203023** | 10. Audit Log Date Range Filter Validation `[FR-4]`<br>11. File Access / Download Expiration Duration `[FR-2 / NFR-1]`<br>12. Batch Recipient Email Notification Limit `[FR-2 / NFR-3]` | • EP<br>• BVA<br>• BVA |

---

# ==============================================================================
# MEMBER 1: FAHIM BIN ZAMAN (ID: 24524203055)
# ==============================================================================

## Feature 1: File Upload Format Validation [FR-1]
- **Requirement:** The system shall validate file formats during upload, supporting only `.txt`, `.jpg`, and `.bmp`. All other formats must be rejected with a descriptive error message.
- **Design Technique:** Equivalence Partitioning (EP)

### Partition Analysis Table
| Partition | Description | Example Input | Expected System Behavior |
| :--- | :--- | :--- | :--- |
| **P1 (Valid)** | Allowed Plain Text File (`.txt`) | `confidential_report.txt` | Accept file for upload & steganography |
| **P2 (Valid)** | Allowed Image File (`.jpg` / `.jpeg`) | `design_architecture.jpg` | Accept file for upload & steganography |
| **P3 (Valid)** | Allowed Bitmap File (`.bmp`) | `network_diagram.bmp` | Accept file for upload & steganography |
| **P4 (Invalid)** | Disallowed Document Formats | `budget_2026.pdf`, `contracts.docx` | Reject with format error message |
| **P5 (Invalid)** | Executable / Script Formats | `trojan.exe`, `script.sh` | Reject with security alert |
| **P6 (Invalid)** | File with missing extension | `secret_archive` | Reject with extension error |

### Summary Test Case Table (EP)
| TC ID | Input | Partition | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC-DLD-EP-01** | `confidential_brief.txt` | Valid plain text extension (P1) | Accept |
| **TC-DLD-EP-02** | `evidence_chart.pdf` | Disallowed document format (P4) | Reject |
| **TC-DLD-EP-03** | `malicious_payload.exe` | Executable script format (P5) | Reject |

---

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-EP-01** |
| **Test Scenario** | Verify that the system accepts valid file formats (`.txt`, `.jpg`, `.bmp`) and strictly rejects unsupported file extensions (`.pdf`, `.exe`) during admin upload (FR-1). |
| **Preconditions** | 1. Administrator is authenticated in the system.<br>2. Administrator is on the "Upload & Distribute File" portal page (`/admin/upload`). |
| **Test Steps** | 1. Click on the "Browse File" button.<br>2. Select an unsupported file: `evidence_chart.pdf`.<br>3. Attempt to submit the file upload form. |
| **Input Data** | - Filename: `evidence_chart.pdf`<br>- MIME Type: `application/pdf`<br>- File Size: 2.4 MB |
| **Expected Result** | System blocks upload and displays an error message: *"Invalid file format. Only .txt, .jpg, and .bmp files are supported for steganographic tracking."* File is not saved to server storage. |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates partition P4 (Disallowed format) using Equivalence Partitioning (EP). |

---

## Feature 2: File Upload Size Limitation [FR-1 / NFR-3]
- **Requirement:** The system enforces a maximum upload file size of **50 MB** ($52,428,800\text{ Bytes}$) per document to preserve server processing capacity and ensure rapid steganographic embedding. Minimum upload file size is **1 Byte** (empty files of 0 Bytes are rejected).
- **Boundary:** Valid Range = [1 Byte, 50 MB (52,428,800 Bytes)]. Boundary threshold = 50 MB.
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Test Value | Byte Equivalent | Meaning | Expected Result |
| :--- | :--- | :--- | :--- |
| **0 Bytes** | 0 Bytes | Just below minimum boundary (Invalid) | Reject (Empty file error) |
| **1 Byte** | 1 Byte | Minimum valid boundary (Valid) | Accept |
| **49.9 MB** | 52,323,942 Bytes | Just below maximum boundary (Valid) | Accept |
| **50.0 MB** | 52,428,800 Bytes | Exact maximum boundary value (Valid) | Accept |
| **50.1 MB** | 52,533,658 Bytes | Just above maximum boundary (Invalid) | Reject (File size limit exceeded) |

### Summary Test Case Table (BVA)
| TC ID | Uploaded File Size | Boundary Meaning | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC-DLD-BVA-01** | `0 Bytes` (empty file) | Below minimum valid boundary | Reject |
| **TC-DLD-BVA-02** | `52,428,800 Bytes` (50.0 MB) | Exact maximum boundary | Accept |
| **TC-DLD-BVA-03** | `52,533,658 Bytes` (50.1 MB) | Just above maximum boundary | Reject |

---

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-02** |
| **Test Scenario** | Verify that the system blocks file uploads that exceed the 50 MB upper size threshold using Boundary Value Analysis (FR-1 / NFR-3). |
| **Preconditions** | 1. Administrator is on the `/admin/upload` module.<br>2. A test file `large_dataset.txt` of size 50.1 MB (52,533,658 bytes) is prepared. |
| **Test Steps** | 1. Open the File Upload module.<br>2. Select `large_dataset.txt` (50.1 MB).<br>3. Click "Upload & Process". |
| **Input Data** | - Filename: `large_dataset.txt`<br>- File Size: `50.1 MB` (`52,533,658 Bytes`) |
| **Expected Result** | System halts upload at backend middleware and responds with HTTP 413 Payload Too Large and displays error: *"Upload failed: File size exceeds the maximum limit of 50 MB."* |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates upper boundary value ($50\text{ MB} + 0.1\text{ MB}$) using Boundary Value Analysis (BVA). |

---

## Feature 3: Target Agent Concurrent Allocation Count [FR-2]
- **Requirement:** When distributing a confidential document, the Administrator can allocate between **1 and 20 authorized agents** concurrently in a single distribution batch. Allocating 0 agents or more than 20 agents is blocked.
- **Boundary:** Valid Range = [1, 20 agents].
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Selected Agents Count | Meaning | Expected System Behavior |
| :--- | :--- | :--- |
| **0 Agents** | Just below minimum boundary (Invalid) | Reject ("Select at least 1 agent") |
| **1 Agent** | Minimum boundary value (Valid) | Accept (Generates 1 watermarked copy) |
| **20 Agents** | Maximum boundary value (Valid) | Accept (Generates 20 watermarked copies) |
| **21 Agents** | Just above maximum boundary (Invalid) | Reject ("Max 20 agents per batch") |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-03** |
| **Test Scenario** | Verify that the system enforces the maximum agent batch selection boundary [1 to 20 agents] during file distribution (FR-2). |
| **Preconditions** | 1. Administrator has uploaded a valid file `strategy_doc.txt`.<br>2. The organization has 25 registered active agents in the database. |
| **Test Steps** | 1. Navigate to the "Target Agent Allocation" screen.<br>2. Select 21 agent checkboxes from the agent list.<br>3. Click "Proceed to Steganographic Distribution". |
| **Input Data** | - Target File: `strategy_doc.txt`<br>- Selected Agents Count: `21 agents` |
| **Expected Result** | System rejects distribution, highlights the agent list, and displays validation warning: *"Batch distribution limit exceeded: Maximum 20 recipients permitted per allocation cycle."* |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates upper boundary $+ 1$ threshold ($20 + 1 = 21$) using Boundary Value Analysis (BVA). |

---

# ==============================================================================
# MEMBER 2: ABID AJMAL TAHMID (ID: 24524203133)
# ==============================================================================

## Feature 4: Steganographic Tracking Token (UUID v4) Format Validation [FR-3]
- **Requirement:** During recipient-specific copy generation, the backend steganography engine generates and validates an RFC 4122 compliant UUID v4 tracking token consisting of **exactly 36 characters** in canonical hexadecimal form (`8-4-4-4-12`, e.g., `550e8400-e29b-41d4-a716-446655440000`). Tokens missing hyphens, wrong length, or with non-hex characters must be rejected by the validation parser before embedding.
- **Design Technique:** Equivalence Partitioning (EP)

### Partition Analysis Table
| Partition | Description | Example Input | Expected System Behavior |
| :--- | :--- | :--- | :--- |
| **P1 (Valid)** | Standard 36-character canonical UUID v4 string | `c4a7e810-7b2a-4f51-9e23-8d6f1234abcd` | Accept for steganographic embedding |
| **P2 (Invalid)** | Short token string (< 36 characters) | `c4a7e810-7b2a-4f51-9e23` (24 chars) | Reject ("Invalid UUID token length") |
| **P3 (Invalid)** | Long token string (> 36 characters) | `c4a7e810-7b2a-4f51-9e23-8d6f1234abcdef01` (42 chars) | Reject ("Invalid UUID token length") |
| **P4 (Invalid)** | Missing hyphen delimiters (raw 32 hex chars) | `c4a7e8107b2a4f519e238d6f1234abcd` | Reject ("Malformed canonical format") |
| **P5 (Invalid)** | Contains non-hexadecimal characters | `c4a7e810-7b2a-4f51-9e23-8d6fZXYZabcd` | Reject ("Non-hexadecimal characters detected") |

### Summary Test Case Table (EP)
| TC ID | Input Token | Partition | Expected Result |
| :--- | :--- | :--- | :--- |
| **TC-DLD-EP-04** | `c4a7e810-7b2a-4f51-9e23` | Length < 36 characters (P2) | Reject |
| **TC-DLD-EP-05** | `c4a7e810-7b2a-4f51-9e23-8d6f1234abcd` | Valid canonical 36-char UUID (P1) | Accept |
| **TC-DLD-EP-06** | `c4a7e810-7b2a-4f51-9e23-8d6fZXYZabcd` | Contains non-hex characters (P5) | Reject |

---

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-EP-05** |
| **Test Scenario** | Verify that the steganographic engine accepts a valid 36-character canonical UUID v4 tracking token for embedding (FR-3). |
| **Preconditions** | 1. Backend steganography pipeline receives file allocation request.<br>2. A valid cover file `blueprint.bmp` is staged in storage. |
| **Test Steps** | 1. Invoke `stegoEngine.validateAndEmbed(coverFile, trackingToken)`.<br>2. Supply valid 36-char token: `c4a7e810-7b2a-4f51-9e23-8d6f1234abcd`.<br>3. Execute watermark embedding routine. |
| **Input Data** | - Cover File: `blueprint.bmp`<br>- Tracking Token: `c4a7e810-7b2a-4f51-9e23-8d6f1234abcd` (Length = 36) |
| **Expected Result** | Token passes regex validation `^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$`, embedding succeeds, and unique recipient copy is generated. |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates valid partition P1 (Canonical UUID) using Equivalence Partitioning (EP). |

---

## Feature 5: Minimum Cover Image Dimensions for LSB Watermarking [FR-3 / NFR-8]
- **Requirement:** For `.jpg` and `.bmp` files, the Least Significant Bit (LSB) steganography engine requires a minimum image resolution of **$256 \times 256$ pixels** ($65,536$ pixels) to embed the 128-bit encrypted tracking token across redundant color channels without causing visible image distortion or payload clipping. Cover images below $256 \times 256$ pixels must be rejected.
- **Boundary:** Minimum Width & Height = 256 pixels.
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Image Dimensions | Total Pixels | Meaning | Expected Result |
| :--- | :--- | :--- | :--- |
| **$255 \times 255$ pixels** | $65,025\text{ px}$ | Just below minimum boundary ($256 - 1$) | Reject (Insufficient carrier resolution) |
| **$256 \times 256$ pixels** | $65,536\text{ px}$ | Exact minimum boundary ($256$) | Accept (Embedding successful) |
| **$257 \times 257$ pixels** | $66,049\text{ px}$ | Just above minimum boundary ($256 + 1$) | Accept (Embedding successful) |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-04** |
| **Test Scenario** | Verify that the LSB steganographic engine rejects images with dimensions below $256 \times 256$ pixels using Boundary Value Analysis (FR-3 / NFR-8). |
| **Preconditions** | 1. Administrator uploads image `icon_small.bmp` with resolution $255 \times 255$ pixels.<br>2. Administrator attempts to allocate this image to an agent. |
| **Test Steps** | 1. Upload `icon_small.bmp` ($255 \times 255$).<br>2. Select target agent.<br>3. Click "Generate Watermarked Copy". |
| **Input Data** | - Filename: `icon_small.bmp`<br>- Image Width: `255 pixels`, Height: `255 pixels` |
| **Expected Result** | System blocks embedding and displays error: *"Steganographic error: Cover image dimensions (255x255) are below the minimum required resolution of 256x256 pixels."* |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates lower boundary value $256 - 1 = 255$ pixels using Boundary Value Analysis (BVA). |

---

## Feature 6: Steganographic Cover Text Payload Capacity [FR-3]
- **Requirement:** For `.txt` files, the zero-width steganographic embedding engine requires a minimum of **50 words** in the cover document to reliably embed the unique 128-bit tracking footprint without line overflow. Text documents with fewer than 50 words cannot accommodate the watermark.
- **Boundary:** Minimum Word Count = 50 words.
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Word Count in Text File | Meaning | Expected System Behavior |
| :--- | :--- | :--- |
| **48 words** | Well below boundary | Reject ("Insufficient cover text capacity") |
| **49 words** | Just below boundary ($50 - 1$) | Reject ("Text must contain at least 50 words") |
| **50 words** | Exact boundary value ($50$) | Accept (Embedding successful) |
| **51 words** | Just above boundary ($50 + 1$) | Accept (Embedding successful) |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-05** |
| **Test Scenario** | Verify that the steganographic engine permits watermark embedding at exactly 50 words and blocks files with 49 words (FR-3 / BVA). |
| **Preconditions** | 1. Administrator selects `memo_short.txt` containing exactly 49 words.<br>2. Administrator selects 1 recipient agent for distribution. |
| **Test Steps** | 1. Upload `memo_short.txt`.<br>2. Select agent `Hojaifa (24524203181)`.<br>3. Click "Generate Watermarked Copy". |
| **Input Data** | - File: `memo_short.txt`<br>- File Content Word Count: `49 words` |
| **Expected Result** | System steganography engine returns an error: *"Steganographic embedding error: Cover text contains 49 words, but a minimum of 50 words is required to encode the tracking token."* |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates lower boundary value $50 - 1 = 49$ words using Boundary Value Analysis (BVA). |

---

# ==============================================================================
# MEMBER 3: HOJAIFA (ID: 24524203181)
# ==============================================================================

## Feature 7: Forensic Bit Error Rate (BER) Tolerance Threshold [FR-5]
- **Requirement:** When extracting watermarks from a recovered leaked file that may have suffered partial compression or noise, the error-correcting Reed-Solomon decoder tolerates a maximum Bit Error Rate (BER) of **5.0%** ($0.050$). If the BER exceeds 5.0%, the watermark packet cannot be safely corrected and is marked unrecoverable.
- **Boundary:** Maximum Tolerable BER = $5.0\%$. Range = [$0.0\%$ to $100.0\%$].
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Bit Error Rate (BER) | Meaning | Decoder Behavior / Output |
| :--- | :--- | :--- |
| **4.9%** | Below maximum boundary ($5.0 - 0.1$) | Error correction successful; token fully recovered |
| **5.0%** | Exact maximum boundary threshold ($5.0\%$) | Error correction successful; token fully recovered |
| **5.1%** | Just above maximum boundary ($5.0 + 0.1$) | Decoder failure; "Watermark unrecoverable due to high noise (> 5.0%)" |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-06** |
| **Test Scenario** | Verify that the forensic extraction engine successfully recovers tokens when the Bit Error Rate is at 5.0% and rejects recovery when BER reaches 5.1% (FR-5 / BVA). |
| **Preconditions** | 1. A recovered leaked file `leaked_distorted.jpg` has undergone noise simulation resulting in a Bit Error Rate of 5.1%.<br>2. Administrator initiates forensic token extraction. |
| **Test Steps** | 1. Navigate to `/admin/investigate`.<br>2. Upload `leaked_distorted.jpg`.<br>3. Click "Extract Watermark Token". |
| **Input Data** | - Leaked File: `leaked_distorted.jpg`<br>- Simulated Noise BER: `5.1%` |
| **Expected Result** | Error correction algorithm halts and returns: *"Extraction failed: Bit Error Rate (5.1%) exceeds maximum recoverable threshold of 5.0%. Watermark is damaged."* |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates upper boundary $+ 0.1$ threshold ($5.0\% + 0.1\% = 5.1\%$) using Boundary Value Analysis (BVA). |

---

## Feature 8: Leaked File Forensic Upload Format Validation [FR-5]
- **Requirement:** During leak investigation, the forensic analysis module extracts steganographic tracking markers from suspected leaked files. The system accepts only supported carrier formats (`.txt`, `.jpg`, `.bmp`) for extraction. Unsupported formats (e.g. `.zip`, `.mp4`, `.docx`) must be rejected prior to analysis.
- **Design Technique:** Equivalence Partitioning (EP)

### Partition Analysis Table
| Partition | Description | Example Input | Expected System Behavior |
| :--- | :--- | :--- | :--- |
| **P1 (Valid)** | Recovered Text Document (`.txt`) | `leaked_memo.txt` | Accept for steganographic token extraction |
| **P2 (Valid)** | Recovered JPEG Image (`.jpg` / `.jpeg`) | `leaked_photo.jpg` | Accept for LSB watermark extraction |
| **P3 (Valid)** | Recovered Bitmap Image (`.bmp`) | `leaked_blueprint.bmp` | Accept for LSB watermark extraction |
| **P4 (Invalid)** | Compressed Archive Files | `leaked_files.zip`, `.rar` | Reject with error message |
| **P5 (Invalid)** | Video or Audio Media | `screen_recording.mp4` | Reject with error message |
| **P6 (Invalid)** | Corrupted / Zero-byte file | `empty_leak.txt` (0 B) | Reject with "Empty file cannot be analyzed" |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-EP-07** |
| **Test Scenario** | Verify that the Forensic Leak Analysis module accepts valid suspected file formats and rejects unsupported archive formats like `.zip` (FR-5). |
| **Preconditions** | 1. Administrator is on the Forensic Analysis Module (`/admin/investigate`). |
| **Test Steps** | 1. Click on "Upload Leaked File".<br>2. Select `leaked_archive.zip`.<br>3. Click "Extract Forensic Markers". |
| **Input Data** | - Leaked File: `leaked_archive.zip`<br>- File Type: `application/zip` |
| **Expected Result** | System forensic parser rejects upload and displays: *"Forensic analysis failed: Unsupported file type. Only original carrier formats (.txt, .jpg, .bmp) can be scanned for tracking tokens."* |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates invalid equivalence partition P4 (Compressed archive) using Equivalence Partitioning (EP). |

---

## Feature 9: Forensic Attribution Similarity Match Threshold [FR-5]
- **Requirement:** When comparing extracted decoy data or steganographic tokens from a recovered file against the database audit logs, the system calculates a match confidence percentage ($0\%$ to $100\%$). A recipient attribution is confirmed positive only if the match confidence is **$\ge 80.0\%$**. Matches below $80.0\%$ are marked "Inconclusive / Tampered".
- **Boundary:** Threshold = $80.0\%$. Range = [$0.0\%$ to $100.0\%$].
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Match Confidence Score | Meaning | System Attribution Output |
| :--- | :--- | :--- |
| **79.0%** | Just below boundary threshold | "Inconclusive: Token degradation detected (Confidence: 79.0%). Cannot attribute." |
| **79.9%** | Infinitesimally below boundary ($80.0 - 0.1$) | "Inconclusive: Below 80.0% confidence threshold." |
| **80.0%** | Exact boundary threshold ($80.0\%$) | "Positive Match: Attributed to Agent [ID & Name] (Confidence: 80.0%)." |
| **80.1%** | Just above boundary threshold ($80.0 + 0.1$) | "Positive Match: Attributed to Agent [ID & Name] (Confidence: 80.1%)." |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-07** |
| **Test Scenario** | Verify that forensic leak attribution triggers a positive agent match at exactly 80.0% confidence and classifies scores below 80.0% as inconclusive (FR-5 / BVA). |
| **Preconditions** | 1. A leaked document `confidential_leak.jpg` has been uploaded to the forensic engine.<br>2. Extracted steganographic bits match Agent Hojaifa's token at exactly 80.0% due to slight compression. |
| **Test Steps** | 1. Initiate forensic scan for `confidential_leak.jpg`.<br>2. Backend calculates token Hamming distance and determines match confidence.<br>3. Observe forensic investigation report generated on the dashboard. |
| **Input Data** | - Leaked File: `confidential_leak.jpg`<br>- Extracted Signature Match: `80.0%` |
| **Expected Result** | System validates that $80.0\% \ge 80.0\%$, confirms positive attribution, and displays: *"Match Confirmed: Recipient copy belongs to Hojaifa (ID: 24524203181) with 80.0% confidence."* |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates exact boundary threshold value ($80.0\%$) using Boundary Value Analysis (BVA). |

---

# ==============================================================================
# MEMBER 4: TEAM MEMBER (ID: 24524203023)
# ==============================================================================

## Feature 10: Audit Log Date Range Filter Validation [FR-4 / Auditability]
- **Requirement:** In the Audit Log viewer, the administrator can filter file distribution and download history by date range (`Start Date` to `End Date`). The system requires that `Start Date` $\le$ `End Date` and neither date can be in the future.
- **Design Technique:** Equivalence Partitioning (EP)

### Partition Analysis Table
| Partition | Description | Example Input | Expected System Behavior |
| :--- | :--- | :--- | :--- |
| **P1 (Valid)** | Standard historical date range | Start: `2026-09-01`, End: `2026-10-01` | Display filtered audit records |
| **P2 (Valid)** | Same-day single date query | Start: `2026-10-08`, End: `2026-10-08` | Display audit logs for that single day |
| **P3 (Invalid)** | Start date chronologically after End date | Start: `2026-10-08`, End: `2026-09-01` | Reject ("Start date cannot be after End date") |
| **P4 (Invalid)** | Future date selected | Start: `2026-10-08`, End: `2026-12-31` | Reject ("Date cannot be in the future") |
| **P5 (Invalid)** | Invalid / Malformed date string | Start: `abcd-ef-gh`, End: `2026-10-08` | Reject ("Invalid date format") |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-EP-08** |
| **Test Scenario** | Verify that the audit log query module rejects queries where the Start Date is later than the End Date (FR-4 / EP). |
| **Preconditions** | 1. Administrator is viewing the Audit Trail Dashboard (`/admin/audit-logs`). |
| **Test Steps** | 1. Select Start Date: `2026-10-08`.<br>2. Select End Date: `2026-09-01`.<br>3. Click "Apply Filter". |
| **Input Data** | - Start Date: `2026-10-08`<br>- End Date: `2026-09-01` |
| **Expected Result** | System blocks the database query and displays error message: *"Invalid date filter: Start date cannot be later than end date."* Log grid remains unchanged. |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates inverted date partition P3 using Equivalence Partitioning (EP). |

---

## Feature 11: File Access / Download Expiration Duration [FR-2 / NFR-1]
- **Requirement:** When sharing a confidential document with agents, the administrator sets a secure access validity period between **1 and 30 days**. After the specified days, the download link automatically revokes access. Inputs of 0 days or exceeding 30 days must be rejected.
- **Boundary:** Valid Range = [1 day, 30 days].
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Configured Duration | Meaning | Expected Result |
| :--- | :--- | :--- |
| **0 days** | Below minimum boundary ($1 - 1$) | Reject ("Minimum validity is 1 day") |
| **1 day** | Minimum valid boundary ($1$) | Accept (Link valid for 24 hours) |
| **30 days** | Maximum valid boundary ($30$) | Accept (Link valid for 30 days) |
| **31 days** | Above maximum boundary ($30 + 1$) | Reject ("Maximum validity is 30 days") |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-08** |
| **Test Scenario** | Verify that the system blocks setting a file access expiration period exceeding 30 days during agent file allocation (FR-2 / NFR-1). |
| **Preconditions** | 1. Administrator is on the File Distribution settings screen.<br>2. File `quarterly_financials.txt` is selected for sharing. |
| **Test Steps** | 1. In the "Access Expiration (Days)" field, enter `31`.<br>2. Select target agents.<br>3. Click "Publish Distribution". |
| **Input Data** | - Access Expiration Days: `31` |
| **Expected Result** | System displays field validation error: *"Invalid validity period: File access duration must be between 1 and 30 days."* Form submission is blocked. |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates upper boundary value $30 + 1 = 31$ days using Boundary Value Analysis (BVA). |

---

## Feature 12: Batch Recipient Email Notification Limit [FR-2 / NFR-3]
- **Requirement:** When a new confidential file is distributed, the backend triggers automatic email notifications to allocated agents. To prevent SMTP server throttling and rate-limit drops, the notification dispatcher processes a maximum of **10 recipient email addresses** per outgoing SMTP transaction batch.
- **Boundary:** Range = [1 to 10 emails per SMTP batch].
- **Design Technique:** Boundary Value Analysis (BVA)

### Boundary Value Analysis Table
| Recipients in Notification Dispatch | Meaning | System Behavior |
| :--- | :--- | :--- |
| **9 recipients** | Below boundary ($10 - 1$) | Transmits in 1 single SMTP batch |
| **10 recipients** | Exact maximum boundary ($10$) | Transmits in 1 single SMTP batch |
| **11 recipients** | Just above boundary ($10 + 1$) | Splits into 2 separate SMTP batches (10 + 1) to respect rate limit |

### Detailed Test Case Specification
| Field | Details |
| :--- | :--- |
| **Test Case ID** | **TC-DLD-BVA-09** |
| **Test Scenario** | Verify that email notification batch dispatch correctly enforces the 10-recipient boundary threshold and queues surplus recipients into a secondary batch (FR-2 / NFR-3 / BVA). |
| **Preconditions** | 1. Administrator initiates distribution of `quarterly_audit.txt` to 11 authorized agents.<br>2. SMTP Notification Service is active. |
| **Test Steps** | 1. Confirm allocation of 11 agents.<br>2. Submit distribution workflow.<br>3. Inspect backend notification dispatcher logs. |
| **Input Data** | - Total Recipients: 11 agents (`agent1@org.com` to `agent11@org.com`) |
| **Expected Result** | Dispatcher packages first 10 recipients into Batch 1, dispatches immediately, and queues the 11th recipient into Batch 2, preventing SMTP rate limit rejection. |
| **Actual Result** | (To be completed during test execution) |
| **Status** | (Pass / Fail) |
| **Remarks** | Validates upper boundary $+ 1$ threshold ($10 + 1 = 11$) using Boundary Value Analysis (BVA). |

---

# ==============================================================================
# SUMMARY CONSOLIDATED TEST SUITE MATRIX (ALL 12 TEST CASES)
# ==============================================================================

| TC ID | Member | Requirement | Test Type | Input Condition / Boundary | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-DLD-EP-01** | Fahim Bin Zaman (3055) | FR-1 File Format | EP | Unsupported `.pdf` file format | Reject upload with format error | (Pass/Fail) |
| **TC-DLD-BVA-01** | Fahim Bin Zaman (3055) | FR-1 File Size | BVA | 50.1 MB ($50\text{ MB} + 0.1\text{ MB}$) | Reject (HTTP 413 Size limit exceeded) | (Pass/Fail) |
| **TC-DLD-BVA-02** | Fahim Bin Zaman (3055) | FR-2 Agent Batch | BVA | 21 agents ($20 + 1$) | Reject (Max 20 agents per batch) | (Pass/Fail) |
| **TC-DLD-EP-02** | Abid Ajmal (3133) | FR-3 Tracking Token | EP | Valid 36-char canonical UUID v4 string | Accept for steganographic embedding | (Pass/Fail) |
| **TC-DLD-BVA-03** | Abid Ajmal (3133) | FR-3 Image Resolution | BVA | $255 \times 255\text{ px}$ ($256 - 1$) | Reject (Below minimum resolution) | (Pass/Fail) |
| **TC-DLD-BVA-04** | Abid Ajmal (3133) | FR-3 Text Cover Words | BVA | 49 words ($50 - 1\text{ words}$) | Reject (Insufficient embedding capacity) | (Pass/Fail) |
| **TC-DLD-BVA-05** | Hojaifa (3181) | FR-5 BER Tolerance | BVA | Bit Error Rate = 5.1% ($5.0\% + 0.1\%$) | Reject recovery (Noise threshold exceeded) | (Pass/Fail) |
| **TC-DLD-EP-03** | Hojaifa (3181) | FR-5 Forensic Upload | EP | Unsupported `.zip` archive | Reject forensic scan | (Pass/Fail) |
| **TC-DLD-BVA-06** | Hojaifa (3181) | FR-5 Match Threshold | BVA | 80.0% match confidence score | Positive match attribution confirmed | (Pass/Fail) |
| **TC-DLD-EP-04** | Member 3023 | FR-4 Audit Date Filter | EP | Start Date after End Date | Reject filter with validation error | (Pass/Fail) |
| **TC-DLD-BVA-07** | Member 3023 | FR-2 Access Expiry Days | BVA | 31 days ($30 + 1\text{ days}$) | Reject (Max access duration 30 days) | (Pass/Fail) |
| **TC-DLD-BVA-08** | Member 3023 | NFR-3 Email Dispatch | BVA | 11 recipient emails ($10 + 1$) | Split into 2 batches (10 + 1) | (Pass/Fail) |
