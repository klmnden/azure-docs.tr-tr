---
title: Hiç çalışma bağlayıcı grubu için uygulama proxy'si uygulama bulundu | Microsoft Docs
description: Hiç çalışma bağlayıcı Azure AD uygulama proxy'si ile uygulamanız için bir bağlayıcı grubunda olduğunda, karşılaşabileceğiniz sorunları
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: e9e00dc199abe833c6a282ae7da7c209566a700f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335603"
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Hiç çalışma bağlayıcı grubu için uygulama proxy'si uygulama bulunamadı

Bu makalede Azure Active Directory ile tümleşik bir uygulama proxy'si uygulama için algılanan bir bağlayıcı olmadığında karşılaştığı yaygın sorunları gidermeye yardımcı olur.

## <a name="overview-of-steps"></a>Adımlara genel bakış
Hiç çalışma uygulamanız için bir bağlayıcı grubundaki bağlayıcısı varsa, bu sorunu gidermek için birkaç yolu vardır:

-   Bağlayıcılar grubunda varsa, şunları yapabilirsiniz:

    -   Sağ şirket içi sunucuda yeni bir bağlayıcı indirin ve bu gruba atayın

    -   Etkin bir bağlayıcı grubuna taşıyın

-   Grubunda active bağlayıcılar varsa, şunları yapabilirsiniz:

    -   Bağlayıcınızı etkin değil nedenini belirleyin ve çözün

    -   Etkin bir bağlayıcı grubuna taşıyın

Sorunu çözmek için uygulamanızda "Uygulama proxy'si" menüsünü açın ve bağlayıcı Grup uyarı ileti arayın. Grubunda bağlayıcılar varsa, uyarı iletisi grubunun en az bir bağlayıcı gerekiyor belirtir. Etkin bağlayıcılar varsa, uyarı iletisi, açıklanmaktadır. Etkin olmayan bağlayıcılar yaygındır. 

   ![Azure portalında bağlayıcı Grup Seçimi](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Bu seçeneklerin her biri hakkında daha fazla bilgi için aşağıdaki ilgili bölümüne bakın. Yönergeleri bağlayıcı yönetim sayfasından başlayarak olduğunu varsayalım. Yukarıdaki hata iletisini arıyorsanız, uyarı iletisi tıklayarak bu sayfasına gidebilirsiniz. Ayrıca sayfasına giderek alabileceğiniz **Azure Active Directory**, üzerinde tıklatmak **kurumsal uygulamalar**, sonra **uygulama proxy'si.**

   ![Azure portalında bağlayıcı Grup Yönetimi](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Yeni bir bağlayıcı indirin

Yeni bir bağlayıcı indirmek için sayfanın en üstünde "Bağlayıcısı'nı indir" düğmesini kullanın.

Doğrudan görüş çizgisine arka uç uygulaması olan bir makinede Bağlayıcısı'nı yükleyin. Genellikle, bağlayıcı uygulama aynı sunucuya yüklenir. Bağlayıcı indirdikten sonra bu menüsünde görüntülenmelidir. Bağlayıcısı'nı tıklatın ve doğru grubuna ait olduğundan emin olmak için "Bağlayıcı grubu" açılan kullanın. Yaptığınız değişikliği kaydedin.

   ![Bağlayıcı Azure portalından indirme](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Etkin bir Bağlayıcıyı taşıma

Grubuna ait olmalıdır ve görüş hedef arka uç uygulaması için olan bir active bağlayıcısı varsa, bağlayıcı atanmış olan Grup taşıyabilirsiniz. Bunu yapmak için Bağlayıcısı'nı tıklatın. "Bağlayıcı grubu" alanında açılan doğru grubunu seçin ve Kaydet'i tıklatın için kullanın.

## <a name="resolve-an-inactive-connector"></a>Etkin olmayan bir bağlayıcı çözümleyin

Yalnızca bağlayıcılar grubundaki etkin olmayan, engellemesini tüm gerekli bağlantı noktaları sahip olmayan bir makineye olasıdır.

Bu sorunu araştırmaya hakkında ayrıntılar için bağlantı noktalarını sorun giderme belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si bağlayıcılar anlama](manage-apps/application-proxy-connectors.md)


