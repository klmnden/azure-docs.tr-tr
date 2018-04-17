---
title: Güvenlik Merkezi platformu geçiş ile ilgili SSS | Microsoft Docs
description: Bu SSS, Azure Güvenlik Merkezi platformu geçişi hakkında sorular yanıtlanmaktadır.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2017
ms.author: terrylan
ms.openlocfilehash: 197b1a844291f2bef2dd35001d1e6b8807ac9805
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="security-center-platform-migration-faq"></a>Güvenlik Merkezi platformu geçiş ile ilgili SSS
Erken Haziran 2017 toplamak ve verilerini depolamak için Microsoft Monitoring Agent'ı kullanarak Azure Güvenlik Merkezi başlamıştır. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi platformu geçiş](security-center-platform-migration.md). Bu SSS platform geçiş ile ilgili sorular yanıtlanmaktadır.

## <a name="data-collection-agents-and-workspaces"></a>Veri toplama, aracıları ve çalışma alanları

### <a name="how-is-data-collected"></a>Verileri nasıl toplanır?
Güvenlik Merkezi Microsoft Monitoring Agent, VM'lerin güvenlik verileri toplamak için kullanır. Güvenlik verileri hakkında bilgiler içerir:

- güvenlik açıklarını tanımlamak için kullanılan güvenlik yapılandırmalarını-
- Güvenlik olayları - tehditleri algılamak için kullanılan

Aracı tarafından toplanan verileri VM'ye bağlı olan bir günlük analizi çalışma veya Güvenlik Merkezi tarafından oluşturulan yeni bir çalışma alanı depolanır. Güvenlik Merkezi, yeni bir çalışma alanı oluşturduğunda, coğrafi konuma VM dikkate alınır.

> [!NOTE]
> Microsoft Monitoring Agent, günlük analizi hizmeti ve System Center Operations Manager (SCOM) tarafından kullanılan aynı aracısıdır.
>
>

Otomatik sağlama (daha önce günlük toplama olarak adlandırılır) etkin olduğunda veya aboneliklerinizi geçirildiğinde, Güvenlik Merkezi, VM'lerin her birinde Azure bir uzantısı olarak Microsoft İzleme Aracısı zaten yüklü olup olmadığını denetler. Microsoft izleme aracısı yüklü değilse, varsayılan olarak Güvenlik Merkezi olur:

- Microsoft Monitoring Agent uzantısı'nı VM'e yükleyin.

   - Güvenlik Merkezi tarafından önceden oluşturulmuş bir çalışma alanı aynı coğrafi konuma VM varsa, aracı bu çalışma alanına bağlı.
   - Bir çalışma alanı mevcut değilse, Güvenlik Merkezi, coğrafi konum içinde yeni bir kaynak grubu ve varsayılan çalışma alanı oluşturur ve aracı, çalışma alanına bağlanır. Çalışma alanı ve kaynak grubu için adlandırma kuralı aşağıdaki gibidir:

       Çalışma alanı: DefaultWorkspace-[abonelik kimliği]-[coğrafi]

       Kaynak grubu: DefaultResouceGroup-[coğrafi]

- Etkin bir Güvenlik Merkezi çözümü VM başına çalışma alanındaki fiyatlandırma katmanı Güvenlik Merkezi'nde ilişkili. Fiyatlandırma hakkında daha fazla bilgi için bkz: [Güvenlik Merkezi fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/).
- Yalnızca geçirilen abonelikleri için Güvenlik Merkezi önceki Azure Monitoring Agent de kaldırılır.

