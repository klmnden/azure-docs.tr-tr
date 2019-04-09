---
title: Azure Otomasyonu ile Vm'leri durdurmayla ve sorun giderme
description: Bu makale başlatma ve durdurma Vm'leri Azure automation'da sorun giderme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 03bad12b7fcba5a247e05884aa0eb0493163a5c4
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59009793"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Vm'leri başlatma/durdurma sırasında saat çözümü kapatmak sorun giderme

## <a name="deployment-failure"></a>Senaryo: Düzgün bir şekilde dağıtmak Başlat/Durdur VM çözümü başarısız

### <a name="issue"></a>Sorun

Dağıtırken [saat çözümü Kapat Vm'leri başlatma/durdurma sırasında](../automation-solution-vm-management.md), aşağıdaki hatalardan birini alıyorsunuz:

```error
Account already exists in another resourcegroup in a subscription. ResourceGroupName: [MyResourceGroup].
```

```error
Resource 'StartStop_VM_Notification' was disallowed by policy. Policy identifiers: '[{\\\"policyAssignment\\\":{\\\"name\\\":\\\"[MyPolicyName]”.
```

```error
The subscription is not registered to use namespace 'Microsoft.OperationsManagement'.
```

```error
The subscription is not registered to use namespace 'Microsoft.Insights'.
```

```error
The scope '/subscriptions/000000000000-0000-0000-0000-00000000/resourcegroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView' cannot perform write operation because following scope(s) are locked: '/subscriptions/000000000000-0000-0000-0000-00000000/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView'. Please remove the lock and try again
```

### <a name="cause"></a>Nedeni

Dağıtımları, aşağıdaki nedenlerden biri nedeniyle başlatılamayabilir:

1. Seçili bölgesinde aynı ada sahip bir Otomasyon hesabı zaten var.
2. Vm'leri başlatma/durdurma çözümün dağıtımı izin vermeyen bir yerde bir ilkedir.
3. `Microsoft.OperationsManagement`, `Microsoft.Insights`, Veya `Microsoft.Automation` kaynak türleri kayıtlı değil.
4. Log Analytics çalışma alanınızda bir kilit üzerinde sahiptir.

### <a name="resolution"></a>Çözüm

Sorun veya aramak için yerler olası çözümler için aşağıdaki listeyi gözden geçirin:

1. Otomasyon hesapları farklı kaynak gruplarında bulunan olsalar bile bir Azure bölgesi içinde benzersiz olması gerekir. Hedef bölgede var olan Otomasyon hesaplarınız denetleyin.
2. Mevcut bir ilkenin dağıtılması Başlat/Durdur VM çözümü için gerekli olan bir kaynak engeller. Azure portalında, ilke atamaları gidin ve bu kaynak dağıtımına izin vermeyen bir ilke ataması gerekip gerekmediğini kontrol edin. Bunun hakkında daha fazla bilgi edinmek için [RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md).
3. VM Başlat/Durdur çözümü dağıtmak için aboneliğiniz için aşağıdaki Azure kaynağı ad kayıtlı olması gerekir:
    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Bkz, [çözmek için kaynak Sağlayıcısı kaydı hataları](../../azure-resource-manager/resource-manager-register-provider-errors.md) sağlayıcıları kaydedilirken hata hakkında daha fazla bilgi edinmek için.
4. Kilit varsa, Log Analytics çalışma alanınızda, Azure portalındaki çalışma alanınıza gidin ve kaynak kilitli kaldırın.

## <a name="all-vms-fail-to-startstop"></a>Senaryo: Tüm sanal makineleri başlatma/durdurma başarısız

### <a name="issue"></a>Sorun

VM başlatma/durdurma çözüm yapılandırdınız ancak başlangıç değil veya yapılandırılmış tüm sanal makineleri durdurmak.

### <a name="cause"></a>Nedeni

Bu hataya aşağıdaki nedenlerden biri tarafından kaynaklanabilir:

1. Bir zamanlama doğru yapılandırılmamış
2. Farklı Çalıştır hesabının düzgün yapılandırılmamış
3. Bir runbook hatalarla karşılaşırsanız çalıştırma
4. Sanal makineler dışlandı

### <a name="resolution"></a>Çözüm

Sorun veya aramak için yerler olası çözümler için aşağıdaki listeyi gözden geçirin:

* VM Başlat/Durdur çözümü bir zamanlama düzgün şekilde yapılandırdınız denetleyin. Bir zamanlamayı yapılandırma konusunda bilgi için bkz: [zamanlamaları](../automation-schedules.md) makalesi.

* Denetleme [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) tüm hataları aramak için. Portalında, Otomasyon hesabınıza gidin ve seçin **işleri** altında **süreç otomasyonu**. Gelen **işleri** işleri aşağıdaki runbook'lar arasından sayfasında bakın:

  * AutoStop_CreateAlert_Child
  * AutoStop_CreateAlert_Parent
  * AutoStop_Disable
  * AutoStop_VM_Child
  * ScheduledStartStop_Base_Classic
  * ScheduledStartStop_Child_Classic
  * ScheduledStartStop_Child
  * ScheduledStartStop_Parent
  * SequencedStartStop_Parent

* Doğrulayın, [RunAs hesabı](../manage-runas-account.md) başlatmak veya durdurmak için çalıştığınız VM'ler için uygun izinlere sahip. Kaynak izinlerini denetlemek nasıl öğrenmek için bkz: [hızlı başlangıç: Azure portalını kullanarak bir kullanıcıya atanmış olan rolleri görüntülemek](../../role-based-access-control/check-access.md). Farklı Çalıştır hesabı tarafından kullanılan hizmet sorumlusunun uygulama kimliğini sağlamanız gerekir. Azure portalında Otomasyon hesabınıza giderek bu değeri alabilirsiniz seçerek **farklı çalıştır hesapları** altında **hesap ayarları** ve uygun farklı çalıştır hesabı'nı tıklatın.

* Vm'leri başlatılan veya varsa bunlar açıkça dışlanan durduruldu. Vm'leri kümesinde, hariç tutulan **External_ExcludeVMNames** çözüm için Otomasyon hesabında değişken dağıtılır. Aşağıdaki örnekte, bu değeri PowerShell ile nasıl Sorgulayabileceğiniz gösterilmektedir.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="some-vms-fail-to-startstop"></a>Senaryo: Bazı kendi sanal makinelerimin başlatmak veya durdurmak başarısız

### <a name="issue"></a>Sorun

VM başlatma/durdurma çözüm yapılandırdınız ancak Başlat değil veya yapılandırılmış Vm'lerden bazıları durdurun.

### <a name="cause"></a>Nedeni

Bu hataya aşağıdaki nedenlerden biri tarafından kaynaklanabilir:

1. Dizisi senaryo kullanıyorsanız, bir etiketi eksik veya yanlış olabilir
2. VM hariç tutulması
3. Farklı Çalıştır hesabını VM üzerinde yeterli izinlere sahip olmayabilir
4. VM başlatılıyor veya durduruluyor durduran bir sorun olabilir.

### <a name="resolution"></a>Çözüm

Sorun veya aramak için yerler olası çözümler için aşağıdaki listeyi gözden geçirin:

* Kullanırken [dizisi senaryo](../automation-solution-vm-management.md#scenario-2-startstop-vms-in-sequence-by-using-tags) Başlat/Durdur VM sırasında saat çözümü kapatmak, başlatmak veya durdurmak istediğiniz her bir VM uygun etiketi olduğundan emin olun gerekir. Başlatmak istediğiniz Vm'lere sahip olduğundan emin olun `sequencestart` etiketi ve durdurmak istediğiniz Vm'leri `sequencestop` etiketi. Her iki etiketleri artı bir tamsayı değeri gerektirir. Etiketleri ve değerleri ile tüm Vm'leri bulmak için aşağıdaki örneğe benzer bir sorgu kullanabilirsiniz.

  ```powershell-interactive
  Get-AzureRmResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* Vm'leri başlatılan veya varsa bunlar açıkça dışlanan durduruldu. Vm'leri kümesinde, hariç tutulan **External_ExcludeVMNames** çözüm için Otomasyon hesabında değişken dağıtılır. Aşağıdaki örnekte, bu değeri PowerShell ile nasıl Sorgulayabileceğiniz gösterilmektedir.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* Başlatmak ve durdurmak için Otomasyon hesabı için RunAs hesabı VM'ye uygun izinlere sahip olmalıdır. Kaynak izinlerini denetlemek nasıl öğrenmek için bkz: [hızlı başlangıç: Azure portalını kullanarak bir kullanıcıya atanmış olan rolleri görüntülemek](../../role-based-access-control/check-access.md). Farklı Çalıştır hesabı tarafından kullanılan hizmet sorumlusunun uygulama kimliğini sağlamanız gerekir. Azure portalında Otomasyon hesabınıza giderek bu değeri alabilirsiniz seçerek **farklı çalıştır hesapları** altında **hesap ayarları** ve uygun farklı çalıştır hesabı'nı tıklatın.

* VM serbest bırakılıyor veya başlatma bir sorun vardır, bu davranışı VM üzerinde bir sorun neden olabilir. Bazı örnekler veya olası sorunları olan bir hizmet kapanması için çalışırken bir güncelleştirme uygulanan askıda kalır ve daha fazlası). Sanal makine kaynağınıza gidin ve kontrol **etkinlik günlüklerini** günlüklerde hataları olup olmadığını görmek için. Olay günlüklerinde hata olup olmadığını görmek için VM'de oturum açmak deneyebilir. Sanal makinenizin sorun giderme hakkında daha fazla bilgi için bkz [sorun giderme Azure sanal makineler](../../virtual-machines/troubleshooting/index.md)

* Denetleme [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) tüm hataları aramak için. Portalında, Otomasyon hesabınıza gidin ve seçin **işleri** altında **süreç otomasyonu**.

## <a name="custom-runbook"></a>Senaryo: Başlatma veya durdurma Vm'lerimi özel runbook'um başarısız

### <a name="issue"></a>Sorun

Özel bir runbook yazılan veya bir PowerShell Galerisi'nden indirilir ve düzgün şekilde çalışmıyor.

### <a name="cause"></a>Nedeni

Hatanın nedenini birçok şeylerden biri olabilir. Azure portal ve seçin, Otomasyon hesabınıza gidin **işleri** altında **süreç otomasyonu**. Gelen **işleri** sayfasında, tüm iş başarısızlık görüntülemek için runbook işlerini bakın.

### <a name="resolution"></a>Çözüm

Kullanılması önerilir [saat çözümü Kapat Vm'leri başlatma/durdurma sırasında](../automation-solution-vm-management.md) Vm'leri Azure automation'da durdurmak ve başlatmak. Bu çözüm, Microsoft tarafından yazılmıştır. Özel runbook'ları, Microsoft tarafından desteklenmez. Ziyaret ederek, özel bir runbook için bir çözüm bulabilirsiniz [runbook sorunlarını giderme](runbooks.md) makalesi. Bu makalede, genel rehberlik ve tüm türlerdeki runbook'lar için sorun giderme sağlar. Denetleme [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) tüm hataları aramak için. Portalında, Otomasyon hesabınıza gidin ve seçin **işleri** altında **süreç otomasyonu**.

## <a name="dont-start-stop-in-sequence"></a>Senaryo: Vm'leri başlatma yok veya doğru sırayla durdurma

### <a name="issue"></a>Sorun

Çözümde yapılandırdığınız VM'lerin Başlat veya Durdur doğru sırayla kullanmayın.

### <a name="cause"></a>Nedeni

Bu, yanlış Vm'lerde etiketleyerek kaynaklanır.

### <a name="resolution"></a>Çözüm

Çözümü doğru şekilde yapılandırıldığından emin olmak için aşağıdaki adımları uygulayın.

1. Tüm VM'lerin başlatılması veya durdurulmasına yaşamalarını bir `sequencestart` veya `sequencestop` durumunuza bağlı olarak bir etiket. Bu etiketler, pozitif bir tamsayı değeri olarak gerekir. Vm'leri bu değere göre artan sırada işlenir.
2. Sanal makinelerin başlatılması kaynak gruplarını emin olun veya durdurulmuş bulunan `External_Start_ResourceGroupNames` veya `External_Stop_ResourceGroupNames` durumunuza bağlı olarak, değişkenleri.
3. Yaptığınız değişiklikleri yürüterek test `SequencedStartStop_Parent` WHATIF parametresi ile runbook değişikliklerinizi önizlemesini görüntülemek için True olarak ayarlayın.

Sıralı sanal makineleri durdurmak ve başlatmak çözümü kullanma hakkında daha ayrıntılı ve ek yönergeler için bkz [dizideki Vm'leri başlatma/durdurma](../automation-solution-vm-management.md#scenario-2-startstop-vms-in-sequence-by-using-tags).

## <a name="403"></a>Senaryo: VM başlatma/durdurma işi 403 Yasak durumu ile başarısız oluyor 

### <a name="issue"></a>Sorun

İle başarısız olan işler bulduğunuz bir `403 forbidden` Vm'leri başlatma/durdurma sırasında saat çözüm runbook'ları kapatmak için hata.

### <a name="cause"></a>Nedeni

Bir yanlış yapılandırılmış veya süresi dolan farklı çalıştır hesabı tarafından bu soruna neden olabilir. Ayrıca, Otomasyon hesapları farklı çalıştır hesabı tarafından sanal makine kaynaklarının yetersiz izinler nedeniyle olabilir.

### <a name="resolution"></a>Çözüm

Farklı Çalıştır hesabı düzgün yapılandırıldığını denetlemek için Otomasyon hesabınızı seçin ve portal azure'da gidin **farklı çalıştır hesapları** altında **hesap ayarları**. Burada hesapları çalıştırma durumunu görürsünüz, farklı çalıştır hesabı yanlış yapılandırılmış veya süresi dolduğunda bu gösterilir.

Farklı Çalıştır hesabı ise [yapılandırılmış](../manage-runas-account.md#misconfiguration), farklı çalıştır hesabınız silip.

Sertifika farklı çalıştır hesabınız için süresi dolmuşsa, listelenen adımları takip [otomatik olarak imzalanan sertifika yenileme](../manage-runas-account.md#cert-renewal) sertifikayı yenilemek için.

Eksik izinler tarafından soruna neden olabilir. Kaynak izinlerini denetlemek nasıl öğrenmek için bkz: [hızlı başlangıç: Azure portalını kullanarak bir kullanıcıya atanmış olan rolleri görüntülemek](../../role-based-access-control/check-access.md). Farklı Çalıştır hesabı tarafından kullanılan hizmet sorumlusunun uygulama kimliğini sağlamanız gerekir. Azure portalında Otomasyon hesabınıza giderek bu değeri alabilirsiniz seçerek **farklı çalıştır hesapları** altında **hesap ayarları** ve uygun farklı çalıştır hesabı'nı tıklatın.

## <a name="other"></a>Senaryo: Yukarıda sorunum listede yok

### <a name="issue"></a>Sorun

Bir sorun veya beklenmeyen bir sonuç Vm'leri başlatma/durdurma sırasında bu sayfada listelenmeyen yoğun olmayan saatlerde çözüm kullanırken karşılaşırsınız.

### <a name="cause"></a>Nedeni

Çözüm eski ve güncel bir sürümünü kullanarak birden çok kez hataları neden olabilir.

### <a name="resolution"></a>Çözüm

Birçok hataları gidermek için bunu kaldırıp çözümü güncelleştirecek önerilir. Çözüm güncelleştirme hakkında bilgi edinmek için bkz: [Vm'leri başlatma/durdurma sırasında saat çözümü Kapat güncelleştirme](../automation-solution-vm-management.md#update-the-solution). Ayrıca, denetleyebilirsiniz [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) tüm hataları aramak için. Portalında, Otomasyon hesabınıza gidin ve seçin **işleri** altında **süreç otomasyonu**.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
