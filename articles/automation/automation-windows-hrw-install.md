---
title: Azure Otomasyonu Windows Karma Runbook Çalışanı
description: Bu makale bir Azure Otomasyon karma Runbook, runbook'lar yerel veri merkezinde veya Bulut ortamında Windows tabanlı bilgisayarlarda çalıştırmak için kullanabileceğiniz Worker yükleme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e449e6e457c4fa568b5a4de5823014b4dcea82d0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064074"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı dağıtma

Runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmak için Azure Otomasyon karma Runbook çalışanı özelliğini kullanabilirsiniz. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim. Bu makalede, bir Windows makinesinde karma Runbook çalışanı yüklemeyi açıklar.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı yükleme

Yüklemek ve bir Windows karma Runbook çalışanı yapılandırmak için iki yöntem kullanabilirsiniz. Önerilen yöntem, tam bir Windows bilgisayarı yapılandırma işlemini otomatikleştirmek için bir Otomasyon runbook'u kullanıyor. İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam takip ediyor.

> [!NOTE]
> İstenen durum yapılandırması (DSC) ile karma Runbook çalışanı rolünü destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir.

Windows karma Runbook çalışanı için minimum gereksinimleri şunlardır:

* Windows Server 2012 veya üzeri.
* Windows PowerShell 4.0 veya üstünü ([WMF 4.0 indirme](https://www.microsoft.com/download/details.aspx?id=40855)). Windows PowerShell 5.1 öneririz ([WMF 5.1 karşıdan](https://www.microsoft.com/download/details.aspx?id=54616)) daha yüksek güvenilirlik için.
* .NET framework 4.6.2 veya sonraki bir sürümü.
* İki çekirdek.
* 4 GB RAM.
* Bağlantı noktası 443 (giden).

Karma Runbook çalışanı için daha fazla ağ gereksinimlerini almak için bkz: [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning).

DSC ile yönetimi için hazırlanma sunucuları hakkında daha fazla bilgi için bkz: [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).
Etkinleştirirseniz, [güncelleştirme yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md), bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak Azure günlük analizi çalışma alanına bağlı herhangi bir Windows bilgisayar otomatik olarak yapılandırılır. Ancak, bu Otomasyon hesabında zaten tanımlanmış bir karma çalışanı grubu ile kayıtlı değil. 

Bilgisayar grubuna Automation runbook çözümü ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanmakta olduğunuz sürece desteklemek için bir karma Runbook çalışanı Otomasyon hesabınızda eklenebilir. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

Bir runbook worker başarıyla dağıttıktan sonra gözden [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.

### <a name="automated-deployment"></a>Otomatik dağıtım

Yükleme ve Windows karma çalışan rolü yapılandırmasını otomatik hale getirmek için aşağıdaki adımları gerçekleştirin:

1. Yeni OnPremiseHybridWorker.ps1 komut dosyasını karşıdan [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/DisplayScript) doğrudan karma Runbook çalışanı rolünü çalıştıran bilgisayara veya başka bir bilgisayardan, ortamınızdaki. Komut dosyası için çalışan kopyalayın.

   Yeni OnPremiseHybridWorker.ps1 komut dosyası yürütme sırasında aşağıdaki parametreleri gerektirir:

   * *AutomationAccountName* (zorunlu): Otomasyon hesabınızın adını.
   * *AAResourceGroupName* (zorunlu): Otomasyon hesabınızla ilişkili kaynak grubunun adı.
   * *OMSResourceGroupName* (isteğe bağlı): Operations Management Suite çalışma kaynak grubunun adı. Bu kaynak grubu belirtilmezse, *AAResourceGroupName* kullanılır.
   * *HybridGroupName* (zorunlu): Bu senaryoyu desteklemek runbook'ları için hedef olarak belirttiğiniz bir karma Runbook çalışanı grubunun adı.
   * *Subscriptionıd* (zorunlu): Automation hesabınız olarak Azure abonelik kimliği.
   * *WorkspaceName* (isteğe bağlı): günlük analizi çalışma alanı adı. Günlük analizi çalışma alanı yoksa, komut dosyası oluşturur ve bir yapılandırır.

     > [!NOTE]
     > Günlük analizi ile tümleştirme için desteklenen tek Otomasyon bölgeler şu anda, uygun **Avustralya Güneydoğu**, **Doğu ABD 2**, **Güneydoğu Asya**, ve  **Batı Avrupa**. Otomasyon hesabınızı bu bölgeler içinde değilse, komut dosyası günlük analizi çalışma alanı oluşturur, ancak bunları birlikte Bağla olamaz, sizi uyarır.

1. Bilgisayarınızda açmak **Windows PowerShell** gelen **Başlat** ekran Yönetici modunda.
1. PowerShell komut satırı kabuğundan indirdiğiniz komut dosyasını içeren klasöre gidin. Parametre değerlerini değiştirmek *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- Subscriptionıd*, ve *- WorkspaceName*. Daha sonra komut dosyasını çalıştırın.

     > [!NOTE]
     > Betiği çalıştırdıktan sonra Azure kimlik doğrulaması istenir. *Gerekir* abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

1. NuGet yüklemeyi kabul istenir ve Azure kimlik bilgilerinizle kimliğinizi istenir.

1. Komut dosyası tamamlandıktan sonra **karma çalışan grupları** sayfası, yeni Grup ve üye sayısını gösterir. Varolan bir grubu ise, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** sayfasından seçim yapıp **karma çalışanları** döşeme. Üzerinde **karma çalışanları** sayfasında, listelenen grubun her üyesi bakın.

### <a name="manual-deployment"></a>El ile dağıtımı

İlk iki adımı Otomasyon ortamınız için bir kez gerçekleştirin ve sonra her bir çalışan bilgisayar için kalan adımları yineleyin.

#### <a name="1-create-a-log-analytics-workspace"></a>1. Günlük analizi çalışma alanı oluşturma

Günlük analizi çalışma alanı zaten sahip değilseniz, yönergeleri kullanarak bir tane oluşturmak [çalışma alanınızı yönetmek](../log-analytics/log-analytics-manage-access.md). Zaten varsa, varolan bir çalışma alanını kullanabilirsiniz.

#### <a name="2-add-the-automation-solution-to-the-log-analytics-workspace"></a>2. Otomasyon çözümünü günlük analizi çalışma alanıma Ekle

Çözümler, Log Analytics’e işlevler ekler. Otomasyon çözümünü Azure Otomasyon karma Runbook çalışanı desteği dahil olmak üzere için işlevsellik ekler. Çözüm, çalışma alanına eklediğinizde, otomatik olarak çalışan bileşenleri sonraki adımda yükleyecek aracı bilgisayar için iter.

Eklemek için **Otomasyon** günlük analizi çalışma alanınız, çözüme kısmındaki yönergeleri izleyin [Çözümleri Galerisi kullanarak bir çözüm eklemek için](../log-analytics/log-analytics-add-solutions.md).

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Microsoft Monitoring Agent Yükleme

Microsoft Monitoring Agent için günlük analizi bilgisayarlara bağlanır. Şirket içi bilgisayarınıza aracıyı yüklemek ve alanınıza bağlanın, karma Runbook çalışanı için gerekli olan bileşenleri otomatik olarak yükler.

Şirket içi bilgisayara aracı yüklemek için yönergeleri izleyin [günlük analizi bağlanmak Windows bilgisayarlara](../log-analytics/log-analytics-windows-agent.md). Birden çok Worker ortamınıza eklemek için birden çok bilgisayar için bu işlemi yineleyebilirsiniz.

Aracı için günlük analizi başarıyla bağlandı, listelendiğini **bağlı kaynakları** günlük analizi sekmesinde **ayarları** sayfası. Adlı bir klasör varsa, aracı doğru Otomasyon çözümünü indirdiğini doğrulamak **AzureAutomationFiles** C:\Program Files\Microsoft Monitoring Agent\Agent içinde. Karma Runbook çalışanı sürümünü onaylamak için C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ ve Not göz atabilirsiniz \\ *sürüm* alt klasörü.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Runbook ortamını yüklemek ve Azure Otomasyonu bağlanın

Bir aracı için günlük analizi eklediğinizde, Otomasyon çözümünü iter **HybridRegistration** içeren PowerShell Modülü **Add-HybridRunbookWorker** cmdlet'i. Bu cmdlet, bilgisayarda runbook ortamını yüklemek ve Azure Automation ile kaydetmek için kullanın.

Yönetici modunda bir PowerShell oturumu açın ve modülü içeri aktarmak için aşağıdaki komutları çalıştırın:

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module HybridRegistration.psd1
```

Ardından çalıştırın **Add-HybridRunbookWorker** aşağıdaki sözdizimini kullanarak:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Bu cmdlet olanağını için gerekli bilgileri almak **anahtarları Yönet** Azure portalında sayfası. Bu sayfayı seçerek açın **anahtarları** gelen seçeneği **ayarları** sayfa Otomasyon hesabınızda.

!["Anahtarları Yönet" sayfası](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** karma Runbook çalışanı grubunun adıdır. Bu grubun Automation hesabında zaten varsa, geçerli bilgisayar kendisine eklenir. Bu grup yoksa, eklenir.
* **Uç nokta** olan **URL** girişinde **anahtarları Yönet** sayfası.
* **Belirteç** olan **birincil erişim ANAHTARINI** girişinde **anahtarları Yönet** sayfası.

Yükleme hakkında ayrıntılı bilgi almak için kullandığınız **-Verbose** anahtarı ile **Add-HybridRunbookWorker**.

#### <a name="5-install-powershell-modules"></a>5. PowerShell modülleri yükleme

Runbook'lar etkinlikler ve Azure Otomasyonu ortamınızda yüklü modülleri tanımlanan cmdlet'leri birini kullanabilirsiniz. El ile yüklemeniz gerekir böylece bu modülleri şirket içi bilgisayarlara otomatik olarak dağıtılmaz. Özel durumu varsayılan olarak yüklenir ve Azure Automation ile ilgili tüm Azure Hizmetleri ve etkinlikleri cmdlet'leri için erişim sağlayan Azure modülüdür.

Karma Runbook çalışanı özelliği birincil amacı, yerel kaynakları yönetmek için olduğundan, büyük olasılıkla bu kaynakları destekleyen modüllerin yüklemeniz gerekir. Windows PowerShell modülleri yükleme hakkında daha fazla bilgi için bkz: [yükleme modülleri](http://msdn.microsoft.com/library/dd878350.aspx). 

Yüklü modülleri tarafından başvurulan bir konumda olmalı **PSModulePath** ortam değişkeni böylece karma çalışanı otomatik olarak onları içeri aktarabilirsiniz. Daha fazla bilgi için bkz: [PSModulePath yükleme yolunun değiştirerek](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx).

## <a name="troubleshoot"></a>Sorun giderme

Karma Runbook çalışanları sorunlarını giderme konusunda bilgi almak için bkz: [Windows karma Runbook çalışanları sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#windows)

Güncelleştirme yönetimi ile ilgili sorunları giderme konusunda ek adımlar için bkz: [güncelleştirme yönetimi: sorun giderme](troubleshoot/update-management.md).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md).
* Karma Runbook çalışanları kaldırma hakkında daha fazla yönerge için bkz: [Azure Otomasyon karma Runbook çalışanları kaldırmak](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).