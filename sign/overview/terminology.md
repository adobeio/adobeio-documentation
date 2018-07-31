# Terminology

**Transient document**  
The document that is used to create an agreement or a widget. The document is first uploaded to Adobe Sign by the sender. This is referred to as _transient_ since it is available for use only for 7 days after the upload.

**Library template**  
A library template can be reused or repurposed multiple times. Adobe Sign supports two types of library templates:

- _Document templates:_ A document template is a reusable document that can be shared with other users in your account, allowing multiple users to send out the same document without needing to make any changes.
- _Form field templates:_ A form field template is a reusable field layer that can be applied to any document. Form field templates can also be shared with other users in your account. Form field templates are ideal in the following situations:
  * You have one field layout that works for multiple documents.
  * You have a document that can be sent in a number of different ways.
  * You need to revise a document&rsquo;s content, but the fields remain in the same place.

Instead of creating a new library document every time a document is updated, the same form field layer can be applied. Form field templates can be edited to facilitate changes in the arrangement of fields or field properties.

**Agreement**  
When a document is sent to recipients for signing or approval, an agreement is created. You can track the status and completion of an agreement using APIs.

**Widget**  
Widgets are hosted documents that can be signed by anyone who has access to them. They are ideal for signup sheets, waivers, or any document you need many people to access and sign online. See [Parental Consent](../scenarios/parental-consent.md) for an example.

**Mega Signing**  
The Mega Sign process allows you to send a document to hundreds of individuals at once. Each signer signs his/her own copy of the document and these individual agreements are returned to you. This process can be used to collect NDAs, HR documents, or permission slips.

**Sender**  
The person that initiates the transaction of sending an agreement for signing or approval.

**Recipient**  
The person that receives the agreement and signs or approves the agreement (depending on the role). The recipient can be a:

- _Signer_: Person who needs to sign the document.
- _Approver_: Person who needs to approve the document.
- _Delegator_: Person who needs to delegate to someone who needs to sign or approve the document.
- _CC:_ People who receive a signed copy of the agreement.

**Recipient set**  
A group of recipients where only one member of the group needs to take action, sign or approve depending on the role specified for the set. While using the Sign APIs, recipients are always specified as recipient sets.

**Routing order**  
Adobe Sign supports having multiple recipients and routing orders making it easy to collect signatures in the right order. The sequence for the transaction can be sequential, parallel, or hybrid. In sequential signing, a definite order is followed for signing or approving. In parallel workflow, every recipient will get the agreement simultaneously allowing them to sign or approve in any order. You can even have a combination of the two (hybrid routing).