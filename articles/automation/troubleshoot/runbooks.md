---
title: Azure Otomasyonu runbook'ları hatalarıyla ilgili sorunları giderme
description: Azure Otomasyon çalışma kitabı ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: bb340b8439927f191bc4a22f385d85d4e21b1cdb
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064070"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Runbook'larla hatalarında sorun giderme

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Azure Otomasyon çalışma kitabı ile çalışırken, kimlik doğrulama hataları

### <a name="sign-in-failed"></a>Senaryo: Başarısız Azure hesabınızda oturum açın

#### <a name="issue"></a>Sorun

İle çalışırken aşağıdaki hata iletisini `Add-AzureAccount` veya `Connect-AzureRmAccount` cmdlet'leri.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Nedeni

Kimlik bilgisi varlığı adı geçerli değil veya kullanıcı adı ve Otomasyonu kimlik bilgisi varlığı ayarlamak için kullanılan parola geçerli değilse bu hata oluşur.

#### <a name="resolution"></a>Çözüm

Sorunun ne olduğunu belirlemek için aşağıdaki adımları uygulayın:  

1. Dahil olmak üzere tüm özel karakterleri içermediğinden emin olun **@** karakteri, Azure'a bağlanmak için kullandığınız Otomasyonu kimlik bilgisi varlığı adı.  
2. Azure Otomasyonu kimlik bilgisi yerel PowerShell ISE düzenleyicinizde depolanmış parola ve kullanıcı adı kullanıp kullanmadığını denetleyin. PowerShell ISE aşağıdaki cmdlet'leri çalıştırarak bunu yapabilirsiniz:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Yerel olarak, kimlik doğrulama başarısız olursa, bu Azure Active Directory bilgilerinizi düzgün ayarlamadıysanız anlamına gelir. Başvurmak [Azure Active Directory'yi kullanarak Azure için kimlik doğrulama](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) doğru şekilde ayarlanan Azure Active Directory hesap almak için blog postası.  

### <a name="unable-to-find-subscription"></a>Senaryo: Azure aboneliği bulunamıyor

#### <a name="issue"></a>Sorun

İle çalışırken aşağıdaki hata iletisini `Select-AzureSubscription` veya `Select-AzureRmSubscription` cmdlet'leri.:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Hata

Abonelik adı geçerli değil veya abonelik ayrıntıları alınmaya çalışılırken Azure Active Directory kullanıcı Abonelik Yöneticisi olarak yapılandırılmamışsa, bu hata oluşur.

#### <a name="resolution"></a>Çözüm

Azure'a düzgün bir şekilde doğrulandı ve seçmek için çalıştığınız abonelik erişimi belirlemek için aşağıdaki adımları uygulayın:  

1. Çalıştırdığınızdan emin olun **Add-AzureAccount** çalıştırmadan önce **Select-AzureSubscription** cmdlet'i.  
2. Hala bu hata iletisini görürseniz, kodunuzu ekleyerek değiştirmeye **Get-AzureSubscription** cmdlet'i aşağıdaki **Add-AzureAccount** cmdlet'i ve kod yürütebilir. Şimdi Get-AzureSubscription çıktısını abonelik ayrıntılarınızı içerip içermediğini doğrulayın.  

   * Tüm abonelik ayrıntıları çıktıda görmüyorsanız, bu abonelik henüz başlatılmadı anlamına gelir.  
   * Abonelik Ayrıntıları çıktıda görürseniz doğru abonelik adı veya kimliği ile kullandığınızdan emin olun **Select-AzureSubscription** cmdlet'i.

### <a name="auth-failed-mfa"></a>Senaryo: çok faktörlü kimlik doğrulamasının etkin olduğundan Azure kimlik doğrulaması başarısız oldu

#### <a name="issue"></a>Sorun

Aldığınız Azure kullanıcı adı ve parola ile Azure için kimlik doğrulaması yapılırken aşağıdaki hata:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Nedeni

Çok faktörlü kimlik doğrulaması Azure hesabınız varsa, Azure için kimlik doğrulaması için bir Azure Active Directory kullanıcı kullanamaz. Bunun yerine, Azure için kimlik doğrulaması için bir sertifika veya bir hizmet sorumlusu kullanmanız gerekir.

#### <a name="resolution"></a>Çözüm

