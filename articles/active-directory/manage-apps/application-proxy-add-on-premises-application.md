---
title: Şirket içi uygulama - Azure Active Directory'de uygulama ara sunucusu ekleme | Microsoft Docs
description: Azure Active Directory (Azure AD), kullanıcıların kendi Azure AD hesabıyla oturum açarak şirket uygulamalarına erişmelerini sağlayan bir uygulama proxy'si hizmeti vardır. Bu öğretici, uygulama proxy'si ile kullanmak için ortamınızı hazırlama işlemini göstermektedir ve ardından Azure AD kiracınız için bir şirket içi uygulama eklemek için Azure portalını kullanır.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: tutorial
ms.date: 03/12/2019
ms.author: celested
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc454fdba6ec875c3d3b572a7aba91bb9d389845
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617233"
---
# <a name="tutorial-add-an-on-premises-application-for-remote-access-through-application-proxy-in-azure-active-directory"></a>Öğretici: Azure Active Directory Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme

Azure Active Directory (Azure AD), kullanıcıların kendi Azure AD hesabıyla oturum açarak şirket uygulamalarına erişmelerini sağlayan bir uygulama proxy'si hizmeti vardır. Bu öğretici, uygulama proxy'si ile kullanmak için ortamınızı hazırlar. Ortamınızı hazır hale geldikten sonra Azure AD kiracınız için bir şirket içi uygulama eklemek için Azure portalını kullanacaksınız.

Bu öğreticide:

> [!div class="checklist"]
> * Giden trafik için bağlantı noktalarını açar ve belirli URL'lere erişim sağlar.
> * Bağlayıcı, Windows sunucuya yükler ve uygulama proxy'si ile kaydeder.
> * Yüklü ve kayıtlı doğru bağlayıcı doğrular.
> * Azure AD kiracınız için bir şirket içi uygulama ekler.
> * Bir test kullanıcısı bir Azure AD hesabını kullanarak uygulamada oturum açabilir doğrular.

## <a name="before-you-begin"></a>Başlamadan önce

Kiracınıza uygulama eklemek için şunlara ihtiyacınız vardır:

