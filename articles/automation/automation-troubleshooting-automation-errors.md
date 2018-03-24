---
title: Azure Automation ile ilgili genel sorunları giderme | Microsoft Docs
description: Bu makalede, sorun giderme ve ortak Azure Otomasyonu hataları giderin yardımcı olacak bilgiler sağlar.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
tags: top-support-issue
keywords: sorun giderme, Otomasyon hatası, sorunu
ms.openlocfilehash: 9764068dd7a1a499c61695f39bff726a8ea3aac9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshooting-common-issues-in-azure-automation"></a>Azure Automation sık karşılaşılan sorunları giderme 
Bu makalede Azure Otomasyonu'nda karşılaşabilirsiniz ve bunları gidermek için olası çözümleri önerir ortak hatalarında sorun giderme yardım sağlar.

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Azure Otomasyon çalışma kitabı ile çalışırken, kimlik doğrulama hataları
### <a name="scenario-sign-in-to-azure-account-failed"></a>Senaryo: Başarısız Azure hesabınızda oturum açın
**Hata:** Add-AzureAccount veya Login-AzureRmAccount cmdlet ile birlikte çalışırken "Unknown_user_type: Bilinmeyen kullanıcı türü" hatası alıyorsunuz.

**Hatanın nedeni:** kimlik bilgisi varlığı adı geçerli değil veya kullanıcı adı ve Otomasyonu kimlik bilgisi varlığı ayarlamak için kullanılan parola geçerli değilse bu hata oluşur.

**Sorun giderme ipuçları:** sorunun ne olduğunu belirlemek için aşağıdaki adımları uygulayın:  

