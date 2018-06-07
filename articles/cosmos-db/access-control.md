---
title: Erişim denetimi Azure Cosmos DB'de ayarlama | Microsoft Docs
description: Erişim denetimini Azure Cosmos DB'de ayarlama öğrenin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2018
ms.author: sngun
ms.openlocfilehash: 2dae2b6291aa7ce18cc6f612b25cd5bdcc1ba753
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34611152"
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
