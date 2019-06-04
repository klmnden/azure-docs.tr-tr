---
title: Şirket içi uygulama - Azure Active Directory'de uygulama ara sunucusu ekleme | Microsoft Docs
description: Azure Active Directory (Azure AD), kullanıcıların kendi Azure AD hesabıyla oturum açarak şirket uygulamalarına erişmelerini sağlayan bir uygulama proxy'si hizmeti vardır. Bu öğretici, uygulama proxy'si ile kullanmak için ortamınızı hazırlama işlemini göstermektedir. Ardından, Azure AD kiracınız için bir şirket içi uygulama eklemek için Azure portalını kullanır.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: tutorial
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82c7b698f655b82ba95f66127f27a921def02cde
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66472986"
---
# <a name="tutorial-add-an-on-premises-application-for-remote-access-through-application-proxy-in-azure-active-directory"></a>Öğretici: Azure Active Directory Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme

Azure Active Directory (Azure AD), kullanıcıların kendi Azure AD hesabıyla oturum açarak şirket uygulamalarına erişmelerini sağlayan bir uygulama proxy'si hizmeti vardır. Bu öğretici, uygulama proxy'si ile kullanmak için ortamınızı hazırlar. Ortamınızı hazır hale geldikten sonra Azure AD kiracınız için bir şirket içi uygulama eklemek için Azure portalını kullanacaksınız.

Bu öğreticide:

> [!div class="checklist"]
> * Giden trafik için bağlantı noktalarını açar ve belirli URL'lere erişim sağlar.
> * Bağlayıcı, Windows sunucuya yükler ve uygulama proxy'sine
> * Yüklü ve kayıtlı doğru bağlayıcı doğrular
> * Azure AD kiracınız için bir şirket içi uygulama ekler
> * Bir test kullanıcısı bir Azure AD hesabını kullanarak uygulamada oturum açabilir doğrular

## <a name="before-you-begin"></a>Başlamadan önce

Şirket içi bir uygulamayı Azure AD'ye ekleme için gerekir:

