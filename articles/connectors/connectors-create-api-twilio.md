---
title: Twilio Bağlayıcısı, Azure Logic apps'te ekleyin | Microsoft Docs
description: REST API parametrelerle Twilio bağlayıcısına genel bakış
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 8bcf69a7c8e04cb45d795fd0d6f20d477c15865d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38652014"
---
# <a name="get-started-with-the-twilio-connector"></a>Twilio Bağlayıcısı ile çalışmaya başlama
Genel SMS, MMS ve IP iletileri gönderip Twilio'ya bağlanın. Twilio ile şunları yapabilirsiniz:

* Twilio'dan alma verileri temel alan, iş akışınızı oluşturun. 
* Bir ileti, iletileri Listele ve daha fazlasını edinin eylemlerini kullanın. Bu Eylemler, yanıt alın ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, bir yeni Twilio iletisini aldığınızda, bu iletiyi almak ve bir Service Bus iş akışını kullanın. 

Mantıksal uygulama oluşturarak başlayın; bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-twilio"></a>Twilio bağlantı oluşturun
Bu bağlayıcı için logic apps eklediğinizde, Twilio aşağıdaki değerleri girin:

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Hesap Kimliği |Evet |Twilio hesap Kimliğinizi girin |
| Erişim Belirteci |Evet |Twilio erişim belirtecinizi girin |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Twilio erişim belirteci yoksa bkz [kullanıcı kimlik ve erişim belirteçleri](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/twilio/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).