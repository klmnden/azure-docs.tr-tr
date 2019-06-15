---
title: Facebook - Azure Logic Apps'i bağlama | Microsoft Docs
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
ms.openlocfilehash: 25595127d913d3cd093e0af3d7916e33fc7cb352
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105985"
---
# <a name="get-started-with-the-facebook-connector"></a>Facebook Bağlayıcısı ile çalışmaya başlama
Facebook'a bağlanın ve zaman tünelinde gönderi yapın, sayfa akışı ve daha fazlasını edinin. Facebook ile şunları yapabilirsiniz:

* Facebook'tan alma verileri temel alan, iş akışınızı oluşturun. 
* Yeni bir gönderi alındığında bir tetikleyici kullanın.
* Zaman tünelinde gönderi kullanım Eylemler ve sayfa akışı alın. Bu Eylemler, yanıt alın ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, zaman çizelgesinde yeni bir gönderi olduğunda, bu postayı alın ve Twitter akışınızı gönderin. 

Şimdi mantıksal uygulama oluşturmaya başlama, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-facebook"></a>Facebook bağlantı oluşturun
Bu bağlayıcı için logic apps eklediğinizde, mantıksal uygulamalar, Facebook'a bağlanın yetkilendirmeniz gerekir.

1. Facebook hesabınızda oturum açın
2. Seçin **Authorize**ve bağlanma ve sorgulama, Facebook, logic apps olanak tanır. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 


## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/facebook/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).