---
title: Azure AD uygulama ara sunucusu ile SharePoint için uzaktan erişimi etkinleştir | Microsoft Docs
description: Şirket içi SharePoint server, Azure AD uygulama proxy'si ile tümleştirme hakkında temel kavramları kapsar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5eff7925599931104440213112ce288fd521b61
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66473765"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile SharePoint uzaktan erişimi etkinleştirme

Bu makalede, şirket içi SharePoint server, Azure Active Directory (Azure AD) uygulama proxy'si ile tümleştirme nasıl ele alınmaktadır.

Azure AD uygulama ara sunucusu ile SharePoint için uzaktan erişimi etkinleştirmek için adım adım bu makaledeki bölümler izleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, ortamınızda SharePoint 2013 veya daha yeni zaten sahibi olduğunuzu varsayar. Ayrıca, aşağıdaki önkoşulları göz önünde bulundurun:

* SharePoint yerel Kerberos desteği içerir. Bu nedenle, iç sitelere Azure AD uygulama proxy'si aracılığıyla uzaktan erişen kullanıcılar, bir çoklu oturum açma (SSO) deneyimi sağlamak için kabul edilebilir.

* Bu senaryo, SharePoint Server'ınıza yapılandırma değişiklikleri içerir. Bir hazırlık ortamı kullanmanızı öneririz. Bu şekilde güncelleştirmeleri hazırlama sunucunuza ilk olun ve sonra üretime geçmeden önce bir test döngüsünü kolaylaştırmak.

* Yayımlanmış URL'sini SSL zorunlu kılarız. SSL iç URL'SİNDE gönderilen/eşlenen bağlantıları doğru olduğundan emin olmak için de gereklidir.

## <a name="step-1-configure-kerberos-constrained-delegation-kcd"></a>1. adım: Kerberos'u yapılandırma Kısıtlı temsilci (KCD)

