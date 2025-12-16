# **Use the mailbox import and export APIs in Microsoft Graph (preview)**

06/26/2025

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

The mailbox import and export APIs in Microsoft Graph allow your application to import and export contents from Exchange Online mailboxes. Mailbox contents can be accessed as a collection of folders and items in a consistent format, without the need to manage the metadata or structure of each item type individually. These items can be exported as an opaque stream in full fidelity (you can't change the export stream). Full-fidelity exports ensure that when you import an item, Exchange recreates it with no loss of information.

These APIs support access to data in users' primary mailboxes and shared mailboxes on Exchange Online. Items can be imported to the same mailbox or a different one.

#### ) **Important**

The mailbox import and export APIs in Microsoft Graph are not designed for mailbox backup and restore. For mailbox backup and restore in Microsoft 365, see **Microsoft 365 Backup**.

### **How to use the mailbox import and export APIs**

The following steps allow your app to systematically export and import contents from Exchange mailboxes:

- 1. Get a list of mailboxes that belong to a particular user.
- 2. Discover the contents of the mailbox as a set of folders and items.
- 3. Export items from a mailbox.
- 4. Create or update mailbox folders.
- 5. Import an item into the same or a different mailbox.

| Use case                                           | REST resource                            | See also                 |
|----------------------------------------------------|------------------------------------------|--------------------------|
| Create, get, update, or delete a mailbox<br>folder | mailboxFolder                            | mailboxFolder<br>methods |
| Get one or more mailbox items                      | mailboxItem                              | mailboxItem methods      |
| Get delta for folders                              | mailboxFolder<br>mailboxFolder:<br>delta |                          |
| Get delta for items                                | mailboxItem                              | mailboxItem: delta       |
| Import or export mailboxes                         | mailbox                                  | mailbox methods          |
| Get a list of mailboxes that belong to a user      | exchangeSettings                         | List Exchange settings   |

## **Next steps**

Use the mailbox import and export APIs in Microsoft Graph to import and export contents from Exchange mailboxes. To learn more:

- Explore the resources and methods that are most helpful to your scenario.
- Try the API in the Graph Explorer.

## **Related content**

Import an Exchange mailbox item using the mailbox import and export APIs

# **mailbox resource type**

06/26/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Represents a user's mailbox.

Inherits from directoryObject.

### **Methods**

ノ **Expand table**

|  | Create import<br>session | mailboxItemImportSession | Create a session to import an Exchange mailbox<br>item. |
|--|--------------------------|--------------------------|---------------------------------------------------------|
|--|--------------------------|--------------------------|---------------------------------------------------------|

### **Properties**

ノ **Expand table**

| Property        | Type           | Description                                                                                                               |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------|
| deletedDateTime | DateTimeOffset | Date and time when this object was deleted. Always null when the<br>object isn't deleted. Inherited from directoryObject. |
| id              | String         | The unique identifier for the mailbox. Inherited from<br>directoryObject.                                                 |

## **Relationships**

| Relationship | Type                     | Description                               |
|--------------|--------------------------|-------------------------------------------|
| folders      | mailboxFolder collection | The collection of folders in the mailbox. |

### **JSON representation**

The following JSON representation shows the resource type.

```
JSON
{
 "@odata.type": "#microsoft.graph.mailbox",
 "deletedDateTime": "String (timestamp)",
 "id": "String (identifier)"
}
```

# **mailbox: createImportSession**

06/26/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Create a session to import an Exchange mailbox item that was exported using the exportItems API.

This API is available in the following national cloud deployments.

ノ **Expand table**

| Global service | US Government L4 | US Government L5 (DOD) | China operated by 21Vianet |
|----------------|------------------|------------------------|----------------------------|
|                |                  |                        |                            |

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school account)        | MailboxItem.ImportExport        | Not available.                   |
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxItem.ImportExport.All    | Not available.                   |

## **HTTP request**

HTTP

POST /admin/exchange/mailboxes/{mailboxId}/createImportSession

## **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this action returns a 200 OK response code and a mailboxItemImportSession in the response body.

## **Examples**

### **Request**

The following example shows how to create an import session. The opaque URL, returned in the **importUrl** property of the response, is preauthenticated and contains the appropriate authorization token for subsequent POST queries in the https://outlook.office365.com domain. That token expires by **expirationDateTime**. Don't customize this URL for subsequent POST operations.

| HTTP |  |  |
|------|--|--|
| POST |  |  |

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#microsoft.graph.mailboxItemImportSessi
on",
 "importUrl":
"https://outlook.office365.com/api/gbeta/Mailboxes('MBX:e0643f21@a7809c93')/import
Item?authtoken=eyJhbGciOiJSUzI1NiIsImtpZCI6IjFTeXQ1b",
 "expirationDateTime": "2024-10-17T19:00:48.1052906Z"
}
```

# **mailbox: exportItems**

06/26/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Export Exchange mailboxItem objects in full fidelity. This API exports each item as an opaque stream. The data stream isn't intended for parsing, but can be used to import an item back into an Exchange mailbox. For more information, see: Overview of the mailbox import and export APIs in Microsoft Graph (preview)

You can export up to 20 items in a single export request.

This API is available in the following national cloud deployments.

ノ **Expand table**

| Global service | US Government L4 | US Government L5 (DOD) | China operated by 21Vianet |
|----------------|------------------|------------------------|----------------------------|
|                |                  |                        |                            |

## **Permissions**

Delegated (personal Microsoft

account)

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

|                                    |                                 | ノ<br>Expand table                |  |
|------------------------------------|---------------------------------|----------------------------------|--|
| Permission type                    | Least privileged<br>permissions | Higher privileged<br>permissions |  |
| Delegated (work or school account) | MailboxItem.ImportExport        | Not available.                   |  |

Not supported. Not supported.

| Permission type | Least privileged<br>permissions | Higher privileged<br>permissions |
|-----------------|---------------------------------|----------------------------------|
| Application     | MailboxItem.ImportExport.All    | Not available.                   |

### **HTTP request**

HTTP

POST /admin/exchange/mailboxes/{mailboxId}/exportItems

### **Request headers**

#### ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |
| Content-Type  | application/json. Required.                                                  |

## **Request body**

In the request body, supply a JSON representation of the parameters.

The following table lists the parameters that are required when you call this action.

#### ノ **Expand table**

| Parameter | Type                 | Description                                                                                                                                                                                 |
|-----------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| itemIds   | String<br>collection | A collection of identifiers of mailboxItem objects to export. All identifiers in<br>the collection must be for items in the same mailbox. Maximum size of this<br>collection is 20 strings. |

## **Response**

If successful, this action returns a 200 OK response code and a collection of exportItemResponse objects in the response body.

## **Examples**

### **Request**

The following example exports two items present in the user's mailbox. The **itemIds** of the items to be exported are specified in the request body.

```
HTTP
HTTP
  POST
  https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a7809c9
  3/exportItems
  Content-type: application/json
  {
   "itemIds": [
   "EDSVrdi3lRAADmpnf1AAA=",
   "EDSVrdi3lRAAD45b7RAAA="
   ]
  }
```

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#Collection(microsoft.graph.exportItemR
esponse)",
 "value": [
 {
 "itemId": "EDSVrdi3lRAADmpnf1AAA=",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAEu4C+G",
 "data":
"VGhpcyBpcyBhIHRlc3QgZGF0YSB0aGF0IHdpbGwgYmUgY29udmVydGVkIHRvIGEgQmFzZTY0IHN0cmVhb
```

```
 },
 {
 "itemId": "EDSVrdi3lRAAD45b7RAAA=",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAD4pUax",
 "data":
"VGhpcyBpcyBhIHRlc3QgZGF0YSB0aGF0IHdpbGwgYmUgY29udmVydGVkIHRvIGEgQmFzZTY0IHN0cmVhb
Q=="
 }
 ]
}
```

# **mailboxItem resource type**

Article • 01/30/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Represents an item in a mailboxFolder. Items are Exchange mailbox items like message, task, event, contact, or note.

This resource supports delta query to track incremental additions, deletions, and updates, by providing a delta function. It also supports single-value and multi-value extended properties for filtering on custom data that isn't already exposed in the Microsoft Graph API metadata.

Inherits from outlookItem.

### **Methods**

#### ノ **Expand table**

| List<br>Get                         | mailboxItem<br>collection<br>mailboxItem | Get the mailboxItem collection within a specified<br>mailboxFolder in a mailbox.<br>Read the properties and relationships of a mailboxItem<br>object. |
|-------------------------------------|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| Get delta<br>Extended<br>properties | mailboxItem<br>collection                | Get a set of mailboxItem objects that have been added,<br>deleted, or updated in a specified mailboxFolder.                                           |
| Get single-value<br>property        | mailboxItem                              | Get mailbox items that contain a single-value extended<br>property by using \$expand or \$filter                                                      |

| Method                      | Return type | Description                                                                           |
|-----------------------------|-------------|---------------------------------------------------------------------------------------|
| Get multi-value<br>property | mailboxItem | Get a mailbox item that contains a multi-value extended<br>property by using \$expand |

### **Properties**

#### ノ **Expand table**

| Property<br>categories | Type<br>String | Description<br>The categories associated with the message. Inherited                                                                                                                                                                     |
|------------------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        | collection     | from outlookItem.                                                                                                                                                                                                                        |
| changeKey              | String         | The version of the item. Inherited from outlookItem.                                                                                                                                                                                     |
| createdDateTime        | DateTimeOffset | The date and time when the item was created. The<br>date and time information uses ISO 8601 format and is<br>always in UTC. For example, midnight UTC on Jan 1,<br>2021 is 2021-01-01T00:00:00Z<br>. Inherited from<br>outlookItem.      |
| id                     | String         | The unique identifier for the item. Inherited from<br>outlookItem.                                                                                                                                                                       |
| lastModifiedDateTime   | DateTimeOffset | The date and time when the item was last changed.<br>The date and time information uses ISO 8601 format<br>and is always in UTC. For example, midnight UTC on<br>Jan 1, 2021 is 2021-01-01T00:00:00Z<br>. Inherited from<br>outlookItem. |
| size                   | Int64          | The length of the item in bytes.                                                                                                                                                                                                         |
| type                   | String         | The message class ID of the item.                                                                                                                                                                                                        |

### **Relationships**

#### ノ **Expand table**

| Relationship<br>multiValueExtendedProperties | Type<br>multiValueLegacyExtendedProperty<br>collection | Description<br>The collection of multi<br>value extended |
|----------------------------------------------|--------------------------------------------------------|----------------------------------------------------------|
|                                              |                                                        | properties defined for<br>the mailboxItem.               |

| Relationship                  | Type                                            | Description                                                                              |
|-------------------------------|-------------------------------------------------|------------------------------------------------------------------------------------------|
| singleValueExtendedProperties | singleValueLegacyExtendedProperty<br>collection | The collection of single<br>value extended<br>properties defined for<br>the mailboxItem. |

### **JSON representation**

The following JSON representation shows the resource type.

```
JSON
{
 "@odata.type": "#microsoft.graph.mailboxItem",
 "categories": ["String"],
 "changeKey": "String",
 "createdDateTime": "String (timestamp)",
 "id": "String (identifier)",
 "lastModifiedDateTime": "String (timestamp)",
 "size": "Int64",
 "type": "String"
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **List items**

Article • 02/05/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Get the mailboxItem collection within a specified mailboxFolder in a mailbox.

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school<br>account)     | MailboxItem.Read                | Not available.                   |
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxItem.Read.All            | Not available.                   |

# **HTTP request**

**Optional query parameters**

HTTP

GET /admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/items

This method supports some of the OData query parameters to help customize the response. For general information, see OData query parameters.

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

### **Response**

If successful, this method returns a 200 OK response code and a collection of mailboxItem objects in the response body.

## **Examples**

### **Example 1: List items**

The following example shows how to get the mailbox items within a specified folder in a mailbox.

#### **Request**

The following example shows a request.

| HTTP                                                                                                                            |  |
|---------------------------------------------------------------------------------------------------------------------------------|--|
| HTTP                                                                                                                            |  |
| GET<br>https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a<br>7809c93/folders/NJWt2LeVEAAAIBDAAAAA==/items |  |

#### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders('NJWt2LeVEAAAIBDAAAAA==')/items",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zPIo\"",
 "id": "EDSVrdi3lRAAE9J-20AAA=",
 "createdDateTime": "2021-09-15T12:16:38Z",
 "lastModifiedDateTime": "2021-09-15T12:16:41Z",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zPIo",
 "categories": [],
 "type": "IPM.Note",
 "size": 71133
 },
 {
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zO5W\"",
 "id": "EDSVrdi3lRAAE9J-2zAAA=",
 "createdDateTime": "2021-09-15T11:06:36Z",
 "lastModifiedDateTime": "2021-09-15T11:06:40Z",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zO5W",
 "categories": [],
 "type": "IPM.Note",
 "size": 79968
 }
 ],
 "@odata.nextLink":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93/folders('NJWt2LeVEAAAIBDAAAAA==')/items?%24skip=10"
}
```

### **Example 2: List items using query parameter**

The following example uses the \$filter , \$select , and \$top query parameters. The \$filter parameter refines the results and returns only items with **createdDateTime** between 2021-08-21 and 2021-09-16 . The \$select parameter specifies to return only a subset of the properties of each item in the response, and the \$top parameter sets the page size of the result set to return only the top item in the mailbox under the specified folder.

### **Request**

The following example shows a request.

| HTTP |                                                                                                                                                                                                                               |
|------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      | HTTP<br>GET<br>https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a<br>7809c93/folders/Inbox/items?\$filter=createdDateTime ge 2021-08-21 and<br>createdDateTime lt 2021-09-16&\$select=type,size&\$top=1 |

#### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
```

```
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders('Inbox')/items",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAFOoqvk\"",
 "id": "EDSVrdi3lRAAE9J-2xAAA=",
 "type": "IPM.Note",
 "size": 91339
 }
 ],
 "@odata.nextLink":
```

"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780

```
9c93/folders('Inbox')/items?%24filter=createdDateTime+ge+2021-08-
21+and+createdDateTime+lt+2021-09-
16&%24select=type%2csize&%24top=1&%24skip=1"
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Get mailboxItem**

07/23/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Read the properties and relationships of a mailboxItem object.

This API is available in the following national cloud deployments.

ノ **Expand table**

| Global service | US Government L4 | US Government L5 (DOD) | China operated by 21Vianet |
|----------------|------------------|------------------------|----------------------------|
|                |                  |                        |                            |

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school account)        | MailboxItem.Read                | Not available.                   |
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxItem.Read.All            | Not available.                   |

### **HTTP request**

```
HTTP
```

```
GET
/admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/items/{mailboxItem
Id}
```

## **Optional query parameters**

This method supports some of the OData query parameters to help customize the response. For general information, see OData query parameters.

## **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this method returns a 200 OK response code and a mailboxItem object in the response body.

## **Examples**

### **Request**

The following example shows a request.

| HTTP |  |  |  |
|------|--|--|--|
| HTTP |  |  |  |

```
GET
https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0648f21@aab09c9
3/folders/AAMkAGVmMDEzM/items/AAMkAGI1AAAoZCfHAAA=
```

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-Type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e0643f21@
a7809c93/folders('Inbox')/items$entity",
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zPIo\"",
 "id": "EDSVrdi3lRAAE9J-20AAA=",
 "createdDateTime": "2021-09-15T12:16:38Z",
 "lastModifiedDateTime": "2021-09-15T12:16:41Z",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zPIo",
 "categories": [],
 "type": "IPM.Note",
 "size": 71133
}
```

# **mailboxItem: delta**

Article • 02/11/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Get a set of mailboxItem objects that have been added, deleted, or updated in a specified mailboxFolder.

A **delta** function call for items in a folder is similar to a GET request, except that by appropriately applying state tokens in one or more of these calls, you can query for incremental changes in the items in that folder. This approach allows you to maintain and synchronize a local store of a user's mailbox items without having to fetch the entire set of items from the server every time.

This API is available in the following national cloud deployments.

ノ **Expand table**

| Global  | US Government | US Government L5 | China operated by |
|---------|---------------|------------------|-------------------|
| service | L4            | (DOD)            | 21Vianet          |
|         |               |                  |                   |

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school<br>account)     | MailboxItem.Read                | Not available.                   |
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxItem.Read.All            | Not available.                   |

### **HTTP request**

HTTP

GET

/admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/items/delta

### **Query parameters**

Tracking changes in items incurs a round of one or more **delta** function calls. If you use any query parameter (other than \$deltaToken and \$skipToken ), you must specify it in the initial **delta** request. Microsoft Graph automatically encodes any specified parameters into the token portion of the nextLink or deltaLink URL provided in the response. You only need to specify any desired query parameters once upfront. In subsequent requests, simply copy and apply the nextLink or deltaLink URL from the previous response, as that URL already includes the encoded, desired parameters.

ノ **Expand table**

| \$deltaToken | A state token returned in the deltaLink URL of the previous delta function call for<br>the same item collection, which indicates the completion of that round of change<br>tracking. Save and apply the entire deltaLink URL including this token in the first |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              | request of the next round of change tracking for that collection.                                                                                                                                                                                              |
| \$skipToken  | A state token returned in the nextLink URL of the previous delta function call,<br>indicating further changes are available to be tracked in the same item collection.                                                                                         |

### **OData query parameters**

- You can use the \$select query parameter to specify only the properties you need for best performance. The **id** property is always returned.
- This delta query supports the \$select and \$top query parameters for items.
- Limited support exists for \$filter and \$orderby :
  - The only supported \$filter expresssions are \$filter=receivedDateTime+ge+ {value} and \$filter=receivedDateTime+gt+{value} .
  - The only supported \$orderby expression is \$orderby=receivedDateTime+desc . If you don't include an \$orderby expression, the return order isn't guaranteed.
- The \$search query parameter isn't supported.

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |
| Prefer        | odata.maxpagesize={x}. Optional.                                             |

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this function returns a 200 OK response code and a collection of mailboxItem objects in the response body.

### **Examples**

### **Request**

The following example shows how to make a single **delta** function call and limit the maximum number of items in the response body to two.

To track changes in the items in a folder, you make one or more **delta** function calls to get the set of incremental changes since the last delta query.

For an example that shows a round of delta query calls, see Get incremental changes to items in a folder.

| HTTP                                                                                                                                                              |  |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|--|
| HTTP                                                                                                                                                              |  |
| GET<br>https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a<br>7809c93/folders/AAMkAGUwNjQ4ZyTAAA=/items/delta<br>Prefer: odata.maxpagesize=2 |  |

### **Response**

If the request is successful, the response includes a state token that is either a \$skipToken (in an **@odata.nextLink** response header) or a \$deltaToken (in an **@odata.deltaLink** response header). Respectively, they indicate whether you should continue with the round or you completed getting all the changes for that round.

The following example shows a \$skipToken in an **@odata.nextLink** response header.

**Note:** The response object shown here might be shortened for readability.

HTTP

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-length: 337
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#Collection(mailboxItem)",
 "@odata.nextLink":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93/folders/AAMkAGUwNjQ4ZyTAAA=/items/delta?$skiptoken={_skipToken_}",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAFR+6ZT\"",
 "createdDateTime": "2021-10-19T06:30:30Z",
 "lastModifiedDateTime": "2021-10-19T07:17:06Z",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAFR+6ZT",
 "categories": [],
 "type": "IPM.Note",
 "size": 75329,
```

#### } ] }

## **Related content**

- Use delta query to track changes in Microsoft Graph data
- Get incremental changes to folders in a mailbox

### **Feedback**

**Was this page helpful? Yes No**

Provide product feedback

# **Get singleValueLegacyExtendedProperty**

Article • 03/17/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

#### U **Caution**

Outlook tasks is deprecated and stopped returning data on August 10, 2022. Update existing apps that use this feature with Outlook tasks.

You can get a single resource instance expanded with a specific extended property, or a collection of resource instances that include extended properties matching a filter.

Using the query parameter \$expand allows you to get the specified resource instance expanded with a specific extended property. Use a \$filter and eq operator on the **id** property to specify the extended property. This is currently the only way to get the singleValueLegacyExtendedProperty object that represents an extended property.

To get resource instances that have certain extended properties, use the \$filter query parameter and apply an eq operator on the **id** property. In addition, for numeric extended properties, apply one of the following operators on the **value** property: eq , ne , ge , gt , le , or lt . For string-typed extended properties, apply a contains , startswith , eq , or ne operator on **value**.

Filtering the string name ( Name ) in the **id** of an extended property is case-sensitive. Filtering the **value** property of an extended property is case-insensitive.

The following user resources are supported:

- calendar
- contact
- contactFolder
- event

- message
- Outlook task
- Outlook task folder

As well as the following group resources:

- group calendar
- group event
- group post

See Extended properties overview for more information about when to use open extensions or extended properties, and how to specify extended properties.

This API is available in the following national cloud deployments.

#### ノ **Expand table**

| Global  | US Government | US Government L5 | China operated by |
|---------|---------------|------------------|-------------------|
| service | L4            | (DOD)            | 21Vianet          |
|         |               |                  |                   |

### **Permissions**

Depending on the resource you're getting the extended property from and the permission type (delegated or application) you request, the permission specified in the following table is the minimum required to call this API. To learn more, including how to choose permissions, see Permissions.

ノ **Expand table**

| Supported<br>resource | Delegated (work or<br>school account) | Delegated (personal<br>Microsoft account) | Application    |
|-----------------------|---------------------------------------|-------------------------------------------|----------------|
| calendar              | Calendars.Read                        | Calendars.Read                            | Calendars.Read |
| contact               | Contacts.Read                         | Contacts.Read                             | Contacts.Read  |
| contactFolder         | Contacts.Read                         | Contacts.Read                             | Contacts.Read  |
| event                 | Calendars.Read                        | Calendars.Read                            | Calendars.Read |
| group calendar        | Group.Read.All                        | Not supported                             | Not supported  |
| group event           | Group.Read.All                        | Not supported                             | Not supported  |

| Supported<br>resource  | Delegated (work or<br>school account) | Delegated (personal<br>Microsoft account) | Application    |
|------------------------|---------------------------------------|-------------------------------------------|----------------|
| group post             | Group.Read.All                        | Not supported                             | Group.Read.All |
| mailFolder             | Mail.Read                             | Mail.Read                                 | Mail.Read      |
| message                | Mail.Read                             | Mail.Read                                 | Mail.Read      |
| Outlook task           | Tasks.Read                            | Tasks.Read                                | Not supported  |
| Outlook task<br>folder | Tasks.Read                            | Tasks.Read                                | Not supported  |

### **HTTP request**

### **GET a resource instance expanded with an extended property that matches a filter**

Get a resource instance expanded with the extended property which matches a filter on the **id** property. Make sure you apply URL encoding to the space characters in the filter string.

Get a **message** instance:

```
GET /me/messages/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/messages/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/mailFolders/{id}/messages/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

HTTP

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **mailFolder** instance:

```
GET /me/mailFolders/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/mailFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **event** instance:

#### HTTP

```
GET /me/events/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/events/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get a **calendar** instance:

HTTP

```
GET /me/calendars/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/calendars/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get a **contact** instance:

HTTP

```
GET /me/contacts/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/contacts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/contactFolders/{id}/contacts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}/contacts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **contactFolder** instance:

HTTP

```
GET /me/contactfolders/{id}?$expand=singleValueExtendedProperties($filter=id
eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTask** instance:

```
HTTP
GET /me/outlook/tasks/{id}?$expand=singleValueExtendedProperties($filter=id
eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/tasks/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskFolders/{id}/tasks/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

```
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET
/users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks
```

```
/{id}?$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTaskFolder** instance:

HTTP

```
GET /me/outlook/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a group **event** instance:

HTTP

```
GET /groups/{id}/events/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

Get a group **post** instance:

```
GET /groups/{id}/threads/{id}/posts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /groups/{id}/conversations/{id}/threads/{id}/posts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

### **GET resource instances that include numeric extended properties matching a filter**

Get instances of a supported resource that have a numeric extended property matching a filter. The filter uses an eq operator on the **id** property, and one of the following operators on the **value** property: eq , ne , ge , gt , le , or lt . Make sure you apply Make sure you apply URL encoding to the following characters in the filter string - colon, forward slash, and space.

The following syntax lines show a filter that uses an eq operator on the id, and another eq operator on the property value. You can substitute the eq operator on the **value** by any one of the other operators ( ne , ge , gt , le , or lt ) that apply to numeric values.

Get **message** instances:

| GET /me/messages?\$filter=singleValueExtendedProperties/Any(ep: ep/id eq |
|--------------------------------------------------------------------------|
| '{id_value}' and ep/value eq '{property_value}')                         |
| GET /users/{id userPrincipalName}/messages?                              |
| \$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and |
| ep/value eq '{property_value}')                                          |
| GET /me/mailFolders/{id}/messages?                                       |
| \$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and |
| ep/value eq '{property_value}')                                          |

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **mailFolder** instances:

HTTP

GET /me/mailFolders?\$filter=singleValueExtendedProperties/Any(ep: ep/id eq

```
GET /users/{id|userPrincipalName}/mailFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get **event** instances:

#### HTTP

```
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get **calendar** instances:

#### HTTP

```
GET /me/calendars?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/calendars?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **contact** instances:

```
GET /me/contacts?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/contacts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/contactFolders/{id}/contacts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}/contacts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **contactFolder** instances:

HTTP

```
GET /me/contactfolders?$filter=singleValueExtendedProperties/Any(ep: ep/id
eq '{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/contactFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTask** instance:

HTTP

```
GET /me/outlook/tasks?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
```

```
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/outlook/taskFolders/{id}/tasks?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders/{id}/tasks?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET
/users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks
?$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

HTTP

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTaskFolder** instance:

```
GET /me/outlook/taskFolders?$filter=singleValueExtendedProperties/Any(ep:
ep/id eq '{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/outlook/taskGroups/{id}/taskFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get group **event** instances:

```
HTTP
```

```
GET /groups/{id}/events?$filter=singleValueExtendedProperties/Any(ep: ep/id
eq '{id_value}' and ep/value eq '{property_value}')
```

Get group **post** instances:

```
HTTP
GET /groups/{id}/threads/{id}/posts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /groups/{id}/conversations/{id}/threads/{id}/posts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

### **GET resource instances with string-typed extended properties matching a filter**

Get instances of the **message** or **event** resource that have a string-typed extended property matching a filter. The filter uses an eq operator on the **id** property, and one of the following operators on the **value** property: contains , startswith , eq , or ne . Make sure you apply URL encoding to the following characters in the filter string - colon, forward slash, and space.

Get **message** instances:

```
HTTP
GET /me/messages?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and contains(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
contains(ep/value, '{property_value}'))
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
contains(ep/value, '{property_value}'))
GET /me/messages?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and startswith(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
startswith(ep/value, '{property_value}'))
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
startswith(ep/value, '{property_value}'))
```

```
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/messages?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value ne '{property_value}')
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value ne '{property_value}')
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value ne '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **event** instances:

HTTP

```
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and contains(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
contains(ep/value, '{property_value}'))
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and startswith(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
startswith(ep/value, '{property_value}'))
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value ne '{property_value}')
GET /users/{id|userPrincipalName}/events?
```

\$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id\_value}' and ep/value ne '{property\_value}')

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get group **event** instances:

