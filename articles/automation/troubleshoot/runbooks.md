---
title: Azure Otomasyonu runbook'ları hatalarla ilgili sorunları giderme
description: Azure Otomasyonu runbook'ları ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: b96d723f6c7ca423343c0586f59770abb55ada9f
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929358"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Runbook'ları ile hatalarını giderme

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Azure Otomasyonu runbook'ları ile çalışırken kimlik doğrulaması hataları

### <a name="sign-in-failed"></a>Senaryo: Azure hesap için oturum açın

#### <a name="issue"></a>Sorun

İle çalışırken aşağıdaki hata iletisini `Add-AzureAccount` veya `Connect-AzureRmAccount` cmdlet'leri.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Nedeni

Kimlik bilgisi varlığı adı geçerli değil veya kullanıcı adı ve Otomasyonu kimlik bilgisi varlığı ' için kullandığınız parola geçerli değilse bu hata oluşur.

#### <a name="resolution"></a>Çözüm

Neyin yanlış olduğunu belirlemek için aşağıdaki adımları uygulayın:  

1. Dahil olmak üzere herhangi bir özel karakter içermediğinden emin olun **@** Azure'a bağlanmak için kullandığınız Otomasyon kimlik bilgisi varlığı ad karakteri.  
2. Azure Otomasyonu kimlik bilgisi, yerel PowerShell ISE Düzenleyici içinde depolanan parola ve kullanıcı adı kullanıp kullanmadığını denetleyin. PowerShell ISE'de aşağıdaki cmdlet'leri çalıştırarak bunu yapabilirsiniz:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Yerel olarak, kimlik doğrulama başarısız olursa, bu, Azure Active Directory bilgilerinizi düzgün bir şekilde kurmadığınızı fark anlamına gelir. Başvurmak [Azure Active Directory'yi kullanarak Azure için kimlik doğrulama](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blog gönderisi doğru şekilde ayarlanan Azure Active Directory hesabı.  

### <a name="unable-to-find-subscription"></a>Senaryo: Azure aboneliği bulunamadı

#### <a name="issue"></a>Sorun

İle çalışırken aşağıdaki hata iletisini `Select-AzureSubscription` veya `Select-AzureRmSubscription` cmdlet'leri.:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Hata

Abonelik adı geçerli değil veya abonelik ayrıntıları get yapılmaya çalışılırken Azure Active Directory kullanıcı Abonelik Yöneticisi olarak yapılandırılmamış olması durumunda bu hata oluşur.

#### <a name="resolution"></a>Çözüm

Düzgün bir şekilde Azure'a kimliği doğrulanmış ve seçmek için çalıştığınız abonelik erişimi belirlemek için aşağıdaki adımları uygulayın:  

1. Siz çalıştırdığınızdan emin olun **Add-AzureAccount** çalıştırmadan önce **Select-AzureSubscription** cmdlet'i.  
2. Ekleyerek bu hatayı görmeye devam ediyorsanız, kodunuzu değiştirmeniz **Get-AzureSubscription** cmdlet'i aşağıdaki **Add-AzureAccount** cmdlet'ini ve ardından kod yürütün. Şimdi, Get-AzureSubscription çıktısını abonelik bilgilerinizi içerip içermediğini doğrulayın.  

   * Herhangi bir abonelik ayrıntıları çıktıda görmüyorsanız, bu abonelik henüz başlatılmadı anlamına gelir.  
   * Abonelik Ayrıntıları çıktıda görürseniz doğru abonelik adını veya kimliği ile kullandığınızdan emin olun **Select-AzureSubscription** cmdlet'i.

### <a name="auth-failed-mfa"></a>Senaryo: çok faktörlü kimlik doğrulaması etkin olmadığından Azure kimlik doğrulaması başarısız oldu

#### <a name="issue"></a>Sorun

Aldığınız Azure'da, Azure kullanıcı adı ve parola ile kimlik doğrulaması yapılırken şu hata:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Nedeni

Çok faktörlü kimlik doğrulaması Azure hesabınız varsa, Azure için kimlik doğrulaması için bir Azure Active Directory kullanıcı kullanamazsınız. Bunun yerine, bir sertifika veya bir hizmet sorumlusu, Azure'de kimlik doğrulamasını kullanmanız gerekir.

#### <a name="resolution"></a>Çözüm

Azure Klasik dağıtım modeli cmdlet'leriyle bir sertifikayı kullanmak için başvurmak [oluşturma ve Azure hizmetlerini yönetmek için bir sertifika ekleme.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Bir hizmet sorumlusu ile Azure Resource Manager cmdlet'lerini kullanmak için başvurmak [hizmet sorumlusu Azure portalını kullanarak oluşturma](../../azure-resource-manager/resource-group-create-service-principal-portal.md) ve [Azure Resource Manager ile hizmet sorumlusu kimlik doğrulaması.](../../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Runbook'larla çalışırken sık karşılaşılan hatalar

### <a name="job-attempted-3-times"></a>Senaryo: Runbook işi başlangıç üç kez denendi ancak her zaman başlatılamadı

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden kaynaklanabilir:

1. Bellek sınırı. Bir korumalı alan için ayrılan bellek miktarını üzerinde belgelenmiş sınırlamanın vardır [Otomasyon hizmet sınırları](../../azure-subscription-service-limits.md#automation-limits) şekilde 400 MB'tan fazla bellek kullanıyorsa, bir işi başarısız olabilir.

2. Modül uyumsuz. Modül bağımlılıklarının doğru değildir ve değilse, runbook'unuzda genellikle "komut bulunamadı" döndürürse oluşabilir veya "parametresi bağlanılamıyor" iletisi.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Bellek sınırı içinde çalışmak için önerilen yöntemler bellekteki gereksiz çıkış, runbook'lardan yazamazlar kadar verileri işlemek veya PowerShell akışınıza yazma kaç kontrol noktaları göz önünde bulundurun değil iş yükünü birden çok runbook arasında bölmek üzeresiniz runbook'ları.  

* Azure modüllerini adımları izleyerek [Azure automation'da Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Senaryo: Runbook seri durumdan çıkarılmış nesne nedeniyle başarısız olur.

#### <a name="issue"></a>Sorun

Runbook'unuzda hatasıyla başarısız oluyor:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Nedeni

Runbook'unuzdaki bir PowerShell iş akışı, iş akışını askıya alınırsa, runbook durumu kalıcı hale getirmek için seri durumdan çıkarılmış biçimde karmaşık nesneler depolar.

#### <a name="resolution"></a>Çözüm

Aşağıdaki üç çözümlerden birini bu sorunu düzeltin:

1. Karmaşık bir cmdlet nesnelerden diğerine öğesine ekleyerek sorgularınızı, bu cmdlet'ler bir Inlinescript kaydır.
2. Tüm nesneyi geçirmek yerine karmaşık nesnesinden adını veya ihtiyaç duyduğunuz değerini geçirin.
3. PowerShell runbook'u bir PowerShell iş akışı runbook'u yerine kullanın.

### <a name="quota-exceeded"></a>Senaryo: Runbook işi başarısız oldu, ayrılmış kotasını aştığı için

#### <a name="issue"></a>Sorun

Runbook iş şu hatayla başarısız oluyor:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Nedeni

İş yürütme hesabınız için 500 dakikalık ücretsiz kotayı aşıyor. Bu hata oluşur. Bu kota bir işi test etme, portaldan bir işi başlatılıyor, Web kancalarını kullanma ve Azure portalını kullanarak veya yürütmek için bir iş zamanlama iş yürütme, veri merkezinizdeki iş yürütme görevleri tüm türleri için geçerlidir. Otomasyon için fiyatlandırma hakkında daha fazla bilgi için bkz: [Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Çözüm

Ücretsiz aboneliğinizi değiştirmek gereken işleme başına aylık 500 dakikadan fazla kullanmak istiyorsanız, temel katmana katman. Aşağıdaki adımları izleyerek, temel katmana yükseltebilirsiniz:  

1. Azure aboneliğinizde oturum açın  
2. Yükseltmek istediğiniz Otomasyon hesabını seçin  
3. Tıklayarak **ayarları** > **fiyatlandırma**.
4. Tıklayın **etkinleştirme** hesabınıza yükseltmek için alt sayfa üzerinde **temel** katmanı.

### <a name="cmdlet-not-recognized"></a>Senaryo: bir runbook çalıştırılırken tanınmıyor cmdlet'i

#### <a name="issue"></a>Sorun

Runbook iş şu hatayla başarısız oluyor:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Nedeni

PowerShell altyapısı runbook'unuzu kullandığınız cmdlet bulamadığında bu hataya neden olur. Bu cmdlet içeren modül hesaptan eksik olduğundan, bir runbook adı ile bir ad çakışması veya cmdlet ayrıca başka bir modülde var ve Otomasyon adı çözümlenemiyor olabilir.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:  

* Cmdlet adı doğru yazdığınızdan emin olun.  
* Cmdlet, Otomasyon hesabınızda var olduğundan ve hiçbir çakışma yok emin olun. Cmdlet'in mevcut olup olmadığını doğrulamak için düzenleme modu ve arama çalıştırın veya kitaplığı içinde bulmak istediğiniz cmdlet'in bir runbook'u açın `Get-Command <CommandName>`. Hesabına cmdlet kullanılabilir doğrulandıktan ve diğer cmdlet'leri veya runbook'ları adı çakışması yok sonra tuvaline ekleyin ve runbook'unuzu ayarlayın, geçerli bir parametre kullandığından emin olun.  
* Bir ad çakışması varsa ve iki farklı modülde cmdlet kullanılabilir, bu cmdlet'i için tam olarak nitelenmiş adını kullanarak çözebilirsiniz. Örneğin, kullanabileceğiniz **ModuleName\CmdletName**.  
* Karma çalışanı grubu içinde şirket runbook yürütme, modül/cmdlet karma çalışanı barındıran makinede yüklü olduğundan emin olun.

### <a name="evicted-from-checkpoint"></a>Senaryo: Uzun süre çalışan bir runbook tutarlı bir şekilde özel durumuyla başarısız oluyor: "işi tekrar tekrar aynı denetim noktasından çıkarıldığından çalıştırmaya devam edemiyor"

#### <a name="issue"></a>Sorun

Bu davranış, üç saatten uzun çalıştırırsa, otomatik olarak bir runbook'u askıya Azure automation'da işlemlerinin "Adil Share" izleme nedeniyle tasarım gereğidir. Ancak, döndürülen hata iletisi "sonraki adım" sağlamaz seçenekleri.

#### <a name="cause"></a>Nedeni

Bir runbook için pek çok askıya alınabilir. Sorun çoğunlukla hatalar nedeniyle askıya alır. Örneğin, yakalanmayan bir özel durum bir runbook, bir ağ hatası veya runbook'un çalıştığı Runbook çalışanı üzerinde bir kilitlenme ve runbook'un askıya alınması ve sürdürüldü olduğunda, son denetim noktasından Başlat tüm neden.

#### <a name="resolution"></a>Çözüm

Bu sorunu önlemek için belgelenmiş çözümü, bir iş akışında denetim noktası kullanmaktır. Daha fazla bilgi edinmek için bkz [Learning PowerShell iş akışları](../automation-powershell-workflow.md#checkpoints). Daha kapsamlı bir açıklama "Adil paylaşımı" ve kontrol noktası bu blog makalesinde bulunabilir [kontrol noktalarının kullanılması sayesinde runbook'larda](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

### <a name="long-running-runbook"></a>Senaryo:, Tamamlanması uzun süre çalışan bir runbook başarısız.

#### <a name="issue"></a>Sorun

Bu davranış, üç saatten uzun çalıştırırsa, otomatik olarak bir runbook'u askıya Azure automation'da işlemlerinin "Adil Share" izleme nedeniyle Azure korumalı alanlarda bulunan tasarım gereğidir.

#### <a name="cause"></a>Nedeni

Runbook bir Azure sanal adil paylaşımı tarafından izin verilen 3 saatlik sınırın üstünde çalıştı

#### <a name="resolution"></a>Çözüm

Runbook'u çalıştırmak için önerilen çözümdür bir [karma Runbook çalışanı](../automation-hrw-run-runbooks.md). Karma çalışanları tarafından sınırı yoktur [adil paylaşımı](../automation-runbook-execution.md#fair-share) Azure sanal olan 3 saat runbook sınırı.

## <a name="common-errors-when-importing-modules"></a>Modüller içeri aktarılırken yaygın hataları

### <a name="module-fails-to-import"></a>Senaryo: Modülü içeri aktarmak başarısız olursa veya cmdlet'leri içeri aktardıktan sonra yürütülemez.

#### <a name="issue"></a>Sorun

Bir modülü içeri aktarmak başarısız olursa veya başarıyla alır, ancak hiçbir cmdlet'leri ayıklanır.

#### <a name="cause"></a>Nedeni

Bir modül başarıyla Azure Otomasyonu'na ekleme alabilir olmayan bazı yaygın nedenleri şunlardır:

* Yapı Otomasyonu olması gereken yapısı eşleşmiyor.
* Modül, Otomasyon hesabınıza dağıtılan başka bir modül bağımlıdır.
* Modülün bağımlılıklarını klasöründe eksik.
* `New-AzureRmAutomationModule` Cmdlet'i, modül karşıya yüklemek için kullanılıyor ve tam depolama yolu verilmedi veya genel olarak erişilebilir bir URL kullanarak modülü yüklenmedi.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Modül aşağıdaki biçimde uyduğundan emin olun: ModuleName.Zip **->** ModuleName veya sürüm numarası **->** (ModuleName.psm1, ModuleName.psd1)
* .Psd1 dosyasını açın ve modülü herhangi bir bağımlılığın olup olmadığına bakın. Aksi halde bu modüller Otomasyon hesabına yükleyin.
* Başvurulan tüm .dll Modülü klasörde mevcut olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görülmez veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.