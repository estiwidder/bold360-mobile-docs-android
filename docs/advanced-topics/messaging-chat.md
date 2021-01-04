---
layout: default
title: Messaging Chat
parent: Advanced Topics
nav_order: 5

---

# Messaging Chat {{site.data.vars.need-review}}
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
A messaging chat enables both the User and the live agent to connect to the chat at any time to post messages. Posting a message in the chat does not require both parties to be connected at the same time.   
{: .overview}
The SDK enables the app to create and reconnect to  messaging chat and session.   
Messaging chat configurations are provided by the hosting App over an `AsyncAccount` instance, and passed to the ChatConltroller upon chat creation.
{: .overview}

--- 

## AsyncAccount
The AsyncAccount should be used to create a messaging chat session. 
This account creates the chat and provide details and configurations including: user details, a configuration for continuing a previous chat. 

### Create account
In order to create a messaging chat, a valid apiKey and applicationId are required.

```kotlin
val account = AsyncAccount(API_KEY, APPLICATION_ID)
```

### Session Configurations
- **applicationId <sub>[mandatory]</sub>** - An Id  created by a Bold account administrator. The applicationId can be provided upon account creation or later set in the session info property.  
`account.info.applicationId = APPLICATION_ID`

- **userInfo <sub>[optional]</sub>** - Defines the user that participates in the chat. 
A messaging `Conversation` is a collection of chat sessions which refer to the same user, defined by the userId.
`UserInfo` defines the link of chat sessions to a user and provides user details that will then be viewed by the agent that accepts the chat.   

  > By default, an account is created with a new UserInfo and an automatically generated userId.

  * <a id="relate"></a> **In order to link chat sessions to a specific user**, set a UserInfo object with the specific userId value, in the account session configuration.   
  `account.info.userInfo = UserInfo(USER_ID)`

  * **In order to provide user details**, such as first/last name, email etc., provide them on your created UserInfo object or access the default one.

    ```kotlin
    account.info.userInfo = UserInfo(USER_ID).apply{
        firstName = FIRST_NAME // optional
        lastName = LAST_NAME // optional
        email = EMAIL_ADDRESS // optional
        phoneNumber = PHONE_NUMBER // optional
    }
    // OR
    account.info.userInfo.apply{
        firstName = "..."
        email = "..." 
        ...  
    }
    ```

- **SenderId <sub>[optional]</sub>** - This is the Chat session id, for restoring an **_active_** chat session. 
This Id should be set to retrieve messages that were sent on the chat session to the user by the live agent, while the user was not connected to the chat. 
If no iD is provided, a new Id will be created and a new chat window will be opened on the agent console. 

- **ShouldStartNewChat <sub>[optional]</sub>** - Defines whethear there is a previous opened chat window, on agent workspace that should be used for this session.
Chat restore will fail if the chat session was already been closed by agent. Then a new chat session will be created, with newly generated SenderId. Missed messages of the closed session will be fetched and displayed.

- **LastReceivedMessageId <sub>[optional]</sub>** - Defines the last message that was viewed by the user, in order to retrieve all missed messages from this point onward. If this Id is not provided, messages will be retrieved from the session start.

### Account updates
- Once the chat session is created, the account is updated  [`AccountInfoProvider`]({{'/docs/chat-configuration/setting-account/account-info-provider' | relative_url}}) implementation.  
From the updated account the `SenderId` can be retrieved in order to sync to the last created session id.
`account.info.SenderId` 

- To get updates for `LastReceivedMessageId`, implement an [`AccountSessionListener`]({{'/docs/chat-configuration/setting-account/account-info-provider#ongoing-session-configurations-changes' | relative_url}}) instead.    

---
## How to

### Start a messaging chat
{: .d-inline-block .mt-2}   

The SDK enables to start a chat, continue a messaging chat, or escalate to a chat with a live agent from a chat with a chatbot.

- Create a [`ChatController`]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}}) with an AsyncAccount.

    ```kotlin
    val account = AsyncAccount(API_KEY, APPLICATION_ID)
    val chatController = ChatController.Builder(context)                                                     
                                        .build(account, ...)
    ```
    