| HTTP                                                                                                                                                                                       |  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--|
| GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id<br>eq '{id_value}' and contains(ep/value, '{property_value}'))                                                |  |
| GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id                                                                                                               |  |
| eq '{id_value}' and startswith(ep/value, '{property_value}'))<br>GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id                                              |  |
| eq '{id_value}' and ep/value eq '{property_value}')<br>GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id<br>eq '{id_value}' and ep/value ne '{property_value}') |  |
|                                                                                                                                                                                            |  |

### **Path parameters**

ノ **Expand table**

| id_value       | String | The ID of the extended property to match. It must follow one of the<br>supported formats. See Outlook extended properties overview for more                                                                                   |
|----------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| property_value | String | information. Required.<br>The value of the extended property to match. Required where listed in                                                                                                                               |
|                |        | the HTTP request section above. If {property_value} is not a string, make<br>sure you explicitly cast ep/value to the appropriate Edm data type when<br>comparing it with {property_value}. See request 4 below for examples. |

### **Request headers**

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this method returns a 200 OK response code.

### **GET resource instance using \$expand**

The response body includes an object representing the requested resource instance, expanded with the matching singleValueLegacyExtendedProperty object.

### **GET resource instances that contain an extended property matching a filter**

The response body includes one or more objects representing the resource instances that contain a matching extended property. The response body does not include the extended property.

## **Example**

### **Request 1**

The first example gets and expands the specified message by including a single-value extended property. The filter returns the extended property that has its **id** matching the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color (with URL encoding removed here for ease of reading).

| HTTP                                                                            |  |
|---------------------------------------------------------------------------------|--|
|                                                                                 |  |
| msgraph                                                                         |  |
| GET<br>https://graph.microsoft.com/beta/me/messages/AAMkAGE1M2_bs88AACHsLqWAAA= |  |

### **Response 1**

The response body includes all the properties of the specified message and extended property returned from the filter.

Note: The **message** object shown here is truncated for brevity. All of the properties will be returned from an actual call.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#Me/messages/$entity",
 "@odata.id": "https://graph.microsoft.com/beta/users('ddfcd489-628b-
40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-
709fad664b77')/messages('AAMkAGE1M2_bs88AACHsLqWAAA=')",
 "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AACbyS4H\"",
 "id": "AAMkAGE1M2_bs88AACHsLqWAAA=",
 "subject": "RE: Talk about emergency prep",
 "sender": {
 "emailAddress": {
 "name": "Christine Irwin",
 "address": "christine@contoso.com"
 }
 },
 "from": null,
 "toRecipients": [
 {
 "emailAddress": {
 "name": "Christine Irwin",
 "address": "christine@contoso.com"
 }
 }
 ],
 "singleValueExtendedProperties": [
 {
 "id": "String {66f5a359-4659-4830-9070-00047ec6ac6e} Name
Color",
 "value": "Green"
 }
 ]
}
```

### **Request 2**

The second example gets messages that have the string-typed single-value extended property specified in the filter. The filter looks for the extended property that has:

- Its **id** equal to the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color (with URL encoding removed here for ease of reading).
- Its **value** equal to the string Green .

```
HTTP
```

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties%2FAny(ep%3A%20ep%2Fid%20eq%20'String%2
0{66f5a359-4659-4830-9070-
00047ec6ac6e}%20Name%20Color'%20and%20ep%2Fvalue%20eq%20'Green')
```

#### **Response 2**

A successful response is indicated by an HTTP 200 OK response code, and the response body includes all the properties of the messages that have the extended property matching the filter. The response body is similar to the response from getting a message collection. The response does not include the matching extended property.

### **Request 3**

The third example gets messages that have the string-typed single-value extended property specified in the filter. The filter looks for the extended property that has:

- Its **id** equal to the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color (with URL encoding removed here for ease of reading).
- Its **value** containing the string green .

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties/any(ep:ep/Id eq 'String {66f5a359-
4659-4830-9070-00047ec6ac6e} Name Color' and contains(ep/Value, 'green'))
```

HTTP

A successful response is indicated by an HTTP 200 OK response code, and the response body includes all the properties of the messages that have the extended property matching the filter. For example, a message that has a single-value extended property with the **id** equal to the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color , and the **value** Light green , would match the filter and be included in the response.

The response body is similar to the response from getting a message collection. The response does not include the matching extended property.

### **Request 4**

The next 2 examples show how to get messages that have non-string typed single-value extended properties. For ease of reading, they do not include the necessary URL encoding.

The following example shows a filter that looks for the extended property that has:

- Its **id** matching the string CLSID {00062008-0000-0000-C000-000000000046} Name ConnectorSenderGuid .
- Its **value** being the GUID b9cf8971-7d55-4b73-9ffa-a584611b600b . To compare the property value with a GUID, cast ep/value to Edm.Guid .

HTTP

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties/any(ep:ep/id eq 'CLSID {00062008-0000-
0000-C000-000000000046} Name ConnectorSenderGuid' and cast(ep/value,
Edm.Guid) eq (b9cf8971-7d55-4b73-9ffa-a584611b600b))
```

The next example shows a filter that looks for the extended property that has:

- Its **id** matching the string Integer {66f5a359-4659-4830-9070-00047ec6ac6e} Name Pallete .
- Its **value** equal to the integer 12. To compare the property value with an integer, cast ep/value to Edm.Int32 .

HTTP

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties/any(ep:ep/id eq 'Integer {66f5a359-
```

#### **Response 4**

For each of the preceding 2 examples, a successful response is indicated by an HTTP 200 OK response code, and the response body includes all the properties of the messages that have the extended property matching the corresponding filter. The response body is similar to the response from getting a message collection. The response does not include the matching extended property.

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Get multiValueLegacyExtendedProperty**

Article • 03/17/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

#### U **Caution**

Outlook tasks is deprecated and stopped returning data on August 10, 2022. Update existing apps that use this feature with Outlook tasks.

Get a resource instance that contains a multi-value extended property by using \$expand .

Using the query parameter \$expand allows you to get the specified instance expanded with the indicated extended property. This is currently the only way to get the multiValueLegacyExtendedProperty object that represents an extended property.

The following user resources are supported:

- calendar
- contact
- contactFolder
- event
- mailFolder
- message
- Outlook task
- Outlook task folder

As well as the following group resources:

- group calendar
- group event
- group post

See Extended properties overview for more information about when to use open

This API is available in the following national cloud deployments.

#### ノ **Expand table**

| Global  | US Government | US Government L5 | China operated by |
|---------|---------------|------------------|-------------------|
| service | L4            | (DOD)            | 21Vianet          |
|         |               |                  |                   |

### **Permissions**

Depending on the resource you're getting the extended property from and the permission type (delegated or application) you request, the permission specified in the following table is the minimum required to call this API. To learn more, including how to choose permissions, see Permissions.

ノ **Expand table**

| calendar                               | Calendars.Read           | Calendars.Read           | Calendars.Read                 |
|----------------------------------------|--------------------------|--------------------------|--------------------------------|
| contact                                | Contacts.Read            | Contacts.Read            | Contacts.Read                  |
| contactFolder                          | Contacts.Read            | Contacts.Read            | Contacts.Read                  |
| event                                  | Calendars.Read           | Calendars.Read           | Calendars.Read                 |
| group calendar                         | Group.Read.All           | Not supported            | Not supported                  |
| group event                            | Group.Read.All           | Not supported            | Not supported                  |
| group post                             | Group.Read.All           | Not supported            | Group.Read.All                 |
| mailFolder                             | Mail.Read                | Mail.Read                | Mail.Read                      |
| message                                | Mail.Read                | Mail.Read                | Mail.Read                      |
| Outlook task<br>Outlook task<br>folder | Tasks.Read<br>Tasks.Read | Tasks.Read<br>Tasks.Read | Not supported<br>Not supported |

## **HTTP request**

Get a resource instance expanded with the extended property which matches a filter on the **id** property. Make sure you apply URL encoding to the space characters in the filter string.

Get a **message** instance:

HTTP GET /me/messages/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}') GET /users/{id|userPrincipalName}/messages/{id}? \$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}') GET /me/mailFolders/{id}/messages/{id}? \$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}')

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **mailFolder** instance:

HTTP GET /me/mailFolders/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}') GET /users/{id|userPrincipalName}/mailFolders/{id}? \$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}')

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **event** instance:

HTTP

```
GET /me/events/{id}?$expand=multiValueExtendedProperties($filter=id eq
'{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get a **calendar** instance:

| HTTP                                                                        |
|-----------------------------------------------------------------------------|
|                                                                             |
| GET /me/calendars/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq |
| '{id_value}')                                                               |
| GET /users/{id userPrincipalName}/calendars/{id}?                           |
| \$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')          |

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **contact** instance:

#### HTTP

```
GET /me/contacts/{id}?$expand=multiValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/contacts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /me/contactFolders/{id}/contacts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}/contacts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **contactFolder** instance:

#### HTTP

```
GET /me/contactfolders/{id}?$expand=multiValueExtendedProperties($filter=id
eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get an **outlookTask** instance:

| HTTP                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GET /me/outlook/tasks/{id}?\$expand=multiValueExtendedProperties(\$filter=id<br>eq '{id_value}')<br>GET /users/{id userPrincipalName}/outlook/tasks/{id}?<br>\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')<br>GET /me/outlook/taskFolders/{id}/tasks/{id}?<br>\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')<br>GET /users/{id userPrincipalName}/outlook/taskFolders/{id}/tasks/{id}?<br>\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')<br>GET /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}?<br>\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')<br>GET<br>/users/{id userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks<br>/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}') |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get an **outlookTaskFolder** instance:

HTTP

```
GET /me/outlook/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a group **event** instance:

HTTP

```
GET /groups/{id}/events/{id}?$expand=multiValueExtendedProperties($filter=id
eq '{id_value}')
```

Get a group **post** instance:

HTTP

```
GET /groups/{id}/threads/{id}/posts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /groups/{id}/conversations/{id}/threads/{id}/posts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

## **Path parameters**

ノ **Expand table**

| Parameter | Type   | Description                                                          |
|-----------|--------|----------------------------------------------------------------------|
| id_value  | String | The ID of the extended property to match. It must follow one of the  |
|           |        | supported formats. See Outlook extended properties overview for more |

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

### **Response**

If successful, this method returns a 200 OK response code.

The response body includes an object representing the requested resource instance, expanded with the matching multiValueLegacyExtendedProperty object.

## **Example**

#### **Request**

This example gets and expands the specified event by including a multi-value extended property. The filter returns the extended property that has its **id** matching the string StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation (with URL encoding removed here for ease of reading).

```
HTTP
```

GET

```
https://graph.microsoft.com/beta/me/events('AAMkAGE1M2_bs88AACbuFiiAAA=')?
$expand=multiValueExtendedProperties($filter=id%20eq%20'StringArray%20{66f5a
359-4659-4830-9070-00050ec6ac6e}%20Name%20Recreation')
```

#### **Response**

The response body includes all the properties of the specified event and extended property returned from the filter.

Note: The **event** object shown here is truncated for brevity. All of the properties will be returned from an actual call.

HTTP

