---
title: Azure AD Connect performansını etkileyen faktörler
description: Bu belgede, çeşitli Etkenler altyapısı sağlama Azure AD Connect etkilemek açıklanmaktadır. Bu etkenler, kuruluşların kendi eşitleme gereksinimlerini karşıladığından emin olmak için kendi Azure AD Connect dağıtımını planlamak için yardımcı olur.
services: active-directory
author: billmath
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/06/2018
ms.reviewer: martincoetzer
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a3a57fbe5df690e4dbdba8cbab85e62648bb298
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60295390"
---
# <a name="factors-influencing-the-performance-of-azure-ad-connect"></a>Azure AD Connect performansını etkileyen faktörler

Azure AD Connect'in Active Directory'nizi Azure AD'ye eşitler. Bu sunucu, kullanıcı kimliklerinizin buluta taşıma kritik bir bileşenidir. Bir Azure AD Connect performansını etkileyen temel unsurlar şunlardır:

| **Tasarım faktörü**| **Tanım** |
|:-|-|
| Topoloji| Uç noktaları ve bileşenlerinin dağıtımını ağda Azure AD Connect yönetmeniz gerekir. |
| Ölçek| Nesne sayısını, kullanıcıları, grupları ve Azure AD Connect tarafından yönetilecek OU'lar, ister. |
| Donanım| Donanım (fiziksel veya sanal) Azure AD Connect ve CPU, bellek, ağ ve sabit sürücü yapılandırması dahil olmak üzere her bir donanım bileşenini bağımlı performans kapasitesi. |
| Yapılandırma| Azure AD Connect işlemleri dizinleri ve bilgiler. |
| Yükleme| Nesne değişikliklerini sıklığı. Yükler, bir saat, gün veya hafta değişebilir. Bileşene bağlı olarak, yoğun yük veya ortalama yük için tasarlamak gerekebilir. |

