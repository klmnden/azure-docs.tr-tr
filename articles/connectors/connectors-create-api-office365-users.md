---
title: Office 365 kullanıcıları - Azure mantıksal uygulamaları bağlanma | Microsoft Docs
description: Office 365 kullanıcıları REST API'leri ve Azure Logic Apps ile kullanıcı profillerini yönetme
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 08/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 92eaa2f323da9a96b0be4379d2dbcea8d90733e4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34609437"
---
# <a name="get-started-with-the-office-365-users-connector"></a>Office 365 kullanıcıları Bağlayıcısı ile çalışmaya başlama
Profilleri, arama kullanıcıları ve daha fazla bilgi almak için Office 365 kullanıcıları için bağlayın. Office 365 kullanıcıları ile şunları yapabilirsiniz:

* Office 365 kullanıcılardan alma verileri temel alan, iş akışınızı oluşturun. 
* Doğrudan raporları Al kullanım Eylemler, bir yöneticinin kullanıcı profili ve daha fazla bilgi alın. Bu eylemler bir yanıt ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, bir kişinin doğrudan rapor alın ve sonra bu bilgileri almak ve bir SQL Azure veritabanı güncelleştirme. 

Bir mantıksal uygulama'yi şimdi oluşturmaya başlamak, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office-365-users"></a>Office 365 kullanıcıları bağlantı oluşturun.
Bu bağlayıcı mantıksal uygulamalarınızı eklediğinizde, Office 365 kullanıcıları hesabınızda oturum açın ve gerekir mantıksal uygulamaları hesabınıza bağlanmasına olanak sağlar.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Bağlantı oluşturduktan sonra kullanıcı kimliği gibi Office 365 kullanıcıları özelliklerini girin **REST API Başvurusu** bu makalede bu özellikleri açıklar.

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/officeusers/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).