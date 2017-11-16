---
title: "Ayrı ağlar ve Azure AD uygulaması Proxy Bağlayıcısı gruplarını kullanarak konumlar uygulamaları yayımlama | Microsoft Docs"
description: "Azure AD uygulama proxy'si bağlayıcılarını grupları oluşturmak ve yönetmek nasıl ele alınmaktadır."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 477f20bd552460176be92f1db70bb0f76de8bac1
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Ayrı ağlarda ve konumları bağlayıcı gruplarını kullanarak uygulama yayımlama

Müşteriler daha da fazla senaryoları için Azure AD uygulama proxy'si ve uygulamaları kullanın. Bu nedenle uygulama Proxy daha esnek daha fazla topolojileri etkinleştirerek yaptık. Böylece, belirli uygulamaları sunmaya belirli bağlayıcılar atayabilirsiniz uygulama Proxy Bağlayıcısı grupları oluşturabilirsiniz. Bu yetenek, daha fazla denetim ve uygulama proxy'si dağıtımınızı en iyi duruma getirmek için yollar sağlar. 

Her uygulama ara sunucusu Bağlayıcısı bağlayıcı grubuna atanır. Aynı bağlayıcı grubuna ait tüm bağlayıcılar, yüksek kullanılabilirlik için ayrı bir birim olarak davranmasına ve Yük Dengeleme. Tüm bağlayıcılar, bağlayıcı grubuna ait. Grupları oluşturmazsanız, tüm bağlayıcılar bir varsayılan grubunda yer alan. Yöneticinizin yeni gruplar oluşturabilir ve bağlayıcılar Azure portalında atayabilirsiniz. 

Tüm uygulamaları bir bağlayıcı grubuna atanır. Grupları oluşturmazsanız, tüm uygulamalarınızın varsayılan grubuna atanır. Ancak, bağlayıcılar gruplar halinde düzenlemek, belirli bir bağlayıcının grubu ile çalışmak için her bir uygulama ayarlayabilirsiniz. Bu durumda, yalnızca o gruptaki bağlayıcılar uygulama istek üzerine hizmet. Bu özellik, uygulamalarınızı farklı konumlarda barındırılan yararlıdır. Uygulamalar her zaman fiziksel olarak bunları yakın olan bağlayıcılar tarafından sunulan konuma göre bağlayıcı grupları oluşturabilirsiniz.

>[!TIP] 
>Büyük bir uygulama proxy'si dağıtımınız varsa, tüm uygulamaları varsayılan bağlayıcı grubuna atamayın. Bir active Bağlayıcısı gruba atanıncaya kadar bu şekilde, tüm dinamik trafik yeni bağlayıcılar almadığınız. Bu yapılandırma bakım kullanıcılarınızın etkilemeden gerçekleştirebilmeleri için bağlayıcıları bir boşta geri varsayılan grubuna taşıyarak moduna sağlar.

## <a name="prerequisites"></a>Önkoşullar
Bağlayıcılar grubuna emin olmak sahip [birden çok bağlayıcı yüklü](active-directory-application-proxy-enable.md). Yeni bir bağlayıcı yüklediğinizde otomatik olarak birleştiren **varsayılan** bağlayıcı grubu.

## <a name="create-connector-groups"></a>Bağlayıcı grupları oluşturma
İstediğiniz sayıda bağlayıcı grubu oluşturmak için aşağıdaki adımları kullanın. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **uygulama proxy'si**.
2. Seçin **yeni bağlayıcı grubu**. Yeni bir bağlayıcı grubu dikey penceresi görünür.

   ![Yeni bir bağlayıcı grubunu seçin](./media/active-directory-application-proxy-connectors-azure-portal/new-group.png)

3. Yeni bağlayıcı grubunuzu adlandırın, sonra hangi bağlayıcılar bu gruba ait seçmek için açılır menüyü kullanın.
4. **Kaydet**’i seçin.

## <a name="assign-applications-to-your-connector-groups"></a>Uygulamaları bağlayıcı gruplarınıza atayın
Uygulama proxy'si ile yayımlanan her uygulama için bu adımları kullanın. Bir uygulama bir bağlayıcı grubuna ilk yayımladığınızda veya istediğiniz zaman atamasını değiştirmek için aşağıdaki adımları kullanabilirsiniz atayabilirsiniz.   

