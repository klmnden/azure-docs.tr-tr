---
title: Azure Otomasyonu Windows Karma Runbook Çalışanı
description: Bu makale bir Azure Otomasyon karma Runbook yerel veri merkezinde veya Bulut ortamında Windows tabanlı bilgisayarlarda runbook'ların çalışmasına izin veren Worker yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: e63e4afb5c60f193d46e30ab884d72912a6a5054
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="how-to-deploy-a-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı dağıtma

Azure Otomasyon karma Runbook çalışanı özelliği runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmanıza olanak sağlar. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim. Bu karma Runbook çalışanı bir Windows makinesinde yükleme decribes makalesi.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı yükleme

Yüklemek ve bir Windows karma Runbook çalışanı yapılandırmak için iki yöntemi vardır. Önerilen yöntem, tam bir Windows bilgisayarı yapılandırmak için gerekli işlemini otomatikleştirmek için bir Otomasyon runbook'u kullanıyor. İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam takip ediyor.

> [!NOTE]
> İstenen durum yapılandırması (DSC) ile karma Runbook çalışanı rolü destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir.

Windows karma Runbook çalışanı için minimum gereksinimleri verilmiştir.

* Windows Server 2012 veya üzeri.
* Windows PowerShell 4.0 veya üstü gereklidir ([WMF 4.0 indirme](https://www.microsoft.com/download/details.aspx?id=40855)). Windows PowerShell 5.1 ([karşıdan WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)) daha fazla güvenilirlik için önerilir.
* .NET framework 4.6.2 veya sonraki bir sürümü.
* En az iki çekirdek.
* En az 4 GB RAM.
* Bağlantı noktası 443 (giden)

Karma Runbook çalışanı ek ağ gereksinimlerini görmek için bkz: [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning).

Ekleme hakkında daha fazla bilgi için DSC ile yönetimine görebileceği [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).
Etkinleştirirseniz, [güncelleştirme yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md), günlük analizi çalışma alanına bağlı herhangi bir Windows bilgisayarı otomatik olarak bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak yapılandırılır. Ancak, bu Otomasyon hesabında zaten tanımlanmış bir karma çalışanı grubu kayıtlı değil. Bilgisayar grubuna Otomasyon runbook'ları çözüm ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanarak sürece desteklemek için bir karma Runbook çalışanı Otomasyon hesabınızda eklenebilir. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

Bir runbook worker başarıyla dağıttıktan sonra gözden [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.

### <a name="automated-deployment"></a>Otomatik dağıtım

Yükleme ve Windows karma çalışan rolü yapılandırmasını otomatik hale getirmek için aşağıdaki adımları gerçekleştirin.

1. Karşıdan *yeni OnPremiseHybridWorker.ps1* betikten [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/DisplayScript) doğrudan karma Runbook çalışanı rolünü çalıştıran bilgisayara veya başka bir bilgisayardan, ortamınızdaki ve çalışana kopyalayın.

   *Yeni OnPremiseHybridWorker.ps1* komut dosyası yürütme sırasında aşağıdaki parametreleri gerektirir:

   * *AutomationAccountName* (zorunlu) - Otomasyon hesabınızın adını.
   * *AAResourceGroupName* (zorunlu) - kaynak grubunun adı, Automation hesabınız ile ilişkili
   * *OMSResourceGroupName* (isteğe bağlı) - OMS çalışma kaynak grubunun adı. Belirtilmezse, AAResourceGroupName kullanılır.
   * *HybridGroupName* (zorunlu) - Bu senaryoyu destekleyen runbook'ları için hedef olarak belirttiğiniz bir karma Runbook çalışanı grubunun adı.
   * *Subscriptionıd* (zorunlu) - Automation hesabınız olarak Azure abonelik kimliği.
   * *WorkspaceName* (isteğe bağlı) - günlük analizi çalışma alanı adı. Günlük analizi çalışma alanı yoksa, komut dosyası oluşturur ve bir yapılandırır.

     > [!NOTE]
     > Şu anda günlük analizi ile tümleştirme için desteklenen tek Otomasyon bölgeleri şunlardır: - **Avustralya Güneydoğu**, **Doğu ABD 2**, **Güneydoğu Asya**, ve  **Batı Avrupa**. Otomasyon hesabınızı bu bölgeler içinde değilse, komut dosyası günlük analizi çalışma alanı oluşturur ama, bunları birlikte bağlayamazsınız, sizi uyarır.

1. Bilgisayarınızda Başlat **Windows PowerShell** gelen **Başlat** ekran Yönetici modunda.
1. PowerShell komut satırı kabuğundan indirilir ve bu parametrelerin değerlerini değiştirme yürütmek komut dosyasını içeren klasöre gidin *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- Subscriptionıd*, ve *- WorkspaceName*.

     > [!NOTE]
     > Betiği yürüttükten sonra Azure kimlik doğrulaması istenir. **Gerekir** abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

1. Yüklemeyi kabul istenir **NuGet** ve Azure kimlik bilgilerinizle kimliğinizi isteyip istemediğiniz sorulur.

1. Komut dosyası tamamlandıktan sonra yeni Grup ve üye sayısı karma çalışan grupları sayfası gösterilir veya varolan bir grubu, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** sayfasından seçim yapıp **karma çalışanları** döşeme. Üzerinde **karma çalışanları** sayfasında, listelenen grubun her üyesi bakın.

### <a name="manual-deployment"></a>El ile dağıtımı

İlk iki adımı Otomasyon ortamınız için bir kez gerçekleştirin ve sonra her bir çalışan bilgisayar için kalan adımları yineleyin.

#### <a name="1-create-log-analytics-workspace"></a>1. Log Analytics çalışma alanı oluşturma

Günlük analizi çalışma alanı zaten yoksa yönergeleri kullanarak bir tane oluşturmak [çalışma alanınızı yönetmek](../log-analytics/log-analytics-manage-access.md). Zaten varsa, varolan bir çalışma alanını kullanabilirsiniz.

#### <a name="2-add-automation-solution-to-log-analytics-workspace"></a>2. Otomasyon çözümünü günlük analizi çalışma alanıma Ekle

Çözümler, Log Analytics’e işlevler ekler. Otomasyon çözümünü Azure Otomasyon karma Runbook çalışanı desteği dahil olmak üzere için işlevsellik ekler. Çözüm, çalışma alanına eklediğinizde, otomatik olarak çalışan bileşenleri sonraki adımda yükleyecek aracı bilgisayar için iter.

Bölümündeki yönergeleri izleyin [Çözümleri Galerisi kullanarak bir çözüm eklemek için](../log-analytics/log-analytics-add-solutions.md) eklemek için **Otomasyon** günlük analizi çalışma alanınız çözüme.

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Microsoft Monitoring Agent Yükleme

Microsoft Monitoring Agent için günlük analizi bilgisayarlara bağlanır. Şirket içi bilgisayarınıza aracıyı yüklemek ve alanınıza bağlanın, karma Runbook çalışanı için gerekli bileşenleri otomatik olarak indirir.

Bölümündeki yönergeleri izleyin [günlük analizi bağlanmak Windows bilgisayarlara](../log-analytics/log-analytics-windows-agent.md) şirket içi bilgisayara aracı yüklemek için. Birden çok Worker ortamınıza eklemek için birden çok bilgisayar için bu işlemi yineleyebilirsiniz.

Aracı için günlük analizi başarıyla bağlandı, listelendiğini **bağlı kaynakları** günlük analizi sekmesinde **ayarları** sayfası. Adlı bir klasör varsa, aracı doğru Otomasyon çözümünü indirdiğini doğrulamak **AzureAutomationFiles** C:\Program Files\Microsoft Monitoring Agent\Agent içinde. Karma Runbook çalışanı sürümünü onaylamak için C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ ve Not gezinebilirsiniz \\ *sürüm* alt klasörü.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Runbook ortamını yüklemek ve Azure Otomasyonu bağlanın

Bir aracı için günlük analizi eklediğinizde, Otomasyon çözümünü iter **HybridRegistration** içeren PowerShell Modülü **Add-HybridRunbookWorker** cmdlet'i. Bu cmdlet, bilgisayarda runbook ortamını yüklemek ve Azure Automation ile kaydetmek için kullanın.

Yönetici modunda bir PowerShell oturumu açın ve modülü içeri aktarmak için aşağıdaki komutları çalıştırın.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module HybridRegistration.psd1
```

Ardından çalıştırın **Add-HybridRunbookWorker** aşağıdaki sözdizimini kullanarak cmdlet:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Bu cmdlet olanağını için gerekli bilgileri almak **anahtarları Yönet** Azure portalında sayfası. Bu sayfayı seçerek açın **anahtarları** gelen seçeneği **ayarları** sayfa Otomasyon hesabınızda.

![Karma Runbook çalışanı genel bakış](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** karma Runbook çalışan grubu adıdır. Bu grubun automation hesabında zaten varsa, geçerli bilgisayar kendisine eklenir. Daha sonra zaten yoksa eklenir.
* **Uç nokta** olan **URL** alanındaki **anahtarları Yönet** sayfası.
* **Belirteç** olan **birincil erişim anahtarını** içinde **anahtarları Yönet** sayfası.

Kullanım **-Verbose** anahtarı ile **Add-HybridRunbookWorker** yüklemesi hakkında ayrıntılı bilgi almak için.

#### <a name="5-install-powershell-modules"></a>5. PowerShell modülleri yükleme

Runbook'lar etkinlikler ve Azure Otomasyonu ortamınızda yüklü modülleri tanımlanan cmdlet'leri birini kullanabilirsiniz. Otomatik olarak bu modülleri el ile yüklemeniz gerekir böylece şirket içi bilgisayarlara yine de dağıtılmaz. Özel durum için Azure Automation cmdlet'leri erişimi tüm Azure Hizmetleri ve etkinlikleri sağlama varsayılan olarak yüklü Azure modülüdür.

Karma Runbook çalışanı özelliği birincil amacı, yerel kaynakları yönetmek için olduğundan, büyük olasılıkla bu kaynakları destekleyen modüllerin yüklemeniz gerekir. Başvurabilirsiniz [yükleme modülleri](http://msdn.microsoft.com/library/dd878350.aspx) Windows PowerShell modülleri yükleme hakkında bilgi için. Yüklü modülleri, böylece karma çalışanı tarafından otomatik olarak içe aktarılır PSModulePath ortam değişkeni tarafından başvurulan bir konumda olması gerekir. Daha fazla bilgi için bkz: [PSModulePath yükleme yolunun değiştirerek](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx).

## <a name="next-steps"></a>Sonraki Adımlar

* Gözden geçirme [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.
* Karma Runbook çalışanları kaldırma hakkında daha fazla yönerge için bkz: [Azure Otomasyon karma Runbook çalışanları Kaldır](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker)