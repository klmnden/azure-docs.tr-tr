---
title: "Klasik Portalı'nda Azure AD uygulama ara sunucusunu etkinleştirme | Microsoft Docs"
description: "Klasik Azure portalındaki Uygulama Ara Sunucusunu kapatıp ters ara sunucuya ilişkin Bağlayıcıları yükleyin."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 073d81e495b9cacbe81f375b09bfcad23aadb22e
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="enable-application-proxy-in-the-classic-portal-and-download-connectors"></a>Klasik Portalı'nda uygulama ara sunucusunu etkinleştirme ve bağlayıcılar indirin
Bu makale, Azure AD'deki bulut dizininiz için Microsoft Azure AD Uygulama Ara Sunucusunu etkinleştirme adımlarında size kılavuzluk eder.

Uygulama Ara Sunucusu ile neler yapabileceğinizi öğrenmek istiyorsanız [Şirket içi uygulamalara güvenli uzaktan erişim sağlama](active-directory-application-proxy-get-started.md) hakkında daha fazla bilgi edinin.

## <a name="application-proxy-prerequisites"></a>Uygulama Ara Sunucusu önkoşulları
Uygulama Ara Sunucusu hizmetlerini etkinleştirip kullanabilmeniz için şunlara sahip olmanız gerekir:

