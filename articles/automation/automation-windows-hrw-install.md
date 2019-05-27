---
title: Azure Otomasyonu Windows Karma Runbook Çalışanı
description: Bu makale, bir Azure Otomasyonu karma Runbook, runbook'ları yerel veri merkezinde veya Bulut ortamında Windows tabanlı bilgisayarlarda çalıştırmak için kullanabileceğiniz çalışanı yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/21/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cc3307a4f32d77b9b8d259ac846c4db1c1ae4a99
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002524"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı dağıtma

Runbook'ları doğrudan rolünü barındıran bilgisayarda ve ortamınızda bu yerel kaynakları yönetmek için kaynakların karşı çalıştırmak için Azure Otomasyon karma Runbook çalışanı özelliğini kullanabilirsiniz. Runbook'ları tutulur ve yönetilen Azure Otomasyonu'nda ve sonra bir veya daha fazla atanmış bilgisayarlara teslim. Bu makalede, bir Windows makinede karma Runbook çalışanı yükleneceğini açıklar.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı'nı yükleme

Yükleme ve yapılandırma Windows karma Runbook çalışanı için iki yöntem kullanabilirsiniz. Önerilen yöntem, tam bir Windows bilgisayar yapılandırma işlemi otomatik hale getirmek için bir Otomasyon runbook'unu kullanıyor. İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam kullanılmasıdır.

> [!NOTE]
> Karma Runbook çalışanı rolü Desired State Configuration (DSC) ile destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir.

Windows karma Runbook çalışanı için en düşük gereksinimler şunlardır:

