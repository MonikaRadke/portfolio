## Table of Contents 
1. User Manual (Fragment) 

2. Text Localization Tool (Fragment)

### 1. User Manual (Fragment): SES Overbooking Overview
**What is SES Overbooking?**  
SES (Service Entry Sheet) Overbooking occurs when the total value of service entries for a purchase order item exceeds the permitted limit. Validation is performed during SES confirmation. If the overbooking limit is exceeded, the system displays the following message:
*“Overbooking is not allowed. Please adjust your entries.”*

Is approval required for overbooking?  
Yes, in cases of overbooking, additional approval is required during the Confirmation and Approval steps.

---
#### Configuring Overbooking Behavior
Configuration Attribute:
<code>jcxxlog.purxxer.allowServiceEntryOverbooking</code> (boolean)

- Default Setting: <code>false</code> (overbooking is not allowed).
- To Enable Overbooking: Set the attribute to <code>true</code>.  

---
#### Overbooking Tolerance Limits  
If overbooking is disabled (default setting) but specific limits are needed, the <code>overbookingTolerance.groovy</code> script can be used to define them. This script:

- Can be tailored for individual customers or applied globally.
- Allows setting tolerance limits based on:  
  - Absolute value (fixed amount).
  - Relative percentage (percentage of the original value).  

Script Loctaion:

- Customer-specific: <code>./customers/<customerID>/serviceEntry</code>  
- Global use: <code>./global/serviceEntry</code>  

Criteria for Custom Limits:

- Product ID
- Classification Group
- Supplier ID

---
#### Approving SES Overbooking
When SES overbooking occurs, the system determines eligible approvers based on the following rules:

1. Eligibility Parameters:
   - Authority = User
   - Responsibility Type = Requisition
   - Level = CostObjectApproval
   - Customer = Requisition’s customer
   - Cost Object = Requisition’s cost object
   - Approval Limit > Requisitioner’s Spend Limit
2. Matching Requirements:
   - Cost Object in the Requisition aligns with Responsibility.
   - The currency of the Responsibility matches the PO currency.
3. Approver Status:
   - Only users with active accounts are considered.
   - The requisitioner is excluded from the list of approvers.  

Approver Selection:

- The first eligible user from the generated list has the authority to approve overbooking.
- A default approver (role: <code>ProcDefaultApprover</code>) steps in when no other approvers are available.
- If no approvers can be identified, the SES remains in the "released" status and does not proceed to "submitted."  

---
#### Requisitioner Limitations
By default, requisitioners cannot approve, inspect, or perform similar actions on their own requisitions. To enable this functionality, modify the <code>approvalStepsAllowedForRequisitioner.groovy</code> script.  
Script Location:
- Customer-specific: <code>./customers/<customerID>/workflow</code>
- Global: <code>./global/workflow</code>
---
#### Frequently Asked Questions (FAQ)
**1. Can I allow overbooking for certain purchase orders?**

Yes, you can enable overbooking system-wide by setting the <code>jxxtalog.purchaseOrder.allowServiceEntryOverbooking</code> attribute to <code>true</code>. For more control, define specific overbooking limits using the <code>overbookingTolerance.groovy</code> script. 

**2. What are overbooking tolerance limits?**  

Overbooking tolerance limits specify how much overbooking is allowed before the system flags an error or requires approval. These limits can be defined as:
- Absolute value (e.g., $500 over the PO amount).
- Relative percentage (e.g., 10% over the PO amount).  

**3. Where do I configure overbooking tolerance limits?**  

Use the <code>overbookingTolerance.groovy</code> script located in:
- <code>./customers/<customerID>/serviceEntry</code> (for customer-specific limits)
- <code>./global/serviceEntry</code> (for global limits)   

**4. Who approves overbooking cases?**  

The system selects approvers based on responsibility parameters, such as authority level, cost object, and approval limits. Only the first eligible user in the list can approve. If no eligible user is found, the default approver (role: <code>ProcDefaultApprover</code>) is assigned.  

**5. What happens if no approver is found?**  

If no approvers are identified, the SES remains in the "released" status and does not proceed to "submitted."  

**6. Can a requisitioner approve their own overbooking request?**  

By default, requisitioners are restricted from approving their own requests. This restriction can be lifted by modifying the <code>approvalStepsAllowedForRequisitioner.groovy</code> script.   

**8. Can overbooking tolerances differ by customer or product type?**  

Yes, tolerances can be customized by customer, product, supplier, or classification group within the <code>overbookingTolerance.groovy</code> script.    

**9. Can I add additional steps for overbooking approval?**  

Yes, modify the <code>approvalStepsAllowedForRequisitioner.groovy</code> script to define additional workflow steps globally or for specific customers.

---

### 2. Text Localization Tool (Fragment):

#### Overview

Text localization is the process of adapting content to meet the linguistic and cultural preferences of a specific locale. The **Text Localization Tool (TLT)** facilitates this process by enabling efficient management of translations and message updates across applications. With TLT, you can:

- Handle newly added messages.
- Modify existing messages.
- Add and edit translations for existing languages.
- Introduce new languages.
- Generate reports.
- Import and export translations.

#### Glossary

- TLT: Text Localization Tool.
- Default Language: The default language is English (en).
- Source Message Bundle (SoMB): The default messages used in Grails applications. These files (en.js) are located in <code>${xx}/reaxxules/${reaxxdule}/src/comxxents/${component}/i18n/en.js.</code> SoMB contains default messages (and Translation Keys) that appear on the UI when translations for a specific language are unavailable. Applications send the SoMB to TLT.
- Standard Message Bundle (SMB): Language-specific property files exported from TLT as a zip. These files contain translations for all supported languages and are stored in a separate repository (link). SMBs are included during application installation or runtime.
- Modified Message Bundle (MMB): Messages edited via TLT that differ from SMB. MMB overwrites SMB.
- Active Message Bundle (AMB): The final bundle sent back to applications, containing the combined SMB and MMB:
<code>SMB + MMB = AMB (MMB ≥ SMB)</code>
- Translation Key: A unique identifier for a message, following a specific naming convention.
- Dispute Tab: A section in TLT where newly added messages and conflicting messages (refer to Dispute Statuses) are displayed.
- Dispute Statuses: Automatically assigned by TLT:  
  - CONFLICT: Occurs when there is a mismatch among the Source Message (So), Standard Message (St), or English Translation (T). The conflict is resolved when all three values are equal:  
Conflict: <code>So ≠ St ∨ So ≠ T ∨ St ≠ T</code>
Resolution: <code>So = St = T</code>
  - NEW: Identifies new messages with only the Source Message and Translation Key provided.











