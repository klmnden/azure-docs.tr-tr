---
title: Bu şirket uygulama hatası uygulama proxy'si uygulama kullanırken erişemiyor | Microsoft Docs
description: Azure AD uygulama proxy'si uygulamalarla ortak erişim sorunlarını gidermek nasıl.
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
ms.reviewer: harshja
ms.openlocfilehash: c2571a7ca9e92c5088aec600f1865f84736c5bfa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592519"
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>Uygulama proxy'si uygulama kullanılırken "Kurumsal bu uygulamaya erişemezsiniz" hatası

Bu makalede Azure AD uygulama proxy'si uygulamada "Bu Kurumsal uygulama erişilemiyor" hatanın sık karşılaşılan sorunları gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış
Bu hata gördüğünüzde, durum kodu hata sayfasında bulun. Bu kodu aşağıdaki durum kodları olabilir:

-   **Ağ geçidi zaman aşımı**: uygulama ara Sunucusu hizmeti, bağlayıcı ulaşmak. Bu hata genellikle bağlayıcı atama, bağlayıcının kendisi, bir sorun olduğunu gösteriyor veya ağ bağlayıcı kuralları.

-   **Hatalı ağ geçidi**: arka uç uygulamasına ulaşması bağlayıcı alamıyor. Bu hata, uygulamanın bir yanlış yapılandırma olduğunu gösteriyor olabilir.

-   **Yasak**: kullanıcının uygulamaya erişim yetkisi yok. Bu hata, kullanıcının Azure Active Directory uygulamasına atanmadığında veya arka uçta kullanıcının uygulamaya erişim izni yoksa gerçekleşebilir.

Kod bulmak için hata iletisi "Durum kodu" alanı için sol alt metni bakın. Tüm ek ipuçları sayfanın sonundaki ayrıca arayın.

   ![Ağ geçidi zaman aşımı hatası](./media/application-proxy/connection-problem.png)

Bu hatalar kök nedenini sorun giderme hakkında ayrıntılar ve önerilen düzeltmeler hakkında daha fazla bilgi için aşağıdaki ilgili bölümüne bakın.

## <a name="gateway-timeout-errors"></a>Ağ geçidi zaman aşımı hataları

Bir ağ geçidi zaman aşımı hizmet bağlayıcı erişmeye çalıştığında gerçekleşir ve zaman aşımı penceresinde kurulamıyor. Bu hata genellikle çalışma bağlayıcılar sahip bir bağlayıcı grubuna atanmış bir uygulama tarafından nedeni veya bağlayıcı tarafından gerekli bazı bağlantı noktaları açık değil.


## <a name="bad-gateway-errors"></a>Hatalı ağ geçidi hataları

Hatalı ağ geçidi hata bağlayıcı arka uç uygulaması ulaşamadı olduğunu gösterir. doğru uygulama yayımlandığından emin olun. Bu hataya neden yaygın hatalar şunlardır:

-   Bir yazım hatasından veya iç URL hata

-   Uygulama kök yayımlama değil. Örneğin, yayımlama <http://expenses/reimbursement> ancak erişim çalışılırken <http://expenses>

-   Kerberos Kısıtlı temsilci (KCD) yapılandırma sorunları

-   Arka uç uygulaması sorunları

## <a name="forbidden-errors"></a>Yasaklanmış hataları

Yasak hata görürseniz, kullanıcı uygulamaya atanmadı. Bu hata, Azure Active Directory'de veya arka uç uygulama olabilir.

