---
title: Azure Key Vault ile yönetilen uygulamaları kullanma | Microsoft Docs
description: Yönetilen uygulamaları dağıtırken erişim gizli dizilerini Azure Key Vault'ta kullanma işlemini gösterir
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 07/09/2018
ms.author: tomfitz
ms.openlocfilehash: 232bea437b38335bdaa189e504d4e5fd9b080a05
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724060"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Azure yönetilen uygulamaları dağıtırken access Key Vault gizli

(Parola gibi) güvenli bir değerle, dağıtım sırasında parametre olarak geçirmeniz gerektiğinde, değerini almak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Yönetilen uygulamaları dağıtırken Key Vault'a erişmek için erişim izni vermesi gerekir **Gereci kaynak sağlayıcısı** hizmet sorumlusu. Bu makalede yönetilen uygulamalarla çalışmak için Key Vault yapılandırma açıklanır.

## <a name="enable-template-deployment"></a>Şablon dağıtımı etkinleştir

1. Portalda, anahtar kasanızı seçin.

1. **Erişim ilkeleri**'ni seçin.   

   ![Erişim ilkelerini seçin](./media/key-vault-access/select-access-policies.png)

1. Seçin **Gelişmiş erişim ilkelerini görüntülemek için tıklayın**.

   ![Gelişmiş erişim ilkelerini görüntülemek](./media/key-vault-access/advanced.png)

1. Seçin **şablon dağıtımı için Azure Resource Manager'a erişimi etkinleştir**. Ardından **Kaydet**’i seçin.

   ![Şablon dağıtımı etkinleştir](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Katkıda bulunan olarak hizmet Ekle

1. Seçin **erişim denetimi (IAM)**.

   ![Erişim denetimi seçin](./media/key-vault-access/access-control.png)

1. **Add (Ekle)** seçeneğini belirleyin.

   ![Ekle'yi seçin](./media/key-vault-access/add-access-control.png)

1. Seçin **katkıda bulunan** rolü için. Arama **Gereci kaynak sağlayıcısı** ve kullanılabilir seçenekler arasından seçin.

   ![Arama sağlayıcısı](./media/key-vault-access/search-provider.png)

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Key Vault'unuza yönetilen bir uygulama dağıtımı sırasında erişilebilir olacak şekilde yapılandırdınız.

* Şablon parametresi olarak bir anahtar Kasası'ndaki bir değer geçirme hakkında daha fazla bilgi için bkz: [dağıtım sırasında güvenli bir parametre geçirmek için Azure anahtar kasası kullanım](../azure-resource-manager/resource-manager-keyvault-parameter.md).
* Yönetilen uygulama örnekleri için bkz. [örnek projeler için Azure yönetilen uygulamalar](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.