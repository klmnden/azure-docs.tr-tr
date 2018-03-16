---
title: "Logic apps içinde Google sürücü bağlayıcısını ekleyin | Microsoft Docs"
description: "REST API parametreleri Google sürücü bağlayıcısıyla genel bakış"
services: 
suite: 
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: b2bcebc5-02d2-435b-b0da-ef53bc51c4b6
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 9cea2ea13b93e798912e4feea012f6bd64b90cac
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-google-drive-connector"></a>Google sürücü Bağlayıcısı ile çalışmaya başlama
Dosyaları, get satırları ve daha fazlasını oluşturmak için Google sürücüye bağlanın. Google sürücüyle şunları yapabilirsiniz: 

* İş akışınız aramanıza alma verileri temel alan oluşturun. 
* Eylemler görüntüleri arama, Haberler ve daha fazla bilgi aramak için kullanın. Bu eylemler bir yanıt ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, bir video arayın ve akış bir Twitter video göndermek için Twitter kullanın.

Bir mantıksal uygulama'yi şimdi oluşturmaya başlamak, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-the-connection-to-google-drive"></a>Google sürücü bağlantısı oluşturma
Bu bağlayıcı mantıksal uygulamalarınızı eklediğinizde, Google sürücüsüne bağlanacak şekilde mantıksal uygulamalar yetkilendirmeniz gerekir.

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]
> 
> 

Bağlantı oluşturduktan sonra klasör yolu veya dosya adı gibi Google sürücü özelliklerini girin. 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/googledrive/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).
