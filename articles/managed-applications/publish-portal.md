---
title: Azure Yayımlama Portalı aracılığıyla uygulama yönetilen | Microsoft Docs
description: Azure'u nasıl kullanacağınızı gösteren Azure oluşturmak için portalı yönetilen uygulama kuruluşunuzun üyeleri için tasarlanmıştır.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: f27d30d4709fbf373c8458629d0c8c5af4333acf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61297092"
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Azure Portalı aracılığıyla bir hizmet Kataloğu uygulaması yayımlama

Yayımlamak için Azure portalını kullanabilirsiniz [yönetilen uygulamaları](overview.md) kuruluşunuzun üyeleri için yöneliktir. Örneğin, BT departmanı kurumsal standartlara uyumu sağlamak için yönetilen uygulamalar yayımlayabilir. Bu yönetilen uygulamalara Azure marketten değil, hizmet kataloğu üzerinden erişilebilir.

## <a name="prerequisites"></a>Önkoşullar

Yönetilen bir uygulama yayımladığınızda, kaynakları yönetmek için bir kimlik belirtin. Bir Azure Active Directory kullanıcı grubu belirtin öneririz. Bir Azure Active Directory kullanıcı grubu oluşturmak için bkz [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). 

Yönetilen uygulama tanımını içeren .zip dosyasını bir URI kullanılabilir olması gerekir. Depolama blobu, .zip dosyasını karşıya yüklemenizi öneririz. 

## <a name="create-managed-application-with-portal"></a>Portal ile yönetilen uygulama oluşturma

1. Sol üst köşedeki seçin **+ yeni**.

   ![Yeni hizmet](./media/publish-portal/new.png)

1. Arama **hizmet Kataloğu**.

1. Sonuçlarda bulana kadar kaydırın **hizmet Kataloğu yönetilen uygulaması tanımı**. Bunu seçin.

   ![Yönetilen uygulama tanımlarını arayın](./media/publish-portal/select-managed-apps-definition.png)

1. Seçin **Oluştur** yönetilen uygulama tanımı oluşturma işlemini başlatmak için.

   ![Yönetilen uygulama tanımı oluşturma](./media/publish-portal/create-definition.png)

1. Adı, görünen ad, açıklama, konum, abonelik ve kaynak grubu için değerler sağlayın. Paket dosya URI'si, oluşturduğunuz ZIP dosyasının yolunu belirtin.

   ![Değerleri sağlayın](./media/publish-portal/fill-application-values.png)

1. Kimlik doğrulaması ve kilit düzeyi bölümüne aldığınızda seçin **ekleme yetkilendirme**.

   ![Yetkilendirme Ekle](./media/publish-portal/add-authorization.png)

1. Kaynakları yönetmek ve seçmek için bir Azure Active Directory grubu seçmeniz **Tamam**.

   ![Yetkilendirme grubu Ekle](./media/publish-portal/add-auth-group.png)

1. Tüm değerleri sağlandığında seçin **Oluştur**.

   ![Yönetilen uygulama oluşturma](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Yönetilen uygulama örnekleri için bkz. [örnek projeler için Azure yönetilen uygulamalar](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.