```
HTTP/1.1 200 OK
Content-type: application/json
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#Me/events/$entity",
 "@odata.id": "https://graph.microsoft.com/beta/users('ddfcd489-628b-
40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-
709fad664b77')/events('AAMkAGE1M2_bs88AACbuFiiAAA=')",
 "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAm8k15A==\"",
 "id": "AAMkAGE1M2_bs88AACbuFiiAAA=",
 "start": {
 "dateTime": "2015-11-26T17:00:00.0000000",
 "timeZone": "UTC"
 },
 "end": {
 "dateTime": "2015-11-30T05:00:00.0000000",
 "timeZone": "UTC"
 },
 "organizer": {
 "emailAddress": {
 "name": "Christine Irwin",
 "address": "christine@contoso.com"
 }
 },
 "multiValueExtendedProperties": [
 {
 "id": "StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name
Recreation",
 "value": [
 "Food",
 "Hiking",
 "Swimming"
 ]
 }
 ]
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **mailboxFolder resource type**

Article • 01/30/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Represents a folder in a user's mailbox, such as inbox, drafts, or other user created folders. Folders can contain various mailbox items like messages, events, contacts, other Outlook items, and child folders.

This resource supports delta query to track incremental additions, deletions, and updates, by providing a delta function. It also supports single-value and multi-value extended properties for storing and accessing custom data that isn't already exposed in the Microsoft Graph API metadata.

### **Methods**

ノ **Expand table**

| Method    | Return type                 | Description                                                                                              |
|-----------|-----------------------------|----------------------------------------------------------------------------------------------------------|
| List      | mailboxFolder<br>collection | Get all the mailboxFolder objects in the specified<br>mailbox, including any search folders.             |
| Create    | mailboxFolder               | Create a new mailboxFolder or child mailboxFolder in<br>a user's mailbox.                                |
| Get       | mailboxFolder               | Read the properties and relationships of a<br>mailboxFolder object.                                      |
| Update    | mailboxFolder               | Update mailboxFolder properties such as the<br>displayName within a mailbox.                             |
| Delete    | None                        | Delete a mailboxFolder or a child mailboxFolder within<br>a mailbox.                                     |
| Get delta | mailboxFolder<br>collection | Get a set of mailboxFolder objects that have been<br>added, deleted, or removed from the user's mailbox. |

| List items in folder<br>Extended<br>properties | mailboxItem<br>collection | Get the mailboxItem collection within a specified<br>mailboxFolder in a mailbox.                   |
|------------------------------------------------|---------------------------|----------------------------------------------------------------------------------------------------|
| Create single<br>value property                | mailboxFolder             | Create one or more single-value extended properties<br>in a new or existing mailbox folder.        |
| Get single-value<br>property                   | mailboxFolder             | Get mailbox folders that contain a single-value<br>extended property by using \$expand or \$filter |
| Create multi-value<br>property                 | mailboxFolder             | Create one or more multi-value extended properties in<br>a new or existing mailbox folder.         |
| Get multi-value<br>property                    | mailboxFolder             | Get a mailbox folder that contains a multi-value<br>extended property by using \$expand            |

### **Properties**

#### ノ **Expand table**

| Property         | Type   | Description                                                                                                                                                                                                             |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| childFolderCount | Int32  | The number of immediate child folders in the current folder.                                                                                                                                                            |
| displayName      | String | The display name of the folder.                                                                                                                                                                                         |
| id               | String | The unique identifier for the folder.                                                                                                                                                                                   |
| parentFolderId   | String | The unique identifier for the parent folder of this folder.                                                                                                                                                             |
| parentMailboxUrl | String | The routing link to the actual underlying mailbox where the folder<br>physically resides. The folder can be accessed using GET<br>{parentMailboxUrl}/folders/{id} , which treats the entire URL as an<br>opaque string. |
|                  |        | This method is especially important when auto-expanding archiving<br>is enabled for a user's in-place archive mailbox. The user's archive<br>content can span across multiple mailboxes in such scenarios.              |
| totalItemCount   | Int32  | The number of items in the folder.                                                                                                                                                                                      |

type String Describes the folder class type.

## **Relationships**

ノ **Expand table**

| Relationship                  | Type                                            | Description                                                                                |
|-------------------------------|-------------------------------------------------|--------------------------------------------------------------------------------------------|
| childFolders                  | mailboxFolder collection                        | The collection of child<br>folders in this folder.                                         |
| items                         | mailboxItem collection                          | The collection of items<br>in this folder.                                                 |
| multiValueExtendedProperties  | multiValueLegacyExtendedProperty<br>collection  | The collection of multi<br>value extended<br>properties defined for<br>the mailboxFolder.  |
| singleValueExtendedProperties | singleValueLegacyExtendedProperty<br>collection | The collection of single<br>value extended<br>properties defined for<br>the mailboxFolder. |

## **JSON representation**

The following JSON representation shows the resource type.

```
JSON
{
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "displayName": "String",
 "childFolderCount": "Int32",
 "id": "String (identifier)",
 "parentFolderId": "String",
 "parentMailboxUrl": "String",
 "totalItemCount": "Int32",
 "type": "String"
}
```

**Yes No**

# **Feedback**

**Was this page helpful?**

Provide product feedback

# **List folders**

Article • 02/05/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Get all the mailboxFolder objects in the specified mailbox, including any search folders.

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Delegated (work or school<br>account)     | permissions<br>MailboxFolder.Read | permissions<br>MailboxFolder.ReadWrite |
|-------------------------------------------|-----------------------------------|----------------------------------------|
| Delegated (personal Microsoft<br>account) | Not supported.                    | Not supported.                         |
| Application                               | MailboxFolder.Read.All            | MailboxFolder.ReadWrite.All            |

## **HTTP request**

HTTP

GET /admin/exchange/mailboxes/{mailboxId}/folders

**Optional query parameters**

This method supports some of the OData query parameters to help customize the response. For general information, see OData query parameters.

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

### **Response**

If successful, this method returns a 200 OK response code and a collection of mailboxFolder objects in the response body.

## **Examples**

### **Example 1: List folders**

The following example shows how to get the **mailboxFolder** collection directly under the root folder of the user's mailbox.

#### **Request**

The following example shows a request.

| HTTP |                                                                          |
|------|--------------------------------------------------------------------------|
| HTTP | https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a |
| GET  | 7809c93/folders                                                          |

#### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "id": "EDSVrdi3lRAAACgfQBAAA=",
 "displayName": "Archive",
 "parentFolderId": "NJWt2LeVEAAAIBCAAAAA==",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93",
 "childFolderCount": 0,
 "totalItemCount": 2,
 "type": "IPF.Note"
 },
 {
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "id": "NJWt2LeVEAAAIBDQAAAA==",
 "displayName": "Calendar",
 "parentFolderId": "NJWt2LeVEAAAIBCAAAAA==",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93",
 "childFolderCount": 5,
 "totalItemCount": 6,
 "type": "IPF.Appointment"
 }
 ],
 "@odata.nextLink":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93/folders?%24skip=10"
}
```

### **Example 2: List folders with query parameters**

The following example uses the \$filter , \$select , and \$top query parameters. The \$filter parameter refines the results and returns only folders of **type** IPF.Appointment . The \$select parameter is specified to return only the **displayName** and **type** properties, and the \$top parameter sets the page size of the result set to return the first five folders in the mailbox.

#### **Request**

The following example shows a request.

| HTTP                                                                                                                                                                       |  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--|
| GET<br>https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a<br>7809c93/folders?\$filter=type eq<br>'IPF.Appointment'&\$select=displayName,type&\$top=5 |  |

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

HTTP

```
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "id": "NJWt2LeVEAAAIBDQAAAA==",
 "displayName": "Calendar",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93",
 "type": "IPF.Appointment"
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Create mailboxFolder**

Article • 02/05/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Create a new mailboxFolder or child **mailboxFolder** in a user's mailbox.

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school<br>account)     | MailboxFolder.ReadWrite         | Not available.                   |
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxFolder.ReadWrite.All     | Not available.                   |

## **HTTP request**

HTTP

POST /admin/exchange/mailboxes/{mailboxId}/folders POST /admin/exchange/mailboxes/{mailboxId}/folders/inbox/childFolders

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |
| Content-Type  | application/json. Required.                                                  |

## **Request body**

In the request body, supply a JSON representation of the mailboxFolder object.

You can specify the following properties when you create a **mailboxFolder**.

#### ノ **Expand table**

| Property<br>displayName | Type<br>String | Description<br>The display name of the folder. Required. |
|-------------------------|----------------|----------------------------------------------------------|
|                         |                |                                                          |
| type                    | String         | Describes the folder class type. Required.               |

### **Response**

If successful, this method returns a 201 Created response code and a mailboxFolder object in the response body.

## **Examples**

### **Request**

The following example shows how to create a new mailbox folder.

| HTTP |                                                                          |  |
|------|--------------------------------------------------------------------------|--|
|      |                                                                          |  |
| HTTP |                                                                          |  |
| POST | https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0648f21@a |  |

```
{
 "displayName": "Announcements",
 "type": "IPF.Note",
 "singleValueExtendedProperties": [
 {
 "id": "String 0x3001",
 "value": "Announcements"
 }
 ]
}
```

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

HTTP

```
HTTP/1.1 201 Created
Content-type: application/json
Content-length: 179
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes('MBX%3A
73c326ef%402829ab8a')/folders/$entity",
 "id": "AQMkAGUw==",
 "displayName": "Announcements",
 "parentFolderId": "AQMkAGUc==",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0648f21@aab0
9c93",
 "childFolderCount": 0,
 "totalItemCount": 0,
 "type": "IPF.Note"
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Get mailboxFolder**

Article • 02/11/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Read the properties and relationships of a mailboxFolder object.

This API is available in the following national cloud deployments.

ノ **Expand table**

| Global  | US Government | US Government L5 | China operated by |
|---------|---------------|------------------|-------------------|
| service | L4            | (DOD)            | 21Vianet          |
|         |               |                  |                   |

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Delegated (work or school<br>account)     | permissions<br>MailboxFolder.Read | permissions<br>MailboxFolder.ReadWrite |
|-------------------------------------------|-----------------------------------|----------------------------------------|
| Delegated (personal Microsoft<br>account) | Not supported.                    | Not supported.                         |
| Application                               | MailboxFolder.Read.All            | MailboxFolder.ReadWrite.All            |

## **HTTP request**

HTTP

```
GET /admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}
GET
/admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/childFolders
/{mailboxFolderId}
```

## **Optional query parameters**

This method supports some of the OData query parameters to help customize the response. For general information, see OData query parameters.

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this method returns a 200 OK response code and a mailboxFolder object in the response body.

### **Examples**

### **Request**

The following example shows a request.

HTTP

```
HTTP
```

GET

https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a 7809c93/folders/NJWt2LeVEAAAIBDAAAAA==

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders$entity",
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "id": "NJWt2LeVEAAAIBDAAAAA==",
 "displayName": "Inbox",
 "parentFolderId": "NJWt2LeVEAAAIBCAAAAA==",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93",
 "childFolderCount": 3,
 "totalItemCount": 58,
 "type": "IPF.Note"
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Update mailboxFolder**

Article • 02/05/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Update mailboxFolder properties such as the **displayName** within a mailbox.

#### 7 **Note**

The folder **type** can't be updated. Instead, the folder needs to be deleted and a new folder can be created.

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school<br>account)     | MailboxFolder.ReadWrite         | Not available.                   |
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxFolder.ReadWrite.All     | Not available.                   |

## **HTTP request**

PATCH /admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId} PATCH /admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/childFolders /{mailboxFolderId}

### **Request headers**

ノ **Expand table**

| Name<br>Authorization | Description<br>Bearer {token}. Required. Learn more about authentication and authorization. |  |
|-----------------------|---------------------------------------------------------------------------------------------|--|
|                       |                                                                                             |  |
| Content-Type          | application/json. Required.                                                                 |  |

## **Request body**

In the request body, supply *only* the values for properties to update. Existing properties that aren't included in the request body maintain their previous values or are recalculated based on changes to other property values.

The following table specifies the properties that can be updated.

ノ **Expand table**

| Property    | Type   | Description                     |
|-------------|--------|---------------------------------|
| displayName | String | The display name of the folder. |

### **Response**

If successful, this method returns a 200 OK response code and an updated mailboxFolder object in the response body.

## **Examples**

**Request**

The following example shows how to update certain folder properties of a mailbox folder.

```
HTTP
HTTP
  PATCH
  https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0648f21@a
  ab09c93/folders/AAMkAGVmMDEzM
  {
   "displayName": "Announcements",
   "singleValueExtendedProperties": [
   {
   "id": "String 0x3001",
   "value": "Announcements"
   }
   ]
  }
```

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
```

```
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 179
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes('MBX%3A
73c326ef%402829ab8a')/folders/$entity",
 "id": "AQMkAGUw==",
 "displayName": "Announcements",
 "parentFolderId": "AQMkAGUc==",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0648f21@aab0
9c93",
 "childFolderCount": 0,
 "totalItemCount": 0,
 "type": "IPF.Note"
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Delete mailboxFolder**

Article • 02/05/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Delete a mailboxFolder or a child **mailboxFolder** within a mailbox.

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type<br>Delegated (work or school | Least privileged<br>permissions<br>MailboxFolder.ReadWrite | Higher privileged<br>permissions<br>Not available. |
|----------------------------------------------|------------------------------------------------------------|----------------------------------------------------|
| account)                                     |                                                            |                                                    |
| Delegated (personal Microsoft<br>account)    | Not supported.                                             | Not supported.                                     |
| Application                                  | MailboxFolder.ReadWrite.All                                | Not available.                                     |

## **HTTP request**

HTTP

```
DELETE /admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/$ref
DELETE
/admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/childFolders
/{mailboxFolderId}/$ref
```

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

### **Response**

If successful, this method returns a 204 No Content response code.

## **Examples**

### **Request**

The following example shows how to delete a mailbox folder.

| HTTP                                                                                                                      |  |
|---------------------------------------------------------------------------------------------------------------------------|--|
| HTTP                                                                                                                      |  |
| DELETE<br>https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0648f21@a<br>ab09c93/folders/AAMkAGVmMDEzM/\$ref |  |

### **Response**

The following example shows the response.

HTTP

HTTP/1.1 204 No Content

### **Feedback**

**Was this page helpful?**

Provide product feedback

# **mailboxFolder: delta**

07/23/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Get a set of mailboxFolder objects that have been added, deleted, or removed from the user's mailbox.

A **delta** function call for folders in a mailbox is similar to a GET request, except that by appropriately applying state tokens in one or more of these calls, you can query for incremental changes in the folders. This approach allows you to maintain and synchronize a local store of a user's mail folders without having to fetch all the folders of that mailbox from the server every time.

This API is available in the following national cloud deployments.

|                |                  |                        | ノ<br>Expand table          |
|----------------|------------------|------------------------|----------------------------|
| Global service | US Government L4 | US Government L5 (DOD) | China operated by 21Vianet |
|                |                  |                        |                            |

## **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                    | Least privileged<br>permissions | Higher privileged<br>permissions |
|------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school account) | MailboxFolder.Read              | MailboxFolder.ReadWrite          |

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxFolder.Read.All          | MailboxFolder.ReadWrite.All      |

## **HTTP request**

HTTP GET /admin/exchange/mailboxes/{mailboxId}/folders/delta GET /admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/childFolders/delta

## **Query parameters**

Tracking changes in folders incurs a round of one or more **delta** function calls. If you use any query parameter (other than \$deltaToken and \$skipToken ), you must specify it in the initial **delta** request. Microsoft Graph automatically encodes any specified parameters into the token portion of the nextLink or deltaLink URL provided in the response. You only need to specify any desired query parameters once upfront. In subsequent requests, simply copy and apply the nextLink or deltaLink URL from the previous response, as that URL already includes the encoded, desired parameters.

ノ **Expand table**

| Query<br>parameter<br>\$deltaToken | Description<br>A state token returned in the deltaLink URL of the previous delta function call for the<br>same folder collection, which indicates the completion of that round of change tracking. |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                    | Save and apply the entire deltaLink URL including this token in the first request of the<br>next round of change tracking for that collection.                                                     |
| \$skipToken                        | A state token returned in the nextLink URL of the previous delta function call, indicating<br>further changes are available to be tracked in the same folder collection.                           |

