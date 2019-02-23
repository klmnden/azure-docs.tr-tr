---
title: Azure Otomasyonu Paylaşılan kaynaklarla hatalarını giderme
description: Azure Otomasyonu paylaşılan kaynakları ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: abce40958f8d775e0a579a18cf8d1351740031ff
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56671072"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Paylaşılan kaynaklar hatalarla ilgili sorunları giderme

Bu makalede, paylaşılan kaynaklar Azure Automation'da kullanırken çalışabilir genelinde sorunları çözmek için çözümleri açıklanmaktadır.

## <a name="modules"></a>Modüller

### <a name="module-stuck-importing"></a>Senaryo: Bir modülü içeri aktarma takıldı

#### <a name="issue"></a>Sorun

Bir modül olarak takılmış **alma** aktardığınızda veya modüllerinizi Azure Otomasyonu'nda güncelleştirme durumu.

#### <a name="cause"></a>Nedeni

PowerShell modüllerini içeri aktarma karmaşık çok adımlı bir işlemdir. Bu işlem, doğru şekilde içeri değil bir modül olasılığını tanıtır. Bu gerçekleşirse, geçici bir durumda aldığınız modülü takılabilir. Bu işlem hakkında daha fazla bilgi için bkz. [PowerShell modülünü içeri aktararak]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için takılmış modülü kaldırmak **alma** kullanarak durum [Remove-Azurermautomationmodule'a](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet'i. Modülün içeri aktarılması daha sonra yeniden deneyebilir.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

### <a name="update-azure-modules-importing"></a>Senaryo: AzureRM modülleri denedikten sonra bunları güncelleştirmek alma takılır

#### <a name="issue"></a>Sorun

Şu iletiyle bir başlık, AzureRM modüllerini denedikten sonra hesabınızda kalır:

```
Azure modules are being updated
```

#### <a name="cause"></a>Nedeni

0 ile başlayan sayısal bir ada sahip bir kaynak grubu içinde bir Otomasyon hesabı AzureRM modülleri güncelleştirme ile ilgili bilinen bir sorun yoktur.

#### <a name="resolution"></a>Çözüm

Otomasyon hesabınızda, Azure modüllerini güncelleştir için alfasayısal bir ada sahip bir kaynak grubunda olmalıdır. 0 ile başlayan sayısal adlara sahip kaynak gruplarını AzureRM modülleri şu anda güncelleştiremiyor.

### <a name="module-fails-to-import"></a>Senaryo: Modülü içeri aktarmak başarısız olursa veya cmdlet'leri içeri aktardıktan sonra yürütülemez.

#### <a name="issue"></a>Sorun

Bir modülü içeri aktarmak başarısız olursa veya başarıyla alır, ancak hiçbir cmdlet'leri ayıklanır.

#### <a name="cause"></a>Nedeni

Bir modül başarıyla Azure Otomasyonu'na ekleme alabilir olmayan bazı yaygın nedenleri şunlardır:

* Yapı Otomasyonu olması gereken yapısı eşleşmiyor.
* Modül, Otomasyon hesabınıza dağıtmamış başka bir modül bağlıdır.
* Modülün bağımlılıklarını klasöründe eksik.
* `New-AzureRmAutomationModule` Cmdlet'i, modül karşıya yüklemek için kullanılıyor ve tam depolama yolu verildiğinde yapmadıysanız veya modülü genel olarak erişilebilir bir URL kullanarak yüklenen henüz.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Modül aşağıdaki biçimde uyduğundan emin olun: ModuleName.Zip **->** ModuleName veya sürüm numarası **->** (ModuleName.psm1, ModuleName.psd1)
* .Psd1 dosyasını açın ve modülü herhangi bir bağımlılığın olup olmadığına bakın. Aksi halde bu modüller Otomasyon hesabına yükleyin.
* Başvurulan tüm .dll Modülü klasörde mevcut olduğundan emin olun.

### <a name="all-modules-suspended"></a>Senaryo: Modülleri güncelleştirme yapılırken güncelleştirme AzureModule.ps1 askıya alır

#### <a name="issue"></a>Sorun

Kullanırken [güncelleştirme AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) modülü güncelleştirme güncelleştirme işlemini askıya, Azure modüllerini güncelleştirmek için runbook.

#### <a name="cause"></a>Nedeni

Kaç tane modül aynı anda güncelleştirilmesini belirlemek için varsayılan ayar 10 kullanıldığında `Update-AzureModule.ps1` betiği. Çok fazla modül aynı anda güncelleştirildiğinde güncelleştirme işleminin hatalara açıktır.

#### <a name="resolution"></a>Çözüm

AzureRM modülleri aynı Otomasyon hesabında gereklidir yaygın değildir. Yalnızca gereksinim duyduğunuz AzureRM modülleri içeri aktarmak için önerilir.

> [!NOTE]
> İçeri aktarma önlemek **AzureRM** modülü. İçeri aktarma **AzureRM** modülleri neden olan tüm **AzureRM.\***  modülleri içeri aktarılacak gidermede kullanılması bu değil.

Güncelleştirme işlemi askıya alır, eklemenize gerek `SimultaneousModuleImportJobCount` parametresi `Update-AzureModules.ps1` betik ve 10 varsayılandan daha düşük bir değer sağlayın. 3 veya 5 değeriyle başlatmak için bu mantıksal uygulamanız önerilir. `SimultaneousModuleImportJobCount` bir parametresi `Update-AutomationAzureModulesForAccount` Azure modüllerini güncelleştirmek için kullanılan sistem runbook. Bu değişiklik işlemi artık çalışmasına neden olur, ancak tamamlama daha iyi belirtme şansı olur. Aşağıdaki örnek, parametre ve runbook'ta yerleştirileceği yeri gösterir:

 ```powershell
         $Body = @"
            {
               "properties":{
               "runbook":{
                   "name":"Update-AutomationAzureModulesForAccount"
               },
               "parameters":{
                    ...
                    "SimultaneousModuleImportJobCount":"3",
                    ... 
               }
              }
           }
"@
```

## <a name="run-as-accounts"></a>Farklı Çalıştır hesapları

### <a name="unable-create-update"></a>Senaryo: Oluşturulacak veya güncelleştirilecek bir farklı çalıştır hesabı oluşturulamıyor

#### <a name="issue"></a>Sorun

Oluşturulacak veya güncelleştirilecek bir farklı çalıştır hesabı denediğinizde aşağıdaki hata iletisine benzer bir hata iletisi:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Nedeni

Bir kaynak grubu düzeyinde kaynak kilitli veya oluşturmak veya farklı çalıştır hesabını güncelleştirmek gereken izinlere sahip değilsiniz.

#### <a name="resolution"></a>Çözüm

Oluşturun veya bir farklı çalıştır hesabını güncelleştirmek için farklı çalıştır hesabı kullanılan çeşitli kaynakları için uygun izinleri olmalıdır. Oluşturmak veya bir farklı çalıştır hesabını güncelleştirmek için gereken izinleri hakkında bilgi edinmek için [farklı çalıştır hesabı izinleri](../manage-runas-account.md#permissions).

Bir kilit nedeniyle sorunu yaşıyorsanız, kilidi kaldırmak için Tamam olduğunu doğrulayın. Ardından, kilitli kaynağa gidin, kilit sağ tıklatın ve seçin **Sil** kilidini kaldırmak için.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.