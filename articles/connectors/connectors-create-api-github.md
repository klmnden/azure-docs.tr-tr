---
title: GitHub - Azure mantıksal uygulamaları bağlanma | Microsoft Docs
description: GitHub REST API'leri ve Azure Logic Apps ile GitHub olaylarını izleme
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: c6bd21626c1934f40520b0ddc2d78eeb97eee9a9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34609921"
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