### **OData query parameters**

You can use the \$select query parameter to specify only the properties you need for best performance. The **id** and **parentMailboxUrl** properties are always returned.

## **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |
| Prefer        | odata.maxpagesize={x}. Optional.                                             |

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this function returns a 200 OK response code and a collection of mailboxFolder objects in the response body.

## **Examples**

### **Request**

The following example shows how to make a single **delta** function call, and limit the maximum number of folders in the response body to two.

To track changes in the folders of a mailbox, you make one or more **delta** function calls, with appropriate state tokens, to get the set of incremental changes since the last delta query.

For a similar example that shows how to use the state tokens to track changes in the items of a folder, see Get incremental changes to messages in a folder. The main differences between tracking folders and tracking items in a folder are in the delta query request URLs and the query responses that return **folder** rather than **item** collections.

```
GET
https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a7809c9
3/folders/delta
Prefer: odata.maxpagesize=2
```

### **Response**

HTTP

If the request is successful, the response includes a state token that is either a \$skipToken (in an **@odata.nextLink** response header) or a \$deltaToken (in an **@odata.deltaLink** response header). Respectively, they indicate whether you should continue with the round or you completed getting all the changes for that round.

The following example shows a \$deltaToken in an **@odata.deltaLink** response header.

**Note:** The response object shown here might be shortened for readability.

```
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 254
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#Collection(mailboxFolder)",
 "@odata.deltaLink":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a7809c93/f
olders/delta?$deltatoken={_deltaToken_}",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "displayName": "Inbound",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/Exchange/Mailboxes/MBX:e0643f21@a7809c93",
 "id":
"AAMkAGUwNjQ4ZjIxLTQ3Y2YtNDViMi1iZjc4LTMzNjMwNWM0ZGE2YQAuAAAAAADbrwBIJbBSTKolRbhHU
zSHAQCQ2fKdhq8oSKEDSVrdi3lRAAACgfP9AAA="
 },
 {
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "displayName": "Outbound",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/Exchange/Mailboxes/MBX:e0643f21@a7809c93",
 "id":
"AAMkAGUwNjQ4ZjIxLTQ3Y2YtNDViMi1iZjc4LTMzNjMwNWM0ZGE2YQAuAAAAAADbrwBIJbBSTKolRbhHU
zSHAQCQ2fKdhq8oSKEDSVrdi3lRAAACgfP_AAA="
 }
```

#### ] }

### **Related content**

- Use delta query to track changes in Microsoft Graph data
- Get incremental changes to items in a folder

# **List childFolders**

Article • 02/11/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Get the mailboxFolder collection under the specified mailboxFolder in a mailbox.

This API is available in the following national cloud deployments.

ノ **Expand table**

| Global  | US Government | US Government L5 | China operated by |
|---------|---------------|------------------|-------------------|
| service | L4            | (DOD)            | 21Vianet          |
|         |               |                  |                   |

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                           | Least privileged<br>permission | Higher privileged<br>permissions |
|-------------------------------------------|--------------------------------|----------------------------------|
| Delegated (work or school<br>account)     | MailboxFolder.Read             | MailboxFolder.ReadWrite          |
| Delegated (personal Microsoft<br>account) | Not supported.                 | Not supported.                   |
| Application                               | MailboxFolder.Read.All         | MailboxFolder.ReadWrite.All      |

## **HTTP request**

HTTP

GET

/admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/childFolders

## **Optional query parameters**

This method supports some of the OData query parameters to help customize the response. For general information, see OData query parameters.

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

### **Response**

If successful, this method returns a 200 OK response code and a collection of mailboxFolder objects in the response body.

## **Examples**

### **Request**

The following example shows how to get the mailbox folder collection under a specified folder.

HTTP

```
HTTP
```

```
GET
https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a
7809c93/folders/NJWt2LeVEAAAIBDAAAAA==/childFolders
```

### **Response**

}

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders('NJWt2LeVEAAAIBDAAAAA==')/childFolders",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "id": "EDSVrdi3lRAAEvNS16AAA=",
 "displayName": "Folder_1",
 "parentFolderId": "NJWt2LeVEAAAIBDAAAAA==",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93",
 "childFolderCount": 0,
 "totalItemCount": 20,
 "type": "IPF.Note"
 },
 {
 "@odata.type": "#microsoft.graph.mailboxFolder",
 "id": "EDSVrdi3lRAAEED0yTAAA=",
 "displayName": "Folder_2",
 "parentFolderId": "NJWt2LeVEAAAIBDAAAAA==",
 "parentMailboxUrl":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93",
 "childFolderCount": 0,
 "totalItemCount": 1,
 "type": "IPF.Note"
 }
 ]
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **List items**

Article • 02/05/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Get the mailboxItem collection within a specified mailboxFolder in a mailbox.

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                           | Least privileged<br>permissions | Higher privileged<br>permissions |
|-------------------------------------------|---------------------------------|----------------------------------|
| Delegated (work or school<br>account)     | MailboxItem.Read                | Not available.                   |
| Delegated (personal Microsoft<br>account) | Not supported.                  | Not supported.                   |
| Application                               | MailboxItem.Read.All            | Not available.                   |

# **HTTP request**

**Optional query parameters**

HTTP

GET /admin/exchange/mailboxes/{mailboxId}/folders/{mailboxFolderId}/items

This method supports some of the OData query parameters to help customize the response. For general information, see OData query parameters.

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

### **Response**

If successful, this method returns a 200 OK response code and a collection of mailboxItem objects in the response body.

## **Examples**

### **Example 1: List items**

The following example shows how to get the mailbox items within a specified folder in a mailbox.

#### **Request**

The following example shows a request.

| HTTP                                                                                                                     |  |
|--------------------------------------------------------------------------------------------------------------------------|--|
| HTTP<br>GET                                                                                                              |  |
| https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a<br>7809c93/folders/NJWt2LeVEAAAIBDAAAAA==/items |  |

#### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders('NJWt2LeVEAAAIBDAAAAA==')/items",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zPIo\"",
 "id": "EDSVrdi3lRAAE9J-20AAA=",
 "createdDateTime": "2021-09-15T12:16:38Z",
 "lastModifiedDateTime": "2021-09-15T12:16:41Z",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zPIo",
 "categories": [],
 "type": "IPM.Note",
 "size": 71133
 },
 {
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zO5W\"",
 "id": "EDSVrdi3lRAAE9J-2zAAA=",
 "createdDateTime": "2021-09-15T11:06:36Z",
 "lastModifiedDateTime": "2021-09-15T11:06:40Z",
 "changeKey": "CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAE8zO5W",
 "categories": [],
 "type": "IPM.Note",
 "size": 79968
 }
 ],
 "@odata.nextLink":
"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780
9c93/folders('NJWt2LeVEAAAIBDAAAAA==')/items?%24skip=10"
}
```

### **Example 2: List items using query parameter**

The following example uses the \$filter , \$select , and \$top query parameters. The \$filter parameter refines the results and returns only items with **createdDateTime** between 2021-08-21 and 2021-09-16 . The \$select parameter specifies to return only a subset of the properties of each item in the response, and the \$top parameter sets the page size of the result set to return only the top item in the mailbox under the specified folder.

### **Request**

The following example shows a request.

#### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
```

```
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#admin/exchange/mailboxes/MBX:e06
43f21@a7809c93/folders('Inbox')/items",
 "value": [
 {
 "@odata.type": "#microsoft.graph.mailboxItem",
 "@odata.etag": "W/\"CQAAABYAAACQ2fKdhq8oSKEDSVrdi3lRAAFOoqvk\"",
 "id": "EDSVrdi3lRAAE9J-2xAAA=",
 "type": "IPM.Note",
 "size": 91339
 }
 ],
 "@odata.nextLink":
```

"https://graph.microsoft.com/beta/admin/exchange/mailboxes/MBX:e0643f21@a780

```
9c93/folders('Inbox')/items?%24filter=createdDateTime+ge+2021-08-
21+and+createdDateTime+lt+2021-09-
16&%24select=type%2csize&%24top=1&%24skip=1"
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Create single-value extended property**

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

#### U **Caution**

Outlook tasks is deprecated and stopped returning data on August 10, 2022. Update existing apps that use this feature with Outlook tasks.

Create one or more single-value extended properties in a new or existing instance of a resource.

The following user resources are supported:

- calendar
- contact
- contactFolder
- event
- mailFolder
- message
- Outlook task
- Outlook task folder
- todoTask

The following group resources are supported:

- group calendar
- group event
- group post

For more information about when to use open extensions or extended properties, and how to specify extended properties, see Extended properties overview.

This API is available in the following national cloud deployments.

| Global service | US Government L4 | US Government L5 (DOD) | China operated by 21Vianet |
|----------------|------------------|------------------------|----------------------------|
|                |                  |                        |                            |

### **Permissions**

Depending on the resource you're creating the extended property in and the permission type (delegated or application) you request, the permission specified in the following table is the minimum required to call this API. To learn more, including how to choose permissions, see Permissions.

#### ノ **Expand table**

| contact                            | Contacts.ReadWrite                 | Contacts.ReadWrite                | Contacts.ReadWrite               |
|------------------------------------|------------------------------------|-----------------------------------|----------------------------------|
| contactFolder                      | Contacts.ReadWrite                 | Contacts.ReadWrite                | Contacts.ReadWrite               |
| event                              | Calendars.ReadWrite                | Calendars.ReadWrite               | Calendars.ReadWrite              |
| group calendar                     | Group.ReadWrite.All                | Not supported.                    | Not supported.                   |
| group event                        | Group.ReadWrite.All                | Not supported.                    | Not supported.                   |
| group post                         | Group.ReadWrite.All                | Not supported.                    | Not supported.                   |
| mailFolder                         | Mail.ReadWrite                     | Mail.ReadWrite                    | Mail.ReadWrite                   |
| message                            | Mail.ReadWrite                     | Mail.ReadWrite                    | Mail.ReadWrite                   |
| Outlook task                       | Tasks.ReadWrite                    | Tasks.ReadWrite                   | Not supported.                   |
| Outlook task<br>folder<br>todoTask | Tasks.ReadWrite<br>Tasks.ReadWrite | Tasks.ReadWrite<br>Not supported. | Not supported.<br>Not supported. |

## **HTTP request**

You can create extended properties in a new or existing resource instance.

To create one or more extended properties in a *new* resource instance, use the same REST request as creating the instance, and include the properties of the new resource instance *and extended property* in the request body. Some resources support creation in more than one way. For more information on how to create these resource instances, see the corresponding topics for creating a message, mailFolder, event, calendar, contact, contactFolder, Outlook task, Outlook task folder, group event, group post, and todoTask.

The following is the syntax of the requests.

| HTTP                                                                                                                                                                                                                                                                                                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| POST /me/messages<br>POST /users/{id userPrincipalName}/messages<br>POST /me/mailFolders/{id}/messages                                                                                                                                                                                                                                     |
| POST /me/mailFolders                                                                                                                                                                                                                                                                                                                       |
| POST /users/{id userPrincipalName}/mailFolders                                                                                                                                                                                                                                                                                             |
| POST /me/events<br>POST /users/{id userPrincipalName}/events                                                                                                                                                                                                                                                                               |
| POST /me/calendars                                                                                                                                                                                                                                                                                                                         |
| POST /users/{id userPrincipalName}/calendars                                                                                                                                                                                                                                                                                               |
| POST /me/contacts<br>POST /users/{id userPrincipalName}/contacts                                                                                                                                                                                                                                                                           |
| POST /me/contactFolders<br>POST /users/{id userPrincipalName}/contactFolders                                                                                                                                                                                                                                                               |
| POST /me/outlook/tasks<br>POST /users/{id userPrincipalName}/outlook/tasks<br>POST /me/outlook/taskFolders/{id}/tasks<br>POST /users/{id userPrincipalName}/outlook/taskFolders/{id}/tasks<br>POST /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks<br>POST /users/{id userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks |
| POST /me/outlook/taskFolders<br>POST /users/{id userPrincipalName}/outlook/taskFolders<br>POST /me/outlook/taskGroups/{id}/taskFolders<br>POST /users/{id userPrincipalName}/outlook/taskGroups/{id}/taskFolders                                                                                                                           |
| POST /me/todo/lists/{todoTaskListId}/tasks/{todoTaskId}?<br>\$expand=singleValueExtendedProperties(\$filter=id eq<br>'{singleValueExtendedPropertyId}')<br>POST /me/todo/lists/{todoTaskListId}/tasks?<br>\$expand=singleValueExtendedProperties(\$filter=id eq<br>'{singleValueExtendedPropertyId}')                                      |

```
POST /groups/{id}/conversations/{id}/threads/{id}/posts/{id}/reply
POST /groups/{id}/threads/{id}/reply
POST /groups/{id}/conversations/{id}/threads/{id}/reply
POST /groups/{id}/threads
POST /groups/{id}/conversations
```

7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

To create one or more extended properties in an existing resource instance, specify the instance in the request, and include the extended property in the request body.

**Note** You can't create an extended property in an existing group post.

HTTP

```
PATCH /me/messages/{id}
PATCH /users/{id|userPrincipalName}/messages/{id}
PATCH /me/mailFolders/{id}/messages/{id}
PATCH /me/mailFolders/{id}
PATCH /users/{id|userPrincipalName}/mailFolders/{id}
PATCH /me/events/{id}
PATCH /users/{id|userPrincipalName}/events/{id}
PATCH /me/calendars/{id}
PATCH /users/{id|userPrincipalName}/calendars/{id}
PATCH /me/contacts/{id}
PATCH /users/{id|userPrincipalName}/contacts/{id}
PATCH /me/contactFolders/{id}
PATCH /users/{id|userPrincipalName}/contactFolders/{id}
PATCH /me/outlook/tasks/{id}
PATCH /users/{id|userPrincipalName}/outlook/tasks/{id}
PATCH /me/outlook/taskFolders/{id}/tasks/{id}
PATCH /users/{id|userPrincipalName}/outlook/taskFolders/{id}/tasks/{id}
PATCH /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}
PATCH
/users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}
PATCH /me/outlook/taskFolders/{id}
PATCH /users/{id|userPrincipalName}/outlook/taskFolders/{id}
PATCH /me/outlook/taskGroups/{id}/taskFolders/{id}
PATCH /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}
```

```
PATCH /me/todo/lists/{todoTaskListId}/tasks?
$expand=singleValueExtendedProperties($filter=id eq
'{singleValueExtendedPropertyId}')
PATCH /me/todo/lists/{todoTaskListId}/tasks/{todoTaskId}?
$expand=singleValueExtendedProperties($filter=id eq
'{singleValueExtendedPropertyId}')
```

```
PATCH /groups/{id}/events/{id}
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