* Windows Server 2012 veya üzeri.
* Windows PowerShell 5.1 veya üstünü ([WMF 5.1 indirme](https://www.microsoft.com/download/details.aspx?id=54616)).
* .NET framework 4.6.2 veya üzeri.
* İki çekirdek.
* 4 GB RAM.
* Bağlantı noktası 443 (giden).

Karma Runbook çalışanı için daha fazla ağ gereksinimleri almak için bkz. [ağınıza yapılandırma](automation-hybrid-runbook-worker.md#network-planning).

DSC ile yönetim sunucuları ekleme hakkında daha fazla bilgi için bkz. [makineleri Azure Automation DSC tarafından Yönetim için hazırlama](automation-dsc-onboarding.md).
Etkinleştirirseniz [güncelleştirme yönetimi çözümünü](../operations-management-suite/oms-solution-update-management.md), Azure Log Analytics çalışma alanınıza bağlı herhangi bir Windows bilgisayarda otomatik olarak bu çözümde yer alan runbook'ların desteklenmesi için bir karma Runbook çalışanı olarak yapılandırılır. Ancak, Otomasyon hesabınızda önceden tanımlı karma çalışanı gruplarıyla kayıtlı değil. 

Bilgisayar çözüm ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon gruplarını desteklemek için Otomasyon hesabınızdaki bir karma Runbook çalışanı grubuna eklenebilir. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

Başarılı bir şekilde bir runbook worker dağıtımından sonra gözden [bir karma Runbook çalışanı üzerinde runbook çalıştırma](automation-hrw-run-runbooks.md) şirket içi veri merkezinizde veya diğer bulut ortamı işlemlerini otomatikleştirmek için runbook'larınızı yapılandırma hakkında bilgi edinmek için.

### <a name="automated-deployment"></a>Otomatik dağıtım

Yükleme ve yapılandırma Windows karma çalışan rolünün otomatik hale getirmek için aşağıdaki adımları gerçekleştirin:

1. New-OnPremiseHybridWorker.ps1 betiği indirin [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker) doğrudan karma Runbook çalışanı rolünü çalıştıran bilgisayar veya başka bir bilgisayardan ortamınızdaki. Çalışana betiği kopyalayın.

   New-OnPremiseHybridWorker.ps1 betik yürütme sırasında için aşağıdaki parametreler gereklidir:

   * *AutomationAccountName* (zorunlu): Otomasyon hesabınızın adı.
   * *AAResourceGroupName* (zorunlu): Otomasyon hesabınızla ilişkili kaynak grubunun adı.
   * *OMSResourceGroupName* (isteğe bağlı): Log Analytics çalışma alanı için kaynak grubunun adı. Bu kaynak grubu belirtilmezse *AAResourceGroupName* kullanılır.
   * *HybridGroupName* (mandatory): Bu senaryoyu desteklemek runbook'ları için hedef olarak belirttiğiniz bir karma Runbook çalışanı grubunun adıdır.
   * *Subscriptionıd* (zorunlu): Otomasyon hesabının bulunduğu Azure abonelik kimliği.
   * *WorkspaceName* (isteğe bağlı): Log Analytics çalışma alanı adı. Bir Log Analytics çalışma alanınız yoksa, betiği oluşturur ve bir yapılandırır.

   > [!NOTE]
   > Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
   >
   > Desteklenen eşleme çiftlerine bir listesi için bkz. [Otomasyon hesabının ve Log Analytics çalışma alanı için bölge eşleme](how-to/region-mappings.md).

2. Bilgisayarınızda açma **Windows PowerShell** gelen **Başlat** Yönetici modunda ekran.
3. PowerShell komut satırı kabuğundan, indirdiğiniz komut dosyasını içeren klasöre göz atın. Parametreler için değerleri değiştirme *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- Subscriptionıd*, ve *- WorkspaceName*. Ardından, betiği çalıştırın.

     > [!NOTE]
     > Betiği çalıştırdıktan sonra Azure ile kimlik doğrulaması istenir. *Gerekir* abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesap bilgilerinizle oturum açın.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

4. NuGet yüklemeyi kabul etmeniz istenir ve Azure kimlik bilgilerinizle kimlik doğrulaması istenir.

5. Betik tamamlandıktan sonra **karma çalışan grupları** sayfasında, yeni Grup ve üye sayısı gösterilir. Varolan bir grubu ise, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** sayfasından seçim yapıp **karma çalışanları** Döşe. Üzerinde **karma çalışanları** sayfasında listelenen bir grubun her üyesi görürsünüz.

### <a name="manual-deployment"></a>El ile dağıtım

İlk iki kez Otomasyon ortamınız için adımları ve her bir çalışan bilgisayar için kalan adımları yineleyin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

#### <a name="1-create-a-log-analytics-workspace"></a>1. Log Analytics çalışma alanı oluşturma

Bir Log Analytics çalışma alanı yoksa, adresindeki yönergeleri kullanarak bir tane oluşturmak [çalışma alanınızı yönetme](../azure-monitor/platform/manage-access.md). Zaten varsa var olan bir çalışma alanını kullanabilirsiniz.

#### <a name="2-add-the-automation-solution-to-the-log-analytics-workspace"></a>2. Bir Otomasyon çözümü Log Analytics çalışma alanına ekleme

Otomasyon Azure izleme günlükleri çözümü, Azure Otomasyon karma Runbook çalışanı desteği dahil olmak üzere için işlevsellik ekler. Çözüm çalışma alanınıza eklediğinizde, bu otomatik olarak çalışan bileşenleri sonraki adımda yükler aracıyı bilgisayara iter.

Eklenecek **Otomasyon** Azure İzleyici, çözüm aşağıdaki PowerShell komutunu çalıştırın, çalışma alanına kaydeder.

```powershell-interactive
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true
```

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Microsoft Monitoring Agent'ı yükleme

Microsoft Monitoring Agent, Azure İzleyici günlüklerine bilgisayarlara bağlanır. Şirket içi bilgisayarınıza aracıyı yüklemek ve çalışma alanınıza bağlamak, karma Runbook çalışanı için gerekli olan bileşenleri otomatik olarak indirir.

Şirket içi bilgisayarda aracıyı yüklemek için yönergeleri izleyin. [bağlanmak Windows bilgisayarları Azure İzleyici günlüklerine](../log-analytics/log-analytics-windows-agent.md). Birden fazla çalışana ortamınıza eklemek birden çok bilgisayar için bu işlemi yineleyebilirsiniz.

Aracı başarılı bir şekilde Azure İzleyici günlüklerine bağlandığında, listelenmiş olup **bağlı kaynaklar** log analytics için sekmesinde **ayarları** sayfası. Adlı bir klasör varsa aracıyı düzgün bir Otomasyon çözümü indirdiğini doğrulamak **AzureAutomationFiles** C:\Program Files\Microsoft Monitoring Agent\Agent içinde. Karma Runbook çalışanı sürümünü onaylamak için C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ ve Not göz atabilirsiniz \\ *sürüm* alt.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Runbook ortamını yükleyin ve Azure Otomasyonu bağlanın

Otomasyon çözümünü gönderim Azure İzleyici günlüklerine birini eklediğinizde, bu aracı **HybridRegistration** içeren PowerShell Modülü **Add-HybridRunbookWorker** cmdlet'i. Bu cmdlet'i runbook ortamını bilgisayara yükleyin ve Azure Otomasyonu'na kaydetmek için kullanın.

Yönetici modunda bir PowerShell oturumu açın ve bir modülü içeri aktarmak için aşağıdaki komutları çalıştırın:

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Ardından çalıştırın **Add-HybridRunbookWorker** cmdlet'i aşağıdaki söz dizimini kullanarak:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Bu cmdlet için gereken bilgileri alabilirsiniz **anahtarları Yönet** Azure portalında sayfası. Bu sayfayı seçerek açın **anahtarları** seçeneğini **ayarları** Otomasyon hesabınızdaki sayfası.

!["Anahtarları Yönet" sayfası](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** karma Runbook çalışanı grubunun adıdır. Bu grup Otomasyon hesabında zaten varsa, geçerli bilgisayar kendisine eklenir. Bu grup mevcut değilse eklenir.
* **Uç nokta** olduğu **URL** girişinde **anahtarları Yönet** sayfası.
* **Belirteç** olduğu **birincil erişim anahtarı** girişinde **anahtarları Yönet** sayfası.

Yükleme hakkında ayrıntılı bilgi almak için kullandığınız **-Verbose** anahtarı ile **Add-HybridRunbookWorker**.

#### <a name="5-install-powershell-modules"></a>5. PowerShell modüllerini yükleyin

Runbook'lar, etkinlikler ve Azure Otomasyonu ortamınızda yüklü modülleri'nde tanımlanan cmdlet'leri herhangi birini kullanabilirsiniz. El ile yüklemeniz gerekir, böylece bu modüller, şirket içi bilgisayarlara otomatik olarak dağıtılmaz. Varsayılan olarak yüklenir ve Azure otomasyonu için tüm Azure Hizmetleri ve etkinlikleri cmdlet'leri için erişim sağlayan Azure modülü istisnadır.

Karma Runbook çalışanı özelliğini birincil amacı, yerel kaynakları yönetmek için olduğundan, büyük olasılıkla bu kaynakları destekleyen modül yüklemeniz gerekir. Windows PowerShell modülleri yükleme hakkında daha fazla bilgi için bkz. [modülleri yükleme](/powershell/developer/windows-powershell). 

Yüklü modülleri tarafından başvurulan bir konumda olmalıdır **PSModulePath** ortam değişkeni böylece karma çalışanı bunları otomatik olarak aktarabilirsiniz. Daha fazla bilgi için [PSModulePath yükleme yolunu değiştirmek](/powershell/developer/windows-powershell).

## <a name="next-steps"></a>Sonraki adımlar

* Şirket içi veri merkezinizde veya diğer bulut ortamı işlemlerini otomatikleştirmek için runbook'larınızı yapılandırma konusunda bilgi için bkz: [bir karma Runbook çalışanı üzerinde runbook çalıştırma](automation-hrw-run-runbooks.md).
* Karma Runbook çalışanlarını kaldırma yönergeleri için bkz: [Azure Otomasyon karma Runbook çalışanlarını kaldırma](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
* Sorun giderme, karma Runbook çalışanları öğrenmek için bkz [Windows karma Runbook çalışanları sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#windows)
* Güncelleştirme yönetimi ile ilgili sorunları giderme konusunda ek adımlar için bkz: [güncelleştirme yönetimi: sorun giderme](troubleshoot/update-management.md).
