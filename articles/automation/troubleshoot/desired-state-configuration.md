---
title: İle Azure Otomasyonu istenen durum yapılandırması (DSC) sorunlarını giderme
description: Bu makalede istenen durum Yapılandırması'nı (DSC) sorunlarını giderme hakkında bilgiler sağlanmaktadır.
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5d2eae67fcff74a7016f7f6125e31a9c8c2bda97
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064125"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>İstenen durum yapılandırması (DSC) sorunlarını giderme

Bu makalede, istenen durum yapılandırması (DSC) ile sorunlarını giderme hakkında bilgi sağlar.

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>İstenen durum yapılandırması (DSC) ile çalışırken sık karşılaşılan hataları

### <a name="failed-not-found"></a>Senaryo: Bir "Bulunamadı" hatası ile başarısız durumundaki düğümdür

#### <a name="issue"></a>Sorun

Bir raporla düğümü sahip **başarısız** durum ve hata içeren:

```
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Nedeni

Düğüm yapılandırma adı (örneğin, ABC) atandığında, bu hata genellikle düğüm yapılandırma adı (örneğin, ABC. yerine oluşur Web sunucusu).

#### <a name="resolution"></a>Çözüm

* "Düğüm yapılandırması adı" ve "yapılandırma adı değil" düğümle atıyorsanız emin olun.
* Azure portalını kullanarak bir düğüme veya bir PowerShell cmdlet ile bir düğüm yapılandırması atayabilirsiniz.

  * Düğüm yapılandırması Azure portal kullanarak bir düğüme atamak için açık **DSC düğümleri** sayfası, ardından bir düğüm seçin ve tıklayın **Ata düğüm yapılandırması** düğmesi.  
  * Düğüm yapılandırması PowerShell cmdlet'ini kullanarak bir düğüme atamak için kullanılması **kümesi AzureRmAutomationDscNode** cmdlet'i

### <a name="no-mof-files"></a>Senaryo: düğüm yapılandırması (MOF dosyaları) bir yapılandırma derlendiğinde oluşturulamadığı

#### <a name="issue"></a>Sorun

DSC derleme işi şu hata ile askıya alır:

```
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Nedeni

Zaman ifade aşağıdaki **düğümü** DSC yapılandırmada anahtar sözcüğü hesaplar için `$null`, düğüm yapılandırması üretilen sonra.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden birini sorunu düzeltin:

* Olduğundan emin olun yanındaki ifade **düğümü** $null için anahtar sözcüğü yapılandırma tanımında değerlendirme değil.
* Yapılandırma derlerken ConfigurationData geçirme, gelen yapılandırma gerektirir beklenen değerler geçirdiğinizden emin olun [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Senaryo: DSC düğümü rapor "Sürüyor" durumunda kalmış olur

#### <a name="issue"></a>Sorun

DSC Aracısı çıkarır:

```
No instance found with given property values
```

#### <a name="cause"></a>Nedeni

WMF sürümünüzü yükselttiniz ve WMI bozulmuş.

#### <a name="resolution"></a>Çözüm

Sorun izleme yönergeleri düzeltmek için [bilinen sorunlar ve sınırlamalar DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) makalesi.

### <a name="issue-using-credential"></a>Senaryo: Bir kimlik bilgisi bir DSC yapılandırması kullanılamıyor

#### <a name="issue"></a>Sorun

DSC derleme işi şu hata ile askıya alındı:

```
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Nedeni

Bir kimlik bilgisi bir yapılandırmada kullanmış ancak uygun sağlamadı **ConfigurationData** ayarlamak için **PSDscAllowPlainTextPassword** her düğüm yapılandırması için true.

#### <a name="resolution"></a>Çözüm

* Uygun aktardığınızdan emin olun **ConfigurationData** ayarlamak için **PSDscAllowPlainTextPassword** yapılandırmasında belirtilen her düğüm yapılandırması için true. Daha fazla bilgi için bkz: [Azure Otomasyonu DSC varlıkları](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmemeniz veya sorunu çözmek oluşturulamıyor, daha fazla destek için aşağıdaki kanallar birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma gereksinim duyarsanız, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.