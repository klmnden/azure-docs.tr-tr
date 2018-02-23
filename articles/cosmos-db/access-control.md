---
title: "Erişim denetimi Azure Cosmos DB'de ayarlama | Microsoft Docs"
description: "Erişim denetimini Azure Cosmos DB'de ayarlama öğrenin."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: mimig
ms.openlocfilehash: 5ef4a12c8229d8801a5d9749514a69c2c1e19499
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
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
