---
title: Azure Otomasyonu durumu yapılandırma Chocolatey ile sürekli dağıtım
description: Paket Yöneticisi Azure Otomasyon durum yapılandırması, DSC ve Chocolatey kullanarak sürekli dağıtım DevOps.  Örnek tam JSON Resource Manager şablonu ve PowerShell kaynağı.
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b53cb65ec99637dadb16ed9d97c495571be956d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61073921"
---
# <a name="usage-example-continuous-deployment-to-virtual-machines-using-automation-state-configuration-and-chocolatey"></a>Kullanım örneği: Sanal makinelere Otomasyon durum yapılandırması ve Chocolatey kullanarak sürekli dağıtım

DevOps dünyada çeşitli noktaları ile sürekli tümleştirme işlem hattında yardımcı olmak üzere birçok araç vardır. Azure Otomasyonu durumu, DevOps takımları dağıtabileceklerinizle seçenekleri Hoş Geldiniz yeni eklenen yapılandırmadır. Bu makalede, bir Windows bilgisayar için yedekleme sürekli dağıtım (CD) ayarı gösterilmektedir. (Bir web sitesi, örneğin gibi) rolü ve ek roller için buradan gerektiği kadar Windows bilgisayarları dahil etmesini teknik kolayca genişletebilirsiniz.