1. Dahil olmak üzere tüm özel karakterleri içermediğinden emin olun **@** karakteri, Azure'a bağlanmak için kullandığınız Otomasyonu kimlik bilgisi varlığı adı.  
2. Azure Otomasyonu kimlik bilgisi yerel PowerShell ISE düzenleyicinizde depolanmış parola ve kullanıcı adı kullanıp kullanmadığını denetleyin. PowerShell ISE aşağıdaki cmdlet'leri çalıştırarak bunu yapabilirsiniz:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred
3. Yerel olarak, kimlik doğrulama başarısız olursa, bu Azure Active Directory bilgilerinizi düzgün ayarlamadıysanız anlamına gelir. Başvurmak [Azure Active Directory'yi kullanarak Azure için kimlik doğrulama](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) doğru şekilde ayarlanan Azure Active Directory hesap almak için blog postası.  

### <a name="scenario-unable-to-find-the-azure-subscription"></a>Senaryo: Azure aboneliği bulunamıyor
**Hata:** hatasını alıyorsunuz "adlı abonelik ``<subscription name>`` bulunamıyor" Select-AzureSubscription veya Select-AzureRmSubscription cmdlet ile birlikte çalışırken.

**Hatanın nedeni:** abonelik adı geçerli değil veya abonelik ayrıntıları alınmaya çalışılırken Azure Active Directory kullanıcı Abonelik Yöneticisi olarak yapılandırılmamışsa, bu hata oluşur.

**Sorun giderme ipuçları:** Azure'a düzgün bir şekilde doğrulandı ve seçmek için çalıştığınız abonelik erişimi belirlemek için aşağıdaki adımları uygulayın:  

1. Çalıştırdığınızdan emin olun **Add-AzureAccount** çalıştırmadan önce **Select-AzureSubscription** cmdlet'i.  
2. Hala bu hata iletisini görürseniz, kodunuzu ekleyerek değiştirmeye **Get-AzureSubscription** cmdlet'i aşağıdaki **Add-AzureAccount** cmdlet'i ve kod yürütebilir. Şimdi Get-AzureSubscription çıktısını abonelik ayrıntılarınızı içerip içermediğini doğrulayın.  

   * Tüm abonelik ayrıntıları çıktıda görmüyorsanız, bu abonelik henüz başlatılmadı anlamına gelir.  
   * Abonelik Ayrıntıları çıktıda görürseniz doğru abonelik adı veya kimliği ile kullandığınızdan emin olun **Select-AzureSubscription** cmdlet'i.   

### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Senaryo: çok faktörlü kimlik doğrulamasının etkin olduğundan Azure kimlik doğrulaması başarısız oldu
**Hata:** hatasını alıyorsunuz "Add-AzureAccount: AADSTS50079: güçlü kimlik doğrulaması kayıt (yukarı kanıt) gereklidir" Azure kullanıcı adı ve parola ile Azure için kimlik doğrulama zaman.

**Hatanın nedeni:** çok faktörlü kimlik doğrulaması Azure hesabınız varsa, Azure için kimlik doğrulaması için bir Azure Active Directory kullanıcı kullanamaz. Bunun yerine, Azure için kimlik doğrulaması için bir sertifika veya bir hizmet sorumlusu kullanmanız gerekir.

**Sorun giderme ipuçları:** Azure Klasik dağıtım modeli cmdlet'leriyle bir sertifika kullanmak üzere başvurmak [oluşturma ve Azure hizmetleri yönetmek için bir sertifika ekleniyor.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Bir hizmet sorumlusu ile Azure Resource Manager cmdlet'lerini kullanmak için başvurmak [hizmet sorumlusu Azure portalını kullanarak oluşturuluyor](../azure-resource-manager/resource-group-create-service-principal-portal.md) ve [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması.](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Runbook'larla çalışırken sık karşılaşılan hataları
### <a name="scenario-the-runbook-job-start-was-attempted-three-times-but-it-failed-to-start-each-time"></a>Senaryo: Runbook işi başlangıç üç kez denendi, ancak her zaman başlatılamadı
**Hata:** runbook'unuz "" iş üç kez denedi ancak işlem başarısız oldu."hatasıyla başarısız oluyor

**Hatanın nedeni:** bu hatayı aşağıdaki nedenlerden kaynaklanabilir:  

1. Bellek sınırı. Bir korumalı alan için ayrılan bellek miktarını üzerinde belgelenmiş sınırlamanın vardır [Otomasyon hizmet sınırları](../azure-subscription-service-limits.md#automation-limits) şekilde 400 MB'tan fazla bellek kullanıyorsa, bir işin başarısız. 

2. Modül uyumlu değil. Bu modül bağımlılıkları doğru değildir ve değilse, runbook'a genellikle "komutu bulunamadı" döndürürse oluşabilir veya "parametresi bağlanılamıyor" iletisi. 

**Sorun giderme ipuçları:** aşağıdaki çözümlerden birini sorunu düzeltin:

* Bellek sınırı içinde çalışmak için önerilen yöntem, birden çok runbook arasında iş yükünü bölme veya değil, bellekte gereksiz çıktı, runbook'lardan yazamaz kadar verileri işlemek, PowerShell iş akışı runbook'larınızda yazma kaç kontrol noktaları göz önünde bulundurun üzeresiniz.  

* Aşağıdaki adımları izleyerek Azure modüllerinizi güncelleştirme [Azure PowerShell modülleri Azure Automation güncelleştirmek nasıl](automation-update-azure-modules.md).  


### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Senaryo: Runbook seri durumdan çıkarılmış nesne nedeniyle başarısız.
**Hata:** runbook'unuz hatasıyla başarısız olur. "parametre bağlanamıyor ``<ParameterName>``. Dönüştürülemiyor ``<ParameterType>`` Deserialized türünde değer ``<ParameterType>`` yazmak için ``<ParameterType>``".

**Hatanın nedeni:** runbook'unuz bir PowerShell iş akışı ise, bu karmaşık nesne seri durumdan çıkarılmış biçimde iş akışı askıya alınırsa, runbook durumu devam ettirmek için depolar.

**Sorun giderme ipuçları:** aşağıdaki üç çözümlerden birini bu sorunu düzeltin:

1. Bir cmdlet karmaşık nesneleri başka bir cmdlet'ine varsa, bu cmdlet'leri bir Inlinescript alın.
2. Nesnenin tamamı geçirme yerine karmaşık nesnesinden adını veya gereksinim duyduğunuz değerini geçirin.
3. Bir PowerShell runbook'u bir PowerShell iş akışı runbook yerine kullanın.

### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Senaryo: Runbook işi başarısız oldu, ayrılmış kotasını aştığı için
**Hata:** runbook işi, "aylık toplam iş yürütme süresi kotasına, bu abonelik için de ulaşıldı" hatasıyla başarısız oluyor.

**Hatanın nedeni:** iş yürütme hesabınız için 500 dakikalık ücretsiz kotayı aşan bu hata oluşur. Bu kota bir işi sınama, gibi portaldan bir iş başlatmayı Web kancalarını kullanarak ve Azure portalını kullanarak veya yürütmek için bir iş zamanlaması bir iş yürütme, veri merkezinizdeki iş yürütme görevleri tüm türleri için geçerlidir. Otomasyon için fiyatlandırma hakkında daha fazla bilgi için bkz: [Otomasyon fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/).

**Sorun giderme ipuçları:** 500 dakikadan fazla işleme aylık kullanmak istiyorsanız, ücretsiz aboneliğinizi değiştirmek gereken temel katmana katmanı. Aşağıdaki adımları uygulayarak temel katmana yükseltebilirsiniz:  

1. Azure aboneliğinizde oturum açın  
2. Yükseltmek istediğiniz Otomasyon hesabını seçin  
3. Tıklayın **ayarları** > **fiyatlandırma**.
4. Tıklatın **etkinleştirmek** hesabınıza yükseltmek için sayfa altta **temel** katmanı.

### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Senaryo: bir runbook yürütülürken tanınmayan cmdlet'i
**Hata:** , runbook işi şu hata ile başarısız "``<cmdlet name>``: terimi ``<cmdlet name>`` cmdlet, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor."

**Hatanın nedeni:** PowerShell altyapısı runbook'unuzda kullandığınız cmdlet bulamadığında bu hataya neden olur. Bu cmdlet içeren modülü hesabından eksik olduğundan, runbook adı ile ad çakışması veya başka bir modülde mevcut cmdlet da ve Otomasyon adı çözümlenemiyor olabilir.

**Sorun giderme ipuçları:** aşağıdaki çözümlerden birini sorunu düzeltin:  

* Cmdlet adı doğru yazdığınızdan emin olun.  
* Cmdlet, Automation hesabınız var ve herhangi bir çakışma olduğundan emin olun. Cmdlet'in mevcut olup olmadığını doğrulamak için bir runbook düzenleme modu ve arama çalıştırın veya kitaplık bulmak istediğiniz cmdlet'in açın **Get-Command ``<CommandName>``** . Cmdlet hesabına kullanılabilir olan doğrulatma ve diğer cmdlet'leri veya runbook'ları ad çakışması yok sonra tuvale Ekle ve geçerli bir parametre runbook'unuzda kümesi kullandığınızdan emin olun.  
* Bir ad çakışması varsa ve iki farklı modüllerde cmdlet kullanılabilir, bu cmdlet'i için tam adı kullanarak çözebilirsiniz. Örneğin, kullanabileceğiniz **ModuleName\CmdletName**.  
* Karma çalışanı grubu içinde içi runbook çalıştırıldığında modül/cmdlet karma çalışanı barındıran makinede yüklü olduğundan emin olun.

### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Senaryo: Bir uzun süre çalışan runbook sürekli özel durumuyla başarısız: "iş sürekli olarak aynı denetim noktasından çıkarıldı çünkü çalışmaya devam edemiyor".
**Hatanın nedeni:** Bu davranış "Orta Share" üç saatten uzun yürütülürse, otomatik olarak bir runbook'u askıya Azure Otomasyonu süreçlerinde izlenmesini nedeniyle tasarım gereğidir. Ancak, döndürülen hata iletisi "İleri hangi" sağlamaz seçenekleri. Bir runbook birçok nedenden dolayı askıya alınabilir. Durum çoğunlukla hatalar nedeniyle askıya alır. Örneğin, yakalanmayan bir özel durum bir runbook, bir ağ hatası veya çökmeyle runbook'u çalıştıran Runbook çalışanı üzerindeki tüm askıya ve sürdürülebilir olduğunda, son denetim noktasından başlatmak runbook neden olur.

**Sorun giderme ipuçları:** belgelenen çözüm bu sorunu önlemek için bir iş akışında denetim noktaları kullanmaktır. Daha fazla bilgi edinmek için bkz [öğrenme PowerShell iş akışları](automation-powershell-workflow.md#checkpoints). Daha kapsamlı bir açıklama "Orta paylaşımı" ve denetim noktası bu blog makalesinde bulunabilir [kullanarak denetim noktaları runbook'lardaki](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Modülleri içeri aktarırken sık karşılaşılan hataları
### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Senaryo: Modülü içeri aktarmak başarısız veya cmdlet'leri içeri aktardıktan sonra yürütülemez.
**Hata:** bir modülü içeri aktarmak başarısız olursa veya başarıyla alır, ancak hiçbir cmdlet'leri ayıklanır.

**Hatanın nedeni:** bir modül başarıyla Azure Otomasyonu alabilir değil bazı genel nedenleri şunlardır:

* Yapısı, Otomasyon olması gereken yapısıyla uyuşmuyor.
* Modül, Automation hesabınızı dağıtılmadı başka bir modül bağımlıdır.
* Modülün bağımlılıklarını klasöründeki eksik.
* **Yeni AzureRmAutomationModule** cmdlet, modül karşıya yüklemek için kullanılıyor ve tam depolama için belirtilen yol değil veya modülü genel olarak erişilebilir bir URL kullanarak yüklenmedi.

**Sorun giderme ipuçları:** aşağıdaki çözümlerden birini sorunu düzeltin:

* Modül aşağıdaki biçime uyduğundan emin olun: ModuleName.Zip **->** ModuleName veya sürüm numarası **->** (ModuleName.psm1, ModuleName.psd1)
* .Psd1 dosyasını açın ve modül bağımlılıkları olup olmadığına bakın. Varsa, bu modülleri Automation hesabına yükleyin.
* Başvurulan tüm .dll Modülü klasörde mevcut olduğundan emin olun.

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>İstenen durum yapılandırması (DSC) ile çalışırken sık karşılaşılan hataları
### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Senaryo: Bir "Bulunamadı" hatası ile başarısız durumundaki düğümdür
**Hata:** sahip bir rapor düğümü sahip **başarısız** durum ve hata içeren "sunucu https:// eylemi alma denemesi``<url>``//accounts/ ``<account-id>`` /Nodes(AgentId=``<agent-id>``) / GetDscAction başarısız olduğundan geçerli bir yapılandırma ``<guid>`` bulunamıyor. "

**Hatanın nedeni:** düğüm yapılandırma adı (örneğin, ABC) atandığında, bu hata genellikle düğüm yapılandırma adı (örneğin, ABC. yerine oluşur Web sunucusu).

**Sorun giderme ipuçları:**

* "Düğüm yapılandırması adı" ve "yapılandırma adı değil" düğümle atıyorsanız emin olun.
* Azure portalını kullanarak bir düğüme veya bir PowerShell cmdlet ile bir düğüm yapılandırması atayabilirsiniz.

  * Düğüm yapılandırması Azure portal kullanarak bir düğüme atamak için açık **DSC düğümleri** sayfası, ardından bir düğüm seçin ve tıklayın **Ata düğüm yapılandırması** düğmesi.  
  * Düğüm yapılandırması PowerShell cmdlet'ini kullanarak bir düğüme atamak için kullanılması **kümesi AzureRmAutomationDscNode** cmdlet'i

### <a name="scenario-no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Senaryo: düğüm yapılandırması (MOF dosyaları) bir yapılandırma derlendiğinde oluşturulamadığı
**Hata:** DSC derleme işi şu hata ile askıya alır: "derleme başarıyla tamamlandı, ancak hiçbir düğüm configuration.mofs oluşturuldu".

**Hatanın nedeni:** zaman ifade aşağıdaki **düğümü** DSC yapılandırmada anahtar sözcüğü hesaplar için `$null`, düğüm yapılandırması üretilen sonra.

**Sorun giderme ipuçları:** aşağıdaki çözümlerden birini sorunu düzeltin:

* Olduğundan emin olun yanındaki ifade **düğümü** $null için anahtar sözcüğü yapılandırma tanımında değerlendirme değil.
* Yapılandırma derlerken ConfigurationData geçirme, gelen yapılandırma gerektirir beklenen değerler geçirdiğinizden emin olun [ConfigurationData](automation-dsc-compile.md#configurationdata).

### <a name="scenario-the-dsc-node-report-becomes-stuck-in-progress-state"></a>Senaryo: DSC düğümü rapor "Sürüyor" durumunda kalmış olur
**Hata:** DSC Aracısı çıkarır "ile özellik değerlerini bulunan örneği yok."

**Hatanın nedeni:** WMF sürümünüzü yükseltmişseniz ve WMI bozulmuş.

**Sorun giderme ipuçları:** sorunu izleyin yönergeleri düzeltmek için [bilinen sorunlar ve sınırlamalar DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) makalesi.

### <a name="scenario-unable-to-use-a-credential-in-a-dsc-configuration"></a>Senaryo: Bir kimlik bilgisi bir DSC yapılandırması kullanılamıyor
**Hata:** DSC derleme işi şu hata ile askıya alındı: "özellik türü ' kimlik bilgisi' işlenirken System.InvalidOperationException hata ``<some resource name>``: dönüştürme ve şifreli bir parola düz metin yalnızca izin verilir olarak depolama PSDscAllowPlainTextPassword ayarlandığında true ".

**Hatanın nedeni:** bir kimlik bilgisi bir yapılandırmada kullanmış ancak uygun sağlamadı **ConfigurationData** ayarlamak için **PSDscAllowPlainTextPassword** her düğüm için true yapılandırma.

**Sorun giderme ipuçları:**

* Uygun aktardığınızdan emin olun **ConfigurationData** ayarlamak için **PSDscAllowPlainTextPassword** yapılandırmasında belirtilen her düğüm yapılandırması için true. Daha fazla bilgi için bkz: [Azure Otomasyonu DSC varlıkları](automation-dsc-compile.md#assets).

## <a name="common-errors-when-onboarding-solutions"></a>Sık karşılaşılan zaman ekleme çözümleri

Karşılaşabileceğiniz ekleme çözümleri zaman hataları. Üzerinden çalışabilir sık karşılaşılan bir listesi verilmiştir.

### <a name="computergroupqueryformaterror"></a>ComputerGroupQueryFormatError

**Hatanın nedeni:**

Bu hata kodu çözümü hedeflemek için kullanılan kayıtlı arama bilgisayar grubu sorgusu doğru biçimlendirilmedi anlamına gelir. Sorgu değiştirilmiş veya sistem tarafından değiştirilmiş olabilir.

**Sorun giderme ipuçları:**

Bu çözüm ve reonboard sorguyu yeniden oluşturur çözüm için sorgu silebilirsiniz. Sorgu, çalışma alanınızı içinde altında bulunabilir **kayıtlı aramalar**. Sorgu adı **MicrosoftDefaultComputerGroup**, ve sorgu kategorisini bu sorguyla ilişkilendirilen çözüm adıdır. Birden çok çözümleri etkinleştirilirse, **MicrosoftDefaultComputerGroup** altında birden çok kez gösterir **kayıtlı aramaları**.

### <a name="policyviolation"></a>PolicyViolation

**Hatanın nedeni:**

Bu hata kodu, dağıtım bir veya daha fazla ilke ihlali nedeniyle başarısız oldu anlamına gelir.

**Sorun giderme ipuçları:**

Başarıyla çözümü dağıtmak için belirtilen ilke değiştirilmesine dikkate almanız gerekir. Farklı türlerde tanımlanabilir ilkeleri olarak yapılması gereken belirli değişiklikler ihlal ilkesindeki bağlıdır. Bir ilke, belirli türde bir kaynak grubu içindeki kaynaklara içeriğini değiştirme izni bir kaynak grubu üzerinde tanımlanmışsa, örneğin, örneğin, aşağıdakilerden birini yapmanız olabilir:

*   İlkeyi tamamen kaldırın.
* Farklı bir kaynak grubu eklemek için deneyin.
* İlke ile örneğin gözden geçirin:
   * İlkeyi yeniden hedefleme bir belirli bir kaynak (belirli bir Otomasyon hesabı gibi).
   * Belirlenen düzeltilmesi kaynakları bu ilkeyi reddedecek şekilde yapılandırıldı.

Azure portalının sağ üst köşedeki Bildirimlerde denetleyin veya seçin ve automation hesabı içeren kaynak grubuna gidin **dağıtımları** altında **ayarları** başarısız görüntülemek için dağıtımı. Azure ilke ziyaret hakkında daha fazla bilgi edinmek için: [Azure ilke genel bakış](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

Önceki sorun giderme adımları izlediğinizde ve yanıt bulamadıysanız, aşağıdaki seçenekleri şu ek destek gözden geçirebilirsiniz:

* Azure uzmanlarından yardım alın. Sorununuz için gönderme [MSDN Azure veya yığın taşması forumları](https://azure.microsoft.com/support/forums/).
* Bir Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) tıklatıp **alma desteği** altında **teknik ve faturalama desteği**.
* İleti bir betik isteği [Komut Merkezi](https://azure.microsoft.com/documentation/scripts/) bir Azure Otomasyonu runbook çözümü veya bir tümleştirme modülü arıyorsanız.
* Azure Automation ile ilgili geribildirim ya da özellik isteğiniz ileti [kullanıcı sesi](https://feedback.azure.com/forums/34192--general-feedback).
