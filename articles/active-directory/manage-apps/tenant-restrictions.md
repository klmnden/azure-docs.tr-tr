---
title: Kiracılar - Azure kısıtlayarak bulut uygulamalarına erişimini yönetme | Microsoft Docs
description: Kiracı kısıtlamaları hangi kullanıcıları yönetmek için nasıl kullanılacağını kendi Azure AD Kiracı göre uygulamalara erişebilir.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: celested
ms.reviewer: richagi
ms.openlocfilehash: 6cb917b2c213321e4ea8088993ca77ab7c712e6f
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961321"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>Bulut uygulamalarınızı SaaS erişimi yönetmek için kullanım Kiracı kısıtlamaları

Güvenlik vurgulamak büyük kuruluşlar, bulut Hizmetleri Office 365 gibi ancak kullanıcılar yalnızca onaylanan kaynaklara erişebilir bilmeniz gereken taşımak istiyorum. Geleneksel olarak, erişimi yönetmek istediklerinde şirket etki alanı adlarını veya IP adreslerini kısıtlayın. Bu yaklaşım, SaaS uygulamalarında paylaşılan etki alanı adları outlook.office.com ve login.microsoftonline.com gibi çalışan bir genel bulutta barındırıldığı bir dünyada başarısız olur. Bu adresler engelleme, kullanıcıların web üzerinde Outlook'u tamamen yalnızca bunları onaylı kimlikleri ve kaynaklarına erişimi kısıtlama yerine erişmesini tutacak.

Azure Active Directory'nin bu çözüme Kiracı kısıtlamaları adlı bir özelliktir. Kiracı kısıtlamaları, kuruluşların uygulamalar için çoklu oturum açmayı kullanan Azure AD kiracısına bağlı SaaS bulut uygulamalarına erişimini denetlemek etkinleştirir. Örneğin, diğer kuruluşların bu aynı uygulama örneklerini erişim sağlarken kuruluşunuzun Office 365 uygulamalarına erişim vermek isteyebilirsiniz.  

Kiracı kısıtlamaları kuruluşların kullanıcıları erişmesine izin verilen kiracılar listesinde belirtme olanağı sağlar. Ardından Azure AD, yalnızca bu izin verilen kiracılar erişim verir.

Bu makalede, Office 365 için Kiracı kısıtlamaları odaklanır, ancak özellik çoklu oturum açma için Azure AD ile modern kimlik doğrulama protokolleri kullanan SaaS bulut uygulaması ile çalışması gerekir. Office 365 tarafından kullanılan kiracıda farklı bir Azure AD ile uygulamaları Kiracı SaaS kullanırsanız, gerekli tüm kiracılar için izin verilen emin olun. SaaS bulut uygulamaları hakkında daha fazla bilgi için bkz: [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/).

## <a name="how-it-works"></a>Nasıl çalışır?

Genel çözümü aşağıdaki bileşenlerden oluşur: 

1. **Azure AD** – `Restrict-Access-To-Tenants: <permitted tenant list>` varsa, Azure AD yalnızca sorunlar güvenlik belirteçleri için izin verilen kiracılar olduğu. 

2. **Şirket içi proxy sunucu altyapısı** – listesini içeren başlık eklemek için yapılandırılmış SSL incelenmesi özellikli bir proxy cihaz kiracılar için Azure AD giden trafiğe izin. 

3. **İstemci yazılımını** – Kiracı kısıtlamalarını desteklemek amacıyla istemci yazılımını istemeniz gerekir belirteçlerini doğrudan Azure AD'den, böylece trafik proxy altyapısı tarafından müdahale edilebilir. Kiracı kısıtlamaları şu anda desteklenen tarayıcı tabanlı bir Office 365 uygulamaları ve Office istemcileri tarafından modern kimlik doğrulaması (gibi OAuth 2.0) kullanıldığında. 

