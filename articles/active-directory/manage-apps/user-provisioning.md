---
title: SaaS uygulama Azure AD'de kullanıcı sağlamayı otomatik | Microsoft Docs
description: Otomatik olarak sağlamak için Azure AD'ye nasıl kullanabileceğinizi giriş sağlamasını ve kullanıcı hesapları arasında birden çok üçüncü taraf SaaS uygulamaları sürekli olarak güncelleştirin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2019
ms.author: celested
ms.reviewer: asmalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67956b3369394f68d067fc4753a859c066428aea
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191487"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Sağlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına kullanıcı otomatikleştirin

## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>SaaS uygulamaları için kullanıcı sağlamayı otomatik nedir?
Azure Active Directory (Azure AD) oluşturulmasını, Bakım ve bulutta kullanıcı kimliklerini kaldırılmasını otomatikleştirmenize olanak tanır ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) Dropbox, Salesforce, ServiceNow ve diğer uygulamalar.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**Bu özellik sağlar:**

- Otomatik olarak ekip veya kuruluş katıldığında yeni kişiler için doğru sistemlerinde yeni hesapları oluşturun.
- Otomatik olarak kişiler ekip veya kuruluş çıktığınızda doğru sistemlerinde hesapları devre dışı bırakın.
- Uygulamalarınızı ve sistemlerinizi kimliklerini directory ya da İnsan Kaynakları sisteminizi değişikliklere göre güncel tutulduğundan emin olun.
- Gruplara, bunları destekleyen uygulamalar gibi kullanıcı dışındaki nesneler sağlayın.

**Otomatik kullanıcı hazırlama, aynı zamanda bu işlevselliği içerir:**

- Kaynak ve hedef sistemleri arasında mevcut kimlikleri eşleştirmek yeteneği.
- Hangi kullanıcı verilerini tanımlayan özelleştirilebilir öznitelik eşlemelerini kaynak sistemden hedef sisteme akışı.
- Sağlama hataları için isteğe bağlı bir e-posta uyarıları'nı tıklatın.
- İzleme ve sorun giderme Yardımı için raporlama ve etkinlik günlükleri.

## <a name="why-use-automated-provisioning"></a>Otomatik sağlama neden kullanmalısınız?

Bu özelliği kullanmak için bazı ortak motivasyonlardan şunlardır:

- Maliyetleri, verimsiz ve el ile sağlama işlemleriyle ilişkili İnsan hatası önleme.
- Barındırma ve özel geliştirilmiş sağlama çözümleri ve betikleri ile ilişkili maliyetleri engelleme.
- Kuruluşunuz, bunlar kuruluştan ayrılan anında kullanıcıların kimliklerini anahtar SaaS uygulamalarından kaldırarak güvenliğini sağlama.
- Kolayca çok sayıda kullanıcı, belirli bir SaaS uygulama veya sistem içeri aktarılıyor.
- İlkeleri kullanan bir uygulamada oturum açabilir ve kimin sağlanan belirlemek için tek bir dizi sahip.

## <a name="how-does-automatic-provisioning-work"></a>Otomatik sağlamayı nasıl çalışır?
    
**Azure AD sağlama hizmeti** kullanıcıların SaaS uygulamaları ve diğer sistemler her uygulama satıcısı tarafından sağlanan kullanıcı yönetim API uç noktaları bağlanarak sağlar. Bu kullanıcı yönetim API uç noktaları, programlı olarak oluşturmak, güncelleştirmek ve kullanıcıları kaldırmak Azure AD verin. Seçili uygulamalar için sağlama hizmeti oluşturma, güncelleştirme da gruplar ve roller gibi ek kimlikle ilgili nesneleri kaldırın. 

![Sağlama](./media/user-provisioning/provisioning0.PNG)
*Şekil 1: Azure AD sağlama hizmeti*

![Giden sağlama](./media/user-provisioning/provisioning1.PNG)
*Şekil 2: "Giden" kullanıcı popüler SaaS uygulamaları için Azure ad iş akışı sağlama*

![Sağlama gelen](./media/user-provisioning/provisioning2.PNG)
*Şekil 3: "Azure Active Directory ve Windows Server Active Directory için popüler İnsan büyük Yönetim (HCM) uygulamalardan kullanıcı sağlama iş akışı gelen"*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Hangi uygulamalar ve sistemler Azure AD'ye otomatik kullanıcı hazırlama ile kullanabilir miyim?

