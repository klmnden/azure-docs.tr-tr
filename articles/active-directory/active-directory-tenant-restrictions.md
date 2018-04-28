---
title: Bulut uygulamalarında kiracılar - Azure kısıtlayarak erişimini yönetme | Microsoft Docs
description: Kiracı kısıtlamaları hangi kullanıcıları yönetmek için nasıl kullanılacağını kendi Azure AD kiracısı tabanlı uygulamalara erişebilir.
services: active-directory
documentationcenter: ''
author: kgremban
manager: mtillman
editor: yossib
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2018
ms.author: kgremban
ms.openlocfilehash: dae4599db5127ac8fd266d5e0f299e1284fc9b9c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>SaaS erişimi yönetmek için kullanım Kiracı kısıtlamaları bulut uygulamalarında

Güvenlik vurgulamak büyük kuruluşlar, Office 365 gibi hizmetler bulut, ancak, kullanıcılar yalnızca onaylanan kaynaklara erişebilir bilmeniz gereken taşımak istersiniz. Geleneksel olarak, erişimi yönetmek üzere istediğinizde şirket etki alanı adlarını veya IP adreslerini kısıtlayın. Bu yaklaşım, paylaşılan etki alanı adları outlook.office.com ve login.microsoftonline.com gibi çalışan bir genel bulutta SaaS uygulamaları barındırıldığı dünyada başarısız olur. Bu adresler engelleme kullanıcılar Outlook web üzerinde tamamen yalnızca bunları onaylanan kimlikleri ve kaynaklar için kısıtlama yerine erişmesini önlemek.

Bu sorunu Azure Active Directory'nin çözüme Kiracı kısıtlamaları adlı bir özelliktir. Kiracı kısıtlamaları SaaS bulut uygulamalarını, uygulamaları için çoklu oturum açmayı kullanan Azure AD kiracısı göre erişimi denetlemek kuruluş sağlar. Örneğin, bu aynı uygulama diğer kuruluşlardan örneklerinin erişim sağlarken, kuruluşunuzun Office 365 uygulamalarına erişmesine izin vermek isteyebilirsiniz.  

Kiracı kısıtlamaları kuruluşlar kullanıcılara erişim izni verilen kiracılar listesini belirtme olanağı sağlar. Ardından Azure AD, yalnızca bu kiracılar izin verilen erişim verir.

Bu makalede, Office 365 için Kiracı kısıtlamalar odaklanır, ancak özellik çoklu oturum açma için Azure AD ile modern kimlik doğrulama protokollerini kullanan herhangi bir SaaS bulut uygulama ile çalışması gerekir. Office 365 tarafından kullanılan Kiracı öğesinden farklı bir Azure AD ile uygulamaları Kiracı SaaS kullanırsanız, gerekli tüm kiracılar izin verilen emin olun. SaaS bulut uygulamaları hakkında daha fazla bilgi için bkz: [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/).

## <a name="how-it-works"></a>Nasıl çalışır?

Genel çözümünün aşağıdaki bileşenlerden oluşur: 

1. **Azure AD** – varsa `Restrict-Access-To-Tenants: <permitted tenant list>` izin verilen kiracılar için mevcut, Azure AD yalnızca sorunları güvenlik belirteçleri değil. 

2. **Şirket içi proxy sunucu altyapısını** – listesini içeren başlık eklemek için yapılandırılmış SSL incelenmesi yeteneğine sahip bir proxy aygıtını kiracılar için Azure AD giden trafiğe izin. 

3. **İstemci yazılımı** – Kiracı kısıtlamalarını desteklemek amacıyla istemci yazılımı istemeniz gerekir belirteçleri doğrudan Azure AD'den böylece trafik proxy altyapısı tarafından müdahale edilebilir. Kiracı kısıtlamaları şu anda desteklenen tarayıcı tabanlı Office 365 uygulamaları ve Office istemcileri tarafından modern kimlik doğrulaması (gibi OAuth 2.0) kullanıldığında. 

