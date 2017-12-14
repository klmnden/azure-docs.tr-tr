---
title: "Azure Otomasyonu Windows karma Runbook çalışanı | Microsoft Docs"
description: "Bu makale bir Azure Otomasyon karma Runbook yerel veri merkezinde veya Bulut ortamında Windows tabanlı bilgisayarlarda runbook'ların çalışmasına izin veren Worker yükleme hakkında bilgi sağlar."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 5f55d64d08a9c3ed98a47aa4ba00a6b1f698144e
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="how-to-deploy-a-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı dağıtma

Azure bulutta çalışan beri Azure Otomasyonu'nda Runbook'lar diğer Bulut veya şirket içi ortamınız kaynaklarına erişemez.  Azure Otomasyon karma Runbook çalışanı özelliği runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmanıza olanak sağlar. Runbook'ları depolanan ve Azure Otomasyonu'nda yönetilir ve bir veya daha fazla atanmış bilgisayarlara teslim.  

Bu işlevsellik aşağıdaki resimde gösterilmiştir:<br>

![Karma Runbook çalışanı genel bakış](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Karma Runbook çalışanı rolü ve dağıtım konuları teknik bir genel bakış için bkz: [Otomasyon mimarisine genel bakış](automation-offering-get-started.md#automation-architecture-overview).

## <a name="hybrid-runbook-worker-groups"></a>Karma Runbook çalışan grupları

Her karma Runbook çalışanı aracı yüklediğinizde, belirttiğiniz bir karma Runbook çalışanı grubunun bir üyesidir.  Bir gruba bir aracıyı ekleyebilirsiniz, ancak yüksek kullanılabilirlik grubunda birden çok aracı yükleyebilirsiniz.

Bir karma Runbook çalışanını bir runbook'u başlattığınızda, üzerinde çalıştığı grubu belirtin.  Grubun üyelerini isteği hangi çalışan hizmetleri belirleyin.  Belirli bir çalışan belirtemezsiniz.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı yükleme 

Yüklemek ve bir Windows karma Runbook çalışanı yapılandırmak için iki yöntemi vardır.  Önerilen yöntem, tam bir Windows bilgisayarı yapılandırmak için gerekli işlemini otomatikleştirmek için bir Otomasyon runbook'u kullanıyor.  İkinci yöntem, el ile rolünü yüklemek ve yapılandırmak için adım adım bir yordam takip ediyor.  

> [!NOTE]
> İstenen durum yapılandırması (DSC) ile karma Runbook çalışanı rolü destekleyen sunucularınızın yapılandırmasını yönetmek için DSC düğümleri olarak eklemeniz gerekir.  Ekleme hakkında daha fazla bilgi için DSC ile yönetimine görebileceği [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).        
Etkinleştirirseniz, [güncelleştirme yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md), OMS çalışma alanına bağlı herhangi bir Windows bilgisayarı otomatik olarak bu çözümde bulunan runbook'ları desteklemek için bir karma Runbook çalışanı olarak yapılandırılır.  Ancak, bu Otomasyon hesabında zaten tanımlanmış bir karma çalışanı grubu kayıtlı değil.  Bilgisayar grubuna Otomasyon runbook'ları çözüm ve karma Runbook çalışanı grup üyeliği için aynı hesabı kullanarak sürece desteklemek için bir karma Runbook çalışanı Otomasyon hesabınızda eklenebilir.  Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.  

Aşağıdaki bilgileri gözden geçirin ilgili [donanım ve yazılım gereksinimleri](automation-offering-get-started.md#hybrid-runbook-worker) ve [ağınıza hazırlamaya yönelik bilgi](automation-offering-get-started.md#network-planning) bir karma Runbook çalışanı dağıtmaya başlamadan önce.  Bir runbook worker başarıyla dağıttıktan sonra gözden [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.  
 
### <a name="automated-deployment"></a>Otomatik dağıtım

Yükleme ve Windows karma çalışan rolü yapılandırmasını otomatik hale getirmek için aşağıdaki adımları gerçekleştirin.  

1. Karşıdan *yeni OnPremiseHybridWorker.ps1* betikten [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) doğrudan karma Runbook çalışanı rolünü çalıştıran bilgisayara veya başka bir bilgisayardan, ortamınızdaki ve çalışana kopyalayın.  

    *Yeni OnPremiseHybridWorker.ps1* komut dosyası yürütme sırasında aşağıdaki parametreleri gerektirir:

  * *AutomationAccountName* (zorunlu) - Otomasyon hesabınızın adını.  
  * *ResourceGroupName* (zorunlu) - kaynak grubunun adını Otomasyon hesabınızla ilişkilendirilmiş.  
  * *HybridGroupName* (zorunlu) - Bu senaryoyu destekleyen runbook'ları için hedef olarak belirttiğiniz bir karma Runbook çalışanı grubunun adı. 
  *  *Subscriptionıd* (zorunlu) - Automation hesabınız olarak Azure abonelik kimliği.
  *  *WorkspaceName* (isteğe bağlı) - OMS çalışma alanı adı.  Bir OMS çalışma alanı yoksa, komut dosyası oluşturur ve bir yapılandırır.  

     > [!NOTE]
     > Şu anda OMS ile tümleştirme için desteklenen tek Otomasyon bölgeleri şunlardır: - **Avustralya Güneydoğu**, **Doğu ABD 2**, **Güneydoğu Asya**, ve **Batı Avrupa** .  Otomasyon hesabınızı bu bölgeler içinde değilse, komut dosyasını bir OMS çalışma alanı oluşturur ama, bunları birlikte bağlayamazsınız, sizi uyarır.
     >
2. Bilgisayarınızda Başlat **Windows PowerShell** gelen **Başlat** ekran Yönetici modunda.  
3. PowerShell komut satırı kabuğundan indirilir ve bu parametrelerin değerlerini değiştirme yürütmek komut dosyasını içeren klasöre gidin *- AutomationAccountName*, *- ResourceGroupName*, *- HybridGroupName*, *- Subscriptionıd*, ve *- WorkspaceName*.

     > [!NOTE] 
     > Betiği yürüttükten sonra Azure kimlik doğrulaması istenir.  **Gerekir** abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.  
     >  
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. Yüklemeyi kabul istenir **NuGet** ve Azure kimlik bilgilerinizle kimliğinizi isteyip istemediğiniz sorulur.<br><br> ![Yeni OnPremiseHybridWorker betik yürütme işlemi](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. Komut dosyası tamamlandıktan sonra karma çalışan grupları dikey penceresinde yeni Grup ve üye sayısı gösterilir veya varolan bir grubu, üye sayısı artar.  Grup listesinden seçebilirsiniz **karma çalışan grupları** dikey penceresinde ve select **karma çalışanları** döşeme.  Üzerinde **karma çalışanları** dikey penceresi, her listelenen grubunun üyesi bakın.  

### <a name="manual-deployment"></a>El ile dağıtımı 

İlk iki adımı Otomasyon ortamınız için bir kez gerçekleştirin ve sonra her bir çalışan bilgisayar için kalan adımları yineleyin.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Operations Management Suite çalışma alanı oluşturma

Bir Operations Management Suite çalışma alanı zaten yoksa yönergeleri kullanarak bir tane oluşturmak [çalışma alanınızı yönetmek](../log-analytics/log-analytics-manage-access.md). Zaten varsa, varolan bir çalışma alanını kullanabilirsiniz.

#### <a name="2-add-automation-solution-to-operations-management-suite-workspace"></a>2. Otomasyon çözümünü Operations Management Suite çalışma alanına ekleyin

Çözümleri işlevselliği için Operations Management Suite ekleyin.  Otomasyon çözümünü Azure Otomasyon karma Runbook çalışanı desteği dahil olmak üzere için işlevsellik ekler.  Çözüm, çalışma alanına eklediğinizde, otomatik olarak çalışan bileşenleri sonraki adımda yükleyecek aracı bilgisayar için iter.

Bölümündeki yönergeleri izleyin [Çözümleri Galerisi kullanarak bir çözüm eklemek için](../log-analytics/log-analytics-add-solutions.md) eklemek için **Otomasyon** Operations Management Suite çalışma alanınızı çözüme.

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Microsoft Monitoring Agent Yükleme

Microsoft Monitoring Agent, Operations Management Suite bilgisayarlara bağlanır.  Şirket içi bilgisayarınıza aracıyı yüklemek ve alanınıza bağlanın, karma Runbook çalışanı için gerekli bileşenleri otomatik olarak indirir.

Bölümündeki yönergeleri izleyin [günlük analizi bağlanmak Windows bilgisayarlara](../log-analytics/log-analytics-windows-agent.md) şirket içi bilgisayara aracı yüklemek için.  Birden çok Worker ortamınıza eklemek için birden çok bilgisayar için bu işlemi yineleyebilirsiniz.

Aracı başarıyla Operations Management Suite bağlandığında, üzerinde listelenecektir **bağlı kaynakları** Operations Management Suite sekmesinde **ayarları** bölmesi.  Adlı bir klasör varsa, aracı doğru Otomasyon çözümünü indirdiğini doğrulamak **AzureAutomationFiles** C:\Program Files\Microsoft Monitoring Agent\Agent içinde.  Karma Runbook çalışanı sürümünü onaylamak için C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ ve Not gezinebilirsiniz \\ *sürüm* alt klasörü.   

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Runbook ortamını yüklemek ve Azure Otomasyonu bağlanın

Bir aracı Operations Management Suite eklediğinizde, Otomasyon çözümünü iter **HybridRegistration** içeren PowerShell Modülü **Add-HybridRunbookWorker** cmdlet'i.  Bu cmdlet, bilgisayarda runbook ortamını yüklemek ve Azure Automation ile kaydetmek için kullanın.

Yönetici modunda bir PowerShell oturumu açın ve modülü içeri aktarmak için aşağıdaki komutları çalıştırın.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Ardından çalıştırın **Add-HybridRunbookWorker** aşağıdaki sözdizimini kullanarak cmdlet:

    Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>

Bu cmdlet olanağını için gerekli bilgileri almak **anahtarları Yönet** Azure portaldaki dikey pencere.  Seçerek bu dikey penceresini açmak **anahtarları** gelen seçeneği **ayarları** dikey penceresinde, Automation hesabı.

![Karma Runbook çalışanı genel bakış](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** karma Runbook çalışan grubu adıdır. Bu grubun automation hesabında zaten varsa, geçerli bilgisayar kendisine eklenir.  Daha sonra zaten yoksa eklenir.
* **Uç nokta** olan **URL** alanındaki **anahtarları Yönet** dikey.
* **Belirteç** olan **birincil erişim anahtarını** içinde **anahtarları Yönet** dikey.  

Kullanım **-Verbose** anahtarı ile **Add-HybridRunbookWorker** yüklemesi hakkında ayrıntılı bilgi almak için.

#### <a name="5-install-powershell-modules"></a>5. PowerShell modülleri yükleme

Runbook'lar etkinlikler ve Azure Otomasyonu ortamınızda yüklü modülleri tanımlanan cmdlet'leri birini kullanabilirsiniz.  Otomatik olarak bu modülleri el ile yüklemeniz gerekir böylece şirket içi bilgisayarlara yine de dağıtılmaz.  Özel durum için Azure Automation cmdlet'leri erişimi tüm Azure Hizmetleri ve etkinlikleri sağlama varsayılan olarak yüklü Azure modülüdür.

Karma Runbook çalışanı özelliği birincil amacı, yerel kaynakları yönetmek için olduğundan, büyük olasılıkla bu kaynakları destekleyen modüllerin yüklemeniz gerekir.  Başvurabilirsiniz [yükleme modülleri](http://msdn.microsoft.com/library/dd878350.aspx) Windows PowerShell modülleri yükleme hakkında bilgi için.  Yüklü modülleri, böylece karma çalışanı tarafından otomatik olarak içe aktarılır PSModulePath ortam değişkeni tarafından başvurulan bir konumda olması gerekir.  Daha fazla bilgi için bkz: [PSModulePath yükleme yolunun değiştirerek](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx). 

## <a name="troubleshooting"></a>Sorun giderme 

Karma Runbook çalışanı çalışan kaydetmek, runbook işlerini almak ve durum raporu için Otomasyon hesabınızın iletişim kurmak için Microsoft Monitoring Agent bağlıdır. Çalışan kaydı başarısız olursa hata bazı olası nedenleri şunlardır:  

1. Karma çalışanı bir proxy veya güvenlik duvarı arkasında ' dir.  
    Bilgisayarın, bağlantı noktası 443 üzerinde *.azure automation.net giden erişimi olduğunu doğrulayın.  

2. Karma çalışanı üzerinde çalıştığı bilgisayar en düşük donanım daha azı [gereksinimleri](automation-offering-get-started.md#hybrid-runbook-worker).  
    Karma Runbook çalışanı çalıştıran bilgisayarlar, bu özellik barındırmak için atamadan önce en düşük donanım gereksinimlerini karşılamalıdır. Aksi halde, kaynak kullanımı diğer arka plan işlemleri ve yürütme sırasında runbook'lar tarafından neden Çekişme bağlı olarak bilgisayar üzerinde kullanılan haline gelir ve runbook işi gecikmeler veya zaman aşımları neden.
   Karma Runbook çalışanı özelliği yürütmek için atanan bilgisayarın en düşük donanım gereksinimlerini karşıladığını onaylayın.  Aşması durumunda, karma Runbook çalışanı işlemlerinin performansını ile Windows arasında herhangi bir bağıntı belirlemek için CPU ve bellek kullanımını izleyin.  Bellek veya CPU baskısı ise, bu yükseltme veya ek işlemciler ekleme ya da kaynak sorununu giderin ve hatayı gidermek için bellek artırmak için gereken gösteriyor olabilir. Alternatif olarak, iş yükü taleplerini artıştır gerekli belirttiğinizde, ölçek ve en düşük gereksinimleri destekleyebilen farklı işlem kaynak seçin.
    
3. Microsoft İzleme Aracısı hizmeti çalışmıyor.  
    Microsoft İzleme Aracısı Windows hizmeti çalışmıyorsa bu karma Runbook çalışanı Azure Automation ile iletişim kurmasını engeller.  Aracı, PowerShell içinde aşağıdaki komutu girerek çalıştığını doğrulayın: `get-service healthservice`.  Hizmet durdurulursa, hizmeti başlatmak için PowerShell içinde aşağıdaki komutu girin: `start-service healthservice`.  

4. İçinde **uygulama ve Hizmetleri Logs\Operations Yöneticisi** olay günlüğü olay 4502 ve EventMessage içeren gördüğünüz **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**aşağıdaki açıklamasıyla: *hizmeti tarafından sunulan sertifika <wsid>. oms.opinsights.azure.com Microsoft Hizmetleri için kullanılan bir sertifika yetkilisi tarafından değil yayınlandı. TLS/SSL iletişimi karşılar bir proxy çalıştırıyorsanız görmek için ağ yöneticinize başvurun. KB3126513 makalesine ek sorun giderme bilgileri için bağlantı sorunları var.*
    Bu, proxy veya ağ güvenlik duvarı blockking iletişimi için Microsoft Azure neden olabilir.  Bilgisayarın, 443 numaralı bağlantı noktalarına *.azure automation.net giden erişimi olduğunu doğrulayın.

Günlükleri yerel olarak her karma çalışanı C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes konumunda depolanır.  Herhangi bir uyarı veya hata olayları için yazılmış olup olmadığını denetleyebilir **uygulama ve Hizmetleri Logs\Microsoft-SMA\Operations** ve **uygulama ve Hizmetleri Logs\Operations Yöneticisi** olay günlüğü bir bağlantı veya ekleme rolünün bir Azure Otomasyonu veya sorun normal işlemleri gerçekleştirirken etkileyen diğer sorunu gösterir.  

## <a name="next-steps"></a>Sonraki Adımlar

* Gözden geçirme [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.
* Karma Runbook çalışanları kaldırma hakkında daha fazla yönerge için bkz: [Azure Otomasyon karma Runbook çalışanları Kaldır](automation-remove-hrw.md)