* A [Microsoft Azure AD temel veya premium aboneliği](https://azure.microsoft.com/pricing/details/active-directory)
* Uygulama yönetici hesabı
* Kullanıcı kimliklerini bir şirket içi dizininizden eşitlenmiş veya doğrudan Azure AD kiracılar içinde oluşturulur. Azure AD uygulama proxy'si için bunları erişim verme yayımlanan uygulamaları önce önceden kimlik doğrulamasını ve çoklu oturum açma (SSO) gerçekleştirmek için gerekli kullanıcı kimlik bilgilerini sağlamak için kimlik eşitleme sağlar.

### <a name="windows-server"></a>Windows server

Uygulama proxy'si kullanmak için Windows Server 2012 R2 çalıştıran bir Windows server gerekir veya üzeri. Sunucu üzerinde uygulama ara sunucusu bağlayıcısını yükleyeceksiniz. Uygulama Ara sunucusu hizmetlerini azure'da ve şirket içi uygulamaları yayımlamayı planlama bağlanmak bu bağlayıcı sunucusu gerekir.

Üretim ortamınızda, yüksek kullanılabilirlik için birden fazla Windows sunucusu olması önerilir. Bu öğreticide, bir Windows server yeterli olur.

#### <a name="recommendations-for-the-connector-server"></a>Bağlayıcı sunucusu için öneriler

1. Fiziksel olarak yakın uygulama sunucularını bağlayıcıyı ve uygulama arasında performansını iyileştirmek için bağlayıcı sunucusu bulun. Daha fazla bilgi için [ağ topolojisi hakkında önemli noktalar](application-proxy-network-topology.md).

2. Bağlayıcı sunucusu ve web uygulama sunucuları aynı Active Directory etki alanına ait veya gerekir güvenen etki alanlarına yayılan. Sunucuların aynı etki alanında olması veya güvenen etki alanlarına tümleşik Windows kimlik doğrulaması (IWA) ve Kerberos Kısıtlı temsilci (KCD) ile çoklu oturum açma (SSO) kullanarak bir gereksinimdir. Bağlayıcı sunucusu ve web uygulama sunucuları farklı Active Directory etki alanlarında ise, kaynak tabanlı temsilci seçme için çoklu oturum açmayı kullanmanız gerekir. Daha fazla bilgi için [uygulama proxy'si ile çoklu oturum açma için KCD](application-proxy-configure-single-sign-on-with-kcd.md).

#### <a name="tls-requirements"></a>TLS gereksinimleri

Windows bağlayıcı sunucusu TLS 1.2 uygulama ara sunucusu bağlayıcısını yüklemeden önce etkin olmalıdır.

TLS 1.2 etkinleştirmek için:

1. Aşağıdaki kayıt defteri anahtarlarını ayarlayın:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. Sunucuyu yeniden başlatın.

>[!Important] 
> Müşterilerimiz için sınıfının en iyisi şifreleme sağlamak, güncelleştirmeleri yalnızca TLS 1.2 protokolleri erişimi sınırlamak için uygulama proxy'si hizmeti yapıyoruz. Değişiklik aşamalı olarak yalnızca TLS 1.2 protokolleri kullanıyorsanız ve bu değişiklik herhangi bir etki görmez müşterilere sunulacaktır müşteri hazırlık temel. TLS 1.0 ve 1.1 kullanımdan kaldırma 31 Ağustos 2019 üzerinde tamamlanır ve müşterilerin bu değişikliğe hazırlanmak için gelişmiş uyarı alırsınız. Tüm istemci-sunucu ve tarayıcı sunucu birleşimleri için bu değişiklik yapma, emin hazırlamak için uygulama ara Sunucusu hizmetine bağlantıyı korumak için TLS 1.2 kullanmak üzere güncelleştirilir. Bunlar, kullanıcılarınızın uygulama proxy'si aracılığıyla yayımlanan uygulamalara erişmek için kullandığınız istemci içerir. Hazırlama için bkz. [TLS 1.2 Office 365'te](https://support.microsoft.com/help/4057306/preparing-for-tls-1-2-in-office-365) yararlı başvurular ve kaynaklar.

## <a name="prepare-your-on-premises-environment"></a>Şirket içi ortamınızı hazırlama

Azure AD uygulama proxy'si için ortamınızı hazırlamak için Azure veri merkezlerine iletişim etkinleştirerek başlatın. Yolda bir güvenlik duvarı varsa, açık olduğundan emin olun. Açık bir güvenlik duvarı uygulama ara sunucusuna HTTPS (TCP) isteğinde bulunmak bağlayıcı sağlar.

### <a name="open-ports"></a>Bağlantı noktalarını açma

Aşağıdaki bağlantı noktalarının açık **giden** trafiği. 

   | Bağlantı noktası numarası | Nasıl kullanılır |
   | --- | --- |
   | 80 | İndirme sertifika iptal listelerini (CRL'ler SSL sertifikası doğrulanırken) |
   | 443 | Uygulama proxy'si hizmeti ile tüm giden iletişimi |

Ayrıca duvarınız kaynak kullanıcılar için trafiği zorunlu kılarsa ağ hizmeti olarak çalışan Windows hizmetlerinden 80 ve trafiği için 443 bağlantı noktalarını açın.

Uygulama Ara sunucusu zaten kullanıyorsanız, yüklü bağlayıcı daha eski bir sürümü olabilir. Bağlayıcı en son sürümünü yüklemek için bu öğreticiden yararlanın. Sürümleri 1.5.132.0 daha önce de aşağıdaki bağlantı noktalarını açma gerektirir: 5671, 8080, 9090-9091, 9350, 9352, 10100–10120. 

### <a name="allow-access-to-urls"></a>URL'lere erişim izni

Aşağıdaki URL'lere erişim izin ver:

| URL'si | Nasıl kullanılır |
| --- | --- |
| \*. msappproxy.net<br>\*. servicebus.windows.net | Bağlayıcı ve uygulama proxy'si bulut hizmeti arasında iletişim |
| mscrl.microsoft.com:80<br>CRL.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Azure, bu URL'ler sertifikaları doğrulamak için kullanır. |
| login.windows.net<br>login.microsoftonline.com<br>secure.aadcdn.microsoftonline-p.com  | Bağlayıcı, bu URL'ler kayıt işlemi sırasında kullanır. |

Bağlantılara izin vermek \*. msappproxy.net ve \*. güvenlik duvarı veya proxy DNS yapılandırmanıza izin veriyorsa servicebus.windows.net izin listeleri. Erişime izin vermek, gerekirse [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). IP aralıklarını haftalık olarak güncelleştirilir.

## <a name="install-and-register-a-connector"></a>Yükleme ve bir bağlayıcıyı kaydetme

Uygulama proxy'si kullanmak için bağlayıcı uygulama proxy'si hizmeti ile kullandığınız her Windows server yükleyin. Giden bağlantı şirket içi uygulama sunucularından Azure AD'de uygulama ara sunucusuna yöneten bir aracı Bağlayıcıdır. Bir bağlayıcı sunucuları gibi Azure AD Connect'in yüklü diğer kimlik doğrulama aracılarının de yükleyebilirsiniz.

Bağlayıcıyı yüklemek için:

1. Oturum [Azure portalında](https://portal.azure.com/) uygulama proxy'si kullanacağı dizinin bir uygulama yöneticisi olarak. Örneğin, Kiracı etki alanı contoso.com ise yönetici olmalıdır admin@contoso.com ya da bu etki alanındaki başka bir yönetici diğer.
2. Sağ üst köşesinde kullanıcı adınızı seçin. Uygulama Ara sunucusu kullanan bir dizine oturum açtıysanız doğrulayın. Dizinler arasında geçiş yapmak gerekiyorsa seçin **dizini Değiştir** ve uygulama proxy'si kullanan bir dizin seçin.
3. Sol gezinti panelinde seçin **Azure Active Directory**. 
4. Altında **Yönet**seçin **uygulama proxy'si**.
5. Seçin **bağlayıcı hizmeti indir**.
    
    ![Bağlayıcı hizmeti indir](./media/application-proxy-add-on-premises-application/application-proxy-download-connector-service.png)

6. Hizmet koşullarını okuyun.  Hazır olduğunuzda **koşulları kabul et ve indir**.
7. Pencerenin alt kısmında seçin **çalıştırma** Bağlayıcısı'nı yüklemek için. Yükleme Sihirbazı açılır. 
8. Hizmeti yüklemek için sihirbazdaki yönergeleri izleyin. Bağlayıcıyı Azure AD kiracınız için uygulama proxy'si ile kaydetmek için istendiğinde, uygulama Yöneticisi kimlik bilgilerinizi sağlayın.
    - İçin Internet Explorer (IE), varsa **IE Artırılmış Güvenlik Yapılandırması** ayarlanır **üzerinde**, kayıt ekranı göremeyebilirsiniz. Erişim sağlamak için hata iletisindeki yönergeleri uygulayın. Emin olun **Internet Explorer Artırılmış Güvenlik Yapılandırması** ayarlanır **kapalı**.

### <a name="general-remarks"></a>Genel açıklamalar

En son sürümünü almak için bir bağlayıcı daha önce yüklediyseniz, yeniden yükleyin. Daha önce yayımlanmış sürümleri ve bunlar değişiklikler hakkındaki bilgileri görmek için eklemek için bkz: [uygulama ara sunucusu: Sürüm yayınlama geçmişi](application-proxy-release-version-history.md).

Şirket içi uygulamalarınız için birden fazla Windows server seçerseniz, yüklemeniz ve her sunucuya bağlayıcı kaydetmeniz gerekir. Bağlayıcılar, bağlayıcı gruplar halinde düzenleyebilirsiniz. Daha fazla bilgi için [bağlayıcı grupları](application-proxy-connector-groups.md). 

Kuruluşunuz internet'e bağlanmak için proxy sunucuları kullanıyorsa bunları için uygulama ara sunucusunu yapılandırmanız gerekir.  Daha fazla bilgi için [iş mevcut şirket içi proxy sunucuları](application-proxy-configure-connectors-with-proxy-servers.md). 

Bağlayıcılar, kapasite planlaması ve nasıl güncel kalın hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md). 


## <a name="verify-the-connector-installed-and-registered-correctly"></a>Yüklü ve kayıtlı doğru bağlayıcı doğrulayın

Azure portalı veya Windows server'ınıza yeni bir bağlayıcı düzgün yüklendiğini doğrulamak için kullanabilirsiniz.

### <a name="verify-the-installation-through-azure-portal"></a>Azure Portalı aracılığıyla yüklemesini doğrulama

Yüklü ve kayıtlı doğru bağlayıcıyı onaylamak için:

1. Kiracı dizininize oturum [Azure portalında](https://portal.azure.com).
2. Sol gezinti panelinde seçin **Azure Active Directory**ve ardından **uygulama proxy'si** altında **Yönet** bölümü. Tüm bağlayıcılar ve bağlayıcı gruplarını bu sayfada görüntülenir. 
3. Ayrıntılarını doğrulamak için bir bağlayıcı görüntüleyin. Bağlayıcılar, varsayılan olarak genişletilmiştir. Görüntülemek istediğiniz bağlayıcıyı genişletilmemişse, ayrıntılarını görüntülemek için bağlayıcıyı genişletin. Etkin bir yeşil etiket Bağlayıcınızı hizmetine bağlanabileceğini gösterir. Ancak, etiket yeşil olsa bile bir ağ sorunu yine de bağlayıcı iletileri almasını engelleyebilir. 

    ![AzureAD uygulama Proxy Bağlayıcısı](./media/application-proxy-connectors/app-proxy-connectors.png)

Bağlayıcı yükleme ile ilgili daha fazla yardım için bkz [uygulama ara sunucusu Bağlayıcısı'nı yüklerken sorunla](application-proxy-connector-installation-problem.md).

### <a name="verify-the-installation-through-your-windows-server"></a>Windows server'ınıza aracılığıyla yüklemesini doğrulama

Yüklü ve kayıtlı doğru bağlayıcıyı onaylamak için:

1. Windows Hizmetleri Yöneticisi'ni tıklatarak **Windows** anahtarı ve girerek *services.msc*.
2. Aşağıdaki iki hizmetin durumunu olup olmadığını görmek için onay **çalıştıran**.
   - **Microsoft AAD Application Proxy Connector** bağlantısını etkinleştirir.
   - **Microsoft AAD Application Proxy Connector Updater** bir otomatik güncelleştirme hizmetidir. Güncelleştirici, bağlayıcının yeni sürümlerini denetleyen ve bağlayıcıyı gereken şekilde güncelleştiren.

     ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/application-proxy-enable/app_proxy_services.png)

3. Hizmetlerin durumunu değilse **çalıştıran**, her hizmetin seçip sağ tıklayarak **Başlat**. 

## <a name="add-an-on-premises-app-to-azure-ad"></a>Şirket içi bir uygulamayı Azure AD'ye ekleme

Ortamınızı hazırladığınız ve yüklü bir bağlayıcı göre şirket içi uygulamaların Azure AD'ye eklemek hazırsınız.  

1. Yönetici olarak oturum açın [Azure portalında](https://portal.azure.com/).
2. Sol gezinti panelinde seçin **Azure Active Directory**.
3. Seçin **kurumsal uygulamalar**ve ardından **yeni uygulama**.
4. Seçin **şirket içi uygulama**.  
5. İçinde **kendi şirket içi uygulamanızı ekleme** bölümünde, uygulamanız ile ilgili aşağıdaki bilgileri sağlayın:

    | Alan | Açıklama |
    | :---- | :---------- |
    | **Ad** | Uygulamanın erişim panelinde hem de Azure portalında görünecek adı. |
    | **İç URL** | Uygulamaya özel ağınızın içinden erişmek için URL. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde, farklı uygulamalar ile aynı sunucuda farklı siteleri yayımlayabilir; her biri kendi adını ve erişim kuralları belirleyebilirsiniz.<br><br>Bir yol yayımlarsanız uygulamanıza ilişkin tüm gerekli görüntüleri, betikleri ve stil sayfalarını içerdiğinden emin olun. Örneğin, uygulamanız https ise:\//yourapp/uygulama ve kullandığı görüntüleri bulunan https:\//yourapp/medya ve ardından https yayımlama:\//yourapp/ yolu. Bu iç URL, kullanıcıların görmesi giriş sayfası olması gerekmez. Daha fazla bilgi için [yayımlanan uygulamalar için özel bir ana sayfa ayarlamak](application-proxy-configure-custom-home-page.md). |
    | **Dış URL** | Uygulamadan ağınızın dışından erişmek kullanıcıların adresi. Varsayılan uygulama ara sunucusu etki alanı kullanmayı istemiyorsanız okuyun [Azure AD uygulama proxy'sinde özel etki alanları](application-proxy-configure-custom-domain.md).|
    | **Ön kimlik doğrulaması** | Uygulama proxy'si nasıl erişim uygulamanıza vermeden önce kullanıcıları doğrular.<br><br>**Azure Active Directory** -uygulama proxy'si, kullanıcıların dizin ve uygulama izinlerine yönelik kimlik doğrulaması Azure AD'de oturum açmasına yönlendirir. Bu seçenek varsayılan olarak, koşullu erişim ve çok faktörlü kimlik doğrulaması gibi Azure AD güvenlik özelliklerinin avantajlarından yararlanabilmeniz tutulması önerilir. **Azure Active Directory** Microsoft bulut uygulama güvenliği ile bir uygulama izlemek için gereklidir.<br><br>**Geçiş** -kullanıcıların uygulamaya erişmek için Azure AD'de kimlik doğrulaması yapmak zorunda değilsiniz. Kimlik doğrulama gereksinimleri arka uçtaki yine de ayarlayabilirsiniz. |
    | **Bağlayıcı grubu** | Uygulamanız için uzaktan erişim bağlayıcılar işlemek ve bağlayıcı grupları bağlayıcılar ve bölgeyi, ağ veya amaçlı uygulamaların düzenlemenize yardımcı. Bağlayıcı gruplarda henüz sahip değilseniz, uygulamanızın atanan **varsayılan**.<br><br>Uygulamanız bağlanmak için WebSockets kullanıyorsa gruptaki tüm bağlayıcıları sürüm 1.5.612.0 olmalıdır veya üzeri.|

5. Gerekirse, yapılandırma **ek ayarlar**. Çoğu uygulama için bu ayarları varsayılan durumlarına tutmanız gerekir. 

    | Alan | Açıklama |
    | :---- | :---------- |
    | **Arka uç uygulama zaman aşımı** | Bu değer kümesine **uzun** uygulamanız kimlik doğrulaması ve bağlanmak yavaş ise. |
    | **Yalnızca HTTP tanımlama bilgisi kullan** | Bu değer kümesine **Evet** tanımlama bilgileri uygulama proxy'si için HTTP yanıt üst bilgisinde HTTPOnly bayrağını ekleyin. Uzak Masaüstü Hizmetleri'ni kullanarak ayarlarsanız bu değer **Hayır**.|
    | **Güvenli bir tanımlama bilgisi kullan**| Bu değer kümesine **Evet** şifrelenmiş bir HTTPS isteği gibi güvenli bir kanal üzerinden tanımlama bilgileri iletmek için.
    | **Kalıcı bir tanımlama bilgisi kullan**| Bu değeri tutun **Hayır**. Yalnızca tanımlama bilgilerini işlemler arasında paylaşamaz uygulamalar için bu ayarı kullanın. Tanımlama bilgisi ayarları hakkında daha fazla bilgi için bkz. [Azure Active Directory'de şirket içi uygulamalara erişmek için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).
    | **Üst bilgilerinde URL'leri Çevir** | Bu değer olarak tutmak **Evet** özgün ana bilgisayar üst bilgisi'kimlik doğrulama isteği, uygulamanızın gerektirdiği durumlar haricinde. |
    | **Uygulama gövdesi URL'leri Çevir** | Bu değer olarak tutmak **Hayır** sürece diğer şirket içi uygulamalara yönelik sabit kodlanmış HTML bağlantıları ve özel etki alanları kullanmayın. Daha fazla bilgi için [çeviri uygulama ara sunucusu ile bağlantı](application-proxy-configure-hard-coded-link-translation.md).<br><br>Bu değer kümesine **Evet** bu Microsoft Cloud App Security (MCAS) ile uygulama izlemeyi planlıyorsanız. Daha fazla bilgi için [erişim, Microsoft Cloud App Security ve Azure Active Directory ile gerçek zamanlı uygulama izlemeyi yapılandırmanız](application-proxy-integrate-with-microsoft-cloud-application-security.md). |
   
6. **Add (Ekle)** seçeneğini belirleyin.

## <a name="test-the-application"></a>Uygulamayı test etme

Test etmeye hazırsınız uygulaması doğru eklenir. Aşağıdaki adımlarda, uygulamaya bir kullanıcı hesabı eklemeniz ve oturum açmayı deneyin.

### <a name="add-a-user-for-testing"></a>Test etmek için kullanıcı ekleme

Bir kullanıcı uygulamaya eklemeden önce kullanıcı hesabı kurumsal ağ içinde uygulamaya erişmek için izinlere zaten sahip olun.

Bir test kullanıcısı eklemek için:

1. Seçin **kurumsal uygulamalar**ve ardından, test etmek istediğiniz uygulamayı seçin.
2. Seçin **Başlarken**ve ardından **test etmek için kullanıcı atama**.
3. Altında **kullanıcılar ve gruplar**seçin **Kullanıcı Ekle**.
4. Altında **ataması ekleme**seçin **kullanıcılar ve gruplar**. **Kullanıcı ve grupları** bölümü görünür. 
5. Eklemek istediğiniz hesabı seçin. 
6. Seçin **seçin**ve ardından **atama**.

### <a name="test-the-sign-on"></a>Oturum açma testi

Oturum açma için uygulamayı test etmek için:

1. Yayımlama adımında yapılandırdığınız dış URL'yi tarayıcınızda gidin. Başlangıç ekranını görmeniz gerekir.
2. Önceki bölümde oluşturduğunuz kullanıcı olarak oturum açın.

Sorun giderme için bkz: [uygulama proxy'si sorunlarını giderme sorunlarını ve hata iletileri](application-proxy-troubleshoot.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şirket içi ortamınızı uygulaması Ara sunucusu ile çalışacak şekilde hazırlanmış yüklü ve kayıtlı uygulama Proxy Bağlayıcısı. Ardından, Azure AD kiracınızla bir uygulamayı eklendi. Bir kullanıcının uygulamayı bir Azure AD hesabı kullanarak oturum açabilir, doğrulandı.

Şu işlemleri yaptınız:
> [!div class="checklist"]
> * Giden trafik ve belirli URL'lere izin verilen erişim için açılan bağlantı noktaları
> * Bağlayıcı Windows sunucunuzda yüklü ve uygulama ara sunucusu ile kayıtlı
> * Yüklü ve kayıtlı doğru bağlayıcı doğrulandı
> * Azure AD kiracınızla bir şirket içi uygulamaya eklenen
> * Bir test kullanıcısı bir Azure AD hesabını kullanarak uygulamada oturum açabilir doğrulandı

Uygulama için çoklu oturum açma yapılandırmaya hazırsınız. Tek bir oturum açma yöntemi seçin ve çoklu oturum açma öğreticiler bulmak için aşağıdaki bağlantıyı kullanın. 

> [!div class="nextstepaction"]
>[Çoklu oturum açmayı yapılandırma](what-is-single-sign-on.md#choosing-a-single-sign-on-method)
