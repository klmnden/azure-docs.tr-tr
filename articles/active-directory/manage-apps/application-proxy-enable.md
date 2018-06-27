---
title: Azure AD uygulama proxy'si - Başlarken bağlayıcısını yükleme | Microsoft Docs
description: Azure portalında uygulama ara sunucusunu etkinleştirmek ve bağlayıcıları için ters proxy yükleyin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 0ac14f792c63ea06a484eb5b522c4d33958538ed
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026084"
---
# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Uygulama proxy'si ile başlayın ve Bağlayıcısı'nı yüklemek
Bu makale, Azure AD'deki bulut dizininiz için Microsoft Azure AD Uygulama Ara Sunucusunu etkinleştirme adımlarında size kılavuzluk eder.

Hakkında daha fazla bilgi etmediğinizden henüz kuruluşunuz için güvenlik ve verimlilik avantajlarını farkında uygulama proxy'si getirir, [güvenli uzaktan erişim sağlamak şirket içi uygulamalar](application-proxy.md).

## <a name="application-proxy-prerequisites"></a>Uygulama Ara Sunucusu önkoşulları
Uygulama Ara Sunucusu hizmetlerini etkinleştirip kullanabilmeniz için şunlara sahip olmanız gerekir:

* [Microsoft Azure AD temel veya premium aboneliği](../fundamentals/active-directory-whatis.md) ve genel yöneticisi olduğunuz bir Azure AD dizini.
* Windows Server 2012 R2 veya uygulama Proxy Bağlayıcısı yükleme 2016 çalıştıran bir sunucu. Sunucu uygulama ara sunucusu hizmetlerini Bulut ve yayımladığınız şirket içi uygulamalara bağlanmak gerekir.
  * Çoklu oturum açma için Kerberos Kısıtlı temsilci kullanarak, yayımlanan uygulamalar için bu makine etki alanı-yayımladığınız uygulama ile aynı AD etki alanındaki katılması. Bilgi için bkz: [uygulama proxy'si ile çoklu oturum açma için KCD](application-proxy-configure-single-sign-on-with-kcd.md).

Kuruluşunuzun internet'e bağlanmak için proxy sunucuları kullanıyorsa, okuma [varolan çalışma şirket içi proxy sunucuları](application-proxy-configure-connectors-with-proxy-servers.md) uygulama proxy'si ile çalışmaya başlamadan önce bunların nasıl yapılandırılacağı hakkında ayrıntılı bilgi için.

## <a name="open-your-ports"></a>Bağlantı noktalarını açın

Azure AD uygulama proxy'si için ortamınızı hazırlamak için ilk Azure veri merkezlerinde iletişimini etkinleştirmeniz gerekir. Yolda bir güvenlik duvarı varsa Bağlayıcının, Uygulama Ara Sunucusuna HTTPS (TCP) istekleri yapabilmesi için güvenlik duvarının açık olduğundan emin olun.

1. Aşağıdaki bağlantı noktalarını açmak **giden** trafiği:

   | Bağlantı noktası numarası | Nasıl kullanılır |
   | --- | --- |
   | 80 | İndirme sertifika iptal listeleri (CRL'ler SSL sertifikası doğrulanırken) |
   | 443 | Uygulama proxy'si hizmeti ile tüm giden iletişimi |

   Duvarınız kaynak kullanıcılar için trafiği zorunlu bir ağ hizmeti olarak çalışan Windows hizmetlerinden trafik için bu bağlantı noktalarını açın.

   > [!IMPORTANT]
   > Bağlayıcı sürümleri 1.5.132.0 bağlantı noktası gereksinimleri tablosu yansıtır ve daha yeni. Bağlayıcı sürümün hala varsa, aşağıdaki bağlantı noktaları 80 ve 443: 5671, 8080 yanı sıra etkinleştirmek etmeniz 9090-9091 9350, 9352, 10100 – 10120.
   >
   >Bağlayıcılar en yeni sürüme güncelleştirme hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama proxy'si Bağlayıcılar](application-proxy-connectors.md#automatic-updates).

2. Güvenlik Duvarı veya proxy DNS uygulamaları güvenilir listeye almayı izin veriyorsa, msappproxy.net ve servicebus.windows.net beyaz liste bağlantıları kullanabilirsiniz. Erişmesine izin vermek, gerekirse [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), her hafta güncelleştirilir.

3. Microsoft, sertifikaları doğrulamak için dört adreslerini kullanır. Diğer ürünler için yapmadıysanız aşağıdaki URL'lere erişim izin ver:
   * mscrl.microsoft.com:80
   * CRL.microsoft.com:80
   * ocsp.msocsp.com:80
   * www.microsoft.com:80

4. Bağlayıcınızı kayıt işlemi için login.windows.net ve login.microsoftonline.com erişimi olmalıdır.


## <a name="install-and-register-a-connector"></a>Yükleyin ve bir bağlayıcı kaydedin
1. Bir yönetici olarak oturum açın [Azure portal](https://portal.azure.com/).
2. Geçerli dizininiz sağ üst köşedeki kullanıcı adınıza altında görüntülenir. Dizinleri değiştirmeniz gerekiyorsa, bu simgeyi seçin.
3. Git **Azure Active Directory** > **uygulama proxy'si**.

   ![Uygulama proxy'si gidin](./media/application-proxy-enable/app_proxy_navigate.png)

4. Seçin **karşıdan bağlayıcı**.

   ![Bağlayıcıyı indir](./media/application-proxy-enable/download_connector.png)

5. Önkoşullara göre hazırladığınız sunucuda **AADApplicationProxyConnectorInstaller.exe** öğesini çalıştırın.
6. Yüklemek için sihirbazdaki yönergeleri uygulayın. Yükleme sırasında bağlayıcıyı Azure AD kiracınızın uygulama proxy'si ile kaydetmeniz istenir.

   * Azure AD genel yönetici kimlik bilgilerinizi sağlayın. Genel yönetici kiracınız, Microsoft Azure kimlik bilgilerinizden farklı olabilir.
   * Bağlayıcıyı kaydeden yöneticinin Uygulama Proxy hizmetini etkinleştirdiğiniz dizinde olduğundan emin olun. Örneğin, kiracı etki alanı contoso.com ise yönetici admin@contoso.com veya bu etki alanındaki başka bir diğer ad olmalıdır.
   * Varsa **IE Artırılmış Güvenlik Yapılandırması** ayarlanır **üzerinde** Bağlayıcısı'nı yüklediğiniz sunucuda, kayıt ekranı göremeyebilirsiniz. Erişim sağlamak için hata iletisindeki yönergeleri izleyin. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.

Yüksek düzeyde kullanılabilirlik sağlamak için en az iki bağlayıcı dağıtmanız gerekir. Her bağlayıcı ayrı ayrı kaydedilmelidir.

## <a name="test-that-the-connector-installed-correctly"></a>Bağlayıcısı'nı doğru şekilde yüklenmemiş test

Yeni bir bağlayıcı için ya da Azure portal denetleyerek veya sunucunuzda doğru yüklendiğini doğrulayabilirsiniz. 

Kiracınız için Azure portalında oturum açın ve gidin **Azure Active Directory** > **uygulama proxy'si**. Tüm bağlayıcılar ve bağlayıcı gruplarını, bu sayfada görüntülenir. Ayrıntılarını görmek veya farklı bağlayıcı grubuna taşımak için bir bağlayıcı seçin. 

Sunucunuzda, bağlayıcıyı ve connector updater için etkin hizmetlerin listesini kontrol edin. İki hizmet çalıştırdıktan hemen Başlat ancak Aksi durumda, bunları etkinleştirmek gerekir: 

   * **Microsoft AAD Application Proxy Connector** bağlantıyı etkinleştirir

   * **Microsoft AAD Application Proxy Connector Updater** bir otomatik güncelleştirme hizmetidir. Güncelleştirici bağlayıcının yeni sürümlerini denetler ve bağlayıcıyı gereken şekilde güncelleştirir.

   ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/application-proxy-enable/app_proxy_services.png)

Bağlayıcılar ve nasıl güncel kalmasını hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama proxy'si Bağlayıcılar](application-proxy-connectors.md).


## <a name="next-steps"></a>Sonraki adımlar
Artık [Uygulama Ara Sunucusu ile uygulamaları yayımlamaya](application-proxy-publish-azure-portal.md) hazırsınız.

Ayrı ağlarda ya da farklı konumlarda uygulamalarınız varsa, farklı bağlayıcılar mantıksal birimler halinde düzenlemek için bağlayıcı gruplarını kullanın. [Uygulama Proxy bağlayıcıları ile çalışma](application-proxy-connector-groups.md) hakkında daha fazla bilgi edinin.
