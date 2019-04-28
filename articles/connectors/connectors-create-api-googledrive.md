---
title: Google Drive'a - Azure Logic Apps'i bağlama | Microsoft Docs
description: Oluşturun ve dosyalarını Google Drive REST API'leri ve Azure Logic Apps ile yönetme
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
ms.openlocfilehash: 43bd5248f1bb80c71a85935c585deac6152be78b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105098"
---
# <a name="get-started-with-the-google-drive-connector"></a>Google Drive Bağlayıcısı ile çalışmaya başlama
Dosyaları, satırları Al ve daha fazlasını oluşturmak için Google Drive'a bağlanın. Google Drive ile şunları yapabilirsiniz: 

* Aramanızdan alma verileri temel alan, iş akışınızı oluşturun. 
* Eylemler ve haber arama görüntülerini aramak için kullanın. Bu Eylemler, yanıt alın ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, bir video aramak ve Twitter akışındaki video göndermek için Twitter'i kullanın.

Şimdi mantıksal uygulama oluşturmaya başlama, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-the-connection-to-google-drive"></a>Google Drive bağlantısı oluşturma
Bu bağlayıcı için logic apps eklediğinizde, mantıksal uygulamalar, Google Drive'ınıza bağlanmak için yetkilendirilmesi gerekir.

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]
> 
> 

Bağlantıyı oluşturduktan sonra Google Drive özellikleri gibi klasör yolu veya dosya adı girin. 

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/googledrive/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).