Windows kimlik doğrulaması kullanan şirket içi uygulamalar için çoklu oturum açma (SSO) ile Kerberos kimlik doğrulama protokolünü ve Kerberos Kısıtlı temsilci (KCD) adlı bir özellik elde edebilirsiniz. Kullanıcı için Windows doğrudan oturum henüz bile yapılandırıldığında, KCD, bir kullanıcı için bir Windows belirteç almak uygulama Proxy Bağlayıcısı sağlar. KCD hakkında daha fazla bilgi için bkz: [Kerberos Kısıtlı temsilci seçmeye genel bakış](https://technet.microsoft.com/library/jj553400.aspx).

KCD ' için bir SharePoint sunucusu ayarlamak için aşağıdaki sıralı bölümlerdeki yordamları kullanın:

### <a name="ensure-that-sharepoint-web-application-is-running-under-a-domain-account"></a>SharePoint web uygulaması bir etki alanı hesabı altında çalıştığından emin olun

İlk olarak, SharePoint web uygulaması bir etki alanı hesabı altında--değil yerel sistem, yerel hizmet veya ağ hizmeti olarak çalıştığından emin olun. Bu hesap için hizmet asıl adı (SPN) ekleyebilirsiniz, böylece bunu yapabilirsiniz. SPN'ler Kerberos protokolü farklı hizmetleri nasıl tanımlar var. Ve daha sonra KCD yapılandırmak için hesabı gerekir.

> [!NOTE]
> Önceden oluşturulmuş olması gerekir hizmeti için Azure AD hesabı. Bir otomatik parola değişikliği için izin öneririz. Adımları ve sorun giderme konuları tamamı hakkında daha fazla bilgi için bkz. [SharePoint'te otomatik parola değiştirme yapılandırma](https://technet.microsoft.com/library/ff724280.aspx).

Sitelerinizi bir tanımlı bir hizmet hesabı altında çalıştığından emin olun için aşağıdaki adımları gerçekleştirin:

1. Açık **SharePoint Yönetim Merkezi** site.
2. Git **güvenlik** seçip **hizmet hesaplarını yapılandırın**.
3. Seçin **Web uygulama havuzu - SharePoint - 80**. Seçenekler, web havuzu adına göre biraz farklı olabilir veya web havuzu varsayılan olarak SSL kullanır.

   ![Bir hizmet hesabı yapılandırma seçenekleri](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

4. Varsa **bu bileşen için bir hesap seçersiniz** ayarlanmış **yerel hizmet** veya **ağ hizmeti**, bir hesap oluşturmanız gerekir. Değilse, tamamlanmış ve sonraki bölüme geçebilirsiniz.
5. Seçin **kaydı yeni yönetilen hesabı**. Hesabınızı oluşturduktan sonra ayarlamalısınız **Web uygulama havuzu** hesabı kullanabilmeniz için önce.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>SharePoint hizmet hesabı için bir hizmet asıl adı ayarlayın

KCD yapılandırmadan önce yapmanız gerekir:

* Azure AD Proxy açığa çıkarır SharePoint web uygulamasını çalıştıran etki alanı hesabı belirleyin.
* Azure AD Ara sunucusu ve SharePoint yapılandırılacak bir iç URL seçin. İç URL bu web uygulaması zaten kullanılmamalıdır ve hiçbir zaman web tarayıcınızda görünür.

Seçilen İç URL olduğu varsayılırsa <https://sharepoint>, SPN sonra:

```
HTTP/SharePoint
```

> [!NOTE]
> Lütfen İç URL için aşağıdaki önerileri dikkate:
> * HTTPS kullanın
> * Özel bağlantı noktaları kullanmayın
> * DNS'de, SharePoint WFE (veya yük dengeleyici) üzerine bir konak (A) ve olmayan bir diğer ad (CName) oluştur

Bu SPN kaydını kaydetmek için bir etki alanı yöneticisi olarak komut isteminden aşağıdaki komutu çalıştırın:

```
setspn -S HTTP/SharePoint demo\spAppPoolAccount
```

Bu komut, SPN ayarlar _HTTP/SharePoint_ SharePoint uygulama havuzu hesabının _demo\spAppPoolAccount_.

Değiştirin _HTTP/SharePoint_ iç URL'niz için bir SPN ile ve _demo\spAppPoolAccount_ ortamınızda uygulama havuzu hesabı ile. Bunu eklemeden önce SPN'yi Setspn komut arar. İçinde göreceğiniz zaten bir **SPN değeri yinelenen** hata. Bu durumda, doğru uygulama havuzu hesabı altında ayarlanmazsa mevcut SPN kaldırmayı düşünün.

SPN'yi Setspn komut -L seçeneği ile çalıştırarak eklendiğini doğrulayabilirsiniz. Bu komut hakkında daha fazla bilgi için bkz. [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-trusted-for-delegation-to-the-spn-added-to-the-sharepoint-application-pool-account"></a>Bağlayıcı temsilci seçmek için bir SharePoint uygulama havuzu hesabı eklenen SPN güvenilir olduğundan emin olun

SharePoint uygulama havuzu hesabı için kullanıcı kimliklerini Azure AD uygulama proxy'si hizmeti temsilci seçebilirsiniz KCD yapılandırın. KCD, kullanıcılarınız Azure AD'de kimlik doğrulaması için Kerberos anahtarlarını almak uygulama Proxy Bağlayıcısı'nı etkinleştirerek yapılandırın. Daha sonra o sunucu bağlamı hedef uygulama ya da SharePoint için bu durumda geçirir.

KCD yapılandırmak için her bir bağlayıcı makine için aşağıdaki adımları yineleyin:

1. Bir DC için bir etki alanı yöneticisi olarak oturum açın ve ardından açın **Active Directory Kullanıcıları ve Bilgisayarları**.
2. Bağlayıcıyı çalıştıran bilgisayarı bulmak. Bu örnekte, aynı SharePoint sunucusudur.
3. Bilgisayarı çift tıklatın ve ardından **temsilci** sekmesi.
4. Temsilci seçme ayarları ayarlandığından emin olun **bu bilgisayara yalnızca belirtilen hizmetlere temsilci seçmek için güven**. Ardından, **herhangi bir kimlik doğrulama protokolünü kullan**.
5. Tıklayın **Ekle** düğmesini tıklatın, **kullanıcılar veya bilgisayarlar**, SharePoint uygulama havuzu hesabı, örneğin bulun _demo\spAppPoolAccount_.
6. SPN'ler listesinde, daha önce oluşturduğunuz hizmet hesabı için bir tane seçin.
7. **Tamam** düğmesine tıklayın. Tıklayın **Tamam** yeniden değişiklikleri kaydedin.
  
   ![Temsilci seçme ayarları](./media/application-proxy-integrate-with-sharepoint-server/delegation-box2.png)

## <a name="step-2-configure-azure-ad-proxy"></a>2. adım: Azure AD Proxy Yapılandırma

KCD yapılandırdığınıza göre Azure AD uygulama proxy'si yapılandırmaya hazırsınız.

1. Aşağıdaki ayarlar ile SharePoint sitenizi yayımlayın. Adım adım yönergeler için bkz: [Azure AD uygulama proxy'si kullanarak uygulamaları yayımlama](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).
   * **İç URL**: Daha önce seçilen SharePoint İç URL gibi **<https://SharePoint/>** .
   * **Ön kimlik doğrulama yöntemi**: Azure Active Directory
   * **Bilgilerde URL'yi çevir**: NO

   >[!TIP]
   >SharePoint kullanan _ana bilgisayar üst bilgisini_ siteyi aramak için değer. Ayrıca bu değere göre bağlantılar oluşturur. Net etkisiyle SharePoint oluşturan herhangi bir bağlantıyı dış URL'sini kullanmak üzere doğru şekilde yayımlanmış bir URL olmasıdır. Değerini **Evet** Ayrıca arka uç uygulaması isteği iletmek bağlayıcıyı etkinleştirir. Ancak, ayar değeri **Hayır** bağlayıcı iç ana bilgisayar adını göndermez anlamına gelir. Bunun yerine, bağlayıcıyı arka uç uygulaması için yayımlanan URL olarak ana bilgisayar üst bilgisi gönderir.

   ![SharePoint uygulaması olarak Yayımla](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

2. Uygulamanızı yayımladıktan sonra aşağıdaki adımlarla çoklu oturum açma ayarları yapılandırın:

   1. Uygulama sayfasında portalında seçin **çoklu oturum açma**.
   2. Çoklu oturum açma modu için seçin **tümleşik Windows kimlik doğrulaması**.
   3. İç uygulama SPN'si daha önce ayarlanan değere ayarlayın. Bu örnekte, olacak **HTTP/SharePoint**.
   4. Temsilci oturum açma'nde "kimlik", seçin **şirket içi SAM hesabı adı**.

   ![SSO için tümleşik Windows kimlik doğrulamasını yapılandırma](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

3. Uygulamanızı ayarlama işlemini sonlandırmak için şuraya gidin: **kullanıcılar ve gruplar** bölümünde ve bu uygulamaya erişmek için kullanıcı atama. 

## <a name="step-3-configure-sharepoint-to-use-kerberos-and-azure-ad-proxy-urls"></a>3. adım: Kerberos ve Azure AD Proxy URL'ler kullanmak için SharePoint'i yapılandırma

Sonraki adım, SharePoint web uygulaması İç URL için gönderilen gelen istekleri işlemek için Kerberos ve uygun bir alternatif erişim eşlemeyi SharePoint izin verecek şekilde yapılandırılmış yeni bir bölge için genişletme ve bağlantılar için dış URL'yi yerleşik yanıt sağlamaktır.

1. Başlangıç **SharePoint Yönetim Kabuğu'nu**.
2. Extranet bölgesine web uygulaması'nı genişletin ve Kerberos kimlik doğrulamasını etkinleştirmek için aşağıdaki betiği çalıştırın:

   ```powershell
   # Replace "http://spsites/" with the URL of your web application
   # Replace "https://sharepoint-f128.msappproxy.net/" with the External URL in your Azure AD proxy application
   $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
   Get-SPWebApplication "http://spsites/" | New-SPWebApplicationExtension -Name "SharePoint - AAD Proxy" -SecureSocketsLayer -Zone "Extranet" -Url "https://sharepoint-f128.msappproxy.net/" -AuthenticationProvider $winAp
   ```

3. Açık **SharePoint Yönetim Merkezi** site.
4. Altında **sistem ayarlarını**seçin **alternatif erişim eşlemelerini yapılandırma**. Alternatif erişim eşlemeleri kutusu açılır.
5. Örneğin, sitenizi seçin **SharePoint - 80**. Şu anda, Extranet Bölge henüz düzgün ayarlanıp dahili URL'yi sahip değil:

   ![Alternatif erişim eşlemelerini kutusu](./media/application-proxy-integrate-with-sharepoint-server/alternate-access1.png)

6. Tıklayın **İç URL ekleyin**.
7. İçinde **URL protokolü, ana bilgisayar ve bağlantı noktası** metin kutusuna **İç URL** örneğin Azure AD proxy yapılandırılan <https://SharePoint/>.
8. Select bölge **Extranet** aşağı açılan listesinde.
9. **Kaydet**’e tıklayın.
10. Alternatif erişim eşlemeleri gibi görünmelidir:

    ![Alternatif erişim eşlemelerini düzeltin](./media/application-proxy-integrate-with-sharepoint-server/alternate-access3.png)

## <a name="step-4-ensure-that-an-https-certificate-is-configured-for-the-iis-site-of-the-extranet-zone"></a>4. Adım: Bir HTTPS sertifikası Extranet bölgenin IIS site için yapılandırılmış olduğundan emin olun

SharePoint yapılandırması olan artık tamamlandı, ancak iç URL Extranet bölgenin olduğundan <https://SharePoint/>, bu site için bir sertifika ayarlamanız gerekir.

1. Windows PowerShell konsolunu açın.
2. Kendinden imzalı bir sertifika oluşturur ve MY deposunda bilgisayara eklemek için aşağıdaki betiği çalıştırın:

   ```powershell
   # Replace "SharePoint" with the actual hostname of the Internal URL of your Azure AD proxy application
   New-SelfSignedCertificate -DnsName "SharePoint" -CertStoreLocation "cert:\LocalMachine\My"
   ```

   > [!NOTE]
   > Otomatik olarak imzalanan sertifikaları, yalnızca test amaçları için uygundur. Üretim ortamlarında, bunun yerine bir sertifika yetkilisi tarafından verilen sertifikaların kullanılacak önemle tavsiye edilir.

3. "Internet Information Services Manager" konsolunu açın.
4. Ağaç görünümünde sunucuyu genişletin, "siteler", "SharePoint – AAD Proxy" siteyi seçin ve tıklayın **bağlamaları**.
5. HTTPS bağlamasını seçip tıklayın **Düzenle...** .
6. SSL sertifika alanında, seçtiğiniz **SharePoint** sertifika ve Tamam'a tıklayın.

Artık Azure AD uygulama proxy'si aracılığıyla harici olarak SharePoint sitesine erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD uygulama proxy'sinde özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md)
* [Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)
