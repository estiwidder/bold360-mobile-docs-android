---
layout: default
title: Instant Feedback
parent: Feedback
grand_parent: Advanced Topics
nav_order: 1
# permalink: /docs/advanced-topics/feedback/instant-feedback
---

# Instant Feedback<sub>(Feedback Per Article)</sub>  {{site.data.vars.need-work}}
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## Overview  
Feedback is a tool for reviewing user experience and also specific content quality and relevance.   
When enabled, every chatbot response will be followed by a feedback UI element, as long as it valid for feedback submission.
{: .overview }

---

## Instant feedback Configurations
To enable users to provide feedback, this capability needs to be enabled both in  bold360ai console for the knowledgbase, and also in the SDK configuration.

### Admin console configurations
{: .mb-4 }
- **Feedback configuration**   
    {: .fs-4 .fw-300 }
  Refer to the feedback settings for widgets in the Bold360ai console.

  <details close markdown="block">
  <summary>Feedback status settings</summary>
  ![]({{'/assets/images/instant-feedback-console.png' | relative_url}})
  {: image-70 }
  </details> {: .mb-6 }

- **Feedback display type** 
    {: .fs-4 .fw-300 }
    There are two predefined feedback display options, that can be selected on the admin console: textual and iconic.

    <details close markdown="block">
    <summary>Feedback display type settings</summary>
    ![]({{'/assets/images/feedback-display-type.png' | relative_url}})
    {: image-70 }
    </details> {: .mb-4 }

    Bold SDK provides default implementations for both types.


    Iconic feedback:
    {: .blue-sm-tl}

    |![]({{'/assets/images/iconic-idle-feedback.png' | relative_url}})||
    |---|---|
    |![]({{'/assets/images/iconic-negative-feedback.png' | relative_url}})|![]({{'/assets/images/iconic-positive-feedback.png' | relative_url}})|
    {: .table-trans}

    
    Textual feedback:
    {: .blue-sm-tl}

    |![]({{'/assets/images/textual-idle-feedback.png' | relative_url}})||
    |---|---|
    |![]({{'/assets/images/textual-negative-readmore-feedback.png' | relative_url}})|![]({{'/assets/images/textual-positive-feedback.png' | relative_url}})|
    {: .table-trans}

- Textual configurations
  On the Bold360ai console enbles to customize the messages to  be dislayed the user. 

  ![]({{'/assets/images/feedback-texts.png' | relative_url}})
  {: image-70}
 
---

### SDK configurations
The instant feedback display can be overriden by the app, via `ChatUIProvider`.
1. Create custom feedback view by implementing the  `FeedbackUIAdapter` interface.
    ```kotlin
    class CustomIconFeedbackView : LineaLayout, FeedbackUIAdapter {
        ...
    }

    class CustomTextFeedbackView : LineaLayout, FeedbackUIAdapter {
        ...
    }
    ```
2. Implement the `FeedbackFactory`, which createsthe custom feedback view.
    ```kotlin
    class MyCustomFeedbackFactory : FeedbackFactory{
        override fun create(context: Context, feedbackDisplayType: Int): FeedbackUIAdapter {
            return when (feedbackDisplayType) {
                IconicFeedback -> CustomIconFeedbackView(context)
                else -> CustomTextFeedbackView(context)
            }
        }
    }
    ```
3. Set the feedback provider factory to point to your implementation, in the `ChatUIProvider`. 
    ```kotlin
    val chatUIProvider = ChatUIProvider().apply{
        chatElementsUIProvider.incomingUIProvider.feedbackUIProvider.overrideFactory = MyCustomFeedbackFactory
    }

    val chatController = ChatController.Builder(context).apply {
                            chatUIProvider(chatUIProvider)
                            ....
                        }.build(account...)

    ```