Azure Klasik dağıtım modeli cmdlet'leriyle bir sertifika kullanmak üzere başvurmak [oluşturma ve Azure hizmetleri yönetmek için bir sertifika ekleniyor.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Bir hizmet sorumlusu ile Azure Resource Manager cmdlet'lerini kullanmak için başvurmak [hizmet sorumlusu Azure portalını kullanarak oluşturuluyor](../../azure-resource-manager/resource-group-create-service-principal-portal.md) ve [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması.](../../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Runbook'larla çalışırken sık karşılaşılan hataları

### <a name="job-attempted-3-times"></a>Senaryo: Runbook işi başlangıç üç kez denendi, ancak her zaman başlatılamadı

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden kaynaklanabilir:

1. Bellek sınırı. Bir korumalı alan için ayrılan bellek miktarını üzerinde belgelenmiş sınırlamanın vardır [Otomasyon hizmet sınırları](../../azure-subscription-service-limits.md#automation-limits) şekilde 400 MB'tan fazla bellek kullanıyorsa, bir işin başarısız.

2. Modül uyumlu değil. Bu modül bağımlılıkları doğru değildir ve değilse, runbook'a genellikle "komutu bulunamadı" döndürürse oluşabilir veya "parametresi bağlanılamıyor" iletisi.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Bellek sınırı içinde çalışmak için önerilen yöntem bellekte gereksiz çıktı, runbook'lardan yazamaz kadar verileri işleme veya PowerShell iş akışınıza yazma kaç kontrol noktaları göz önünde bulundurun değil birden çok runbook arasında iş yükünü bölme üzeresiniz runbook'ları.  

* Aşağıdaki adımları izleyerek Azure modüllerinizi güncelleştirme [Azure PowerShell modülleri Azure Automation güncelleştirmek nasıl](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Senaryo: Runbook seri durumdan çıkarılmış nesne nedeniyle başarısız.

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Nedeni

Runbook'unuz bir PowerShell iş akışı ise, iş akışını askıya alınırsa, runbook durumu devam ettirmek için seri durumdan çıkarılmış biçimde karmaşık nesne depolar.

#### <a name="resolution"></a>Çözüm

Aşağıdaki üç çözümlerden birini bu sorunu düzeltin:

1. Bir cmdlet karmaşık nesneleri başka bir cmdlet'ine varsa, bu cmdlet'leri bir Inlinescript alın.
2. Nesnenin tamamı geçirme yerine karmaşık nesnesinden adını veya gereksinim duyduğunuz değerini geçirin.
3. Bir PowerShell runbook'u bir PowerShell iş akışı runbook yerine kullanın.

### <a name="quota-exceeded"></a>Senaryo: Runbook işi başarısız oldu, ayrılmış kotasını aştığı için

#### <a name="issue"></a>Sorun

Runbook işi şu hata ile başarısız olur:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Nedeni

İş yürütme hesabınız için 500 dakikalık ücretsiz kota aştığında bu hata oluşur. Bu kota bir işi sınama, gibi portaldan bir iş başlatmayı Web kancalarını kullanarak ve Azure portalını kullanarak veya yürütmek için bir iş zamanlaması bir iş yürütme, veri merkezinizdeki iş yürütme görevleri tüm türleri için geçerlidir. Otomasyon için fiyatlandırma hakkında daha fazla bilgi için bkz: [Otomasyon fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Çözüm

500 dakikadan fazla işleme aylık kullanmak istiyorsanız, ücretsiz aboneliğinizi değiştirmek gereken temel katmana katmanı. Aşağıdaki adımları uygulayarak temel katmana yükseltebilirsiniz:  

1. Azure aboneliğinizde oturum açın  
2. Yükseltmek istediğiniz Otomasyon hesabını seçin  
3. Tıklayın **ayarları** > **fiyatlandırma**.
4. Tıklatın **etkinleştirmek** hesabınıza yükseltmek için sayfa altta **temel** katmanı.

### <a name="cmdlet-not-recognized"></a>Senaryo: bir runbook yürütülürken tanınmayan cmdlet'i

#### <a name="issue"></a>Sorun

Runbook işi şu hata ile başarısız olur:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Nedeni

PowerShell altyapısı runbook'unuzda kullandığınız cmdlet bulamadığında bu hataya neden olur. Bu cmdlet içeren modülü hesabından eksik olduğundan, runbook adı ile ad çakışması veya başka bir modülde mevcut cmdlet da ve Otomasyon adı çözümlenemiyor olabilir.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:  

* Cmdlet adı doğru yazdığınızdan emin olun.  
* Cmdlet, Automation hesabınız var ve herhangi bir çakışma olduğundan emin olun. Cmdlet'in mevcut olup olmadığını doğrulamak için bir runbook düzenleme modu ve arama çalıştırın veya kitaplık bulmak istediğiniz cmdlet'in açın `Get-Command <CommandName>`. Cmdlet hesabına kullanılabilir olan doğrulatma ve diğer cmdlet'leri veya runbook'ları ad çakışması yok sonra tuvale Ekle ve geçerli bir parametre runbook'unuzda kümesi kullandığınızdan emin olun.  
* Bir ad çakışması varsa ve iki farklı modüllerde cmdlet kullanılabilir, bu cmdlet'i için tam adı kullanarak çözebilirsiniz. Örneğin, kullanabileceğiniz **ModuleName\CmdletName**.  
* Karma çalışanı grubu içinde içi runbook çalıştırıldığında modül/cmdlet karma çalışanı barındıran makinede yüklü olduğundan emin olun.

### <a name="evicted-from-checkpoint"></a>Senaryo: Bir uzun süre çalışan runbook sürekli özel durumuyla başarısız: "iş sürekli olarak aynı denetim noktasından çıkarıldı çünkü çalışmaya devam edemiyor"

#### <a name="issue"></a>Sorun

Bu davranış, "Orta Share" üç saatten uzun yürütülürse, otomatik olarak bir runbook'u askıya Azure Otomasyonu süreçlerinde izlenmesini nedeniyle tasarım gereğidir. Ancak, döndürülen hata iletisi "İleri hangi" sağlamaz seçenekleri.

#### <a name="cause"></a>Nedeni

Bir runbook birçok nedenden dolayı askıya alınabilir. Durum çoğunlukla hatalar nedeniyle askıya alır. Örneğin, yakalanmayan bir özel durum bir runbook, bir ağ hatası veya çökmeyle runbook'u çalıştıran Runbook çalışanı üzerindeki tüm askıya ve sürdürülebilir olduğunda, son denetim noktasından başlatmak runbook neden olur.

#### <a name="resolution"></a>Çözüm

Bu sorunu önlemek için belgelenmiş denetim noktaları bir iş akışında kullanmak için çözümüdür. Daha fazla bilgi edinmek için bkz [öğrenme PowerShell iş akışları](../automation-powershell-workflow.md#checkpoints). Daha kapsamlı bir açıklama "Orta paylaşımı" ve denetim noktası bu blog makalesinde bulunabilir [kullanarak denetim noktaları runbook'lardaki](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Modülleri içeri aktarırken sık karşılaşılan hataları

### <a name="module-fails-to-import"></a>Senaryo: Modülü içeri aktarmak başarısız veya cmdlet'leri içeri aktardıktan sonra yürütülemez.

#### <a name="issue"></a>Sorun

Bir modülü içeri aktarmak başarısız olursa veya başarıyla alır, ancak hiçbir cmdlet'leri ayıklanır.

#### <a name="cause"></a>Nedeni

Bir modül başarıyla Azure Otomasyonu alabilir değil bazı yaygın nedenler şunlardır:

* Yapısı, Otomasyon olması gereken yapısıyla uyuşmuyor.
* Modül, Automation hesabınızı dağıtılmadı başka bir modül bağımlıdır.
* Modülün bağımlılıklarını klasöründeki eksik.
* `New-AzureRmAutomationModule` Cmdlet, modül karşıya yüklemek için kullanılıyor ve tam depolama için belirtilen yol değil veya modülü genel olarak erişilebilir bir URL kullanarak yüklenmedi.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Modül aşağıdaki biçime uyduğundan emin olun: ModuleName.Zip **->** ModuleName veya sürüm numarası **->** (ModuleName.psm1, ModuleName.psd1)
* .Psd1 dosyasını açın ve modül bağımlılıkları olup olmadığına bakın. Varsa, bu modülleri Automation hesabına yükleyin.
* Başvurulan tüm .dll Modülü klasörde mevcut olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmemeniz veya sorunu çözmek oluşturulamıyor, daha fazla destek için aşağıdaki kanallar birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma gereksinim duyarsanız, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.