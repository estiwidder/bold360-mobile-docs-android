---
layout: default
title: Chat Entities and Personal information
parent: Advanced Topics
nav_order: 9
# permalink: /docs/advanced-topics/entities-and-personal-info
---

# Chat Entities and Personal information {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}

---

## Overview
Entities enable to turn data into smart chatbot conversations. Entities essentially serve as a database for the information that the bot needs to answer your customers' questions. The SDK provides the bridge to pass this data to the BOT on chat start and while processing responses to user queries.

## COMMENT: THIS IS THE REFERENCE IN THE BOLD360AI SUPPORT SITE: https://support.bold360.com/bold360/help/working-with-entities

{: .overview}

Personal information can be categorized either as entities initialization or as data that is required to provide personalized answers to the user.

<a id="initentities"/>

## Initialization entities
## COMMENT: ARE THESE DATA ENTITIES?

Entity values can be provided for the entire chat session and are not changed dynamically (exp: ids, keys, etc).

## COMMENT: WHAT DOES "NOT CHANGED DRAMATICALLY" MEANS?

### How to initialize chat entities

1. Create the initialization entities map

   ```kotlin
   val entities = mapOf("EntityKey1" to "EntityValue1", "EntityKey2" to "EntityValue2", ... )
   ```

2. Set the initialization entities on the account

   ```kotlin
   val account = BotAccount(...).apply {
                             initializationEntities = entities
                        }
   ```

- Note: The `initialization entities` can be also supplied by the [`AccountInfoProvider.provide`]({{'/docs/chat-configuration/extra/account-info-provider#account-provide' | relative_url}}) method as follows:

    ```kotlin
    override fun provide(account: AccountInfo, callback: Completion<AccountInfo>) {
        ...
        (account as? BotAccount)?.initializationEntities = hashMapOf(Pair("USERID", "12345"))
        ...
    }
    ```

## Missing information for personalize responses
## COMMENT: ARE THESE CUSTOM ENTITIES? THERE IS ALSO AN ISSUE WITH THE TEXT HERE - FOR SOME REASON IT IS SET AS A HYPERLINK.
These entities are use to dynamically retrieve details from the user details required to personalize the response to the user query.
These articles contain entities tags within the response which are recognized by the SDK as the missin information needed to properly display the article as intended (refer to  [Bold360AI provider](https://support.bold360.com/bold360/help/how-do-i-create-a-csv-provider) for more information).

- **Missing entites**:    
  There may be cases in which the chatbot needs additional data from the user in order to providethe appropriate answer.   
  The entities defined in the article identify the missisng details required to provide the user with the response. According to the entities set in the account, the app is asked to provide thoes details by calling the `EntitiesProvider.provide`.   
  ##### Missing entities can be, names, account numbers, etc.

- **Personal information**:   
  This refers to additional personal information related to the user that will be requested in the app by the SDK for  specific entities, to provide a response.
  This occurs when the response (the artocle) contains placeholders for information. The app provides this personalinformation that is then inserted to the appropriate placeholders.
  ##### Personal information can be, account balance, passport number, etc.

--- 

  ![provide missing entites / personal info]({{'/assets/diagrams/personalInfo.png' | relative_url}})
  {: .image-70}

---

### How to provide personal information to entities

1. Create the missing entities array

    ```kotlin
    val missingEntities = arrayOf("EntityKey1", "EntityKey2"... )
    ```

2. Set the entities on the account

     ```kotlin
    val account = BotAccount(...).apply {
                              entities = missingEntities
                         }
    ```

3. Implement the `EntitiesProvider` interface.
   At the `EntitiesProvider` there are two `provide` methods for `personal information` and for `missing entities`.

4. Pass the implementation of the `EntitiesProvider` to the `ChatController` upon the ChatController creation

    ```kotlin
    ChatController.Builder(getContext()).entitiesProvider(the EntitiesProvider implemintation)...build(...)
    ```
---

## COMMENTS: WHAT IS VIEW SAMPLE? DO WE HAVE THIS FOR ALL IMPLEMENTATIONS?
 > view Sample: [`missing entities` and `personal information` usage]({{'/docs/faq/missing-entities-example.md' | relative_url}})
{: .mt-6}