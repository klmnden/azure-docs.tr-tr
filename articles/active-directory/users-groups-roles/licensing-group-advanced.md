---
title: Grup tabanlı lisanslama ek senaryoları - Azure Active Directory | Microsoft Docs
description: Azure Active Directory grup tabanlı lisanslama için daha fazla senaryo
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24bf8e7cf103d583cf6604e0c529ad4ea267ce84
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60471905"
---
# <a name="scenarios-limitations-and-known-issues-using-groups-to-manage-licensing-in-azure-active-directory"></a>Senaryoları, sınırlamalar ve bilinen sorunlar Azure Active Directory'de lisanslama yönetmek için grupları kullanma

Azure Active Directory (Azure AD), Grup tabanlı lisanslama daha gelişmiş anlamak için aşağıdaki bilgi ve örnekler'i kullanın.

## <a name="usage-location"></a>Kullanım konumu

Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Yönetici bir kullanıcıya lisans atanabilmesi için önce belirtmesi gerekir **kullanım konumu** kullanıcı özelliği. İçinde [Azure portalında](https://portal.azure.com), belirleyebilirsiniz **kullanıcı** &gt; **profili** &gt; **ayarları**.

Grup lisansı atama için kullanım konumu belirtilmemiş olmadan herhangi bir kullanıcı dizin konumunu devralır. Birden fazla konumda kullanıcılarınız varsa, lisans gruplarına kullanıcıları eklemeden önce kullanıcı, nesneyi doğru bir şekilde yansıtan emin olun.

> [!NOTE]
> Grup lisansı atama hiçbir zaman bir kullanıcının varolan bir kullanım konumu değeri değiştirir. Lisans ataması sonucu garanti eder (örneğin aracılığıyla yapılandırması) AAD Connect - Azure AD'de kullanıcı oluşturma akışınızı parçası her zaman doğru olduğu kullanım konumu her zaman ayarlayın ve kullanıcıların izin verilmeyen konumlarda Hizmetleri almazsınız öneririz.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Grup tabanlı lisanslama ile dinamik grupları kullanma

Azure AD dinamik grupları ile birleştirilmesi yani herhangi bir güvenlik grubuyla grup tabanlı lisanslama kullanabilirsiniz. Dinamik gruplar kuralları otomatik olarak ekleyin ve kullanıcılar gruptan kaldırmak için nesne öznitelikleri karşı kullanıcı çalıştırın.

Örneğin, bazı kullanıcılara atamak istediğiniz ürünleri kümesi için dinamik bir grup oluşturabilirsiniz. Her grup, kullanıcı tarafından özniteliklerini ekleme kuralı tarafından doldurulur ve her grup, almak istediğiniz lisans atanır. Şirket özniteliği atamak ve Azure AD ile eşitleme veya doğrudan bulutta öznitelik yönetebilirsiniz.

Kısa bir süre sonra gruba eklendikten sonra kullanıcıya lisans atandı. Öznitelik değiştirildiğinde, kullanıcı grupları bırakır ve lisansları kaldırılır.

### <a name="example"></a>Örnek

Hangi kullanıcıların Microsoft web hizmetlerine erişimi karar bir şirket içi kimlik yönetimi çözümü örneği göz önünde bulundurun. Kullandığı **extensionAttribute1** kullanıcı olmalıdır lisansları temsil eden bir dize değeri depolamak için. Azure AD Connect, Azure AD ile eşitler.

Kullanıcılar bir gerekebilir ancak değil başka bir lisans ya da her ikisine birden ihtiyacınız. İçinde dağıtıyorsanız Office 365 Kurumsal E5 ve Enterprise Mobility + Security (EMS) lisansı gruplardaki kullanıcılara, bir örnek aşağıda verilmiştir:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Kurumsal E5: temel Hizmetleri

![Ekran Office 365 Kurumsal E5 temel Hizmetleri](./media/licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: lisanslı kullanıcılar

![Ekran görüntüsü, Enterprise Mobility + Security, kullanıcıların lisansına sahip](./media/licensing-group-advanced/o365-e5-licensed-users.png)

Bu örnekte, bir kullanıcıyı değiştirmek ve kendi extensionAttribute1 değerine ayarlanmasından `EMS;E5_baseservices;` kullanıcının her iki lisans sahip olmasını istiyorsanız. Bu değişikliği yapmak şirket içi. Değişiklik bulutla eşitler sonra kullanıcı her iki grubuna otomatik olarak eklenir ve atanmış lisansların.

![Kullanıcının extensionAttribute1 ayarlama işlemini gösteren ekran görüntüsü](./media/licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Mevcut bir grubun üyelik kuralı değiştirirken dikkatli olun. Bir kural değiştirildiğinde, grubun üyeliğini yeniden değerlendirileceğini ve artık yeni kural eşleşen kullanıcılar (yeni kural, bu işlem sırasında etkilenmez eşleşme hala kaldırılan kullanıcılar) olacaktır. Bu kullanıcıların hizmet kaybı veya bazı durumlarda, neden olabilir işlemi sırasında veri kaybı kaldırılan kullanıcıların lisansları gerekir.
> 
> Lisans ataması için bağımlı bir büyük dinamik grubu varsa, ana gruba uygulamadan önce herhangi bir önemli değişiklik daha küçük bir test grubu doğrulanıyor göz önünde bulundurun.

## <a name="multiple-groups-and-multiple-licenses"></a>Birden çok grup ve birden çok lisans

Bir kullanıcı birden fazla lisans grubu üyesi olabilir. Dikkat etmeniz gerekenler şunlardır:

- Aynı ürün için birden çok lisans binebilir ve kullanıcıya uygulanan tüm etkin hizmetler sonuçlanır. Aşağıdaki örnek, iki lisans grupları gösterir: *E3 temel Hizmetleri* ilk olarak, tüm kullanıcılara dağıtmak için foundation hizmetleri içerir. Ve *Hizmetleri genişletilmiş E3* (Sway ve Planner) yalnızca bazı kullanıcılara dağıtmak için ek hizmetleri içerir. Bu örnekte, kullanıcı her iki grubuna eklendi:

  ![Etkin hizmetler ekran görüntüsü](./media/licensing-group-advanced/view-enabled-services.png)

  Sonuç olarak, kullanıcının etkin, yalnızca bir lisans bu ürün için kullanırken ürünü 7 Hizmetler 12 vardır.

- Seçme *E3* lisans grupları hakkında nedeni ne Hizmetleri kullanıcı için etkin için bilgiler dahil olmak üzere daha fazla ayrıntı gösterilmektedir.

  ![Gruba göre etkin hizmetler ekran görüntüsü](./media/licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Grup lisansları ile doğrudan lisansları bir arada var

Bir kullanıcı bir lisans bir gruptan devraldığında doğrudan kaldıramaz veya kullanıcının özelliklerini bu lisans atamasını değiştirin. Değişiklikleri grubunda yapılan ve tüm kullanıcılar'ı yayılır.

Ancak, aynı ürün lisanslarını doğrudan, devralınmış lisans yanı sıra kullanıcı atamak için mümkündür. Diğer kullanıcıları etkilemeden yalnızca bir kullanıcı için ek hizmetler ürünün etkinleştirebilirsiniz.

Atanan lisansları kaldırılabilir ve etkilemeyen doğrudan lisansları devralındı. Bir Office 365 Kurumsal E3 lisans bir gruptan devralınan kullanıcı göz önünde bulundurun.

1. Kullanıcı lisansı yalnızca başlangıçta devralır *E3 temel Hizmetleri* gösterildiği gibi dört hizmet planları, sağlayan Grup:

   ![Ekran görüntüsü, E3 etkin bir grup Hizmetleri](./media/licensing-group-advanced/e3-group-enabled-services.png)

2. Seçebileceğiniz **atama** doğrudan kullanıcıya bir E3 lisansı atamak için. Bu durumda, Yammer kuruluş dışındaki tüm hizmet planları devre dışı bırakmak için yükleyeceksiniz:

   ![Bir kullanıcıya lisans atamak nasıl ekran görüntüsü](./media/licensing-group-advanced/assign-license-to-user.png)

3. Sonuç olarak, kullanıcı hala E3 ürünün yalnızca bir lisans kullanır. Ancak yalnızca söz konusu kullanıcı için Kurumsal Yammer service doğrudan atanmasına olanak tanır. Grup üyeliği doğrudan atama ile hangi hizmetler etkinleştirilir görebilirsiniz:

   ![Ekran görüntüsü doğrudan atama devralındı](./media/licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Doğrudan atamayı kullandığınızda, aşağıdaki işlemleri izin verilir:

   - Yammer Kurumsal kullanıcı nesnesindeki doğrudan kapatılabilir. **Açık/kapalı** geçiş çizimde, bir hizmeti değiştirir aksine bu hizmet için etkinleştirildi. Hizmet kullanıcı doğrudan etkinleştirilmiş olduğundan, değiştirilebilir.
   - Ek hizmetler de, doğrudan atanan lisans bir parçası olarak etkinleştirilebilir.
   - **Kaldır** düğmesi, kullanıcıdan doğrudan lisans kaldırmak için kullanılabilir. Kullanıcı artık yalnızca devralınan Grup lisansı sahiptir ve yalnızca özgün Hizmetleri etkin kalmaya devam görebilirsiniz:

     ![Doğrudan atamayı kaldırma konusunda gösteren ekran görüntüsü](./media/licensing-group-advanced/remove-direct-license.png)

## <a name="managing-new-services-added-to-products"></a>Ürünler için eklenen yeni hizmetler yönetme
Microsoft, yeni bir hizmet için bir ürün eklediğinde, ürün lisansı atadığınız tüm gruplar halinde varsayılan olarak etkinleştirilir. İçin ürün değişikliklerle ilgili Bildirimlere abone kiracınızdaki kullanıcılar önceden gelecek hizmet eklemeleri hakkında bildiren e-posta alırsınız.

Yönetici olarak, değişiklikten etkilenen tüm gruplarını gözden geçirin ve her gruptaki yeni hizmeti devre dışı bırakma gibi bir işlemi. Örneğin, yalnızca belirli hizmetleri dağıtımı için hedef grupları oluşturduysanız, bu grupları yeniden ziyaret ve tüm hizmetleri devre dışı bırakıldığında eklenen emin emin olun.

Bu işlem aşağıdaki gibi görünebilir bir örnek aşağıda verilmiştir:

1. İlk olarak, size atanan *Office 365 Kurumsal E5* ürün için çeşitli gruplar. Olarak adlandırılan, bu gruplardan birini *O365 E5 - yalnızca Exchange* yalnızca sağlamak için tasarlanmıştır *Exchange Online (Plan 2)* üyeleri için hizmet.

2. Yeni bir hizmetle - E5 ürün genişletilir Microsoft'tan bir bildirim aldı *Microsoft Stream*. Hizmet kiracınızda kullanıma sunulduğunda, aşağıdakileri yapabilirsiniz:

3. Git [ **Azure Active Directory > lisansları > tüm ürünler** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) dikey penceresinde ve seçin *Office 365 Kurumsal E5*, ardından **lisanslı gruplar** Bu ürünle birlikte tüm grupların listesini görüntülemek için.

4. Gözden geçirmek istediğiniz grubun tıklayın (Bu durumda, *O365 E5 - yalnızca Exchange*). Bu açılır **lisansları** sekmesi. E5 lisansı tıklandığında, tüm etkin hizmetler listeleyen bir dikey pencere açılır.
   > [!NOTE]
   > *Microsoft Stream* hizmeti olduğundan otomatik olarak eklenir ve ek olarak bu grup, etkin *Exchange Online* hizmeti:

   ![Grup lisansı eklenen yeni hizmet ekran görüntüsü](./media/licensing-group-advanced/manage-new-services.png)

5. Bu gruba yeni hizmeti devre dışı bırakmak isterseniz **açık/kapalı** geçiş hizmeti yanındaki ve tıklayın **Kaydet** Değişikliğini Onayla düğmesine. Azure AD değişikliği uygulamak için gruptaki tüm kullanıcılar artık işleyecektir; gruba eklenen tüm yeni kullanıcılar olmayacaktır *Microsoft Stream* hizmeti etkinleştirildi.

   > [!NOTE]
   > Kullanıcılar yine de bazı diğer lisans Ataması (başka bir grubu üyeleri veya doğrudan lisans ataması olmaları) etkin hizmet olabilir.

6. Gerekirse aynı adımları diğer gruplar için atanan bu ürünle gerçekleştirin.

## <a name="use-powershell-to-see-who-has-inherited-and-direct-licenses"></a>Kimin devralmıştır ve doğrudan lisansları görmek için PowerShell kullanma
Kullanıcıların doğrudan atanan veya bir gruptan devralınan bir lisans olup olmadığını denetlemek için bir PowerShell Betiği kullanabilirsiniz.

1. Çalıştırma `connect-msolservice` kimlik doğrulaması ve kiracınıza bağlamak için cmdlet'i.

2. `Get-MsolAccountSku` kiracıdaki tüm sağlanan ürün lisansları bulmak için kullanılabilir.

   ![Get-Msolaccountsku cmdlet'inin ekran görüntüsü](./media/licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. Kullanım *AccountSkuId* değeri ile ilgilenen lisans [bu PowerShell Betiği](licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group). Bu lisans nasıl atanacağını hakkında bilgileri içeren bu lisansına sahip kullanıcıları içeren bir liste oluşturur.

## <a name="use-audit-logs-to-monitor-group-based-licensing-activity"></a>Grup tabanlı lisanslama etkinliğini izlemek için denetim günlüklerini kullanın.

Kullanabileceğiniz [Azure AD denetim günlükleri](../reports-monitoring/concept-audit-logs.md#audit-logs) tüm etkinlik görmek için ilgili grup tabanlı lisanslama için de dahil olmak üzere:
- kimin gruplardaki lisansları değişti
- sistem Grup lisans değişikliği işlem başlatıldığında ve bittiğinde,
- Grup lisansı atama sonucu olarak bir kullanıcıya hangi lisans değişiklikler yapıldı.

>[!NOTE]
> Denetim günlükleri çoğu dikey pencereleri portalı Azure Active Directory bölümünde kullanılabilir. Bunları eriştiğinize bağlı olarak, filtre yalnızca etkinlik dikey penceresinin içeriği ilgili gösterecek şekilde önceden uygulanmış olabilir. Beklediğiniz sonuçları görmediğinizden, inceleyin [filtreleme seçenekleri](../reports-monitoring/concept-audit-logs.md#filtering-audit-logs) veya altında filtrelenmemiş denetim günlüklerine erişmek [ **Azure Active Directory > etkinlik > Denetim günlükleri** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit).

### <a name="find-out-who-modified-a-group-license"></a>Grup lisansı kimin değiştirdiğini kullanıma Bul

1. Ayarlama **etkinlik** filtre *Grup lisansı ayarlama* tıklatıp **Uygula**.
2. Sonuçlar ayarlamak veya değiştirilmiş grup üzerinde lisansları tüm örneklerini içerir.
   >[!TIP]
   > Grup içinde adını da yazabilirsiniz *hedef* sonuçları kapsamını belirlemek için filtre.

3. Nelerin değiştiğini ayrıntılarını görmek için liste görünümünde bir öğeye tıklayın. Altında *değiştirilmiş özellikleri* lisans ataması için eski ve yeni değerler listelenmiştir.

Son Grup lisans değişikliklerinin ayrıntıları içeren bir örnek aşağıda verilmiştir:

![Ekran grubu lisans değişiklikleri](./media/licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Grup değişikliklerini başlatıldığında ve işleme tamamlandı öğrenin

Bir gruba lisans değişiklikleri Azure AD değişiklikleri tüm kullanıcılara uygulama başlar.

1. Grupları işlem başlatıldığında görmek için ayarlanmış **etkinlik** filtre *kullanıcılara grup tabanlı lisans uygulama Başlat*. İşlem için aktör olduğuna dikkat edin *Microsoft Azure AD grup tabanlı lisanslama* -tüm Grup lisans değişiklikleri yürütmek için kullanılan bir sistem hesabı.
   >[!TIP]
   > Görmek için listedeki bir öğeye tıklayın *değiştirilmiş özellikleri* alan - işleme için çekilen lisans değişiklikleri gösterir. Bu, bir grup için birden çok değişiklik ve hangisinin işlendiği emin değilseniz yararlı olur.

2. Benzer şekilde, grupları işleme bittiğinde görmek için filtre değeri kullanın. *kullanıcılara grup tabanlı lisans uygulamayı sonlandırma*.
   > [!TIP]
   > Bu durumda, *değiştirilmiş özellikleri* sonuçlarının bir özetini içeren alan - bu işleme herhangi bir hata ile sonuçlandı, hızlı bir şekilde denetlemek yararlıdır. Örnek çıktı:
   > ```
   > Modified Properties
   > ...
   > Name : Result
   > Old Value : []
   > New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
   > ```

3. Nasıl bir grup, tüm kullanıcı değişikliklerini de dahil olmak üzere işlendiği için günlüğün tamamını görmek için aşağıdaki filtreleri ayarlayın:
   - **Başlatan (aktör)** : "Microsoft Azure AD grup tabanlı Lisanslama"
   - **Tarih aralığı** (isteğe bağlı): belirli bir grup bildiğinizde özel aralığını kullanmaya ve işlem tamamlandı

Bu örnek çıkışa işlemi, sonuçlanan tüm kullanıcı değişiklikler ve işleme bitiş başlangıcını gösterir.

![Ekran grubu lisans değişiklikleri](./media/licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> İlgili öğeleri tıklayarak *kullanıcı lisansını değiştirme* her kullanıcıya uygulanan lisans değişikliklerinin ayrıntılarını gösterir.

## <a name="deleting-a-group-with-an-assigned-license"></a>Atanmış bir lisansı olan bir grubu siliniyor

Atanan etkin bir lisansa sahip bir grubu silmek mümkün değildir. Yönetici kullanıcılardan - silinebilmesi için önce ilk olarak, gruptan kaldırılacak herhangi bir lisans isteriz. Bu nedenle kaldırılacak lisansları neden fark değil bir grup silinemedi.

Azure portalında bir grubu silmek çalışırken bir hata bildirimi bu gibi görebilirsiniz: ![Ekran grubu silme işlemi başarısız oldu](./media/licensing-group-advanced/groupdeletionfailed.png)

Git **lisansları** sekme grubunda ve tüm lisansları atanmış olup olmadığını. Yanıt Evet ise, bu lisansları kaldırın ve grubu yeniden silmeyi deneyin.

PowerShell veya Graph API'si ile bir grubu silmek çalışırken, benzer hatalar görebilirsiniz. Şirket içi ad'nizden eşitlenmiş bir grubu kullanıyorsanız, Azure AD'de grubu silmek başarısız olduysa Azure AD Connect'i ayrıca hatalar bildirebilir. Böyle durumlarda grubuna atanmış herhangi bir lisans olup olmadığını kontrol ettiğinizden emin olun ve önce kaldırın.

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar

Grup tabanlı lisanslama kullanıyorsanız, aşağıdaki sınırlamalar ve bilinen sorunların listesi ile tanımak için iyi bir fikirdir.

- Şu anda grup tabanlı lisanslama diğer gruplar (iç içe geçmiş gruplar) içeren gruplar desteklemez. İçine yerleştirilmiş başka bir grup olan bir gruba lisans uyguladığınızda yalnızca grubun birinci düzeyindeki üyelerine lisans atanır.

- Bu özellik, yalnızca güvenlik grupları ve securityEnabled olan Office 365 grupları ile kullanılabilir = TRUE.

- [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com) grup tabanlı lisanslama şu anda desteklemiyor. Bir kullanıcı bir lisans bir gruptan devralınır. Bu lisans normal kullanıcı lisansı olarak Office Yönetim Portalı'nda görüntülenir. Lisans değiştirmek ya da lisans kaldırmaya denerseniz, portal bir hata iletisi döndürür. Devralınan Grup lisansları bir kullanıcı doğrudan değiştirilemez.

- Lisansları atanmış veya büyük bir grup (örneğin, 100.000 kullanıcı) için değiştirilmiş, performans olumsuz etkilenebilir. Özellikle, Azure AD Otomasyonu tarafından oluşturulan değişiklik hacmini, Azure AD arasında dizin eşitlemesi performansını olumsuz etkileyebilir ve şirket içi sistemler.

- Kullanıcılarınızın üyeliklerini yönetmek için dinamik grupları kullanıyorsanız kullanıcının grubun bir parçası olduğunu doğrulayın. Bu durum lisans atama için gereklidir. Aksi takdirde dinamik grubun [üyelik kuralı işleme durumunu denetleyin](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule). 

- Belirli yüksek yük durumlarda bu grup için lisans değişiklikleri veya var olan lisans gruplarına üyelik değişiklikleri işlemek için bir uzun zaman alabilir. Görürseniz değişikliklerinizi grubu boyutu 60 K kullanıcılar veya less, lütfen işlemek için birden fazla 24 saat sürebilir. [bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/supportRequest) araştırmak bize izin vermek için. 

- Lisans Yönetimi Otomasyonu otomatik olarak her türlü ortamda değişikliği için tepki vermez. Örneğin, lisans, bir hata durumunda bazı kullanıcıların neden çalıştırdığınız. İçin kullanılabilir lisans sayısı, boş, bazı doğrudan atanan lisansları diğer kullanıcılardan kaldırabilirsiniz. Ancak, sistem otomatik olarak bu değişiklik react ve hata durumundaki kullanıcıların düzeltin.

  Sınırlamalar bu türde bir geçici çözüm olarak, gidebilirsiniz **grubu** dikey penceresinde Azure AD'de tıklatıp **suretiyle**. Bu komut, bu gruptaki tüm kullanıcılar işler ve hata durumları mümkünse giderir.

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisanslama aracılığıyla lisans yönetimine yönelik diğer senaryolar hakkında daha fazla bilgi edinmek için bkz:

* [Grup tabanlı Azure Active Directory lisansı nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de gruba lisans atama](licensing-groups-assign.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](licensing-groups-migrate-users.md)
* [Kullanıcılar Azure Active Directory'de Grup tabanlı lisanslama kullanarak ürün lisansları arasında geçirme](../users-groups-roles/licensing-groups-change-licenses.md)
* [Azure Active Directory'de Grup tabanlı lisanslama için PowerShell örnekleri](../users-groups-roles/licensing-ps-examples.md)