* A [Microsoft Azure AD temel veya premium aboneliği](https://azure.microsoft.com/pricing/details/active-directory). 
* Uygulama yönetici hesabı.

### <a name="windows-server"></a>Windows server

Uygulama proxy'si kullanmak için Windows Server 2012 R2 çalıştıran bir Windows server gerekir veya üzeri. Sunucu üzerinde uygulama ara sunucusu bağlayıcısını yükleyeceksiniz. Uygulama Ara sunucusu hizmetlerini azure'da ve şirket içi uygulamaları yayımlamayı planlama bağlanmak bu bağlayıcı sunucusu gerekir.

Üretim ortamınızda, yüksek kullanılabilirlik için birden fazla Windows sunucusu olması önerilir. Bu öğreticide, bir Windows server yeterli olur.

#### <a name="recommendations-for-the-connector-server"></a>Bağlayıcı sunucusu için öneriler

1. Fiziksel olarak yakın uygulama sunucularını bağlayıcıyı ve uygulama arasında performansını iyileştirmek için bağlayıcı sunucusu bulun. Daha fazla bilgi için [ağ topolojisi hakkında önemli noktalar](application-proxy-network-topology.md).

2. Bağlayıcı sunucusu ve web uygulama sunucuları aynı Active Directory etki alanına ait veya gerekir güvenen etki alanlarına yayılan. Sunucuların aynı etki alanında olması veya güvenen etki alanlarına tümleşik Windows kimlik doğrulaması (IWA) ve Kerberos Kısıtlı temsilci (KCD) ile çoklu oturum açma (SSO) kullanarak bir gereksinimdir. Bağlayıcı sunucusu ve web uygulama sunucuları farklı Active Directory etki alanlarında ise, kaynak tabanlı temsilci seçme için çoklu oturum açmayı kullanmanız gerekir. Daha fazla bilgi için [uygulama proxy'si ile çoklu oturum açma için KCD](application-proxy-configure-single-sign-on-with-kcd.md).

#### <a name="software-requirements"></a>Yazılım gereksinimleri

Windows bağlayıcı sunucusu TLS 1.2 uygulama ara sunucusu bağlayıcısını yüklemeden önce etkin olmalıdır. Var olan Bağlayıcılarla 1.5.612.0 sürümleri yapılana kadar önceki TLS sürümlerini üzerinde çalışmaya devam eder. 

TLS 1.2 etkinleştirmek için:

1. Aşağıdaki kayıt defteri anahtarlarını ayarlayın:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. Sunucuyu yeniden başlatın

## <a name="prepare-your-on-premises-environment"></a>Şirket içi ortamınızı hazırlama

Azure AD uygulama proxy'si için ortamınızı hazırlama için ilk Azure veri merkezlerine iletişim etkinleştirmeniz gerekir. Yolda bir güvenlik duvarı varsa, bağlayıcı uygulama ara sunucusuna HTTPS (TCP) istekleri verebilmeniz için açık olduğundan emin olun.

### <a name="open-ports"></a>Bağlantı noktalarını aç

Aşağıdaki bağlantı noktalarının açık **giden** trafiği. 

   | Bağlantı noktası numarası | Nasıl kullanılır |
   | --- | --- |
   | 80 | İndirme sertifika iptal listelerini (CRL'ler SSL sertifikası doğrulanırken) |
   | 443 | Uygulama proxy'si hizmeti ile tüm giden iletişimi |

Ayrıca duvarınız kaynak kullanıcılar için trafiği zorunlu kılarsa ağ hizmeti olarak çalışan Windows hizmetlerinden 80 ve trafiği için 443 bağlantı noktalarını açın.

Uygulama Ara sunucusu kullanıyorsanız, yüklü connector'ın daha eski bir sürümü olabilir.  Bağlayıcı en son sürümünü yüklemek için bu öğreticiden yararlanın. Sürümleri 1.5.132.0 daha önce de aşağıdaki bağlantı noktalarını açma gerektirir: 5671, 8080, 9090-9091, 9350, 9352, 10100–10120. 

### <a name="allow-access-to-urls"></a>URL'lere erişim izni

Aşağıdaki URL'lere erişim izin ver:

| URL'si | Nasıl kullanılır |
| --- | --- |
| \*. msappproxy.net<br>\*. servicebus.windows.net | Bağlayıcı ve uygulama proxy'si bulut hizmeti arasında iletişim |
| mscrl.microsoft.com:80<br>CRL.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Azure sertifikaları doğrulamak için bu URL'leri kullanır. |
| login.windows.net<br>login.microsoftonline.com<br>secure.aadcdn.microsoftonline-p.com  | Bağlayıcı, bu URL'ler kayıt işlemi sırasında kullanır. |

Güvenlik Duvarı veya proxy DNS beyaz listeye ekleme izin veriyorsa, beyaz liste bağlantıları için \*. msappproxy.net ve \*. servicebus.windows.net. Erişime izin vermek, gerekirse [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). IP aralıklarını haftalık olarak güncelleştirilir.

## <a name="install-and-register-a-connector"></a>Yükleme ve bir bağlayıcıyı kaydetme

Uygulama proxy'si kullanmak için uygulama proxy'si hizmeti ile kullanmak için seçtiğiniz her Windows server üzerinde bir bağlayıcı yüklemeniz gerekir. Giden bağlantı şirket içi uygulama sunucularından Azure AD'de uygulama ara sunucusuna yöneten bir aracı Bağlayıcıdır. Bir bağlayıcı sunucuları gibi Azure AD Connect'in yüklü diğer kimlik doğrulama aracılarının de yükleyebilirsiniz.

Bağlayıcıyı yüklemek için:

1. Oturum [Azure portalında](https://portal.azure.com/) uygulama proxy'si kullanacağı dizinin bir uygulama yöneticisi olarak. Örneğin, Kiracı etki alanı contoso.com ise yönetici olmalıdır admin@contoso.com ya da bu etki alanındaki başka bir yönetici diğer.
2. Mevcut directory sağ üst köşedeki kullanıcı adınıza altında görünür. Uygulama Ara sunucusu kullanan dizin oturum açmadıysanız doğrulayın. Dizinleri değiştirmeniz gerekiyorsa, bu simgeyi seçin.
3. Sol dikey tıklayın **Azure Active Directory**, ardından **uygulama proxy'si**.
4. Tıklayın **bağlayıcı hizmeti indir**.
5. Hizmet koşullarını okuyun.  Hazır olduğunuzda, tıklayın **koşulları kabul et ve indir**.
6. Pencerenin en altında indirmek için bir istem görürsünüz **AADApplicationProxyConnectorInstaller.exe**. Tıklayın **çalıştırma** Bağlayıcısı'nı yüklemek için. Yükleme Sihirbazı açılır. 
7. Yüklemek için sihirbazdaki yönergeleri uygulayın. Bağlayıcıyı Azure AD kiracınız için uygulama proxy'si ile kaydetmek için istendiğinde, uygulama Yöneticisi kimlik bilgilerinizi sağlayın.
    - İçin Internet Explorer (IE), varsa **IE Artırılmış Güvenlik Yapılandırması** ayarlanır **üzerinde**, kayıt ekranı göremeyebilirsiniz. Erişim sağlamak için hata iletisindeki yönergeleri uygulayın. Internet Explorer Artırılmış Güvenlik ayarlandığından emin olun **kapalı**.

### <a name="general-remarks"></a>Genel açıklamalar

En son sürümünü almak için bir bağlayıcı daha önce yüklediyseniz, yeniden yükleyin. Daha önce yayımlanmış sürümleri ve bunlar değişiklikler hakkındaki bilgileri görmek için eklemek için bkz: [uygulama ara sunucusu - sürüm yayımlama geçmişi](application-proxy-release-version-history.md).

Şirket içi uygulamalarınız için birden fazla Windows server seçerseniz, yüklemeniz ve her sunucuya bağlayıcı kaydetmeniz gerekir. Bağlayıcılar, bağlayıcı gruplar halinde düzenleyebilirsiniz. Daha fazla bilgi için [bağlayıcı grupları](application-proxy-connector-groups.md). 

Kuruluşunuz internet'e bağlanmak için proxy sunucuları kullanıyorsa bunları için uygulama ara sunucusunu yapılandırmanız gerekir.  Daha fazla bilgi için [iş mevcut şirket içi proxy sunucuları](application-proxy-configure-connectors-with-proxy-servers.md). 

Bağlayıcılar, kapasite planlaması ve nasıl güncel kalın hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md). 


## <a name="verify-the-connector-installed-and-registered-correctly"></a>Yüklü ve kayıtlı doğru bağlayıcı doğrulayın

Azure portalı veya Windows server'ınıza yeni bir bağlayıcı düzgün yüklendiğini doğrulamak için kullanabilirsiniz.

### <a name="verify---azure-portal"></a>Doğrulama - Azure portalı

Yüklü ve kayıtlı doğru bağlayıcıyı onaylamak için:

1. Kiracı dizininize oturum [Azure portalında](https://portal.azure.com).
2. Tıklayın **Azure Active Directory** ardından **uygulama proxy'si**. Tüm bağlayıcılar ve bağlayıcı gruplarını bu sayfada görüntülenir. 
3. Ayrıntılarını doğrulamak için bir bağlayıcı seçin. Etkin bir yeşil etiket Bağlayıcınızı hizmetine bağlanabileceğini gösterir. Ancak, etiket yeşil olsa bile bir ağ sorunu yine de bağlayıcı iletileri almasını engelleyebilir. 

    ![AzureAD uygulama Proxy Bağlayıcısı](./media/application-proxy-connectors/app-proxy-connectors.png)

Bağlayıcı yükleme ile ilgili daha fazla yardım için bkz [bir uygulama ara sunucusu Bağlayıcısı yüklemesi sırasında sorunlarla](application-proxy-connector-installation-problem.md).

### <a name="verify---windows-server"></a>Doğrulama - Windows server

Yüklü ve kayıtlı doğru bağlayıcıyı onaylamak için:

1. Windows Hizmetleri Yöneticisi'ni tıklatarak **Windows** anahtarı ve girerek *services.msc*.
2. Aşağıdaki iki hizmetin durumunu olup olmadığını görmek için onay **çalıştıran**.
   - **Microsoft AAD Application Proxy Connector** bağlantıyı etkinleştirir
   - **Microsoft AAD Application Proxy Connector Updater** bir otomatik güncelleştirme hizmetidir. Güncelleştirici, bağlayıcının yeni sürümlerini denetleyen ve bağlayıcıyı gereken şekilde güncelleştiren.

     ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/application-proxy-enable/app_proxy_services.png)

3. Hizmetlerin durumunu değilse **çalıştıran**, her bir hizmete sağ tıklayın ve seçin **Başlat**. 

## <a name="add-an-on-premises-app-to-azure-ad"></a>Şirket içi bir uygulamayı Azure AD'ye ekleme

Ortamınızı hazırladığınız ve yüklü bir bağlayıcı göre şirket içi uygulamaların Azure AD'ye eklemek hazırsınız.  

1. Yönetici olarak oturum açın [Azure portalında](https://portal.azure.com/).
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **yeni uygulama**.

    ![Kurumsal uygulama ekleme](./media/application-proxy-publish-azure-portal/add-app.png)

3. Seçin **tüm**, ardından **şirket içi uygulama**.  

    ![Kendi uygulamanızı ekleyin](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. İçinde **kendi şirket içi uygulamanızı ekleme** dikey penceresinde, uygulama ile ilgili aşağıdaki bilgileri sağlayın:

    ![Şirket içi uygulamanızı yapılandırma](./media/application-proxy-add-on-premises-application/add-on-premises-app-with-application-proxy-updated.png)

    | Alan | Açıklama |
    | :---- | :---------- |
    | **Ad** | Uygulamanın erişim panelinde hem de Azure portalında görünecek adı. |
    | **İç URL** | Uygulamaya özel ağınızın içinden erişmek için URL. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde, farklı uygulamalar ile aynı sunucuda farklı siteleri yayımlayabilir; her biri kendi adını ve erişim kuralları belirleyebilirsiniz.<br><br>Bir yol yayımlarsanız uygulamanıza ilişkin tüm gerekli görüntüleri, betikleri ve stil sayfalarını içerdiğinden emin olun. Örneğin, uygulamanız https ise:\//yourapp/uygulama ve kullandığı görüntüleri bulunan https:\//yourapp/medya ve ardından https yayımlama:\//yourapp/ yolu. Bu iç URL, kullanıcıların görmesi giriş sayfası olması gerekmez. Daha fazla bilgi için [yayımlanan uygulamalar için özel bir ana sayfa ayarlamak](application-proxy-configure-custom-home-page.md). |
    | **Dış URL** | Uygulamadan ağınızın dışından erişmek kullanıcıların adresi. Varsayılan uygulama ara sunucusu etki alanı kullanmayı istemiyorsanız okuyun [Azure AD uygulama proxy'sinde özel etki alanları](application-proxy-configure-custom-domain.md).|
    | **Ön kimlik doğrulaması** | Uygulama proxy'si nasıl erişim uygulamanıza vermeden önce kullanıcıları doğrular.<br><br>**Azure Active Directory** -uygulama proxy'si, kullanıcıların dizin ve uygulama izinlerine yönelik kimlik doğrulaması Azure AD'de oturum açmasına yönlendirir. Azure AD koşullu erişim ve çok faktörlü kimlik doğrulaması gibi güvenlik özelliklerini yararlanabilir, böylece bu seçenek varsayılan olarak tutma öneririz. **Azure Active Directory** Microsoft bulut uygulama güvenliği ile bir uygulama izlemek için gereklidir.<br><br>**Geçiş** -kullanıcılar, Azure uygulamaya erişmek için Active Directory karşı kimlik doğrulaması yapmak zorunda değilsiniz. Kimlik doğrulama gereksinimleri arka uçtaki yine de ayarlayabilirsiniz. |
    | **Bağlayıcı grubu** | Uygulamanız için uzaktan erişim bağlayıcılar işlemek ve bağlayıcı grupları bağlayıcılar ve bölgeyi, ağ veya amaçlı uygulamaların düzenlemenize yardımcı. Bağlayıcı gruplarda henüz sahip değilseniz, uygulamanızın atanan **varsayılan**.<br><br>Uygulamanız bağlanmak için WebSockets kullanıyorsa gruptaki tüm bağlayıcıları sürüm 1.5.612.0 olmalıdır veya üzeri.|

5. Gerekirse, yapılandırma **ek ayarlar**. Çoğu uygulama için bu ayarları varsayılan durumlarına tutmanız gerekir. 

    | Alan | Açıklama |
    | :---- | :---------- |
    | **Arka uç uygulama zaman aşımı** | Bu değer kümesine **uzun** uygulamanız kimlik doğrulaması ve bağlanmak yavaş ise. |
    | **Yalnızca HTTP tanımlama bilgisi kullan** | Bu değer kümesine **Evet** tanımlama bilgileri uygulama proxy'si için HTTP yanıt üst bilgisinde HTTPOnly bayrağını ekleyin. Uzak Masaüstü Hizmetleri'ni kullanarak ayarlarsanız bu değer **Hayır**.|
    | **Güvenli bir tanımlama bilgisi kullan**| Bu değer kümesine **Evet** şifrelenmiş bir HTTPS isteği gibi güvenli bir kanal üzerinden tanımlama bilgileri iletmek için.
    | **Kalıcı bir tanımlama bilgisi kullan**| Bu değeri tutun **Hayır**. Bu ayar, yalnızca tanımlama bilgilerini işlemler arasında paylaşamaz uygulamalar için kullanılmalıdır. Tanımlama bilgisi ayarları hakkında daha fazla bilgi için bkz: [Azure Active Directory'de şirket içi uygulamalara erişmek için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings)
    | **Üst bilgilerinde URL'leri Çevir** | Bu değer olarak tutmak **Evet** özgün ana bilgisayar üst bilgisi'kimlik doğrulama isteği, uygulamanızın gerektirdiği durumlar haricinde. |
    | **Uygulama gövdesi URL'leri Çevir** | Bu değer olarak tutmak **Hayır** sürece diğer şirket içi uygulamalara yönelik sabit kodlanmış HTML bağlantıları ve özel etki alanları kullanmayın. Daha fazla bilgi için [çeviri uygulama ara sunucusu ile bağlantı](application-proxy-configure-hard-coded-link-translation.md).<br><br>Bu değer kümesine **Evet** bu Microsoft Cloud App Security (MCAS) ile uygulama izlemeyi planlıyorsanız. Daha fazla bilgi için [erişim, Microsoft Cloud App Security ve Azure Active Directory ile gerçek zamanlı uygulama izlemeyi yapılandırma](application-proxy-integrate-with-microsoft-cloud-application-security.md) |
   
6. **Add (Ekle)** seçeneğini belirleyin.

## <a name="test-the-application"></a>Uygulamayı test etme

Test etmeye hazırsınız uygulaması doğru eklenir. Aşağıdaki adımlarda, uygulamaya bir kullanıcı hesabı eklemeniz ve oturum açmayı deneyin.

### <a name="add-a-user-for-testing"></a>Test etmek için kullanıcı ekleme

Bir kullanıcı uygulamaya eklemeden önce kullanıcı hesabı kurumsal ağ içinde uygulamaya erişmek için izinlere zaten sahip olun.

Bir test kullanıcısı eklemek için:

1. Yeniden **Hızlı Başlangıç** dikey penceresinde **test etmek için kullanıcı atama**.

    ![Test etmek için kullanıcı atama](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde **Kullanıcı Ekle**.

    ![Bir kullanıcı veya Grup Ekle](./media/application-proxy-publish-azure-portal/add-user.png)

3. Üzerinde **ataması ekleme** dikey penceresinde **kullanıcılar ve gruplar**ve ardından eklemek istediğiniz hesabı seçin. 
4. **Ata**'yı seçin.

### <a name="test-the-sign-on"></a>Oturum açma testi

Oturum açma için uygulamayı test etmek için:

1. Yayımlama adımında yapılandırdığınız dış URL'yi tarayıcınızda gidin. 
2. Başlangıç ekranını görmeniz gerekir.
3. Önceki bölümde oluşturduğunuz kullanıcı olarak oturum açmayı deneyin.

    ![Yayımlanan uygulamanızı test edin](./media/application-proxy-publish-azure-portal/test-app.png)

Sorun giderme için bkz: [uygulama proxy'si sorunlarını giderme sorunlarını ve hata iletileri](application-proxy-troubleshoot.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şirket içi ortamınızı uygulaması Ara sunucusu ile çalışacak şekilde hazırlanmış yüklü ve kayıtlı uygulama Proxy Bağlayıcısı. Ardından, Azure AD kiracınızla bir uygulamayı eklendi. Bir kullanıcının uygulamayı bir Azure AD hesabı kullanarak oturum açabilir, doğrulandı.

Şu işlemleri yaptınız:
> [!div class="checklist"]
> * Giden trafik ve belirli URL'lere izin verilen erişim için açılan bağlantı noktaları
> * Bağlayıcı Windows sunucunuzda yüklü ve uygulama ara sunucusu ile kayıtlı
> * Yüklü ve kayıtlı doğru bağlayıcı doğrulandı
> * Azure AD kiracınızla bir şirket içi uygulamaya eklenen
> * Bir test kullanıcısı bir Azure AD hesabını kullanarak uygulamada oturum açabilir doğrulandı.

Uygulama için çoklu oturum açma yapılandırmaya hazırsınız. Tek bir oturum açma yöntemi seçin ve çoklu oturum açma öğreticiler bulmak için aşağıdaki bağlantıyı kullanın. 

> [!div class="nextstepaction"]
>[Çoklu oturum açmayı yapılandırma](what-is-single-sign-on.md#choosing-a-single-sign-on-method)
