---
layout: default
title: Autocomplete
parent: Advanced Topics
nav_order: 7
permalink: /docs/advanced-topics/autocomplete/
has_children: true
has_toc: false
---

# Autocomplete
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc .mb-0}
- [Autocomplete In chat](./in-chat)
- [Autocomplete Standalone component](./standalone) 

---

## Overview
Autocomplete refers to displaying comptetion suggestions predited by the system, while the user is typing a message.  The user can select from the suggested text or continue typing his message.   
Selected suggestion will be sent as the user's message and the response will then be displayed.
{: .overview }


## AI autocomplete
_Autocomplete feature is available only for chats with chatbots._
{: .fs-4 .fw-300 }

The presented suggestions are retireived from the intent  phrases in the relevant knowledgebase.   

### Enabling Autocomplete
The autocomplete capability can be enabled in bold360ai console.
- <details close markdown="block">
  <summary>Account level configuration</summary>

    ![]({{'/assets/images/autocomplete-account-console.png' | relative_url}}) 
    {: .image-70}

  </details>
{: mt-10em}
- <details close markdown="block">
  <summary>Widget level configuration</summary> 
    ![]({{'/assets/images/autocomplete-widget-console.png' | relative_url}})
    {: .image-70}

  </details>
{: mt-10em}
- [App side configuration](./in-chat)

