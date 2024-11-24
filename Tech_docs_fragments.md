## Table of Contents 
1. User Manual (Fragment) 

2. Functional Requirements 

3. Text Localization Tool

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
By default, requisitioners cannot approve, inspect, or perform similar actions on their own requisitions.  
To allow this modify the <code>approvalStepsAllowedForRequisitioner.groovy</code> script.  
Script Location:
- Customer-specific: <code>./customers/<customerID>/workflow</code>
- Global: <code>./global/workflow</code>






__________________________
**Accounting Details**  
In this section, you can allocate costs across different cost objects and apply them to the entire purchase order. By default, the system prioritizes accounting details for the entire purchase order (if available). To override this and prioritize accounting details for individual items, set the configuration attribute jcataxxostObjects (boolean) to true (default is false).
___________________________
**Attachments**  
Attachments
You can attach documents to the purchase order by either dragging and dropping files or clicking the area labeled 'Drop xls, xlsx, doc, docx, pdf files here' to browse and upload the required files.

- To make an attachment visible to the supplier, select the Available for Suppliers checkbox below the file name.
- To delete an attachment, click the bin icon next to the file name.  

Note: If you attach a file but do not select the Available for Suppliers checkbox, the attachment is treated as an internal note. It will not impact the purchase order, and no changes will occur when you release the edited purchase order.
___________________________
**Available Actions**  
- Back: Located at the top left of the section, this button returns you to the previous screen.
- Release: Found at the top right of the section, this button applies and releases changes to the purchase order. You will be prompted to select a reason for the changes from a drop-down menu in a pop-up window, where you can also leave an optional comment. The list of reasons for purchase order changes is configured in the <code>poxxons.groovy</code> script under the XX Configuration Area.
- Prevent Sending to the Supplier: To prevent the edited purchase order from being sent to the supplier, select this checkbox.
Note: Access to this checkbox requires the <code>/purchxxanageSendToSupplier</code> permission, which can be assigned using the PROV Permission Editor.



