---
title: SaaS erişimi yönetmek için kullanım Kiracı kısıtlamaları bulut uygulamalarına - Azure | Microsoft Docs
description: Kiracı kısıtlamaları yönetmek için hangi kullanıcılar kendi Azure AD Kiracı göre uygulamalara erişebilir kullanma
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
ms.date: 03/28/2019
ms.author: mimart
ms.reviewer: richagi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a340663a1ec4ddf748c6dc2bc3a4e2ce0c4228e
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65824393"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>Kiracı kısıtlamaları SaaS bulut uygulamalarına erişimi yönetmek için kullanın

Güvenlik vurgulamak büyük kuruluşlar, bulut Hizmetleri Office 365 gibi ancak kullanıcılar yalnızca onaylanan kaynaklara erişebilir bilmeniz gereken taşımak istiyorum. Geleneksel olarak, erişimi yönetmek istediklerinde şirket etki alanı adlarını veya IP adreslerini kısıtlayın. Bu yaklaşım yazılım olarak hizmet (veya SaaS) uygulamaları burada barındırılan paylaşılan bir etki alanı adları gibi çalışan bir genel buluttaki bir dünyada başarısız [outlook.office.com](https://outlook.office.com/) ve [login.microsoftonline.com](https://login.microsoftonline.com/). Bu adresler engelleme, kullanıcıların web üzerinde Outlook'u tamamen yalnızca bunları onaylı kimlikleri ve kaynaklarına erişimi kısıtlama yerine erişmesini tutacak.

Kiracı kısıtlamaları denilen bir özelliği bu sınama için Azure Active Directory (Azure AD) çözümüdür. Kiracı kısıtlamaları ile kuruluşlar için çoklu oturum açma uygulamaları kullanan Azure AD kiracısına bağlı SaaS bulut uygulamalarına erişimini denetleyebilirsiniz. Örneğin, diğer kuruluşların bu aynı uygulama örneklerini erişim sağlarken kuruluşunuzun Office 365 uygulamalarına erişim vermek isteyebilirsiniz.  

Kiracı kısıtlamaları ile kuruluşlar, kullanıcılarının erişmesine izin verilen kiracılar listesinde belirtebilirsiniz. Ardından Azure AD, yalnızca bu izin verilen kiracılar erişim verir.

Bu makalede, Office 365 için Kiracı kısıtlamaları odaklanır, ancak özellik çoklu oturum açma için Azure AD ile modern kimlik doğrulama protokolleri kullanan SaaS bulut uygulaması ile çalışması gerekir. Office 365 tarafından kullanılan kiracıda farklı bir Azure AD ile uygulamaları Kiracı SaaS kullanırsanız, gerekli tüm kiracılar için izin verilen emin olun. SaaS bulut uygulamaları hakkında daha fazla bilgi için bkz: [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/).

## <a name="how-it-works"></a>Nasıl çalışır?

Genel çözümü aşağıdaki bileşenlerden oluşur:

1. **Azure AD**: Varsa `Restrict-Access-To-Tenants: <permitted tenant list>` varsa, Azure AD yalnızca sorunlar güvenlik belirteçleri için izin verilen kiracılar olduğu.

2. **Şirket içi proxy sunucu altyapısı**: Güvenli Yuva Katmanı (SSL) denetimi özellikli bir proxy cihaz altyapısıdır. Azure AD için hedefleyen trafiği uygulamasına izin verilen kiracılar listesini içeren başlık eklemek için bir proxy yapılandırmanız gerekir.

3. **İstemci yazılımını**: Proxy altyapı trafiği yakalayabilirsiniz. böylece Kiracı kısıtlamalarını desteklemek amacıyla, istemci yazılımını belirteçlerini doğrudan Azure AD'den istemeniz gerekir. Modern kimlik doğrulaması (gibi OAuth 2.0) kullanan Office istemcileri gibi tarayıcı tabanlı bir Office 365 uygulamaları, şu anda Kiracı kısıtlamaları destekler.

4. **Modern kimlik doğrulaması**: Bulut Hizmetleri, Kiracı kısıtlamaları kullanın ve verilmeyen tüm kiracılar için erişimi engellemek için modern kimlik doğrulaması kullanmanız gerekir. Office 365 bulut Hizmetleri, modern kimlik doğrulama protokolleri, varsayılan olarak kullanmak üzere yapılandırmanız gerekir. Office 365 modern kimlik doğrulaması desteğini en son bilgiler için okuma [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

Aşağıdaki diyagram, üst düzey trafik akışını gösterir. Kiracı kısıtlamaları yalnızca trafiği Azure AD'ye, Office 365 cloud services için SSL denetimi gerektirir. Azure ad kimlik doğrulaması için trafik hacmi genellikle çok daha düşük trafik hacmi Exchange Online ve SharePoint Online gibi SaaS uygulamalarında olduğu için Bu ayrım önemlidir.

![Kiracı kısıtlamaları trafik akışı - diyagram](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Kiracı kısıtlamaları ayarlamak

Kiracı kısıtlamaları ile kullanmaya başlamak için iki adım vardır. İlk olarak, istemcilerinizin doğru adrese bağlanabildiğinden emin olun. İkinci olarak, proxy altyapınızı yapılandırın.

### <a name="urls-and-ip-addresses"></a>URL'leri ve IP adresleri

Kiracı kısıtlamaları kullanmak için istemcilerin kimliğini doğrulamak için aşağıdaki Azure AD URL'lere bağlanabilir olmalıdır: [login.microsoftonline.com](https://login.microsoftonline.com/), [login.microsoft.com](https://login.microsoft.com/), ve [ Login.Windows.NET](https://login.windows.net/). Ayrıca, erişim için Office 365, istemcilerinize de tam etki alanı adlarına (FQDN) URL bağlanabilmesi gerekir ve IP adresleri tanımlanan [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxy yapılandırması ve gereksinimleri

Aşağıdaki yapılandırma, Ara sunucu altyapınızı aracılığıyla Kiracı kısıtlamaları etkinleştirmek için gereklidir. Bu kılavuz genel, olduğundan belirli uygulama adımlarını proxy satıcınızın belgelerine başvurmanız gerekir.

#### <a name="prerequisites"></a>Önkoşullar

- Proxy SSL durdurma, HTTP üst bilgi ekleme, gerçekleştirme ve FQDN'ler/URL'leri kullanarak hedefleri filtre olmalıdır.

- İstemciler, SSL iletişimi için Ara sunucu tarafından sunulan sertifika zincirine güvenmelidir. Örneğin, bir iç sertifika [ortak anahtar altyapısı (PKI)](/windows/desktop/seccertenroll/public-key-infrastructure) olan kullanıldığında, iç veren kök sertifika yetkilisi sertifikası güvenilir olması gerekir.

- Bu özellik, Office 365 aboneliklerine de dahildir, ancak Azure AD Premium 1 lisansları Kiracı kısıtlamaları diğer SaaS uygulamalarına erişimi denetlemek için kullanmak istiyorsanız, daha sonra gereklidir.

#### <a name="configuration"></a>Yapılandırma

Login.microsoftonline.com, login.microsoft.com ve login.windows.net gelen istek için her iki HTTP üst bilgileri ekleyin: *Kısıtlama-erişim--kiracıların* ve *kısıtlama-erişim-bağlam*.

Üst bilgileri, aşağıdaki öğeleri içermelidir:

- İçin *kiracılar için kısıtlama erişim*, değerini kullanın \<Kiracı listesini izin\>, kullanıcıların erişmesine izin vermek istediğiniz kiracılar virgülle ayrılmış listesi verilmiştir. Bir kiracı ile kaydedilen herhangi bir etki alanı, bu listedeki Kiracı tanımlamak için kullanılabilir. Örneğin, hem Contoso ve Fabrikam kiracıları için erişime izin vermek için ad/değer çifti şuna benzer: `Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com`

- İçin *kısıtlama erişim bağlam*hangi Kiracı bildirme olan ayarı Kiracı kısıtlamaları, tek bir dizin kimliği değerini kullanın. Örneğin, Contoso Kiracı kısıtlamaları İlkesi ayarlanan Kiracı olarak bildirmek için ad/değer çifti şuna benzer: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> Dizin Kimliğinizi bulabilirsiniz [Azure Active Directory portalında](https://aad.portal.azure.com/). Bir yönetici olarak oturum açın, select **Azure Active Directory**, ardından **özellikleri**.

Kullanıcıların kendi HTTP üstbilgisi onaylanmamış kiracıyla eklemesi önlemek için proxy değiştirmesi gerekiyor *kiracılar için kısıtlama erişim* zaten gelen istekteki ise üstbilgi.

Login.microsoftonline.com login.microsoft.com ve login.windows.net gönderilen tüm istekler için Ara sunucu kullanmak için istemcileri zorlanması gerekir. Örneğin, son kullanıcıların PAC dosyaları istemcileri bir proxy kullanmak üzere yönlendirmek için kullanılır, düzenleme ya da PAC dosyaları devre dışı olmamalıdır.

## <a name="the-user-experience"></a>Kullanıcı deneyimi

Bu bölümde, hem son kullanıcılar ve yöneticiler deneyimi açıklanmıştır.

### <a name="end-user-experience"></a>Son kullanıcı deneyimi

Bir örnek kullanıcı Contoso ağında, ancak paylaşılan bir SaaS uygulama Outlook gibi çevrimiçi Fabrikam örneği erişmeye çalışıyor. Fabrikam Contoso örneği için verilmeyen bir kiracı yoksa, kullanıcı, BT departmanınız tarafından onaylanmayan bir kuruluşa ait bir kaynağa erişmeye çalıştığınız belirten bir erişim reddi iletisi görür.

### <a name="admin-experience"></a>Yönetici deneyimi

Kiracı kısıtlamaları, yapılandırma, kurumsal bir proxy'nin altyapıya gerçekleştirilir, ancak yöneticileri Azure portalında Kiracı kısıtlamaları raporları doğrudan erişebilirsiniz. Raporları görüntülemek için:

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com/). **Azure Active Directory Yönetim Merkezi** panosu açılır.

2. Sol bölmede **Azure Active Directory**’yi seçin. Azure Active Directory genel bakış sayfası görüntülenir.

3. İçinde **diğer özellikleri** başlığı seçin **Kiracı kısıtlamaları**.

Kısıtlı erişim bağlam Kiracı kullanılan kimliği de dahil olmak üzere Kiracı kısıtlamaları ilkesi nedeniyle engellenen oturum açma işlemleri görmek için bu raporu kullanabilirsiniz olarak belirtilen Kiracı Yöneticisi ve hedef dizin kimliği Kiracı ayarı kısıtlama kullanıcı Kiracı ya da kaynak kiracısı oturum açma için oturum açma dahil edilir.

Azure portalında diğer raporları gibi raporunuzun kapsamını belirtmek için filtreleri kullanabilirsiniz. Belirli bir zaman aralığı, kullanıcı, uygulama, istemci veya durumu üzerinde filtre uygulayabilirsiniz. Seçerseniz **sütunları** düğmesini seçebilirsiniz herhangi bir birleşimini aşağıdaki alanları veriyle görüntülemek:

- **Kullanıcı**
- **Uygulama**
- **Durumu**
- **Tarih**
- **Tarih (UTC)** (UTC olduğu Eşgüdümlü Evrensel Saat)
- **MFA kimlik doğrulama yöntemi** (çok faktörlü kimlik doğrulama yöntemi)
- **MFA kimlik doğrulama ayrıntısı** (çok faktörlü kimlik doğrulaması ayrıntı)
- **MFA sonucu**
- **IP adresi**
- **İstemci**
- **Kullanıcı Adı**
- **Konum**
- **Hedef Kiracı kimliği**

## <a name="office-365-support"></a>Office 365 desteği

Office 365 uygulamaları, Kiracı kısıtlamaları tam olarak desteklemek için iki ölçütleri karşılamalıdır:

1. İstemci, modern kimlik doğrulamasını destekler.
2. Modern kimlik doğrulaması, bulut hizmeti için varsayılan kimlik doğrulama protokolü olarak etkinleştirilir.

Başvurmak [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/) hangi Office istemcileri şu anda modern kimlik doğrulaması desteği en yeni bilgileri. Bu sayfa ayrıca modern kimlik doğrulaması belirli Exchange Online ve Skype Kurumsal çevrimiçi kiracılar için etkinleştirme yönergeleri için bağlantıları içerir. SharePoint Online zaten Modern kimlik doğrulaması varsayılan olarak etkinleştirir.

Office 365 tarayıcı tabanlı uygulamaları (Office portalı, Yammer, SharePoint siteleri, Outlook Web ve daha fazlası) şu anda, Kiracı kısıtlamaları destekler. Büyük istemciler (Outlook, Skype kurumsal iş, Word, Excel, PowerPoint ve daha fazla), modern kimlik doğrulaması kullanırken yalnızca Kiracı kısıtlamaları zorunlu kılabilir.  

Outlook ve Skype için modern kimlik doğrulamasını destekleyen iş istemcileri burada modern kimlik doğrulaması etkin değilse kiracılar karşı eski protokolleri kullanmak hala mümkün olabilir, etkili bir şekilde Kiracı kısıtlamalarını aşabilir. Kiracı kısıtlamaları, kimlik doğrulaması sırasında login.microsoftonline.com, login.microsoft.com veya login.windows.net başvurursanız, eski protokolleri kullanan uygulamalar engelleyebilir.

Windows için Outlook, müşterileri, son kullanıcıların profillerini için onaylanmamış e-posta hesapları eklemesini engelleyen kısıtlamalar uygulamak seçebilir. Örneğin, [varsayılan olmayan Exchange hesapları ekleme engelle](https://gpsearch.azurewebsites.net/default.aspx?ref=1) Grup İlkesi ayarı.

## <a name="testing"></a>Test ediliyor

Kiracı kısıtlamaları tüm kuruluşunuz için uygulamadan önce denemek istiyorsanız, iki seçeneğiniz vardır: Fiddler veya Ara sunucu ayarlarını aşamalı bir sunum gibi bir araç kullanarak ana bilgisayar tabanlı bir yaklaşım.

### <a name="fiddler-for-a-host-based-approach"></a>Ana bilgisayar tabanlı bir yaklaşım için fiddler'ı

Fiddler yakalamak ve HTTP/HTTPS trafiğini, HTTP üst bilgilerini ekleme dahil olmak üzere değiştirmek için kullanılan proxy hata ayıklama ücretsiz bir web tarayıcısı uygulamasıdır. Kiracı kısıtlamaları test etmek için fiddler'ı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. [Fiddler'ı yükleyip](https://www.telerik.com/fiddler).

2. Başına HTTPS trafiği şifresini çözmek için fiddler'ı yapılandırma [Fiddler'ın Yardım belgeleri](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

3. Eklemek için fiddler'ı yapılandırma *kiracılar için kısıtlama erişim* ve *kısıtlama erişim bağlam* üstbilgileri özel kuralları kullanarak:

   1. Fiddler'ı Web hata ayıklayıcısı Aracı'nda seçin **kuralları** menü ve select **kuralları Özelleştir...** CustomRules dosyayı açmak için.

   2. Aşağıdaki satırları ekleyin başında `OnBeforeRequest` işlevi. Değiştirin \<Kiracı etki alanı\> kiracınız ile kayıtlı bir etki alanı ile (örneğin, `contoso.onmicrosoft.com`). Değiştirin \<dizin kimliği\> , kiracınızın Azure AD GUID tanımlayıcısı.

      ```JScript.NET
      if (
          oSession.HostnameIs("login.microsoftonline.com") ||
          oSession.HostnameIs("login.microsoft.com") ||
          oSession.HostnameIs("login.windows.net")
      )
      {
          oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";
          oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";
      }
      ```

      Birden çok kiracının izin vermeniz gerekiyorsa, Kiracı adını ayırmak için virgül kullanın. Örneğin:

      `oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";`

4. CustomRules dosyasını kaydedip kapatın.

Fiddler'ı yapılandırdıktan sonra şuraya giderek trafiği yakalayabilirsiniz **dosya** menü ve seçerek **trafik yakalama**.

### <a name="staged-rollout-of-proxy-settings"></a>Ara sunucu ayarlarını aşamalı dağıtımı

Proxy altyapınızı özelliklerine bağlı olarak, piyasaya ayarları kullanıcılarınıza aşama mümkün olabilir. Göz önünde bulundurmanız gereken birkaç üst düzey seçeneği şunlardır:

1. Test kullanıcıları, normal kullanıcılara üretim proxy altyapısını kullanmaya devam ederken, test proxy altyapısını işaret edecek şekilde PAC dosyalarını kullanın.
2. Bazı Ara sunucuları farklı yapılandırmaları grupları kullanarak destekleyebilir.

Belirli Ayrıntılar için proxy sunucusu belgelerinize bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/)
- Gözden geçirme [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
