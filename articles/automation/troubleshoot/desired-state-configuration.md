---
title: İle Azure Otomasyonu Desired State Configuration (DSC) ile ilgili sorunları giderme
description: Bu makale Desired State Configuration ' nı (DSC) sorun giderme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c8afa671a323e37a99be8b5a43d0a4823fe1877a
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58800885"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Desired State Configuration (DSC) sorunlarını giderme

Bu makalede, sorun giderme konuları Desired State Configuration (DSC) ile hakkında bilgi sağlar.

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Desired State Configuration (DSC) ile çalışırken sık karşılaşılan hatalar

### <a name="unsupported-characters"></a>Senaryo: Portaldan özel karakterler içeren bir yapılandırma silinemez.

#### <a name="issue"></a>Sorun

Portaldan bir DSC yapılandırması silme girişiminde bulunduğunuzda aşağıdaki hatayı görürsünüz:

```error
An error occured while deleteing the DSC configuration '<name>'.  Error-details: The arguement configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Nedeni

Bu hatanın çözülmesi için planlanan geçici bir sorundur.

#### <a name="resolution"></a>Çözüm

* Yapılandırmayı silmek için "Remove-AzAutomationDscConfiguration" Az cmdlet'ini kullanın.
* Bu cmdlet belgeleri henüz güncelleştirilemedi.  O zamana kadar AzureRM modülünü belgelerine bakın.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-not-found"></a>Senaryo: Bir "Bulunamadı" hatası ile başarısız durumundaki düğümüdür

#### <a name="issue"></a>Sorun

Düğüm içeren bir rapor olan **başarısız** durumu ve hata içeren:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Nedeni

Düğüm yapılandırması adı (örneğin, ABC) atandığında bir düğüm yapılandırması adı yerine (örneğin, ABC. Bu hata genellikle oluşur. Web sunucusu).

#### <a name="resolution"></a>Çözüm

* "Düğüm yapılandırması adı" ve "yapılandırma adı değil" düğümle atama emin olun.
* Düğüm yapılandırması, Azure portalını kullanarak bir düğüme veya bir PowerShell cmdlet'i ile atayabilirsiniz.

  * Azure portalını kullanarak bir düğüme bir düğüm yapılandırması Ata için açın **DSC düğümleri** sayfası, ardından bir düğüm seçin ve tıklayın **düğüm yapılandırması Ata** düğmesi.  
  * Düğüm yapılandırması PowerShell cmdlet'ini kullanarak bir düğüme atamak için kullanın **kümesi AzureRmAutomationDscNode** cmdlet'i

### <a name="no-mof-files"></a>Senaryo: Düğüm yapılandırması (MOF dosyaları) bir yapılandırma derlendiğinde üretilmiş

#### <a name="issue"></a>Sorun

DSC derleme işi askıya alır ve şu hata oluştu:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Nedeni

Zaman ifade aşağıdaki **düğüm** DSC yapılandırma anahtar sözcüğü değerlendirilen `$null`, düğüm yapılandırması üretilen sonra.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Emin olun yanındaki ifade **düğüm** yapılandırma tanımı'ndaki anahtar sözcüğü için $null değerlendirme değil.
* Yapılandırma derlenirken ConfigurationData geçiriyorsanız, gelen yapılandırma gerektirir. beklenen değerler geçirdiğinizden emin olun [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Senaryo: DSC düğüm raporu "devam ediyor" durumuna kalmış olur

#### <a name="issue"></a>Sorun

DSC aracı çıkışlar:

```error
No instance found with given property values
```

#### <a name="cause"></a>Nedeni

WMF sürümünüzü yükseltmişseniz ve WMI bozulmuş.

#### <a name="resolution"></a>Çözüm

Bu sorunu düzeltmek için yönergeleri izleyin. [bilinen sorunlar ve sınırlamalar DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) makalesi.

### <a name="issue-using-credential"></a>Senaryo: Bir kimlik bilgisi bir DSC yapılandırması kullanılamıyor

#### <a name="issue"></a>Sorun

Hata, DSC derleme işi askıya alındı:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Nedeni

Bir kimlik bilgisi bir yapılandırmada kullandınız ancak uygun sağlamadı **ConfigurationData** ayarlanacak **PSDscAllowPlainTextPassword** her düğüm yapılandırması için true.

#### <a name="resolution"></a>Çözüm

* Uygun geçirdiğinizden emin olun **ConfigurationData** ayarlanacak **PSDscAllowPlainTextPassword** yapılandırmasında belirtilen her düğüm yapılandırması için true. Daha fazla bilgi için [Azure Automation DSC varlıkları](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