Bu belgenin amacı, altyapısı sağlama Azure AD Connect performansını etkileyen faktörler açıklanmaktadır sağlamaktır. Bunlar Burada özetlenen herhangi bir performans sorunu yaşarsanız büyük veya karmaşık kuruluşların (100. 000'den fazla nesneleri sağlama kuruluşlar), Azure AD Connect geliştirdikleri iyileştirmek için önerileri kullanabilirsiniz. Azure AD Connect, diğer bileşenleri gibi [Azure AD Connect health](how-to-connect-health-agent-install.md) ve aracıları değil dahil burada.

> [!IMPORTANT]
> Microsoft, değiştirme veya Azure AD Connect resmi olarak belgelenen Eylemler dışında çalışan desteklemiyor. Bu eylemler, tutarsız veya desteklenmeyen Azure AD Connect eşitlemesi durumuyla sonuçlanabilir. Sonuç olarak Microsoft, bu tür dağıtımlar için teknik destek sağlayamaz.

## <a name="azure-ad-connect-component-factors"></a>Azure AD Connect bileşenini faktörleri

Aşağıdaki diyagramda, tek bir ormana bağlama altyapısı sağlama, birden çok ormanı desteklenir ancak üst düzey bir mimari gösterilmektedir. Bu mimari, çeşitli bileşenleri birbiriyle nasıl etkileşim kurduğu gösterilmektedir.

![AzureADConnentInternal](media/plan-connect-performance-factors/AzureADConnentInternal.png)

Sağlama altyapısı, her bir Active Directory ormanına ve Azure AD'ye bağlanır. İçeri aktarma işleminin her dizinden bilgileri okunurken çağrılır. Dışarı aktarma sağlama altyapısından dizinleri güncelleştirmeye ifade eder. Eşitleme nesneleri sağlama altyapısı içinde nasıl akar kuralları değerlendirir. Daha ayrıntılı bir bakış için başvurabilirsiniz [Azure AD Connect eşitleme: Mimariyi anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture).

Azure AD Connect eşitleme Active Directory'den Azure AD'ye izin vermek için aşağıdaki hazırlama alanları, kural ve işlemleri kullanır:

* **Bağlayıcı alanı (CS)** -her bağlı dizini (CD), gerçek dizinleri nesnelerden hazırlanır burada ilk sağlama altyapısı tarafından işlenmeden önce. Azure AD'ye kendi CS ve kendi CS bağlandığınız her bir orman sahiptir.
* **Meta veri deposu (MV)** -eşitlenmesi gereken nesneleri olan oluşturmak burada eşitleme kurallara göre. Nesneler, nesneleri ve bağlı bir dizin öznitelikleri doldurabilirsiniz önce MV içinde mevcut olması gerekir. Yalnızca bir MV yoktur.
* **Eşitleme kuralları** -hangi nesnelerin (tahmini) oluşturulan veya nesnelerin MV (birleştirilmiş) bağlı karar. Eşitleme kuralları ayrıca hangi öznitelik değerleri kopyaladığınız veya dizinleri gelen ve giden dönüştürülmüş karar verin.
* **Çalıştırma profilleri** -nesne ve öznitelik değerlerini hazırlama alanları ve bağlı dizinler arasındaki eşitleme kurallarına göre kopyalama işlemi adımlarını oluşturur.

Sağlama altyapısının performansını iyileştirmek için farklı çalıştırma profili yok. Çoğu kuruluş varsayılan zamanlamalar kullanır ve normal işlemler için profil çalıştırın, ancak bazı kuruluşların gerekebilir [zamanlamayı değiştirmek](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler) veya nadir durumlarda gereksinimini karşılamak için diğer çalıştırma profillerini tetikleyin. Aşağıdaki çalıştırma profillerinden kullanılabilir:

### <a name="initial-sync-profile"></a>İlk eşitleme profili

İlk eşitleme ilk kez bir Active Directory ormanı gibi bağlı dizinleri okuma işlemi profilidir. Ardından, eşitleme altyapısı veritabanındaki tüm girişleri analizini yapar. İlk döngüsü, Azure AD'de yeni nesneler oluşturur ve Active Directory ormanlarınız büyükse, tamamlanması çok uzun sürer. İlk eşitleme, aşağıdaki adımları içerir:

1. Tüm bağlayıcılar üzerinde tam içeri aktarma
2. Tüm bağlayıcılar üzerinde tam eşitleme
3. Tüm bağlayıcılarda dışarı aktarma

### <a name="delta-sync-profile"></a>Delta eşitleme profili

Eşitleme işlemini en iyi duruma getirmek için bu farklı çalıştır profili yalnızca işlem değişiklikleri (oluşturur, siler ve güncelleştirmeleri) son eşitleme işlemini beri bağlı dizinlerinizi nesne. Varsayılan olarak, delta eşitleme profili 30 dakikada bir çalışır. Kuruluşlar için 30 dakika, Azure AD güncel olduğundan emin olmak için süresini tutmak çaba göstermelisiniz. Azure AD Connect durumunu izlemek için kullanın [İzleme Aracısı sistem durumu](how-to-connect-health-sync.md) işlemi ile ilgili tüm sorunları görmek için. Delta eşitleme profili, aşağıdaki adımları içerir:

1. Delta içeri aktarma işlemi sırasında tüm bağlayıcılar
2. Tüm bağlayıcılar delta eşitleme
3. Tüm bağlayıcılarda dışarı aktarma

Tipik kurumsal kuruluş delta eşitleme senaryosu şöyledir:

- yaklaşık 1 %'Nesne silindi
- oluşturulan nesnelerin yaklaşık %1
- ~ % 5'nesneleri değiştirme

Değişim hızı, kuruluşunuz Active Directory'niz içindeki kullanıcılar ne sıklıkta güncelleştirmeleri bağlı olarak değişiklik gösterebilir. Örneğin, yüksek oranlarda değişiklik işe alma ve iş gücünü azaltma mevsimsellik ile ortaya çıkabilir.

### <a name="full-sync-profile"></a>Tam eşitleme profili

Şu yapılandırma değişiklikleri yaptıysanız, bir tam eşitleme döngüsü gereklidir:



- Nesneleri veya bağlı dizinlerden içeri aktarılacak öznitelikleri kapsamını artırdık. Örneğin, bir etki alanı veya OU alma kapsamınızı eklediğinizde.
- Eşitleme kuralları için yapılan değişiklikler. Örneğin, bir kullanıcının unvanı extension_attribute3 Active Directory'de Azure AD'den içinde doldurmak için yeni bir kural oluştururken. Bu güncelleştirme, sağlama altyapısı sunulacağından, değişikliği uygulamak için kendi başlıklarında güncelleştirmek için tüm mevcut kullanıcılar yeniden inceleyin gerektirir.

Aşağıdaki işlemleri, bir tam eşitleme döngüsünde dahildir:

1. Tüm bağlayıcılar üzerinde tam içeri aktarma
2. Tüm bağlayıcıların tam/Delta eşitleme
3. Tüm bağlayıcılarda dışarı aktarma

> [!NOTE]
> Active Directory veya Azure AD çok sayıda nesne toplu güncelleştirmeler yaparken dikkatli planlama gereklidir. Toplu güncelleştirmeler, delta eşitleme işlemi, nesneleri çok fazla değiştirilmiş olduğundan, içeri aktarılırken uzun sürmesine neden olur. Toplu güncelleştirme eşitleme işlemini etkilemek değil olsa bile uzun içeri aktarmalar oluşabilir. Örneğin, birçok kullanıcılara lisanslar atama Azure AD'de bir uzun alma döngüsü Azure AD'den neden olur, ancak Active Directory'deki öznitelik değişiklikleri açmayacaktır.

### <a name="synchronization"></a>Eşitleme

Eşitleme işlemi çalışma zamanı performans özellikleri şunlardır:

* Eşitleme iş parçacıklıdır sağlama altyapısı çalıştırma profillerini bağlı dizinleri, nesneler veya öznitelikleri, tüm paralel işleme gerektirmeyen tek olur.
* İçeri aktarma zaman ile eşitlendiğinden nesne sayısını doğrusal oranda artar. 10\.000 nesneleri içeri aktarmak için 10 dakika alırsa, örneğin, durumda 20.000 nesneler yaklaşık 20 dakika aynı sunucu üzerinde kazanacaktır.
* Dışarı aktarma da doğrusal değildir.
* Eşitleme nesneleri diğer nesnelere başvurular sayısına göre katlanarak artar. Üyeleri kullanıcı, nesneyi veya diğer gruplara başvurduğundan grup üyeliklerini ve iç içe geçmiş gruplar ana performans etkileri olabilir. Bu başvurular bulundu ve eşitleme döngüsü tamamlanması MV gerçek nesnelere başvuru.

### <a name="filtering"></a>Filtering

İçeri aktarmak istediğiniz Active Directory topoloji boyutunu sağlama altyapısı iç bileşenleri sürer performans ve genel süresini etkileyen bir numaralı faktördür.

[Filtreleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering) nesneleri eşitlenen azaltmak için kullanılmalıdır. Bu, gereksiz nesnelerin işlenmesi ve Azure AD'ye aktarılan engeller. Tercih sırasına göre filtreleme şu teknikler kullanılabilir:



- **Etki alanı tabanlı filtreleme** – Azure AD ile eşitlemek için özel etki alanları seçmek için bu seçeneği kullanın. Ekleme ve Azure AD Connect Eşitleme'yi yükledikten sonra şirket içi altyapınızı değişiklikler yaptığınızda etki alanları eşitleme altyapısı yapılandırmasından kaldırmanız gerekir.
- **Kuruluş birimi (OU) filtreleme** -belirli nesneleri Azure AD'ye sağlama için Active Directory etki alanlarında hedeflemek için OU'ları kullanır. OU filtreleme mekanizması, filtreleme, basit LDAP kapsam sorgularının daha küçük bir alt nesnelerin Active Directory'den içeri aktarmak için kullandığı için önerilen saniyedir.
- **Öznitelik nesne filtreleme** -belirli bir nesneyi Active Directory'de Azure AD'de sağlanıp sağlanmadığını karar vermek için nesneler üzerinde öznitelik değerleri kullanır. Öznitelik filtreleme, etki alanı ve OU filtreleme karşılamadığında, belirli bir filtreleme gereksinimleri, filtrelerinizi ince ayar yapmak için idealdir. Öznitelik filtreleme değil alma süresini azaltabilir, ancak eşitleme azaltmak ve kez dışarı aktarabilirsiniz.
- **Grup tabanlı filtreleme** -kullanan Grup üyeliği, nesneler Azure AD'de sağlanan olmadığını karar vermek için. Grup tabanlı filtreleme yalnızca test durumlar için uygundur ve üretim için ek yükü grup üyeliğini eşitleme döngüsü sırasında denetlemek için gereken ek nedeniyle önerilmez.

Kalıcı birçok [ayırıcı nesneleri](concept-azure-ad-connect-sync-architecture.md#relationships-between-staging-objects-and-metaverse-objects) sağlama altyapısı eşitleme döngüsünde mümkün bağlantı için her bir ayırıcı nesnesi yeniden değerlendirmek için Active Directory CS'yi uzun eşitleme sürelerini neden olabilir. Bu sorunu gidermek için aşağıdakilerden birini aşağıdaki önerileri göz önünde bulundurun:



- Etki alanı veya OU filtreleme kullanarak içeri aktarma için kapsam dışına ayırıcı nesneleri yerleştirin.
- Proje/kümesi ve MV nesneleri Birleştir [cloudFiltered](how-to-connect-sync-configure-filtering.md#negative-filtering-do-not-sync-these) özniteliği, bu nesnelerin Azure AD CS sağlama önlemek için True olmalıdır.

> [!NOTE]
> Kullanıcılar karıştırılması veya çok fazla nesne filtrelendiğinde uygulama izinleri sorunlar ortaya çıkabilir. Örneğin, karma bir Exchange online uygulama, kullanıcılar şirket içi posta kutularıyla kutularıyla kullanıcılara kendi genel adres listesinde daha fazla kullanıcı Exchange çevrimiçi görür. Diğer durumlarda, bir kullanıcı erişimi bir bulut uygulamasında filtrelenmiş bir kümesi nesnelerin kapsamını bir parçası olmayan başka bir kullanıcıya vermek isteyebilirsiniz.

### <a name="attribute-flows"></a>Öznitelik akışları

Öznitelik akışları kopyalama veya bağlı başka bir dizine bağlı bir alt dizinden nesnelerin öznitelik değerleri dönüştürme işlemi var. Eşitleme kuralları bir parçası olarak tanımlı. Örneğin, bir kullanıcının telefon numarasını Active Directory'nizde değiştirildiğinde, Azure AD'de telefon numarasını güncelleştirilecektir. Kuruluşlar [öznitelik akışları değiştirme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-change-the-configuration) Suite çeşitli gereksinimleri. Bunları değiştirmeden önce mevcut öznitelik akışları kopyalamanız önerilir.

Farklı bir öznitelik için bir öznitelik değeri akışa malzeme performans etkisi yok gibi basit yeniden yönlendirmeleri. Bir yeniden yönlendirme örneği için ofis telefon numarası cep telefonu numarası Active Directory'de Azure AD'de akar.

Öznitelik değerleri dönüştürme için eşitleme işlemi üzerinde bir performans etkisi olabilir. Öznitelik değerleri dönüştürme özniteliklerinin değerlerini çıkararak değiştirme, yeniden biçimlendirme, birleştirme veya içerir.

Kuruluşlar Azure AD'ye akışına belirli öznitelikleri engel olabilir, ancak sağlama altyapısının performansını etkileyen olmaz.

> [!NOTE]
> Eşitleme kurallarınızdaki istenmeyen öznitelik akışları silmeyin. Silinen kuralları, Azure AD Connect yükseltmeler sırasında oluşturulur çünkü yerine bunları, devre dışı önerilir.

## <a name="azure-ad-connect-dependency-factors"></a>Azure AD Connect bağımlılık faktörleri

Azure AD Connect performansını içeri aktarır ve verir bağlı dizinleri performansını bağlıdır. Örneğin, içeri aktarmak için gereken Active Directory veya Azure AD hizmeti ağ gecikme süresini boyutu. Sağlama altyapısını kullanan SQL veritabanı, aynı zamanda eşitleme döngüsü genel performansını etkiler.

### <a name="active-directory-factors"></a>Active Directory faktörleri

Daha önce belirtildiği gibi alınacak nesne sayısı performansını önemli ölçüde etkiler. [Donanım ve Azure AD Connect önkoşulları](how-to-connect-install-prerequisites.md) dağıtımınızın boyutuna göre belirli donanım katmanda özetler. Azure AD Connect açıklandığı gibi belirli topolojileri yalnızca Destek [Azure AD Connect için topolojiler](plan-connect-topologies.md). Hiçbir performans iyileştirmeleri ve desteklenmeyen topolojiler için öneriler bulunmaktadır.

Azure AD Connect sunucunuzu içeri aktarmak istediğiniz, Active Directory boyutuna göre donanım gereksinimlerini karşıladığından emin olun. Azure AD Connect sunucusu ve Active Directory etki alanı denetleyicilerinizin arasında hatalı veya yavaş ağ bağlantısı içeri aktarma işleminiz yavaşlatabilir.

### <a name="azure-ad-factors"></a>Azure AD faktörleri

Azure AD bulut hizmeti, hizmet reddi (DoS) saldırılarına karşı korumak için azaltma kullanır. Şu anda Azure AD, 7000 yazma başına 5 dakika (saat başına 84,000) azaltma sınırı vardır. Örneğin, aşağıdaki işlemleri kısıtlanabilir mi:



- Azure AD için Azure AD Connect dışarı aktarma.
- PowerShell betikleri veya doğrudan Azure AD'ye güncelleştirme uygulamaları, dinamik grup üyeliği gibi arka planda bile.
- Kullanıcılar için mfa'yı veya SSPR (Self Servis parola sıfırlama) kaydetme gibi kendi kimlik kayıtları güncelleştirme.
- Grafik kullanıcı arabirimi işlemlerini.

Azure AD Connect eşitleme döngünüz sınırları kısıtlanarak etkilenmez emin olmak için dağıtım ve bakım görevlerini planlama. Örneğin, kullanıcı kimliklerini binlerce oluşturacağınız büyük bir işe alma wave varsa, bu güncelleştirmeleri lisans atamaları, dinamik grup üyeliği için neden olabilir ve Self Servis parola sıfırlama kaydı. Bu yazma birkaç saat veya birkaç gün içine yaymak daha iyidir.

### <a name="sql-database-factors"></a>SQL veritabanı faktörleri

Kaynak Active Directory topolojiniz boyutu, SQL veritabanı performansı etkiler. İzleyin [donanım gereksinimleri](how-to-connect-install-prerequisites.md) SQL Server veritabanı ve aşağıdaki önerileri göz önünde bulundurun:



- 100\. 000'den fazla kullanıcısı olan kuruluşlar, ağ gecikme süreleriyle birlikte bulundurma SQL veritabanı ile aynı sunucuda sağlama altyapısı tarafından azaltabilir.
- Giriş ve çıkış yüksek disk nedeniyle eşitleme işlemi (g/ç) gereksinimlerini kullanın Katı Hal sürücüleri (SSD) için RAID 0 ya da RAID 1 yapılandırmaları mümkün değilse, SQL veritabanı sağlama altyapısının en iyi sonuçlar için göz önünde bulundurun.
- Tam eşitleme bilindiğinde desteklemez; gereksiz karmaşıklık ve daha yavaş yanıt sürelerine neden olur.

## <a name="conclusion"></a>Sonuç

Azure AD Connect uygulamanızın performansını iyileştirmek için aşağıdaki önerileri göz önünde bulundurun:



- Kullanım [donanım yapılandırması önerilen](how-to-connect-install-prerequisites.md) Azure AD Connect sunucusu için uygulama boyutuna bağlı.
- Azure AD Connect büyük ölçekli dağıtımlarda yükseltirken kullanmayı [swing geçişi yöntemi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version#swing-migration), en az kapalı kalma süresi ve güvenilirlik en iyi olduğundan emin olun. 
- SQL veritabanı için en iyi performans yazmak için SSD kullanın.
- Active Directory kapsamı yalnızca etki alanı, OU ve öznitelik filtreleme kullanarak Azure AD'de sağlanması gereken nesneleri içerecek şekilde filtreleyin.
- Varsayılan öznitelik akışı kuralları değiştirmeniz gerekiyorsa, ilk kuralı kopyalamak, sonra kopyayı değiştirmek ve özgün kuralı devre dışı bırak. Tam eşitleme yeniden unutmayın.
- İlk tam eşitleme çalıştırma profili için yeterli zaman planlayın.
- Delta eşitleme döngüsü 30 dakika içinde tamamlamak çaba harcar. Delta eşitleme profili 30 dakika içerisinde tamamlanmazsa, varsayılan eşitleme sıklığı, tam delta eşitleme döngüsü içerecek şekilde değiştirin.
- İzleyici, [Azure AD Connect eşitleme durumu](how-to-connect-health-agent-install.md) Azure AD'de.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