1. Dizininiz için yönetim panodan seçin **kurumsal uygulamalar** > **tüm uygulamaları** > bir bağlayıcı gruba atamak istediğiniz uygulama > **uygulama proxy'si**.
2. Kullanmak **bağlayıcı grup** açılır menüsünde kullanmak için uygulamayı istediğiniz grubu seçin.
3. Seçin **kaydetmek** değişikliği uygulamak için.

## <a name="use-cases-for-connector-groups"></a>Bağlayıcı grupları için kullanım örnekleri 

Bağlayıcı grupları dahil olmak üzere çeşitli senaryolar için kullanışlıdır:

### <a name="sites-with-multiple-interconnected-datacenters"></a>Birden çok birbirine bağlı veri merkezleri ile siteleri

Birçok kuruluş, birbirine bağlı veri merkezleri sahiptir. Bu durumda, veri merkezi çapraz bağlantıları pahalı ve yavaş olduğundan veri merkezi içinde kadar trafiği mümkün olduğunca tutmak istediğiniz. Bağlayıcılar veri merkezi içinde bulunan uygulamalar sunmak için her veri merkezinde dağıtabilirsiniz. Bu yaklaşım, veri merkezi çapraz bağlantıları en aza indirir ve kullanıcılarınız için tamamen saydam bir deneyim sağlar.

### <a name="applications-installed-on-isolated-networks"></a>Yalıtılmış ağlarda yüklü uygulamalar

Uygulamalar, ana Kurumsal ağın parçası olmayan ağlara barındırılabilir. Ayrıca ağa uygulamalarını yalıtmak için yalıtılmış ağlarda ayrılmış bağlayıcılar yüklemek için bağlayıcı gruplarını kullanabilirsiniz. Bu, genellikle belirli bir uygulama, kuruluşunuz için bir üçüncü taraf satıcı tutar ortaya çıkar. 

Bağlayıcı gruplar, yalnızca belirli uygulamaları yayımlamak bu ağlar için ayrılmış bağlayıcılar yüklemek daha kolay ve uygulama yönetimi üçüncü taraf satıcılar için dış kaynak sağlamak için daha güvenli hale sağlar.

### <a name="applications-installed-on-iaas"></a>Iaas üzerinde yüklü uygulamalar 

Bulut erişimi için Iaas üzerinde yüklü uygulamalar için bağlayıcı grupları güvenli erişim tüm uygulamalar için ortak bir hizmet sağlar. Bağlayıcı gruplar şirket ağınızda ek bağımlılık oluşturun veya uygulama deneyimi parçalara yok. Bağlayıcılar, her bulut veri merkezinde yüklü olması ve yalnızca o ağda bulunan uygulamalar hizmet. Yüksek kullanılabilirlik elde etmek için birkaç bağlayıcı yükleyebilirsiniz.

Sanal ağ için kendi Iaas bağlı birden fazla sanal makine içeren bir kuruluş barındırılan bir örnek olarak alın. Bu uygulamaları kullanmak çalışanların izin vermek için bu özel ağların siteden siteye VPN kullanarak şirket ağına bağlanır. Bu, şirket içinde olan çalışanlar için iyi bir deneyim sağlar. Ancak, aşağıdaki çizimde görüldüğü gibi şirket içinde ek altyapı erişimi yönlendirmek için gerektirdiğinden, uzak çalışanlar için ideal olmayabilir:

