---
title: "Hiç çalışma bağlayıcı grubu için uygulama proxy'si uygulama bulundu | Microsoft Docs"
description: "Hiç çalışma bağlayıcı Azure AD uygulama proxy'si ile uygulamanız için bir bağlayıcı grubunda olduğunda, karşılaşabileceğiniz sorunları"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4945958deedc8a1d9989ff901192c03a5363b4dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Hiç çalışma bağlayıcı grubu için uygulama proxy'si uygulama bulunamadı

Bu makale Yardım için uygulama proxy'si uygulama algılanan bir bağlayıcı olmadığında karşılaştığı yaygın sorunları çözmek için Azure Active Directory ile tümleştirilmiştir.

## <a name="overview-of-steps"></a>Adımlara genel bakış
Hiç çalışma uygulamanız için bir bağlayıcı grubundaki bağlayıcısı varsa, bu sorunu gidermek için birkaç yolu vardır:

-   Bağlayıcılar grubunda varsa, şunları yapabilirsiniz:

    -   Sağ şirket içi sunucuda yeni bir bağlayıcı indirin ve bu gruba atayın

    -   Etkin bir bağlayıcı grubuna taşıyın

-   Grubunda active bağlayıcılar varsa, şunları yapabilirsiniz:

    -   Bağlayıcınızı etkin değil nedenini belirleyin ve çözün

    -   Etkin bir bağlayıcı grubuna taşıyın

Bu sorunu olduğu öğrenmek için uygulamanızda "Uygulama proxy'si" menüsünü açın ve bağlayıcı Grup uyarı ileti arayın. Bu grubun en az bir bağlayıcı (hiçbiri grubunda olması) gerekir veya (büyük olasılıkla etkin olmayan bağlayıcılar elinizde rağmen) etkin bağlayıcılar olduğunu belirtin.

   ![Azure Portalı'nda bağlayıcı Grup Seçimi](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Bu seçeneklerin her biri hakkında daha fazla bilgi için aşağıdaki ilgili bölümüne bakın. Bunların her biri, bağlayıcı yönetim sayfasından başlayarak olduğunu varsayar. Yukarıdaki hata iletisini arıyorsanız, uyarı iletisi tıklayarak bu sayfasına gidebilirsiniz. Aksi takdirde bu giderek bulunabilir **Azure Active Directory**, üzerinde tıklatmak **kurumsal uygulamalar**, ardından **uygulama proxy'si.**

   ![Azure Portalı'nda bağlayıcı Grup Yönetimi](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Yeni bir bağlayıcı indirin

Yeni bir bağlayıcı indirmek için sayfanın en üstünde "Bağlayıcısı'nı indir" düğmesini kullanın.

Not bağlayıcıyı doğrudan görüş çizgisine arka uç uygulaması olan bir makinede yüklü olması gerekir ve genellikle uygulama ile aynı sunucuda yerleştirilir. Bağlayıcı indirdikten sonra bu menüsünde görüntülenmelidir. Bağlayıcısı'nı tıklatın ve doğru grubuna ait olduğundan emin olmak için "Bağlayıcı grubu" açılan kullanın. Yaptığınız değişikliği kaydedin.

   ![Bağlayıcı Azure Portalı'ndan indirin](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Etkin bir Bağlayıcıyı taşıma

Grubuna ait olmalıdır ve görüş hedef arka uç uygulaması için olan bir active bağlayıcısı varsa, bağlayıcı atanmış olan Grup taşıyabilirsiniz. Bunu yapmak için Bağlayıcısı'nı tıklatın. "Bağlayıcı grubu" alanında açılan doğru grubunu seçin ve Kaydet'i tıklatın için kullanın.

## <a name="resolve-an-inactive-connector"></a>Etkin olmayan bir bağlayıcı çözümleyin

Yalnızca bağlayıcılar grubundaki etkin olmayan, engellemesini tüm gerekli bağlantı noktaları sahip olmayan bir makineye olasıdır.

Bu sorunu araştırmaya hakkında ayrıntılar için bağlantı noktalarını sorun giderme belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si bağlayıcılar anlama](application-proxy-understand-connectors.md)


