---
title: SaaS uygulama Azure AD'de kullanıcı sağlamayı otomatik | Microsoft Docs
description: Otomatik olarak sağlamak için Azure AD'ye nasıl kullanabileceğinizi giriş sağlamasını ve kullanıcı hesapları arasında birden çok üçüncü taraf SaaS uygulamaları sürekli olarak güncelleştirin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: 8a84f2f13318dea5c2b99af0b880f2adb1343c8d
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042794"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Sağlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına kullanıcı otomatikleştirin
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>SaaS uygulamaları için kullanıcı sağlamayı otomatik nedir?
Azure Active Directory (Azure AD), oluşturma, Bakım ve bulutta kullanıcı kimliklerini kaldırılmasını otomatik hale getirmenizi sağlar ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) Dropbox, Salesforce, ServiceNow ve diğer uygulamalar.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**Bu özellik, hangi sağlar ilişkin bazı örnekler aşağıdadır:**

* Otomatik olarak ekip veya kuruluş katıldığında yeni kişiler için doğru sistemlerinde yeni hesapları oluşturun.
* Otomatik olarak kişiler ekip veya kuruluş çıktığınızda doğru sistemlerinde hesapları devre dışı bırakın.
* Uygulamalarınızı ve sistemlerinizi kimliklerini directory ya da İnsan Kaynakları sisteminizi değişikliklere göre güncel tutulduğundan emin olun.
* Gruplara, bunları destekleyen uygulamalar gibi kullanıcı dışındaki nesneler sağlayın.

**Otomatik kullanıcı hazırlama, aşağıdaki işlevleri içerir:**

* Kaynak ve hedef sistemleri arasında mevcut kimlikleri eşleştirmek yeteneği.
* Hangi kullanıcı verilerini tanımlayan özelleştirilebilir öznitelik eşlemelerini kaynak sistemden hedef sisteme akışı.
* Sağlama hataları için isteğe bağlı bir e-posta uyarıları
* İzleme ve sorun giderme Yardımı için raporlama ve etkinlik günlükleri.

## <a name="why-use-automated-provisioning"></a>Otomatik sağlama neden kullanmalısınız?
Bu özelliği kullanmak için bazı ortak motivasyonlardan şunlardır:

* Maliyetleri, verimsiz ve el ile sağlama işlemleriyle ilişkili İnsan hatası önleme.
* Barındırma ve özel geliştirilmiş sağlama çözümleri ve betikleri ile ilişkili maliyetleri engelleme
* Kuruluştan ayrılan zaman anında kullanıcıların kimliklerini anahtar SaaS uygulamalarından kaldırarak, kuruluşunuzun güvenliğini sağlama.
* Çok sayıda kullanıcı belirli bir SaaS uygulama veya sistem kolayca içeri aktarmak için.
* İlkeleri kullanan bir uygulamada oturum açabilir ve kimin sağlanan belirlemek için tek bir dizi zorunda için.

## <a name="how-does-automatic-provisioning-work"></a>Otomatik sağlamayı nasıl çalışır?
    
**Azure AD sağlama hizmeti** kullanıcıların SaaS uygulamaları ve diğer sistemler her uygulama satıcısı tarafından sağlanan kullanıcı yönetim API uç noktaları bağlanarak sağlar. Bu kullanıcı yönetim API uç noktaları, programlı olarak oluşturmak, güncelleştirmek ve kullanıcıları kaldırmak Azure AD verin. Sağlama hizmeti da oluşturabilirsiniz, seçili uygulamalar için güncelleştirme ve gruplar ve roller gibi ek kimlikle ilgili nesneleri kaldırın. 

![Sağlama](./media/user-provisioning/provisioning0.PNG)
*Şekil 1: Azure AD sağlama hizmeti*

![Giden sağlama](./media/user-provisioning/provisioning1.PNG)
*Şekil 2: "Çıkış" kullanıcı popüler SaaS uygulamaları için Azure ad iş akışı sağlama*

![Gelen sağlama](./media/user-provisioning/provisioning2.PNG)
*Şekil 3: "Giriş" kullanıcı Azure Active Directory ve Windows Server Active Directory için popüler İnsan büyük Yönetim (HCM) uygulamalarından iş akışı sağlama*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Hangi uygulamalar ve sistemler Azure AD'ye otomatik kullanıcı hazırlama ile kullanabilir miyim?

