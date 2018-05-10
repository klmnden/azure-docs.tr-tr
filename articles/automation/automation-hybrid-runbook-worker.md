---
title: Azure Otomasyon karma Runbook çalışanları
description: Bu makalede, yükleme ve yerel veri merkezinde veya Bulut sağlayıcısı makinelerde runbook'lar çalıştırmanıza olanak sağlayan Azure Automation'ın bir özellik olan karma Runbook çalışanı kullanma hakkında bilgiler sağlar.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/04/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 2cf4eecbc45e6d842b5a548c8f25ac49aa8fb216
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Veri merkezi veya karma Runbook çalışanı ile bulut kaynakları otomatikleştirme

Azure bulutta çalışan beri Azure Otomasyonu'nda Runbook'lar diğer Bulut veya şirket içi ortamınız kaynaklarına erişemez. Azure Otomasyon karma Runbook çalışanı özelliği runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmanıza olanak sağlar. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim.

Bu işlevsellik aşağıdaki resimde gösterilmiştir:

![Karma Runbook çalışanı genel bakış](media/automation-hybrid-runbook-worker/automation.png)

Karma Runbook çalışanı rolü ve dağıtım konuları teknik bir genel bakış için bkz: [Otomasyon mimarisine genel bakış](automation-offering-get-started.md#automation-architecture-overview).

## <a name="hybrid-runbook-worker-groups"></a>Karma Runbook çalışan grupları

Her karma Runbook çalışanı aracı yüklediğinizde, belirttiğiniz bir karma Runbook çalışanı grubunun bir üyesidir. Bir gruba bir aracıyı ekleyebilirsiniz, ancak yüksek kullanılabilirlik grubunda birden çok aracı yükleyebilirsiniz.

Bir karma Runbook çalışanını bir runbook'u başlattığınızda, üzerinde çalıştığı grubu belirtin. Grubun üyelerini isteği hangi çalışan hizmetleri belirleyin. Belirli bir çalışan belirtemezsiniz.

## <a name="relationship-to-service-management-automation"></a>Hizmet Yönetimi Otomasyonu ilişkisi

[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) , yerel veri merkezinde Azure Automation tarafından desteklenen aynı runbook'ları çalıştırmanızı sağlar. Windows Azure Pack SMA yönetimi için bir grafik arabirim içerdiğinden SMA Windows Azure paketi ile birlikte dağıtılır. Azure Otomasyonu, SMA runbook'ları ve SMA yapılandırmayı içeren veritabanı API ve Runbook çalışanları runbook işleri yürütmek için barındırmak için web sunucuları içeren yerel bir yükleme gerektirir. Azure Otomasyonu bulutta bu hizmetleri sağlar ve yalnızca yerel ortamınızdaki karma Runbook çalışanları korumak gerektirir.

Varolan bir SMA kullanıcı varsa, Azure Automation'ın herhangi bir değişiklik yapmadan karma Runbook çalışanı ile kullanılan, kendi kimlik doğrulaması kaynaklarına gerçekleştirdikleri varsayılarak açıklandığı üzere runbook'larınızın taşıyabilirsiniz [runbook'lar bir karma Runbook üzerinde çalışır Çalışan](automation-hrw-run-runbooks.md). SMA runbook'ları, bu kimlik doğrulaması runbook'ları için sağlayabilir worker sunucusunda hizmet hesabı bağlamında çalışır.

Azure Otomasyon karma Runbook çalışanı ya da hizmet yönetimi Otomasyonu ile gereksinimleriniz için daha uygun olup olmadığını belirlemek için aşağıdaki ölçütleri kullanabilirsiniz.

* SMA grafiksel Yönetim arabirimine gerekiyorsa, Windows Azure Pack bağlı temel bileşenlerinin yerel bir yükleme gerektirir. Daha fazla yerel kaynaklar yalnızca yerel runbook çalışanları üzerinde yüklü bir aracı gereken Azure Otomasyonu daha yüksek bakım maliyetleri gereklidir. Aracılar daha fazla bakım maliyetlerini azaltmak Azure tarafından yönetilir.
* Azure Otomasyonu runbook bulutta depolar ve bunları şirket içi karma Runbook çalışanları için sunar. Güvenlik ilkeniz Bu davranış izin vermez, SMA kullanmanız gerekir.
* System Center ile SMA bulunur; ve bu nedenle, System Center 2012 R2 lisansı gerektirir. Azure Otomasyonu, bir katmanlı abonelik modeline dayanır.
* Azure Otomasyonu Gelişmiş Özellikler SMA'da kullanılamaz grafik runbook'lar gibi.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı yükleme

Yüklemek ve bir Windows karma Runbook çalışanı yapılandırmak için iki yöntemi vardır. Önerilen yöntem, tam bir Windows bilgisayarı yapılandırmak için gerekli işlemini otomatikleştirmek için bir Otomasyon runbook'u kullanıyor. İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam takip ediyor.

> [!NOTE]
> İstenen durum yapılandırması (DSC) ile karma Runbook çalışanı rolü destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir. Ekleme hakkında daha fazla bilgi için DSC ile yönetimine görebileceği [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).
>
>Etkinleştirirseniz, [güncelleştirme yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md), günlük analizi çalışma alanına bağlı herhangi bir Windows bilgisayarı otomatik olarak bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak yapılandırılır. Ancak, bu Otomasyon hesabında zaten tanımlanmış bir karma çalışanı grubu kayıtlı değil. Bilgisayar grubuna Otomasyon runbook'ları çözüm ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanarak sürece desteklemek için bir karma Runbook çalışanı Otomasyon hesabınızda eklenebilir. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

Aşağıdaki bilgileri gözden geçirin ilgili [donanım ve yazılım gereksinimleri](automation-offering-get-started.md#hybrid-runbook-worker) ve [ağınıza hazırlamaya yönelik bilgi](automation-offering-get-started.md#network-planning) bir karma Runbook çalışanı dağıtmaya başlamadan önce. Bir runbook worker başarıyla dağıttıktan sonra gözden [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.

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

2. Bilgisayarınızda Başlat **Windows PowerShell** gelen **Başlat** ekran Yönetici modunda.
3. PowerShell komut satırı kabuğundan indirilir ve bu parametrelerin değerlerini değiştirme yürütmek komut dosyasını içeren klasöre gidin *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- Subscriptionıd*, ve *- WorkspaceName*.

     > [!NOTE]
     > Betiği yürüttükten sonra Azure kimlik doğrulaması istenir. **Gerekir** abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

4. Yüklemeyi kabul istenir **NuGet** ve Azure kimlik bilgilerinizle kimliğinizi isteyip istemediğiniz sorulur.

5. Komut dosyası tamamlandıktan sonra karma çalışan grupları dikey penceresinde yeni Grup ve üye sayısı gösterilir veya varolan bir grubu, üye sayısı artar. Grup listesinden seçebilirsiniz **karma çalışan grupları** dikey penceresinde ve select **karma çalışanları** döşeme. Üzerinde **karma çalışanları** dikey penceresi, her listelenen grubunun üyesi bakın.

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

Adlı bir klasör varsa, aracı doğru Otomasyon çözümünü indirdiğini doğrulamak **AzureAutomationFiles** C:\Program Files\Microsoft Monitoring Agent\Agent içinde. Karma Runbook çalışanı sürümünü onaylamak için C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ ve Not gezinebilirsiniz \\ *sürüm* alt klasörü.  

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Runbook ortamını yüklemek ve Azure Otomasyonu bağlanın

Bir aracı için günlük analizi eklediğinizde, Otomasyon çözümünü iter **HybridRegistration** içeren PowerShell Modülü **Add-HybridRunbookWorker** cmdlet'i. Bu cmdlet, bilgisayarda runbook ortamını yüklemek ve Azure Automation ile kaydetmek için kullanın.

Modülü içeri aktarmak için Yönetici modunda bir PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module HybridRegistration.psd1
```

Ardından çalıştırın **Add-HybridRunbookWorker** aşağıdaki sözdizimini kullanarak cmdlet:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Bu cmdlet için tıklayarak gerekli bilgileri almak **anahtarları** altında seçeneği **hesap ayarlarını** Otomasyon hesabınızda.

* **GroupName** karma Runbook çalışan grubu adıdır. Bu grubun automation hesabında zaten varsa, geçerli bilgisayar kendisine eklenir. Daha sonra zaten yoksa eklenir.
* **Uç nokta** olan **URL** alanındaki **anahtarları** sayfası.
* **Belirteç** olan **birincil erişim anahtarını** içinde **anahtarları** sayfası.

Kullanım **-Verbose** anahtarı ile **Add-HybridRunbookWorker** yüklemesi hakkında ayrıntılı bilgi almak için.

#### <a name="5-install-powershell-modules"></a>5. PowerShell modülleri yükleme

Runbook'lar etkinlikler ve Azure Otomasyonu ortamınızda yüklü modülleri tanımlanan cmdlet'leri birini kullanabilirsiniz. Otomatik olarak bu modülleri el ile yüklemeniz gerekir böylece şirket içi bilgisayarlara yine de dağıtılmaz. Özel durum için Azure Automation cmdlet'leri erişimi tüm Azure Hizmetleri ve etkinlikleri sağlama varsayılan olarak yüklü Azure modülüdür.

Karma Runbook çalışanı özelliği birincil amacı, yerel kaynakları yönetmek için olduğundan, büyük olasılıkla bu kaynakları destekleyen modüllerin yüklemeniz gerekir. Başvurabilirsiniz [yükleme modülleri](http://msdn.microsoft.com/library/dd878350.aspx) Windows PowerShell modülleri yükleme hakkında bilgi için. Yüklü modülleri, böylece karma çalışanı tarafından otomatik olarak içe aktarılır PSModulePath ortam değişkeni tarafından başvurulan bir konumda olması gerekir. Daha fazla bilgi için bkz: [PSModulePath yükleme yolunun değiştirerek](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx).

## <a name="removing-hybrid-runbook-worker"></a>Karma Runbook çalışanı kaldırma

Bir veya daha fazla karma Runbook çalışanları bir gruptan kaldırdığınızda veya grup gereksinimlerinize bağlı olarak kaldırabilirsiniz. Bir karma Runbook çalışanı bir şirket içi bilgisayardan kaldırmak için aşağıdaki adımları gerçekleştirin:

1. Azure Portal'da, Automation hesabınızı gidin.
2. Gelen **ayarları** dikey penceresinde, select **anahtarları** ve alan değerlerini Not **URL** ve **birincil erişim anahtarını**. Bu bilgiler sonraki adımda gerekir.

### <a name="windows"></a>Windows

Yönetici modunda bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın. Kullanım **-Verbose** geçiş kaldırma işleminin ayrıntılı günlüğü için.

```powershell
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>
```

### <a name="linux"></a>Linux

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

Eski makineler karma çalışanı gruptan kaldırmak için isteğe bağlı kullanın `machineName` parametresi.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

> [!NOTE]
> Bu Microsoft İzleme Aracısı bilgisayardan, yalnızca işlevsellik ve karma Runbook çalışanı rolü yapılandırmasını kaldırmaz.

## <a name="remove-hybrid-worker-groups"></a>Karma çalışan grupları kaldırın

Bir grubu kaldırmak için önce bir karma Runbook çalışanı daha önce gösterilen yordamı kullanarak grubunun bir üyesi olan her bilgisayarda kaldırmanız gerekir ve ardından grubunu kaldırmak için aşağıdaki adımları gerçekleştirin.

1. Azure Portal'da Automation hesabını açın.
1. Altında **işlem Otomasyonu**seçin **karma çalışan grupları**. Silmek istediğiniz grubu seçin. Belirli grup seçtikten sonra **karma çalışanı grubu** özellikleri dikey penceresi görüntülenir.

   ![Karma Runbook çalışan grubu dikey penceresi](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Özellikleri dikey penceresinde seçili grubun tıklatın **silmek**. Bu eylemi onaylamanızı isteyen bir ileti görüntülenir seçin **Evet** devam etmek istediğinizden eminseniz.

   ![Grubu Sil onay iletişim kutusu](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Bu işlemin tamamlanması birkaç saniye alabilir ve ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

Karma Runbook çalışanı çalışan kaydetmek, runbook işlerini almak ve durum raporu için Otomasyon hesabınızın iletişim kurmak için Microsoft Monitoring Agent bağlıdır. Çalışan kaydı başarısız olursa hata bazı olası nedenleri şunlardır:

1. Karma çalışanı bir proxy veya güvenlik duvarı arkasında ' dir.

   Bilgisayarın, bağlantı noktası 443 üzerinde *.azure automation.net giden erişimi olduğunu doğrulayın.

2. Karma çalışanı üzerinde çalıştığı bilgisayar en düşük donanım daha azı [gereksinimleri](automation-offering-get-started.md#hybrid-runbook-worker).

   Karma Runbook çalışanı çalıştıran bilgisayarlar, bu özellik barındırmak için atamadan önce en düşük donanım gereksinimlerini karşılamalıdır. Aksi halde, kaynak kullanımı diğer arka plan işlemleri ve yürütme sırasında runbook'lar tarafından neden Çekişme bağlı olarak bilgisayar üzerinde kullanılan olur ve bu runbook işi gecikmeler veya zaman aşımları neden.

   Karma Runbook çalışanı özelliği yürütmek için atanan bilgisayarın en düşük donanım gereksinimlerini karşıladığını onaylayın. Aşması durumunda, karma Runbook çalışanı işlemlerinin performansını ile Windows arasında herhangi bir bağıntı belirlemek için CPU ve bellek kullanımını izleyin. Bellek veya CPU baskısı ise, bu yükseltme veya ek işlemciler ekleme ya da kaynak sorununu giderin ve hatayı gidermek için bellek artırmak için gereken gösteriyor olabilir. Alternatif olarak, iş yükü taleplerini artıştır gerekli belirttiğinizde, ölçek ve en düşük gereksinimleri destekleyebilen farklı işlem kaynak seçin.

3. Microsoft İzleme Aracısı hizmeti çalışmıyor.

   Microsoft İzleme Aracısı Windows hizmeti çalışmıyorsa bu karma Runbook çalışanı Azure Automation ile iletişim kurmasını engeller. Aracı, PowerShell içinde aşağıdaki komutu girerek çalıştığını doğrulayın: `get-service healthservice`. Hizmet durdurulursa, hizmeti başlatmak için PowerShell içinde aşağıdaki komutu girin: `start-service healthservice`.

4. İçinde **uygulama ve Hizmetleri Logs\Operations Yöneticisi** olay günlüğü olay 4502 ve EventMessage içeren gördüğünüz **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**aşağıdaki açıklamasıyla: *hizmeti tarafından sunulan sertifika \<wsid\>. oms.opinsights.azure.com Microsoft Hizmetleri için kullanılan bir sertifika yetkilisi tarafından değil yayınlandı. TLS/SSL iletişimi karşılar bir proxy çalıştırıyorsanız görmek için ağ yöneticinize başvurun. KB3126513 makalesine ek sorun giderme bilgileri için bağlantı sorunları var.*
    Bu Microsoft Azure iletişimini engelleyen proxy veya ağ güvenlik duvarını neden olabilir. Bilgisayarın, 443 numaralı bağlantı noktalarına *.azure automation.net giden erişimi olduğunu doğrulayın.

Günlükleri yerel olarak her karma çalışanı C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes konumunda depolanır. Herhangi bir uyarı veya hata olayları için yazılmış olup olmadığını denetleyebilir **uygulama ve Hizmetleri Logs\Microsoft-SMA\Operations** ve **uygulama ve Hizmetleri Logs\Operations Yöneticisi** olay günlüğü bir bağlantı veya ekleme rolünün bir Azure Otomasyonu veya sorun normal işlemleri gerçekleştirirken etkileyen diğer sorunu gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.
