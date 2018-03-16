---
title: "GitHub Azure Logic Apps ile bağlanma | Microsoft Docs"
description: "Azure Logic Apps ile GitHub için iş akışlarını otomatikleştirmek"
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/02/2018
ms.author: estfan; ladocs
ms.openlocfilehash: 830c3e6b5462c5d2d917388c0d0924e444342f93
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="connect-to-github"></a>GitHub için Bağlan

GitHub tüm dağıtılmış düzeltme denetim ve kaynak kodu Yönetimi (SCM) Git işlevindeki ve diğer özellikleri sunan bir web tabanlı Git deposu barındırma hizmetidir.

GitHub Bağlayıcısı ile çalışmaya başlamak için [mantıksal uygulama ilk oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>GitHub bağlantı oluşturun.

Bir mantıksal uygulama GitHub Bağlayıcısı'nı kullanmak için önce oluşturmanız gerekir bir *bağlantı* ve ardından ayrıntılar için bu özellikleri sağlar: 

| Özellik | Gerekli | Açıklama | 
| -------- | -------- | ----------- | 
| Belirteç | Evet | GitHub kimlik bilgilerinizi sağlayın. |

Bağlantı oluşturduktan sonra Eylemler yürütür ve bu makalede açıklanan Tetikleyicileri dinler.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tetikleyiciler ve Eylemler Swagger ve herhangi bir sınır içinde tanımlanan için gözden [Bağlayıcısı ayrıntıları](/connectors/github/).

## <a name="find-more-connectors"></a>Daha fazla bağlayıcıları Bul

* Gözden geçirme [bağlayıcılar listesi](apis-list.md).