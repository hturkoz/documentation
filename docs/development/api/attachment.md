# API :: Attachment

* [Uploading](#uploading)
* [Downloading](#downloading)

### Uploading

`POST Attachment`

* Method: *POST*
* Action: `Attachment`
* Headers: `Content-Type: application/json`

Payload fields:

* name - file name;
* type - mime type;
* role - specify value `Attachment`;
* relatedType - entity type attachment is related to (only for fields of *File* type);
* relatedId - record ID attachment is related to (only for fields of *File* type);
* parentType - entity type attachment is related to (only for fields of *Attachment Multiple* type);
* parentId - record ID attachment is related to (only for fields of *Attachment Multiple* type);
* field - field name of related record attachment is related through;
* file - file contents.

#### Example (File type)

The attachment to be stored in the field of *File* type. 

Payload:
```json
{
    "name": "test.txt",
    "type": "text/plain",
    "role": "Attachment",
    "relatedType": "Document",
    "field": "file",
    "file": "data:text/plain;base64,FILE_CONTENTS_ENCODED_WITH_BASE64"
}
```

Returns attachment record attributes:

```json
{
    "id": "some-id",
    "name": "test.txt"
}
```

Returned ID can be used in the following API call that creates or updated a record to which the attachment is supposed to be related to. In our example we create a record of *Document* entity type. You will need to fill *fileId* attribute with ID returned after *POST Attachment* request.


#### Example (Attachment-Multiple type)

The attachment to be stored in the field of *Attachment-Multiple* type. 

Payload:
```json
{
    "name": "test.txt",
    "type": "text/plain",
    "role": "Attachment",
    "parentType": "Note",
    "parentId": "id-of-note-record",
    "field": "attachments",
    "file": "data:text/plain;base64,FILE_CONTENTS_ENCODED_WITH_BASE64"
}
```

This request will upload attachment and will relate to existing Note (since *parentId* is specified).

It's possible to upload attachment before Note creation. When create Note, you need to specify attachment ID in *attachmentsIds* attribute:

```json
{
    "attachmnetsIds": ["id-of-attachment"]
}
```

### Downloading

Available since version 5.9.0.

`GET Attachment/file/{id}`

Where `{id}` is an ID of the attachment record.

Returns attachment contents.
