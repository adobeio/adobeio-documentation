# Adobe I/O Runtime: Frequently Asked Questions

On this page: 
- [Getting Access ](#gettingaccess)
- [Supported Programming Languages](#supportedprogramminglanguages)
- [Price](#price)
- [Usage Quota](#usagequota)
- [Support (Zendesk)](#supportzendesk)

## Getting Access 
**How can I get access to I/O Runtime?**  
I/O Runtime is in private beta. You can sign up [here](https://adobeio.typeform.com/to/RWhT8Y) to get notified when it is general available.

## Supported Programming Languages 
**Which languages are supported in I/O Runtime?**  
For now, Adobe I/O Runtime only supports Node.js. We will add support for other languages as the platform matures.

## Price 
**What does it cost to use Adobe I/O Runtime?**  
Presently, Adobe I/O Runtime is free to use for Adobe enterprise customers and partners.

## Usage Quotas 
**What usage quotas are in place for Adobe I/O Runtime?**  
There are presently no usage quotas on Adobe I/O Runtime.

**Which limits are imposed onto actions?**  
All available limits (and the default values) are given in the [OpenWhisk documentation](https://github.com/apache/incubator-openwhisk/blob/master/docs/reference.md). Notable limits are the timeout for functions and the maximum payload that can get posted to a function.

Runtime exceptions to the OpenWhisk limits are as follows:

- Invocations per minute: **600**
- Triggers per minute: **600**
- Action size (including NPMs): **20MB**

## Support (Zendesk)
You can submit support request through our [Zendesk chanel](https://adobeio.zendesk.com).