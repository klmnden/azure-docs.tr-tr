---
title: SaaS uygulaması Azure AD'de kullanıcı sağlamayı otomatik | Microsoft Docs
description: Otomatik olarak sağlamak için Azure AD nasıl kullanabileceğiniz bir giriş sağlanmasını ve sürekli olarak kullanıcı hesaplarını birden çok üçüncü taraf SaaS uygulamalarında güncelleştirin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
manager: mtillman
editor: ''
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/07/2018
ms.author: asmalser
ms.openlocfilehash: fce7ea66f5e10aae4f1a0a3f0ed92ca57e6112c7
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293305"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirme
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>SaaS uygulamaları için kullanıcı sağlamayı otomatik nedir?
Azure Active Directory (Azure AD) oluşturulması, Bakım ve kullanıcı kimlikleri bulutta kaldırılmasını otomatikleştirmenizi sağlar ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) Dropbox, Salesforce, ServiceNow ve daha fazlası gibi uygulamalar.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**Aşağıda bu özellik, yapmak nelere izin ilişkin bazı örnekler şunlardır:**

* Otomatik olarak ekip veya kuruluş katıldığınızda yeni kişiler için doğru sistemlerinde yeni hesapları oluşturun.
* Otomatik olarak kişiler ekip veya kuruluş ayrıldığında sağ sistemlerinde hesapları devre dışı bırakın.
* Uygulamalar ve sistemler kimlikleri dizini ya da İnsan Kaynakları sisteminizi değişikliklere göre güncel tutulduğundan emin olun.
* Bunları destekleyen uygulamalar için gruplar gibi kullanıcı olmayan nesneler sağlayın.

**Otomatik kullanıcı hazırlama aşağıdaki işlevleri içerir:**

* Kaynak ve hedef sistemleri arasında varolan kimlikleri eşleşen yeteneği.
* Hangi kullanıcı veri tanımlama özelleştirilebilir öznitelik eşlemelerini kaynak sistemden hedef sisteme akışı.
* Hazırlama hataları için isteğe bağlı bir e-posta uyarıları
* İzleme ve sorun giderme Yardımı için raporlama ve etkinlik günlükleri.

## <a name="why-use-automated-provisioning"></a>Otomatik sağlama neden kullanılır?
Bu özelliği kullanmak için bazı ortak sözleri şunları içerir:

* Maliyetleri, verimsiz ve el ile sağlama işlemlerle ilişkili İnsan hatası önleme.
* Barındırma ve özel geliştirilmiş sağlama çözümleri ve komut dosyaları korumanın ilişkili maliyetleri önleme
* Kuruluşunuz kuruluştan ayrılan anında kullanıcıların kimliklerini anahtar SaaS uygulamalardan kaldırarak güvenli hale getirmek için.
* Çok sayıda kullanıcı belirli bir SaaS uygulama veya sistem kolayca almak için.
* İlkeleri kimin hazırlanmıştır ve bir uygulamaya oturum açabilir kimin belirlemek için tek bir dizi sahip keyfini için.


## <a name="how-does-automatic-provisioning-work"></a>Otomatik sağlama nasıl çalışır?
    
**Azure AD hizmeti sağlama** her uygulama satıcısı tarafından sağlanan kullanıcı yönetimi API uç noktaları bağlanarak kullanıcılara SaaS uygulamaları ve diğer sistemler sağlar. Bu kullanıcı yönetim API uç noktaları program aracılığıyla oluşturmak, güncelleştirmek ve kullanıcıları kaldırmak Azure AD verin. Sağlama hizmeti de oluşturabilir, seçilen uygulamalar için güncelleştirme ve grupları ve rolleri gibi ek kimlikle ilgili nesneleri kaldırın. 

