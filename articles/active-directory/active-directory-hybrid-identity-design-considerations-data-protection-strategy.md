---
title: Karma kimlik tasarımı - veri koruma stratejisini Azure | Microsoft Docs
description: Tanımladığınız iş gereksinimlerini karşılamak karma kimlik çözümünü veri koruma stratejisini tanımlayın.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: f0def105997213ae5d356de89e6189b6441facbd
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295578"
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Karma kimlik çözümü için veri koruma stratejisini tanımlayın
Bu görevde tanımlanan iş gereksinimlerini karşılamak karma kimlik çözümünü veri koruma stratejisini tanımlayın:

* [Veri koruma gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [İçerik Yönetimi gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Erişim denetimi gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Olay yanıtlama gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Veri koruma seçeneklerini tanımlayın
İçinde anlatıldığı gibi [dizin eşitleme gereksinimlerini belirlemek](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD, şirket içi Active Directory etki alanı Hizmetleri ile (AD DS) eşitleyebilir. Bu tümleştirme kuruluşlar kullanım kurumsal kaynaklara erişmeye çalışırken kullanıcıların kimlik bilgilerini doğrulamak için Azure AD sağlar. Her iki senaryo için bunu yapabilirsiniz: veri rest şirket içi ve bulut. Azure AD veri erişimi bir güvenlik belirteci hizmeti (STS) aracılığıyla kullanıcı kimlik doğrulaması gerektirir.

Kimlik doğrulaması yapıldıktan sonra kullanıcı asıl adı (UPN) kimlik doğrulama belirtecinden okunur. Ardından, yetkilendirme sistemi çoğaltılmış bölüm ve kullanıcının etki alanı için karşılık gelen kapsayıcı belirler. Ardından bilgileri kullanıcının varlığı, etkin durumunu ve rol hedef Kiracı erişimi kullanıcı için bu oturumda yetkilendirilip yetkilendirilmediğini belirlemek yetkilendirme sistemi yardımcı olur. Yetkili belirli eylemleri (özellikle, kullanıcı ve parola sıfırlama Oluştur) kullanan bir kiracı Yöneticisi daha sonra uyumluluk çaba ya da araştırmalar yönetmek için bir denetim izi oluşturun.

Azure Storage, şirket içi veri merkezine Internet bağlantısı üzerinden taşıma verileri her zaman veri birimi, bant genişliği kullanılabilirliğini veya diğer noktalar nedeniyle uygulanabilir olmayabilir. [Azure depolama içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) yerleştirme ve alma büyük miktarda veriyi blob depolama için bir donanım tabanlı seçeneği sağlar. Göndermenize olanak sağlayan [BitLocker şifrelenmiş](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) sabit disk sürücülerine burada bulut operatörleri depolama hesabınıza içeriği karşıya yükleyebilir veya sürücülerinizin döndürmek için Azure verilerinizi karşıdan yükleyebileceğiniz doğrudan Azure veri merkezi. Yalnızca şifrelenmiş diskleri (Proje Kurulumu sırasında hizmetin kendisi tarafından oluşturulan bir BitLocker anahtarını kullanarak) Bu işlem için kabul edilir. BitLocker anahtarını Azure'a böylece bant anahtar paylaşımı dışında sağlayan ayrı ayrı sağlanır.

Aktarımdaki verileri farklı senaryolarında gerçekleştirilebildiği olduğundan, aynı zamanda Microsoft Azure kullandığını bilmek uygundur [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) birbirinden, ana bilgisayar ve Konuk düzeyinde güvenlik duvarları, IP paket filtreleme, bağlantı noktası engelleme ve HTTPS uç noktaları gibi önlemleri kullanan Kiracı trafiğini yalıtmak için. Ancak, Azure'nın iç iletişimler, altyapı altyapı ve altyapı müşteri (şirket) dahil olmak üzere çoğu de şifrelenir. Azure veri merkezleri içinde iletişim başka bir önemli bir senaryo ise; Microsoft hiçbir VM taklit veya başka bir IP adresinde misafiri güvence altına almak için ağlarını yönetir. TLS/SSL, Azure Storage veya SQL veritabanları erişirken veya Bulut hizmetlerine bağlanırken kullanılır. Bu durumda, müşteri yönetici TLS/SSL sertifika edinme ve Kiracı altyapılarını dağıtma sorumludur. Sanal makineler aynı dağıtımda veya Microsoft Azure sanal ağı aracılığıyla tek bir dağıtımda kiracılar arasında trafiği verilerini taşıma HTTPS, SSL/TLS veya diğerleri gibi şifreli iletişim protokolleri üzerinden korunabilir.

Nasıl sorulara verdiğiniz yanıtlara bağlı olarak [veri koruma gereksinimlerini belirlemek](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), verilerinizi korumak istediğiniz nasıl ve ne karma kimlik çözümü, bu işlemle yardımcı olabilir belirlemek mümkün olması gerekir. Aşağıdaki tabloda, her veri koruma senaryosu için kullanılabilir olan Azure tarafından desteklenen seçenekler gösterilmektedir.

| Veri koruma seçenekleri | Buluttaki bekleyen | REST şirket içi | Aktarım sırasında |
| --- | --- | --- | --- |
| BitLocker Sürücü Şifrelemesi |X |X | |
| SQL Server'ın veritabanları şifrele |X |X | |
| VM VM şifreleme | | |X |
| SSL/TLS | | |X |
| VPN | | |X |

> [!NOTE]
> Okuma [uyumluluk özelliği tarafından](https://azure.microsoft.com/support/trust-center/services/) adresindeki [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) her Azure hizmeti ile uyumludur sertifikalar hakkında daha fazla bilgi edinmek için.
> Veri koruma seçeneklerini çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma olmayan bu görev için uygulanabilir. Verileri her durum için kullanılabilir tüm seçenekleri yararlanarak emin olun.
>
>

## <a name="define-content-management-options"></a>İçerik yönetimi seçeneklerini tanımlayın
Karma kimlik altyapınızı yönetmek için Azure AD kullanmanın bir avantajı, işlem son kullanıcının açısından tamamen saydam olmasıdır. Kullanıcı bir paylaşılan kaynağa erişmeye çalışırsa, kaynak kimlik doğrulaması gerektirir, belirteç edinme ve kaynağa erişmek için Azure AD kimlik doğrulama isteği göndermek kullanıcının sahip. Tüm bu işlem, kullanıcı etkileşimi olmadan arka planda gerçekleşir. İzni vermek mümkündür bir [grup](fundamentals/active-directory-manage-groups.md#getting-started-with-access-management) ortak belirli eylemleri gerçekleştirmek üzere bunları izin vermek üzere kullanıcı.

Genellikle veri gizliliğiyle ilgili endişeleri olan kuruluşlar veri sınıflandırması için kendi çözümü gerektirir. Geçerli şirket içi altyapısını veri sınıflandırması kullanıyorsa, Azure AD için kullanıcının kimliğini ana deposu olarak kullanmak da mümkündür. Veri Sınıflandırması adlı için kullanılan şirket içi olduğundan emin genel bir aracı [veri sınıflandırma Araç Seti](https://msdn.microsoft.com/library/Hh204743.aspx) Windows Server 2012 R2 için. Bu araç tanımlamak, sınıflandırmak ve özel bulutunuzdaki dosya sunucularındaki verileri korumaya yardımcı olabilir. Kullanmak da mümkündür [otomatik dosya sınıflandırma](https://technet.microsoft.com/library/hh831672.aspx) bu görevi gerçekleştirmek için Windows Server 2012'de.

Kuruluşunuzun veri sınıflandırma yerinde sahip değil, ancak yeni sunucuları şirket içi eklemenize gerek kalmadan gizli dosyaları korumak gereken, Microsoft kullanabileceklerini [Azure Rights Management hizmeti](https://technet.microsoft.com/library/JJ585026.aspx).  Dosyaları ve e-posta güvenli hale getirmek için Azure RMS kullanan şifreleme, kimlik ve yetkilendirme ilkelerini ve birden çok cihazda çalıştığı — telefonları, Tablet ve bilgisayar. Azure RMS bir bulut hizmeti olduğundan, korumalı içeriği kendileriyle paylaşabilmek için öncelikle güvenleri diğer kuruluşlarla açıkça yapılandırmanıza gerek yoktur. Bir Office 365 veya Azure AD dizini zaten varsa, kuruluşlar arası işbirliği otomatik olarak desteklenir. Ayrıca, yalnızca Azure RMS için Azure Active Directory Eşitleme Hizmetleri (AAD eşitleme) veya Azure AD Connect kullanarak şirket içi Active Directory hesaplarınız için ortak bir kimliği desteklemek için gereken dizin özniteliklerini eşitleyebilirsiniz.

İçerik Yönetimi sürecinin hayati bir parçası kimin hangi kaynağın erişmekte olduğunu anlamak için bu nedenle kimlik yönetimi çözümü için zengin günlüğe kaydetme özelliğine önemlidir. Azure AD günlük dahil olmak üzere 30 gün sağlar:

* Rol üyelik değişiklikleri (örn: kullanıcı genel Yönetici rolüne eklenen)
* Kimlik bilgisi güncelleştirmeleri (örn: parola değişikliklerini)
* Etki alanı yönetimi (örn: bir etki alanı kaldırma özel bir etki alanı doğrulama)
* Ekleme veya uygulamaları kaldırma
* Kullanıcı Yönetimi (örn: ekleme, kaldırma, bir kullanıcı güncelleştirme)
* Ekleme veya lisanslarını kaldırma

> [!NOTE]
> Okuma [Microsoft Azure güvenlik ve Denetim Günlüğü Yönetimi](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) Azure günlüğe kaydetme özellikleri hakkında daha fazla bilgi edinmek için.
> Nasıl sorulara verdiğiniz yanıtlara bağlı olarak [içerik yönetimi gereklilikleri](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), karma kimlik çözümünüzde yönetilmesi için içeriğin nasıl istediğiniz belirlemek mümkün olması gerekir. Tablo 6'kullanıma sunulan tüm seçenekleri Azure AD ile tümleştirilebilen olsa da, iş gereksinimleriniz için daha uygun olduğu tanımlamak önemlidir.
>
>

| İçerik yönetimi seçenekleri | Avantajları | Olumsuz yönleri |
| --- | --- | --- |
| Şirket içinde Merkezi (Active Directory Rights Management sunucusu) |Verileri sınıflandırmak için sorumlu sunucu altyapısı üzerinde tam denetim <br> Yerleşik yetenek Windows Server'daki ek lisans ya da abonelik gerek yoktur <br> Karma bir senaryoda Azure AD ile tümleşik <br> Exchange Online ve SharePoint Online ve Office 365 gibi Microsoft Online Services Bilgi Hakları Yönetimi (IRM) özelliklerini destekler <br> Exchange Server, SharePoint Server ve Windows Server ve dosya sınıflandırma altyapısı (FCI) çalıştıran dosya sunucuları gibi şirket içi Microsoft sunucu ürünlerini destekler. |Bu yana daha yüksek Bakım (Canlı yukarı güncelleştirmeler, yapılandırma ve olası yükseltmeleri), BT sunucu sahibi <br> Şirket içi sunucu altyapısı gerektirir<br> Yerel olarak Azure özelliklerden yararlanacak değil |
| (Azure RMS) bulutta Merkezi |Yönetilmesi daha kolay şirket içi çözüm ile karşılaştırıldığında <br> Karma bir senaryoda AD DS ile tümleştirilir. <br>  Azure AD ile tamamen tümleşik <br> Bir sunucu hizmeti dağıtmak için şirket içi gerektirmez <br> Destekleyen şirket içi Microsoft Exchange Server, SharePoint, sunucu ve Windows Server ve dosya sınıflandırma altyapısı (FCI) çalıştıran dosya sunucuları gibi sunucu ürünleri <br> BT, BYOK yeteneği kiracısının anahtarıyla tam denetime sahip olabilir. |Kuruluşunuz RMS'yi destekleyen bir bulut aboneliğine sahip olmalıdır <br> Kuruluşunuz için RMS kullanıcı kimlik doğrulamasını desteklemek için Azure AD dizini olmalıdır |
| Karma (Azure RMS ile tümleşik, şirket içi Active Directory Rights Management sunucusu) |Bu senaryo iki, merkezi şirket içinde ve bulutta avantajları birikir. |Kuruluşunuz RMS'yi destekleyen bir bulut aboneliğine sahip olmalıdır <br> Kuruluşunuz için RMS kullanıcı kimlik doğrulamasını desteklemek için Azure AD dizini olmanız gerekir. <br> Azure bulut hizmeti arasında bir bağlantı gerektirir ve şirket içi altyapı |

## <a name="define-access-control-options"></a>Erişim denetim seçeneklerini tanımlayın
Kimlik doğrulama, yetkilendirme ve erişim denetimi özelliklerinden Azure AD'de kullanılabilen yararlanarak kullanıcıların sağlarken merkezi kimlik deposu kullanmak, şirketinizin etkinleştirebilirsiniz ve tek kullanmak üzere iş ortakları oturum açma (SSO) aşağıdaki resimde gösterildiği gibi özelliğini:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Merkezi Yönetim'i ve tam olarak diğer dizinleri ile tümleştirme

Şirket içi web uygulamaları ve Azure Active Directory binlerce SaaS uygulamasına çoklu oturum açma sağlar. Bkz: [Azure Active Directory Federasyon Uyumluluğu Listesi: çoklu oturum açmayı uygulamak için kullanılan üçüncü taraf kimlik sağlayıcıları](https://msdn.microsoft.com/library/azure/jj679342.aspx) Microsoft tarafından test edilmiş SSO üçüncü taraf hakkında daha fazla ayrıntı için makalenin. Bu özellik, kimlik ve erişim yönetimi denetimin tutarken B2B senaryolarını çeşitli uygulama kuruluşun sağlar. Ancak, B2B sırasında işlemini tasarlama iş ortağı tarafından kullanılan kimlik doğrulama yöntemini anlamak ve bu yöntem Azure tarafından desteklenip desteklenmediğini doğrulamak önemlidir. Şu anda, aşağıdaki yöntemlerden Azure AD tarafından desteklenir:

* Güvenlik onaylama işlemi biçimlendirme dili (SAML)
* OAuth
* Kerberos
* Belirteçler
* Sertifikalar

> [!NOTE]
> Okuma [Azure Active Directory kimlik doğrulama protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx) her protokolü ve Azure özelliklerini hakkında daha fazla ayrıntı bilmek.
>
>

Azure AD desteği kullanarak, mobil iş uygulamaları, çalışanların kendi mobil uygulamalar kullanıcıların şirket Active Directory kimlik bilgileriyle oturum izin vermek için aynı kolay Mobile Services kimlik doğrulama deneyimi kullanabilirsiniz. Bu özellik ile Mobile Services'ın kimlik sağlayıcısı (hangi Microsoft Accounts, Facebook kimliği, Google kimliği ve Twitter kimliği dahil) zaten desteklenen diğer kimlik sağlayıcılardan yanı sıra Azure AD desteklenir. Şirket içi uygulamalar şirket AD DS bulunan kullanıcının kimlik bilgileri kullanıyorsanız, iş ortakları ve buluttan gelen kullanıcıların erişimden saydam olmalıdır. Kullanıcının koşullu erişim denetimi (bulut tabanlı) web uygulamaları, web API, Microsoft bulut Hizmetleri, üçüncü taraf SaaS uygulamaları ve yerel (mobil) istemci uygulamaları yönetme ve güvenlik, avantajları sahip denetimi, hepsi bir raporlama yerleştirin. Ancak, bir üretim dışı ortamında veya sınırlı sayıda kullanıcının uygulama doğrulamak için önerilir.

> [!TIP]
> AD DS olduğu gibi Azure AD Grup İlkesi yok anlatılması önemlidir. Cihazlar için ilke zorlamak için bir mobil cihaz yönetimi çözümü gibi ihtiyacınız [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

Kullanıcı Azure AD kullanarak kimlik doğrulaması gerçekleştikten sonra kullanıcının sahip olduğu erişim düzeyini değerlendirmek önemlidir. Kullanıcının kaynak üzerinde sahip olduğu erişim düzeyini farklılık gösterebilir. Azure AD bazı kaynaklara erişim denetleyerek bir ek güvenlik katmanı ekleyebilirsiniz, ancak göz önünde bulundurmanız kaynak Ayrıca kendi erişim denetim listesi ayrı ayrı bir dosya sunucusunda bulunan dosyalar için erişim denetimi gibi bulunabilir. Aşağıdaki şekil, karma bir senaryoda olabilir erişim denetimi düzeylerini özetlenmektedir:

![](./media/hybrid-id-design-considerations/accesscontrol.png)

Şekil X gösterdi diyagramdaki her etkileşim Azure AD tarafından kapsanan bir erişim denetimi senaryosunun temsil eder. Aşağıda, her senaryonun açıklaması vardır:

  1. Şirket içinde barındırılan uygulamalara koşullu erişim: Windows Server 2012 R2 ile AD FS kullanmak üzere yapılandırılan uygulamalar için erişim ilkeleri ile kayıtlı cihazları kullanabilirsiniz. Şirket içi uygulamalara koşullu erişimi ayarlama hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory Cihaz Kaydı hizmetini kullanarak Şirket İçi Uygulamalara Koşullu Erişim](active-directory-conditional-access-azure-portal.md).

  2. Azure portalına erişim denetimi: Azure de olanak tanır, rol tabanlı erişim denetimi (RBAC) kullanarak portalına erişim denetim). Bu yöntem tek bir Azure portalında yapabileceğiniz işlemleri sayısını sınırlamak şirket sağlar. Portal erişimini denetlemek için RBAC kullanarak, BT yöneticilerinin aşağıdaki erişim yönetimi yaklaşımlar kullanılarak erişim atayabilirsiniz:

   - Grup tabanlı rol ataması: erişim eşitlenebilen Azure AD grupları yerel Active Directory'den atayabilirsiniz. Bu, kuruluşunuzun araçları ve grupları yönetmek için işlemlerdeki yaptı Yatırımlar yararlanan sağlar. Azure AD Premium temsilci Grup Yönetimi özelliği de kullanabilirsiniz.
   - Azure içinde yerleşik rolleri kullanır: üç rol kullanabilirsiniz — sahibi, katkıda bulunan ve okuyucu, kullanıcılar ve gruplar yalnızca işlerini gerçekleştirmek için ihtiyaç duydukları görevleri gerçekleştirme izniniz olduğundan emin olun.
   -  Kaynaklar için ayrıntılı erişim: Kullanıcıları ve grupları belirli bir abonelik için kaynak grubu veya bir Web sitesi veya veritabanı gibi ayrı bir Azure kaynağı roller atayabilirsiniz. Bu şekilde, kullanıcıların ihtiyaç duydukları tüm kaynaklara ve yönetmek zorunda kalmazsınız kaynaklara erişemez sahip olduğunuzdan emin olabilirsiniz.

   > [!NOTE]
   > Uygulamaları oluşturmak ve bunları için erişim denetimi özelleştirmek istediğiniz, ayrıca yetkilendirme için Azure AD uygulama rolleri kullanmak da mümkündür. Bu gözden [WebApp RoleClaims DotNet örnek](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) nasıl bu özellikten yararlanabilmek için uygulamanızı oluşturun.


  3. Microsoft Intune ile Office 365 uygulamaları için koşullu erişim: BT yöneticileri bilgi çalışanlarının hizmetlere erişmek için uyumlu cihazlara izin verme aynı anda sırada şirket kaynaklarına güvenli hale getirmek için koşullu erişim cihaz ilkeleri sağlayabilirler. Daha fazla bilgi edinmek için bkz. [Office 365 hizmetleri için Koşullu Erişim Cihaz İlkeleri](active-directory-conditional-access-device-policies.md).

  4. Saas uygulamaları için koşullu erişim: [bu özellik](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work/) , uygulama başına çok faktörlü kimlik doğrulama erişim kuralları ve güvenilir bir ağda değil, kullanıcılar için erişimi engelleme özelliği yapılandırmanıza olanak sağlar. Uygulama ya da yalnızca belirtilen güvenlik gruplarının içinde kullanıcılar için atanmış tüm kullanıcılar için çok faktörlü kimlik doğrulama kurallarını uygulayabilirsiniz. Kullanıcılar, kuruluşun içindeki ağ bir IP adresinden uygulama erişiyorsa çok faktörlü kimlik doğrulaması gereksinimden atlanabilir.

Erişim denetimi için seçenekleri çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma olmayan bu görev için uygulanabilir. Kaynaklarınıza erişimi denetlemek gerektiren her senaryo için kullanılabilir tüm seçenekleri yararlanarak emin olun.

## <a name="define-incident-response-options"></a>Olay yanıtlama seçeneklerini tanımlayın
Azure AD yardımcı olabilir kimlik olası güvenlik risklerini kullanıcının etkinliğini izleme tarafından ortamında DÖNÜŞTÜRÜLEMİYOR. BT ve kullanım raporları bütünlük ve kuruluşunuzun dizin güvenliğini görünürlük elde etmek için Azure AD erişim kullanabilirsiniz. Bu bilgi ile bir BT yöneticisi böylece bunlar bu riskleri azaltmak yeterli planlayabilirsiniz olası güvenlik riskleri burada bulunan daha iyi belirleyebilirsiniz.  [Azure AD Premium aboneliği](fundamentals/active-directory-get-started-premium.md) etkinleştirebilirsiniz güvenlik raporları ayarlanmış bu bilgileri almak için BT. [Azure AD raporları](active-directory-view-access-usage-reports.md) aşağıdaki gibi kategorilere ayrılır:

* **Kural dışı durum raporları**: anormal bulundu oturum açma olayları içerir. Bu tür etkinlik farkında olun ve bir olay şüpheli olup olmadığı hakkında bir belirleme yapmanızı etkinleştirmek için belirtilir.
* **Uygulama rapor tümleşik**: Bulut uygulamalarını, kuruluşunuzda nasıl kullanıldığını içine Öngörüler sağlar. Azure Active Directory bulut uygulamalarını binlerce ile tümleştirme sağlar.
* **Hata raporlarını**: dış uygulamalara hesap sağlamada oluşabilecek hatalar gösterir.
* **Kullanıcıya özgü raporları**: Aygıt/oturum belirli bir kullanıcı için etkinlik verileri görüntüleyebilirsiniz.
* **Etkinlik günlükleri**: Son 24 saat, son 7 gün veya son 30 Günün yanı sıra grup etkinlik değişikliklerini ve parola sıfırlama ve kayıt etkinliği içinde tüm Denetlenen olayları kaydını içerir.

> [!TIP]
> Ayrıca bir servis talebi üzerinde çalışan olay yanıtı takım yardımcı olabilecek başka bir rapordur [sızan kimlik bilgilerine sahip kullanıcı](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials/) rapor. Bu rapor, sızan kimlik bilgileri listesi ve Kiracı arasında eşleşmeleri ortaya çıkarır.
>


Diğer önemli yerleşik raporları bir olay yanıtlama İnceleme sırasında kullanılabilir ve Azure AD'de:

* **Parola sıfırlama etkinliği**: ne kadar etkin parola sıfırlama kuruluşunuzda kullanılan içine ınsights ile yönetim sağlar.
* **Parola sıfırlama kayıt etkinlik**: içine kullanıcılar parola sıfırlama için kendi yöntemlerini kaydettirdiğiniz Öngörüler sağlar ve hangi yöntemlerin seçili.
* **Grup etkinlik**: gruba değişikliklerin geçmişini sağlar (örn: eklenen veya kaldırılan kullanıcılar) erişim panelinde başlatılan.

Bir olay yanıtı araştırma işlemi sırasında BT de kullanabilirsiniz, Azure AD Premium çekirdek raporlama özelliği yanı sıra gibi bilgi edinmek için Denetim raporun yararlanın:

* Rol üyeliğini (örneğin, kullanıcı genel Yönetici rolüne eklenen) değişiklikleri
* Kimlik bilgisi güncelleştirmeleri (örneğin, parola değişikliklerini)
* Etki alanı yönetimi (örneğin, bir etki alanı kaldırma özel bir etki alanı doğrulama)
* Ekleme veya uygulamaları kaldırma
* Kullanıcı Yönetimi (örneğin, ekleme, kaldırma, bir kullanıcı güncelleştirme)
* Ekleme veya lisanslarını kaldırma

Olay yanıtlama seçeneklerini çok katmanlı bir yaklaşım kullandığından, bu seçenekler arasında karşılaştırma bu görev için geçerli değildir. Azure AD raporlama özelliği, şirketinizin olay yanıtlama işleminin bir parçası kullanılmasını gerektirir her senaryo için kullanılabilir tüm seçenekleri yararlanarak emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik yönetimi görevleri belirleme](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)
