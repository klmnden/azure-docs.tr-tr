---
title: Logic apps Salesforce Bağlayıcısı'nı kullanmayı öğrenin | Microsoft Docs
description: Logic apps, Azure App service ile oluşturun. Salesforce Bağlayıcısı bir API'nin Salesforce nesneleriyle birlikte çalışmasını sağlar.
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 54fe5af8-7d2a-4da8-94e7-15d029e029bf
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/05/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 4278837bb5653b66223374aa728bdc81b279fff7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38237310"
---
# <a name="get-started-with-the-salesforce-connector"></a>Salesforce Bağlayıcısı ile çalışmaya başlama
Salesforce Bağlayıcısı bir API'nin Salesforce nesneleriyle birlikte çalışmasını sağlar.

Kullanılacak [bağlayıcıyı](apis-list.md), ilk mantıksal uygulamanızı oluşturmak gerekir. Tarafından oluşturabileceğinize dair [artık bir mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-salesforce-connector"></a>Salesforce Bağlayıcısı bağlanma
Mantıksal uygulamanızı herhangi bir hizmete erişebilmeniz için önce ilk oluşturmak için ihtiyacınız bir *bağlantı* hizmeti. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar.  

### <a name="create-a-connection-to-salesforce-connector"></a>Salesforce Bağlayıcısı bir bağlantı oluşturun
> [!INCLUDE [Steps to create a connection to Salesforce Connector](../../includes/connectors-create-api-salesforce.md)]
> 
> 

## <a name="use-a-salesforce-connector-trigger"></a>Salesforce Bağlayıcısı tetikleyicisini kullanma
Bir tetikleyici bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

> [!INCLUDE [Steps to create a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Koşul ekle
> [!INCLUDE [Steps to create a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]
> 
> 

## <a name="use-a-salesforce-connector-action"></a>Salesforce Bağlayıcısı eylemini kullanma
Bir eylem, bir mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

> [!INCLUDE [Steps to create a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]
> 
> 

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/salesforce/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

