---
title: "Logic Apps içinde Facebook bağlayıcısını ekleyin | Microsoft Docs"
description: "REST API parametrelerle Facebook bağlayıcısını genel bakış"
services: 
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: f4d6f0ed-c09b-488c-be1c-8cf2b5b1d4b8
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: estfan; ladocs
ms.openlocfilehash: b0a1e2f04b6a4c7992db582f1238be4bcc3c6174
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
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