---
title: "Klasik portal Azure AD uygulaması Proxy bağlayıcılar | Microsoft Docs"
description: "Azure AD uygulama proxy'si bağlayıcılarını grupları oluşturmak ve yönetmek nasıl ele alınmaktadır."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: fc65c4053c45d9c16c62ee0fe65924133a4bb94a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Ayrı ağlarda ve konumları bağlayıcı gruplarını kullanarak uygulama yayımlama
> [!div class="op_single_selector"]
> * [Azure portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Klasik Azure Portalı](active-directory-application-proxy-connectors.md)
>
>

Bağlayıcı grupları dahil olmak üzere çeşitli senaryolar için kullanışlıdır:

* Birden çok birbirine bağlı veri merkezleri siteleriyle. Bu durumda, veri merkezi çapraz bağlantıları pahalı ve yavaş olduğundan veri merkezi içinde kadar trafiği mümkün olduğunca tutmak istediğiniz. Bağlayıcılar veri merkezi içinde bulunan uygulamalar sunmak için her veri merkezinde dağıtabilirsiniz. Bu yaklaşım, veri merkezi çapraz bağlantıları en aza indirir ve kullanıcılarınız için tamamen saydam bir deneyim sağlar.
* Ana Kurumsal ağın parçası olmayan yalıtılmış ağlarda yüklü uygulamaları yönetme. Ayrıca ağa uygulamalarını yalıtmak için yalıtılmış ağlarda ayrılmış bağlayıcılar yüklemek için bağlayıcı gruplarını kullanabilirsiniz.
* Bulut erişimi için Iaas üzerinde yüklü uygulamalar için bağlayıcı grupları güvenli erişim tüm uygulamalar için ortak bir hizmet sağlar. Bağlayıcı gruplar şirket ağınızda ek bağımlılık oluşturun veya uygulama deneyimi parçalara yok. Bağlayıcılar, her bulut veri merkezinde yüklü ve bu ağda bulunan uygulamalar sunar. Yüksek kullanılabilirlik elde etmek için birkaç bağlayıcı yükleyebilirsiniz.
* İçinde belirli bağlayıcılar orman dağıtılabilir ve belirli uygulamaları sunmaya ayarlamak Çoklu orman ortamları için destek.
* Bağlayıcı grupları için ana site ya da yük devretme algılamak için olağanüstü durum kurtarma sitelerdeki veya yedekleme olarak kullanılabilir.
* Bağlayıcı grupları, birden çok şirket tek bir kiracı hizmet vermek için de kullanılabilir.

## <a name="prerequisite-create-your-connectors"></a>Önkoşul: oluşturma, bağlayıcılar
Bağlayıcılar grubuna [birden çok bağlayıcı yükleme](active-directory-application-proxy-enable.md), ardından ad ve gruplandırın. Son olarak belirli uygulamalarla atamanız gerekir.

## <a name="step-1-create-connector-groups"></a>1. adım: bağlayıcı grupları oluşturma
İstediğiniz sayıda bağlayıcı grupları oluşturabilirsiniz. Bağlayıcı grubu oluşturma, Klasik Azure portalında gerçekleştirilir.

1. Dizininizi seçin ve tıklatın **yapılandırma**.  
    ![Uygulama proxy'si, yapılandırma ekran görüntüsü - bağlayıcı gruplarının Yönet'e tıklayın](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. Uygulama proxy'si altında tıklatın **bağlayıcı grupları yönet** ve grubun bir ad verip bir bağlayıcı grubu oluşturun.  
    ![Uygulama Ara sunucusu Bağlayıcısı ekran görüntüsü - ad yeni grup gruplar](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>2. adım: bağlayıcılar, gruplara atayın.
Bağlayıcı grupları oluşturulduktan sonra uygun gruba bağlayıcıları taşıyın.

1. Altında **uygulama proxy'si**, tıklatın **yönetmek Bağlayıcılar**.
2. Altında **grup**, her bağlayıcı için istediğiniz grubu seçin. Bu bağlayıcılar yeni Grup etkinleşmesi için en fazla 10 dakika sürebilir.  
    ![Uygulama proxy'si bağlayıcılar ekran görüntüsü - açılır menüsünden grubunu seçin](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>3. adım: uygulamaları bağlayıcı gruplarınıza atayın
Her uygulama, hizmet verdiği bağlayıcı gruba ayarlamak için son adım olacaktır.

1. Dizininizde, Azure Klasik Portalı'ndaki, önce gruba atamak istediğiniz uygulamayı seçin **yapılandırma**.
2. Altında **bağlayıcı grup**, kullanmak için uygulamayı istediğiniz grubu seçin. Bu değişikliği hemen uygulanır.  
    ![Uygulama proxy Bağlayıcısı Grup ekran görüntüsü - açılır menüsünden grubunu seçin](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Ayrıca bkz.
* [Uygulama Ara sunucusunu etkinleştirme](active-directory-application-proxy-enable.md)
* [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
* [Koşullu erişimi etkinleştirme](active-directory-application-proxy-conditional-access.md)
* [Uygulama Ara sunucusu ile ilgili sorunları giderme](active-directory-application-proxy-troubleshoot.md)

En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](http://blogs.technet.com/b/applicationproxyblog/) göz atın
