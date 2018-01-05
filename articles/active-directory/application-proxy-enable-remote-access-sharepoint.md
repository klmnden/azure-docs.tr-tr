---
title: "SharePoint Azure AD uygulama proxy'si ile uzaktan erişimi etkinleştir | Microsoft Docs"
description: "Bir şirket içi SharePoint sunucusu Azure AD uygulama proxy'si ile tümleştirme hakkında temel bilgiler yer almaktadır."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c6a1b82b82dc89378533e375bd8a5d4868ae5308
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>SharePoint Azure AD uygulama proxy'si ile uzaktan erişimi etkinleştir

Bu makalede, Azure Active Directory (Azure AD) uygulama ara sunucusu ile bir şirket içi SharePoint sunucusu tümleştirme anlatılmaktadır.

SharePoint Azure AD uygulama proxy'si ile uzaktan erişimi etkinleştirmek için bu makaledeki adım adım bölümler izleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, ortamınızda SharePoint 2013 veya daha yeni zaten sahip olduğunuzu varsayar. Ayrıca, aşağıdaki önkoşulları göz önünde bulundurun:

* SharePoint yerel Kerberos desteği içerir. Bu nedenle, iç siteleri Azure AD uygulama proxy'si aracılığıyla uzaktan erişen kullanıcılar, çoklu oturum açma (SSO) deneyimi sağlamak için kabul edilebilir.

* Bu senaryo, SharePoint server yapılandırma değişiklikleri içerir. Hazırlama ortamında kullanmanızı öneririz. Bu şekilde, güncelleştirmelerinin hazırlama sunucunuza ilk olması ve üretime geçmeden önce bir test döngüsü kolaylaştırmak.

* Biz yayımlanan URL üzerinde SSL gerektirir. SSL gönderilen ve eşlenen bağlantılar doğru olduğundan emin olmak için iç sitenizde etkin olması gerekir. SSL yapılandırmadıysanız bkz [SharePoint 2013 için SSL yapılandırma](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) yönergeler için. Ayrıca, bağlayıcı makine dağıttığınız sertifika güvendiğinden emin olun. (Sertifika herkese açık şekilde verilmesi gerekmez.)

## <a name="step-1-set-up-single-sign-on-to-sharepoint"></a>1. adım: çoklu oturum açma SharePoint'e ayarlama

