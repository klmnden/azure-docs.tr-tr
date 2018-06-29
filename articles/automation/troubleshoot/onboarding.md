---
title: Hataları ekleme güncelleştirme yönetimi, değişiklik izleme ve stok sorun giderme
description: Güncelleştirme yönetimi, değişiklik izleme ve stok çözümlerle ekleme hatalarında sorun giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 044cb56b8991a1eb2dd6a1d35be621f2ffab3250
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064039"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Hatalarında sorun giderme zaman ekleme çözümleri

Güncelleştirme yönetimi veya değişiklik izleme ve stok ekleme çözümleri gibi zaman hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunların nasıl çözüleceği açıklanır.

## <a name="general-errors"></a>Genel hata

### <a name="computer-grou-query-format-error"></a>Senaryo: ComputerGroupQueryFormatError

#### <a name="issue"></a>Sorun

Bu hata kodu çözümü hedeflemek için kullanılan kayıtlı arama bilgisayar grubu sorgusu doğru biçimlendirilmedi anlamına gelir. 

#### <a name="cause"></a>Nedeni

Sorgu değiştirilmiş veya sistem tarafından değiştirilmiş olabilir.

#### <a name="resolution"></a>Çözüm

Bu çözüm ve reonboard sorguyu yeniden oluşturur çözüm için sorgu silebilirsiniz. Sorgu, çalışma alanınızı içinde altında bulunabilir **kayıtlı aramalar**. Sorgu adı **MicrosoftDefaultComputerGroup**, ve sorgu kategorisini bu sorguyla ilişkilendirilen çözüm adıdır. Birden çok çözümleri etkinleştirilirse, **MicrosoftDefaultComputerGroup** altında birden çok kez gösterir **kayıtlı aramaları**.

### <a name="policy-violation"></a>Senaryo: PolicyViolation

#### <a name="issue"></a>Sorun

Bu hata kodu, dağıtım bir veya daha fazla ilke ihlali nedeniyle başarısız oldu anlamına gelir.

#### <a name="cause"></a>Nedeni 

İşlemin tamamlanmasını engelleyen bir yerde bir ilkedir.

#### <a name="resolution"></a>Çözüm

Başarıyla çözümü dağıtmak için belirtilen ilke değiştirilmesine dikkate almanız gerekir. Farklı türlerde tanımlanabilir ilkeleri olarak yapılması gereken belirli değişiklikler ihlal ilkesindeki bağlıdır. Bir ilke, belirli türde bir kaynak grubu içindeki kaynaklara içeriğini değiştirme izni bir kaynak grubu üzerinde tanımlanmışsa, örneğin, örneğin, aşağıdakilerden birini yapmanız olabilir:

* İlkeyi tamamen kaldırın.
* Farklı bir kaynak grubu eklemek için deneyin.
* İlke ile örneğin gözden geçirin:
  * Belirli bir kaynak (belirli bir Otomasyon hesabı gibi) ilkeyi yeniden hedefleme.
  * Belirlenen düzeltilmesi kaynakları bu ilkeyi reddedecek şekilde yapılandırıldı.

Azure portalının sağ üst köşedeki Bildirimlerde denetleyin veya seçin ve automation hesabı içeren kaynak grubuna gidin **dağıtımları** altında **ayarları** başarısız görüntülemek için dağıtımı. Azure ilke ziyaret hakkında daha fazla bilgi edinmek için: [Azure ilke genel bakış](../../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>MMA uzantı hataları

Bir çözüm dağıtırken ilgili kaynaklar çeşitli dağıtılır. Bu kaynaklar Microsoft İzleme Aracısı uzantısı ya da Linux için OMS aracısının biridir. Bu sanal makine ikili dosyaları indirilmesi sonraki düzenleme amacıyla yapılandırılan Operations Management Suite (OMS) çalışma alanı ile iletişim kurmak için sorumlu olduğu bir sanal makinenin Konuk aracısı tarafından yüklenen uzantıları ve onboarding olduğunuz çözüm kez kendisine bağımlı diğer dosyaları yürütme başlar.
Genellikle ilk MMA veya OMS Aracısı Linux yükleme hatalarını görünen bildirim hub'ı bir bildirim için farkına. Bu bildirim tıklayarak daha fazla belirli hata hakkında bilgi sağlar. Gezinme kaynak gruplarının kaynak ve ardından içerdiği dağıtımları öğesi de oluştu dağıtım hatalarını ayrıntıları sağlar.
MMA veya OMS Aracısı yüklenmesi Linux için çeşitli nedenlerden dolayı başarısız olabilir ve bu hataları gidermek için uygulanması gereken adımlar, soruna bağlı olarak değişebilir. Belirli sorun giderme adımlarını izleyin.

Aşağıdaki bölümde onboarding MMA uzantısı dağıtımdaki bir hata neden olduğunda, karşılaştığınız çeşitli sorunları açıklar.

### <a name="webclient-exception"></a>Senaryo: WebClient isteği sırasında özel durum oluştu

Sanal makinede MMA uzantısı dış kaynaklara ve dağıtım başarısız iletişim alamıyor.

#### <a name="issue"></a>Sorun

Döndürülen hata iletilerini örnekleri verilmiştir:

```
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Nedeni

Bu hata için olası nedenleri şunlardır:

* Yalnızca belirli bağlantı noktalarına izin veren bir VM'de, yapılandırılmış bir proxy yoktur.

* Bir güvenlik duvarı ayarını gerekli bağlantı noktaları ve adreslerinin erişimi engelledi.

#### <a name="resolution"></a>Çözüm

Uygun bağlantı noktalarının olması ve iletişim için adreslerini açma emin olun. Bağlantı noktaları ve adresleri listesi için bkz: [ağınızı planlama](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Senaryo: Geçici ortam sorunları nedeniyle yükleme başarısız oldu

Başka bir yükleme veya yüklemeyi engelleme eylemi nedeniyle dağıtım sırasında Microsoft İzleme Aracısı uzantısı yüklenemedi

#### <a name="issue"></a>Sorun

Hata iletileri örnekleri döndürülebilir şunlardır:

```
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Nedeni

Bu hata için olası nedenleri şunlardır:

* Başka bir yükleme devam ediyor
* Sistem şablon dağıtımı sırasında yeniden tetiklendi

#### <a name="resolution"></a>Çözüm

Bu hata, yapısı geçici bir hatadır. Uzantıyı yüklemek için dağıtımı yeniden deneyin.

### <a name="installation-timeout"></a>Senaryo: Yükleme zaman aşımı

MMA uzantının yüklenmesi bir zaman aşımı nedeniyle tamamlanmadı.

#### <a name="issue"></a>Sorun

Döndürülen hata iletisi örneği verilmiştir:

```
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Nedeni

Bu hata, yükleme sırasında ağır bir yük altında olan sanal makine ilgilidir.

### <a name="resolution"></a>Çözüm

VM alt yük altında olduğunda MMA uzantısı yüklenmeye çalışıldı.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmemeniz veya sorunu çözmek oluşturulamıyor, daha fazla destek için aşağıdaki kanallar birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma gereksinim duyarsanız, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
