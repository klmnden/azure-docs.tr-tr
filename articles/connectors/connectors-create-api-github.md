---
title: "GitHub bağlayıcı Azure Logic Apps içinde | Microsoft Docs"
description: "Logic apps ile Azure uygulama hizmeti oluşturun. GitHub hizmetini barındıran bir web tabanlı Git deposu ' dir. Dağıtılmış değişiklik denetimi ve kaynak kodu Yönetimi (SCM) işlevselliğini Git yanı sıra kendi özellik ekleme tümünün sunar."
services: logic-apps
documentationcenter: .net,nodejs,java
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
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c9120babaa5f6da4f33bd60ba27434e24cb2f45e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-github-connector"></a>GitHub Bağlayıcısı ile çalışmaya başlama
GitHub hizmetini barındıran bir web tabanlı Git deposu ' dir. Dağıtılmış değişiklik denetimi ve kaynak kodu Yönetimi (SCM) işlevselliğini Git yanı sıra kendi özellik ekleme tümünün sunar.

Bir mantıksal uygulama'yi şimdi oluşturmaya başlamak, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>GitHub bağlantı oluşturun.
Logic apps ile GitHub oluşturmak için önce oluşturmanız gerekir bir **bağlantı** ardından ayrıntılar için aşağıdaki özellikleri sağlar: 

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Belirteç |Evet |GitHub Kimlik Bilgilerini girin |

Bağlantı oluşturduktan sonra Eylemler yürütür ve bu makalede açıklanan Tetikleyicileri dinlemek için kullanabilirsiniz. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/github/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).