---
title: "Mantıksal uygulamalarınızı Azure Service Bus Bağlayıcısı'nı kullanmayı öğrenin | Microsoft Docs"
description: "Logic apps ile Azure uygulama hizmeti oluşturun. İleti gönderme ve alma için Azure Service Bus hizmetine bağlanın. Kuyruk, konu başlığına göndermek, kuyruktan alma ve abonelikten almak için gönderme gibi eylemleri gerçekleştirebilirsiniz."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 89bf0ffec759fca4af5f99af1b6a2dd8d641ff6f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-azure-service-bus-connector"></a>Azure Service Bus Bağlayıcısı ile çalışmaya başlama
İleti gönderme ve alma için Azure Service Bus hizmetine bağlanın. Kuyruk, konu başlığına göndermek, kuyruktan alma ve abonelikten almak için gönderme gibi eylemleri gerçekleştirebilirsiniz.

Kullanılacak [tüm bağlayıcıların](apis-list.md), ilk mantıksal uygulama oluşturmanız gerekir. Tarafından başlayabiliriz [şimdi mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-service-bus"></a>Hizmet veri yoluna bağlayın
Mantıksal uygulamanızı herhangi bir hizmet erişmeden önce ilk hizmetine bir bağlantı oluşturmanız gerekir. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar.  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Hizmet veri yolu tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Bir hizmet veri yolu eylemi kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/servicebus/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

