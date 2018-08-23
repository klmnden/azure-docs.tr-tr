---
title: Azure Cosmos DB'de erişim denetimi ayarlama | Microsoft Docs
description: Azure Cosmos DB'de erişim denetimini ayarlama konusunda bilgi edinin.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2018
ms.author: rafats
ms.openlocfilehash: 0d4595bcf8a9f009dce928d3f3cbc442328ec39b
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42059740"
---
# <a name="access-control-in-azure-cosmos-db"></a>Azure Cosmos DB'de erişim denetimi

Azure Cosmos DB hesabı okuyucusu erişimi kullanıcı hesabınıza eklemek için Azure portalında aşağıdaki adımları uygulayın, bir abonelik sahibi var.

1. Azure portalını açın ve Azure Cosmos DB hesabınızı seçin.
2. Tıklayın **erişim denetimi (IAM)** sekmesine ve ardından **+ Ekle**.
3. İçinde **izinleri eklemek** bölmesinde, **rol** kutusunda **Cosmos DB hesabı okuyucusu rolü**.
4. İçinde **kutusuna erişim atama**seçin **Azure AD kullanıcı, Grup veya uygulama**.
5. Dizininizde, erişim vermek istediğiniz kullanıcı, Grup veya uygulama seçin.  Görünen ad, e-posta adresi veya nesne tanımlayıcıları dizin arama yapabilirsiniz.
    Seçilen kullanıcıya, gruba veya uygulamaya seçili Üyeler listesinde görünür.
6. **Kaydet**’e tıklayın.

Varlık, artık Azure Cosmos DB kaynaklarını okuyabilir.