### **Request headers**

ノ **Expand table**

| Name          | Value                                                                        |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |
| Content-Type  | application/json                                                             |

## **Request body**

Provide a JSON body of each singleValueLegacyExtendedProperty object in the **singleValueExtendedProperties** collection property of the resource instance.

#### ノ **Expand table**

| singleValueExtendedProperties | singleValueLegacyExtendedProperty<br>collection | An array of one or more single<br>valued extended properties.                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                            | String                                          | For each property in the<br>singleValueExtendedProperties<br>collection, specify this to identify<br>the property. It must follow one<br>of the supported formats. For<br>more information, see Extended<br>properties overview. Required. |

| Property | Type   | Description                                                                                                       |
|----------|--------|-------------------------------------------------------------------------------------------------------------------|
| value    | string | For each property in the<br>singleValueExtendedProperties<br>collection, specify the property<br>value. Required. |

When creating an extended property in a *new* resource instance, in addition to the new **singleValueExtendedProperties** collection, provide a JSON representation of that resource instance (that is, a message, mailFolder, event, todoTask, etc.)

### **Response**

### **Response code**

An operation that successfully creates an extended property in a new resource instance returns a 201 Created response code. In the case of a new group post, depending on the method used, the operation returns either a 200 OK or a 202 Accepted response code.

In an existing resource instance, a successful create operation returns 200 OK .

### **Response body**

When you create an extended property, the response includes only the new or existing instance but not the new extended property. To see the newly created extended property, get the instance expanded with the extended property.

When you create an extended property in a *new* group post by replying to a thread or post, the response includes only a response code but not the new post nor the extended property.

### **Examples**

### **Request 1**

The first example creates a new event and a single-value extended property in the same POST operation. Apart from the properties you'd normally include for a new event, the request body includes the **singleValueExtendedProperties** collection that contains one single-value extended property, and the following for the property:

**id** specifies the property type as String , the GUID, and the property named Fun .

**value** specifies Food as the value of the Fun property.

#### HTTP

```
POST https://graph.microsoft.com/beta/me/events
Content-Type: application/json
{
 "subject": "Celebrate Thanksgiving",
 "body": {
 "contentType": "HTML",
 "content": "Let's get together!"
 },
 "start": {
 "dateTime": "2015-11-26T18:00:00",
 "timeZone": "Pacific Standard Time"
 },
 "end": {
 "dateTime": "2015-11-26T23:00:00",
 "timeZone": "Pacific Standard Time"
 },
 "attendees": [
 {
 "emailAddress": {
 "address": "Terrie@contoso.com",
 "name": "Terrie Barrera"
 },
 "type": "Required"
 }
 ],
 "singleValueExtendedProperties": [
 {
 "id":"String {66f5a359-4659-4830-9070-00040ec6ac6e} Name Fun",
 "value":"Food"
 }
 ]
}
```

### **Response 1**

A successful response is indicated by an HTTP 201 Created response code, and includes the new event in the response body, similar to the response from creating just an event. The response doesn't include any newly created extended properties.

To see the newly created extended property, get the event expanded with the extended property.

### **Request 2**

The second example creates one single-value extended property for the specified existing message. That extended property is the only element in the **singleValueExtendedProperties** array. The request body includes the following for the extended property:

- **id** specifies the property type as String , the GUID, and the property named Color .
- **value** specifies Green as the value of the Color property.

```
HTTP
PATCH https://graph.microsoft.com/beta/me/messages('AAMkAGE1M2_bs88AACHsLqWAAA=')
Content-Type: application/json
{
 "singleValueExtendedProperties": [
 {
 "id":"String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color",
 "value":"Green"
 }
 ]
}
```

### **Response 2**

A successful response is indicated by an HTTP 200 OK response code, and includes the specified message in the response body, similar to the response from updating a message. The response doesn't include the newly created extended property.

To see the newly created extended property, get the message expanded with the extended property.

**Last updated on 10/29/2025**

# **Get singleValueLegacyExtendedProperty**

Article • 03/17/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

#### U **Caution**

Outlook tasks is deprecated and stopped returning data on August 10, 2022. Update existing apps that use this feature with Outlook tasks.

You can get a single resource instance expanded with a specific extended property, or a collection of resource instances that include extended properties matching a filter.

Using the query parameter \$expand allows you to get the specified resource instance expanded with a specific extended property. Use a \$filter and eq operator on the **id** property to specify the extended property. This is currently the only way to get the singleValueLegacyExtendedProperty object that represents an extended property.

To get resource instances that have certain extended properties, use the \$filter query parameter and apply an eq operator on the **id** property. In addition, for numeric extended properties, apply one of the following operators on the **value** property: eq , ne , ge , gt , le , or lt . For string-typed extended properties, apply a contains , startswith , eq , or ne operator on **value**.

Filtering the string name ( Name ) in the **id** of an extended property is case-sensitive. Filtering the **value** property of an extended property is case-insensitive.

The following user resources are supported:

- calendar
- contact
- contactFolder
- event

- message
- Outlook task
- Outlook task folder

As well as the following group resources:

- group calendar
- group event
- group post

See Extended properties overview for more information about when to use open extensions or extended properties, and how to specify extended properties.

This API is available in the following national cloud deployments.

#### ノ **Expand table**

| Global  | US Government | US Government L5 | China operated by |
|---------|---------------|------------------|-------------------|
| service | L4            | (DOD)            | 21Vianet          |
|         |               |                  |                   |

### **Permissions**

Depending on the resource you're getting the extended property from and the permission type (delegated or application) you request, the permission specified in the following table is the minimum required to call this API. To learn more, including how to choose permissions, see Permissions.

ノ **Expand table**

| calendar<br>contact | Calendars.Read<br>Contacts.Read | Calendars.Read<br>Contacts.Read | Calendars.Read<br>Contacts.Read |
|---------------------|---------------------------------|---------------------------------|---------------------------------|
| contactFolder       | Contacts.Read                   | Contacts.Read                   | Contacts.Read                   |
| event               | Calendars.Read                  | Calendars.Read                  | Calendars.Read                  |
| group calendar      | Group.Read.All                  | Not supported                   | Not supported                   |
| group event         | Group.Read.All                  | Not supported                   | Not supported                   |

| Supported<br>resource  | Delegated (work or<br>school account) | Delegated (personal<br>Microsoft account) | Application    |
|------------------------|---------------------------------------|-------------------------------------------|----------------|
| group post             | Group.Read.All                        | Not supported                             | Group.Read.All |
| mailFolder             | Mail.Read                             | Mail.Read                                 | Mail.Read      |
| message                | Mail.Read                             | Mail.Read                                 | Mail.Read      |
| Outlook task           | Tasks.Read                            | Tasks.Read                                | Not supported  |
| Outlook task<br>folder | Tasks.Read                            | Tasks.Read                                | Not supported  |

### **HTTP request**

### **GET a resource instance expanded with an extended property that matches a filter**

Get a resource instance expanded with the extended property which matches a filter on the **id** property. Make sure you apply URL encoding to the space characters in the filter string.

Get a **message** instance:

```
GET /me/messages/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/messages/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/mailFolders/{id}/messages/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

HTTP

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **mailFolder** instance:

```
GET /me/mailFolders/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/mailFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **event** instance:

#### HTTP

```
GET /me/events/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/events/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get a **calendar** instance:

HTTP

```
GET /me/calendars/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/calendars/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get a **contact** instance:

HTTP

```
GET /me/contacts/{id}?$expand=singleValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/contacts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/contactFolders/{id}/contacts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}/contacts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **contactFolder** instance:

HTTP

```
GET /me/contactfolders/{id}?$expand=singleValueExtendedProperties($filter=id
eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTask** instance:

```
HTTP
GET /me/outlook/tasks/{id}?$expand=singleValueExtendedProperties($filter=id
eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/tasks/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskFolders/{id}/tasks/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

```
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET
/users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks
```

```
/{id}?$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTaskFolder** instance:

HTTP

```
GET /me/outlook/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a group **event** instance:

HTTP

```
GET /groups/{id}/events/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

Get a group **post** instance:

```
GET /groups/{id}/threads/{id}/posts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
GET /groups/{id}/conversations/{id}/threads/{id}/posts/{id}?
$expand=singleValueExtendedProperties($filter=id eq '{id_value}')
```

### **GET resource instances that include numeric extended properties matching a filter**

Get instances of a supported resource that have a numeric extended property matching a filter. The filter uses an eq operator on the **id** property, and one of the following operators on the **value** property: eq , ne , ge , gt , le , or lt . Make sure you apply Make sure you apply URL encoding to the following characters in the filter string - colon, forward slash, and space.

The following syntax lines show a filter that uses an eq operator on the id, and another eq operator on the property value. You can substitute the eq operator on the **value** by any one of the other operators ( ne , ge , gt , le , or lt ) that apply to numeric values.

Get **message** instances:

| HTTP                                                                                                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GET /me/messages?\$filter=singleValueExtendedProperties/Any(ep: ep/id eq<br>'{id_value}' and ep/value eq '{property_value}')                               |
| GET /users/{id userPrincipalName}/messages?<br>\$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and<br>ep/value eq '{property_value}') |
| GET /me/mailFolders/{id}/messages?<br>\$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and<br>ep/value eq '{property_value}')          |

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **mailFolder** instances:

HTTP

GET /me/mailFolders?\$filter=singleValueExtendedProperties/Any(ep: ep/id eq

```
GET /users/{id|userPrincipalName}/mailFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get **event** instances:

#### HTTP

```
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get **calendar** instances:

#### HTTP

```
GET /me/calendars?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/calendars?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **contact** instances:

|  |  | HTTP |
|--|--|------|
|  |  |      |

```
GET /me/contacts?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/contacts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/contactFolders/{id}/contacts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}/contacts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **contactFolder** instances:

HTTP

```
GET /me/contactfolders?$filter=singleValueExtendedProperties/Any(ep: ep/id
eq '{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/contactFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTask** instance:

HTTP

```
GET /me/outlook/tasks?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
```

```
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/outlook/taskFolders/{id}/tasks?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders/{id}/tasks?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET
/users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks
?$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

HTTP

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **outlookTaskFolder** instance:

```
GET /me/outlook/taskFolders?$filter=singleValueExtendedProperties/Any(ep:
ep/id eq '{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/outlook/taskGroups/{id}/taskFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get group **event** instances:

```
HTTP
```

```
GET /groups/{id}/events?$filter=singleValueExtendedProperties/Any(ep: ep/id
eq '{id_value}' and ep/value eq '{property_value}')
```

Get group **post** instances:

```
HTTP
GET /groups/{id}/threads/{id}/posts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /groups/{id}/conversations/{id}/threads/{id}/posts?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
```

### **GET resource instances with string-typed extended properties matching a filter**

Get instances of the **message** or **event** resource that have a string-typed extended property matching a filter. The filter uses an eq operator on the **id** property, and one of the following operators on the **value** property: contains , startswith , eq , or ne . Make sure you apply URL encoding to the following characters in the filter string - colon, forward slash, and space.

Get **message** instances:

```
HTTP
GET /me/messages?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and contains(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
contains(ep/value, '{property_value}'))
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
contains(ep/value, '{property_value}'))
GET /me/messages?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and startswith(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
startswith(ep/value, '{property_value}'))
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
startswith(ep/value, '{property_value}'))
```

```
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/messages?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value ne '{property_value}')
GET /users/{id|userPrincipalName}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value ne '{property_value}')
GET /me/mailFolders/{id}/messages?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value ne '{property_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get **event** instances:

HTTP

```
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and contains(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
contains(ep/value, '{property_value}'))
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and startswith(ep/value, '{property_value}'))
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
startswith(ep/value, '{property_value}'))
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value eq '{property_value}')
GET /users/{id|userPrincipalName}/events?
$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id_value}' and
ep/value eq '{property_value}')
GET /me/events?$filter=singleValueExtendedProperties/Any(ep: ep/id eq
'{id_value}' and ep/value ne '{property_value}')
GET /users/{id|userPrincipalName}/events?
```

\$filter=singleValueExtendedProperties/Any(ep: ep/id eq '{id\_value}' and ep/value ne '{property\_value}')

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get group **event** instances:

| HTTP                                                                                                                                          |  |
|-----------------------------------------------------------------------------------------------------------------------------------------------|--|
| GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id<br>eq '{id_value}' and contains(ep/value, '{property_value}'))   |  |
| GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id<br>eq '{id_value}' and startswith(ep/value, '{property_value}')) |  |
| GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id<br>eq '{id_value}' and ep/value eq '{property_value}')           |  |
| GET /groups/{id}/events?\$filter=singleValueExtendedProperties/Any(ep: ep/id<br>eq '{id_value}' and ep/value ne '{property_value}')           |  |

### **Path parameters**

ノ **Expand table**

