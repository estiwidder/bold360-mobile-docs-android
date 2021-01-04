---
layout: default
title: Incoming options
np_toc: true
nav_exclude: true
---

# Incoming message options {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Persistent options
Incoming chatbot responses can have persistent options, which will not dissappear after a user selection.   

### How to customize
THe chatbot incoming element can be customized by overriding the default implementation of the PersistentOptionsUIProvider.
- The stype options are sutomized in `PersistentOptionsUIProvider.optionsStyleConfig`   
- The wrapping bubble can be customized in `PersistentOptionsUIProvider.contentStyleConfig`
  ```kotlin
  ChatUIProvider(context).apply {
      
      chatElementsUIProvider.incomingUIProvider.persistentOptionsUIProvider.apply {
          // customize options text style:
          optionsStyleConfig = StyleConfig(...)
          
          // customize the wrapping bubble look:
          contentStyleConfig = { adapter -> 
              adapter.textBackground(...)
              
              // do more adjustments
          }
      }    
  }
  ```

- The options ldisplay can be also customized by overriding the following:
```kotlin
ChatUIProvider(context).apply {
    
    chatElementsUIProvider.incomingUIProvider.persistentOptionsUIProvider.overrideFactory = 
                object : UIInfoFactory {
                    override fun info(): ViewInfo {
                        /* return your own ViewInfo object - 
                                which defines the layout resource for the options */
                    }
                }
}
```

## QuickOptions
The chatbot responses can have different options for the user to choose from. These options disappear after the user's action.

### How to customize
1. Customization by overriding the default with your own layout resource:  
    {: .strong-sub-title}   

    
    ```kotlin
    ChatUIProvider(context).apply {
        
        chatElementsUIProvider.incomingUIProvider.quickOptionsUIProvider.overrideFactory = 
            object : QuickOptionUIProvider.QuickOptionsFactory {
                override fun info(): ViewInfo {
                    /* return your own ViewInfo object - 
                                    which defines the layout resource for the options */
                }
            }
    ```
2. Customization by changing properties on the `QuickOptionsUIProvider`:   
   {: .strong-sub-title} 

    ```kotlin
    uiProvider.chatElementsUIProvider
        .incomingUIProvider.quickOptionsUIProvider.apply { 
                optionsMargin = xxx // dp value
                startMargin = xxx // dp value
        }
    ```

## Channels
- Channels are a sub type of QuickOptions. Channels provide  the user with escalation paths.   
- Channels may appear as a response options or as part of an article page.  
- Channels are defined in the [Bold360ai console](https://support.bold360.com/ai).

### <U>Customizing the channel's icon</U>
- #### Via the Bold360ai console:
    ![]({{'/assets/images/console-channeling-icons.png' | relative_url}})
    {: .image-40}

- #### Via the SDK, by overriding the SDK's default icons. Two options are available as see bellow: 
    
   - Overriding the channel's drawable resources:

        - Phone : `R.drawable.call_channel`
        - Chat : `R.drawable.chat_channel`
        - Ticket: `R.drawable.email_channel`

    - Overriding the icons generating method:

      ```kotlin
      ChatUIProvider(context).apply {
        chatElementsUIProvider.incomingUIProvider.quickOptionsUIProvider.overrideFactory =
            object : QuickOptionUIProvider.QuickOptionsFactory {
                override fun generateDefaultChannelImage(context: Context, channelType: Int): Drawable? {
                    // return drawable according to channelType
                }
            }
      ```