> [!NOTE]
> Microsoft Monitoring Agent otomatik olarak yüklenmesini geçersiz kılabilir ve kendi çalışma alanını kullanın.  Bkz: [otomatik aracı yükleme ve çalışma alanı oluşturma işlemini durdurmak nasıl](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation) ve [varolan çalışma alanınızı kullanmak nasıl](#how-can-i-use-my-existing-log-analytics-workspace).
>
>

Çalışma alanı konumunu VM konumuna bağlıdır. Daha fazla bilgi için bkz: [veri güvenliği](security-center-data-security.md). Bir abonelik birden çok geolocations VM'ler içeriyorsa, Güvenlik Merkezi birden çok çalışma alanı oluşturur. Birden çok çalışma veri gizlilik kuralları korumak için oluşturulur.

> [!NOTE]
> Platform geçişten önce Güvenlik Merkezi güvenlik verileri Azure Monitoring Agent kullanarak Vm'leriniz toplanan ve veri depolama hesabında depolanır. Güvenlik Merkezi, platform geçişten sonra aynı veri depolamak ve toplamak için Microsoft izleme aracısı ve çalışma alanı kullanır. Depolama hesabı geçişten sonra kaldırılabilir.  Güvenlik Merkezi, önceden yüklenmiş Azure izleme aracılarını platform geçişten sonra da kaldırır.
>
>

### <a name="am-i-billed-for-log-analytics-on-the-workspaces-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan çalışma alanları üzerinde günlük analizi için fatura 'M?
Hayır. Güvenlik Merkezi tarafından oluşturulan çalışma alanları faturalama, düğüm başına günlük analizi için yapılandırılmış olsa günlük analizi ücrete tabi değildir. Güvenlik Merkezi faturalama her zaman Güvenlik Merkezi güvenlik ilkesi ve bir çalışma alanı'na yüklü çözümleri dayanır:

- **Ücretsiz katmanı** – Güvenlik Merkezi varsayılan çalışma alanı 'SecurityCenterFree' çözümü sağlar. Ücretsiz katmanı için faturalandırılır değil.
- **Standart katmanı** – Güvenlik Merkezi varsayılan çalışma alanı 'Security' çözümü sağlar.

Fiyatlandırma hakkında daha fazla bilgi için bkz: [Güvenlik Merkezi fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/). Fiyatlandırma sayfasını güvenlik veri depolama ve eşit olarak bölünmüş fatura Haziran 2017 başlangıç değişiklikler giderir.

> [!NOTE]
> Fiyatlandırma katmanı Güvenlik Merkezi tarafından oluşturulan çalışma alanlarının günlük analizi Güvenlik Merkezi faturalama etkilemez.
>
>

### <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>Hangi Microsoft Monitoring Agent yüklemesini otomatik sağlamak için bir VM niteleyen?
Windows veya Linux Iaas VM'ler varsa nitelemek:

- Microsoft Monitoring Agent uzantısı VM üzerinde şu anda yüklü değil.
- VM çalışır durumda olduğunu.
- Windows veya Linux VM Aracısı yüklenir.
- VM, web uygulaması güvenlik duvarı ya da yeni nesil güvenlik duvarı gibi bir gereç olarak kullanılmaz.

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanları silebilmek için?
**Varsayılan çalışma alanı siliniyor önerilmez.** Güvenlik Merkezi, VM'ler güvenlik verilerini depolamak için varsayılan çalışma alanı kullanır.  Bir çalışma alanı silerseniz, bu verileri ve bazı güvenlik önerileri toplamak Güvenlik Merkezi alamıyor ve Uyarıları kullanılamaz.

Kurtarmak için Microsoft Monitoring Agent silinen çalışma alanına bağlı VM'ler kaldırın. Güvenlik Merkezi, aracı yeniden yükler ve yeni varsayılan çalışma alanı oluşturur.

### <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Var olan günlük analizi çalışma Alanım nasıl kullanabilir miyim?

Güvenlik Merkezi tarafından toplanan verileri depolamak için mevcut bir günlük analizi çalışma seçebilirsiniz. Var olan günlük analizi çalışma alanınızı kullanmak için:

- Çalışma alanı, seçili Azure aboneliğiniz ile ilişkilendirilmiş olması gerekir.
- En azından, çalışma alanına erişmek için Okuma izinleriniz olmalıdır.

Varolan bir günlük analizi çalışma alanını seçmek için:

1. Altında **güvenlik ilkesi – veri toplama**seçin **başka bir çalışma alanını kullanın**.

   ![Başka bir çalışma alanı kullan][5]

2. Aşağı açılır menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

   > [!NOTE]
   > Açılır menü, erişimi ve Azure aboneliğinizde olan çalışma alanları gösterilir.
   >
   >

3. **Kaydet**’i seçin.
4. Seçtikten sonra **kaydetmek**, izlenen RECONFIGURE VM'ler isteyip istemediğinizi istenir.

   - Seçin **Hayır** yeni çalışma alanı ayarları istiyorsanız **yalnızca yeni Vm'lere uygulamak**. Yeni çalışma alanı ayarları yalnızca yeni aracı yüklemeleri için geçerlidir; Microsoft izleme aracısı yüklü olmayan yeni bulunan VM'ler.
   - Seçin **Evet** yeni çalışma alanı ayarları istiyorsanız **tüm sanal makinelerin uygulamak**. Ayrıca, çalışma alanı oluşturulduğunda Güvenlik Merkezi'ne bağlı her VM için yeni hedef çalışma alanına bağlanır.

   > [!NOTE]
   > Evet'i seçerseniz, yeni hedef çalışma alanına tüm sanal makineleri yeniden bağlanması kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanları silmemelisiniz. Bir çalışma alanı çok erken silinirse, bu işlem başarısız olur.
   >
   >

   - Seçin **iptal** işlemi iptal etmek için.

      ![İzlenen VM'ler yeniden yapılandırın][6]

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Ne Microsoft İzleme Aracısı VM üzerinde bir uzantısı olarak zaten yüklendi mi?
Güvenlik Merkezi, kullanıcı çalışma alanları var olan bağlantılara kılmaz. Güvenlik Merkezi güvenlik verilerini VM zaten bağlı çalışma alanında depolar. Güvenlik Merkezi Güvenlik Merkezi kullanımını desteklemek için VM Azure kaynak Kimliğini içerecek şekilde genişletme sürümü güncelleştirir.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Ne ı makinedeki ancak uzantı olarak değil yüklü Microsoft İzleme Aracısı oldu?
Microsoft Monitoring Agent doğrudan (olarak değil bir Azure uzantısı) VM üzerinde yüklüyse, Güvenlik Merkezi Microsoft Monitoring Agent yüklemez ve güvenlik izleme sınırlıdır.

Daha fazla bilgi için sonraki bölüme bakın [SCOM veya OMS Aracısı my VM üzerinde zaten yüklü doğrudan ne olur?](security-center-platform-migration-faq.md#what-happens-if-a-scom-or-oms-direct-agent-is-already-installed-on-my-vm)

### <a name="what-happens-if-a-scom-or-oms-direct-agent-is-already-installed-on-my-vm"></a>SCOM veya OMS Aracısı doğrudan ne olur my VM üzerinde zaten yüklü?
Güvenlik Merkezi, önceden bir aracının yüklü olduğunu belirleyemiyor.  Güvenlik Merkezi, Microsoft Monitoring Agent uzantısı yüklemeye çalışır ve nedeniyle var olan yüklü aracıya başarısız olur.  Bu hata, çalışma alanına agent'ın bağlantı ayarlarını geçersiz kılma engeller ve birden çok giriş oluşturulmasını engeller.

> [!NOTE]
> Aracı sürümü, en son OMS Aracısı sürümüne güncelleştirilir.  Bu SCOM kullanıcılar için de geçerlidir.
>
>

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Bu uzantılar kaldırmanın etkisi nedir?
Microsoft Monitoring uzantısı kaldırırsanız, Güvenlik Merkezi güvenlik verileri VM ve bazı güvenlik önerileri toplamanın mümkün değildir ve Uyarıları kullanılamaz. VM uzantısı eksik ve uzantıyı yükler, 24 saat içinde Güvenlik Merkezi belirler.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Otomatik aracı yükleme ve çalışma alanı oluşturma nasıl Durdur?
Otomatik aboneliklerinizi güvenlik ilkesinde sağlamayı devre dışı bırakabilir, ancak bu önerilmez. Otomatik sağlama sınırları Güvenlik Merkezi önerileri ve uyarılar kapatma. Otomatik sağlama standart fiyatlandırma katmanı Aboneliklerde için gereklidir. Otomatik sağlamayı devre dışı bırakmak için:

1. Aboneliğinizi standart katmanı için yapılandırdıysanız, bu abonelik için Güvenlik İlkesi'ni açın ve seçin **serbest** katmanı.

   ![Fiyatlandırma katmanı][1]

2. Ardından, kapatma seçerek otomatik sağlamayı devre dışı **kapalı** üzerinde **güvenlik ilkesi – veri toplama** dikey.
   ![Veri toplama][2]

### <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Otomatik aracı yükleme ve çalışma alanı oluşturma dışında opt?

> [!NOTE]
> Bölümleri gözden geçirmeyi unutmayın [kullanmama etkileri nelerdir?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) ve [kullanmama, önerilen adımlar](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) dışı otomatik sağlamayı tercih seçerseniz.
>
>

Aşağıdaki sizin için geçerliyse, otomatik sağlamayı dışında opt isteyebilirsiniz:

- Güvenlik Merkezi tarafından otomatik aracı yükleme, tüm abonelik için geçerlidir.  Sanal makineleri bir kısmı için otomatik yükleme uygulanamıyor. Microsoft İzleme Aracısı ile yüklenemez kritik VM'ler varsa, otomatik sağlamayı dışında tercih.
- Microsoft Monitoring Agent uzantısını Yükleme aracının sürümünü güncelleştirir. Bu, doğrudan bir aracı ve SCOM aracısı için geçerlidir. SCOM aracı yüklü olan sürümü 2012 ve yükseltildiğinde, SCOM server sürüm 2012 olduğunda yönetilebilirlik özellikleri kaybolabilir. Yüklü SCOM Aracısı sürümü 2012 ise, otomatik sağlamayı dışında kullanmama göz önünde bulundurmalısınız.
- Bir özel çalışma alanı (merkezi bir çalışma alanına) aboneliğine dış varsa dışı otomatik sağlamayı tercih. El ile Microsoft Monitoring Agent uzantısını yükleyin ve Güvenlik Merkezi çalışma alanınızı, bağlantı geçersiz kılma bağlanın.
- Abonelik başına birden çok çalışma oluşturulmasını önlemek istiyorsanız ve abonelik içindeki özel kendi çalışma alanı varsa, iki seçeneğiniz vardır:

   1. Dışı otomatik sağlamayı tercih edebilirsiniz. Geçişten sonra varsayılan çalışma alanı açıklandığı gibi ayarlar [nasıl kullanabilir miyim mevcut günlük analizi çalışma Alanım?](#how-can-i-use-my-existing-log-analytics-workspace)
   2. Veya, geçiş, sanal makinelerin, yüklenecek Microsoft İzleme Aracısı tamamlamak izin verebilir ve VM'ler için oluşturulan çalışma alanına bağlı. Ardından, önceden yüklenmiş aracıları yeniden için seçim ile varsayılan çalışma alanı ayar ayarlayarak kendi özel çalışma alanını seçin. Daha fazla bilgi için bkz: [nasıl kullanabilir miyim mevcut günlük analizi çalışma Alanım?](#how-can-i-use-my-existing-log-analytics-workspace)

### <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Otomatik sağlama dışında kullanmama etkileri nelerdir?
Geçiş tamamlandıktan sonra Güvenlik Merkezi güvenlik verileri VM ve bazı güvenlik önerileri toplamanın mümkün değildir ve Uyarıları kullanılamaz. Çıkarsanız, Microsoft Monitoring Agent el ile yüklemeniz gerekir. Bkz: [kullanmama, önerilen adımlar](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).

### <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Otomatik sağlama dışında kullanmama zaman önerilen adımları nelerdir?
Güvenlik Merkezi, VM'lerin güvenlik verileri toplamak ve önerileri ve uyarılar sağlayabilmesi için Microsoft Monitoring Agent el ile yüklemeniz gerekir. Bkz: [Azure günlük analizi hizmeti bağlanmak Windows bilgisayarlara](../log-analytics/log-analytics-windows-agent.md) yükleme hakkında yönergeler için.

Aracıyı tüm mevcut özel çalışma alanına bağlayın veya Güvenlik Merkezi çalışma alanı oluşturulur. Özel bir çalışma alanı etkin 'Security' veya 'SecurityCenterFree' çözümleri yoksa, bir çözümü uygulamak gerekir. Uygulamak için özel çalışma veya aboneliği seçin ve bir fiyatlandırma katmanı üzerinden uygulama **güvenlik ilkesi – fiyatlandırma katmanı** dikey.

   ![Fiyatlandırma katmanı][1]

Güvenlik Merkezi seçilen fiyatlandırma katmanını temel alan çalışma doğru çözümü sağlar.

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Güvenlik Merkezi tarafından yüklenen OMS uzantıları nasıl kaldırılsın mı?
Microsoft Monitoring Agent el ile kaldırabilirsiniz. Güvenlik Merkezi önerileri ve uyarılar sınırlar bu önerilmez.

> [!NOTE]
> Veri toplama etkinleştirilirse, Güvenlik Merkezi kaldırdıktan sonra aracıyı yeniden yükler.  El ile aracı kaldırmadan önce veri toplamayı devre dışı bırakmanız gerekir. Bkz: [nasıl ı Durdur otomatik aracı yükleme ve çalışma alanı oluşturma?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?) veri toplamayı devre dışı bırakma hakkında yönergeler için.
>
>

Aracıyı el ile kaldırmak için:

1.  Portalı'nda açmak **günlük analizi**.
2.  Günlük analizi dikey penceresinde, bir çalışma alanı seçin:
3.  İzleme ve seçmek için istemediğiniz her bir VM seçin **Bağlantıyı Kes**.

   ![Aracıyı kaldırın][3]

> [!NOTE]
> Bir Linux VM uzantısı olmayan OMS Aracısı zaten varsa, uzantısını kaldırma aracı da kaldırır ve yeniden yüklemek müşteri sahiptir.
>
>

## <a name="existing-log-analytics-customers"></a>Var olan günlük analizi müşterileri

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Güvenlik Merkezi, VM'ler ve çalışma alanları arasında var olan tüm bağlantıları devre dışı bırakmaz?
Bir VM Azure uzantı olarak yüklü Microsoft İzleme Aracısı zaten varsa, Güvenlik Merkezi varolan çalışma bağlantı kılmaz. Bunun yerine, Güvenlik Merkezi, varolan çalışma kullanır.

Güvenlik Merkezi çözüm çalışma alanı yüklü henüz yoksa ve çözüm yalnızca ilgili VM'ler uygulanır. Bir çözümü eklediğinizde, varsayılan olarak, günlük analizi çalışma alanına bağlı tüm Windows ve Linux aracıları için otomatik olarak dağıtılır. [Çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) kapsam çözümlerinizi uygulamanıza imkan sağlar.

Microsoft Monitoring Agent doğrudan (olarak değil bir Azure uzantısı) VM üzerinde yüklüyse, Güvenlik Merkezi Microsoft Monitoring Agent yüklemez ve güvenlik izleme sınırlıdır.

### <a name="what-should-i-do-if-i-suspect-that-the-data-platform-migration-broke-the-connection-between-one-of-my-vms-and-my-workspace"></a>Veri Platformu geçiş Vm'lerimin ve çalışma Alanım biri arasındaki bağlantıyı ihlal ne yapmam gerekir?
Bu durum oluşmamalıdır. Bu, ardından görülüyorsa [Azure destek isteği oluşturmak](../azure-supportability/how-to-create-azure-support-request.md) ve aşağıdaki ayrıntıları içerir:

- Etkilenen VM Azure kaynak kimliği
- Bağlantı kesildi önce uzantısına göre yapılandırılmış çalışma Azure kaynak kimliği
- Aracı ve daha önce yüklenen sürüm

### <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Güvenlik Merkezi çözümleri my mevcut günlük analizi çalışma alanlarına yükler mi? Fatura etkileri nelerdir?
Güvenlik Merkezi bir VM zaten oluşturduğunuz bir çalışma alanına bağlı belirlediğinde, Güvenlik Merkezi bu çalışma alanındaki fiyatlandırma katmanınızı göre çözümleri sağlar. Çözümleri aracılığıyla yalnızca ilgili Azure VM'ler için uygulanan [çözüm hedefleme](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting), faturalama aynı kalır.

- **Ücretsiz katmanı** – Güvenlik Merkezi çalışma alanı 'SecurityCenterFree' çözüm yükler. Ücretsiz katmanı için faturalandırılır değil.
- **Standart katmanı** – Güvenlik Merkezi çalışma alanı 'Security' çözüm yükler.

   ![Varsayılan çalışma alanı çözümlerini][4]

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Çalışma Alanım ortamda zaten var, bunları güvenlik verileri toplamak için kullanabilir miyim?
Güvenlik Merkezi, bir VM Azure uzantı olarak yüklü Microsoft İzleme Aracısı zaten varsa, mevcut bağlı çalışma kullanır. Güvenlik Merkezi çözüm çalışma alanı yüklü henüz yoksa ve çözüm yalnızca ilgili VM'ler uygulanan [çözüm hedefleme](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting).

Güvenlik Merkezi sanal makine Microsoft İzleme Aracısı yüklendiğinde, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanları kullanır. En kısa sürede müşteriler hangi çalışma alanları kullanılan yapılandırmanız mümkün olacaktır.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Zaten bir güvenlik çözümü my çalışma alanlarına var. Fatura etkileri nelerdir?
Güvenlik ve denetim çözüm Azure VM'ler için Güvenlik Merkezi standart katman özellikleri etkinleştirmek için kullanılır. Güvenlik ve denetim çözümü bir çalışma alanı'na zaten yüklüyse Güvenlik Merkezi, varolan bir çözümü kullanır. Faturalama değişiklik yoktur.

## <a name="next-steps"></a>Sonraki adımlar
Güvenlik Merkezi platformu geçiş hakkında daha fazla bilgi için bkz:

- [Azure Güvenlik Merkezi platformu geçiş](security-center-platform-migration.md)
- [Azure Güvenlik Merkezi sorun giderme kılavuzu](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png
[6]: ./media/security-center-platform-migration-faq/reconfigure-monitored-vm.png
