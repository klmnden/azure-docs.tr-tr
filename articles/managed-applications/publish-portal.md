---
title: Bir Azure yayımlama yönetilen portalı üzerinden uygulama | Microsoft Docs
description: Azure kullanmayı gösterir Azure oluşturmak için portal yönetilen kuruluşunuzun üyeleri için hedeflenen uygulama.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 69d31a7199347574e8866b275ec17ba3997d80c2
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Azure Portalı aracılığıyla hizmet Kataloğu uygulama yayımlama

Yayımlamak için Azure portalını kullanabilirsiniz [yönetilen uygulamaları](overview.md) kuruluşunuzun üyeleri için yöneliktir. Örneğin, bir BT departmanının kuruluş standartlarıyla uyumluluğu sağlamak yönetilen uygulamaları yayımlayabilirsiniz. Bu yönetilen uygulamaların Azure Market hizmet Kataloğu kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Yönetilen bir uygulama yayımladığınızda, kaynakları yönetmek için bir kimlik belirtin. Bir Azure Active Directory kullanıcı grubu belirtin öneririz. Bir Azure Active Directory kullanıcı grubu oluşturmak için bkz: [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../active-directory/active-directory-groups-create-azure-portal.md). 

Yönetilen uygulama tanımını içeren .zip dosyası bir URI kullanılabilir olmalıdır. .Zip dosyanızın bir depolama blob karşıya öneririz. 

## <a name="create-managed-application-with-portal"></a>Portal ile yönetilen uygulama oluşturma

1. Sol üst köşedeki seçin **+ yeni**.

   ![Yeni hizmet](./media/publish-portal/new.png)

1. Arama **hizmet Kataloğu**.

1. Sonuçlarda bulana kadar kaydırın **hizmet Kataloğu yönetilen uygulama tanımını**. Bunu seçin.

   ![Yönetilen uygulama tanımları için arama](./media/publish-portal/select-managed-apps-definition.png)

1. Seçin **oluşturma** yönetilen uygulama tanımı oluşturma işlemini başlatmak için.

   ![Yönetilen uygulama tanımı oluşturun](./media/publish-portal/create-definition.png)

1. Ad, görünen ad, açıklama, konum, abonelik ve kaynak grubu için değerler sağlayın. Paket dosyası URI oluşturduğunuz ZIP dosyasının yolunu belirtin.

   ![Değerleri sağlayın](./media/publish-portal/fill-application-values.png)

1. Kimlik doğrulaması ve kilit düzeyi bölümüne aldığınızda, seçin **eklemek yetkilendirme**.

   ![Yetkilendirme ekleme](./media/publish-portal/add-authorization.png)

1. Kaynakları yönetme ve seçmek için bir Azure Active Directory grubu seçin **Tamam**.

   ![Yetkilendirme grubu Ekle](./media/publish-portal/add-auth-group.png)

1. Tüm değerleri sağlandığında seçin **oluşturma**.

   ![Yönetilen uygulama oluşturma](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Yönetilen uygulama örnekler için bkz: [Azure örnek projelerine yönetilen uygulamaları](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.