![Azuread'i Iaas ağ](./media/application-proxy-publish-apps-separate-networks/application-proxy-iaas-network.png)
  
Azure AD uygulama ara sunucusu Bağlayıcısı gruplarıyla şirket ağınızda ek bağımlılık oluşturmadan tüm uygulamalara erişimi güvenli hale getirmek ortak bir hizmet etkinleştirebilirsiniz:

![Azuread'i Iaas birden çok bulut satıcılar](./media/application-proxy-publish-apps-separate-networks/application-proxy-multiple-cloud-vendors.png)

### <a name="multi-forest--different-connector-groups-for-each-forest"></a>Çoklu orman – her orman için farklı bağlayıcı grupları

Uygulama proxy'si dağıtmış olan müşterilerin çoğu, Kerberos Kısıtlı temsilci (KCD) gerçekleştirerek, çoklu oturum açma (SSO) özellikleri kullanıyor. Bunun için bağlayıcı'nın makinelerin kullanıcılar uygulamayı doğru devredebilirsiniz bir etki alanına katılmış olması gerekir. KCD, ormanlar arası özelliklerini destekler. Ancak aralarında güven ayrı Çoklu orman ortamlarıyla olan şirketler için tek bir bağlayıcıyı tüm ormanlar için kullanılamaz. 

Bu durumda, belirli bağlayıcılar orman dağıtılması ve yalnızca belirli bu ormandaki kullanıcıların sunmak için yayımlanan uygulamalar hizmet edecek şekilde ayarlayın. Her bağlayıcı grubu farklı bir ormanda temsil eder. Tüm ormanlar için Kiracı ve çoğu deneyimi birleşik karşın, kullanıcıların Azure AD grupları kullanarak orman uygulamalarını atanabilir.
 
### <a name="disaster-recovery-sites"></a>Olağanüstü durum kurtarma sitelerinde

Sitelerinizin nasıl uygulandığını bağlı olarak bir olağanüstü durum kurtarma (DR) site ile yapabileceğiniz iki farklı yaklaşım vardır:

* DR sitenizi burada ana sitenin tam olarak gibi ve aynı ağ ve AD ayarları sahip etkin-etkin modda oluşturulursa, ana site ile aynı bağlayıcı grubunda DR sitesinde bağlayıcıları oluşturabilirsiniz. Bu, yük devretme işlemlerini sizin için algılamak Azure AD sağlar.
* DR sitenizi ana sitesinden ayrı, DR sitede farklı bağlayıcı grubu oluşturabilirsiniz ve ya da 1) yedekleme uygulamaları varsa veya 2) el ile DR bağlayıcı Grup varolan bir uygulamaya gerektiğinde yönlendir.
 
### <a name="serve-multiple-companies-from-a-single-tenant"></a>Tek bir kiracı birden çok şirket hizmet

Tek bir hizmet sağlayıcısı dağıtır ve Azure AD tutan bir modeli uygulamak için birçok farklı yolu ilgili birden çok şirket hizmetleri vardır. Bağlayıcı gruplar bağlayıcılar ve farklı gruplar halinde uygulamaları kurabilmeleri yönetici yardımcı olur. Küçük şirketler için uygun olan bir şekilde farklı şirketlerin kendi etki alanı adı ve ağlar varken Kiracı tek bir Azure AD olmalıdır. Bu da burada birkaç şirketler Düzenleyici ya da iş nedenleri için tek bir BT bölme hizmet M & A senaryoları ve durumlar için geçerlidir. 

## <a name="sample-configurations"></a>Örnek Yapılandırması

Uygulayabileceğiniz bazı örnekler aşağıdaki bağlayıcı gruplarını içerir.
 
### <a name="default-configuration--no-use-for-connector-groups"></a>Varsayılan yapılandırma – bağlayıcı grupları için hiçbir kullanın

Bağlayıcı grupları kullanmıyorsanız, yapılandırmanızı şöyle olabilir:

![Azuread'i bağlayıcı grup yok](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-1.png)
 
Bu yapılandırma, küçük dağıtımları ve testleri için yeterlidir. İyi kuruluşunuzun bir düz ağ topolojisi varsa da çalışır.
 
### <a name="default-configuration-and-an-isolated-network"></a>Varsayılan yapılandırma ve yalıtılmış bir ağı

Bu yapılandırma, varsayılan bir Iaas sanal ağ gibi yalıtılmış bir ağda çalışan belirli bir uygulamanın olduğu bir evrimi şöyledir: 

![Azuread'i bağlayıcı grup yok](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-2.png)
 
### <a name="recommended-configuration--several-specific-groups-and-a-default-group-for-idle"></a>Önerilen yapılandırması – birkaç belirli grupları için bir varsayılan grubu boşta

Büyük ve karmaşık kuruluşlar için önerilen yapılandırma, herhangi bir uygulama hizmet değil ve boşta veya yeni yüklenen bağlayıcıları için kullanılan bir grup olarak varsayılan bağlayıcı grubu sağlamaktır. Tüm uygulamalar, özelleştirilmiş bağlayıcı grupları kullanılarak sunulur. Bu, yukarıda açıklanan senaryoları tüm karmaşıklığı sağlar.

Aşağıdaki örnekte, iki veri merkezi, A ve B, her site hizmet iki bağlayıcı ile şirket sahiptir. Her sitenin üzerinde çalışan farklı uygulamalar vardır. 

![Azuread'i bağlayıcı grup yok](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-3.png)
 
## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD uygulama proxy'si bağlayıcılar anlama](application-proxy-understand-connectors.md)
* [Çoklu oturum açmayı etkinleştirme](application-proxy-sso-overview.md)


