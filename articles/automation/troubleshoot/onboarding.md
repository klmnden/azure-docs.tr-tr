---
title: Hata ekleme güncelleştirme yönetimi, değişiklik izleme ve stok sorunlarını giderme
description: Güncelleştirme yönetimi, değişiklik izleme ve stok çözümleriyle ekleme hatalarında sorun giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: eaafee304f606ae4d511a6cea1824c26db838635
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119138"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Hatalarında sorun giderme, onboarding çözümleri

Güncelleştirme yönetimi veya değişiklik izleme ve sayım gibi çözümleri ekleme sırasında hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hataları ve bunların nasıl çözüleceğine açıklanır.

## <a name="general-errors"></a>Genel hatalar

### <a name="missing-write-permissions"></a>Senaryo: Çözüm etkinleştirilemiyor - ekleme iletisi ile başarısız oluyor

#### <a name="issue"></a>Sorun

Yerleşik bir çözüm için bir sanal makine denediğinizde aşağıdaki iletilerinden birini alabilirsiniz:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Nedeni

Bu hata, çalışma alanında, sanal makine veya kullanıcı için eksik veya hatalı izinleri kaynaklanır.

#### <a name="resolution"></a>Çözüm

Sanal makine eklemek için doğru izinlere sahip olun. Gözden geçirme [makine için gereken izinleri](../automation-role-based-access-control.md#onboarding) ve ekleme için çözümü yeniden deneyin. Hatasını alırsanız `The solution cannot be enabled on this VM because the permission to read the workspace is missing`, olduğundan emin olun `Microsoft.OperationalInsights/workspaces/read` VM için bir çalışma alanına eklenen olup olmadığını öğrenmek için izni.

### <a name="computer-group-query-format-error"></a>Senaryo: ComputerGroupQueryFormatError

#### <a name="issue"></a>Sorun

Bu hata kodu, çözüm hedeflemek için kayıtlı arama bilgisayar grubu sorgusu düzgün biçimlendirilmemiş anlamına gelir. 

#### <a name="cause"></a>Nedeni

Sorgu değiştirmiş veya sistem tarafından değiştirilmiş olabilir.

#### <a name="resolution"></a>Çözüm

Bu çözüm ve sorguyu yeniden oluşturur çözüm reonboard için sorgu silebilirsiniz. Sorgu içinde çalışma alanınızda, altında bulunabilir **kayıtlı aramalar**. Sorgu adı **MicrosoftDefaultComputerGroup**, ve sorgu kategorisini bu sorguyla ilişkilendirilen çözüm adıdır. Birden çok çözümü etkinleştirilip etkinleştirilmediğini **MicrosoftDefaultComputerGroup** altında birden çok kez gösterir **kayıtlı aramalar**.

### <a name="policy-violation"></a>Senaryo: PolicyViolation

#### <a name="issue"></a>Sorun

Bu hata kodu dağıtımda bir veya daha fazla ilke ihlali nedeniyle başarısız olduğunu gösterir.

#### <a name="cause"></a>Nedeni 

İşlemin tamamlanmasını engelleyen bir yerde bir ilkedir.

#### <a name="resolution"></a>Çözüm

Çözüm başarıyla dağıtmak için belirtilen ilke değiştirmeyi göz önüne almanız gerekir. Farklı türlerde tanımlanabilir ilkeleri gibi gerekli belirli değişiklikleri ihlal ilkesindeki bağlıdır. Bir ilke, belirli bir türdeki kaynakları bu kaynak grubunda içeriğini değiştirme izni olan bir kaynak grubu tanımlanmışsa, örneğin, örneğin, aşağıdakilerden birini yapmanız olabilir:

* İlke kaydetme işlemini tamamen kaldırın.
* Farklı kaynak grubuna eklemek için bu seçeneği deneyin.
* İlke ile bir örneğin gözden geçirin:
  * Yeniden hedefleme İlkesi belirli bir kaynağa (gibi belirli bir Otomasyon hesabı).
  * Düzeltme kümesi kaynakları bu ilke reddetmek üzere yapılandırıldı.

Azure portalının sağ üst köşedeki bildirimleri denetleyin veya seçin ve Otomasyon hesabını içeren kaynak grubuna gidin **dağıtımları** altında **ayarları** başarısız görüntülemek için Dağıtım. Azure İlkesi ziyaret hakkında daha fazla bilgi için: [Azure İlkesi'ne genel bakış](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>MMA uzantısı hatalarında

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Bir çözümü dağıtırken, çeşitli ilgili kaynaklara dağıtılır. Bu kaynakları Linux için Microsoft Monitoring Agent uzantısı veya Log Analytics aracısını biridir. Bu ikili dosyaları indiriliyor, sonraki koordinasyon amacıyla yapılandırılan Log Analytics çalışma alanıyla iletişim kurmak için sorumlu olduğu bir sanal makinenin Konuk aracısı tarafından yüklenen sanal makine uzantıları ve diğer dosyalar İşiniz çözüm ekleme bağlı olduğu yürütme başladıktan sonra.
Genellikle ilk Linux yükleme hatalarını bildirim Merkezi'nde görünen bildirim için MMA veya Log Analytics Aracısı'nın haberdar edilir. Bu bildirimi tıklayarak daha fazla belirli hata hakkında bilgi sağlar. Kaynak grupları kaynağa ve içerdiği dağıtımları öğeye gezinme, ayrıca oluşan dağıtım hatalarının ayrıntıları sağlar.
Linux için MMA veya Log Analytics aracısını yükleme çeşitli nedenlerden dolayı başarısız olabilir ve bu hataları gidermek için adımları, soruna bağlı olarak değişebilir. Belirli sorun giderme adımlarını izleyin.

Aşağıdaki bölümde onboarding MMA uzantısı dağıtımda bir hata neden olduğunda, gelen arasında çeşitli sorunlar açıklanmaktadır.

### <a name="webclient-exception"></a>Senaryo: WebClient isteği sırasında bir özel durum oluştu

Sanal makinede MMA uzantısı başarısız dış kaynaklara ve dağıtım ile iletişim kuramıyor.

#### <a name="issue"></a>Sorun

Döndürülen hata iletilerini örnekleri şunlardır:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Nedeni

Bu hata için olası nedenleri şunlardır:

* Yalnızca belirli bağlantı noktalarına izin veren bir VM'de, yapılandırılmış bir proxy yoktur.

* Bir güvenlik duvarı ayarını, gerekli bağlantı noktaları ve adres erişimi engelledi.

#### <a name="resolution"></a>Çözüm

Doğru bağlantı noktalarının olması ve iletişim için adreslerini açmak emin olun. Bağlantı noktaları ve adresleri listesi için bkz. [ağınızı planlama](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Senaryo: Geçici ortam sorunları nedeniyle yükleme başarısız oldu

Microsoft Monitoring Agent uzantısını yükleme dağıtım sırasında başka bir yükleme veya yüklemeyi engelleme eylemi nedeniyle başarısız oldu

#### <a name="issue"></a>Sorun

Döndürülen hata iletilerini örnekleri şunlardır:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Nedeni

Bu hata için olası nedenleri şunlardır:

* Başka bir yükleme devam ediyor
* Şablon dağıtımı sırasında yeniden başlatmak için sistem tetiklenir

#### <a name="resolution"></a>Çözüm

Bu hata, yapısı geçici bir hatadır. Uzantıyı yüklemek için dağıtımı yeniden deneyin.

### <a name="installation-timeout"></a>Senaryo: Yükleme zaman aşımı

MMA uzantının yüklenmesi bir zaman aşımı nedeniyle tamamlanmadı.

#### <a name="issue"></a>Sorun

Aşağıdaki örnek, döndürülen bir hata iletisini aynıdır:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Nedeni

Bu hata oluşur yükleme sırasında yoğun yük altında olan sanal makine.

### <a name="resolution"></a>Çözüm

VM daha düşük bir yük altında olduğunda MMA uzantıyı yükleme girişimi.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
