---
Description: This sample builds on the Auto Claims Form sample by integrating the PenInputPanel object. The sample is in the C\# PIPanel directory in the AutoClaims folder.
ms.assetid: 9a2c1565-fb24-4767-bfa5-0257129f4bd4
title: PenInputPanel Sample
ms.date: 05/31/2018
ms.topic: article
ms.author: windowssdkdev
ms.prod: windows
ms.technology: desktop
---

# PenInputPanel Sample

This sample builds on the Auto Claims Form sample by integrating the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object. The sample is in the C\# PIPanel directory in the AutoClaims folder.

> [!Note]  
> This sample requires that your system is equipped with a pen device. If you are using just a mouse (or other non-human interface device (HID) pointing device) the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) does not appear.

 

For more information about the Auto Claims Form sample, see [Auto Claims Form Sample](auto-claims-form-sample.md). For more information about the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object, see [Programming the Input Panel Using the PenInputPanel Class](programming-the-input-panel-using-the-peninputpanel-class.md).

In the sample, the Auto Claims Form contains five fields into which the user is asked to put information relevant to the claim: policy number, insured name, the year, make, and model of the car. A [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object is attached to each input field to provide an easy means for entering values with a pen.

There are two techniques for attaching a [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object to the input fields on your form. The first technique is to assign a separate instance of the object to each input field at design time. The second is to create a single instance of the object and then attach that object instance at run time to a field when it receives focus. This sample demonstrates both techniques.

There are tradeoffs involved in deciding which technique to use. Creating a unique instance of the object for each form field requires slightly more memory when the form is loaded. However, it saves having to handle focus events for the fields to assign a single instance to the current field at run time.

Because the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object is supported only on a Tablet PC, the sample creates the PenInputPanel objects within an exception-handling block.

## One Object per Field

The sample demonstrates the first technique (one [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object per field) by assigning the input fields for policy number (`inkEdPolicyNumber`) and insured name (`inkEdName`) a unique instance of the PenInputPanel object. An overloaded constructor for the PenInputPanel object can take the name of the input control as an argument, thus associating the controls. The following lines from the form's [Load](frlrfSystemWindowsFormsFormClassLoadTopic) event handler shows this:


```C++
pipPolicyNumber = new PenInputPanel(inkEdPolicyNumber);
pipName = new PenInputPanel(inkEdName);
```



## One Object per Form

The second technique is also shown in the sample: a single instance of a [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object, `pipShared`, is shared between the Year, Make, and Model input fields. The shared object is created by using the default constructor.


```C++
pipShared = new PenInputPanel();
```



Using this technique requires that your form have only a single instance of the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object. This saves memory, but you must add code to handle the event when an input field receives focus. When a control that uses a shared instance of a PenInputPanel object gets focus, set the PenInputPanel object's [AttachedEditControl](frlrfMicrosoftInkPenInputPanelClassAttachedEditControlTopic) property to that control. The following code shows an event handler for the Year, Make, and Model fields' `Enter` events.


```C++
private void inkEdYear_Enter(object sender, System.EventArgs e)
{
    // Attach the shared PenInputPanel to the Year field
    pipShared.AttachedEditControl = inkEdYear;

    // set the NUMBER factoid to bias recognition for numbers
    pipShared.Factoid = "NUMBER";

    // Enable correction UI on the inkEdYear field
    pipShared.EnableTsf(true);
}

private void inkEdMake_Enter(object sender, System.EventArgs e)
{
    // Attach the shared PenInputPanel to the Make field
    pipShared.AttachedEditControl = inkEdMake;

    // reset the factoid to bias recognition for general text
    pipShared.Factoid = "DEFAULT";

    // Enable correction UI on the inkEdMake field
    pipShared.EnableTsf(true);
}
private void inkEdModel_Enter(object sender, System.EventArgs e)
{
    // Attach the shared PenInputPanel to the Model field
    pipShared.AttachedEditControl = inkEdModel;

    // reset the factoid to bias recognition for general text
    pipShared.Factoid = "DEFAULT";

    // Enable correction UI on the inkEdModel field
    pipShared.EnableTsf(true);
}
```



Be sure to set any properties that need to be set when focus changes to a new control. In the previous event handlers, for example, the [Factoid](frlrfMicrosoftInkPenInputPanelClassFactoidTopic) property is set as appropriate.

## Usability Considerations

Keep the following usability considerations in mind when using the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object in your application.

### Positioning the PenInputPanel

Because the fields are laid out vertically on the form in this sample, the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) user interface for each input control is positioned slightly to the right of the input control to make it easier to use. This prevents the PenInputPanel from covering up the next edit box, making it easier to target the next edit box.


```C++
pipShared.HorizontalOffset = 32;
pipPolicyNumber.HorizontalOffset = 32;
pipName.HorizontalOffset = 32;
```



### Selecting Input Panel to Display

Because policy numbers are often combinations of numbers, letters, and other characters, they can be prone to recognition errors. Therefore, the sample sets the default panel displayed by the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object to be the keyboard when it is attached to the policy number field.


```C++
pipPolicyNumber.DefaultPanel = PanelType.Keyboard;
```



The default behavior of the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object is to use the panel the user selected last.

### Text Services Framework Correction User Interface

In this sample all of the input fields are [InkEdit](frlrfMicrosoftInkInkEditClassTopic) controls. This is significant because the InkEdit control has built-in support for the [Text Services Framework](tsf.text_services_framework) (TSF) and is thus capable of supporting the in-place correction user interface for input received from the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object.

The default value for [EnableTsf](frlrfMicrosoftInkPenInputPanelClassEnableTsfTopic) is **TRUE**. This causes the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) object to attempt to start the Text Services Framework (TSF) on the attached control. If successful, the correction user interface shows in the control, and it allows access to recognition alternates. Calling this method with a **FALSE** parameter attempts to shut down TSF on the attached control.

The [InkEdit](frlrfMicrosoftInkInkEditClassTopic) control already provides a correction user interface, but in the sample [EnableTsf](frlrfMicrosoftInkPenInputPanelClassEnableTsfTopic) is used to enable the [PenInputPanel](frlrfMicrosoftInkPenInputPanelClassTopic) to use the TSF insertion recognizer context rather than the [**SendInput**](inputdev.sendinput) function to send the handwriting recognition results into the control. The result is that text can be inserted even if the field no longer has focus.


```C++
  pipName.EnableTsf(true);
  pipPolicyNumber.EnableTsf(true);
```



## Closing the Form

In the Windows Form Designer generated code, the [InkEdit](frlrfMicrosoftInkInkEditClassTopic) and [InkPicture](frlrfMicrosoftInkInkPictureClassTopic) controls are added to the form's component list when the form is initialized. When the form closes, the InkEdit and InkPicture controls are disposed, as well as the other components of the form, by the form's [Dispose](frlrfSystemWindowsFormsFormClassDisposeTopic) method. The form's Dispose method also disposes the [Ink](frlrfMicrosoftInkInkClassTopic) objects that are created for the form.

 

 


