---
title: Erişim denetimi Azure Cosmos DB'de ayarlama | Microsoft Docs
description: Erişim denetimini Azure Cosmos DB'de ayarlama öğrenin.
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: sngun
ms.openlocfilehash: 740f1ca560207ada95dd03fbecc7f9940ee7b2a0
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="access-control-in-azure-cosmos-db"></a>Azure Cosmos veritabanı erişim denetimi

Kullanıcı hesabınıza Azure Cosmos DB hesabı okuyucu erişimi eklemek için Azure portalında aşağıdaki adımları gerçekleştirin abonelik sahibi sahip.

1. Azure Portalı'nı açın ve Azure Cosmos DB hesabınızı seçin.
2. Tıklatın **erişim denetimi (IAM)** sekmesini ve ardından **+ Ekle**.
3. İçinde **izinleri eklemek** bölmesi, **rol** kutusunda **Cosmos DB hesap okuyucu rolüne**.
4. İçinde **kutusuna erişim atamak**seçin **Azure AD kullanıcı, Grup veya uygulama**.
5. Dizininizde, erişim vermek istediğiniz kullanıcı, Grup veya uygulamayı seçin.  Görünen ad, e-posta adresi veya nesne tanımlayıcıları dizin arama yapabilirsiniz.
    Seçilen kullanıcı, Grup veya uygulama seçili Üyeler listesinde görünür.
6. **Kaydet**’e tıklayın.

Varlık artık Azure Cosmos DB kaynakları okuyabilir.
