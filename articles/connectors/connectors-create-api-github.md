---
title: "GitHub Azure Logic Apps ile bağlanma | Microsoft Docs"
description: "Azure Logic Apps ile GitHub için iş akışlarını otomatikleştirmek"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
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
ms.author: mandia; ladocs
ms.openlocfilehash: cd7cd3babbfb7efc5917d3a7ec5b9d10112ba791
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
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