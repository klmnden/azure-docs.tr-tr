---
title: Office 365 kullanıcıları - Azure Logic Apps'i bağlama | Microsoft Docs
description: Office 365 kullanıcılar REST API'lerini ve Azure Logic Apps ile kullanıcı profillerini yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 08/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 3865fbc4fbc39da0860218565b0a8956b2dad8ee
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105880"
---
# <a name="get-started-with-the-office-365-users-connector"></a>Office 365 kullanıcıları Bağlayıcısı ile çalışmaya başlama
Profilleri, kullanıcı arama ve daha fazla bilgi almak için Office 365 kullanıcıları için bağlanın. Office 365 kullanıcıları ile şunları yapabilirsiniz:

* Office 365 kullanıcılarından almak verileri temel alan, iş akışınızı oluşturun. 
* Bağlı çalışanları Al kullanım Eylemler, bir yöneticinin kullanıcı profili ve daha fazlasını alın. Bu Eylemler, yanıt alın ve çıkış diğer eylemler için kullanılabilir yapın. Örneğin, bir kişinin bağlı çalışanları Al ve ardından bu bilgileri almak ve bir SQL Azure veritabanını güncelleştirmek. 

Şimdi mantıksal uygulama oluşturmaya başlama, bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-office-365-users"></a>Office 365 kullanıcıları bağlantısı oluşturma
Bu bağlayıcı için logic apps eklediğinizde, Office 365 kullanıcıları hesabınızda oturum açın ve gerekir hesabınıza bağlanmak logic apps olanak tanır.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Bağlantıyı oluşturduktan sonra kullanıcı kimliği gibi Office 365 kullanıcıları özelliklerini girin **REST API Başvurusu** bu makalede bu özellikleri açıklar.

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/officeusers/).

## <a name="more-connectors"></a>Daha fazla bağlayıcı
Geri Git [API listesi](apis-list.md).