![Sağlama](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*Şekil 1: hizmet sağlama Azure AD*

![Giden sağlama](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*Şekil 2: "Giden" kullanıcı Azure AD'den iş akışı popüler SaaS uygulamaları için hazırlama*

![Gelen hazırlama](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*Şekil 3: "Giriş" kullanıcı Azure Active Directory ve Windows Server Active Directory popüler İnsan sermaye Yönetimi (HCM) uygulamalardan iş akışı sağlama*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Hangi uygulamalar ve sistemler Azure AD otomatik kullanıcı sağlamayı kullanabilir miyim?

Azure AD özelliklerini SCIM'yi 2.0 standart belirli kısımlarını uygulamak uygulamaları için genel destek yanı sıra çeşitli popüler SaaS uygulamaları ve İnsan Kaynakları sistemleri için destek önceden tümleştirilmiştir.

Azure AD olan tüm uygulamaların önceden tümleştirilmiş sağlama bağlayıcı destekleyen bir listesi için bkz: [kullanıcı sağlamayı uygulama öğreticiler listesi](active-directory-saas-tutorial-list.md).

Azure AD kullanıcı uygulamaya sağlama desteği ekleme hakkında daha fazla bilgi için bkz: [kullanıcıları ve grupları Azure Active Directory'den uygulamalara otomatik olarak sağlamak için SCIM'yi kullanma](manage-apps/use-scim-to-provision-users-and-groups.md).

Azure AD kişiye üzerinden bir ileti gönderme mühendislik ekibi ek uygulamalar için sağlama destek istemek için [Azure Active Directory geri bildirim Forumunda](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).    

> [!NOTE]
> Sırayla otomatik kullanıcı sağlamayı desteklemek için bir uygulama için önce gerekli kullanıcı yönetimi için dış programların oluşturulması, Bakım ve kullanıcıların kaldırılmasını otomatik hale getirmek izin API'leri sağlamanız gerekir. Bu nedenle, tüm SaaS uygulamaları bu özellik ile uyumlu değildir. Kullanıcı Yönetimi API'leri destekleyen uygulamaları için Azure AD mühendislik ekibi ardından uygulamalarla sağlama bir bağlayıcı oluşturmak mümkün olacaktır ve bu iş geçerli ve olası müşteriler gereksinimlerinize göre öncelik. 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Bir uygulamaya otomatik sağlamayı nasıl ayarlarım?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Seçilen bir uygulamaya başlar için hizmet sağlama Azure ad yapılandırma  **[Azure portal](https://portal.azure.com)**. İçinde **Azure Active Directory > Kurumsal uygulamalar** bölümünde, select **Ekle**, ardından **tüm**, senaryonuza bağlı olarak aşağıdakilerden birini ekleyin:

* Tüm uygulamalar **özellikli uygulamalara** destek bölümüne otomatik sağlama. Bkz: [kullanıcı sağlamayı uygulama öğreticiler listesi](active-directory-saas-tutorial-list.md) ek olanlar.

* Özel geliştirilmiş SCIM'yi tümleştirmeler için "galeri olmayan uygulama" seçeneğini kullanın

![Galeri](./media/active-directory-saas-app-provisioning/gallery.png)

Uygulama Yönetimi ekran sağlama yapılandırılan **sağlama** sekmesi.

![Ayarlar](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* **Yönetici kimlik bilgileri** kullanıcı yönetim API'si uygulama tarafından sağlanan bağlanmak için sağlayacak hizmet sağlama Azure ad sağlanmalıdır. Bu bölümde Ayrıca, kimlik bilgileri başarısız veya sağlama işi girmeyeceğini e-posta bildirimleri etkinleştirmenize izin verir [karantina](#quarantine).

* **Öznitelik eşlemelerini** yapılandırılabilir belirtmek, kaynak sistemde alanlar (örnek: Azure AD) hedef sistemde hangi alanlar için içeriklerini eşitlenmemiş (örnek: ServiceNow). Hedef uygulama destekliyorsa, bu bölümde, isteğe bağlı olarak kullanıcı hesaplarının yanı sıra gruplarının sağlama yapılandırmanıza izin verir. "Eşleşen Özellikler" hangi alanların hesapları sistemleri arasında eşleştirmek için kullanılan seçmenize olanak tanır. "[İfadeleri](active-directory-saas-writing-expressions-for-attribute-mappings.md)" değiştirebilir ve hedef sisteme yazılmadan önce kaynak sistemden alınan değerleri dönüştüren olanak sağlar. Daha fazla bilgi için bkz: [öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md).

![Ayarlar](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Kapsam belirleme filtreleri** hangi kullanıcılara sağlama hizmeti söyleyin ve kaynak sistemi grubunda sağlanan ve/veya hedef sistem sağlaması kaldırılıyor. sağlaması. Kapsam belirleme birlikte değerlendirildiğini filtreleri için iki nokta belirleyen sağlama kapsamında kim:

    * **Öznitelik değerleri üzerinde filtre** -öznitelik eşlemelerini "Kaynak nesnenin Scope" Menüde özel öznitelik değerlerine göre filtrelemeye olanak tanır. Örneğin, yalnızca bir "Departman" özniteliği "Satış" kullanıcılarla sağlama kapsamında olması gerektiğini belirtebilirsiniz. Daha fazla bilgi için bkz: [kapsam filtreleri kullanarak](active-directory-saas-scoping-filters.md).

    * **Atamaları filtresini** -Hazırlama "Scope" menüde > portal ayarları bölümünde olup yalnızca "atanan" Kullanıcılar ve gruplar sağlama kapsamında olması gerekir veya tüm kullanıcılar Azure AD dizininde olup olmayacağını belirtmenize olanak verir sağlandı. "Kullanıcılar ve gruplar atama" hakkında daha fazla bilgi için bkz: [bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atamak](manage-apps/assign-user-or-group-access-portal.md).
    
* **Ayarları** veya şu anda çalışıyor olup olmadığını da dahil olmak üzere, bir uygulama için sağlama hizmet işlemini denetleyen.

* **Denetim günlükleri** kayıtları hizmet sağlama Azure AD tarafından gerçekleştirilen her işlemin sağlar. Daha fazla ayrıntı için bkz: [sağlama raporlama Kılavuzu](active-directory-saas-provisioning-reporting.md).

![Ayarlar](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

> [!NOTE]
> Hizmet sağlama Azure AD kullanıcısının de yapılandırılabilir ve yönetilen kullanarak [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>Sağlama işlemi sırasında ne olur?

Azure AD kaynak sistemi olduğunda sağlama hizmeti kullanan [fark sorgu Özelliği Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) kullanıcılar ve gruplar izlemek için. Sağlama hizmeti ilk eşitleme kaynak ve hedef sistemi, düzenli artımlı eşitlemeler tarafından izlenen karşı çalışır. 

### <a name="initial-sync"></a>İlk eşitleme
Sağlama hizmeti başlatıldığında, herhangi bir zamanda gerçekleştirilen ilk eşitleme yapar:

1. Tüm kullanıcılar ve gruplar içinde tanımlanan tüm öznitelikleri alma kaynak sistemden sorgu [öznitelik eşlemelerini](active-directory-saas-customizing-attribute-mappings.md).
2. Kullanıcıları ve grupları döndürdü, yapılandırılmış kullanarak filtre [atamaları](manage-apps/assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam belirleme filtreleri](active-directory-saas-scoping-filters.md).
3. Ne zaman atanacak bir kullanıcı bulunamadı veya sağlama kapsamda hedef sistem belirlenen kullanarak eşleşen bir kullanıcı için hizmet sorgular [öznitelikleri eşleşen](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties). Örnek: kaynak sistemde userPrincipal adı eşleşen öznitelik ise ve eşlendiği hedef sistem sonra sağlama hizmeti kullanıcı hedef sistem kaynak sistemde userPrincipal ad değerleriyle eşleşen kullanıcı adları için sorgular.
4. Eşleşen kullanıcı hedef sistemde bulunmazsa, kaynak sistemden öznitelikleri kullanılarak oluşturulur.
5. Eşleşen kullanıcı bulunursa, kaynak sistem tarafından sağlanan öznitelikleri kullanılarak güncelleştirilir.
6. Öznitelik eşlemelerini "başvuru" öznitelikleri içeriyorsa, hizmet oluşturmak ve başvurulan nesneleri bağlamak için hedef sistemde ek güncelleştirmeleri gerçekleştirir. Örneğin, bir kullanıcı bir "Yönetici" özniteliği hedef sistemde oluşturulan başka bir kullanıcıya bağlı hedef sistem olabilir.
7. Filigran sonraki artımlı eşitlemeler için başlangıç noktasıdır ilk eşitleme işleminin sonunda kalıcı olmasını sağlar.

Yalnızca kullanıcıların sağlama, ancak Ayrıca grupları ve üyeleri sağlama ServiceNow, Google Apps ve kutusunu desteği gibi bazı uygulamalar. İçinde grup sağlama etkinse, bu durumlarda [eşlemeleri](active-directory-saas-customizing-attribute-mappings.md), hizmet sağlama kullanıcılar ve gruplar eşitler ve daha sonra grup üyeliklerini eşitler. 

### <a name="incremental-syncs"></a>Artımlı eşitlemeler
İlk eşitleme sonrasında tüm sonraki eşitlemeler olur:

1. Tüm kullanıcılar ve son Filigran depolandıktan sonra güncelleştirilmiş gruplar için kaynak sistemi sorgu.
2. Kullanıcıları ve grupları döndürdü, yapılandırılmış kullanarak filtre [atamaları](manage-apps/assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam belirleme filtreleri](active-directory-saas-scoping-filters.md).
3. Ne zaman atanacak bir kullanıcı bulunamadı veya sağlama kapsamda hedef sistem belirlenen kullanarak eşleşen bir kullanıcı için hizmet sorgular [öznitelikleri eşleşen](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties).
4. Eşleşen kullanıcı hedef sistemde bulunmazsa, kaynak sistemden öznitelikleri kullanılarak oluşturulur.
5. Eşleşen kullanıcı bulunursa, kaynak sistem tarafından sağlanan öznitelikleri kullanılarak güncelleştirilir.
6. Öznitelik eşlemelerini "başvuru" öznitelikleri içeriyorsa, hizmet oluşturmak ve başvurulan nesneleri bağlamak için hedef sistemde ek güncelleştirmeleri gerçekleştirir. Örneğin, bir kullanıcı bir "Yönetici" özniteliği hedef sistemde oluşturulan başka bir kullanıcıya bağlı hedef sistem olabilir.
7. Önceden sağlama kapsamında olan bir kullanıcı (atanmamış dahil) kapsamdan kaldırdıysanız, hizmet kullanıcı hedef sistemdeki bir güncelleştirme aracılığıyla devre dışı bırakır.
8. Önceden sağlama kapsamında olan bir kullanıcı devre dışı veya kaynak sistemde geçici olarak silinen hizmeti kullanıcı hedef sistemdeki bir güncelleştirme aracılığıyla devre dışı bırakır.
9. Önceden sağlama kapsamında olan bir kullanıcı silinmiş sabit kaynak sistemde ise, hizmet hedef sistemde kullanıcıyı siler. Azure AD'de kalıcı olarak silinmiş, geçici olarak silinen sonraki 30 gün kullanıcılardır.
10. Yeni bir filigran sonraki artımlı eşitlemeler için başlangıç noktasıdır Artımlı eşitleme işleminin sonunda kalıcı olmasını sağlar.

>[!NOTE]
> İsteğe bağlı olarak oluşturma, güncelleştirme veya silme işlemleri kullanarak devre dışı bırakabilirsiniz **hedef nesne eylemleri** onay kutuları [öznitelik eşlemelerini](active-directory-saas-customizing-attribute-mappings.md) bölümü. Güncelleştirme sırasında bir kullanıcı devre dışı bırakmak için mantığı da "accountEnabled" gibi bir alandan özellik eşlemesi aracılığıyla denetlenir.

Sağlama hizmeti uçtan uca artımlı eşitlemeler kalıcı olarak tanımlanan aralıklarla çalışmaya devam edecek [öğretici her uygulama için belirli](active-directory-saas-tutorial-list.md), aşağıdaki olaylardan biri gerçekleşene kadar:

* Hizmeti Azure Portalı'nı kullanarak veya uygun grafik API'si komutunu kullanarak el ile durduruldu 
* Yeni bir ilk eşitleme kullanarak tetiklenir **durumu temizleyin ve yeniden** seçeneği Azure portalında veya uygun grafik API'si komutunu kullanarak. Bu, depolanan tüm Filigran temizler ve yeniden değerlendirilmesi tüm kaynak nesneleri neden olur.
* Yeni bir ilk eşitleme öznitelik eşlemelerini veya kapsam filtreler değişiklik nedeniyle tetiklenir. Bu ayrıca depolanan tüm Filigran temizler ve yeniden değerlendirilmesi tüm kaynak nesneleri neden olur.
* Sağlama işlemi nedeniyle yüksek hata oranı (aşağıya bakın) karantinaya gider ve dört hafta boyunca birden fazla Karantinadaki kalır. Bu durumda hizmet otomatik olarak devre dışı bırakılır.

### <a name="errors-and-retries"></a>Hata ve yeniden deneme 
Tek bir kullanıcı eklenemez, güncelleştirilmiş veya hedef sistemdeki bir hata nedeniyle hedef sistemde silindi, işlemi sonraki eşitleme döngüsünde denenecek. Kullanıcı başarısız olmaya devam ederse, günde tek girişimi dön kademeli olarak ölçekleme daha az sıklıkla, gerçekleşmesi yeniden deneme başlar. Hatayı gidermek için Yöneticiler denetlemeniz gerekir [denetim günlüklerini](active-directory-saas-provisioning-reporting.md) "işlemi emanet" kök belirlemek için olayları neden ve uygun eylemi gerçekleştirin. Ortak hataları içerebilir:

* Kullanıcılar hedef sistemde gerekli kaynak sistemde doldurulan bir özniteliğe sahip değil
* Bir öznitelik değeri için olduğu benzersiz bir kısıtlama hedef sistemde kaynak sistemde ve aynı değere sahip kullanıcılar başka bir kullanıcı kaydındaki yok

Bu hatalar, kaynak sistemde etkilenen kullanıcının öznitelik değerlerini ayarlayarak ya da çakışmalarına neden değil için öznitelik eşlemelerini ayarlayarak çözülebilir.   

### <a name="quarantine"></a>Karantinaya Al
Çoğu ya da tüm aramalarının hedef sistem karşı sürekli olarak yapılan (geçersiz yönetici kimlik bilgileri durumunda olduğu gibi) bir hata nedeniyle başarısız, ardından sağlama işi bir "karantina" durumuna geçtiğinde. Bu belirtilen [özet raporu sağlama](active-directory-saas-provisioning-reporting.md)ve e-posta bildirimleri Azure portalında yapılandırıldıysa e-posta yoluyla. 

Karantinadaki olduğunda, artımlı eşitlemeler sıklığını kademeli olarak günde bir kez için azaltılır. 

Sağlama işi karantinadan tüm sabit sorunlu hataları sonra kaldırılır ve sonraki eşitleme döngüsü başlatır. Sağlama işi dört hafta boyunca birden fazla Karantinadaki kalırsa, sağlama işi devre dışı bırakıldı.


## <a name="how-long-will-it-take-to-provision-users"></a>Ne kadar kullanıcılar sağlayacak sürer?

Önceki bölümde açıklandığı gibi performansı sağlama işinizi bir başlangıç eşitlemesi ya da bir artımlı eşitleme gerçekleştirme üzerinde bağlıdır.

İçin **ilk eşitlemeler**, kullanıcılar ve gruplar sağlama kapsamında sayısı ve kullanıcı ve grup kaynak sistemdeki toplam sayısı dahil çeşitli Etkenler, çeşitli iş zaman bağlıdır. İlk eşitleme performansını etkileyen faktörler kapsamlı bir listesini Bu bölümde daha sonra özetlenir.

İçin **artımlı eşitlemeler**, iş süresi değişiklikler bu eşitleme döngüsü algılandı sayısına bağlı olarak değişir. 5. 000'den az kullanıcı veya grup üyeliği değişiklikleri varsa, iş tek Artımlı eşitleme döngüsü içinde bitirebilirsiniz. 

Eşitleme zamanlarını sağlama yaygın senaryolar için aşağıdaki tabloda özetlenmiştir. Bu senaryoda, kaynak sistemi Azure AD ve hedef sistem bir SaaS uygulaması. Eşitleme zamanlarını ServiceNow, çalışma alanı, Salesforce ve Google Apps SaaS uygulamaları için eşitleme işlerinin istatistiksel çözümleme türetilir.


| Kapsam yapılandırması | Kullanıcıları, grupları ve üyeleri kapsamında | İlk eşitleme zamanı | Artımlı eşitleme zamanı |
| -------- | -------- | -------- | -------- |
| Atanan kullanıcılar ve gruplar yalnızca Eşitle |  < 1.000 |  < 30 dakika | < 30 dakika |
| Atanan kullanıcılar ve gruplar yalnızca Eşitle |  1.000 - 10.000 | 142 - 708 dakika | < 30 dakika |
| Atanan kullanıcılar ve gruplar yalnızca Eşitle |   10.000 - 100.000 | 1,170 - 2,340 dakika | < 30 dakika |
| Tüm kullanıcıların ve grupların Azure AD'de eşitleme |  < 1.000 | < 30 dakika  | < 30 dakika |
| Tüm kullanıcıların ve grupların Azure AD'de eşitleme |  1.000 - 10.000 | < 30-120 dakika | < 30 dakika |
| Tüm kullanıcıların ve grupların Azure AD'de eşitleme |  10.000 - 100.000  | 713 - 1,425 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme|  < 1.000  | < 30 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme | 1.000 - 10.000  | 43 - 86 dakika | < 30 dakika |


Yapılandırma için **eşitleme atanan kullanıcı ve grupları yalnızca**, yaklaşık minimum ve maksimum beklenen belirlemek için aşağıdaki formülleri kullanabilirsiniz **ilk eşitleme** saatleri:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Tamamlamak süresini etkileyen faktörler özetini bir **ilk eşitleme**:

* Kullanıcılar ve gruplar sağlama kapsamında toplam sayısı

* Kullanıcılar, gruplar ve Grup üyeleri kaynak sistemde bulunan (Azure AD) toplam sayısı

* Olsun veya olmasın sağlama kapsamında kullanıcılar hedef uygulamada var olan kullanıcılara eşleşen veya ilk kez oluşturulması gerekir. Eşitleme işleri, tüm kullanıcılar ilk kez oluşturulur ele yaklaşık *iki kez sürece* olarak eşitleme işleri, tüm kullanıcılar için mevcut kullanıcıları eşleştirilir.

* Hataların sayısı [denetim günlüklerini](active-directory-saas-provisioning-reporting.md). Performans birçok hata vardır ve sağlama hizmeti karantina durumuna geçti yavaş ise   

* Oran sınırları ve hedef sistem tarafından uygulanan kısıtlama isteyin. Bazı hedef sistemleri isteği oran sınırları ve hangi büyük eşitleme işlemler sırasındaki performansınızı etkileyebilir azaltma uygular. Bu koşullar altında çok fazla ister çok hızlı bir uygulama veya kendi yanıt hızını yavaş bağlantı kapatın. Performansı artırmak için bağlayıcı uygulama isteklerini uygulama bunları işleyebileceğinden daha hızlı göndererek değil ayarlamak gerekir. Microsoft tarafından oluşturulan sağlama bağlayıcılar bu ayarlama olun. 

* Sayı ve boyutu, atanan grupları. Grupları atanan eşitleniyor kullanıcılar eşitleniyor daha uzun sürer. Hem sayı hem de atanan grupları boyutlarını performansını etkiler. Bir uygulama varsa [grup nesnesi eşitleme için etkinleştirilmiş eşlemeleri](active-directory-saas-customizing-attribute-mappings.md#editing-group-attribute-mappings), grup özellikleri gibi Grup adları ve üyeliklerinin yanı sıra kullanıcıların eşitlenen. Bu ek eşitlemeler yalnızca kullanıcı nesneleri eşitleniyor daha uzun sürer.
 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Geçerli sağlama işinin ilerleme durumunu nasıl izleyebilir miyim?**

Bkz: [sağlama raporlama Kılavuzu](active-directory-saas-provisioning-reporting.md).

**Kullanıcıların düzgün sağlanan alma başarısız olursa nasıl anlarım?**

Tüm hataları Azure AD'de kayıtlı denetim günlükleri. Daha fazla bilgi için bkz: [sağlama raporlama Kılavuzu](active-directory-saas-provisioning-reporting.md).

**Sağlama hizmeti ile çalışan bir uygulama nasıl oluşturabilir miyim?**

Bkz: [kullanıcıları ve grupları Azure Active Directory'den uygulamalara otomatik olarak sağlamak için SCIM'yi kullanma](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning).

**Mühendislik ekibine geribildirim nasıl gönderebilir miyim?**

Aracılığıyla Bize Ulaşın [Azure Active Directory geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/).


## <a name="related-articles"></a>İlgili makaleler
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)
* [Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kapsam belirleme filtreleri kullanıcı sağlama](active-directory-saas-scoping-filters.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](manage-apps/use-scim-to-provision-users-and-groups.md)
* [Azure AD eşitleme API genel bakış](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
* [Giden kullanıcı bir uygulama sağlama için adım adım dağıtım planı](https://aka.ms/userprovisioningdeploymentplan)