Azure AD özellikleri SCIM 2.0 standart belirli bölümlerini uygulayan uygulamaları için genel destek yanı sıra çeşitli popüler SaaS uygulamaları ve İnsan Kaynakları sistemleri için destek önceden tümleştirilmiş.

### <a name="pre-integrated-applications"></a>Önceden tümleştirilmiş uygulamalar
Azure AD, tüm uygulamaların önceden tümleştirilmiş sağlama bağlayıcı destekleyen bir listesi için bkz [kullanıcı sağlama için uygulama öğreticilerin listesine](../saas-apps/tutorial-list.md).

Azure AD kişiye üzerinden bir ileti gönderme mühendislik ekibinin ek uygulamalar için sağlama destek istemek için [Azure Active Directory geri bildirim Forumu](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> Otomatik kullanıcı sağlamayı desteklemek bir uygulama için sırada, gerekli kullanıcı yönetimi için dış programlarda oluşturulmasını, Bakım ve kullanıcıların kaldırılmasını otomatik hale getirmek izin API'leri ilk sağlamalısınız. Bu nedenle, tüm SaaS uygulamaları bu özellik ile uyumlu değildir. Kullanıcı Yönetimi API'leri destekleyen uygulamalar için Azure AD mühendislik ekibi ardından uygulamalarla sağlama bağlayıcı oluşturmak mümkün olacaktır ve bu iş, geçerli ve potansiyel müşterilerin gereksinimlerine göre önceliklendirilir. 

### <a name="connecting-applications-that-support-scim-20"></a>SCIM 2.0 destekleyen uygulamalar arasında bağlantı kurma
SCIM kullanan uygulamalar genel olarak bağlanma hakkında bilgi için 2.0 - tabanlı kullanıcı yönetimi API'leri bkz [kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak sağlamak için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md).

    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Bir uygulamayı otomatik sağlamayı ayarlama nasıl ayarlayabilirim?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Yapılandırma başlar seçili uygulama için sağlama hizmetini Azure AD'nin  **[Azure portalında](https://portal.azure.com)**. İçinde **Azure Active Directory > Kurumsal uygulamalar** bölümünden **Ekle**, ardından **tüm**, senaryonuza bağlı olarak aşağıdakilerden birini ekleyin:

* Tüm uygulamaları **özellikli uygulamalara** destek bölümüne otomatik sağlama. Bkz: [kullanıcı sağlama için uygulama öğreticilerin listesine](../saas-apps/tutorial-list.md) ek olanlar.

* İçin özel olarak geliştirilmiş SCIM tümleştirmeler "galeri dışı uygulama" seçeneğini kullanın

![Galeri](./media/user-provisioning/gallery.png)

Uygulama Yönetimi ekranında, sağlama yapılandırılan **sağlama** sekmesi.

![Ayarlar](./media/user-provisioning/provisioning_settings0.PNG)


* **Yönetici kimlik bilgileri** Azure AD Kullanıcı Yönetimi uygulama tarafından sağlanan API bağlanmak için olanak tanıyan hizmet sağlama için sağlanmalıdır. Bu bölümde Ayrıca, kimlik bilgileri başarısız veya sağlama işi girmeyeceğini e-posta bildirimlerini etkinleştirmek sağlar [karantina](#quarantine).

* **Öznitelik eşlemeleri** yapılandırılabilir, kaynak sistemde alanları belirtin (örnek: Azure AD) hedef sistemde hangi alanları içeriklerini eşitlemiş (örnek: ServiceNow). Hedef uygulama destekliyorsa, bu bölümde, isteğe bağlı olarak kullanıcı hesaplarını yanı sıra gruplarının sağlama yapılandırmanıza izin verir. "Eşleşen Özellikler" hangi alanların hesapları sistemleri arasında eşleştirmek için kullanılan seçmenize olanak tanır. "[İfadeleri](functions-for-customizing-application-data.md)" değiştirmek ve hedef sistemde yazıldıkları önce kaynak sisteminden alınan değerleri dönüştürme olanak sağlar. Daha fazla bilgi için [öznitelik eşlemelerini özelleştirme](customize-application-attributes.md).

![Ayarlar](./media/user-provisioning/provisioning_settings1.PNG)

* **Kapsam filtreleri** hangi kullanıcıların sağlama hizmeti söyleyin ve kaynak sistemi grubunda alabileceğiniz ve/veya hedef sisteme sağlaması kaldırıldı. Kapsam birlikte değerlendirildiğini filtreleri gereken iki unsur vardır kimin sağlama kapsamında olduğunu belirleyin:

    * **Öznitelik değerleri üzerinde filtre** -belirli bir öznitelik değerleri üzerinde filtreleme "Kaynak nesne kapsamı" menüsünde öznitelik eşlemelerini sağlar. Örneğin, yalnızca "Sales", "Departman" özniteliğine sahip kullanıcıların sağlama kapsamında olması gerektiğini belirtebilirsiniz. Daha fazla bilgi için [kapsam belirleme filtrelerini kullanma](define-conditional-rules-for-provisioning-user-accounts.md).

    * **Süzgeç atamalarını üzerinde** -Hazırlama "Scope" menüsünde > Portalı Ayarları bölümü olup yalnızca "atanan" Kullanıcılar ve gruplar sağlama kapsamında olmalıdır veya Azure AD dizini içindeki tüm kullanıcılar'ın kaldırılması gerekip gerekmediğini belirtmenizi sağlar sağlanan. "Kullanıcılar ve gruplar atama" hakkında daha fazla bilgi için bkz: [kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamak](assign-user-or-group-access-portal.md).
    
* **Ayarları** veya şu anda çalışıyor olsun dahil olmak üzere, bir uygulama için sağlama hizmetinin işlem denetimi.

* **Denetim günlükleri** sağlama hizmetini Azure AD tarafından gerçekleştirilen her işlemin kayıtlarını sağlar. Daha fazla ayrıntı için [sağlama raporlama Kılavuzu](check-status-user-account-provisioning.md).

![Ayarlar](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> Azure AD kullanıcı sağlama hizmeti de yapılandırılabilir ve yönetilen kullanarak [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>Sağlama sırasında ne olacak?

Azure AD, kaynak sistem çalışırken, sağlama hizmeti kullanan [fark sorgusu Özelliği Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) kullanıcılar ve Gruplar'ı izlemek için. Sağlama hizmeti bir ilk eşitleme, kaynak ve hedef sistemi, düzenli aralıklarla artımlı eşitlemeler tarafından izlenen karşı çalışır. 

### <a name="initial-sync"></a>İlk eşitleme
Sağlama hizmeti başlatıldığında, şimdiye kadar yapılan ilk eşitleme yapar:

1. Tüm kullanıcılar ve Gruplar'ı kaynak sistemden içinde tanımlanan tüm öznitelikleri alınırken sorgu [öznitelik eşlemelerini](customize-application-attributes.md).
2. Kullanıcıları ve yapılandırılmış kullanarak döndürülen, grupları filtre [atamaları](assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md).
3. Ne zaman atanmış bir kullanıcı bulunamadı veya sağlama için kapsam içinde hizmet belirlenmiş kullanarak eşleşen bir kullanıcı için hedef sistemde sorgular [öznitelikleri eşleşen](customize-application-attributes.md#understanding-attribute-mapping-properties). Örnek: kaynak sistemindeki userPrincipal adı eşleşen bir özniteliktir ve eşler kullanıyorsanız kaynak sistemde userPrincipal adı değerlerle eşleşen kullanıcı adları için hedef sistemde kullanıcı hedef sistemde sonra sağlama hizmeti sorgular.
4. Eşleşen kullanıcı hedef sistemde bulunamazsa, kaynak sistemden öznitelikleri kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra sağlama hizmeti algılar ve kullanıcı gelecekteki tüm işlemleri gerçekleştirmek için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
5. Eşleşen bir kullanıcı bulunamazsa, kaynak sistem tarafından sağlanan öznitelikleri kullanılarak güncelleştirilir. Kullanıcı hesabı eşleşen sonra sağlama hizmeti algılar ve kullanıcı gelecekteki tüm işlemleri gerçekleştirmek için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
6. Öznitelik eşlemeleri "başvuru" özniteliği içermiyorsa, hizmet oluşturmak ve başvurulan nesneler bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir kullanıcı bir "Yönetici" özniteliği başka bir kullanıcı hedef sistemde oluşturulan bağlı hedef sistem olabilir.
7. Filigran sonraki artımlı eşitlemeler için başlangıç noktası sağlar ilk eşitleme sonunda kalıcı hale getirin.

Yalnızca kullanıcılar sağlama, ancak grupları ve bunların üyeleri de sağlama ServiceNow, Google Apps ve kutusunu desteği gibi bazı uygulamalar. İçinde grup sağlama etkinse, bu durumlarda [eşlemeleri](customize-application-attributes.md), sağlama hizmeti kullanıcıları ve grupları eşitler ve daha sonra grubu üyeliğini eşitler. 

### <a name="incremental-syncs"></a>Artımlı eşitlemeler
İlk eşitlemeden sonra sonraki tüm eşitlemeler olur:

1. Tüm kullanıcılar ve son Filigran depolanmış beri güncelleştirildi gruplar için kaynak sistemi sorgulayın.
2. Kullanıcıları ve yapılandırılmış kullanarak döndürülen, grupları filtre [atamaları](assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md).
3. Ne zaman atanmış bir kullanıcı bulunamadı veya sağlama için kapsam içinde hizmet belirlenmiş kullanarak eşleşen bir kullanıcı için hedef sistemde sorgular [öznitelikleri eşleşen](customize-application-attributes.md#understanding-attribute-mapping-properties).
4. Eşleşen kullanıcı hedef sistemde bulunamazsa, kaynak sistemden öznitelikleri kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra sağlama hizmeti algılar ve kullanıcı gelecekteki tüm işlemleri gerçekleştirmek için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
5. Eşleşen bir kullanıcı bulunamazsa, kaynak sistem tarafından sağlanan öznitelikleri kullanılarak güncelleştirilir. Eşleşen yeni atanmış bir hesabınız varsa, sağlama hizmetine algılar ve kullanıcı gelecekteki tüm işlemleri gerçekleştirmek için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
6. Öznitelik eşlemeleri "başvuru" özniteliği içermiyorsa, hizmet oluşturmak ve başvurulan nesneler bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir kullanıcı bir "Yönetici" özniteliği başka bir kullanıcı hedef sistemde oluşturulan bağlı hedef sistem olabilir.
7. Daha önce sağlama kapsamında olan bir kullanıcı (atanmamış dahil) kapsamdan kaldırdıysanız, hizmetin kullanıcı hedef sistemdeki bir güncelleştirme aracılığıyla devre dışı bırakır.
8. Daha önce sağlama kapsamında olan bir kullanıcı devre dışı ya da geçici olarak silinen kaynak sistemde hizmeti kullanıcı hedef sistemdeki bir güncelleştirme aracılığıyla devre dışı bırakır.
9. Daha önce sağlama kapsamında olan bir kullanıcı silindi sabit kaynak sistemde varsa hizmet hedef sistemde kullanıcıyı siler. Azure AD'de kalıcı olarak silinmiş, geçici olarak silinen sonraki 30 gün kullanıcılardır.
10. Sonraki artımlı eşitlemeler için başlangıç noktası sağlar Artımlı eşitleme sonuna yeni bir filigran kalıcı hale getirin.

>[!NOTE]
> İsteğe bağlı olarak oluşturma, güncelleştirme veya silme işlemleri kullanarak devre dışı bırakabilirsiniz **hedef nesne eylemleri** onay kutularına [öznitelik eşlemelerini](customize-application-attributes.md) bölümü. Güncelleştirme sırasında bir kullanıcı devre dışı bırakmak için mantığı da "accountEnabled" gibi bir alandan bir özellik eşlemesi aracılığıyla denetlenir.

Sağlama hizmeti uçtan uca artımlı eşitlemeler, tanımlanan aralıklarla çalışmaya devam edecek [öğretici her uygulamaya özgü](../saas-apps/tutorial-list.md), aşağıdaki olaylardan biri gerçekleşene kadar:

* Azure portalını kullanarak ya da uygun Graph API'si komutunu kullanarak hizmeti el ile durduruldu 
* Kullanarak yeni bir ilk eşitleme tetiklenen **durumu temizleyin ve yeniden** seçeneği Azure portalında ya da uygun Graph API'si komutu. Bu saklı bir filigran temizler ve tüm kaynak nesneleri yeniden değerlendirilecek neden olur.
* Yeni bir ilk eşitleme kapsamı belirleme filtreleri ile öznitelik eşlemelerini de bir değişiklik nedeniyle tetiklenir. Bu ayrıca depolanan tüm Filigran temizler ve tüm kaynak nesneleri yeniden değerlendirilecek neden olur.
* Sağlama işlemi nedeniyle yüksek hata oranı (aşağıya bakın) karantina girer ve dört hafta için karantinaya kalır. Bu durumda hizmet otomatik olarak devre dışı bırakılır.

### <a name="errors-and-retries"></a>Hataları ve yeniden deneme 
Bireysel bir kullanıcı eklendiğinde, güncelleştirilemiyor veya hedef sistemdeki bir hata nedeniyle hedef sistemde silindi, işlem sonraki eşitleme döngüsünde denenecektir. Kullanıcı başarısız olmaya devam ederse, yeniden denemeler kademeli olarak gün başına yalnızca bir girişimi dön ölçeklendirme daha düşük bir sıklıkta gerçekleşecek şekilde başlar. Hatayı gidermek için Yöneticiler denetlemeniz gerekir [denetim günlükleri](check-status-user-account-provisioning.md) "emanet işleme" kök belirlemek için olayları neden ve uygun eylemi gerçekleştirin. Sık karşılaşılan hatalar içerebilir:

* Kullanıcılar hedef sistemde gerekli kaynak sistemindeki doldurulmuş bir özniteliğe sahip değil
* Kullanıcılar bir öznitelik değeri var olan benzersiz kısıtlama hedef sistemde kaynak sistemindeki ve aynı değere sahip başka bir kullanıcı kaydı var.

Bu hatalar, kaynak sistemde etkilenen kullanıcının öznitelik değerleri ayarlayarak ya da çakışmalara neden olmayan için öznitelik eşlemelerini ayarlayarak çözülebilir.   

### <a name="quarantine"></a>Karantina
Çoğu veya tüm çağrıların hedef sistemde karşı sürekli olarak yapılan (örneğin geçersiz yönetici kimlik bilgileri olduğu gibi) bir hata nedeniyle başarısız, ardından sağlama işi bir "karantina" durumuna geçtiğinde. Bu belirtilen [özet raporu sağlama](check-status-user-account-provisioning.md), e-posta bildirimleri Azure portalında yapılandırıldıysa e-posta aracılığıyla. 

Karantinadaki olduğunda, artımlı eşitlemeler sıklığını kademeli olarak günde bir kere için azaltılır. 

Sağlama işi karantinadan tüm düzeltilmekte olan soruna neden olan hataları sonra kaldırılır ve sonraki eşitleme döngüsü başlatır. Sağlama işi dört hafta için karantinaya kalırsa, sağlama işi devre dışı bırakıldı.


## <a name="how-long-will-it-take-to-provision-users"></a>Ne kadar kullanıcıları sağlama sürer?

Önceki bölümde açıklandığı gibi performansı sağlama iş bir ilk eşitleme ya da bir artımlı eşitleme gerçekleştirmek üzerinde bağlıdır.

İçin **ilk eşitlemeler**, kullanıcılar ve gruplar sağlama kapsamında sayısını ve kullanıcı ve grup kaynak sistemindeki toplam sayısı dahil çeşitli Etkenler, çeşitli iş zaman bağlıdır. İlk eşitleme performansı etkileyen faktörleri kapsamlı bir listesi bu bölümde daha sonra özetlenir.

İçin **artımlı eşitlemeler**, işi zaman bu eşitleme döngüsü algılandı değişikliklerinin sayısına bağlı olarak değişir. 5. 000'den daha az kullanıcı veya grup üyeliği değişiklikleri varsa, bir tek Artımlı eşitleme döngüsü içinde iş tamamlayabilir. 

Eşitleme zamanlarını sağlama yaygın senaryolar için aşağıdaki tabloda özetlenmiştir. Bu senaryolarda, Azure AD kaynaklı sistemidir ve hedef sistemde bir SaaS uygulamasıdır. Eşitleme sürelerini ServiceNow, çalışma alanı, Salesforce ve Google Apps, SaaS uygulamaları için eşitleme işlerinin istatistiksel çözümleme türetilmiştir.


| Kapsam yapılandırması | Kullanıcılara, gruplara veya kapsamda üyeleri | İlk eşitleme zamanı | Artımlı eşitleme zamanı |
| -------- | -------- | -------- | -------- |
| Atanan kullanıcı ve grupları yalnızca Eşitle |  < 1.000 |  < 30 dakika | < 30 dakika |
| Atanan kullanıcı ve grupları yalnızca Eşitle |  1.000 - 10.000 | 142 - 708 dakika | < 30 dakika |
| Atanan kullanıcı ve grupları yalnızca Eşitle |   10.000 - 100.000 | 1,170 - 2,340 dakika | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  < 1.000 | < 30 dakika  | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  1.000 - 10.000 | < 30-120 dakika | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  10.000 - 100.000  | 713 - 1,425 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme|  < 1.000  | < 30 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme | 1.000 - 10.000  | 43 - 86 dakika | < 30 dakika |


Yapılandırma için **eşitleme atanan kullanıcı ve grupları yalnızca**, şu formüllerden yaklaşık minimum ve maksimum beklenen belirlemek için kullanabileceğiniz **ilk eşitleme** saatler:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Tamamlamak süresini etkileyen faktörler özeti bir **ilk eşitleme**:

* Kullanıcılar ve gruplar sağlama kapsamında toplam sayısı

* Kullanıcılar, gruplar ve Grup üyeleri kaynak sistemde bulunan (Azure AD) toplam sayısı

* Olup olmadığını kullanıcıların sağlama kapsamında hedef uygulamada mevcut kullanıcılar eşleştirilir veya ilk kez oluşturulması gerekir. Eşitleme işleri, tüm kullanıcılar için ilk kez oluşturulur olması yaklaşık *iki kez sürece* olarak eşitleme işleri, tüm kullanıcılar için mevcut kullanıcıların eşleştirilir.

* Hataların sayısı [denetim günlükleri](check-status-user-account-provisioning.md). Çok sayıda hata ve sağlama hizmeti bir karantina duruma geçti performansı daha yavaştır 

* İstek hız sınırları ve hedef sistem tarafından uygulanan kısıtlama. Bazı hedef sistemleri istek hızı sınırlarını ve hangi büyük eşitleme işlemler sırasındaki performansınızı etkileyebilir azaltma uygular. Bu şartlar altında çok fazla istek çok hızlı aldığı uygulama yanıt hızını yavaş veya bağlantıyı kapatın. Performansı artırmak için bağlayıcı uygulama isteklerini uygulama bunları işleyebileceğinden daha hızlı göndererek değil ayarlamak gerekir. Microsoft tarafından oluşturulan sağlama bağlayıcılar bu ayarı yapın. 

* Atanan gruplar boyutunu ve sayı. Atanan gruplar eşitleniyor kullanıcıları eşitleme daha uzun sürer. Sayı ve boyutları atanan gruplar performansı etkileyebilir. Bir uygulama varsa [eşlemeleri için nesne eşitleme grubu etkin](customize-application-attributes.md#editing-group-attribute-mappings)grubu adları gibi Grup Özellikleri ve üyeliklerinin yanı sıra kullanıcılar eşitlenmiş. Bu ek eşitlemeler yalnızca kullanıcı, nesneyi eşitleme daha uzun sürer.


##<a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>Kullanıcı düzgün hazırlanmadı olmadığını nasıl anlayabilirim?

Sağlama hizmeti kullanıcı tarafından gerçekleştirilen tüm işlemler Azure AD'de kayıtlı denetim günlükleri. Bu tüm içeren okuma ve yazma işlemleri kaynak ve hedef sistemlerinin yanı sıra kullanıcı verileri okuma veya her işlemi sırasında yazılan çalışıldı.

Nasıl okuma Azure portalında denetim günlükleri hakkında daha fazla bilgi için bkz: [sağlama raporlama Kılavuzu](check-status-user-account-provisioning.md).


##<a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>Kullanıcı hazırlama sorunlarını nasıl giderebilirim?

Senaryo tabanlı otomatik kullanıcı hazırlama sorunlarını giderme konusunda yönergeler için bkz. [yapılandırmak ve uygulamaya kullanıcı hazırlama sorunlarını](application-provisioning-config-problem.md).


##<a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>Otomatik kullanıcı hazırlama kullanıma alma için en iyi uygulamalar nelerdir?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Bir uygulamaya giden kullanıcı sağlama için bir örnek adım adım dağıtım planlama için [kimlik kullanıcı sağlama için Dağıtım Kılavuzu](https://aka.ms/userprovisioningdeploymentplan).

##<a name="more-frequently-asked-questions"></a>Diğer sık sorulan sorular

###<a name="does-automatic-user-provisioning-to-saas-apps-work-with-b2b-users-in-azure-ad"></a>Otomatik kullanıcı iş SaaS uygulamaları ile B2B kullanıcıları Azure AD'de sağlamayı mu?

Evet, SaaS uygulamaları için Azure AD'de hizmet sağlama B2B (veya konuk) kullanıcıları sağlama Azure AD Kullanıcınızı kullanmanız mümkündür.

Ancak, B2B kullanıcıları Azure AD kullanarak SaaS uygulamasında oturum açabilmesi SaaS uygulama belirli bir şekilde yapılandırılmış, SAML tabanlı çoklu oturum açma özelliği olması gerekir. SaaS uygulamalarını B2B kullanıcıları oturum açma işlemleri desteklemek için yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma SaaS uygulamaları için B2B işbirliği]( https://docs.microsoft.com/azure/active-directory/b2b/configure-saas-apps).

###<a name="does-automatic-user-provisioning-to-saas-apps-work-with-dynamic-groups-in-azure-ad"></a>Otomatik kullanıcı SaaS uygulamaları çalışmaya dinamik grupları ile Azure AD'de sağlamayı mu?

Evet. Ne zaman "eşitleme yalnızca atanan kullanıcılar ve gruplar için" yapılandırılmış, sağlama hizmetini Azure AD kullanıcı sağlama kaldırabilir kullanıcılara veya bir SaaS uygulamasında üyesi olduğu olup olmadığı hakkında bir [dinamik grup](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule]). Dinamik gruplar, "tüm kullanıcılar ve grupları eşitleme" seçeneği ile de çalışır.

Ancak, dinamik gruplar kullanımını SaaS uygulamaları için Azure AD'den sağlama uçtan uca kullanıcı genel performansını etkileyebilir. Lütfen dinamik gruplar kullanırken, bu uyarılar ve öneriler göz önünde bulundurun:

* Dinamik grup üyeliği değişiklikleri ne kadar hızlı değerlendirebilirsiniz nasıl hızla kullanıcı dinamik bir grup olarak sağlanan veya bir SaaS uygulamasında sağlaması bağlıdır. Dinamik bir grup işleme durumunu denetleme hakkında daha fazla bilgi için bkz: [bir üyelik kuralı için işlem durumunu denetleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule#check-processing-status-for-a-membership-rule).

* Üyelik kaybı bir sağlamayı kaldırma olay sonuçlanacağı dinamik gruplar kullanırken kuralları dikkatli bir şekilde hazırlama ve sağlamayı göz önünde bulundurun kullanıcıyla dikkate alınmalıdır.

###<a name="does-automatic-user-provisioning-to-saas-apps-work-with-nested-groups-in-azure-ad"></a>Otomatik kullanıcı SaaS uygulamaları çalışmaya iç içe grupları ile Azure AD'de sağlamayı mu?

Hayır. "Eşitleme yalnızca atanan kullanıcılar ve gruplar için" yapılandırıldığında, Azure AD kullanıcı sağlama hizmeti okuyun veya iç içe geçmiş gruplardaki kullanıcıların sağlamak mümkün değil. Yalnızca okuma ve anında açıkça atanan grubun üyesi olan kullanıcılar sağlayın.

Bu bir sınırlamadır "Grup tabanlı atamaları uygulamalar için", aynı zamanda çoklu oturum açma etkiler ve açıklanan [SaaS uygulamalarına erişimi yönetmek için bir grup kullanmanızı](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/groups-saasapps ).

Geçici bir çözüm olarak, açıkça atamasını (veya başka türlü [kapsamını](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)) sağlanması gereken kullanıcıları içeren gruplar.

## <a name="related-articles"></a>İlgili makaleler
* [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
* [Azure AD eşitleme API genel bakış](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