Azure AD özellikleri birçok popüler SaaS uygulamaları ve İnsan Kaynakları sistemleri için destek ve belirli bölümlerini SCIM 2.0 standart uygulayan uygulamaları için genel destek önceden tümleştirilmiş.

### <a name="pre-integrated-applications"></a>Önceden tümleştirilmiş uygulamalar

Azure AD, tüm uygulamaların önceden tümleştirilmiş sağlama bağlayıcı destekleyen bir listesi için bkz [kullanıcı sağlama için uygulama öğreticilerin listesine](../saas-apps/tutorial-list.md).

Azure AD kişiye üzerinden bir ileti gönderme mühendislik ekibinin ek uygulamalar için sağlama destek istemek için [Azure Active Directory geri bildirim Forumu](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> Otomatik kullanıcı sağlamayı desteklemek bir uygulama için sırada, gerekli kullanıcı yönetimi için dış programlarda oluşturulmasını, Bakım ve kullanıcıların kaldırılmasını otomatik hale getirmek izin API'leri ilk sağlamalısınız. Bu nedenle, tüm SaaS uygulamaları bu özellik ile uyumlu değildir. Kullanıcı Yönetimi API'leri destekleyen uygulamalar için Azure AD mühendislik ekibi ardından uygulamalarla sağlama bağlayıcı oluşturabilir miyim ve bu iş, geçerli ve potansiyel müşterilerin gereksinimlerine göre önceliklendirilir. 

### <a name="connecting-applications-that-support-scim-20"></a>SCIM 2.0 destekleyen uygulamalar arasında bağlantı kurma

SCIM kullanan uygulamalar genel olarak bağlanma hakkında bilgi için 2.0 - tabanlı kullanıcı yönetimi API'leri bkz [kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak sağlamak için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md).

    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Bir uygulamayı otomatik sağlamayı ayarlama nasıl ayarlayabilirim?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Azure Active Directory portalındaki Azure AD sağlama hizmeti için seçilen bir uygulamaya yapılandırmak için kullanın.

1. Açık  **[Azure Active Directory portalında](https://aad.portal.azure.com)**.

1. Seçin **kurumsal uygulamalar** sol bölmeden. Tüm yapılandırılmış uygulamaların bir listesini göster ' dir.

1. Seçin **+ yeni uygulama** bir uygulama eklemek için. Senaryonuza bağlı olarak aşağıdakilerden birini ekleyin:

   - **Kendi uygulamanızı ekleyin** seçeneği özel olarak geliştirilmiş SCIM tümleştirmeler destekler.

   - Tüm uygulamaları **Galeriden Ekle** > **özellikli uygulamalara** destek bölümüne otomatik sağlama. Bkz: [kullanıcı sağlama için uygulama öğreticilerin listesine](../saas-apps/tutorial-list.md) ek olanlar.

1. Tüm ayrıntıları sağlayın ve seçin **Ekle**. Yeni uygulama, kurumsal uygulamalar listesine eklenir ve uygulama yönetimi ekranına açar.

1. Seçin **sağlama** hazırlama ayarları uygulaması için kullanıcı hesabını yönetmek için.

   ![Ayarlar](./media/user-provisioning/provisioning_settings0.PNG)

1. Otomatik seçeneğini **sağlama modu** yönetici kimlik bilgileri, başlatma ve durdurma, eşlemeler ve eşitleme ayarlarını belirtmek için.

   - Genişletin **yönetici kimlik bilgileri** uygulamanın kullanıcı yönetimi API'sine bağlanmak Azure AD için gerekli kimlik bilgilerini girmek için. Bu bölümde Ayrıca, kimlik bilgileri başarısız veya sağlama işi girmeyeceğini e-posta bildirimlerini etkinleştirme sağlar [karantina](#quarantine).

   - Genişletin **eşlemeleri** görüntüleme ve Azure AD arasında akış kullanıcı özniteliklerini düzenleyin ve kullanıcı hesaplarını sağlandığında veya güncelleştirildiğinde, hedef uygulama. Hedef uygulama destekliyorsa, bu bölümde, isteğe bağlı olarak gruplar ve hesaplar sağlama yapılandırmanıza olanak sağlar. Bir eşleme burada görüntüleyebilir ve kullanıcı özniteliklerine özelleştirme sağa eşleme düzenleyicisini açmak için tablo seçin.
   
     **Kapsam filtreleri** hangi kullanıcıların sağlama hizmeti söyleyin ve kaynak sistemi gruplarında sağlanan veya hedef sistem için sağlaması kaldırıldı. İçinde **öznitelik eşlemesi** bölmesinde **kaynak nesne kapsamı** belirli öznitelik değerlerine göre filtre uygulamak için. Örneği yalnızca "Departman" özniteliği "Satış" olan kullanıcıların hazırlama kapsamına girmesini sağlayabilirsiniz. Daha fazla bilgi için bkz. [Kapsam filtrelerini kullanma](define-conditional-rules-for-provisioning-user-accounts.md).
    
     Daha fazla bilgi için [öznitelik eşlemelerini özelleştirme](customize-application-attributes.md).

   - **Ayarları** işlemi şu anda çalışıyor olsun dahil olmak üzere, bir uygulama için sağlama hizmetinin denetim. **Kapsam** menü yalnızca atanan kullanıcı ve grupları sağlama kapsamında olup olmayacağını veya Azure AD dizinindeki tüm kullanıcılara sağlanmalıdır belirtmenize olanak sağlar. Kullanıcıları ve grupları "atama" hakkında bilgi için bkz. [Azure Active Directory'de kurumsal uygulamalara kullanıcı veya grup atama](assign-user-or-group-access-portal.md).

Uygulama Yönetimi ekranında seçin **denetim günlükleri** görüntülemek için her işlem kayıtlarını Azure AD tarafından sağlama hizmeti çalıştırın. Daha fazla bilgi için [sağlama raporlama Kılavuzu](check-status-user-account-provisioning.md).

![Ayarlar](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> Azure AD kullanıcı sağlama hizmeti de yapılandırılabilir ve yönetilen kullanarak [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>Sağlama sırasında ne olacak?

Azure AD, kaynak sistem çalışırken, sağlama hizmeti kullanan [fark sorgusu Özelliği Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) kullanıcılar ve Gruplar'ı izlemek için. Sağlama hizmeti bir ilk eşitleme, kaynak ve hedef sistemi, düzenli aralıklarla artımlı eşitlemeler tarafından izlenen karşı çalışır. 

### <a name="initial-sync"></a>İlk eşitleme

Sağlama hizmeti başlatıldığında, ilk eşitleme çalıştırmak hiç olmadığı kadar olur:

1. Tüm kullanıcılar ve Gruplar'ı kaynak sistemden içinde tanımlanan tüm öznitelikleri alınırken sorgu [öznitelik eşlemelerini](customize-application-attributes.md).
2. Kullanıcıları ve yapılandırılmış kullanarak döndürülen, grupları filtre [atamaları](assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md).
3. Ne zaman atanmış bir kullanıcı veya sağlama için kapsam içinde belirtilen kullanarak eşleşen bir kullanıcı için hedef sistemde hizmet sorgular [öznitelikleri eşleşen](customize-application-attributes.md#understanding-attribute-mapping-properties). Örnek: Kaynak sistemindeki userPrincipal adı eşleşen bir özniteliktir ve eşlendiği hedef sistem sonra sağlama hizmeti, kullanıcı adı kaynak sistemde userPrincipal adı değerlerle eşleşen kullanıcı adları için hedef sistemde sorgular.
4. Eşleşen kullanıcı hedef sistemde bulunamazsa, kaynak sistemden öznitelikleri kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra sağlama hizmeti algılar ve kullanıcı gelecekteki tüm işlemleri çalıştırmak için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
5. Eşleşen bir kullanıcı bulunamazsa, kaynak sistem tarafından sağlanan öznitelikleri kullanılarak güncelleştirilir. Kullanıcı hesabı eşleşen sonra sağlama hizmeti algılar ve kullanıcı gelecekteki tüm işlemleri çalıştırmak için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
6. Öznitelik eşlemeleri "başvuru" özniteliği içermiyorsa, hizmet oluşturmak ve başvurulan nesneler bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir kullanıcı bir "Yönetici" özniteliği başka bir kullanıcı hedef sistemde oluşturulan bağlı hedef sistem olabilir.
7. Başlangıç noktası için sonraki artımlı eşitlemeler sağlayan ilk eşitleme sonunda Filigran kalıcı hale getirin.

Yalnızca kullanıcılar sağlama, ancak grupları ve bunların üyeleri de sağlama ServiceNow, G Suite ve kutusunu desteği gibi bazı uygulamalar. İçinde grup sağlama etkinse, bu durumlarda [eşlemeleri](customize-application-attributes.md), sağlama hizmeti kullanıcıları ve grupları eşitler ve daha sonra grubu üyeliğini eşitler. 

### <a name="incremental-syncs"></a>Artımlı eşitlemeler

İlk eşitleme sonrasında, diğer tüm eşitlemeler olur:

1. Tüm kullanıcılar ve son Filigran depolanmış beri güncelleştirildi gruplar için kaynak sistemi sorgulayın.
2. Kullanıcıları ve yapılandırılmış kullanarak döndürülen, grupları filtre [atamaları](assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md).
3. Ne zaman atanmış bir kullanıcı veya sağlama için kapsam içinde belirtilen kullanarak eşleşen bir kullanıcı için hedef sistemde hizmet sorgular [öznitelikleri eşleşen](customize-application-attributes.md#understanding-attribute-mapping-properties).
4. Eşleşen kullanıcı hedef sistemde bulunamazsa, kaynak sistemden öznitelikleri kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra sağlama hizmeti algılar ve kullanıcı gelecekteki tüm işlemleri çalıştırmak için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
5. Eşleşen bir kullanıcı bulunamazsa, kaynak sistem tarafından sağlanan öznitelikleri kullanılarak güncelleştirilir. Eşleşen yeni atanmış bir hesabı ise, sağlama hizmetine algılar ve kullanıcı gelecekteki tüm işlemleri çalıştırmak için kullanılan yeni kullanıcı kimliği hedef sistemin önbelleğe alır.
6. Öznitelik eşlemeleri "başvuru" özniteliği içermiyorsa, hizmet oluşturmak ve başvurulan nesneler bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir kullanıcı bir "Yönetici" özniteliği başka bir kullanıcı hedef sistemde oluşturulan bağlı hedef sistem olabilir.
7. Daha önce sağlama kapsamında olan bir kullanıcı (atanmamış dahil) kapsamdan kaldırdıysanız, hizmetin kullanıcı hedef sistemdeki bir güncelleştirme aracılığıyla devre dışı bırakır.
8. Daha önce sağlama kapsamında olan bir kullanıcı devre dışı ya da geçici olarak silinen kaynak sistemde hizmeti kullanıcı hedef sistemdeki bir güncelleştirme aracılığıyla devre dışı bırakır.
9. Daha önce sağlama kapsamında olan bir kullanıcı silindi sabit kaynak sistemde varsa hizmet hedef sistemde kullanıcıyı siler. Azure AD'de kalıcı olarak silinmiş, geçici olarak silinen sonraki 30 gün kullanıcılardır.
10. Başlangıç noktası için sonraki artımlı eşitlemeler sağlayan Artımlı eşitleme sonuna yeni bir filigran kalıcı hale getirin.

>[!NOTE]
> İsteğe bağlı olarak devre dışı bırakabilirsiniz **Oluştur**, **güncelleştirme**, veya **Sil** kullanarak işlemlerini **hedef nesne eylemleri** onay kutuları [Eşlemeleri](customize-application-attributes.md) bölümü. Güncelleştirme sırasında bir kullanıcı devre dışı bırakmak için mantığı da "accountEnabled" gibi bir alandan bir özellik eşlemesi aracılığıyla denetlenir.

Sağlama hizmeti uçtan uca artımlı eşitlemeler, tanımlanan aralıklarla sürdürür [öğretici her uygulamaya özgü](../saas-apps/tutorial-list.md), aşağıdaki olaylardan biri gerçekleşene kadar:

- Azure portalını kullanarak ya da uygun Graph API'si komutunu kullanarak hizmeti el ile durduruldu 
- Kullanarak yeni bir ilk eşitleme tetiklenen **durumu temizleyin ve yeniden** seçeneği Azure portalında ya da uygun Graph API'si komutu. Bu eylem, depolanmış tüm Filigran temizler ve tüm kaynak nesneleri yeniden değerlendirilecek neden olur.
- Yeni bir ilk eşitleme kapsamı belirleme filtreleri ile öznitelik eşlemelerini de bir değişiklik nedeniyle tetiklenir. Bu eylem, ayrıca depolanan tüm Filigran temizler ve tüm kaynak nesneleri yeniden değerlendirilecek neden olur.
- Sağlama işlemi bir yüksek hata oranı nedeniyle karantinaya (aşağıya bakın) geçer ve dört hafta için karantinaya kalır. Bu durumda hizmet otomatik olarak devre dışı bırakılır.

### <a name="errors-and-retries"></a>Hataları ve yeniden deneme

Ardından tek bir kullanıcı eklendi, güncelleştirilemiyor veya hedef sistemde bulunan bir hata nedeniyle hedef sistemde silindi, işlem sonraki eşitleme döngüsünde yeniden denenir. Kullanıcı başarısız olmaya devam ederse, yeniden denemeler kademeli olarak gün başına yalnızca bir girişimi dön ölçeklendirme daha düşük bir sıklıkta gerçekleşecek şekilde başlar. Hatayı gidermek için Yöneticiler denetlemelidir [denetim günlükleri](check-status-user-account-provisioning.md) "emanet işleme" kök belirlemek için olayları neden ve uygun eylemi gerçekleştirin. Sık karşılaşılan hatalar içerebilir:

- Kullanıcılar hedef sistemde gerekli kaynak sistemindeki doldurulmuş bir özniteliğe sahip değil
- Kullanıcılar bir öznitelik değeri var olan benzersiz kısıtlama hedef sistemde kaynak sistemindeki ve aynı değere sahip başka bir kullanıcı kaydı var.

Bu hatalar, kaynak sistemde etkilenen kullanıcının öznitelik değerleri ayarlayarak ya da çakışmalara neden olmayan için öznitelik eşlemelerini ayarlayarak çözülebilir.   

### <a name="quarantine"></a>Karantina

Çoğu veya tüm hedef sistem karşı sürekli olarak yapılan çağrıları (olduğu gibi geçersiz yönetici kimlik bilgileri gibi) bir hata nedeniyle başarısız olursa, sağlama işi bir "karantina" durumuna geçtiğinde. Bu durum belirtilen [özet raporu sağlama](check-status-user-account-provisioning.md) ve e-posta bildirimleri Azure portalında yapılandırıldıysa e-posta yoluyla. 

Karantinadaki olduğunda, artımlı eşitlemeler sıklığını kademeli olarak günde bir kere için azaltılır. 

Sağlama işi karantinadan tüm soruna neden olan hataları sabittir ve sonraki eşitleme döngüsü başladıktan sonra kaldırılır. Sağlama işi dört hafta için karantinaya kalırsa, sağlama işi devre dışı bırakıldı.


## <a name="how-long-will-it-take-to-provision-users"></a>Ne kadar kullanıcıları sağlama sürer?

Sağlama iş bir ilk eşitleme veya bir artımlı eşitleme çalıştıran performans bağlıdır.

İçin **ilk eşitlemeler**, sağlama, kapsamında kullanıcıların ve grupların sayısı da dahil olmak üzere, birçok faktöre ve kullanıcı ve grup kaynak sistemindeki toplam sayısı işi zaman bağlıdır. İlk eşitleme performansı etkileyen faktörleri kapsamlı bir listesi bu bölümde daha sonra özetlenir.

İçin **artımlı eşitlemeler**, işi zaman bu eşitleme döngüsü algılandı değişikliklerinin sayısına bağlı olarak değişir. 5. 000'den daha az kullanıcı veya grup üyeliği değişiklikleri varsa, bir tek Artımlı eşitleme döngüsü içinde iş tamamlayabilir. 

Eşitleme zamanlarını sağlama yaygın senaryolar için aşağıdaki tabloda özetlenmiştir. Bu senaryolarda, Azure AD kaynaklı sistemidir ve hedef sistemde bir SaaS uygulamasıdır. Eşitleme sürelerini ServiceNow, çalışma alanı, Salesforce ve G Suite SaaS uygulamaları için eşitleme işlerinin istatistiksel çözümleme türetilmiştir.


| Kapsam yapılandırması | Kullanıcılara, gruplara veya kapsamda üyeleri | İlk eşitleme zamanı | Artımlı eşitleme zamanı |
| -------- | -------- | -------- | -------- |
| Atanan kullanıcı ve grupları yalnızca Eşitle |  < 1,000 |  < 30 dakika | < 30 dakika |
| Atanan kullanıcı ve grupları yalnızca Eşitle |  1.000 - 10.000 | 142 - 708 dakika | < 30 dakika |
| Atanan kullanıcı ve grupları yalnızca Eşitle |   10,000 - 100,000 | 1,170 - 2,340 dakika | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  < 1,000 | < 30 dakika  | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  1.000 - 10.000 | < 30-120 dakika | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  10,000 - 100,000  | 713 - 1,425 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme|  < 1,000  | < 30 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme | 1.000 - 10.000  | 43 - 86 dakika | < 30 dakika |


Yapılandırma için **eşitleme atanan kullanıcı ve grupları yalnızca**, şu formüllerden yaklaşık minimum ve maksimum beklenen belirlemek için kullanabileceğiniz **ilk eşitleme** saatler:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Tamamlamak süresini etkileyen faktörler özeti bir **ilk eşitleme**:

- Kullanıcılar ve gruplar sağlama kapsamında toplam sayısı.

- Kullanıcılar, gruplar ve Grup üyeleri kaynak sistemde bulunan (Azure AD) toplam sayısı.

- Olup kullanıcıların sağlama kapsamında hedef uygulamada mevcut kullanıcılar eşleştirilir veya ilk kez oluşturulması gerekir. Eşitleme işleri, tüm kullanıcılar için ilk kez oluşturulur ele hakkında *iki kez sürece* olarak eşitleme işleri, tüm kullanıcılar için mevcut kullanıcıların eşleştirilir.

- Hataların sayısı [denetim günlükleri](check-status-user-account-provisioning.md). Çok sayıda hata ve sağlama hizmeti bir karantina duruma geçti performans daha yavaş olur.    

- İstek hız sınırları ve hedef sistem tarafından uygulanan kısıtlama. Bazı hedef sistemleri, istek hızı sınırlarını ve kısıtlama, büyük eşitleme işlemler sırasındaki performansınızı etkileyebilir uygular. Bu şartlar altında çok fazla istek çok hızlı aldığı uygulama yanıt hızını yavaş veya bağlantıyı kapatın. Performansı artırmak için bağlayıcı uygulama isteklerini uygulama bunları işleyebileceğinden daha hızlı göndererek değil ayarlamak gerekir. Microsoft tarafından oluşturulan sağlama bağlayıcılar bu ayarı yapın. 

- Atanan gruplar boyutunu ve sayı. Atanan gruplar eşitleniyor kullanıcıları eşitleme daha uzun sürer. Sayı ve boyutları atanan gruplar performansı etkileyebilir. Bir uygulama varsa [eşlemeleri için nesne eşitleme grubu etkin](customize-application-attributes.md#editing-group-attribute-mappings)grubu adları gibi Grup Özellikleri ve üyeliklerinin yanı sıra kullanıcılar eşitlenmiş. Bu ek eşitlemeler yalnızca kullanıcı, nesneyi eşitleme daha uzun sürer.


## <a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>Kullanıcı düzgün hazırlanmadı olmadığını nasıl anlayabilirim?

Sağlama hizmeti kullanıcı tarafından çalıştırılan tüm işlemleri Azure AD'de kayıtlı denetim günlükleri. Bu tüm içeren okuma ve yazma işlemleri kaynak ve hedef sistemleri ve okuma veya her işlemi sırasında yazılan kullanıcı verileri için yapılır.

Azure portalında denetim günlükleri okuma hakkında daha fazla bilgi için bkz: [sağlama raporlama Kılavuzu](check-status-user-account-provisioning.md).


## <a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>Kullanıcı hazırlama sorunlarını nasıl giderebilirim?

Senaryo tabanlı otomatik kullanıcı hazırlama sorunlarını giderme konusunda yönergeler için bkz. [yapılandırmak ve uygulamaya kullanıcı hazırlama sorunlarını](application-provisioning-config-problem.md).


## <a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>Otomatik kullanıcı hazırlama kullanıma alma için en iyi uygulamalar nelerdir?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Bir uygulamaya giden kullanıcı sağlama için bir örnek adım adım dağıtım planlama için [kimlik kullanıcı sağlama için Dağıtım Kılavuzu](https://aka.ms/userprovisioningdeploymentplan).

## <a name="more-frequently-asked-questions"></a>Diğer sık sorulan sorular

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-b2b-users-in-azure-ad"></a>Otomatik kullanıcı iş SaaS uygulamaları ile B2B kullanıcıları Azure AD'de sağlamayı mu?

Evet, SaaS uygulamaları için Azure AD'de hizmet sağlama B2B (veya konuk) kullanıcıları sağlama Azure AD Kullanıcınızı kullanmanız mümkündür.

Ancak, B2B kullanıcıları Azure AD kullanarak SaaS uygulaması için oturum açmak SaaS uygulama belirli bir şekilde yapılandırılmış, SAML tabanlı çoklu oturum açma özelliği olması gerekir. B2B kullanıcıları oturum açmalar SaaS uygulamaları destekleyecek şekilde yapılandırma hakkında daha fazla bilgi için bkz: [yapılandırma SaaS uygulamaları için B2B işbirliği]( https://docs.microsoft.com/azure/active-directory/b2b/configure-saas-apps).

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-dynamic-groups-in-azure-ad"></a>Otomatik kullanıcı SaaS uygulamaları çalışmaya dinamik grupları ile Azure AD'de sağlamayı mu?

Evet. Sağlama hizmetini Azure AD kullanıcısı, "eşitleme yalnızca atanan kullanıcılar ve gruplar için" yapılandırıldığında sağlayabilirsiniz kaldırabilir kullanıcılara veya bir SaaS uygulamasında üyeleri olup olmadıklarını üzerinde bir [dinamik grup](../users-groups-roles/groups-create-rule.md). Dinamik gruplar, "tüm kullanıcılar ve grupları eşitleme" seçeneği ile de çalışır.

Ancak, dinamik gruplar kullanımını SaaS uygulamaları için Azure AD'den sağlama uçtan uca kullanıcı genel performansını etkileyebilir. Dinamik gruplar kullanırken, bu uyarılar ve öneriler göz önünde bulundurun:

- Dinamik grup üyeliği değişiklikleri ne kadar hızlı değerlendirebilirsiniz nasıl hızla kullanıcı dinamik bir grup olarak sağlanan veya bir SaaS uygulamasında sağlaması bağlıdır. Dinamik bir grup işleme durumunu denetleme hakkında daha fazla bilgi için bkz: [bir üyelik kuralı için işlem durumunu denetleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

- Dinamik gruplar kullanırken, hazırlama ve göz önünde bulundurun üyeliği sonuçları bir sağlamayı kaldırma olay kaybı olarak sağlamayı kullanıcıyla dikkatle kuralları alınmalıdır.

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-nested-groups-in-azure-ad"></a>Otomatik kullanıcı SaaS uygulamaları çalışmaya iç içe grupları ile Azure AD'de sağlamayı mu?

Hayır. "Eşitleme yalnızca atanan kullanıcılar ve gruplar için" yapılandırıldığında, Azure AD kullanıcı sağlama hizmeti okuyun veya iç içe geçmiş gruplardaki kullanıcıların sağlamak mümkün değildir. Yalnızca okuma ve anında açıkça atanan grubun üyesi olan kullanıcılar sağlayın.

Bu bir sınırlamadır "Grup tabanlı atamaları uygulamalar için", aynı zamanda çoklu oturum açma etkiler ve açıklanan [SaaS uygulamalarına erişimi yönetmek için bir grup kullanmanızı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-saasapps ).

Geçici bir çözüm olarak, açıkça atamanız gerekir (veya başka türlü [kapsamını](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)) sağlanması gereken kullanıcıları içeren gruplar.

### <a name="is-provisioning-between-azure-ad-and-a-target-application-using-an-encrypted-channel"></a>Azure AD arasında sağlama ve şifreli bir kanal kullanarak bir hedef uygulama?

Evet. HTTPS SSL şifrelemesi için sunucusu hedef kullanırız. 

## <a name="related-articles"></a>İlgili makaleler

- [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
- [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
- [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
- [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
- [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
- [Azure AD eşitleme API genel bakış](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
