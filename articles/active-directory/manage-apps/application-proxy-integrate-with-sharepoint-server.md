---
title: Azure AD uygulama ara sunucusu ile SharePoint için uzaktan erişimi etkinleştir | Microsoft Docs
description: Şirket içi SharePoint server, Azure AD uygulama proxy'si ile tümleştirme hakkında temel kavramları kapsar.
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
ms.date: 10/19/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 89852e90daa548dc82455cb6317d367b7423ba65
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425217"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile SharePoint uzaktan erişimi etkinleştirme

Bu makalede, şirket içi SharePoint server, Azure Active Directory (Azure AD) uygulama proxy'si ile tümleştirme nasıl ele alınmaktadır.

Azure AD uygulama ara sunucusu ile SharePoint için uzaktan erişimi etkinleştirmek için adım adım bu makaledeki bölümler izleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, ortamınızda SharePoint 2013 veya daha yeni zaten sahibi olduğunuzu varsayar. Ayrıca, aşağıdaki önkoşulları göz önünde bulundurun:

* SharePoint yerel Kerberos desteği içerir. Bu nedenle, iç sitelere Azure AD uygulama proxy'si aracılığıyla uzaktan erişen kullanıcılar, bir çoklu oturum açma (SSO) deneyimi sağlamak için kabul edilebilir.

* Bu senaryo, SharePoint Server'ınıza yapılandırma değişiklikleri içerir. Bir hazırlık ortamı kullanmanızı öneririz. Bu şekilde güncelleştirmeleri hazırlama sunucunuza ilk olun ve sonra üretime geçmeden önce bir test döngüsünü kolaylaştırmak.

* Yayımlanmış URL'sini SSL zorunlu kılarız. SSL iç sitenizde gönderilen/eşlenen bağlantıları doğru olduğundan emin olmak için etkin olması gerekir. SSL yapılandırmadıysanız, bkz. [SharePoint 2013 için SSL yapılandırma](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) yönergeler için. Ayrıca, bağlayıcı makinesinde dağıttığınız sertifikaya güvendiğinden emin olun. (Sertifika genel olarak verilmiş olması gerekmez.)

## <a name="step-1-set-up-single-sign-on-to-sharepoint"></a>1. adım: SharePoint için çoklu oturum açma ayarlama

