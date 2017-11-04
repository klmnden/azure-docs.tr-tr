---
title: "Bir Azure yayımlama yönetilen portalı üzerinden uygulama | Microsoft Docs"
description: "Azure kullanmayı gösterir Azure oluşturmak için portal yönetilen kuruluşunuzun üyeleri için hedeflenen uygulama."
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 3d3c6c95763bc8eb67f092bda61bb58c7af9daf2
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Azure Portalı aracılığıyla hizmet Kataloğu uygulama yayımlama

Yayımlamak için Azure portalını kullanabilirsiniz [yönetilen uygulamaları](overview.md) kuruluşunuzun üyeleri için yöneliktir. Örneğin, bir BT departmanının kuruluş standartlarıyla uyumluluğu sağlamak yönetilen uygulamaları yayımlayabilirsiniz. Bu yönetilen uygulamaların Azure Market hizmet Kataloğu kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

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

   ![yönetilen uygulama oluşturma](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [yönetilen uygulama genel bakış](overview.md).
* Yönetilen uygulama örnekler için bkz: [Azure örnek projelerine yönetilen uygulamaları](sample-projects.md).
* Azure Marketi'nde yayımlama yönetilen uygulamalar hakkında daha fazla bilgi için bkz: [Azure Market uygulamalarda yönetilen](publish-marketplace-app.md).
* Yönetilen bir uygulama için bir kullanıcı Arabirimi tanımı dosyası oluşturmayı öğrenmek için bkz: [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).