| Parameter      | Type   | Description                                                                                                                                                                                                                                                                                            |
|----------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id_value       | String | The ID of the extended property to match. It must follow one of the<br>supported formats. See Outlook extended properties overview for more<br>information. Required.                                                                                                                                  |
| property_value | String | The value of the extended property to match. Required where listed in<br>the HTTP request section above. If {property_value} is not a string, make<br>sure you explicitly cast ep/value to the appropriate Edm data type when<br>comparing it with {property_value}. See request 4 below for examples. |

### **Request headers**

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this method returns a 200 OK response code.

### **GET resource instance using \$expand**

The response body includes an object representing the requested resource instance, expanded with the matching singleValueLegacyExtendedProperty object.

### **GET resource instances that contain an extended property matching a filter**

The response body includes one or more objects representing the resource instances that contain a matching extended property. The response body does not include the extended property.

## **Example**

### **Request 1**

The first example gets and expands the specified message by including a single-value extended property. The filter returns the extended property that has its **id** matching the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color (with URL encoding removed here for ease of reading).

| msgraph                                                                  |  |
|--------------------------------------------------------------------------|--|
| GET                                                                      |  |
| https://graph.microsoft.com/beta/me/messages/AAMkAGE1M2_bs88AACHsLqWAAA= |  |

### **Response 1**

The response body includes all the properties of the specified message and extended property returned from the filter.

Note: The **message** object shown here is truncated for brevity. All of the properties will be returned from an actual call.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#Me/messages/$entity",
 "@odata.id": "https://graph.microsoft.com/beta/users('ddfcd489-628b-
40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-
709fad664b77')/messages('AAMkAGE1M2_bs88AACHsLqWAAA=')",
 "@odata.etag": "W/\"CQAAABYAAACY4MQpaFz9SbqUDe4+bs88AACbyS4H\"",
 "id": "AAMkAGE1M2_bs88AACHsLqWAAA=",
 "subject": "RE: Talk about emergency prep",
 "sender": {
 "emailAddress": {
 "name": "Christine Irwin",
 "address": "christine@contoso.com"
 }
 },
 "from": null,
 "toRecipients": [
 {
 "emailAddress": {
 "name": "Christine Irwin",
 "address": "christine@contoso.com"
 }
 }
 ],
 "singleValueExtendedProperties": [
 {
 "id": "String {66f5a359-4659-4830-9070-00047ec6ac6e} Name
Color",
 "value": "Green"
 }
 ]
}
```

### **Request 2**

The second example gets messages that have the string-typed single-value extended property specified in the filter. The filter looks for the extended property that has:

- Its **id** equal to the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color (with URL encoding removed here for ease of reading).
- Its **value** equal to the string Green .

```
HTTP
```

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties%2FAny(ep%3A%20ep%2Fid%20eq%20'String%2
0{66f5a359-4659-4830-9070-
00047ec6ac6e}%20Name%20Color'%20and%20ep%2Fvalue%20eq%20'Green')
```

#### **Response 2**

A successful response is indicated by an HTTP 200 OK response code, and the response body includes all the properties of the messages that have the extended property matching the filter. The response body is similar to the response from getting a message collection. The response does not include the matching extended property.

### **Request 3**

The third example gets messages that have the string-typed single-value extended property specified in the filter. The filter looks for the extended property that has:

- Its **id** equal to the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color (with URL encoding removed here for ease of reading).
- Its **value** containing the string green .

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties/any(ep:ep/Id eq 'String {66f5a359-
4659-4830-9070-00047ec6ac6e} Name Color' and contains(ep/Value, 'green'))
```

HTTP

A successful response is indicated by an HTTP 200 OK response code, and the response body includes all the properties of the messages that have the extended property matching the filter. For example, a message that has a single-value extended property with the **id** equal to the string String {66f5a359-4659-4830-9070-00047ec6ac6e} Name Color , and the **value** Light green , would match the filter and be included in the response.

The response body is similar to the response from getting a message collection. The response does not include the matching extended property.

### **Request 4**

The next 2 examples show how to get messages that have non-string typed single-value extended properties. For ease of reading, they do not include the necessary URL encoding.

The following example shows a filter that looks for the extended property that has:

- Its **id** matching the string CLSID {00062008-0000-0000-C000-000000000046} Name ConnectorSenderGuid .
- Its **value** being the GUID b9cf8971-7d55-4b73-9ffa-a584611b600b . To compare the property value with a GUID, cast ep/value to Edm.Guid .

HTTP

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties/any(ep:ep/id eq 'CLSID {00062008-0000-
0000-C000-000000000046} Name ConnectorSenderGuid' and cast(ep/value,
Edm.Guid) eq (b9cf8971-7d55-4b73-9ffa-a584611b600b))
```

The next example shows a filter that looks for the extended property that has:

- Its **id** matching the string Integer {66f5a359-4659-4830-9070-00047ec6ac6e} Name Pallete .
- Its **value** equal to the integer 12. To compare the property value with an integer, cast ep/value to Edm.Int32 .

HTTP

```
GET https://graph.microsoft.com/beta/me/messages?
$filter=singleValueExtendedProperties/any(ep:ep/id eq 'Integer {66f5a359-
```

#### **Response 4**

For each of the preceding 2 examples, a successful response is indicated by an HTTP 200 OK response code, and the response body includes all the properties of the messages that have the extended property matching the corresponding filter. The response body is similar to the response from getting a message collection. The response does not include the matching extended property.

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **Create multi-value extended property**

03/17/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

#### U **Caution**

Outlook tasks is deprecated and stopped returning data on August 10, 2022. Update existing apps that use this feature with Outlook tasks.

Create one or more multi-value extended properties in a new or existing instance of a resource.

The following user resources are supported:

- calendar
- contact
- contactFolder
- event
- mailFolder
- message
- Outlook task
- Outlook task folder

And the following group resources:

- group calendar
- group event
- group post

See Extended properties overview for more information about when to use open extensions or extended properties, and how to specify extended properties.

This API is available in the following national cloud deployments.

| Global service | US Government L4 | US Government L5 (DOD) | China operated by 21Vianet |
|----------------|------------------|------------------------|----------------------------|
|                |                  |                        |                            |

### **Permissions**

Depending on the resource you're creating the extended property in and the permission type (delegated or application) you request, the permission specified in the following table is the minimum required to call this API. To learn more, including how to choose permissions, see Permissions.

ノ **Expand table**

| Supported<br>resource  | Delegated (work or school<br>account) | Delegated (personal<br>Microsoft account) | Application         |
|------------------------|---------------------------------------|-------------------------------------------|---------------------|
| calendar               | Calendars.ReadWrite                   | Calendars.ReadWrite                       | Calendars.ReadWrite |
| contact                | Contacts.ReadWrite                    | Contacts.ReadWrite                        | Contacts.ReadWrite  |
| contactFolder          | Contacts.ReadWrite                    | Contacts.ReadWrite                        | Contacts.ReadWrite  |
| event                  | Calendars.ReadWrite                   | Calendars.ReadWrite                       | Calendars.ReadWrite |
| group calendar         | Group.ReadWrite.All                   | Not supported                             | Not supported       |
| group event            | Group.ReadWrite.All                   | Not supported                             | Not supported       |
| group post             | Group.ReadWrite.All                   | Not supported                             | Not supported       |
| mailFolder             | Mail.ReadWrite                        | Mail.ReadWrite                            | Mail.ReadWrite      |
| message                | Mail.ReadWrite                        | Mail.ReadWrite                            | Mail.ReadWrite      |
| Outlook task           | Tasks.ReadWrite                       | Tasks.ReadWrite                           | Not supported       |
| Outlook task<br>folder | Tasks.ReadWrite                       | Tasks.ReadWrite                           | Not supported       |

## **HTTP request**

You can create extended properties in a new or existing resource instance.

To create one or more extended properties in a *new* resource instance, use the same REST request as creating the instance, and include the properties of the new resource instance *and extended property* in the request body. Some resources support creation in more than one way. For more information on creating these resource instances, see the corresponding topics for creating a message, mailFolder, event, calendar, contact, contactFolder, Outlook task, Outlook task folder, group event, and group post.

The following is the syntax of the requests.

```
HTTP
POST /me/messages
POST /users/{id|userPrincipalName}/messages
POST /me/mailFolders/{id}/messages
POST /me/mailFolders
POST /users/{id|userPrincipalName}/mailFolders
POST /me/events
POST /users/{id|userPrincipalName}/events
POST /me/calendars
POST /users/{id|userPrincipalName}/calendars
POST /me/contacts
POST /users/{id|userPrincipalName}/contacts
POST /me/contactFolders
POST /users/{id|userPrincipalName}/contactFolders
POST /me/outlook/tasks
POST /users/{id|userPrincipalName}/outlook/tasks
POST /me/outlook/taskFolders/{id}/tasks
POST /users/{id|userPrincipalName}/outlook/taskFolders/{id}/tasks
POST /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks
POST /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks
POST /me/outlook/taskFolders
POST /users/{id|userPrincipalName}/outlook/taskFolders
POST /me/outlook/taskGroups/{id}/taskFolders
POST /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders
POST /groups/{id}/events
POST /groups/{id}/threads/{id}/posts/{id}/reply
POST /groups/{id}/conversations/{id}/threads/{id}/posts/{id}/reply
POST /groups/{id}/threads/{id}/reply
POST /groups/{id}/conversations/{id}/threads/{id}/reply
POST /groups/{id}/threads
POST /groups/{id}/conversations
```

7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

To create one or more extended properties in an existing resource instance, specify the instance in the request, and include the extended property in the request body.

**Note** You can't create an extended property in an existing group post.

