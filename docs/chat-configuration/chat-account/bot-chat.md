---
layout: default
title: AI Chat
parent: Chat Account
grand_parent: Chat Configuration
nav_order: 1
---

# Bold AI chatbot <sup>chatbot</sup> {{site.data.vars.need-review}}
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview
The SDKs chat with AI bridges your APP with the conversational platform of Bold360 ai. This platform enables you to provide a messaging-based user experience similar to that of a conversation with a human agent. The platform will provide app users with the most suitable information based on the users input and contextual information it gathers.
{: .overview}

The chat type is defined by the account type configured in the SDK.

## BotAccount
Set the BotAccount for a Bold AI chat.
This ccount enables to create the chatbot and provide additional details and configurations required for the chat session.

### Creating account

```kotlin
val account = BotAccount(API_KEY, ACCOUNT_NAME,
                        KNOWLEDGE_BASE, SERVER)
```  

- API_KEY - Configured in your account and should be provided by an account administrator.
- ACCOUNT_NAME <sub>[mandatory]</sub> - Configured in your account and should be provided by an account administrator.
- KNOWLEDGE_BASE <sub>[mandatory]</sub> - The knowledge base that holds the information to be provided to the app users in this chat.
- SERVER <sub>[mandatory]</sub> - Configured in your account and should be provided by an account administrator.

### Define a unique User id
The UserId property is the unique identifier for a user to be used mainly for analytics, to accuratly track and analyze usage data. The UserId should be set by the application upon chat creation. In case the UserID is not provided by the application, the SDK creates a new id. In order to get the most out of the Bold analytics platform, a unique identified should be assign to a unique user.

```kotlin 
val account = BotAccount(...).apply{
    userId = SAVED_USER_ID
}
```

> Hosting application can get the newly created UserId on [`AccountInfoProvider.update`]({{'/docs/chat-configuration/extra/account-info-provider#account-update' | relative_url}}) and keep it for later use.
```kotlin 
override fun update(account: AccountInfo) {
    account.userId
}
```


### Configure Contexts
If your account supports context based answers, you may want to configure those on your account to be used during the chat.

    ```kotlin
    // Create contexts map 
    val contexts = mapOf("ContextKey1" to "ContextValue1",
                        "ContextKey2" to "ContextValue2", ... )

    // set on constructor:
    val account = BotAccount(API_KEY, ACCOUNT_NAME,
                            KNOWLEDGE_BASE, SERVER, contexts)


    // or later in time, before chat creation:                           
    account.contexts = contexts
    ```

### Configure [Initialization Entities]({{'/docs/advanced-topics/entities-and-personal-info#initentities' | relative_url}})
Custom Entities, are pices of information that should be provided by the application/user during the chat, instead of using a form, we use the conversation to gather those details.
Some entities are static and are not going to change during the chat, therefore can be provided on the chat creation. Those entities are provided on the account.

 ```kotlin
 account.initializationEntities = mapOf("EntityKey1" to "EntityValue1",
                                     "EntityKey2" to "EntityValue2", ... )
 ```
 ---

## AI Chat continuity
Chatbot continuity has no special meaning as in live chat. 
Previous conversationId can be provided, for the initiated chat session, and if valid will be used, otherwise a new chat session will be created.
The created  conversationId is available for Hosting app managment via [`AccountInfoProvider.update`]({{'/docs/chat-configuration/extra/account-info-provider#account-update' | relative_url}}).
```kotlin 
override fun update(account: AccountInfo) {
    account.info.id // conversationId
}
```

---


## How to
- ### Customize and override chat [Welcome Message]({{'/docs/chat-configuration/extra/welcome-message' | relative_url}})
  Bold360 console configured welcome message article, can be overridden, or provided if none configured, on the account. 

    ```kotlin
    botAccount.welcomeMessage = ARTICLE_ID 
    // if id is not valid, no message will be displayed

    botAccount.welcomeMessage = BotAccount.None 
    // prevent welcome message display                 
    ```