- Use an existing `ChatController` and call [`chatController.startChat`]({{'/docs/chat-configuration/extra/chatcontroller#start-chats' | relative_url}})/`chatController.restoreChat` with an AsyncAccount.
    ```kotlin
    val account = AsyncAccount(API_KEY, APPLICATION_ID)
    chatController.startChat(account)
    // or
    chatController.restoreChat(account = account)
   
    ```



### Escalate to a chat woth a live agent from a chat with a chatbot
Chat escalation occurs when the user selects a [Chat typed channel](https://support.nanorep.com/API-Integrations/Chat-Integration/1009694282/How-to-integrate-LiveChat-Inc-chat.htm) configured on the Bold360ai console, from a bot response options.   
The channel must contain the accounts apiKey and the applicationId as follows: 
> `async:APPLICATION_ID:API_KEY`

In case the hosting app implements the AccountInfoProvider, the [`AccountInfoProvider.provide`]({{'/docs/chat-configuration/extra/account-info-provider#account-provide' | relative_url}}) method will be called with a basic AsyncAccount, according to the data provided from the channel. The `provide` method call, enables the hosting app to [configure](#configure-session) the chat session properties.   
> If `AccountInfoProvider` implementation was not provided, a new chat conversation will be created with an atomated user details.

### Messaging chat continuity
The SDK enables to connect to an open or closed chat session and retrieve the messages that were sent in it while the user was not actively connected to that chat, and continue the chat.
This needs to be configured in the AsyncAccount instance.   

{: .mb-4}   
UserInfo   
{: .strong-sub-title}   
Set in the `UserInfo` the userId for which the conversation session will be started. 
>

```kotlin
// sets a userInfo to the async session
asyncAccount.info.UserInfo = UserInfo(UserId).apply{ ... }
```

ShouldStartNewChat   
{: .strong-sub-title .mt-6}   

```kotlin
asyncAccount.info.ShouldStartNewChat = true/false
```

SenderId   
{: .strong-sub-title .mt-6}   
Define the SenderId of a previous created session. 
```kotlin
asyncAccount.info.SenderId = Long_Sender_Value
``` 

- Enable retrieval of missed messages from a previously opened/closed chat. [`ShouldStartNewChat==true`]  
   
LastReceivedMessageId   
{: .strong-sub-title .mt-6}   
In order to get only the messages that the user did not yet received, configure [`LastReceivedMessageId`](#lastReceivedMessageId), occording to the last message id, provided on the [session updates](#how-to-get-lastReceivedMessageId-updates).
     
```kotlin
asyncAccount.info.LastReceivedMessageId = Last_Message_Id
``` 
### Start a chat session that will [relate to the same user](#relate)
    
#### Get messages that were sent while the user was not active in the chat.

1. [Configure the session UserInfo](#configure-user-info) with the specific user id. 
2. Configure the `SenderId` with the id of the session that the messages were sent on.
3. Configure `LastReceivedMessageId` with the last agent message that was viewed by the user, in order to get only the relevant messages.
    The users missed messages will be fetched from the previous session, if has any.

    ```kotlin
    account.info.apply{
        account.info.userInfo = UserInfo(USER_ID)
        SenderId = PREVIOUS_SESSION_ID
        LastReceivedMessageId = LAST_AGENT_MESSAGE_ID
    }
    ```

#### Continue chat on an open/new chat window
1. Configure `UserInfo` and `SenderId` and `LastReceivedMessageId` as mentioned before.
2. Configure `ShouldStartNewChat` to `false` to use an open chat window otherwise set to `true`     

    If the previous session chat was closed by the user or the agent, a new chat window will be created with a new SenderId, no matter what was configured on `ShouldStartNewChat`. The users missed messages will be fetched from the previous session.

    ```kotlin
    account.info.apply{
        account.info.userInfo = UserInfo(USER_ID)
        SenderId = PREVIOUS_SESSION_ID
        LastReceivedMessageId = LAST_AGENT_MESSAGE_ID
        ShouldStartNewChat = false/true
    }
    ```