Azure uygulamasında kullanıcılara atamak öğrenmek için bkz: [yapılandırma belgelerine](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Kullanıcı, Azure uygulamasında atanmış onaylarsanız, arka uç uygulamasındaki kullanıcı yapılandırmasını denetleyin. Kerberos Kısıtlı temsilci/tümleşik Windows kimlik doğrulaması kullanıyorsanız, yönergeler KCD sorun giderme sayfasına bakın.

## <a name="check-the-applications-internal-url"></a>Uygulamanın iç URL'yi denetleyin

İlk hızlı bir adım olarak, çift onay ve iç URL üzerinden uygulama açarak düzeltme **kurumsal uygulamalar**, ardından seçerek **uygulama proxy'si** menüsü. Şirket içi ağınızdan uygulamaya erişmek için kullanılan bir iç URL olduğunu doğrulayın.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Uygulama bağlayıcısı Grup çalışan atandığından emin olun

Uygulama doğrulamak için bir çalışma bağlayıcı Grup atanır:

1.  Giderek portalda uygulamayı açmak **Azure Active Directory**, üzerinde tıklatmak **kurumsal uygulamalar**, ardından **tüm uygulamaları.** Uygulamasını açın ve ardından **uygulama proxy'si** sol menüden.

2.  Bağlayıcı grubu alanına bakın. Grupta etkin bağlayıcılar yoksa, bir uyarı görürsünüz. Tüm uyarılar görmüyorsanız, üzerinde "Güvenilenler listesine tüm gerekli bağlantı noktalarının doğrula" taşıyın.

3.  Yanlış bağlayıcı Grup gösteriliyorsa, doğru grubunu seçin ve artık bu uyarılar gördüğünüz onaylamak için açılan listeyi kullanın. Hedeflenen bağlayıcı Grup gösteriyorsa, bağlayıcı yönetimiyle sayfasını açmak için uyarı iletisi tıklayın.

4.  Buradan, daha ayrıntılı incelemek üzere birkaç yolu vardır:

  * Etkin bir bağlayıcı grubuna taşıyın: Bu gruba ait olması gerekir ve görüş hedef arka uç uygulaması için olan bir active bağlayıcısı varsa, bağlayıcı atanmış olan Grup taşıyabilirsiniz. Bunu yapmak için Bağlayıcısı'nı tıklatın. "Bağlayıcı grubu" alanında açılan doğru grubunu seçin ve Kaydet'i tıklatın için kullanın.

  * Bu grup için yeni bir bağlayıcı indirin: Bu sayfadan bağlantısını alma [yeni bir bağlayıcı indirin](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). Doğrudan görüş çizgisine arka uç uygulaması olan bir makinede Bağlayıcısı'nı yükleyin. Typicall, bağlayıcı uygulama ile aynı sunucuda yüklüdür. Bir bağlayıcı hedef makine üzerine indirmek için indirme Bağlayıcısı bağlantıyı kullanın. Ardından, bağlayıcı tıklayın ve sağ grubuna ait olduğundan emin olmak için "Bağlayıcı grubu" açılan kullanın.

  * Etkin olmayan bir bağlayıcı araştırın: bağlayıcıyı devre dışı olarak gösteriyorsa, bu hizmete erişmek alamıyor. Bu hata genellikle engellenme bazı gerekli bağlantı noktaları nedeniyle kaynaklanır. Bu sorunu çözmek için üzerinde "Güvenilenler listesine tüm gerekli bağlantı noktaları olduğundan emin olun." gitme

Kullandıktan sonra uygulama bağlayıcıları, çalışma ile bir gruba atanan emin olmak için aşağıdaki adımları uygulamayı yeniden sınayın. Hala çalışmıyorsa sonraki bölüme devam edin.

## <a name="check-all-required-ports-are-whitelisted"></a>Tüm gerekli bağlantı noktalarının Güvenilenler listesine olduğunu denetleyin

Gerekli tüm bağlantı noktalarının açık olduğunu doğrulamak için bağlantı noktaları açma belgelerine bakın. Tüm gerekli bağlantı noktalarının açık olması durumunda sonraki bölüme taşıyın.

## <a name="check-for-other-connector-errors"></a>Diğer bağlayıcı hataları denetle

Yukarıdakilerin hiçbiri sorunu çözmezse, sonraki adıma sorunları veya hatalar Bağlayıcısı ile aramaya yöneliktir. Bazı yaygın hatalar görebilirsiniz [sorun giderme belge](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

Ayrıca, tüm hataları belirlemek için doğrudan Connector günlüklerine bakabilirsiniz. Hata iletileri çoğunu düzeltmeleri yönelik belirli öneriler paylaşır. Günlükleri görüntülemek için bkz: [bağlayıcılar belgelerine](manage-apps/application-proxy-connectors.md#under-the-hood).

## <a name="additional-resolutions"></a>Ek çözümler

Yukarıdaki sorunu çözmeye alamadık, birkaç farklı olası nedenleri vardır. Sorunu belirlemek için:

Uygulamanız, tümleşik Windows kimlik doğrulaması (IWA) kullanmak üzere yapılandırılmışsa, çoklu oturum açma olmadan uygulamayı test etme. Aksi halde, sonraki paragrafa taşıyın. Uygulamayı çoklu oturum açma olmadan denetlemek için uygulamanız aracılığıyla açın **kurumsal uygulamalar** ve Git **çoklu oturum açma** menüsü. Aşağı açılan "Tümleşik Windows kimlik doğrulamasını" "Azure AD çoklu oturum devre dışı açma için" değiştirin. 

Şimdi bir tarayıcı açın ve uygulamayı yeniden erişmeyi deneyin. Kimlik doğrulaması için istenir ve uygulamasına alın. Kimlik doğrulaması için, sorunu çoklu oturum açma sağlar Kerberos Kısıtlı temsilci (KCD) yapılandırmaya sahip olur. Daha fazla bilgi için KDC sorun giderme sayfasına bakın.

Hatayı görmeye devam ederseniz, bağlayıcının yüklendiği makineye bir tarayıcı açın ve uygulama için kullanılan iç URL ulaşmaya çalışır gidin. Bağlayıcı aynı makineden başka bir istemci gibi davranır. Uygulama bağlanamazsa, bu makine neden uygulamasına ulaşması ya da uygulamaya erişim olanağına sahip bir sunucuda bağlayıcı kullanmak araştırın.

Sorunları veya hatalar Bağlayıcısı ile aramak için o makine uygulamadan erişebiliyorsa. Bazı yaygın hatalar görebilirsiniz [sorun giderme belge](active-directory-application-proxy-troubleshoot.md#connector-errors). Ayrıca, tüm hataları belirlemek için doğrudan Connector günlüklerine bakabilirsiniz. Bizim hata iletilerinin çoğunu düzeltmeler için daha özel öneriler paylaşamaz. Günlükleri görüntülemek öğrenmek için bkz: [bağlayıcılar Belgelerimizi](manage-apps/application-proxy-connectors.md#under-the-hood).

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si bağlayıcılar anlama](manage-apps/application-proxy-connectors.md)