Windows kimlik doğrulaması kullanan şirket içi uygulamalar için çoklu oturum açma (SSO) Kerberos kimlik doğrulama protokolünü ve kısıtlı Kerberos temsilci (KCD) adlı bir özelliği ile elde edebilirsiniz. Kullanıcı Windows doğrudan oturum kurmadı olsa bile yapılandırıldığında, KCD, bir kullanıcı için bir Windows belirteç almak uygulama ara sunucusu Bağlayıcısı sağlar. KCD hakkında daha fazla bilgi için bkz: [Kerberos Kısıtlı temsilci genel bakış](https://technet.microsoft.com/library/jj553400.aspx).

Bir SharePoint sunucusu için KCD ayarlamak için aşağıdaki sıralı bölümlerdeki yordamları kullanın:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>SharePoint hizmet hesabı altında çalıştığından emin olun

İlk olarak, SharePoint bir tanımlanmış hizmet hesabı altında--değil yerel sistem, yerel hizmet veya ağ hizmeti çalışır durumda olduğundan emin olun. Geçerli bir hesap için hizmet asıl adları (SPN) iliştirebilirsiniz böylece bunu yapabilirsiniz. SPN'ler Kerberos protokolü farklı hizmetleri nasıl tanımlar ' dir. Ve daha sonra KCD yapılandırmak için hesabı gerekir.

> [!NOTE]
Daha önce oluşturulmuş olmasına gerek hizmeti için Azure AD hesabı. Bir otomatik parola değişikliğini izin öneririz. Adımları ve sorunlarını giderme tamamını hakkında daha fazla bilgi için bkz: [SharePoint 2013'te Otomatik parola değişikliği yapılandırma](https://technet.microsoft.com/library/ff724280.aspx).

Sitelerinizi tanımlanmış hizmet hesabı altında çalıştığından emin olmak için aşağıdaki adımları gerçekleştirin:

1. Açık **SharePoint 2013 Yönetim Merkezi** site.
2. Git **güvenlik** seçip **hizmet hesaplarını yapılandır**.
3. Seçin **Web uygulama havuzu - SharePoint - 80**. Seçenekler, web havuzu adına göre biraz farklı olabilir veya web havuzu varsayılan olarak SSL kullanıyorsa.

  ![Bir hizmet hesabı yapılandırma seçenekleri](./media/application-proxy-remote-sharepoint/service-web-application.png)

4. Varsa **bu bileşen için bir hesap seçin** alan ayarlanmış **yerel hizmet** veya **ağ hizmeti**, bir hesap oluşturmanız gerekir. Değilse, tamamlanmış ve sonraki bölüme geçebilirsiniz.
5. Seçin **kaydı yeni yönetilen hesabı**. Hesabınızı oluşturduktan sonra ayarlamalısınız **Web uygulama havuzu** hesabını kullanabilmeniz için.

### <a name="configure-sharepoint-for-kerberos"></a>Kerberos için SharePoint Yapılandırma

KCD, çoklu oturum açma SharePoint sunucusuna gerçekleştirmek için kullanın.

SharePoint siteniz için Kerberos kimlik doğrulamasını yapılandırmak için:

1. Açık **SharePoint 2013 Yönetim Merkezi** site.
2. Git **Uygulama Yönetimi**seçin **web uygulamalarını yönetme**, SharePoint sitenizi seçin. Bu örnek, **SharePoint - 80**.

  ![SharePoint sitesi seçme](./media/application-proxy-remote-sharepoint/manage-web-applications.png)

3. Tıklatın **kimlik doğrulama sağlayıcıları** araç çubuğunda.
4. İçinde **kimlik doğrulama sağlayıcıları** kutusunda, **varsayılan bölge** ayarlarını görüntülemek için.
5. İçinde **kimlik doğrulamasını Düzenle** iletişim kutusunda, görene kadar aşağı kaydırın **talep kimlik doğrulama türleri**. Emin her ikisi de **Windows kimlik doğrulamasını etkinleştir** ve **tümleşik Windows kimlik doğrulaması** seçilir.
6. Tümleşik Windows kimlik doğrulaması alan için açılan kutuya olduğundan emin olun **Negotiate (Kerberos)** seçilir.

  ![Kimlik doğrulama iletişim kutusunu Düzenle](./media/application-proxy-remote-sharepoint/service-edit-authentication.png)

7. Ekranın alt kısmındaki **kimlik doğrulamasını Düzenle** iletişim kutusu, tıklatın **kaydetmek**.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>SharePoint hizmet hesabı hizmet asıl adı ayarlama

KCD yapılandırmadan önce yapılandırdığınız hizmet hesabı olarak çalışan SharePoint hizmeti tanımlamak gerekir. Hizmet, bir SPN ayarlayarak tanımlayın. Daha fazla bilgi için bkz: [hizmet asıl adları](https://technet.microsoft.com/library/cc961723.aspx).

SPN biçimi şöyledir:

```
<service class>/<host>:<port>
```

SPN biçimi:

* _hizmet sınıfı_ hizmeti için benzersiz bir addır. SharePoint için kullandığınız **HTTP**.

* _ana bilgisayar_ tam etki alanı ya da hizmetini çalıştıran ana bilgisayarın NetBIOS adı. Bir SharePoint sitesi için bu metni kullandığınız IIS sürümüne bağlı olarak site URL'sini olması gerekebilir.

* _bağlantı noktası_ isteğe bağlıdır.

SharePoint sunucusu FQDN'si ise:

```
sharepoint.demo.o365identity.us
```

SPN sonra:

```
HTTP/sharepoint.demo.o365identity.us demo
```

SPN'ler belirli siteleri için sunucunuzda ayarlamak gerekebilir. Daha fazla bilgi için bkz: [yapılandırma Kerberos kimlik doğrulaması](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Kapat "Kerberos kimlik doğrulaması kullanan Web uygulamalarınız için hizmet asıl adları oluşturma" bölümünde dikkat edin.

SPN'ler ayarlamanızı en kolay yolu zaten siteleriniz için bulunabilecek SPN biçimleri izlemektir. Karşı hizmet hesabını kaydetmek için bu SPN kopyalayın. Bunu yapmak için:

1. SPN sitesiyle başka bir makineden göz atın.
 Bunu yaptığınızda, Kerberos biletleri ilgili kümesini makinede önbelleğe alınır. Bu biletleri için taranan hedef site SPN'sini içerir.

2. Adlı bir araç kullanarak bu site için SPN çekebilir [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html). Siteyi tarayıcıda erişen kullanıcının aynı bağlamda çalışan bir komut penceresinde aşağıdaki komutu çalıştırın:
```
Klist
```
Klist sonra hedef SPN kümesini döndürür. Bu örnekte vurgulanmış gerekli olan SPN değeridir:

  ![Örnek Klist sonuçları](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)

4. SPN sahip olduğunuza göre doğru bir şekilde daha önce web uygulaması için ayarladığınız hizmet hesabının yapılandırıldığından emin emin olun. Komut isteminden aşağıdaki komutu etki alanının yönetici olarak çalıştırın:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Bu komut olarak çalışan SharePoint hizmet hesabı SPN'yi ayarlar _demo\sp_svc_.

 Değiştir _http/sharepoint.demo.o365identity.us_ sunucunuz için SPN ile ve _demo\sp_svc_ , ortamınızdaki hizmet hesabıyla. Bunu eklemeden önce SPN'yi Setspn komut arar. Bu durumda, görebileceğiniz bir **yinelenen SPN değeri** hata. Bu hatayı görürseniz, değer hizmeti hesabı ile ilişkili olduğundan emin olun.

SPN -m seçeneğiyle Setspn komutunu çalıştırarak eklendiğini doğrulayabilirsiniz. Bu komut hakkında daha fazla bilgi için bkz: [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-set-as-a-trusted-delegate-to-sharepoint"></a>Bağlayıcı SharePoint'e güvenilen temsilci olarak ayarlandığından emin olun

KCD, Azure AD uygulama proxy'si hizmeti kullanıcı kimlikleri SharePoint hizmetine devredebilirsiniz şekilde yapılandırın. KCD, kullanıcılarınızın Azure AD içinde kimlik doğrulaması için Kerberos biletleri almak uygulama ara sunucusu Bağlayıcısı etkinleştirerek yapılandırın. Daha sonra sunucu bağlamı hedef uygulama ya da SharePoint için bu durumda geçirir.

KCD yapılandırmak için her bağlayıcı makine için aşağıdaki adımları yineleyin:

1. Bir etki alanı denetleyicisinin etki alanı yöneticisi olarak oturum açın ve ardından açın **Active Directory Kullanıcıları ve Bilgisayarları**.
2. Bağlayıcı üzerinde çalıştığı bilgisayar bulunamadı. Bu örnekte, aynı SharePoint sunucusudur.
3. Bilgisayarı çift tıklatın ve ardından **temsilci** sekmesi.
4. Temsilci ayarları ayarlandığından emin olun **bu bilgisayara yalnızca belirtilen hizmetlere temsilci seçmek için güven**. Ardından, seçin **herhangi bir kimlik doğrulama protokolünü kullan**.

  ![Temsilci ayarları](./media/application-proxy-remote-sharepoint/delegation-box.png)

5. Tıklatın **Ekle** düğmesini tıklatın, **kullanıcılar veya bilgisayarlar**ve hizmet hesabını bulun.

  ![Hizmet hesabı için SPN ekleme](./media/application-proxy-remote-sharepoint/users-computers.png)

6. SPN'ler listesinde, hizmet hesabı için daha önce oluşturduğunuz bir tanesini seçin.
7. **Tamam**’a tıklayın. Tıklatın **Tamam** yeniden değişiklikleri kaydedin.

## <a name="step-2-enable-remote-access-to-sharepoint"></a>2. adım: SharePoint için uzaktan erişimi etkinleştir

Kerberos ve yapılandırılmış KCD için SharePoint etkinleştirdikten, SharePoint grubu Azure AD uygulama proxy'si aracılığıyla uzaktan erişim için yayımlamaya hazırsınız.

1. Aşağıdaki ayarlarla, SharePoint sitenizi yayımlayın. Adım adım yönergeler için bkz: [Azure AD uygulama proxy'si kullanarak uygulamalar yayımlamayı](application-proxy-publish-azure-portal.md). 
   - **İç URL**: SharePoint sitesinin URL'sini dahili olarak gibi **https://SharePoint/**. Bu örnekte, kullandığınızdan emin olun **https**
   - **Ön kimlik doğrulama yöntemi**: Azure Active Directory
   - **Üstbilgiler URL'de çevir**: Hayır

   >[!TIP]
   >SharePoint kullanan _ana bilgisayar üstbilgisi_ siteyi aramak için değer. Aynı zamanda bu değere göre bağlantılar da oluşturur. SharePoint oluşturur herhangi bir bağlantıyı dış URL'sini kullanmak üzere doğru şekilde ayarlanması yayımlanan bir URL olduğunu net etkisidir. Ayar değeri **Evet** Ayrıca arka uç uygulama isteği iletmek bağlayıcı sağlar. Ancak, ayar değeri **Hayır** bağlayıcı iç ana bilgisayar adını göndermez anlamına gelir. Bunun yerine, ana bilgisayar üstbilgisi bağlayıcı arka uç uygulaması için yayımlanan URL olarak gönderir.

   ![SharePoint uygulama yayımlama](./media/application-proxy-remote-sharepoint/publish-app.png)

2. Uygulamanızı yayımlandığında, çoklu oturum açma ayarları aşağıdaki adımlarla yapılandırın:

   1. Portalı'nda uygulama sayfasında seçin **çoklu oturum açma**.
   2. Tek modu'nın oturum açma seçin **tümleşik Windows kimlik doğrulaması**.
   3. İç uygulama SPN daha önce belirlediğiniz değerine ayarlayın. Bu örnek için olacaktır **http/sharepoint.demo.o365identity.us**.

   ![SSO için tümleşik Windows kimlik doğrulamasını yapılandırma](./media/application-proxy-remote-sharepoint/configure-iwa.png)

3. Uygulamanızı kurulumunu tamamlamak için şu adrese gidin **kullanıcılar ve gruplar** bölümünde ve bu uygulamaya erişmek için kullanıcıları atayın. 

## <a name="step-3-ensure-that-sharepoint-knows-about-the-external-url"></a>Adım 3: SharePoint dış URL hakkında bilir emin olun.

Son adım, böylece bu dış URL temel alınarak bağlantılar işler SharePoint dış URL temel alınarak site bulabilirsiniz sağlamaktır. Bunun için SharePoint sitesi için diğer erişim eşleşmeleri yapılandırarak.

1. Açık **SharePoint 2013 Yönetim Merkezi** site.
2. Altında **sistem ayarlarını**seçin **yapılandırma diğer erişim eşleşmeleri**. Diğer erişim eşleşmeleri kutusu açılır.

  ![Diğer erişim eşleşmeleri kutusu](./media/application-proxy-remote-sharepoint/alternate-access1.png)

3. Aşağı açılan listesinde **alternatif erişim eşleme koleksiyonu**seçin **değişiklik alternatif erişim eşleme koleksiyonu**.
4. Örneğin, siteniz--seçin **SharePoint - 80**.
5. Bir iç URL veya genel bir URL olarak yayımlanan URL'ye eklemeyi seçebilirsiniz. Bu örnek genel bir URL extranet kullanır.
6. ' I tıklatın **Düzenle ortak URL'leri** içinde **Extranet** , yol ve uygulama yayımlandığında, oluşturulan dış URL'yi girin. Örneğin **https://sharepoint-iddemo.msappproxy.net**.

  ![Yolun girme](./media/application-proxy-remote-sharepoint/alternate-access3.png)

7. **Kaydet**’e tıklayın.

Artık Azure AD uygulama proxy'si aracılığıyla harici olarak SharePoint sitesine erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD uygulama proxy'si özel etki alanları ile çalışma](active-directory-application-proxy-custom-domains.md)
- [Azure AD uygulama proxy'si bağlayıcılar anlama](application-proxy-understand-connectors.md)

