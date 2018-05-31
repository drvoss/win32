---
Description: The StylusInput APIs allow you to interact with the tablet pen data stream. To interact with the data stream add a RealTimeStylus object to your application, and add plug-ins to the RealTimeStylus object.
ms.assetid: 88bab0ab-df9f-4813-9a9f-9a137813f0b4
title: Architecture of the StylusInput APIs
ms.date: 05/31/2018
ms.topic: article
ms.author: windowssdkdev
ms.prod: windows
ms.technology: desktop
---

# Architecture of the StylusInput APIs

The StylusInput APIs allow you to interact with the tablet pen data stream. To interact with the data stream add a [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object to your application, and add plug-ins to the **RealTimeStylus** object.

Two plug-ins are provided in the StylusInput APIs. The [**DynamicRenderer**](/windows/win32/RTSCom/?branch=master) object implements the [**IStylusSyncPlugin**](/windows/win32/RTSCom/?branch=master) interface. The **DynamicRenderer** object renders the ink in real time, as it is being drawn. The [**GestureRecognizer**](/windows/win32/RTSCom/?branch=master) object implements the **IStylusSyncPlugin** and [**IStylusAsyncPlugin**](/windows/win32/RTSCom/?branch=master) interfaces. The **GestureRecognizer** object recognizes application gestures.

## Definitions

The following terms are used in the sections describing the StylusInput APIs:

Synchronous plug-in

A class that implements the [**IStylusSyncPlugin**](/windows/win32/RTSCom/?branch=master) interface. Synchronous plug-ins are generally called directly by the [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object.

Asynchronous plug-in

A class that implements the [**IStylusAsyncPlugin**](/windows/win32/RTSCom/?branch=master) interface. Asynchronous plug-ins are generally called on the application's user interface (UI) thread.

Synchronous plug-in collection

A [StylusSyncPluginCollection](frlrfmicrosoftstylusinputstylussyncplugincollectionclasstopic) collection, which is an ordered collection of [IStylusSyncPlugin](frlrfmicrosoftstylusinputistylussyncpluginclasstopic) objects. A synchronous plug-in collection usually refers to the collection assigned to the [SyncPluginCollection](frlrfmicrosoftstylusinputrealtimestylusclasssyncplugincollectiontopic) property of a [RealTimeStylus](frlrfmicrosoftstylusinputrealtimestylusclasstopic) object. Only synchronous plug-ins may be added to a synchronous plug-in collection.

Asynchronous plug-in collection

A [StylusAsyncPluginCollection](frlrfmicrosoftstylusinputstylusasyncplugincollectionclasstopic) collection, which is an ordered collection of [IStylusAsyncPlugin](frlrfmicrosoftstylusinputistylusasyncpluginclasstopic) objects. An asynchronous plug-in collection usually refers to the collection assigned to the [AsyncPluginCollection](frlrfmicrosoftstylusinputrealtimestylusclassasyncplugincollectiontopic) property of a [RealTimeStylus](frlrfmicrosoftstylusinputrealtimestylusclasstopic) object. Only asynchronous plug-ins may be added to an asynchronous plug-in collection.

## Synchronous and Asynchronous Plug-ins

The [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object is designed to provide real-time access to the data stream from a tablet pen. Create or use synchronous plug-ins for tasks that require real-time access to the data stream and are computationally undemanding, such as for packet filtering. Create or use asynchronous plug-ins for tasks that do not require real-time access to the data stream, such as for creating and storing strokes in an [**InkDisp**](/windows/win32/msinkaut/?branch=master) object.

Certain tasks may be computationally demanding yet require real-time access to the data stream, such as multistroke gesture recognition. To address these needs, the StylusInput APIs provide a cascaded [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) model that allows you to use two **RealTimeStylus** objects, each running on its own thread. For more information about the cascaded **RealTimeStylus** model, see [The Cascaded RealTimeStylus Model](the-cascaded-realtimestylus-model.md).

For more information about using and creating plug-ins, see [Working with the StylusInput APIs](working-with-the-stylusinput-apis.md).

## The Tablet Pen Data Stream

The [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object has two internal queues that carry the tablet pen data, the input queue and the output queue. The pen data is converted into instances of the classes in the [Microsoft.StylusInput.PluginData](frlrfmicrosoftstylusinputplugindata) namespace. The following list describes how the **RealTimeStylus** object handles the tablet pen data:

The [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object checks for plug-in data objects first on its input queue and then from the tablet pen data stream.

The [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object sends one plug-in data object to the objects in its synchronous plug-in collection. Each synchronous plug-in can add data to either the input or output queue.

After the plug-in data object has been sent to all members of the synchronous plug-in collection, the plug-in data object is placed on the [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object's output queue.

The [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object then checks for the next plug-in data object to process.

While the [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object's output queue contains data, the **RealTimeStylus** object sends one plug-in data object from its output queue to the objects in its asynchronous plug-in collection. Each asynchronous plug-in can add data to either the input or output queue. However, because the asynchronous plug-ins run on the UI thread, the data is added to the queue in relation to the current pen data the **RealTimeStylus** object is processing, and not in relation to the data that the asynchronous plug-in is processing.

The following diagram illustrates the flow of tablet pen data through the [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object and its plug-in collections.

![flow of tablet pen data through the realtimestylus object and its plug-in collections](images/5a698bc9-833b-4b24-9fa2-70be0ca61032.gif)

In this diagram, the circles labeled "A" and "B" represent tablet pen data that has already been added to the [**RealTimeStylus**](/windows/win32/RTSCom/?branch=master) object's output queue and that has not yet been sent to the asynchronous plug-in collection. The circle labeled "C" represents the tablet pen data that the **RealTimeStylus** object is currently processing. It is sent to the synchronous plug-in collection and placed on the output queue. The empty circle represents the position in the output queue where future tablet pen data is added.

For more information about how specific data is added to the queue and processed, see [Plug-in Data and the RealTimeStylus Class](plug-in-data-and-the-realtimestylus-class.md).

## The StylusInput APIs

The StylusInput APIs reside primarily in the [Microsoft.StylusInput](frlrfmicrosoftstylusinput) and [Microsoft.StylusInput.PluginData](frlrfmicrosoftstylusinputplugindata) namespaces. However, the StylusInput APIs also reference some classes in the [Microsoft.Ink](frlrfMicrosoftInk) namespace, such as the [Tablet](frlrfMicrosoftInkTabletClassTopic) class, the [TabletPropertyDescriptionCollection](frlrfMicrosoftInkTabletPropertyDescriptionCollectionClassTopic) collection, and the [ApplicationGesture](frlrfMicrosoftInkApplicationGestureClassTopic) and [SystemGesture](frlrfMicrosoftInkSystemGestureClassTopic) enumerations.

## Related topics

<dl> <dt>

[**DynamicRenderer**](/windows/win32/RTSCom/?branch=master)
</dt> <dt>

[**GestureRecognizer**](/windows/win32/RTSCom/?branch=master)
</dt> <dt>

[**RealTimeStylus**](/windows/win32/RTSCom/?branch=master)
</dt> <dt>

[**IStylusAsyncPlugin**](/windows/win32/RTSCom/?branch=master)
</dt> <dt>

[**IStylusSyncPlugin**](/windows/win32/RTSCom/?branch=master)
</dt> <dt>

[Working with the StylusInput APIs](working-with-the-stylusinput-apis.md)
</dt> <dt>

[The Cascaded RealTimeStylus Model](the-cascaded-realtimestylus-model.md)
</dt> </dl>

 

 


