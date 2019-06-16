---
title: Hiçbir çalışma bağlayıcı grubu için bir uygulama proxy'si uygulaması bulunamadı | Microsoft Docs
description: Hiçbir çalışma Bağlayıcısı Azure AD uygulama ara sunucusu ile uygulamanız için bağlayıcı grubunda olduğunda, karşılaşabileceğiniz sorunları
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f829b8e8a4bc08b43d3c30a6333771ccd4e26e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65783621"
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Çalışma bağlayıcı grubu için bir uygulama proxy'si uygulaması bulunamadı

Bu makale, Azure Active Directory ile tümleşik bir uygulama proxy'si uygulaması için algılanan bir bağlayıcı olmadığında karşılaştığı yaygın sorunları çözmenize yardımcı olur.

## <a name="overview-of-steps"></a>Adımlara genel bakış
Hiçbir çalışma uygulamanız için bir bağlayıcı grubu bağlayıcısında varsa, sorunu çözmek için birkaç yolu vardır:

-   Bağlayıcılar grubunda varsa, şunları yapabilirsiniz:

    -   Şirket içi sunucuda sağ taraftaki yeni bir bağlayıcı indirmek ve bu gruba atayın

    -   Etkin bir bağlayıcı grubuna Taşı

-   Grup içinde etkin bağlayıcı yok varsa, şunları yapabilirsiniz:

    -   Bağlayıcınızı etkin olmasının nedeni tanımlama ve çözme

    -   Etkin bir bağlayıcı grubuna Taşı

Sorunu anlamasını için uygulamanıza "Uygulama proxy'si" menüsünü açın ve bağlayıcı grubu uyarısı ileti arayın. Grup içinde bağlayıcılar varsa, uyarı iletisi en az bir bağlayıcı grubu gerekiyor belirtir. Etkin bağlayıcı yok varsa, uyarı iletisi açıklar. Devre dışı bağlayıcılar yaygındır. 

   ![Azure portalında bağlayıcı Grup Seçimi](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Bu seçeneklerin her biri hakkında daha fazla bilgi için aşağıdaki ilgili bölümüne bakın. Yönergeler bağlayıcı yönetim sayfasından başlangıç varsayar. Hata iletisi yukarıdaki arıyorsanız, uyarı iletisi tıklayarak bu sayfasına gidebilirsiniz. Sayfasına gidip da edinebilirsiniz **Azure Active Directory**, öğesine tıklayarak **kurumsal uygulamalar**, ardından **uygulama proxy'si.**

   ![Azure portalında Grup Yönetimi Bağlayıcısı](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Yeni bir bağlayıcı indirmek

Yeni bir bağlayıcı indirmek için sayfanın en üstündeki "Bağlayıcısı'nı indir" düğmesini kullanın.

Bağlayıcıyı doğrudan görebilmesi için arka uç uygulaması ile bir makineye yükleyin. Genellikle, bağlayıcı uygulama ile aynı sunucuya yüklenir. İndirdikten sonra bağlayıcıyı bu menüde görünmeli. Bağlayıcı tıklayın ve "Bağlayıcı grubu" açılan doğru grubuna ait olduğundan emin olun. Yaptığınız değişikliği kaydedin.

   ![Bağlayıcı Azure portalından indirin](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Etkin bir Bağlayıcıyı taşıma

Görebilmesi için hedef arka uç uygulama grubuna ait olmalıdır ve bir etkin bağlayıcı varsa, bağlayıcı atanan grubuna taşıyabilirsiniz. Bunu yapmak için bağlayıcı'yı tıklayın. "Bağlayıcı grubu" alanında doğru grubunu seçin ve Kaydet'e tıklayın için açılan listeyi kullanın.

## <a name="resolve-an-inactive-connector"></a>Etkin olmayan bir bağlayıcı çözümleyin

Gruptaki yalnızca bağlayıcıları devre dışı ise engeli kaldırılmış tüm gerekli bağlantı noktalarına sahip olmayan bir makineye olasıdır.

Bu sorunu araştırma hakkında ayrıntılar için bağlantı noktalarını sorun giderme belgesi bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)