* [Microsoft Azure AD temel veya premium aboneliği](active-directory-editions.md) ve genel yöneticisi olduğunuz bir Azure AD dizini.
* Windows Server 2012 R2 veya uygulama Proxy Bağlayıcısı yükleme 2016 çalıştıran bir sunucu. Sunucu, buluttaki Uygulama Ara Sunucusu hizmetlerine istek gönderir ve yayımlamakta olduğunuz uygulamalara yönelik bir HTTP veya HTTPS bağlantısının olmasını gerektirir.
  * Yayımladığınız uygulamalarda çoklu oturum açma için bu makinenin yayımlamakta olduğunuz uygulamalarla aynı AD etki alanına katılmış olması gerekir. Bilgi için bkz: [uygulama proxy'si ile çoklu oturum açma](active-directory-application-proxy-sso-using-kcd.md)
* Kuruluşunuzun internet'e bağlanmak için proxy sunucuları kullanıyorsa, okuma [varolan çalışma şirket içi proxy sunucuları](application-proxy-working-with-proxy-servers.md) bunların nasıl yapılandırılacağı hakkında ayrıntılı bilgi için.

## <a name="open-your-ports"></a>Bağlantı noktalarını açın

Azure AD uygulama proxy'si için ortamınızı hazırlamak için ilk Azure veri merkezlerinde iletişimini etkinleştirmeniz gerekir. Yolda bir güvenlik duvarı varsa Bağlayıcının, Uygulama Ara Sunucusuna HTTPS (TCP) istekleri yapabilmesi için güvenlik duvarının açık olduğundan emin olun.

1. Aşağıdaki bağlantı noktalarını açmak **giden** trafiği:

   | Bağlantı noktası numarası | Nasıl kullanılır |
   | --- | --- |
   | 80 | İndirme sertifika iptal listeleri (CRL'ler SSL sertifikası doğrulanırken) |
   | 443 | Uygulama proxy'si hizmeti ile tüm giden iletişimi |

   Güvenlik duvarınız kaynak kullanıcılar için trafiği zorunlu kılarsa Ağ Hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için bu bağlantı noktalarını açın.

   > [!IMPORTANT]
   > Bağlayıcı sürümleri 1.5.132.0 bağlantı noktası gereksinimleri tablosu yansıtır ve daha yeni. Bağlayıcı sürümün hala varsa, ayrıca aşağıdaki bağlantı noktalarını etkinleştirmeniz gerekir: 5671, 8080, 9090, 9091, 9350, 9352 ve 10100 – 10120.
   >
   >Bağlayıcılar en yeni sürüme güncelleştirme hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama proxy'si Bağlayıcılar](application-proxy-understand-connectors.md#automatic-updates).

2. Güvenlik Duvarı veya proxy DNS uygulamaları güvenilir listeye almayı izin veriyorsa, msappproxy.net ve servicebus.windows.net beyaz liste bağlantıları kullanabilirsiniz. Erişmesine izin vermek, gerekirse [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), her hafta güncelleştirilir.

3. Kullanım [Azure AD uygulama Proxy Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/) Bağlayıcınızı uygulama proxy'si hizmeti ulaşabilir doğrulanamadı. En azından, Orta ABD bölgesi ve size en yakın bölgeyi tüm yeşil onay işaretli olduğundan emin olun. Bunun ötesinde, daha fazla yeşil onay işaretleri büyük esneklik anlamına gelir.

## <a name="enable-application-proxy-in-azure-ad"></a>Azure AD'de uygulama ara sunucusunu etkinleştirme
1. [Klasik Azure portalında](https://manage.windowsazure.com/) yönetici olarak oturum açın.
2. Active Directory'ye gidip Uygulama Ara Sunucusunu etkinleştirmek istediğiniz dizini seçin.

    ![Active Directory - simge](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Dizin sayfasında **Configure (Yapılandır)** seçeneğini belirleyip **Application Proxy (Uygulama Ara Sunucusu)** için aşağı kaydırın.
4. **Enable Application Proxy Services for this Directory (Bu Dizin için Uygulama Ara Sunucusu Hizmetlerini Etkinleştir)** seçeneğini **Enabled (Etkin)** olarak değiştirin.

    ![Uygulama Ara Sunucusunu etkinleştirme](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. **Download now (Şimdi indir)** seçeneğini belirleyin. **Azure AD uygulama ara sunucusu Bağlayıcısı indirme** açar. Lisans koşullarını okuyup kabul edin ve bağlayıcıya ait Windows Installer dosyasını (.exe) kaydetmek için **İndir**'e tıklayın.

## <a name="install-and-register-the-connector"></a>Yükleyin ve bağlayıcı kaydedin
1. Önkoşullara göre hazırladığınız sunucuda **AADApplicationProxyConnectorInstaller.exe** öğesini çalıştırın.
2. Yüklemek için sihirbazdaki yönergeleri uygulayın.
3. Yükleme sırasında bağlayıcıyı Azure AD kiracınızın uygulama proxy'si ile kaydetmeniz istenir.

   * Azure AD genel yönetici kimlik bilgilerinizi sağlayın. Genel yönetici kiracınız, Microsoft Azure kimlik bilgilerinizden farklı olabilir.
   * Bağlayıcıyı kaydeden yöneticinin Uygulama Proxy hizmetini etkinleştirdiğiniz dizinde olduğundan emin olun. Örneğin, kiracı etki alanı contoso.com ise yönetici admin@contoso.com veya bu etki alanındaki başka bir diğer ad olmalıdır.
   * Varsa **IE Artırılmış Güvenlik Yapılandırması** ayarlanır **üzerinde** sunucuda, kayıt ekranı engellenebilir. Erişime izin vermek için hata iletisindeki yönergeleri izleyin. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.
   * Bağlayıcı kaydı başarısız olursa bkz. [Uygulama Proxy’si Sorunlarını Giderme](active-directory-application-proxy-troubleshoot.md).  
4. Yükleme tamamlandığında sunucunuza iki yeni hizmet eklenir:

   * **Microsoft AAD Application Proxy Connector** bağlantıyı etkinleştirir

     * **Microsoft AAD Application Proxy Connector Updater** bir otomatik güncelleştirme hizmetidir. Düzenli aralıklarla bağlayıcının yeni sürümlerini denetler ve bağlayıcıyı gereken şekilde güncelleştirir.

     ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Yükleme penceresinde **Son**'a tıklayın.

Bağlayıcılar hakkında bilgi için bkz. [Azure AD Uygulama Ara Sunucusu bağlayıcıları](application-proxy-understand-connectors.md).

Yüksek düzeyde kullanılabilirlik sağlamak için en az iki bağlayıcı dağıtmanız gerekir. Daha fazla bağlayıcılar dağıtmak için 2 ve 3. adımları yineleyin. Her bağlayıcı ayrı ayrı kaydedilmelidir.

Bağlayıcıyı kaldırmak isterseniz hem Bağlayıcı hizmetini hem de Updater hizmetini kaldırın. Hizmeti tam olarak kaldırmak için bilgisayarınızı yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Artık [Uygulama Ara Sunucusu ile uygulamaları yayımlamaya](active-directory-application-proxy-publish.md) hazırsınız.

Ayrı ağlarda veya farklı konumlarda uygulamalarınız varsa farklı bağlayıcıları mantıksal birimler halinde düzenlemek için bağlayıcı gruplarını kullanabilirsiniz. [Uygulama Proxy bağlayıcıları ile çalışma](active-directory-application-proxy-connectors-azure-portal.md) hakkında daha fazla bilgi edinin.
