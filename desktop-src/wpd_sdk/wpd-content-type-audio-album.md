---
Description: WPD\_CONTENT\_TYPE\_AUDIO\_ALBUM
ms.assetid: 41b7a587-01e0-4eee-b750-2f6be06c3227
title: WPD\_CONTENT\_TYPE\_AUDIO\_ALBUM
ms.date: 05/31/2018
ms.topic: article
ms.author: windowssdkdev
ms.prod: windows
ms.technology: desktop
---

# WPD\_CONTENT\_TYPE\_AUDIO\_ALBUM

An object that describes its type as WPD\_CONTENT\_TYPE\_AUDIO\_ALBUM represents a collection of audio files. An audio album is functionally equivalent to a playlist of audio files, but is used to represent a physical album to the user.

This type of object supports the following properties.



| Property Name                                                                                                         | Required or Optional                                                           |
|-----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| [WPD\_OBJECT\_ID](object-properties.md#wpd-object-id)                                                                | Required, read-only. A client cannot set this property, even at creation time. |
| [WPD\_OBJECT\_PARENT\_ID](object-properties.md#wpd-object-parent-id)                                                 | Required.                                                                      |
| [WPD\_OBJECT\_NAME](object-properties.md#wpd-object-name)                                                            | Required if the object represents a file.                                      |
| [WPD\_OBJECT\_PERSISTENT\_UNIQUE\_ID](object-properties.md#wpd-object-persistent-unique-id)                          | Required, read-only. A client cannot set this property, even at creation time. |
| [WPD\_OBJECT\_FORMAT](object-properties.md#wpd-object-format)                                                        | Required.                                                                      |
| [WPD\_OBJECT\_CONTENT\_TYPE](object-properties.md#wpd-object-content-type)                                           | Required.                                                                      |
| [WPD\_OBJECT\_ISHIDDEN](object-properties.md#wpd-object-ishidden)                                                    | Required if the object is hidden.                                              |
| [WPD\_OBJECT\_ISSYSTEM](object-properties.md#wpd-object-issystem)                                                    | Required if the object is a system object (represents a system file).          |
| [WPD\_OBJECT\_SIZE](object-properties.md#wpd-object-size)                                                            | Required if the object has at least one resource.                              |
| [WPD\_OBJECT\_ORIGINAL\_FILE\_NAME](object-properties.md#wpd-object-original-file-name)                              | Required if the object represents a file.                                      |
| [WPD\_OBJECT\_NON\_CONSUMABLE](object-properties.md#wpd-object-non-consumable)                                       | Recommended if the object is not meant for consumption by the device.          |
| [WPD\_OBJECT\_REFERENCES](object-properties.md#wpd-object-references)                                                | Required if the object has references to other objects.                        |
| [WPD\_OBJECT\_KEYWORDS](object-properties.md#wpd-object-keywords)                                                    | Optional.                                                                      |
| [WPD\_OBJECT\_SYNC\_ID](object-properties.md#wpd-object-sync-id)                                                     | Optional.                                                                      |
| [WPD\_OBJECT\_IS\_DRM\_PROTECTED](object-properties.md#wpd-object-is-drm-protected)                                  | Required if the object is protected by DRM technology.                         |
| [WPD\_OBJECT\_DATE\_CREATED](object-properties.md#wpd-object-date-created)                                           | Optional.                                                                      |
| [WPD\_OBJECT\_DATE\_MODIFIED](object-properties.md#wpd-object-date-modified)                                         | Recommended.                                                                   |
| [WPD\_OBJECT\_DATE\_AUTHORED](object-properties.md#wpd-object-date-authored)                                         | Optional.                                                                      |
| [WPD\_OBJECT\_BACK\_REFERENCES](object-properties.md)                                                                | Recommended if the object is referenced by another object.                     |
| [WPD\_OBJECT\_CONTAINER\_FUNCTIONAL\_OBJECT\_ID](object-properties.md#wpd-object-container-functional-object-id)     | Optional.                                                                      |
| [WPD\_OBJECT\_GENERATE\_THUMBNAIL\_FROM\_RESOURCE](object-properties.md#wpd-object-generate-thumbnail-from-resource) | Optional.                                                                      |
| [WPD\_OBJECT\_CAN\_DELETE](object-properties.md)                                                                     | Required if the object cannot be deleted.                                      |
| [WPD\_OBJECT\_LANGUAGE\_LOCALE](object-properties.md)                                                                | Optional.                                                                      |



 

## Typical Resources

These objects typically do not host resources.

## Related topics

<dl> <dt>

[**Requirements for Objects**](requirements-for-objects.md)
</dt> </dl>

 

 