![Iaas Vm'leri için sürekli dağıtım](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>Yüksek düzeyde

Buraya giderek oldukça bit vardır, ancak iki ana süreçlerine Neyse bölünebilir:

- Kod yazma, test, oluşturma ve yükleme paketleri için birincil ve ikincil sürüme sisteminin yayımlama.
- Oluşturma ve yükleyecek ve paketleri kod yürütmesine Vm'leri yönetme.  

Her iki çekirdek işlem yerinde olduktan sonra otomatik olarak yeni sürümlerle oluşturulan ve dağıtılan herhangi belirli VM'de çalışan paketi güncelleştirmeye kısa bir adımdır.

## <a name="component-overview"></a>Bileşenine genel bakış

Yöneticileri gibi paket [apt-get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) Linux dünya, ancak vermeyecek kadar Windows dünyanın oldukça iyi bilinen sorunlardır.
[Chocolatey](https://chocolatey.org/) böyle bir şey ve Scott Hanselman'ın [blog](https://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) konuda harika bir giriş olduğundan. Buna koysalar Chocolatey paketleri paketleri merkezi bir depodan komut satırını kullanarak bir Windows sistemine yüklemek sağlar. Oluşturabilir ve kendi deponuzda yönetmek ve Chocolatey belirlediğiniz depoları arasında herhangi bir sayı paketlerini yükleyebilirsiniz.

Desired State Configuration ' nı (DSC) ([genel bakış](/powershell/dsc/overview)) bir makine için istediğiniz yapılandırmayı bildirmek izin veren bir PowerShell aracıdır. Örneğin, söylediğiniz, "yüklü Chocolatey istiyorum, yüklü IIS istiyorum, bağlantı noktası 80 açık istiyorum, yüklü sürüm 1.0.0 sitemde istiyorum." DSC yerel Configuration Manager (LCM) yapılandırma uygular. Bir DSC çekme sunucusuna bir depo makineleriniz için yapılandırmaları içerir. Her bir makinede LCM düzenli aralıklarla yapılandırmasıyla depolanan yapılandırma eşleşip eşleşmediğini teslim eder. Bu durum raporu veya makine depolanan yapılandırma ile aynı doğrultuda içine geri getirmek çalışır. Bir makine ya da değiştirilmiş yapılandırma ile aynı doğrultuda gelmesini makineler kümesi neden çekme sunucusunda depolanan yapılandırmasını düzenleyebilirsiniz.

Azure Otomasyonu, runbook'ları, düğümler, kimlik bilgileri, kaynaklar ve zamanlamaları ve genel değişkenler gibi varlıklar'ı kullanarak çeşitli görevleri otomatik hale getirmenizi sağlayan Microsoft Azure yönetilen bir hizmettir.
Azure Otomasyonu durum yapılandırması bu Otomasyon PowerShell DSC araçları dahil etme yeteneği genişletir. Harika bir işte [genel bakış](automation-dsc-overview.md).

DSC kaynağı, ağ, Active Directory ya da SQL Server'ı yönetme gibi belirli özellikleri olan kod için kullanılan bir modüldür. Chocolatey DSC kaynak erişim (diğerlerinin arasında) bir NuGet sunucusu, paketleri indirin, paketleri yüklemek ve benzeri nasıl bilir. Diğer birçok DSC kaynaklarında vardır [PowerShell Galerisi](https://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title).
Bu modüller (sizin tarafınızdan) çekme Azure Otomasyonu durumu yapılandırma sunucusu yüklü yapılandırmalarınızı tarafından kullanılabilmesi için.

Resource Manager şablonları, altyapınızı - ağlar, alt ağlar, ağ güvenliği gibi şeyleri oluşturmanın bildirim temelli bir yöntemini sağlar ve yönlendirme, yük Dengeleyiciler, NIC'ler, VM'ler ve benzeri. İşte bir [makale](../azure-resource-manager/resource-manager-deployment-model.md) , Resource Manager dağıtım modelini (bildirim) (zorunlu) Azure Hizmet Yönetimi (ASM veya Klasik) dağıtım modeli ile karşılaştırır ve çekirdek kaynak sağlayıcıları açıklar, bilgi işlem, depolama ve ağ.

Temel özelliklerinden biri Resource Manager şablonu olarak sağlandığından, VM'de oturum VM uzantısı yükleme yeteneğidir. VM uzantısı, özel bir betik çalıştırarak, virüsten koruma yazılımı veya bir DSC yapılandırma betiği çalıştırma gibi belirli özelliklere sahiptir. VM uzantıları diğer birçok tür vardır.

## <a name="quick-trip-around-the-diagram"></a>Diyagram etrafında hızlı seyahat

Üstten başlayarak, kodunuzu yazma derleme ve test edin, ardından bir yükleme paketi oluşturun.
Chocolatey yükleme paketleri, MSI, MSU, ZIP gibi çeşitli türlerde başa çıkabilir. Ve Chocolateys yerel özellikleri oldukça, en fazla değilseniz, gerçek yükleme yapmak için PowerShell tam gücünü sahipsiniz. Paket bazı yere erişilebilir – bir paket deposu yerleştirin. Bu kullanım örneği, bir Azure blob depolama hesabındaki ortak klasör kullanır, ancak herhangi bir yerde olabilir. Chocolatey NuGet sunucularını ve birkaç başka Yönetim Paketi meta verileri için yerel olarak çalışır. [Bu makalede](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) seçenekler açıklanmaktadır. Bu kullanım örneği NuGet kullanır. Bir Nuspec, takımınızın paketleriniz hakkındaki meta verilerdir. Nuspec'ın "NuPkg ait derlenmiş" ve bir NuGet sunucusunda depolanan. Yapılandırmanıza bir paket adına göre istekleri ve NuGet sunucusuna başvuran, Chocolatey DSC kaynağı (artık VM) paket Dallarınızla ve sizin için yükler. Ayrıca, belirli bir paket sürümü isteyebilir.

Resmin sol alt kısmında bir Azure Resource Manager şablonu yoktur. Bu kullanım örneğinde, VM uzantısı, VM'nin Azure Otomasyonu durumu yapılandırma çekme sunucusuna (diğer bir deyişle, bir çekme sunucusu) bir düğüm olarak kaydeder. Yapılandırma çekme sunucusunda depolanır.
Aslında, iki kez depolandığını: düz metin olarak ve (için olanlar gibi şeyler hakkında bilmeniz.) bir MOF dosyası olarak derlenmiş sonra bir kez Portalda, MOF bir "düğüm" (aksine sadece "yapılandırma") bir yapılandırmadır. Düğüm yapılandırması bilir, böylece bir düğümle ilişkili yapıdır. Düğüm yapılandırması düğüme atamak nasıl ayrıntılarını aşağıda göstermek.

Büyük olasılıkla zaten en üst veya en fazla bit yaptığınızı. Nuspec oluşturmak, derlemek ve bir NuGet Server'da depolamak küçük bir şeydir. Ve VM'ler zaten yönettiğiniz. Sonraki adım için sürekli dağıtım Sürüyor (kez) çekme sunucusu ayarlama, düğümlerinizi kendisiyle (sonra), kaydetme ve oluşturma ve yapılandırma vardır (başlangıçta) depolama gerektirir. Ardından paketleri yükseltilmiş ve depoya dağıtılan düğüm yapılandırması ve yapılandırma (gerektiği şekilde yineleyin) çekme sunucusu yenileyin.

Bir Resource Manager şablonu ile başlatıyorsanız değil, bu da normaldir. Çekme sunucusu ve tüm rest ile Vm'leri kaydetme yardımcı olmak için tasarlanan bir PowerShell cmdlet'leri vardır. Daha fazla ayrıntı için bu makaleye bakın: [Makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md).

## <a name="step-1-setting-up-the-pull-server-and-automation-account"></a>1\. adım: Çekme sunucusu ve Otomasyon hesabı ayarlama

Kimliği doğrulanmış bir konumunda (`Connect-AzureRmAccount`) PowerShell komut satırı: (çekme sunucusu ayarlama sırada birkaç dakika sürebilir)

```azurepowershell-interactive
New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT
```

Otomasyon hesabınızı (konum olarak da bilinir) aşağıdaki bölgelerden birini koyabilirsiniz: Doğu ABD 2, Orta Güney ABD, ABD Devleti Virginia, Batı Avrupa, Güneydoğu Asya, Japonya Doğu, Orta Hindistan ve Avustralya Güneydoğu, Kanada Orta, Kuzey Avrupa.

## <a name="step-2-vm-extension-tweaks-to-the-resource-manager-template"></a>2\. adım: VM uzantısı tweaks için Resource Manager şablonu

Bu konuda sağlanan Ayrıntılar (PowerShell DSC VM uzantısı kullanarak) VM kayıt için [Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver).
Bu adım, yeni VM durumu yapılandırması düğümleri listesinden çekme sunucusu ile kaydeder. Düğüm yapılandırması düğüme uygulanacak belirten bu kaydı bir parçası. Bu düğüm yapılandırması burada bu ilk kez gerçekleştirilir 4. adım olan Tamam, bu nedenle çekme Sunucusu'nda henüz mevcut gerekmez. Ancak burada 2. adımda düğümün adını ve yapılandırmanın adını verdiniz gerekir. Bu kullanım örneğinde düğümüdür 'isvbox' ve 'ISVBoxConfig' yapılandırmadır. Düğüm yapılandırması adı (DeploymentTemplate.json belirtilmesi için) 'ISVBoxConfig.isvbox' olması.

## <a name="step-3-adding-required-dsc-resources-to-the-pull-server"></a>3\. adım: Çekme sunucusu gerekli DSC kaynakları ekleme

PowerShell Galerisi, Azure Otomasyonu hesabınızı DSC kaynaklarını yüklemek için işaretlenmiş.
"Dağıtmak için Azure Otomasyonu" düğmesine tıklayın ve istediğiniz kaynağa gidin.

![PowerShell Galerisi örneği](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Başka bir yöntem yeni Azure Portalı'na eklenen yeni modülleri çekme veya mevcut modüllerini sağlar. Otomasyon hesabı kaynak varlıklar kutucuğa tıklayın ve son olarak modülleri kutucuk. Galeriye Gözat simgesi, galerideki modüllerin listesini görmek için aşağı detaylarına ve sonuç olarak, Otomasyon hesabına aktarın sağlar. Bu, modüllerinizi zaman zaman güncel tutmak için harika bir yoludur. Ayrıca, hiçbir şey eşitlenmemiş alır emin olmak için diğer modüllerle bağımlılıkları içeri aktarma özelliğini denetler.

Veya el ile bir yaklaşım yoktur. Bir Windows bilgisayar için bir PowerShell tümleştirme modülü klasör yapısını, Azure Otomasyonu tarafından beklenen klasör yapısı biraz farklıdır.
Bu, bulunmanıza biraz ince ayarlar yapma gerektirir. Ancak sabit değil ve (gelecek yükseltmek istemediğiniz sürece.), kaynak başına yalnızca bir kez gerçekleştirilir Bu makalede PowerShell tümleştirme modülleri yazma ile ilgili daha fazla bilgi için bkz: [İçin Azure Automation tümleştirme modülleri yazma](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

- İş istasyonunuzda aşağıdaki gibi ihtiyacınız modülünü yükleyin:
  - Yükleme [Windows Management Framework v5](https://aka.ms/wmf5latest) (Windows 10 için gerekli değildir)
  - `Install-Module –Name MODULE-NAME`    < — modülü PowerShell Galerisi'ndeki Dallarınızla
- Modül klasörüne kopyalayın `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` geçici bir klasöre
- Örnekler ve belgeler ana klasöründen silin.
- ZIP dosyasını aynı klasöre adlandırma ana klasör zip 
- ZIP dosyası, bir Azure depolama hesabındaki blob depolama gibi erişilebilir bir HTTP konuma yerleştirin.
- Bu PowerShell çalıştırın:

  ```powershell
  New-AzureRmAutomationModule `
    -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Name MODULE-NAME –ContentLink 'https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip'
  ```

Dahil edilen örnek cChoco ve xNetworking için aşağıdaki adımları gerçekleştirir. Bkz: [notları](#notes) cChoco için özel işleme için.

## <a name="step-4-adding-the-node-configuration-to-the-pull-server"></a>4\. Adım: Düğüm yapılandırması çekme sunucusu ekleme

Özel derleme ve çekme sunucusu yapılandırmanızı alma ilk kez hakkında bir şey yoktur. Tüm sonraki içeri aktarma/derler aynı yapılandırmaya sahip tam olarak aynı görünür. Paketiniz güncelleştirin ve üretime dışına gerek her zaman bu adım bunu yapılandırma dosyasında doğru olduğundan olduktan sonra – paketinizi yeni sürümü dahil olmak üzere. PowerShell ve yapılandırma dosyası aşağıda verilmiştir:

ISVBoxConfig.ps1:

```powershell
Configuration ISVBoxConfig
{
    Import-DscResource -ModuleName cChoco
    Import-DscResource -ModuleName xNetworking

    Node 'isvbox' {

        cChocoInstaller installChoco
        {
            InstallDir = 'C:\choco'
        }

        WindowsFeature installIIS
        {
            Ensure = 'Present'
            Name   = 'Web-Server'
        }

        xFirewall WebFirewallRule
        {
            Direction    = 'Inbound'
            Name         = 'Web-Server-TCP-In'
            DisplayName  = 'Web Server (TCP-In)'
            Description  = 'IIS allow incoming web site traffic.'
            Enabled       = 'True'
            Action       = 'Allow'
            Protocol     = 'TCP'
            LocalPort    = '80'
            Ensure       = 'Present'
        }

        cChocoPackageInstaller trivialWeb
        {
            Name      = 'trivialweb'
            Version   = '1.0.0'
            Source    = 'MY-NUGET-V2-SERVER-ADDRESS'
            DependsOn = '[cChocoInstaller]installChoco','[WindowsFeature]installIIS'
        }
    }
}
```

Yeni-ConfigurationScript.ps1:

```powershell
Import-AzureRmAutomationDscConfiguration `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 `
    -Published –Force

$jobData = Start-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -ConfigurationName ISVBoxConfig

$compilationJobId = $jobData.Id

Get-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Id $compilationJobId
```

Bu adımların sonucu yeni bir düğüm yapılandırması "çekme sunucusunda yerleştirilen ISVBoxConfig.isvbox" adlı. Düğüm yapılandırması adı "configurationName.nodeName" oluşturulmuştur.

## <a name="step-5-creating-and-maintaining-package-metadata"></a>5\. Adım: Oluşturma ve paket meta verileri koruma

Paket Deposu yerleştirdiğiniz her paket için tanımladığı bir nuspec gerekir.
Bu nuspec derlenmiş ve NuGet sunucunuzun depolanır. Bu işlem açıklanan [burada](https://docs.nuget.org/create/creating-and-publishing-a-package). Bir NuGet sunucusu olarak MyGet.org kullanabilirsiniz. Bu hizmet satmak, ancak ücretsiz SKU Başlatıcı sahip. NuGet.org kendi NuGet sunucusu, özel paketler için yükleme yönergeleri bulabilirsiniz.

## <a name="step-6-tying-it-all-together"></a>6\. Adım: Tümünü bir araya bağlanma

Sürüm QA geçirir ve onaylanır her zaman dağıtım için paketin oluşturulduğu, nuspec ve güncelleştirilen ve NuGet sunucuya dağıtılan nupkg. Ayrıca, yeni sürüm numarasıyla kabul etmek için ' % s'yapılandırması (yukarıdaki adım 4) güncelleştirilmesi gerekir. Çekme sunucusuna gönderilen ve derlenmiş gerekir.
Bu noktadan itibaren güncelleştirme çekme ve yüklemek için bu yapılandırmasına göre değişir Vm'leri aittir. Bu güncelleştirmelerin her biri basit - yalnızca bir çizgi veya iki PowerShell. Azure DevOps söz konusu olduğunda, bunlardan bazıları bir yapı içinde birbirine zincirlenebilir derleme görevleri kapsüllenir. Bu [makale](https://www.visualstudio.com/docs/alm-devops-feature-index#continuous-delivery) daha fazla ayrıntı sağlar. Bu [GitHub deposunu](https://github.com/Microsoft/vso-agent-tasks) çeşitli kullanılabilir yapı görevleri ayrıntıları.

## <a name="notes"></a>Notlar

Bu kullanım örneği, Azure Galerisi'nden genel bir Windows Server 2012 R2 görüntüden bir VM ile başlar. Depolanmış bir görüntüden başlatabilir ve DSC yapılandırması ile buradan ince ayar.
Ancak, görüntüye hazır yapılandırma değiştirme DSC kullanarak yapılandırmasını dinamik olarak güncelleştirilmesi daha çok daha zordur.

Bu tekniği Vm'leriniz ile kullanmak için Resource Manager şablonu ve sanal makine uzantısını kullanmak zorunda değilsiniz. Ve Vm'lerinizi CD Yönetimi altında olması için Azure üzerinde olması gerekmez. Gerekli olan tek şey Chocolatey yüklenmesi ve çekme sunucusu olduğu bilmesi LCM VM'de yapılandırılır.

Elbette, üretimde olan bir VM üzerindeki bir paket güncelleştirmesi, güncelleştirme yüklenirken bu VM'ye rotasyon dışında olması gerekir. Bunu nasıl yapacağınız büyük farklılık gösterir. Örneğin, bir Azure yük dengeleyici arkasındaki bir VM ile bir özel araştırma ekleyebilirsiniz. Sanal makine güncelleştirilirken bir 400 dönüş araştırma uç noktası vardır. İnce bu değişikliğe neden gerekli Güncelleştirme tamamlandıktan sonra geri 200 döndürmek için geçiş yapmak için ince gibi içinde bir yapılandırma olabilir.

Bu kullanım örneği için tam kaynak konusu [bu Visual Studio projesi](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) GitHub üzerinde.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Automation DSC genel bakış](automation-dsc-overview.md)
* [Azure Otomasyonu DSC cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.automation#automation)
* [Makineleri Azure Automation DSC tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure Otomasyon durum yapılandırması](automation-dsc-overview.md)
- Başlamak için bkz: [Azure Otomasyon durum yapılandırması ile çalışmaya başlama](automation-dsc-getting-started.md)
- Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [yapılandırmaları Azure Automation durumu yapılandırma derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz: [Azure Otomasyon durum yapılandırması için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
- Bir sürekli dağıtım işlem hattı, Azure Otomasyonu durum yapılandırmasını kullanarak bir örnek görmek için bkz: [sürekli dağıtımı kullanarak Azure Otomasyon durum yapılandırması ve Chocolatey](automation-dsc-cd-chocolatey.md)