Windows kimlik doğrulaması kullanan şirket içi uygulamalar için çoklu oturum açma (SSO) ile Kerberos kimlik doğrulama protokolünü ve Kerberos Kısıtlı temsilci (KCD) adlı bir özellik elde edebilirsiniz. Kullanıcı için Windows doğrudan oturum henüz bile yapılandırıldığında, KCD, bir kullanıcı için bir Windows belirteç almak uygulama Proxy Bağlayıcısı sağlar. KCD hakkında daha fazla bilgi için bkz: [Kerberos Kısıtlı temsilci seçmeye genel bakış](https://technet.microsoft.com/library/jj553400.aspx).

KCD ' için bir SharePoint sunucusu ayarlamak için aşağıdaki sıralı bölümlerdeki yordamları kullanın:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>SharePoint hizmet hesabı altında çalıştığından emin olun

İlk olarak, SharePoint bir tanımladığınız hizmet hesabı altında--değil yerel sistem, yerel hizmet veya ağ hizmeti olarak çalıştığından emin olun. Hizmet asıl adları (SPN'ler) için geçerli bir hesap eklemek için bunu yapabilirsiniz. SPN'ler Kerberos protokolü farklı hizmetleri nasıl tanımlar var. Ve daha sonra KCD yapılandırmak için hesabı gerekir.

> [!NOTE]
Önceden oluşturulmuş olması gerekir hizmeti için Azure AD hesabı. Bir otomatik parola değişikliği için izin öneririz. Adımları ve sorun giderme konuları tamamı hakkında daha fazla bilgi için bkz. [otomatik parola değiştirme SharePoint 2013'te yapılandırma](https://technet.microsoft.com/library/ff724280.aspx).

Sitelerinizi bir tanımlı bir hizmet hesabı altında çalıştığından emin olun için aşağıdaki adımları gerçekleştirin:

1. Açık **SharePoint 2013 Yönetim Merkezi** site.
2. Git **güvenlik** seçip **hizmet hesaplarını yapılandırın**.
3. Seçin **Web uygulama havuzu - SharePoint - 80**. Seçenekler, web havuzu adına göre biraz farklı olabilir veya web havuzu varsayılan olarak SSL kullanır.

  ![Bir hizmet hesabı yapılandırma seçenekleri](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

4. Varsa **bu bileşen için bir hesap seçersiniz** ayarlanmış **yerel hizmet** veya **ağ hizmeti**, bir hesap oluşturmanız gerekir. Değilse, tamamlanmış ve sonraki bölüme geçebilirsiniz.
5. Seçin **kaydı yeni yönetilen hesabı**. Hesabınızı oluşturduktan sonra ayarlamalısınız **Web uygulama havuzu** hesabı kullanabilmeniz için önce.

### <a name="configure-sharepoint-for-kerberos"></a>SharePoint için Kerberos'u yapılandırma

KCD, SharePoint server için çoklu oturum açma gerçekleştirmek için kullanın.

SharePoint siteniz için Kerberos kimlik doğrulamasını yapılandırmak için:

1. Açık **SharePoint 2013 Yönetim Merkezi** site.
2. Git **Uygulama Yönetimi**seçin **web uygulamalarını yönetme**ve SharePoint sitenizi seçin. Bu örnekte olduğu **SharePoint - 80**.

  ![SharePoint sitesi seçme](./media/application-proxy-integrate-with-sharepoint-server/manage-web-applications.png)

3. Tıklayın **kimlik doğrulama sağlayıcıları** araç.
4. İçinde **kimlik doğrulama sağlayıcıları** kutusunun **varsayılan bölge** ayarlarını görüntülemek için.
5. İçinde **kimlik doğrulamasını Düzenle** iletişim kutusunda, görene kadar aşağı kaydırın **talep kimliği doğrulama türleri**. Emin her ikisi de **Windows kimlik doğrulamasını etkinleştir** ve **tümleşik Windows kimlik doğrulaması** seçilir.
6. Tümleşik Windows kimlik doğrulaması alanı için aşağı açılan kutusunda emin **anlaş (Kerberos)** seçilir.

  ![Kimlik doğrulaması iletişim kutusunu Düzenle](./media/application-proxy-integrate-with-sharepoint-server/service-edit-authentication.png)

7. Sayfanın alt kısmında **kimlik doğrulamasını Düzenle** iletişim kutusu, tıklayın **Kaydet**.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>SharePoint hizmet hesabı için bir hizmet asıl adı ayarlayın

KCD yapılandırmadan önce yapılandırdığınız hizmet hesabı olarak çalışan SharePoint hizmeti tanımlamak gerekir. Hizmet, bir SPN ayarlayarak belirleyin. Daha fazla bilgi için [hizmet asıl adları](https://technet.microsoft.com/library/cc961723.aspx).

SPN biçimi şu şekildedir:

```
<service class>/<host>:<port>
```

SPN biçimi:

* _hizmet sınıfı_ hizmeti için benzersiz bir addır. SharePoint için kullandığınız **HTTP**.

* _konak_ tam etki alanı veya hizmetin üzerinde çalıştığı konak NetBIOS adı. Bir SharePoint sitesi için bu metin, kullanmakta olduğunuz IIS sürümüne bağlı olarak sitenin URL'sini olması gerekebilir.

* _bağlantı noktası_ isteğe bağlıdır.

SharePoint sunucusunun FQDN'sini ise:

```
sharepoint.demo.o365identity.us
```

SPN sonra:

```
HTTP/sharepoint.demo.o365identity.us demo
```

SPN'ler için belirli siteleri sunucunuzda ayarlamak gerekebilir. Daha fazla bilgi için [yapılandırma Kerberos kimlik doğrulaması](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Kapat "Kerberos kimlik doğrulaması kullanan Web uygulamalarınız için hizmet asıl adları oluşturma" bölümüne dikkat edin.

SPN'ler ayarlamanız için en kolay yolu, zaten sitelerinizin bulunabilecek SPN biçimleri takip etmektir. Karşı hizmet hesabını kaydetmek için bu SPN kopyalayın. Bunu yapmak için:

1. SPN ile site başka bir makineden göz atın.
 Bunu yaptığınızda, Kerberos biletleri uygun kümesi makinede önbelleğe alınır. Bu anahtarları için taranan hedef siteye SPN'sini içerir.

2. Adında bir araç kullanarak bu site için SPN çekebilirsiniz [Klist](https://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html). Tarayıcıda siteye erişmek için kullanıcı olarak aynı bağlamda çalışan bir komut penceresinde aşağıdaki komutu çalıştırın:
```
Klist
```
Klist ardından hedef SPN'ler kümesini döndürür. Bu örnekte, vurgulanan gereken SPN değerdir:

  ![Örnek Klist sonuçları](./media/application-proxy-integrate-with-sharepoint-server/remote-sharepoint-target-service.png)

4. SPN olduğuna göre onu doğru daha önce web uygulaması için ayarladığınız hizmet hesabının yapılandırıldığından emin olun. Aşağıdaki komut komut isteminden etki alanının yönetici olarak çalıştırın:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Bu komut olarak çalışan SharePoint hizmet hesabı için SPN'nin ayarlar _demo\sp_svc_.

 Değiştirin _http/sharepoint.demo.o365identity.us_ sunucunuz için bir SPN ile ve _demo\sp_svc_ ortamınızdaki hizmet hesabı ile. Bunu eklemeden önce SPN'yi Setspn komut arar. Bu durumda, görebileceğiniz bir **SPN değeri yinelenen** hata. Bu hatayı görürseniz, değer hizmet hesabı ile ilişkili olduğundan emin olun.

SPN'yi Setspn komut -l seçeneği ile çalıştırarak eklendiğini doğrulayabilirsiniz. Bu komut hakkında daha fazla bilgi için bkz. [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-set-as-a-trusted-delegate-to-sharepoint"></a>Bağlayıcı için SharePoint güvenilen temsilci olarak ayarlandığından emin olun

Azure AD uygulama proxy'si hizmeti kullanıcı kimliklerini SharePoint hizmetine devredebilirsiniz KCD yapılandırın. KCD, kullanıcılarınız Azure AD'de kimlik doğrulaması için Kerberos anahtarlarını almak uygulama Proxy Bağlayıcısı'nı etkinleştirerek yapılandırın. Daha sonra o sunucu bağlamı hedef uygulama ya da SharePoint için bu durumda geçirir.

KCD yapılandırmak için her bir bağlayıcı makine için aşağıdaki adımları yineleyin:

1. Bir DC için bir etki alanı yöneticisi olarak oturum açın ve ardından açın **Active Directory Kullanıcıları ve Bilgisayarları**.
2. Bağlayıcıyı çalıştıran bilgisayarı bulmak. Bu örnekte, aynı SharePoint sunucusudur.
3. Bilgisayarı çift tıklatın ve ardından **temsilci** sekmesi.
4. Temsilci seçme ayarları ayarlandığından emin olun **bu bilgisayara yalnızca belirtilen hizmetlere temsilci seçmek için güven**. Ardından, **herhangi bir kimlik doğrulama protokolünü kullan**.

  ![Temsilci seçme ayarları](./media/application-proxy-integrate-with-sharepoint-server/delegation-box.png)

5. Tıklayın **Ekle** düğmesini tıklatın, **kullanıcılar veya bilgisayarlar**, hizmet hesabını bulun.

  ![Hizmet hesabının SPN'sini ekleme](./media/application-proxy-integrate-with-sharepoint-server/users-computers.png)

6. SPN'ler listesinde, daha önce oluşturduğunuz hizmet hesabı için bir tane seçin.
7. **Tamam** düğmesine tıklayın. Tıklayın **Tamam** yeniden değişiklikleri kaydedin.

## <a name="step-2-enable-remote-access-to-sharepoint"></a>2. adım: SharePoint için uzaktan erişimi etkinleştir

Kerberos ve yapılandırılmış KCD için SharePoint etkinleştirdikten sonra SharePoint grubu Azure AD uygulama proxy'si aracılığıyla uzaktan erişim için yayımlamaya hazır.

1. Aşağıdaki ayarlar ile SharePoint sitenizi yayımlayın. Adım adım yönergeler için bkz: [Azure AD uygulama proxy'si kullanarak uygulamaları yayımlama](application-proxy-publish-azure-portal.md). 
   - **İç URL**: SharePoint sitesinin URL'sini dahili olarak gibi **https://SharePoint/**. Bu örnekte, kullandığınızdan emin olun **https**
   - **Ön kimlik doğrulama yöntemi**: Azure Active Directory
   - **Bilgilerde URL'yi çevir**: Hayır

   >[!TIP]
   >SharePoint kullanan _ana bilgisayar üst bilgisini_ siteyi aramak için değer. Ayrıca bu değere göre bağlantılar oluşturur. Net etkisiyle SharePoint oluşturan herhangi bir bağlantıyı dış URL'sini kullanmak üzere doğru şekilde yayımlanmış bir URL olmasıdır. Değerini **Evet** Ayrıca arka uç uygulaması isteği iletmek bağlayıcıyı etkinleştirir. Ancak, ayar değeri **Hayır** bağlayıcı iç ana bilgisayar adını göndermez anlamına gelir. Bunun yerine, bağlayıcıyı arka uç uygulaması için yayımlanan URL olarak ana bilgisayar üst bilgisi gönderir.

   ![SharePoint uygulaması olarak Yayımla](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

2. Uygulamanızı yayımladıktan sonra aşağıdaki adımlarla çoklu oturum açma ayarları yapılandırın:

   1. Uygulama sayfasında portalında seçin **çoklu oturum açma**.
   2. Çoklu oturum açma modu için seçin **tümleşik Windows kimlik doğrulaması**.
   3. İç uygulama SPN'si daha önce ayarlanan değere ayarlayın. Bu örnekte, olacak **http/sharepoint.demo.o365identity.us**.

   ![SSO için tümleşik Windows kimlik doğrulamasını yapılandırma](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

3. Uygulamanızı ayarlama işlemini sonlandırmak için şuraya gidin: **kullanıcılar ve gruplar** bölümünde ve bu uygulamaya erişmek için kullanıcı atama. 

## <a name="step-3-ensure-that-sharepoint-knows-about-the-external-url"></a>3. adım: SharePoint dış URL hakkında bilmesi emin olun.

Son adımınız, böylece söz konusu dış URL'yi tabanlı bağlantılar oluşturur SharePoint dış URL temel alınarak site bulabilirsiniz sağlamaktır. SharePoint sitesi için alternatif erişim eşlemelerini yapılandırarak bunu yapabilirsiniz.

1. Açık **SharePoint 2013 Yönetim Merkezi** site.
2. Altında **sistem ayarlarını**seçin **alternatif erişim eşlemelerini yapılandırma**. Alternatif erişim eşlemeleri kutusu açılır.

  ![Alternatif erişim eşlemelerini kutusu](./media/application-proxy-integrate-with-sharepoint-server/alternate-access1.png)

3. Aşağı açılan listesinde **alternatif erişim eşleme koleksiyonu**seçin **değişiklik alternatif erişim eşleme koleksiyonu**.
4. Örneğin, siteniz--seçin **SharePoint - 80**.
5. Bir iç URL ya da genel bir URL olarak yayımlanmış URL'sini eklemek seçebilirsiniz. Bu örnek, extranet genel bir URL kullanır. Özel bağlantı noktası oluşturma, özel bir bağlantı noktası URL'ye eklediğinizden emin kullanıyorsanız.
6. Tıklayın **Düzenle ortak URL'leri** içinde **Extranet** yolu ve ardından uygulamayı yayımladığınızda, oluşturulan dış URL'yi girin. Örneğin, **https://sharepoint-iddemo.msappproxy.net**.

  ![Yolun girme](./media/application-proxy-integrate-with-sharepoint-server/alternate-access3.png)

7. **Kaydet**’e tıklayın.

Artık Azure AD uygulama proxy'si aracılığıyla harici olarak SharePoint sitesine erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD uygulama proxy'sinde özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md)
- [Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)