4. **Modern kimlik doğrulaması** – bulut Hizmetleri, Kiracı kısıtlamaları kullanın ve verilmeyen tüm kiracılar için erişimi engellemek için modern kimlik doğrulaması kullanmalıdır. Office 365 bulut Hizmetleri, modern kimlik doğrulama protokolleri kullanmak için varsayılan olarak yapılandırılmalıdır. Office 365 modern kimlik doğrulaması desteğini en son bilgiler için okuma [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

Aşağıdaki diyagram, üst düzey trafik akışını gösterir. Yalnızca SSL incelemesi trafiği Azure AD'ye, Office 365 bulut Hizmetleri için gereklidir. Bu ayrım, çünkü Azure ad kimlik doğrulaması için trafik hacmi genellikle çok daha düşük trafik hacmi Exchange Online ve SharePoint Online gibi SaaS uygulamaları için önemlidir.

![Kiracı kısıtlamaları trafik akışı - diyagram](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Kiracı kısıtlamaları ayarlamak

Kiracı kısıtlamaları ile kullanmaya başlamak için iki adım vardır. İlk adım istemcilerinizin doğru adrese bağlanabildiğinden emin olmaktır. Proxy altyapınızı yapılandırmak için kullanılan saniyedir.

### <a name="urls-and-ip-addresses"></a>URL'leri ve IP adresleri

Kiracı kısıtlamaları kullanmak için istemcilerin kimliğini doğrulamak için aşağıdaki Azure AD URL'lere bağlanabilir olmalıdır: login.microsoftonline.com login.microsoft.com ve login.windows.net. Ayrıca, erişim için Office 365, istemcilerinize de FQDN/URL'leri bağlanabilmesi gerekir ve IP adresleri tanımlanan [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxy yapılandırması ve gereksinimleri

Kiracı kısıtlamaları proxy altyapınızın etkinleştirmek için aşağıdaki yapılandırma gerekir. Bu kılavuz genel, olduğundan belirli uygulama adımlarını proxy satıcınızın belgelerine başvurmanız gerekir.

#### <a name="prerequisites"></a>Önkoşullar

- Proxy SSL durdurma, HTTP üst bilgi ekleme, gerçekleştirme ve FQDN'ler/URL'leri kullanarak hedefleri filtre olmalıdır. 

- İstemciler, SSL iletişimi için Ara sunucu tarafından sunulan sertifika zincirine güvenmelidir. Örneğin, bir iç PKI sertifikaları kullanılıyorsa, iç veren kök sertifika yetkilisi sertifikası güvenilir olması gerekir.

- Bu özellik, Office 365 aboneliklerine de dahildir, ancak Azure AD Premium 1 lisansları diğer SaaS uygulamalarına erişimi denetlemek için Kiracı kısıtlamaları kullanmak istiyorsanız, daha sonra gereklidir.

#### <a name="configuration"></a>Yapılandırma

Login.microsoftonline.com, login.microsoft.com ve login.windows.net gelen istek için her iki HTTP üst bilgileri ekleyin: *Kısıtlama-erişim--kiracıların* ve *kısıtlama-erişim-bağlam*.

Üst bilgileri, aşağıdaki öğeleri içermelidir: 
- İçin *kiracılar için kısıtlama erişim*, değerini \<Kiracı listesini izin\>, kullanıcıların erişmesine izin vermek istediğiniz kiracılar virgülle ayrılmış listesi verilmiştir. Bir kiracı ile kaydedilen herhangi bir etki alanı, bu listedeki Kiracı tanımlamak için kullanılabilir. Örneğin, hem Contoso ve Fabrikam kiracıları için erişime izin vermek için ad/değer çifti şuna benzer:  `Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- İçin *kısıtlama erişim bağlam*, hangi Kiracı, Kiracı kısıtlamaları ayarlama bildirmek, bir tek dizin kimliği değeri. Örneğin, Contoso Kiracı kısıtlamaları İlkesi ayarlamak Kiracı olarak bildirmek için ad/değer çifti şuna benzer: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> Dizin Kimliğinizi bulabilirsiniz [Azure portalında](https://portal.azure.com). Bir yönetici olarak oturum açın, select **Azure Active Directory**, ardından **özellikleri**.

Kullanıcıların kendi HTTP üstbilgisi onaylanmamış kiracıyla eklemesi önlemek için proxy zaten gelen istekteki ise kiracılar için kısıtlama erişim üstbilgi değiştirmesi gerekiyor. 

Login.microsoftonline.com login.microsoft.com ve login.windows.net gönderilen tüm istekler için Ara sunucu kullanmak için istemcileri zorlanması gerekir. PAC dosyaları istemcileri bir proxy kullanmak üzere yönlendirmek için kullanılır, örneğin, son kullanıcıların düzenlemek ya da PAC dosyaları devre dışı olmaması gerekir.

## <a name="the-user-experience"></a>Kullanıcı deneyimi

Bu bölümde, hem son kullanıcılar ve Yöneticiler için deneyim gösterilir.

### <a name="end-user-experience"></a>Son kullanıcı deneyimi

Bir örnek kullanıcı Contoso ağında, ancak paylaşılan bir SaaS uygulama Outlook gibi çevrimiçi Fabrikam örneği erişmeye çalışıyor. Fabricam Contoso örneği için verilmeyen bir kiracı yoksa, kullanıcı şu sayfayı görür:

![Erişim engellendi sayfasıyla kiracılar verilmeyen kullanıcılar için](./media/tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Yönetici deneyimi

Kiracı kısıtlamaları, yapılandırma, kurumsal bir proxy'nin altyapıya gerçekleştirilir, ancak yöneticileri Azure portalında Kiracı kısıtlamaları raporları doğrudan erişebilirsiniz. Raporları görüntülemek için Azure Active Directory genel bakış sayfasına gidin, sonra 'Altında diğer capabilities' bakın.

Kısıtlı erişim bağlam Kiracı kullanılan kimliği de dahil olmak üzere Kiracı kısıtlamaları ilkesi nedeniyle engellenen oturum açma işlemleri görmek için bu raporu kullanabilirsiniz olarak belirtilen Kiracı Yöneticisi ve hedef dizin kimliği Kiracı ayarı kısıtlama kullanıcı Kiracı ya da kaynak kiracısı oturum açma için oturum açma dahil edilir.

![Kısıtlı oturum açma denemesi görüntülemek için Azure portalını kullanma](./media/tenant-restrictions/portal-report.png)

Azure portalında diğer raporları gibi raporunuzun kapsamını belirtmek için filtreleri kullanabilirsiniz. Belirli bir kullanıcı, uygulama, istemci veya zaman aralığı üzerinde filtre uygulayabilirsiniz.

## <a name="office-365-support"></a>Office 365 desteği

Office 365 uygulamaları, Kiracı kısıtlamaları tam olarak desteklemek için iki ölçütleri karşılamalıdır:

1. Modern kimlik doğrulaması kullanılan istemci destekler
2. Modern kimlik doğrulaması, bulut hizmeti için varsayılan kimlik doğrulama protokolü olarak etkinleştirilir.

Başvurmak [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) hangi Office istemcileri şu anda modern kimlik doğrulaması desteği en yeni bilgileri. Bu sayfa ayrıca modern kimlik doğrulaması belirli Exchange Online ve Skype Kurumsal çevrimiçi kiracılar için etkinleştirme yönergeleri için bağlantıları içerir. Modern kimlik doğrulaması, zaten SharePoint Online'da varsayılan olarak etkindir.

Kiracı kısıtlamaları şu anda Office 365 tarayıcı tabanlı uygulamalar tarafından desteklenir (Office portalı, Yammer, SharePoint siteleri, Outlook Web, vs.). Büyük istemciler için (Outlook, Skype kurumsal iş, Word, Excel, PowerPoint, vb.) Modern kimlik doğrulaması kullanıldığında Kiracı kısıtlamaları yalnızca zorunlu tutulabilir.  

Outlook ve Skype için modern kimlik doğrulamasını destekleyen iş istemcileri burada modern kimlik doğrulaması etkin kiracılar karşı eski protokolleri kullanmak hala mümkün olabilir, etkili bir şekilde Kiracı kısıtlamalarını aşabilir. Kimlik doğrulaması sırasında login.microsoftonline.com, login.microsoft.com veya login.windows.net başvurursanız, eski protokolleri kullanan uygulamalar Kiracı kısıtlamaları tarafından engelleniyor olabilir.

Windows için Outlook, müşterileri, son kullanıcıların profillerini için onaylanmamış e-posta hesapları eklemesini engelleyen kısıtlamalar uygulamak seçebilir. Örneğin, [varsayılan olmayan Exchange hesapları ekleme engelle](https://gpsearch.azurewebsites.net/default.aspx?ref=1) Grup İlkesi ayarı.

## <a name="testing"></a>Test Etme

Kiracı kısıtlamaları tüm kuruluşunuz için uygulamadan önce denemek istiyorsanız, iki seçenek vardır: Fiddler veya Ara sunucu ayarlarını aşamalı bir sunum gibi bir araç kullanarak ana bilgisayar tabanlı bir yaklaşım.

### <a name="fiddler-for-a-host-based-approach"></a>Ana bilgisayar tabanlı bir yaklaşım için fiddler'ı

Fiddler yakalamak ve HTTP/HTTPS trafiğini, HTTP üst bilgilerini ekleme dahil olmak üzere değiştirmek için kullanılan proxy hata ayıklama ücretsiz bir web tarayıcısı uygulamasıdır. Kiracı kısıtlamaları test etmek için fiddler'ı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1.  [Fiddler'ı yükleyip](https://www.telerik.com/fiddler).
2.  Başına HTTPS trafiği şifresini çözmek için fiddler'ı yapılandırma [Fiddler'ın Yardım belgeleri](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Eklemek için fiddler'ı yapılandırma *kiracılar için kısıtlama erişim* ve *kısıtlama erişim bağlam* üstbilgileri özel kuralları kullanarak:
    1. Fiddler'ı Web hata ayıklayıcısı Aracı'nda seçin **kuralları** menü ve select **kuralları Özelleştir...** CustomRules dosyayı açmak için.
    2. Aşağıdaki satırları ekleyin başında *OnBeforeRequest* işlevi. Değiştirin \<Kiracı etki alanı\> bir etki alanında kayıtlı kiracınız ile Örneğin, contoso.onmicrosoft.com. Değiştirin \<dizin kimliği\> , kiracınızın Azure AD GUID tanımlayıcısı.

    ```
    if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
    ```

    Birden çok kiracının izin vermeniz gerekiyorsa, Kiracı adını ayırmak için virgül kullanın. Örneğin:

    ```
    oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
    ```

4. CustomRules dosyasını kaydedip kapatın.

Fiddler'ı yapılandırdıktan sonra şuraya giderek trafiği yakalayabilirsiniz **dosya** menü ve seçerek **trafik yakalama**.

### <a name="staged-rollout-of-proxy-settings"></a>Ara sunucu ayarlarını aşamalı dağıtımı

Proxy altyapınızı özelliklerine bağlı olarak, piyasaya ayarları kullanıcılarınıza aşama mümkün olabilir. Göz önünde bulundurmanız gereken birkaç üst düzey seçeneği şunlardır:

1.  Test kullanıcıları, normal kullanıcılara üretim proxy altyapısını kullanmaya devam ederken, test proxy altyapısını işaret edecek şekilde PAC dosyalarını kullanın.
2.  Bazı Ara sunucuları farklı yapılandırmaları grupları kullanarak destekleyebilir.

Belirli Ayrıntılar için proxy sunucusu belgelerinize bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Gözden geçirme [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
