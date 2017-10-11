---
title: "Office 365 kullanıcıları bağlayıcı Logic Apps içinde ekleme | Microsoft Docs"
description: "Office 365 kullanıcıları bağlayıcı REST API parametrelerle genel bakış"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 330f733440932a769eb0fe6031cd0d947a820080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office-365-users-connector"></a>Office 365 kullanıcıları Bağlayıcısı ile çalışmaya başlama
Profilleri, arama kullanıcıları ve daha fazla bilgi almak için Office 365 kullanıcıları için bağlayın. Office 365 kullanıcıları ile şunları yapabilirsiniz:

* Office 365 kullanıcılardan alma verileri temel alan, iş akışınızı oluşturun. 
* Doğrudan raporları Al kullanım Eylemler, bir yöneticinin kullanıcı profili ve daha fazla bilgi alın. Bu eylemler bir yanıt ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, bir kişinin doğrudan rapor alın ve sonra bu bilgileri almak ve bir SQL Azure veritabanı güncelleştirme. 

Bir mantıksal uygulama'yi şimdi oluşturmaya başlamak, bkz: [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-office-365-users"></a>Office 365 kullanıcıları bağlantı oluşturun.
Bu bağlayıcı mantıksal uygulamalarınızı eklediğinizde, Office 365 kullanıcıları hesabınızda oturum açın ve gerekir mantıksal uygulamaları hesabınıza bağlanmasına olanak sağlar.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Bağlantı oluşturduktan sonra kullanıcı kimliği gibi Office 365 kullanıcıları özelliklerini girin **REST API Başvurusu** bu konuda bu özellikleri açıklar.

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/officeusers/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).