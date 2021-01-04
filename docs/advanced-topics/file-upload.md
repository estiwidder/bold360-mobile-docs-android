---
layout: default
title: File Upload
parent: Advanced Topics
nav_order: 9
# permalink: /docs/advanced-topics/file-upload
---

# File Upload {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
Files can be uploaded to the chat either by the SDK uplaod mechanism ,or by a proprietary mechanism.   
{: .overview}

Follow the next steps to integrate the upload ability onto your chat:

> If you are not using the SDK for the chat ui creation, but still want to use the SDK default file upload mechanism, go to [step 2.](#Use-SDK's-provided-uploader)

## 1.  Define the file upload trigger   
This defines the UI component to be used by the user to trigger file upload. 

- ### Default file upload trigger by the SDK   
  The SDK provides an upload trigger (attach icon) which will be displayed within input field. To be notified when a user activates the trigger toupload files, there is a  need to implement `onUploadFileRequest` in the  `ChatEventListener`.
  ```kotlin
    override fun onUploadFileRequest(){
        // The user triggers an upload content for the agent
    }
  ```

  - Customizing the upload icon: 
  {: .light-title}
    through the `ChatInputUIProvider` configuration object.

    ```kotlin
    val chatController = ChatController.Builder(this).apply {
        ...
        chatUIProvider(ChatUIProvider().apply{
            chatInputUIProvider.uiConfig.uploadImage(...)
        })
        ...
    }
    ```

- ### Proprietary file upload trigger  
  - Apply a proprietary file upload UI component.   
    Ensure that the upload feature is enabled, before displaying the UI to the user:
    ```kotlin
    chatController.isEnabled(ChatFeatures.FileUpload);
    ```
    > Llearn more on how to enable/disable the file transfer on the admin console [here]({{'/assets/images/console-upload.png' | relative_url}})

  - Hide the SDKs upload icon:

    ```kotlin
    val chatController = ChatController.Builder(this).apply {
        ...
        chatUIProvider(ChatUIProvider().apply{
            chatInputUIProvider.uiConfig.showUpload = false
        })
        ...
    }
    ```


## 2. Choose the file upload provider

- ### Use SDK's provided uploader

  - Create a `FileUploadInfo` object, for each content to be uploaded.

    ```kotlin
    //... user selected the file to be uploaded
    val uploadInfo = FileUploadInfo().apply{
        type = ... // as defined in @FileType
        name = ... // can differ from the actual file name
        filePath = ... // actual selected file path
        content = file.readBytes()... // if it was not provided on the constructor
    }
    ```
     
## 3. Activate the file upload mechanism
    
      When using the SDK's provided uploader

        ```kotlin
        BoldLiveUploader().upload(uploadInfo, AccountDetails(...)) { uploadResults ->
            //.... got UploadResults and do whatever
            uploadsResults.error?.run{
                Log.e(TAG, "Got an error on ${uploadResults.data.name} 
                                    file upload: ${uploadsResults.error}")
                ...
            }
        }
    
    
    When using the ChatController 
    
        ```kotlin
        chatController.uploadFile(uploadInfo) { uploadResults ->
            //.... got UploadResults and do whatever
            uploadsResults.error?.run{
                Log.e(TAG, "Got an error on ${uploadResults.data.name} 
                                    file upload: ${uploadsResults.error}")
                ...
            }
        }
        ```
    Upload results are passed over the provided callback.    ```


- ### Use a proprietary file upload mechanism 

    - When user triggers an upload and chooses the content to be uploaded, upload that content with your uploader.
    - Pass an UploadEvent to the `chatController` with the upload results, in order to have the upload bubble in the chat.

      ```kotlin
      MyUploader.upload(...){
          //.... do upload stuff

          // pass results to the SDK
          val uploadResults = UploadResults(FileUploadInfo?, NRError?)
          chatController.handleEvent(Upload, new UploadEvent(uploadResult));
      }
      ```

## Extra
##  COMMENT: WHY IS THIS EXTRA? IS THIS HEADER NEEDED?

### Listening to file upload notifications

The app can listen to file upload events via the `ChatController`, by registering to the available uploads notifications needed.

```kotlin
chatController.subscribeNotifications(notifiableImpl:Notifiable,
                    Notifications.UploadEnd, Notifications.UploadStart,
                    Notifications.UploadProgress, Notifications.UploadFailed)

// Notifiable implementation (notifiableImpl):
{
    ...

    override fun onNotify(notification: Notification, dispatcher: DispatchContinuation) {

        when (notification.notification) {
            Notifications.UploadStart,
            Notifications.UploadEnd,
            Notifications.UploadFailed -> {
                (notification as UploadNotification).apply {
                    // uploadInfo:FileUploadInfo member is available
                    ...
                }
            }

            Notifications.UploadProgress -> {
                (notification as ProgressNotification).apply {
                    // uploadInfo:FileUploadInfo member is available
                    // progress:Int member is also available
                    ...
                }
            }
        }
    }
}
```
## <a name="uicustom"/>UI Customization

- #### Customizing the upload progress indication
   The SDK provides an upload propress indication bar   

    ![]({{'assets/images/uploads_bar.png' | relative_url}})

    - The uploads bar can customized via `ChatUIProvider.uploadsCmpUIProvider.configure`.

        ```kotlin
        // display uploads summary bar as floating component 
        chatUIProvider.uploadsCmpUIProvider.configure = { adapter:UploadsCmpAdapter->
            adapter.apply {
                ...            
            }
        }
        ```

    - The uploads bar can be overriden by a proprietary implementation, using `ChatUIProvider.uploadsCmpUIProvider.overrideFactory`   
      ```kotlin
      chatUIProvider.uploadsCmpUIProvider.overrideFactory =
            object: UploadsbarCmpUIProvider.UploadFactory {
                override fun create(context: Context): UploadsCmpAdapter {
                    return .... // created custom component
                }
            }
      ```
      
    - In order to notify the SDK that the component is done with the uploadsand therefore the bar should be removed, pass the `CmpEvent` to the `ChatController`:

        ```kotlin
        chatController.handleEvent(CmpEvent.EventName,
                        CmpEvent(ComponentType.UploadsStripCmp, CmpEvent.Idle))
        ```
      > Customizations that were configured with `UploadsCmpUIProvider.configure` will be applied on proprietary component as well.

- #### Customizing upload outgoing bubble
  This customixation is the same customization done to a regular outgoing bubble  [outgoing bubble customizations]({{'/docs/chat-configuration/ui-customization/how-it-works' | relative_url}}).

  - ##### Customizing the upload file images
    Default icons can be overriden by appying new drawables resources in the app resources with matching reasources ids.   
    > picture_ico, default_ico, excel_ico, archive_ico
---
