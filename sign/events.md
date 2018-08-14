# Events

Adobe Sign capabilities can be incorporated into external applications by directly embedding the Adobe Sign user interface (UI) within these applications. Adobe Sign also supports sending events (status updates) to the third-party application pages so that the external application is aware of the actions that the user is performing with the Adobe Sign UI. These events are passed between the controller window and a receiver window running on different domains for event communication.

This page provides a guide to all the events supported by Adobe Sign.

## **System requirements**

To use the event framework within Adobe Sign, you need a browser that supports `postMessage`. See  [this page](https://developer.mozilla.org/en-US/docs/Web/API/Window.postMessage) to get a list of all supported browsers.

## **Supported events**

The following table lists the Adobe Sign supported UI events that can be embedded and presented within the UI of external applications.

### **Workflow events**

| **Event Type** | **Data** | **Description** |
| --- | --- | --- |
| `ESIGN` | NONE | This event gets fired after a user has successfully signed an agreement. |
| `REJECT` | NONE | This event is fired after a user rejects an agreement. |
| `PREFILL` | NONE | This event is fired after a user completes prefilling an agreement and sends it. |

### **Page Load events**

<table>
    <thead>
      <tr>
         <th>Event Type</th>
         <th>Data</th>
         <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
         <td><strong>'PAGE_LOAD'</strong></td>
         <td>
            <ul>
               <li>pageName: 'POST_SEND'</li>
               <li>apiAgreementId: ''</li>
            </ul>
         </td>
         <td>This event gets fired after a user has successfully signed an agreement. This event fires when an agreement has been successfully sent and the post send page has been loaded.</td>
      </tr>
      <tr>
         <td><strong>'PAGE_LOAD'</strong></td>
         <td>
            <ul>
               <li>pageName: 'DIGSIG_DOWNLOAD'</li>
            </ul>
         </td>
         <td>This is a special event that is fired for documents requiring Digital Signatures. This event fires when a user has completed all the required fields in a document and the page to download the document for Digital Signature gets loaded.</td>
      </tr>
      <tr>
         <td><strong>'PAGE_LOAD'</strong></td>
         <td>
            <ul>
               <li>pageName: 'AUTHORING'<br></li>
            </ul>
         </td>
         <td>This event fires when the form-field authoring page loads for an agreement.<br></td>
      </tr>
      <tr>
         <td>'PAGE_LOAD'</strong></td>
         <td>
            <ul>
               <li>pageName: 'DELEGATION'</li>
            </ul>
         </td>
         <td>This event fires when the page from which an agreement can be delegated gets loaded. The loading of the page does guarantee that delegation has or will actually occur.</td>
      </tr>
      <tr>
         <td><strong>'PAGE_LOAD'</strong></td>
         <td>
            <ul>
               <li>pageName: 'MANAGE'</li>
            </ul>
         </td>
         <td>This event fires when the manage page loads.</td>
      </tr>
      <tr>
         <td><strong>'PAGE_LOAD'</strong></td>
         <td>
            <ul>
               <li>pageName: 'LOGIN'</li>
            </ul>
         </td>
         <td>This event fires when the login page loads.</td>
      </tr>
   </tbody>
</table>

### **Session events**

<table>
    <thead>
      <tr>
         <th>Event Type</th>
         <th>Data</th>
         <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
         <td><strong>'ESIGN''SESSION_TIMEOUT'</strong></td>
         <td>
            <ul>
               <li>message:'PRE_SESSION_TIMEOUT'</li>
               <li>warningTimeMinutes: &lt;float&gt;</li>
               <li>expirationTimeMinutes: &lt;float&gt;</li>
            </ul>
         </td>
         <td>This event is triggered two seconds before session timeout dialogue is displayed to the user. The UI shows “Your session is about to expire" message to the user. The warningTimeMinutes and expirationTimeMinutes values correspond to the warning &amp; session timeout times in minutes.</td>
      </tr>
      <tr>
         <td><strong>'SESSION_TIMEOUT'</strong></td>
         <td>
            <ul>
               <li>message:'POST_SESSION_TIMEOUT'</li>
               <li>warningTimeMinutes: &lt;float&gt;</li>
               <li>expirationTimeMinutes: &lt;float&gt;</li>
            </ul>
         </td>
         <td>This event is triggered when the users' session times out.</td>
      </tr>
      <tr>
         <td><strong>'ERROR'</strong></td>
         <td>
            <ul>
               <li>message: &lt;varies&gt;</li>
            </ul>
         </td>
         <td>This event fires when an error dialog or an error page is displayed to the user. System Error: 500 or 503 is returned.</td>
      </tr>
   </tbody>
</table>

### **User action events**

<table>
    <thead>
      <tr>
         <th>Event Type</th>
         <th>Data</th>
         <th>Description</th>
      </tr>
    </thead>
   <tbody>
      <tr>
         <td><strong>'CLICK'</strong></td>
         <td>
            <ul>
               <li>pageName: 'POST_SEND' or 'POST_SIGN'</li>
               <li>target: 'MANAGE_LINK'</li>
               <li>url: ''</li>
               <li>apiAgreementId: ''</li>
            </ul>
         </td>
         <td>This event fires when the user clicks the "Manage this document" button in the post-send page. The URL data contains the full URL needed to bring up the manage page with the particular agreement selected. The apiAgreementId is the DocumentKey, used by client application making the API calls.</td>
      </tr>
      <tr>
         <td><strong>'CLICK'</strong></td>
         <td>
            <ul>
               <li>pageName: 'POST_SEND' or 'POST_SIGN'</li>
               <li>target: 'SEND_ANOTHER_LINK'</li>
            </ul>
         </td>
         <td>This event fires when the user clicks the "Send another document" button.</td>
      </tr>
   </tbody>
</table>

## **Using events**

In order to use the events fired by Adobe Sign, the external application should include a callback handler in the parent page that embeds the Adobe Sign application UI.

The following example depicts an event handler that can be placed in the parent page:

```js
functioneventHandler(e){
    if(e.origin =="https://secure.echosign.com"){
        console.log("Event from Adobe Sign!", JSON.parse(e.data));
    }else{
        console.log("Do not process this!");
    }
}
if(!window.addEventListener){
    window.attachEvent('onmessage', eventHandler);
}else{
    window.addEventListener('message', eventHandler,false);
}
```