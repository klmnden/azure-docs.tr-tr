---
title: Azure AD uygulama ara sunucusu - Başlarken bağlayıcısını yükleme | Microsoft Docs
description: Azure portalında uygulama ara sunucusunu etkinleştirmek ve için ters proxy bağlayıcıları yükleyin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 59ca9ca7711904fe7882aac4878bd62c597645d8
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51034975"
---
# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Bağlayıcıyı yükleyin ve uygulama ara sunucusu ile çalışmaya başlama
Bu makale, Azure AD'deki bulut dizininiz için Microsoft Azure AD Uygulama Ara Sunucusunu etkinleştirme adımlarında size kılavuzluk eder.

Gerekmez ancak uygulama proxy'si kuruluşunuz için güvenlik ve üretkenlik avantajlarıyla farkında getirir, daha fazla bilgi edinin [güvenli uzaktan erişim sağlamak şirket içi uygulamalara](application-proxy.md).

## <a name="application-proxy-prerequisites"></a>Uygulama Ara Sunucusu önkoşulları
Uygulama Ara Sunucusu hizmetlerini etkinleştirip kullanabilmeniz için şunlara sahip olmanız gerekir:

* [Microsoft Azure AD temel veya premium aboneliği](../fundamentals/active-directory-whatis.md) ve genel yöneticisi olduğunuz bir Azure AD dizini.
* Windows Server 2012 R2 veya uygulama ara sunucusu bağlayıcısını yüklemek, 2016 çalıştıran bir sunucu. Sunucu, Bulut ve yayımlamakta olduğunuz şirket içi uygulamalarda uygulama ara sunucusu hizmetlerine bağlanabilir olması gerekiyor.
  * Çoklu oturum açma için Kerberos kısıtlanmış temsil kullanarak yayımladığınız uygulamalarda, bu makinenin etki alanı-yayımlamakta olduğunuz uygulamalarla aynı AD etki alanına katılması. Bilgi için [uygulama proxy'si ile çoklu oturum açma için KCD](application-proxy-configure-single-sign-on-with-kcd.md).
* TLS 1.2 temel işletim sistemi üzerinde çalışan. TLS 1.2 değiştirmek için adımları izleyin. [etkinleştirme TLS 1.2](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-prerequisites#enable-tls-12-for-azure-ad-connect). Azure AD Connect için içeriği olmakla birlikte, bu yordamı tüm .NET istemcileri için aynıdır.

Kuruluşunuz internet'e bağlanmak için proxy sunucuları kullanıyorsa, okuma [iş mevcut şirket içi proxy sunucuları](application-proxy-configure-connectors-with-proxy-servers.md) uygulaması Ara sunucusu ile çalışmaya başlamadan önce bunları yapılandırma hakkında daha fazla ayrıntı için.

## <a name="open-your-ports"></a>Bağlantı noktalarını açma

Azure AD uygulama proxy'si için ortamınızı hazırlama için ilk Azure veri merkezlerine iletişim etkinleştirmeniz gerekir. Yolda bir güvenlik duvarı varsa Bağlayıcının, Uygulama Ara Sunucusuna HTTPS (TCP) istekleri yapabilmesi için güvenlik duvarının açık olduğundan emin olun.

1. Aşağıdaki bağlantı noktalarının açık **giden** trafik:

   | Bağlantı noktası numarası | Nasıl kullanılır |
   | --- | --- |
   | 80 | İndirme sertifika iptal listelerini (CRL'ler SSL sertifikası doğrulanırken) |
   | 443 | Uygulama proxy'si hizmeti ile tüm giden iletişimi |

   Duvarınız kaynak kullanıcılar için trafiği zorunlu kılarsa ağ hizmeti olarak çalışan Windows hizmetlerinden trafik için bu bağlantı noktalarını açın.

   > [!IMPORTANT]
   > 1.5.132.0 bağlayıcı sürümleri için bağlantı noktası gereksinimleri tabloyu yansıtan ve daha yeni. Hala eski bir bağlayıcı sürümü varsa, aşağıdaki bağlantı noktaları 80 ve 443: 5671, 8080 yanı sıra etkinleştirmek etmeniz 9090 9091, 9350 9352, 10100 – 10120.
   >
   >Bağlayıcılarınızı en son sürüme güncelleştirme hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md#automatic-updates).

2. Güvenlik Duvarı veya proxy DNS beyaz listeye ekleme izin veriyorsa: msappproxy.net ve servicebus.windows.net beyaz liste bağlantıları kullanabilirsiniz. Erişime izin vermek, gerekirse [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), her hafta güncelleştirilir.

3. Microsoft, sertifikaları doğrulamak için dört adresi kullanır. Diğer ürünler için yapmadıysanız aşağıdaki URL'lere erişim izin ver:
   * mscrl.microsoft.com:80
   * CRL.microsoft.com:80
   * ocsp.msocsp.com:80
   * www.microsoft.com:80

4. Bağlayıcınızı, kayıt işlemi için login.windows.net ve login.microsoftonline.com erişmesi gerekir.


## <a name="install-and-register-a-connector"></a>Yükleme ve bir bağlayıcıyı kaydetme
1. Yönetici olarak oturum açın [Azure portalında](https://portal.azure.com/).
2. Mevcut directory sağ üst köşedeki kullanıcı adınıza altında görünür. Dizinleri değiştirmeniz gerekiyorsa, bu simgeyi seçin.
3. Git **Azure Active Directory** > **uygulama proxy'si**.

   ![Uygulama proxy'sine gidin](./media/application-proxy-enable/app_proxy_navigate.png)

4. Seçin **Bağlayıcısı indirme**.

   ![Bağlayıcıyı indir](./media/application-proxy-enable/download_connector.png)

5. Önkoşullara göre hazırladığınız sunucuda **AADApplicationProxyConnectorInstaller.exe** öğesini çalıştırın.
6. Yüklemek için sihirbazdaki yönergeleri uygulayın. Yükleme sırasında bağlayıcıyı Azure AD kiracınızın uygulama Proxy ile kaydetmeniz istenir.

   * Azure AD genel yönetici kimlik bilgilerinizi sağlayın. Genel yönetici kiracınız, Microsoft Azure kimlik bilgilerinizden farklı olabilir.
   * Bağlayıcıyı kaydeden yöneticinin Uygulama Proxy hizmetini etkinleştirdiğiniz dizinde olduğundan emin olun. Örneğin, kiracı etki alanı contoso.com ise yönetici admin@contoso.com veya bu etki alanındaki başka bir diğer ad olmalıdır.
   * Varsa **IE Artırılmış Güvenlik Yapılandırması** ayarlanır **üzerinde** bağlayıcıyı yüklediğiniz sunucuda, kayıt ekranı göremeyebilirsiniz. Erişim sağlamak için hata iletisindeki yönergeleri uygulayın. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.

Yüksek düzeyde kullanılabilirlik sağlamak için en az iki bağlayıcı dağıtmanız gerekir. Her bağlayıcı ayrı ayrı kaydedilmelidir.

## <a name="test-that-the-connector-installed-correctly"></a>Bağlayıcı doğru şekilde yüklenip test

Yeni bir bağlayıcı için Azure portalını ya da denetleyerek veya sunucunuzda doğru yüklendiğini doğrulayabilirsiniz. 

Kiracınız için Azure portalında oturum açın ve gidin **Azure Active Directory** > **uygulama proxy'si**. Tüm bağlayıcılar ve bağlayıcı gruplarını bu sayfada görüntülenir. Ayrıntılarını görmek veya farklı bir bağlayıcı grubuna taşımak için bir bağlayıcı seçin. 

Sunucunuzda, etkin hizmet bağlayıcıyı ve connector updater listesini kontrol edin. İki hizmeti hemen çalıştırmaya başlayın ancak Aksi durumda, bunları açmak gerekir: 

   * **Microsoft AAD Application Proxy Connector** bağlantıyı etkinleştirir

   * **Microsoft AAD Application Proxy Connector Updater** bir otomatik güncelleştirme hizmetidir. Güncelleştirici, bağlayıcının yeni sürümlerini denetleyen ve bağlayıcıyı gereken şekilde güncelleştiren.

   ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/application-proxy-enable/app_proxy_services.png)

Bağlayıcılar ve nasıl güncel kalın hakkında daha fazla bilgi için bkz. [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md).


## <a name="next-steps"></a>Sonraki adımlar
Artık [Uygulama Ara Sunucusu ile uygulamaları yayımlamaya](application-proxy-publish-azure-portal.md) hazırsınız.

Ayrı ağlarda veya farklı konumlarda uygulamalarınız varsa farklı bağlayıcıları mantıksal birimler halinde düzenlemek için bağlayıcı gruplarını kullanın. [Uygulama Proxy bağlayıcıları ile çalışma](application-proxy-connector-groups.md) hakkında daha fazla bilgi edinin.
