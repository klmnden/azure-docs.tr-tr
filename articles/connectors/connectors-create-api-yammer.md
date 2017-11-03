---
title: "Azure Logic Apps içinde Yammer bağlayıcısını ekleyin | Microsoft Docs"
description: "REST API parametreleri Yammer bağlayıcısıyla genel bakış"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5ae0827-fbb3-45ec-8f45-ad1cc2e7eccc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c7a213343b4fb2b5a89a5052a459061b404a431c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-yammer-connector"></a>Yammer Bağlayıcısı ile çalışmaya başlama
Erişim görüşmeleri Kurumsal ağınızda Yammer'a bağlanır. Yammer ile şunları yapabilirsiniz:

* İş akışınız Yammer alma verileri temel alan oluşturun. 
* Aşağıdaki Grup ya da bir akışı yeni bir ileti olduğunda kullanmak için tetikler.
* Eylemler bir ileti gönderme, tüm iletileri ve daha fazla bilgi almak için kullanın. Bu eylemler bir yanıt ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, yeni bir ileti görüntülendiğinde, Office 365 kullanılarak bir e-posta gönderebilirsiniz.

Bir mantıksal uygulama artık oluşturarak başlama; bkz: [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-yammer"></a>Yammer bağlantı oluşturun.
Yammer Bağlayıcısı'nı kullanmak için önce oluşturduğunuz bir **bağlantı** ardından ayrıntılar için bu özellikleri sağlar: 

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Belirteç |Evet |Yammer kimlik bilgilerini sağlayın |

> [!INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]
> 

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/yammer/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).