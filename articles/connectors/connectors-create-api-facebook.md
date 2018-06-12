---
title: Facebook - Azure mantıksal uygulamaları bağlanma | Microsoft Docs
description: Zaman Çizelgesi ve sayfa Facebook REST API'leri ve Azure Logic Apps ile yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 11/07/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 985f3cf70a07b3080f34181e64c5bb1419d530bd
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295015"
---
# <a name="get-started-with-the-facebook-connector"></a>Facebook Bağlayıcısı ile çalışmaya başlama
Facebook hesabına bağlanma ve gönderi, sayfa akışı ve daha fazla bilgi edinin. Facebook ile şunları yapabilirsiniz:

* Facebook alma verileri temel alan, iş akışınızı oluşturun. 
* Yeni bir posta alındığında bir tetikleyici kullanın.
* Zaman Çizelgesi'sonrası kullanım Eylemler ve sayfa akışı alın. Bu eylemler bir yanıt ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, kendi zaman çizelgesi üzerinde yeni bir posta olduğunda bu posta ele ve Twitter akışınıza gönderim. 

Bir mantıksal uygulama'yi şimdi oluşturmaya başlamak, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-facebook"></a>Facebook bağlantı oluşturun.
Bu bağlayıcı mantıksal uygulamalarınızı eklediğinizde, Facebook bağlanmak için logic apps yetkilendirmeniz gerekir.

1. Facebook hesabınızda oturum açın
2. Seçin **Authorize**ve logic apps, Facebook bağlanıp izin verin. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 


## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/facebook/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).