```
HTTP
PATCH /me/messages/{id}
PATCH /users/{id|userPrincipalName}/messages/{id}
PATCH /me/mailFolders/{id}/messages/{id}
PATCH /me/mailFolders/{id}
PATCH /users/{id|userPrincipalName}/mailFolders/{id}
PATCH /me/events/{id}
PATCH /users/{id|userPrincipalName}/events/{id}
PATCH /me/calendars/{id}
PATCH /users/{id|userPrincipalName}/calendars/{id}
PATCH /me/contacts/{id}
PATCH /users/{id|userPrincipalName}/contacts/{id}
PATCH /me/contactFolders/{id}
PATCH /users/{id|userPrincipalName}/contactFolders/{id}
PATCH /me/outlook/tasks/{id}
PATCH /users/{id|userPrincipalName}/outlook/tasks/{id}
PATCH /me/outlook/taskFolders/{id}/tasks/{id}
PATCH /users/{id|userPrincipalName}/outlook/taskFolders/{id}/tasks/{id}
PATCH /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}
PATCH
/users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}
PATCH /me/outlook/taskFolders/{id}
PATCH /users/{id|userPrincipalName}/outlook/taskFolders/{id}
PATCH /me/outlook/taskGroups/{id}/taskFolders/{id}
PATCH /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}
PATCH /groups/{id}/events/{id}
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

### **Request headers**

ノ **Expand table**

| Name          | Value                                                                        |  |
|---------------|------------------------------------------------------------------------------|--|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |  |
| Content-Type  | application/json                                                             |  |

## **Request body**

Provide a JSON body of each multiValueLegacyExtendedProperty object in the **multiValueExtendedProperties** collection property of the resource instance.

#### ノ **Expand table**

| Property                     | Type                                           | Description                                                                                                                                                                                                                                         |
|------------------------------|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| multiValueExtendedProperties | multiValueLegacyExtendedProperty<br>collection | An array of one or more multi<br>valued extended properties.                                                                                                                                                                                        |
| id                           | String                                         | For each property in the<br>multiValueExtendedProperties<br>collection, specify this to identify<br>the property. It must follow one of<br>the supported formats. See<br>Outlook extended properties<br>overview for more information.<br>Required. |
| value                        | string                                         | For each property in the<br>multiValueExtendedProperties<br>collection, specify the property<br>value. Required.                                                                                                                                    |

When creating an extended property in a *new* resource instance, in addition to the new **multiValueExtendedProperties** collection, provide a JSON representation of that resource instance (that is, a message, mailFolder, event, etc.)

### **Response**

**Response code**

An operation successful in creating an extended property in a new resource instance returns 201 Created , except in a new group post, depending on the method used, the operation can return 200 OK or 202 Accepted .

In an existing resource instance, a successful create operation returns 200 OK .

### **Response body**

When creating an extended property in a supported resource other than group post, the response includes only the new or existing instance but not the new extended property. To see the newly created extended property, get the instance expanded with the extended property.

When creating an extended property in a *new* group post, the response includes only a response code but not the new post nor the extended property. You can't create an extended property in an existing group post.

### **Example**

#### **Request 1**

The first example creates a multi-value extended property in a new event all in the same POST operation. Apart from the properties you'd normally include for a new event, the request body includes the **multiValueExtendedProperties** collection that contains one extended property. The request body includes the following for that multi-value extended property:

- **id** which specifies the property as an array of strings with the specified GUID and the name Recreation .
- **value** which specifies Recreation as an array of 3 string values, ["Food", "Hiking", "Swimming"] .

HTTP

```
POST https://graph.microsoft.com/beta/me/events
Content-Type: application/json
{
 "subject": "Family reunion",
 "body": {
 "contentType": "HTML",
 "content": "Let's get together this Thanksgiving!"
 },
 "start": {
 "dateTime": "2015-11-26T09:00:00",
```

```
 },
 "end": {
 "dateTime": "2015-11-29T21:00:00",
 "timeZone": "Pacific Standard Time"
 },
 "attendees": [
 {
 "emailAddress": {
 "address": "Terrie@contoso.com",
 "name": "Terrie Barrera"
 },
 "type": "Required"
 },
 {
 "emailAddress": {
 "address": "Lauren@contoso.com",
 "name": "Lauren Solis"
 },
 "type": "Required"
 }
 ],
 "multiValueExtendedProperties": [
 {
 "id":"StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name
Recreation",
 "value": ["Food", "Hiking", "Swimming"]
 }
 ]
}
```

### **Response 1**

A successful response is indicated by an HTTP 201 Created response code, and includes the new event in the response body, similar to the response from creating just an event. The response doesn't include any newly created extended properties.

To see the newly created extended property, get the event expanded with the extended property.

#### **Request 2**

The second example creates one multi-value extended property for the specified message. That extended property is the only element in the **multiValueExtendedProperties** collection. The request body includes the following for the extended property:

**id** specifies the property as an array of strings with the specified GUID and the name

**value** specifies Palette as an array of 3 string values, ["Green", "Aqua", "Blue"] .

```
HTTP
PATCH https://graph.microsoft.com/beta/me/messages('AAMkAGE1M2_as77AACHsLrBBBA=')
Content-Type: application/json
{
 "multiValueExtendedProperties": [
 {
 "id":"StringArray {66f5a359-4659-4830-9070-00049ec6ac6e} Name Palette",
 "value":["Green", "Aqua", "Blue"]
 }
 ]
}
```

#### **Response 2**

A successful response is indicated by an HTTP 200 OK response code, and includes the specified message in the response body, similar to the response from updating a message. The response doesn't include the newly created extended property.

To see the newly created extended property, get the message expanded with the extended property.

# **Get multiValueLegacyExtendedProperty**

Article • 03/17/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

#### U **Caution**

Outlook tasks is deprecated and stopped returning data on August 10, 2022. Update existing apps that use this feature with Outlook tasks.

Get a resource instance that contains a multi-value extended property by using \$expand .

Using the query parameter \$expand allows you to get the specified instance expanded with the indicated extended property. This is currently the only way to get the multiValueLegacyExtendedProperty object that represents an extended property.

The following user resources are supported:

- calendar
- contact
- contactFolder
- event
- mailFolder
- message
- Outlook task
- Outlook task folder

As well as the following group resources:

- group calendar
- group event
- group post

See Extended properties overview for more information about when to use open

This API is available in the following national cloud deployments.

#### ノ **Expand table**

| Global  | US Government | US Government L5 | China operated by |
|---------|---------------|------------------|-------------------|
| service | L4            | (DOD)            | 21Vianet          |
|         |               |                  |                   |

### **Permissions**

Depending on the resource you're getting the extended property from and the permission type (delegated or application) you request, the permission specified in the following table is the minimum required to call this API. To learn more, including how to choose permissions, see Permissions.

ノ **Expand table**

| calendar<br>contact<br>contactFolder<br>event<br>group calendar | Calendars.Read<br>Contacts.Read<br>Contacts.Read<br>Calendars.Read<br>Group.Read.All | Calendars.Read<br>Contacts.Read<br>Contacts.Read<br>Calendars.Read<br>Not supported | Calendars.Read<br>Contacts.Read<br>Contacts.Read<br>Calendars.Read<br>Not supported |
|-----------------------------------------------------------------|--------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| group event<br>group post                                       | Group.Read.All<br>Group.Read.All                                                     | Not supported<br>Not supported                                                      | Not supported<br>Group.Read.All                                                     |
| mailFolder                                                      | Mail.Read                                                                            | Mail.Read                                                                           | Mail.Read                                                                           |
| message                                                         | Mail.Read                                                                            | Mail.Read                                                                           | Mail.Read                                                                           |
| Outlook task                                                    | Tasks.Read                                                                           | Tasks.Read                                                                          | Not supported                                                                       |
| Outlook task<br>folder                                          | Tasks.Read                                                                           | Tasks.Read                                                                          | Not supported                                                                       |

## **HTTP request**

Get a resource instance expanded with the extended property which matches a filter on the **id** property. Make sure you apply URL encoding to the space characters in the filter string.

Get a **message** instance:

HTTP GET /me/messages/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}') GET /users/{id|userPrincipalName}/messages/{id}? \$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}') GET /me/mailFolders/{id}/messages/{id}? \$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}')

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **mailFolder** instance:

HTTP GET /me/mailFolders/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}') GET /users/{id|userPrincipalName}/mailFolders/{id}? \$expand=multiValueExtendedProperties(\$filter=id eq '{id\_value}')

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get an **event** instance:

HTTP

```
GET /me/events/{id}?$expand=multiValueExtendedProperties($filter=id eq
'{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get a **calendar** instance:

| HTTP                                                                        |  |
|-----------------------------------------------------------------------------|--|
|                                                                             |  |
| GET /me/calendars/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq |  |
| '{id_value}')                                                               |  |
| GET /users/{id userPrincipalName}/calendars/{id}?                           |  |
| \$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')          |  |

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **contact** instance:

#### HTTP

```
GET /me/contacts/{id}?$expand=multiValueExtendedProperties($filter=id eq
'{id_value}')
GET /users/{id|userPrincipalName}/contacts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /me/contactFolders/{id}/contacts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}/contacts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a **contactFolder** instance:

#### HTTP

```
GET /me/contactfolders/{id}?$expand=multiValueExtendedProperties($filter=id
eq '{id_value}')
GET /users/{id|userPrincipalName}/contactFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get an **outlookTask** instance:

| GET /me/outlook/tasks/{id}?\$expand=multiValueExtendedProperties(\$filter=id<br>eq '{id_value}')<br>GET /users/{id userPrincipalName}/outlook/tasks/{id}?<br>\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')<br>GET /me/outlook/taskFolders/{id}/tasks/{id}?                                                                                                                                                                       | HTTP                                                               |  |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|--|
| GET /users/{id userPrincipalName}/outlook/taskFolders/{id}/tasks/{id}?<br>\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')<br>GET /me/outlook/taskGroups/{id}/taskFolders/{id}/tasks/{id}?<br>\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}')<br>GET<br>/users/{id userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}/tasks<br>/{id}?\$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}') | \$expand=multiValueExtendedProperties(\$filter=id eq '{id_value}') |  |

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

#### Get an **outlookTaskFolder** instance:

HTTP

```
GET /me/outlook/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /me/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /users/{id|userPrincipalName}/outlook/taskGroups/{id}/taskFolders/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

#### 7 **Note**

Calling the /me endpoint requires a signed-in user and therefore a delegated permission. Application permissions aren't supported when using the /me endpoint.

Get a group **event** instance:

HTTP

```
GET /groups/{id}/events/{id}?$expand=multiValueExtendedProperties($filter=id
eq '{id_value}')
```

Get a group **post** instance:

HTTP

```
GET /groups/{id}/threads/{id}/posts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
GET /groups/{id}/conversations/{id}/threads/{id}/posts/{id}?
$expand=multiValueExtendedProperties($filter=id eq '{id_value}')
```

## **Path parameters**

ノ **Expand table**

| Parameter | Type   | Description                                                                                                                                 |
|-----------|--------|---------------------------------------------------------------------------------------------------------------------------------------------|
| id_value  | String | The ID of the extended property to match. It must follow one of the<br>supported formats. See Outlook extended properties overview for more |

### **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

### **Response**

If successful, this method returns a 200 OK response code.

The response body includes an object representing the requested resource instance, expanded with the matching multiValueLegacyExtendedProperty object.

## **Example**

#### **Request**

This example gets and expands the specified event by including a multi-value extended property. The filter returns the extended property that has its **id** matching the string StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name Recreation (with URL encoding removed here for ease of reading).

```
HTTP
```

GET

```
https://graph.microsoft.com/beta/me/events('AAMkAGE1M2_bs88AACbuFiiAAA=')?
$expand=multiValueExtendedProperties($filter=id%20eq%20'StringArray%20{66f5a
359-4659-4830-9070-00050ec6ac6e}%20Name%20Recreation')
```

#### **Response**

The response body includes all the properties of the specified event and extended property returned from the filter.

Note: The **event** object shown here is truncated for brevity. All of the properties will be returned from an actual call.

HTTP

```
HTTP/1.1 200 OK
Content-type: application/json
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#Me/events/$entity",
 "@odata.id": "https://graph.microsoft.com/beta/users('ddfcd489-628b-
40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-
709fad664b77')/events('AAMkAGE1M2_bs88AACbuFiiAAA=')",
 "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAm8k15A==\"",
 "id": "AAMkAGE1M2_bs88AACbuFiiAAA=",
 "start": {
 "dateTime": "2015-11-26T17:00:00.0000000",
 "timeZone": "UTC"
 },
 "end": {
 "dateTime": "2015-11-30T05:00:00.0000000",
 "timeZone": "UTC"
 },
 "organizer": {
 "emailAddress": {
 "name": "Christine Irwin",
 "address": "christine@contoso.com"
 }
 },
 "multiValueExtendedProperties": [
 {
 "id": "StringArray {66f5a359-4659-4830-9070-00050ec6ac6e} Name
Recreation",
 "value": [
 "Food",
 "Hiking",
 "Swimming"
 ]
 }
 ]
}
```

### **Feedback**

**Was this page helpful?**

**Yes No**

Provide product feedback

# **exchangeSettings resource type**

06/26/2025

#### Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Represents the Exchange settings for mailbox discovery.

### **Methods**

#### ノ **Expand table**

| Method | Return Type      | Description                                             |
|--------|------------------|---------------------------------------------------------|
| List   | exchangeSettings | Get a list of Exchange mailboxes that belong to a user. |

### **Properties**

ノ **Expand table**

| Property         | Type   | Description                                           |
|------------------|--------|-------------------------------------------------------|
| primaryMailboxId | String | The unique identifier for the user's primary mailbox. |

### **JSON representation**

The following JSON representation shows the resource type.

```
JSON
{
 "@odata.type": "#microsoft.graph.exchangeSettings",
 "primaryMailboxId": "String"
}
```

# **List Exchange settings**

06/26/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Get a list of Exchange mailboxes that belong to a user. Currently, the mailbox types supported are the user's primary mailbox and shared mailboxes. To learn how to get a list of users in a tenant, see List users.

This API is available in the following national cloud deployments.

ノ **Expand table**

| Global service | US Government L4 | US Government L5 (DOD) | China operated by 21Vianet |
|----------------|------------------|------------------------|----------------------------|
|                |                  |                        |                            |

### **Permissions**

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions only if your app requires it. For details about delegated and application permissions, see Permission types. To learn more about these permissions, see the permissions reference.

ノ **Expand table**

| Permission type                        | Least privileged permission | Higher privileged permissions |
|----------------------------------------|-----------------------------|-------------------------------|
| Delegated (work or school account)     | User.Read.All               | User.ReadWrite.All            |
| Delegated (personal Microsoft account) | Not supported.              | Not supported.                |
| Application                            | User.Read.All               | User.ReadWrite.All            |

### **HTTP request**

```
HTTP
```

```
GET /users/{id}/settings/exchange
```

## **Optional query parameters**

This method supports some of the OData query parameters to help customize the response. For general information, see OData query parameters.

## **Request headers**

ノ **Expand table**

| Name          | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| Authorization | Bearer {token}. Required. Learn more about authentication and authorization. |

## **Request body**

Don't supply a request body for this method.

## **Response**

If successful, this method returns a 200 OK response code and an exchangeSettings resource in the response body.

## **Examples**

### **Request**

The following example shows a request.

| HTTP |                                                                                |  |
|------|--------------------------------------------------------------------------------|--|
| HTTP |                                                                                |  |
|      | GET https://graph.microsoft.com/beta/users/megan@contoso.com/settings/exchange |  |

### **Response**

The following example shows the response.

**Note:** The response object shown here might be shortened for readability.

```
HTTP
HTTP/1.1 200 OK
Content-type: application/json
Content-length: 232
{
 "@odata.context":
"https://graph.microsoft.com/beta/$metadata#users('megan%40contoso.com')/settings/
exchange/$entity",
 "primaryMailboxId": "MBX:e0643f21@a7809c93"
}
```

## **exportItemResponse resource type**

06/26/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Represents the result of an export operation performed by the exportItems function.

## **Properties**

#### ノ **Expand table**

| Property  | Type          | Description                                                     |
|-----------|---------------|-----------------------------------------------------------------|
| changeKey | String        | The version of the item.                                        |
| data      | Stream        | Data that represents an item in a base64 encoded opaque stream. |
| error     | mailTipsError | An error that occurs during an action.                          |
| itemId    | String        | The unique identifier of the item.                              |

### **JSON representation**

The following JSON representation shows the resource type.

```
JSON
{
 "@odata.type": "#microsoft.graph.exportItemResponse",
 "changeKey": "String",
 "data": "String",
 "error": {"@odata.type": "microsoft.graph.mailTipsError"},
 "itemId": "String"
}
```

# **mailboxItemImportSession resource type**

06/26/2025

Namespace: microsoft.graph

#### ) **Important**

APIs under the /beta version in Microsoft Graph are subject to change. Use of these APIs in production applications is not supported. To determine whether an API is available in v1.0, use the **Version** selector.

Provides information about how to import items into a user's mailbox.

## **Properties**

ノ **Expand table**

| expirationDateTime | DateTimeOffset | The date and time in UTC when the import session expires. The<br>date and time information uses ISO 8601 format and is always in |
|--------------------|----------------|----------------------------------------------------------------------------------------------------------------------------------|
|                    |                | UTC. For example, midnight UTC on Jan 1, 2021 is 2021-01-<br>01T00:00:00Z .                                                      |
| importUrl          | String         | The URL endpoint that accepts POST requests for uploading a<br>mailbox item exported using exportItems.                          |

### **JSON representation**

The following JSON representation shows the resource type.

```
JSON
{
 "@odata.type": "#microsoft.graph.mailboxItemImportSession",
 "expirationDateTime": "String (timestamp)", 
 "importUrl": "String"
}
```