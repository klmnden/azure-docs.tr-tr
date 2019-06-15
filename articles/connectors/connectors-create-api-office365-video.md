---
title: Office 365 Video - Azure Logic Apps'i bağlama | Microsoft Docs
description: Office 365 Video REST API'lerini ve Azure Logic Apps ile videoları Yönet
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: c10a2aa097b63fd3751be01bbfeb6097080bbb9c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105828"
---
# <a name="get-started-with-the-office365-video-connector"></a>Office365 Video Bağlayıcısı ile çalışmaya başlama
Office 365 video bir Office 365 hakkında bilgi edinin, videoları ve daha fazlasını listesini almak için Video bağlanın. Office 365 Video ile şunları yapabilirsiniz:

* Office 365 Video'dan alma verileri temel alan, iş akışınızı oluşturun. 
* Video portalının durumunu kontrol edin, tüm video ve kanal listesini alma eylemlerini kullanın. Bu Eylemler, yanıt alın ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, Office 365 video aramak için Bing arama Bağlayıcısı'nı kullanın ve ardından o videonun hakkında bilgi almak için Office 365 video Bağlayıcısı'nı kullanın. Video gereksinimlerinizi karşılıyorsa, bu video FaceBook'ta gönderebilir. 

Şimdi mantıksal uygulama oluşturmaya başlama, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office365-video-connector"></a>Office365 Video Bağlayıcısı bağlantı oluşturun
Bu bağlayıcı için logic apps eklediğinizde, Office 365 Video hesabınızda oturum açın ve gerekir hesabınıza bağlanmak logic apps olanak tanır.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]
> 
> 

Bağlantıyı oluşturduktan sonra Kiracı adı gibi Office 365 video özelliklerini girin veya kanal kimliği 


## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/office365videoconnector/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).