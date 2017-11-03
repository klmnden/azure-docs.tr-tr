---
title: "Azure Active Directory grup tabanlı ilave senaryolar lisanslama | Microsoft Docs"
description: "Azure Active Directory grup tabanlı lisans için daha fazla senaryoları"
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: 
author: curtand
manager: femila
editor: piotrci
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/02/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 75cafa6868d54f9d8a7e0dbe9f2a9e85ed43f16f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="scenarios-limitations-and-known-issues-using-groups-to-manage-licensing-in-azure-active-directory"></a>Senaryoları, sınırlamaları ve bilinen sorunlar Azure Active Directory'de lisans işlemlerini yönetmek için grupları kullanma

Azure Active Directory (Azure AD) grup tabanlı lisans daha gelişmiş anlamak için aşağıdaki bilgi ve örnekler kullanın.

## <a name="usage-location"></a>Kullanım konumu

Bazı Microsoft Hizmetleri tüm konumlarda kullanılabilir değil. Bir kullanıcıya bir lisans atanabilmesi için önce belirtmek yönetici olan **kullanım konumu** kullanıcı özelliği. İçinde [Azure portalı](https://portal.azure.com), belirleyebilirsiniz **kullanıcı** &gt; **profil** &gt; **ayarları**.

Grup lisans atama için belirtilen bir kullanım konumu olmayan tüm kullanıcılar dizininin konumunu devralır. Birden çok konumda kullanıcılar varsa, bu lisansların gruplarına kullanıcıları eklemeden önce doğru kullanıcı nesnelerinizi yansıtacak şekilde emin olun.

> [!NOTE]
> Grup lisans atamasını hiçbir zaman bir kullanıcı için kullanım konumu var olan bir değerle değiştirecek. Lisans atama sonucu garanti eder (örneğin AAD Connect yapılandırması aracılığıyla) - Azure AD, kullanıcı oluşturma akışı parçası her zaman doğru olduğu gibi kullanım konumu her zaman ayarlayın ve kullanıcılar izin verilmeyen konumlara Hizmetleri almaz öneririz.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Grup tabanlı dinamik gruplarla lisans kullanın

Azure AD dinamik gruplarla birleştirilebilir yani herhangi bir güvenlik grubuyla grup tabanlı lisans kullanabilirsiniz. Dinamik grupların kuralları otomatik olarak ekleyebilir ve kullanıcıları gruplarından kaldırmak için nesne özniteliği kullanıcı karşı çalıştırın.

Örneğin, bazı kullanıcılara atamak istediğiniz ürünleri kümesi için dinamik bir grup oluşturabilirsiniz. Her grup, kullanıcı tarafından öznitelikleriyle ekleme kuralı tarafından doldurulur ve her grup almak istediğiniz lisansı atanır. Öznitelik şirket içi atayın ve Azure AD ile eşitleme veya Bulut özniteliğinde doğrudan yönetebilirsiniz.

Kısa süre içinde grubuna eklendikten sonra lisansları kullanıcıya atanır. Öznitelik değiştiğinde, kullanıcı grupları bırakır ve lisanslar kaldırılıyor.

### <a name="example"></a>Örnek

Hangi kullanıcıların Microsoft web hizmetlerine erişimi karar bir şirket içi kimlik yönetimi çözümü örneği göz önünde bulundurun. Kullandığı **extensionAttribute1** kullanıcı olmalıdır lisansları gösteren bir string değeri depolamak için. Azure AD Connect, Azure AD ile eşitlenir.

Kullanıcılar bir gerekebilir ancak olmayan başka bir lisans ya da her ikisini de gerekebilir. İçinde dağıtıyorsanız Office 365 Kurumsal E5 ve Enterprise Mobility + gruplardaki kullanıcılara güvenlik (EMS) lisansı örneği şöyledir:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Kurumsal E5: temel Hizmetleri

![Ekran Office 365 Kurumsal E5 temel Hizmetleri](media/active-directory-licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: lisanslı kullanıcılar

![Ekran görüntüsü, Enterprise Mobility + Security kullanıcıları lisanslı](media/active-directory-licensing-group-advanced/o365-e5-licensed-users.png)

Bu örnek için bir kullanıcı değiştirebilir ve kendi extensionAttribute1 değerine ayarlanmış `EMS;E5_baseservices;` hem lisanslara sahip kullanıcıya istiyorsanız. Bu değişikliği yapmak şirket içi. Değişiklik Bulutu ile eşitlenen sonra kullanıcı her iki gruba da otomatik olarak eklenir ve lisansları atanır.

![Kullanıcının extensionAttribute1 nasıl kurulur gösteren ekran görüntüsü](media/active-directory-licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Varolan bir grubun üyeliğini kuralını değiştirirken dikkatli olun. Bir kural değiştiğinde, Grup üyeliğini yeniden değerlendirileceğini ve artık yeni kural eşleşen kullanıcıları (yeni kural bu işlem sırasında etkilenmez eşleşme hala kaldırılan kullanıcılar) olacaktır. Bu kullanıcılar hizmet kaybı veya bazı durumlarda sonuçlanabilir işlemi sırasında veri kaybı kaldırılan lisanslarını sahip olacaktır.

> Lisans atama için bağımlı büyük dinamik bir grup varsa, ana grubuna uygulamadan önce önemli değişikliklere daha küçük bir test grubu üzerinde doğrulama göz önünde bulundurun.

## <a name="multiple-groups-and-multiple-licenses"></a>Birden çok grubu ve birden çok lisans

Bir kullanıcı lisansları sahip birden fazla grup üyesi olabilir. Dikkate alınması gereken bazı noktalar şunlardır:

- Aynı ürün için birden fazla lisans binebilir ve kullanıcıya uygulanan tüm etkin hizmetleri sonuçlanır. Aşağıdaki örnek, iki lisans grupları gösterir: *E3 temel Hizmetleri* ilk olarak, tüm kullanıcılara dağıtmak için foundation hizmetleri içerir. Ve *Hizmetleri genişletilmiş E3* (Sway ve Planlayıcısı) yalnızca bazı kullanıcılara dağıtmak için ek hizmetleri içerir. Bu örnekte, kullanıcı her iki gruba da eklendi:

  ![Etkin hizmetleri ekran görüntüsü](media/active-directory-licensing-group-advanced/view-enabled-services.png)

  Sonuç olarak, kullanıcı etkinleştirilirse, bu ürün için yalnızca bir lisans kullanırken Ürün 12 Hizmetleri 7 sahiptir.

- Seçme *E3* lisans grupları neden hakkında ne Hizmetleri kullanıcı için etkinleştirilmesi için bilgi dahil daha fazla ayrıntı gösterir.

  ![Grup tarafından etkinleştirilmiş hizmetler ekran görüntüsü](media/active-directory-licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Grup lisans doğrudan lisansları bir arada var

Bir kullanıcı bir lisans bir gruptan devralır olduğunda, doğrudan kaldıramaz veya kullanıcının özelliklerini bu lisans atamasını değiştirmek. Değişiklikler grubunda yapılan ve tüm kullanıcılara yayılır.

Ancak, devralınan Lisans ek olarak, doğrudan kullanıcıya aynı Ürün lisans atamak için mümkündür. Diğer kullanıcıları etkilemeden, yalnızca bir kullanıcı için ürün ek hizmetleri etkinleştirebilirsiniz.

Atanan lisansları kaldırılabilir ve etkilemeyen doğrudan lisansları devralınmış. Bir gruptan bir Office 365 Kurumsal E3 lisans devralır kullanıcının göz önünde bulundurun.

1. Başlangıçta, kullanıcı lisansı yalnızca devralır *E3 temel Hizmetleri* gösterildiği gibi dört hizmet planları sağlayan Grup:

  ![Ekran görüntüsü, E3 etkin bir grup Hizmetleri](media/active-directory-licensing-group-advanced/e3-group-enabled-services.png)

2. Seçebileceğiniz **atamak** doğrudan bir E3 lisansı kullanıcıya atamak için. Bu durumda, Yammer kuruluş dışındaki tüm hizmet planları devre dışı olacak:

  ![Doğrudan bir kullanıcıya bir lisans atamak nasıl ekran görüntüsü](media/active-directory-licensing-group-advanced/assign-license-to-user.png)

3. Sonuç olarak, kullanıcı hala E3 ürünün yalnızca bir lisans kullanır. Ancak yalnızca o kullanıcı için Yammer Kurumsal hizmeti doğrudan atanmasına izin verir. Hangi hizmetlerin doğrudan atanmasına karşı grup üyeliği tarafından etkinleştirilen görebilirsiniz:

  ![Ekran görüntüsü karşı doğrudan atanmasına devralınan](media/active-directory-licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Doğrudan atanmasına kullandığınızda, aşağıdaki işlemleri izin verilir:

  - Yammer Kurumsal kullanıcı nesnesindeki doğrudan kapatılabilir. **Açık/kapalı** çizimdeki iki durumlu bir hizmeti değiştirir aksine bu hizmet için etkindir. Hizmeti kullanıcı doğrudan etkinleştirilmiş olduğu için değiştirilebilir.
  - Ek hizmetler de doğrudan atanan lisans bir parçası olarak etkinleştirilebilir.
  - **Kaldırmak** düğmesi, kullanıcıdan doğrudan lisans kaldırmak için kullanılabilir. Kullanıcı artık yalnızca devralınan Grup lisans sahiptir ve yalnızca özgün Hizmetleri etkin kalmaya devam görebilirsiniz:

    ![Doğrudan atamasını kaldırmak nasıl gösteren ekran görüntüsü](media/active-directory-licensing-group-advanced/remove-direct-license.png)

## <a name="managing-new-services-added-to-products"></a>Ürün için eklenen yeni hizmetlerini yönetme
Microsoft ürün için yeni bir hizmet eklediğinde, ürün lisansı atanmış tüm grupları varsayılan olarak etkinleştirilir. Ürün değişikliklerle ilgili Bildirimlere abone kullanıcılar kiracınızda yaklaşan hizmet eklemeleri hakkında bilgilendirmek önceden e-postaları alır.

Yönetici olarak, değişiklikten etkilenen tüm grupları gözden geçirin ve her grubu yeni hizmet devre dışı bırakma gibi eylemi gerçekleştirin. Örneğin, yalnızca belirli hizmetleri dağıtımı için hedef grupları oluşturduysanız, bu grupları yeniden ziyaret ve eklenmiş herhangi bir yeni hizmetlerin devre dışı olduğundan emin olun.

Bu işlem neye benzediğini örneği şöyledir:

1. İlk olarak, size atanan *Office 365 Kurumsal E5* ürün birkaç gruplarına. Adlı bu gruplara birini *O365 E5 - yalnızca Exchange* yalnızca sağlamak için tasarlanmış *Exchange Online (2 planlama)* üyeleri için hizmet.

2. Bir bildirim E5 ürün sahip yeni bir hizmet - uzatılır Microsoft'tan aldığınız *Microsoft Stream*. Hizmet kiracınızda hazır olduğunda, aşağıdakileri yapabilirsiniz:

3. Git [ **Azure Active Directory > lisansları > tüm ürünleri** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) dikey ve seçin *Office 365 Kurumsal E5*seçeneğini belirleyip **lisans grupları** Bu ürünle birlikte tüm gruplarının bir listesini görüntülemek için.

4. Gözden geçirmek istediğiniz grubu üzerinde tıklayın (Bu durumda, *O365 E5 - yalnızca Exchange*). Bu açılır **lisansları** sekmesi. E5 lisansı tıklandığında, tüm etkin hizmetleri listeleme bir dikey pencere açılır.
> [!NOTE]
> *Microsoft Stream* hizmet olduğundan otomatik olarak eklenir ve ek olarak bu gruba etkin *Exchange Online* hizmeti:

  ![Yeni hizmet için bir grup lisansı eklenen ekran görüntüsü](media/active-directory-licensing-group-advanced/manage-new-services.png)

5. Bu gruptaki yeni hizmet devre dışı bırakmak istiyorsanız, **açık/kapalı** geçiş hizmeti yanındaki ve'ı tıklatın **kaydetmek** Değişikliği onaylamak için düğmesini. Azure AD değişikliği uygulamak için gruptaki tüm kullanıcılar artık işleyecek; herhangi bir yeni gruba eklenen kullanıcılar sahip olmamak *Microsoft Stream* hizmetinin etkinleştirilmiş.

  > [!NOTE]
  > Kullanıcılar hala hizmetinin bazı diğer lisans atamasını (başka bir grup üyeleri veya doğrudan lisans atamasını oldukları) etkinleştirilmiş olabilir.

6. Gerekirse, aynı adımları diğer grupları için atanan bu ürünle gerçekleştirin.

## <a name="use-powershell-to-see-who-has-inherited-and-direct-licenses"></a>Kimin Devredilmiş sahiptir ve doğrudan lisansları görmek için PowerShell kullanın
Kullanıcıların doğrudan atanmış veya gruptan devralınan bir lisans olup olmadığını denetlemek için bir PowerShell betiğini kullanabilirsiniz.

1. Çalıştırma `connect-msolservice` kimlik doğrulaması ve kiracınıza bağlamak için cmdlet.

2. `Get-MsolAccountSku`kiracıdaki tüm sağlanan ürün lisansları bulmak için kullanılabilir.

  ![Get-döndürülüp cmdlet'inin ekran görüntüsü](media/active-directory-licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. Kullanım *AccountSkuId* ile ilgilenen lisans için değer [bu PowerShell Betiği](./active-directory-licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group). Bu lisans nasıl atanacağını hakkında bilgi ile bu lisansı olan kullanıcıların bir listesini oluşturur.

## <a name="use-audit-logs-to-monitor-group-based-licensing-activity"></a>Grup tabanlı lisans etkinliğini izlemek için denetim günlüklerini kullanın

Kullanabileceğiniz [Azure AD denetim günlüklerini](./active-directory-reporting-activity-audit-logs.md#audit-logs) tüm etkinlik görmek için ilgili grup tabanlı lisans için de dahil olmak üzere:
- kimin lisans grupları değişti mi
- sistem Grup lisans değişikliği işleme başlatıldığında ve onu bittiğinde
- bir kullanıcıya bir Grup lisans atamasını sonucunda hangi lisans değişiklikler yapıldı.

>[!NOTE]
> Denetim günlüklerini çoğu Kanatlar portal Azure Active Directory bölümünde bulunur. Bunları eriştiğiniz yere bağlı olarak, filtreleri yalnızca etkinlik dikey bağlamında ilgili göstermek için önceden uygulanan olabilir. Beklediğiniz sonuçları görmüyorsanız, inceleyin [filtreleme seçenekleri](./active-directory-reporting-activity-audit-logs.md#filtering-audit-logs) veya filtrelenmemiş denetim günlüklerini altında erişim [ **Azure Active Directory > etkinlik > Denetim günlüklerini** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit).

### <a name="find-out-who-modified-a-group-license"></a>Kimin bir Grup lisans değiştiren çıkışı Bul

1. Ayarlama **etkinlik** için filtre *kümesi Grup lisans* tıklatıp **Uygula**.
2. Sonuçlar lisanslarını ayarlayın veya gruplarında değiştiren tüm örneklerini içerir.
>[!TIP]
> Ayrıca Grup adını yazabilirsiniz *hedef* sonuçları kapsam için filtre.

3. Nelerin değiştiğini ayrıntılarını görmek için liste görünümünde bir öğeyi tıklatın. Altında *değiştirilmiş Özellikler* lisans atamasını hem eski hem de yeni değerleri listelenir.

Ayrıntılarla son Grup lisans değişiklikleri bir örneği burada verilmiştir:

![Ekran Grup lisans değişiklikleri](media/active-directory-licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Grup değişikliklerini başlatıldığında ve işleme tamamlandı öğrenin

Bir lisans grubu üzerinde değiştiğinde, Azure AD değişiklikleri tüm kullanıcılara uygulama başlar.

1. Grupları işleme başladığında görmek için ayarlayın **etkinlik** için filtre *göre grubundaki lisans kullanıcılara uygulama Başlat*. İşlem için aktör olduğuna dikkat edin *Microsoft Azure AD grup tabanlı lisans* -tüm Grup lisans değişiklikleri yürütmek için kullanılan bir sistem hesabı.
>[!TIP]
> Görmek için listedeki bir öğeye tıklayın *değiştirilmiş Özellikler* alan - işleme için çekilen lisans değişiklikleri gösterir. Bu, bir grup için birden fazla değişiklik ve hangisinin işlenmiş emin değilseniz kullanışlıdır.

2. Benzer şekilde, işleme grupları bitirdiğinde görmek için filtre değeri kullanın *göre grubundaki lisans kullanıcılara uygulama son*.
>[!TIP]
> Bu durumda, *değiştirilmiş Özellikler* alan sonuçlarının özetini içeren - işleme hatalarını sonuçlandı varsa hemen kontrol etmek kullanışlıdır. Örnek çıktı:
> ```
Modified Properties
...
Name : Result
Old Value : []
New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
> ```

3. Nasıl bir grubu, tüm kullanıcı değişiklikleri de dahil olmak üzere işlendiği için tam günlük görmek için aşağıdaki filtreleri ayarlayın:
  - **(Aktör) tarafından başlatılan**: "Microsoft Azure AD grup tabanlı lisans"
  - **Tarih aralığı** (isteğe bağlı): belirli bir grup bildiğinizde için özel aralık başlatıldı ve işleme tamamlandı

Bu örnek çıkış işleme, sonuçta ortaya çıkan tüm kullanıcı değişiklikleri ve işleme bitiş başlangıcını gösterir.

![Ekran Grup lisans değişiklikleri](media/active-directory-licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> Öğeleri tıklayarak ilgili *değişiklik kullanıcı lisansı* her kullanıcıya uygulanan lisans değişiklikler Ayrıntılar gösterilir.

## <a name="deleting-a-group-with-an-assigned-license"></a>Atanmış bir lisansa sahip bir grup silme

Etkin bir lisansı atanmış bir grubu silmek mümkün değildir. Yönetici kullanıcılardan - biz gruptan silinebilmesi için önce ilk olarak, kaldırılması için lisans gerektiren bu nedenle kaldırılmasını lisansları neden kullanıldıklarını olmayan bir grup silinemedi.

Azure portalında bir grubu silmek çalışırken bir hata bildirimi şöyle görebilirsiniz: ![ekran grup silme başarısız oldu](media/active-directory-licensing-group-advanced/groupdeletionfailed.png)

Git **lisansları** sekmesinde grubunda ve atanan lisansları olup olmadığını. Yanıt Evet ise, bu lisansları kaldırmasını ve grubu silmeyi tekrar deneyin.

PowerShell veya grafik API'si aracılığıyla grubunu silmek çalışırken benzer hatalar görebilirsiniz. Şirket içi eşitlenen bir grubu kullanıyorsanız, Azure AD'de bir grubu silmek başarısız olduysa Azure AD Connect de hataları bildirebilir. Böyle durumlarda gruba atanmış tüm lisanslar olup olmadığını denetlemek emin olun ve ilk kaldırın.

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar

Grup tabanlı lisanslama kullanıyorsanız, aşağıdaki sınırlamalar ve bilinen sorunlar listesi ile tanımak için iyi bir fikirdir.

- Grup tabanlı şu anda lisans diğer gruplar (iç içe geçmiş gruplar) içeren grupları desteklemez. İç içe geçmiş grup için bir lisans uygularsanız, yalnızca hemen birinci düzey kullanıcı grubunun üyeleri uygulanan lisansınız yok.

- Bu özellik yalnızca güvenlik grupları ile kullanılabilir. Office grubu şu anda desteklenmez ve lisans atama işleminde kullanmanız mümkün olmaz.

- [Office 365 Yönetici portalı](https://portal.office.com ) grup tabanlı lisans şu anda desteklemiyor. Bir kullanıcı bir lisans bir gruptan devralır. Bu lisans normal kullanıcı lisansı Office Yönetim Portalı'nda görüntülenir. Lisans değiştirmek veya lisans kaldırmaya çalışırsanız, portalı bir hata iletisi döndürür. Devralınan Grup lisansları doğrudan bir kullanıcı olarak değiştirilemez.

- Bir kullanıcı bir gruptan kaldırılır ve lisans kaybederse, lisans (örneğin, SharePoint Online) hizmet planlarından ayarlanmıştır bir **askıya** durumu. Bir son, devre dışı durumuna hizmet planları ayarlı değil. Bir yönetim grubu üyeliği Yönetimi'nde bir hata yaparsa bu önlem, kullanıcı verilerinin yanlışlıkla kaldırma önleyebilirsiniz.

- Lisansları atanmış veya büyük bir grup için (örneğin, 100.000 kullanıcı) değiştiren performansını etkileyebilir. Özellikle, Azure AD Otomasyon tarafından üretilen değişikliklerin hacmi, Azure AD arasında dizin eşitlemesi performansını olumsuz etkileyebilir ve şirket içi sistemler.

- Bazı yüksek yük durumlarda, lisans işleme gecikebilir ve değişiklikleri ekleme bir grup ve kaldırma gibi lisans ya da grup, kullanıcıları ekleme ve kaldırma işlenmesi uzun zaman alabilir. Görürseniz değişikliklerinizi, lütfen işlemek için birden fazla 24 saatten uzun sürer [bir destek bileti açmanız](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/supportRequest) bize araştırmak izin vermek için. Bunu erişmeden önce Biz bu özellik performans özellikleri geliştirecektir *genel kullanılabilirlik*.

- Lisans Yönetimi Otomasyonu otomatik olarak değişiklikleri ortamdaki tüm türleri için tepki vermez. Örneğin, lisansları dışında bir hata durumunda olması bazı kullanıcılar neden çalıştırdığınız. İçin kullanılabilir lisans sayısı boşaltın, bazı doğrudan atanan lisansları diğer kullanıcılardan kaldırabilirsiniz. Bununla birlikte, sistem otomatik olarak bu değişiklik tepki ve kullanıcılara, hata durumunda düzeltin.

  Geçici bir çözüm olarak sınırlamaları bu tür, gidebilirsiniz **grup** dikey Azure AD'de tıklatıp **yeniden işlemek**. Bu komut, o gruptaki tüm kullanıcılar işler ve hata durumları mümkünse giderir.

- Bir lisans yinelenen proxy adresi yapılandırması nedeniyle bir kullanıcı için Exchange Online atanamadı olduğunda grup tabanlı lisans hataları kaydetmez; Bu tür kullanıcıların Lisans atama sırasında atlanır. Tanımlamak ve bu sorunu çözmek hakkında daha fazla bilgi için bkz: [Bu bölümde](./active-directory-licensing-group-problem-resolution-azure-portal.md#license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online).

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisans aracılığıyla lisans yönetimi için diğer senaryolar hakkında daha fazla bilgi için bkz:

* [Grup tabanlı Azure Active Directory lisanslaması nedir?](active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'deki bir gruba lisans atama](active-directory-licensing-group-assignment-azure-portal.md)
* [Azure Active Directory'deki bir gruba lisans sorunlarını tanımlama ve](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory'de Grup tabanlı lisans için tek tek lisanslı kullanıcıları geçirme](active-directory-licensing-group-migration-azure-portal.md)
