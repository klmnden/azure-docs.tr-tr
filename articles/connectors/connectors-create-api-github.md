---
title: GitHub - Azure mantıksal uygulamaları bağlanma | Microsoft Docs
description: GitHub REST API'leri ve Azure Logic Apps ile GitHub olaylarını izleme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: ce567dc631c3a147b795eb2355a4961faa8881d6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295821"
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