4. **Modern kimlik doğrulaması** – bulut Hizmetleri, Kiracı kısıtlamaları'nı kullanın ve izin verilen olmayan tüm kiracılar erişimi engellemek için modern kimlik doğrulaması kullanmalıdır. Office 365 bulut Hizmetleri, modern kimlik doğrulama protokolleri kullanmak için varsayılan olarak yapılandırılmalıdır. Office 365 modern kimlik doğrulama desteği en son bilgiler için okuma [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

Aşağıdaki diyagramda, üst düzey trafik akışı gösterilmektedir. SSL denetlemesi yalnızca trafiğinin Azure ad, Office 365 bulut Hizmetleri için gereklidir. Bu ayrım, Azure ad kimlik doğrulaması için trafiği birimi genellikle çok daha az Exchange Online ve SharePoint Online gibi SaaS uygulamaları için trafiği birimden olduğundan önemlidir.

![Kısıtlamaları trafiği akışı Kiracı - diyagram](./media/active-directory-tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Kiracı sınırlamaları

Kiracı kısıtlamalarıyla başlamak için iki adım vardır. İlk adım, istemcilerinizin sağ adreslere bağlanabildiğinden emin olmaktır. İkinci proxy altyapınızı yapılandırmaktır.

### <a name="urls-and-ip-addresses"></a>URL'leri ve IP adresleri

Kiracı kısıtlamaları kullanmak için istemcilerinizin kimliğini doğrulamak için aşağıdaki Azure AD URL'lere bağlanabilmesi gerekir: login.microsoftonline.com, login.microsoft.com ve login.windows.net. Ayrıca, erişimi Office 365, istemcilerinizi de FQDN'ler/URL'leri bağlanabilmesi gerekir ve IP adreslerini tanımlanan [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxy yapılandırmasını ve gereksinimleri

Aşağıdaki yapılandırma proxy altyapınızın Kiracı kısıtlamaları etkinleştirmek için gereklidir. Bu kılavuz için belirli uygulama adımlarını proxy satıcınızın belgelerine başvurmalısınız geneldir.

#### <a name="prerequisites"></a>Önkoşullar

- Proxy SSL kişiler tarafından ele, HTTP üstbilgisi ekleme gerçekleştirmek ve FQDN'ler/URL'leri kullanarak hedefleri filtre kurabilmesi gerekir. 

- İstemciler, SSL iletişimi için proxy tarafından sunulan sertifika zinciri güvenmesi gerekir. Örneğin, bir iç PKI sertifikaları kullandıysanız, iç veren kök sertifika yetkilisi sertifikası güvenilir olması gerekir.

- Bu özellik, Office 365 abonelikleri dahil, ancak diğer SaaS uygulamalarına erişimi denetlemek için Kiracı kısıtlamaları kullanmak istiyorsanız, Azure AD Premium 1 lisansları sonra gerekli.

#### <a name="configuration"></a>Yapılandırma

Login.microsoftonline.com, login.microsoft.com ve login.windows.net her gelen istek için iki HTTP üstbilgi Ekle: *kiracılar için sınırla erişim* ve *sınırla erişim bağlamı*.

Üstbilgiler, aşağıdaki öğeleri içermelidir: 
- İçin *kiracılar için sınırla erişim*, değerini \<Kiracı listesi izin\>, kullanıcıların erişmesine izin vermek istediğiniz kiracılar virgülle ayrılmış bir listesi verilmiştir. Bir kiracı ile kayıtlı herhangi bir etki alanı, bu listede Kiracı tanımlamak için kullanılabilir. Örneğin, Contoso ve Fabrikam kiracıları için erişime izin vermek için ad/değer çifti şuna benzer:  `Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- İçin *sınırla erişim bağlam*, hangi Kiracı Kiracı kısıtlamaları ayarlama bildirme bir tek bir dizin kimliği değeri. Örneğin, Contoso Kiracı kısıtlamaları İlkesi ayarlamak Kiracı olarak bildirmek için ad/değer çifti şuna benzer: `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> Dizin Kimliğinizi bulabilirsiniz [Azure portal](https://portal.azure.com). Bir yönetici olarak oturum açın, select **Azure Active Directory**seçeneğini belirleyip **özellikleri**.

Kullanıcıların onaylanmamış kiracılarla kendi HTTP üstbilgisi ekleme önlemek için proxy zaten gelen istekte ise kiracılar için sınırla erişim üstbilgi değiştirmesi gerekiyor. 

İstemciler login.microsoftonline.com, login.microsoft.com ve login.windows.net tüm istekler için proxy kullanmak için zorunlu gerekir. Örneğin, istemcilerin proxy kullanmasını yönlendirmek için PAC dosyaları kullanılıyorsa, son kullanıcıların düzenlemek ya da PAC dosyaları devre dışı olmamalıdır.

## <a name="the-user-experience"></a>Kullanıcı deneyimi

Bu bölümde, son kullanıcılar ve Yöneticiler için deneyimi gösterir.

### <a name="end-user-experience"></a>Son kullanıcı deneyimi

Bir örnek kullanıcı Contoso ağında, ancak paylaşılan bir SaaS uygulama Outlook gibi çevrimiçi Fabrikam örneğini erişmeye çalışıyor. Bu örnek için izin verilen olmayan bir kiracı contoso ise, kullanıcı aşağıdaki sayfayı görür:

![Sayfa kiracılar izin olmayan kullanıcılar için erişim reddedildi](./media/active-directory-tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Yönetici deneyimi

Kurumsal proxy yapı yapılandırma Kiracı kısıtlama yapılır, ancak yöneticileri Azure portalında Kiracı kısıtlamaları raporları doğrudan erişebilirsiniz. Raporları görüntülemek için Azure Active Directory genel bakış sayfasına gidin ve sonra 'Altında diğer Özellikler' arayın.

Kısıtlı erişim bağlam Kiracı kullanılan kimliği de dahil olmak üzere Kiracı kısıtlamaları ilkesi nedeniyle engellenen tüm oturum açma işlemlerini görmek için bu raporu kullanabilirsiniz olarak belirtilen Kiracı yönetici ve hedef dizin kimliği

![Kısıtlı oturum açma denemeleri görüntülemek için Azure portalını kullanma](./media/active-directory-tenant-restrictions/portal-report.png)

Azure portalındaki diğer raporları gibi raporunuzu kapsamını belirtmek için filtreleri kullanabilirsiniz. Belirli bir kullanıcı, uygulama, istemci veya zaman aralığı üzerinde filtre uygulayabilirsiniz.

## <a name="office-365-support"></a>Office 365 desteği

Office 365 uygulamaları Kiracı kısıtlamalarını tam olarak desteklemek için iki ölçütleri karşılamalıdır:

1. İstemcinin kullandığı modern kimlik doğrulamasını destekler
2. Modern kimlik doğrulaması, bulut hizmeti için varsayılan kimlik doğrulama protokolü olarak etkinleştirilir.

Başvurmak [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) hangi Office istemcileri şu anda modern kimlik doğrulamasını destekleyen en son bilgiler için. Bu sayfa ayrıca modern kimlik doğrulamasını belirli Exchange Online ve Skype Kurumsal çevrimiçi kiracılar için etkinleştirme yönergelere bağlantılar içerir. Modern kimlik doğrulaması zaten SharePoint Online'daki varsayılan olarak etkindir.

Kiracı kısıtlamaları şu an Office 365 tarayıcı tabanlı uygulamalar tarafından desteklenen (Office portalı, Yammer, SharePoint siteleri, Outlook Web, vs.). Kalın istemciler için (Outlook, Skype iş, Word, Excel, PowerPoint, vb.) Modern kimlik doğrulama kullanıldığında, Kiracı kısıtlamaları yalnızca zorunlu tutulabilir.  

Outlook ve Skype modern kimlik doğrulamasını destekleyen iş istemcileri burada modern kimlik doğrulaması etkinleştirilmediğinden kiracılar karşı eski protokolleri kullanmak hala mümkün olabilir için etkili bir şekilde Kiracı kısıtlamaları atlama. Kimlik doğrulaması sırasında login.microsoftonline.com, login.microsoft.com veya login.windows.net başvurursanız, eski protokoller kullanan uygulamalar Kiracı kısıtlamaları tarafından engellenmiş olabilir.

Windows Outlook için müşteriler, son kullanıcıların kendi profillerine onaylanmamış posta hesapları eklemesini engelleyen kısıtlamalar uygulamak seçebilirsiniz. Örneğin, [varsayılan olmayan Exchange hesapları ekleme engelle](http://gpsearch.azurewebsites.net/default.aspx?ref=1) Grup İlkesi ayarı. Windows olmayan platformlarında Outlook ve Skype Kurumsal tüm platformlarda Kiracı kısıtlamaları için tam destek şu anda kullanılamıyor.

## <a name="testing"></a>Test Etme

Tüm kuruluşunuz için uygulamadan önce Kiracı kısıtlamaları denemek istiyorsanız, iki seçenek vardır: Fiddler veya aşamalı bir sunum proxy ayarları gibi bir araç kullanarak ana bilgisayar tabanlı bir yaklaşım.

### <a name="fiddler-for-a-host-based-approach"></a>Ana bilgisayar tabanlı bir yaklaşım için fiddler

Fiddler yakalamak ve HTTP üstbilgileri ekleme de dahil olmak üzere HTTP/HTTPS trafiğini değiştirmek için kullanılan proxy hata ayıklama ücretsiz bir web uygulamasıdır. Kiracı kısıtlamaları test etmek için fiddler'ı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1.  [Fiddler'ı yükleyip](http://www.telerik.com/fiddler).
2.  HTTPS trafiğini başına şifresini çözmek için fiddler'ı yapılandırma [Fiddler'ın Yardım belgelerine](http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Eklemek için fiddler'ı yapılandırma *kiracılar için sınırla erişim* ve *sınırla erişim bağlam* özel kuralları kullanarak üstbilgileri:
  1. Fiddler Web Hata Ayıklayıcısı aracı seçin **kuralları** menü ve select **özelleştirme kuralları...** CustomRules dosya açılamıyor.
  2. Başında aşağıdaki satırları ekleyin *OnBeforeRequest* işlevi. Değiştir \<Kiracı etki alanı\> bir etki alanında kayıtlı ile Kiracı, örneğin, contoso.onmicrosoft.com. Değiştir \<dizin kimliği\> , kiracınıza ait Azure AD GUID tanımlayıcısı.

  ```
  if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
  ```

  Birden çok kiracıya izin vermeniz gerekiyorsa Kiracı adları ayırmak için virgül kullanın. Örneğin:

  ```
  oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
  ```

4. CustomRules dosyasını kaydedip kapatın.

Fiddler'ı yapılandırdıktan sonra giderek trafiği yakalayabilir **dosya** menü ve seçerek **trafiği Yakala**.

### <a name="staged-rollout-of-proxy-settings"></a>Proxy ayarları aşamalı dağıtımı

Proxy altyapınızı özelliklerine bağlı olarak, kullanıcılarınıza ayarları sunum aşama mümkün olabilir. Değerlendirme için birkaç üst düzey seçenekleri şunlardır:

1.  Normal kullanıcılar üretim proxy altyapısını kullanmaya devam ederken test kullanıcılarını test proxy altyapısına işaret edecek şekilde PAC dosyalarını kullanın.
2.  Bazı proxy sunucular farklı yapılandırmaları gruplarını kullanarak destekleyebilir.

Belirli Ayrıntılar için proxy sunucusu belgelerinize bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [güncelleştirilmiş Office 365 modern kimlik doğrulaması](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Gözden geçirme [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
