---
title: Karma kimlik tasarımı - veri koruma stratejisi Azure | Microsoft Docs
description: Tanımladığınız iş gereksinimlerini karşılamak, karma kimlik çözümü için veri koruma stratejisi tanımlarsınız.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 05c1575781f280b3be1843abee0469af52baeb2d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918422"
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için veri koruma stratejisini tanımlayın
Bu görevde, tanımladığınız iş gereksinimlerini karşılamak, karma kimlik çözümü için veri koruma stratejisi tanımlarsınız:

* [Veri koruma gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [İçerik Yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Erişim denetimi gereksinimleri belirleme](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Veri koruma seçeneklerini tanımlayın
İçinde anlatıldığı gibi [dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD, şirket içi Active Directory etki alanı hizmetleriyle (AD DS) eşitleyebilirsiniz. Bu tümleştirme, kuruluşların kullanım şirket kaynaklarına erişmeye çalışırken, kullanıcıların kimlik bilgilerini doğrulamak için Azure AD imkan sağlar. Her iki senaryo için bunu yapabilirsiniz: veri rest şirket içi ve bulut. Azure ad'deki verilerine erişim, bir güvenlik belirteci hizmeti (STS) aracılığıyla kullanıcı kimlik doğrulaması gerektirir.

Kimlik doğrulandıktan sonra kullanıcı asıl adı (UPN) kimlik doğrulaması belirtecinden okunur. Daha sonra çoğaltılan bölümü ve kullanıcının etki alanı için karşılık gelen kapsayıcı yetkilendirme sistemi belirler. Ardından bilgiler kullanıcının varlığı, bir etkin durumunu ve rol hedef kiracıya erişimi için kullanıcının bu oturumda yetki verilip verilmediğini belirlemek yetkilendirme sistem yardımcı olur. Yetkili belirli eylemleri (özellikle, kullanıcı adı ve parola sıfırlama Oluştur) Kiracı Yöneticisi ardından uyumluluk çalışmaları veya araştırmalar yönetmek için kullandığı bir denetim kaydı oluşturur.

Şirket içi veri merkezinizi Azure Depolama'ya bir Internet bağlantısı üzerinden verileri her zaman veri hacmi, bant genişliği kullanılabilirliği veya diğer konular nedeniyle uygun olmayabilir. [Azure depolama içeri/dışarı aktarma hizmeti](../../storage/common/storage-import-export-service.md) blob depolama alanındaki verilerin büyük hacimli yerleştirmek ve almak için donanım tabanlı bir seçenek sunar. Göndermenize olanak sağlayan [BitLocker şifrelenmiş](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) burada bulut operatörleri, depolama hesabınıza içeriği karşıya yükleme veya Azure verilerinizi sürücülerinizi size iade etmek için indirebileceği doğrudan bir Azure veri merkezine sabit disk sürücüleri. Yalnızca şifrelenmiş diskler (iş kurulumu sırasında hizmetin kendisi tarafından oluşturulan bir BitLocker anahtarını kullanarak) Bu işlem için kabul edilir. BitLocker anahtarı böylece anahtar bant dışında paylaşma sağlayarak Azure'a ayrı ayrı sağlanır.

Aktarımdaki verileri farklı senaryolarda gerçekleştirilebildiği olduğundan, aynı zamanda Microsoft Azure'nın kullandığını bilmek ister uygun olan [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) birbirinden, gibi konak ve Konuk düzeyi ölçümleri kullanan Kiracı trafiğini yalıtmak için Güvenlik duvarları, paket IP filtresini, bağlantı noktası engelleme ve HTTPS uç noktaları. Ancak, Azure'nın iç iletişimler, altyapı altyapı ve altyapı müşteri (şirket içi), dahil olmak üzere çoğu de şifrelenir. Başka bir önemli bir senaryo, Azure veri merkezleri içinde iletişimler ise; Microsoft hiçbir VM özelliklerini veya başka bir IP adresinde misafiri güvence altına almak için ağları yönetir. TLS/SSL, Azure depolama veya SQL veritabanları erişirken ya da bulut hizmetlerine bağlanırken kullanılır. Bu durumda, müşteri Yöneticisi bir TLS/SSL sertifikası edinme ve Kiracı altyapılarını dağıtmak için sorumludur. Trafik, sanal makineler aynı dağıtımda veya Microsoft Azure sanal ağı aracılığıyla tek bir dağıtım kiracılar arasında veri taşıma HTTPS, SSL/TLS veya diğerleri gibi şifreli iletişim protokolleri aracılığıyla korunabilir.

Nasıl sorulara verdiğiniz yanıtlara bağlı olarak [veri koruma gereksinimlerini belirlemek](plan-hybrid-identity-design-considerations-dataprotection-requirements.md), nasıl karma kimlik çözümü, bu işlemle size yardımcı olabilir ve verilerinizi korumak istediğiniz belirlemek mümkün olması gerekir. Aşağıdaki tabloda, her veri koruma senaryosu için kullanılabilir olan Azure tarafından desteklenen seçenekleri gösterir.

| Veri koruma seçenekleri | Buluttaki bekleyen | Şirket REST | Yoldaki |
| --- | --- | --- | --- |
| BitLocker Sürücü Şifrelemesi |X |X | |
| SQL Server'ın veritabanları şifrelemek için |X |X | |
| VM VM şifreleme | | |X |
| SSL/TLS | | |X |
| VPN | | |X |

> [!NOTE]
> Okuma [özelliğe göre Uyumluluk](https://azure.microsoft.com/support/trust-center/services/) adresindeki [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) her bir Azure hizmeti ile uyumludur sertifikalar hakkında daha fazla bilgi edinmek için.
> Veri koruma seçeneklerini çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma kazanılan bu görev için geçerli değildir. Verilerin her durum için kullanılabilir tüm seçenekleri yararlanarak emin olun.
>
>

## <a name="define-content-management-options"></a>İçerik yönetimi seçeneklerini tanımlayın

Karma kimlik altyapısını yönetmek için Azure AD kullanarak bir avantajı işlemi son kullanıcı açısından tamamen saydamdır olmasıdır. Kullanıcı paylaşılan bir kaynağa erişmeyi denediğinde, kaynak kimlik doğrulaması gerektiriyor, kullanıcının belirteç edinme ve kaynağa erişmek için Azure AD kimlik doğrulama isteği gönderilecek. Bu işlemi, kullanıcı etkileşimi olmadan arka planda gerçekleşir. 

Genellikle veri gizliliği ile ilgili önemli olan kuruluşlar, veri sınıflandırması için çözüm gerektirir. Geçerli şirket içi altyapılarını veri sınıflandırması kullanıyorsa, Azure AD ana depoya kullanıcının kimlik için kullanılacak mümkündür. Veri sınıflandırması çağrılır şirket içinde olduğundan emin ortak bir araç [veri sınıflandırma Araç Seti](https://msdn.microsoft.com/library/Hh204743.aspx) Windows Server 2012 R2 için. Bu araç tanımlamasına, sınıflandırmasına ve özel bulutunuzda dosya sunucularındaki verileri korumaya yardımcı olabilir. Kullanmak da mümkündür [otomatik dosya sınıflandırmasını](https://technet.microsoft.com/library/hh831672.aspx) bu görevi gerçekleştirmek için Windows Server 2012'de.

Kuruluşunuzun veri sınıflandırma yerinde yok, ancak yeni sunucuları şirket içi eklemenize gerek kalmadan gizli dosyaları korumak gereken Microsoft kullanabilirsiniz [Azure Rights Management hizmeti](https://technet.microsoft.com/library/JJ585026.aspx).  Dosya ve e-posta ile güvenli hale getirmek için Azure RMS kullanan şifreleme, kimlik ve yetkilendirme ilkelerini ve birden çok cihazda çalışır; telefon, Tablet ve bilgisayar. Azure RMS bir bulut hizmeti olduğundan, korumalı içerik paylaşabilmek için önce diğer kuruluşlarla açıkça güvenleri yapılandırmaya gerek yoktur. Bir Office 365 veya Azure AD dizini zaten varsa, kuruluşlar arası işbirliği otomatik olarak desteklenir. Ayrıca, yalnızca Azure RMS için Azure Active Directory Eşitleme Hizmetleri (AAD eşitleme) veya Azure AD Connect kullanarak şirket içi Active Directory hesaplarınız için ortak bir kimliği desteklemek için gereken dizin özniteliklerini eşitleyebilirsiniz.

Hangi kaynak kimin erişmekte olduğunu anlamak için içerik yönetiminin önemli bir parçası olduğundan, bu nedenle bir zengin günlüğe kaydetme özelliği için kimlik yönetimi çözümü önemlidir. Azure AD günlük dahil olmak üzere 30 gün sağlar:

* Rol üyeliği değişiklikleri (örn: kullanıcı genel Yönetici rolüne eklenen)
* Güncelleştirmeleri kimlik bilgisi (örn: parola değişikliklerini)
* Etki alanı yönetimi (örn: bir etki alanı kaldırılıyor özel bir etki alanı doğrulama)
* Ekleme veya uygulamaları kaldırma
* Kullanıcı Yönetimi (örn: ekleme, kaldırma, bir kullanıcı güncelleştiriliyor)
* Ekleme veya kaldırma lisansları

> [!NOTE]
> Okuma [Microsoft Azure güvenlik ve Denetim Günlüğü Yönetimi](https://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) azure'da günlüğe kaydetme özellikleri hakkında daha fazla bilgi edinmek için.
> Nasıl sorulara verdiğiniz yanıtlara bağlı olarak [içerik yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md), karma kimlik çözümünüzde yönetilecek içeriği istediğiniz belirlemek mümkün olması gerekir. Tüm seçenekleri Tablo 6'da sunulan Azure AD ile tümleştirilebilen olsa da, iş gereksinimlerinize daha uygun olduğu tanımlamak önemlidir.
>
>

| İçerik yönetimi seçenekleri | Yararları | Dezavantajları |
| --- | --- | --- |
| Şirket içinde Merkezi (Active Directory Rights Management sunucusu) |Veri sınıflandırma sorumlu sunucu altyapısı üzerinde tam denetim <br> Yerleşik özelliği Windows Server'da ek lisans ya da abonelik gerekmez <br> Karma bir senaryoda Azure AD ile tümleştirilebilir <br> Exchange Online ve SharePoint Online, hem de Office 365 gibi Microsoft Online hizmetlerinde Bilgi Hakları Yönetimi (IRM) özelliklerini destekler. <br> Exchange Server, SharePoint Server ve Windows Server ve dosya sınıflandırma altyapısı (FCI) çalıştıran dosya sunucuları gibi şirket içi Microsoft sunucu ürünlerini destekler. |Bu yana daha yüksek Bakım (Canlı yukarı güncelleştirmeler, yapılandırma ve olası yükseltme), BT sunucunun sahip olduğu <br> Şirket içi sunucu altyapısı gerektirir<br> Yerel olarak Azure özelliklerinden yararlanmak değil |
| (Azure RMS) bulutta Merkezi |Daha kolay yönetmek şirket içi çözüm ile karşılaştırıldığında <br> Karma bir senaryoda, AD DS ile tümleştirilebilir <br>  Azure AD ile tamamen tümleşik <br> Bir sunucu hizmeti dağıtmak için şirket içinde gerektirmez <br> Destekleyen şirket içi Exchange Server, SharePoint, sunucu ve Windows Server ve dosya sınıflandırma altyapısı (FCI) çalıştıran dosya sunucuları gibi Microsoft sunucu ürünleri <br> BT, BYOK özelliğiyle kiracısının anahtarı üzerinde tam denetime sahip olabilir. |Kuruluşunuz RMS'yi destekleyen bir bulut aboneliğine sahip olmalıdır <br> Kuruluşunuz için RMS kullanıcı kimlik doğrulamasını desteklemek için bir Azure AD dizinine sahip olmalıdır |
| Karma (Azure RMS ile tümleşik, şirket içi Active Directory Rights Management Server) |Bu senaryo sağladığı her iki merkezi şirket içinde ve bulutta toplanır. |Kuruluşunuz RMS'yi destekleyen bir bulut aboneliğine sahip olmalıdır <br> Kuruluşunuz için RMS kullanıcı kimlik doğrulamasını desteklemek için bir Azure AD dizinine sahip olmalıdır, <br> Azure bulut hizmeti arasında bir bağlantı gerektirir ve şirket içi altyapısı |

## <a name="define-access-control-options"></a>Erişim denetimi seçeneklerini tanımlayın
Kimlik doğrulama, yetkilendirme ve erişim denetimi özellikleri Azure AD'de kullanılabilir yararlanarak kullanıcıların olanak tanıyan merkezi kimlik deposu kullanmak, şirketinizin etkinleştirebilirsiniz ve iş ortaklarının tek kullanın (SSO) aşağıdaki şekilde gösterildiği gibi oturum:

![Merkezi Yönetim](./media/plan-hybrid-identity-design-considerations/centralized-management.png)

Merkezi Yönetim'i ve tam olarak diğer dizinleri ile tümleştirme

Azure Active Directory, binlerce SaaS uygulamasına çoklu oturum açma sağlar ve şirket içi web uygulamaları. Bkz: [Azure Active Directory Federasyon Uyumluluğu Listesi: çoklu oturum açmayı uygulamak için kullanılan üçüncü taraf kimlik sağlayıcıları](how-to-connect-fed-compatibility.md) makale Microsoft tarafından sınanan SSO üçüncü taraf hakkında daha fazla ayrıntı için. Bu özellik, kimlik ve erişim yönetim denetimi korurken çeşitli B2B senaryolarını uygulamak kuruluş sağlar. Ancak, sırasında B2B süreci, tasarlama iş ortağı tarafından kullanılan kimlik doğrulama yöntemini anlamak ve bu yöntem, Azure tarafından destekleniyorsa doğrulamak önemli. Şu anda, aşağıdaki yöntemlerden Azure AD tarafından desteklenir:

* Güvenlik onaylama işlemi biçimlendirme dili (SAML)
* OAuth
* Kerberos
* Belirteçler
* Sertifikalar

> [!NOTE]
> Okuma [Azure Active Directory kimlik doğrulama protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx) her protokolü ve azure'da özellikleri hakkında daha fazla ayrıntı bilmek.
>
>

Azure AD desteğini kullanarak mobil iş uygulamaları, çalışanların mobil uygulamalarını Kurumsal Active Directory kimlik bilgileriyle oturum açmak izin vermek için aynı bir kolayca mobil hizmetler kimlik doğrulaması deneyimi kullanabilirsiniz. Bu özellik, mobil Hizmetler'deki kimlik sağlayıcısı (içeren Microsoft Accounts, Facebook kimliği, kimliği Google ve Twitter kimliği) zaten desteklenen diğer kimlik sağlayıcılardan yanı sıra Azure AD desteklenir. Şirket içi uygulamaları, şirketin AD DS bulunan kullanıcının kimlik bilgileri kullanıyorsanız, iş ortakları ve buluttan gelen kullanıcıların erişim saydam olmalıdır. Kullanıcı koşullu erişim denetimi (bulut tabanlı) web uygulamaları, web API'si, Microsoft bulut Hizmetleri, üçüncü taraf SaaS uygulamaları ve yerel (mobil) istemci uygulamaları yönetme ve güvenlik avantajlarından sahip denetimi, hepsi bir arada raporlama yerleştirin. Ancak, bir üretim dışı ortamda veya sınırlı sayıda kullanıcıları olan uygulama doğrulamak için önerilir.

> [!TIP]
> AD DS olduğu gibi Azure AD Grup İlkesi yok bahsetmek önemlidir. Cihazlar için ilke uygulamak için bir mobil cihaz yönetimi çözümü aşağıdaki gibi ihtiyacınız [Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

Kullanıcının Azure AD kullanarak kimliği doğrulandıktan sonra kullanıcının erişim düzeyini değerlendirmek önemlidir. Bir kaynak üzerinde kullanıcının erişim düzeyini farklılık gösterebilir. Azure AD, bazı kaynaklara erişimi denetleyerek bir ek güvenlik katmanı ekleyebilirsiniz, ancak göz önünde bulundurun kaynak Ayrıca kendi erişim denetim listesi ayrı ayrı bir dosya sunucusunda bulunan dosyalar için erişim denetimi gibi olduğunu. Karma bir senaryoda sahip olduğunuz erişim denetimi düzeyini aşağıdaki şekilde özetlenmiştir:

![Erişim denetimi](./media/plan-hybrid-identity-design-considerations/accesscontrol.png)

Şekil X gösterdi diyagramdaki her etkileşim, Azure AD tarafından kapsanan bir erişim denetimi senaryoyu temsil eder. Aşağıda, her senaryonun açıklaması vardır:

1. Uygulamalar için koşullu erişim şirket içinde barındırılan: Kayıtlı cihazlar, Windows Server 2012 R2 ile AD FS kullanmak üzere yapılandırılan uygulamalar için erişim ilkeleri ile kullanabilirsiniz.

2. Azure portalına erişim denetimi:  Azure da olanak tanır, rol tabanlı erişim denetimi (RBAC) kullanarak portalına erişim denetimi). Bu yöntem, bir kişi Azure Portalı'nda gerçekleştirebileceğiniz işlemlerin sayısını sınırlamak şirket sağlar. Portal erişimini denetlemek için RBAC kullanarak, BT yöneticileri aşağıdaki erişim yönetimi yaklaşımlardan kullanarak erişim devredebilirsiniz:

   - Grup tabanlı rol ataması: Erişim, yerel Active Directory'nizden eşitlenebilen Azure AD gruplarına atayabilirsiniz. Bu, kuruluşunuz, araçları ve grupları yönetmek için işlemlerdeki yaptı mevcut yatırımlardan yararlanma sağlar. Azure AD Premium Temsilcili Grup Yönetimi özelliğini de kullanabilirsiniz.
   - Yerleşik rolleri Azure üzerinde kullanın: Üç rol kullanabilirsiniz — sahibi, katkıda bulunan ve okuyucu, kullanıcılar ve gruplar yalnızca kullanıcıların işlerini yapmak için ihtiyaç duydukları görevleri gerçekleştirme izniniz olduğundan emin olun.
   -  Kaynakları için ayrıntılı erişim: Kullanıcılar ve gruplar için belirli bir abonelikte, kaynak grubu veya bir Web sitesi veya veritabanı gibi ayrı bir Azure kaynak rolleri atayabilirsiniz. Bu şekilde, kullanıcıların ihtiyaç duydukları tüm kaynaklara erişim ve yönetmek için gerekmeyen kaynaklara erişemez olmasını sağlayabilirsiniz.

   > [!NOTE]
   > Uygulamaları oluşturmak ve bunlar için erişim denetimi özelleştirmek istiyorsanız, ayrıca Azure AD uygulama rolleri için yetkilendirme kullanmak da mümkündür. Bu gözden [WebApp RoleClaims DotNet örnek](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) nasıl bu özellikten yararlanabilmek için uygulamanızı oluşturun.


3. Office 365 uygulamaları için koşullu erişim Intune:  BT yöneticileri bilgi çalışanlarının hizmetlere erişmek için uyumlu cihazlara izin verme aynı zamanda kurumsal kaynakların güvenliğini sağlamak için koşullu erişim cihaz ilkeleri sağlayabilirsiniz. 
  
4. Saas uygulamaları için koşullu erişim: [Bu özellik](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work/) uygulama başına multi-Factor authentication erişim kuralları ve güvenilen bir ağda olmayan kullanıcılar için erişimi engelleme yapılandırmanıza olanak tanır. Uygulama ya da yalnızca belirtilen güvenlik gruplardaki kullanıcılar için atanmış olan tüm kullanıcılar için multi-Factor authentication kuralları uygulayabilirsiniz. Uygulamanın, kuruluşun içindeki ağ bir IP adresinden erişiyorsanız kullanıcıları çok faktörlü kimlik doğrulaması gereksinimden atlanabilir.

Erişim denetimi için seçenekleri çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma kazanılan bu görev için geçerli değildir. Kaynaklarınıza erişimini denetlemek gereken her senaryo için kullanılabilen tüm seçenekleri yararlanarak emin olun.

## <a name="define-incident-response-options"></a>Olay yanıtı seçeneklerini tanımlayın
Azure AD'ye nasıl yardımcı olabileceğine BT ortamında kullanıcı etkinliğini izleyerek olası güvenlik risklerine kimlik. BT işlemlerini kuruluş dizininizle güvenliğini ve bütünlüğünü görünürlük elde etmek için kullanım raporları ve Azure AD erişim kullanabilirsiniz. Bu bilgileri, bir BT yöneticisi, bu riskleri azaltmak yeterince planlayabilirsiniz böylece olası güvenlik risklerini olduğu şekilde daha iyi belirleyebilirsiniz.  [Azure AD Premium aboneliği](../fundamentals/active-directory-get-started-premium.md) etkinleştirebilirsiniz güvenlik raporları içerir. Bu bilgileri almak için BT. [Azure AD raporlar](../reports-monitoring/overview-reports.md) gibi kategorilere ayrılır:

* **Anomali raporları**: Anormal karşılanmadığı oturum açma olaylarını içerir. Hedef tür etkinlik haberdar olun ve bir olay şüpheli olup olmadığı hakkında bir bulguyu olanak sağlamaktır.
* **Tümleşik uygulama raporu**: Kuruluşunuzda bulut uygulamalarının nasıl kullanıldığını etkilendiğine. Azure Active Directory Tümleştirme ile binlerce bulut uygulamasına sunar.
* **Hata raporlarını**: Dış uygulama hesaplarına sağlanırken oluşabilecek hatalar gösterir.
* **Kullanıcıya özel raporları**: Cihaz/oturum belirli bir kullanıcının etkinlik verilerini görüntüler.
* **Etkinlik günlükleri**: Son 24 saat, son 7 gün veya son 30 gün hem de Grup etkinlik değişiklikleri ve parola sıfırlama ve kayıt etkinliği içinde denetlenen tüm olayların bir kaydını içerir.

> [!TIP]
> Bir servis talebi üzerinde çalışma olay yanıtı ekibi ayrıca yardımcı olabilecek başka bir rapor [sızdırılan kimlik bilgilerine sahip kullanıcı](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials/) rapor. Bu rapor, sızan kimlik bilgileri listesi ve kiracınız arasında eşleşmeleri ortaya çıkarır.
>


Diğer önemli yerleşik raporları, bir olay yanıtı araştırması sırasında kullanılabilir ve Azure AD'de:

* **Parola sıfırlama etkinliği**: yönetici parola sıfırlama kuruluşta kullanılmakta ne kadar etkin eriştiğine içine öngörülerle sağlayın.
* **Parola sıfırlama kayıt etkinlik**: hangi kullanıcıların parola sıfırlama için kendi yöntemlerini kaydettiğiniz Öngörüler sağlar ve hangi yöntemlerin seçili.
* **Grup etkinlik**: değişikliklerin geçmişini, gruba sağlar (örn: eklenen veya kaldırılan kullanıcılar) erişim Paneli'nde başlatılan.

Temel raporlama yeteneği de bir olay yanıtı araştırma işlemi sırasında BT kullanan bir Azure AD Premium ek olarak aşağıdaki gibi bilgileri almak için Denetim raporu yararlanın:

* Rol üyeliği (örneğin, kullanıcı genel Yönetici rolüne eklenen) değişiklikleri
* Kimlik bilgisi güncelleştirmeleri (örneğin, parola değişikliklerini)
* Etki alanı yönetimi (örneğin, bir etki alanı kaldırılıyor özel bir etki alanı doğrulama)
* Ekleme veya uygulamaları kaldırma
* Kullanıcı Yönetimi (örneğin, ekleme, kaldırma, bir kullanıcı güncelleştiriliyor)
* Ekleme veya kaldırma lisansları

Bu seçenekler arasında karşılaştırma seçenekleri olay yanıtı için çok katmanlı bir yaklaşım kullandığından, bu görev için geçerli değil. Azure AD raporlama özelliği, şirketinizin olay yanıtı işleminin bir parçası kullanmanızı gerekli hale getirmiş her senaryo için kullanılabilir tüm seçenekleri yararlanarak emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik yönetimi görevleri belirleme](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)
