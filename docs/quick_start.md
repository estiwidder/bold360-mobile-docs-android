---
layout: default
title: Quick Start
nav_order: 2
---

# Quick Start
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

The Quick Start guide provides the steps for embeddinga basic AI or live chat in your app.   

## System Requirements  

* Java 8 or higher.
* Gradle 5.3.6 or higher.
* Android Studio.
* The SDK supports devices with API level 16 or higher. <sub>**(since version 3.5.0**)</sub>


## SDK Set up 
{: .d-inline-block }

1. ### Import SDK dependencies 
    {: .no_toc .strong-sub-title}   
    
    [Check here for the latest SDK version and needed dependencies](https://developer.bold360.com/help/EN/Bold360API/Bold360API/ReleaseNotesAndroid.html) 

    
2. ### Extra configurations on build.gradle:
    {: .no_toc .strong-sub-title}  
   
    Add the following to the _`android{...}`_ block definition

    ```gradle
    android {
        kotlinOptions {
            freeCompilerArgs = ['-Xjvm-default=compatibility']
            jvmTarget = "1.8"
        }

        //overcome the following error: "More than one file was found with OS independent path..."
        packagingOptions{
            pickFirst 'META-INF/atomicfu.kotlin_module'
            
            // those may also be needed:
            pickFirst 'META-INF/main_debug.kotlin_module'
            pickFirst 'META-INF/main_release.kotlin_module'
        }
    }
    ```

---

> ! Now you are ready to integrate and create some chats.

---

## Define chat type   
{: .d-inline-block }

The Android SDK enables embedding different types of chats: AI, live or messaging. The Account defines the chat type.
Click on the specific link below for each of the respective chat types. Then you will be able to advance to starting a chat. 

### Create an Account
{: .d-inline-block }

- [Create `BotAccount` for chat with AI]({{'/docs/chat-configuration/chat-account/bot-chat#botaccount' | relative_url}})
    {: .no_toc }    
- [Create `BoldAccount` for live chat with agent]({{'/docs/chat-configuration/chat-account/bold-chat#boldaccount' | relative_url}})
    {: .no_toc }
- [Create `AsyncAccount` to start a messaging chat]({{'/docs/advanced-topics/messaging-chat#asyncaccount' | relative_url}})
    {: .no_toc }  

---

## Start a chat  
{: .d-inline-block }

1. ### Create [ChatController]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}})

    The ChatController enables creating and controling he different chats.
    The chat type is defined by the Account provided at chat creation.

    ```kotlin
    val chatController = ChatController.Builder(context)
                                          .build(account, ...)
    ...
    // start a new chat, using same chatController:
    chatController.startChat(account)

    // restore active chat or starts new chat
    chatController.restoreChat(fragment?, account?)
    ```
---

2. ### Add the chat fragment to your activity

    Implement the ChatLoadedListener interface and pass it in the `ChatController.Builder` build method.   
    Once the chat build succeeded and the fragment is ready to be displayed, `onComplete` will be called with the fragment on the result. 

    ```kotlin
    ChatController.Builder(context).build(account, object : ChatLoadedListener {
            override fun onComplete(result: ChatLoadResponse) {
                result.error?.run{
                  // report Chat load failed
                } ?: run{
                  // add result.getFragment() to the applications activity.
                }
            }
        })
    ```
---

##### **Notice** 
{: .no_toc } 
> Make sure to activate `ChatController.destruct()`, when the ChatController instance is no longer needed (exp: Chats section in app
  is being closed or the ChatController instance is being replaced). Destruct will verify resources release (including open sockets and communication channels).
    

---

### Code Sample
{: .no_toc .text-delta}
[bold360ai samples](https://github.com/bold360ai/bold360-mobile-samples-android)
-
