---
title: Azure Otomasyonu DSC sürekli dağıtımı Chocolatey ile
description: Azure Otomasyonu DSC ve Chocolatey Paket Yöneticisi'ni kullanarak sürekli dağıtım DevOps.  Örnek tam JSON ARM şablonu ve PowerShell kaynağı.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: bf535dfae4c5f710a423343bc3d76c81d83df2ae
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="usage-example-continuous-deployment-to-virtual-machines-using-automation-dsc-and-chocolatey"></a>Kullanım örneği: Automation DSC ve Chocolatey kullanarak sanal makineleri için sürekli dağıtım
Bir DevOps dünyanın çeşitli noktalarıyla sürekli tümleştirme ardışık düzeninde yardımcı olmak üzere birçok araç vardır.  Azure Otomasyonu istenen durum yapılandırması (DSC) bir DevOps takımlar uygulayabileceğiniz seçeneklerine Hoş Geldiniz yeni ektir.  Bu makalede, bir Windows bilgisayar için yukarı sürekli dağıtımı (CD) ayarı gösterilmektedir.  (Bir web sitesi, örneğin gibi) rolü ve ek roller için buradan gerektiği kadar Windows bilgisayarlar dahil olmak üzere teknik kolayca genişletebilirsiniz.

![Iaas VM'ler için sürekli dağıtım](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>Yüksek düzeyde
Buraya gelmeden oldukça bit yoktur, ancak iki ana süreçleriyle Neyse ayrılabilir: 

* Kod yazma, test, oluşturma ve sistem birincil ve ikincil sürümleri için yükleme paketleri yayımlama. 
* Oluşturma ve yükleyecek ve paketlerinde kod yürütmek VM'ler yönetme.  

Her iki çekirdek işlem yerinde olduktan sonra otomatik olarak yeni sürümler oluşturulan ve dağıtılan herhangi belirli VM üzerinden çalışan paketi güncellemek için kısa bir adımdır.

## <a name="component-overview"></a>Bileşenine genel bakış
Yöneticileri gibi paketini [get apt](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) Linux world ancak değil çok Windows dünyada oldukça iyi bilinen sorunlardır.  [Chocolatey](https://chocolatey.org/) böyle bir şey ve Scott Hanselman'ın [blog](http://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) konusu harika bir giriş değil.  Buna koysalar Chocolatey paketleri merkezi paketleri deposundan komut satırını kullanarak bir Windows sistemine yüklemek sağlar.  Oluşturma ve kendi depo yönetme ve Chocolatey belirttiğiniz depoları arasında herhangi bir sayı paketleri yükleyebilirsiniz.

İstenen durum Yapılandırması'nı (DSC) ([genel bakış](https://technet.microsoft.com/library/dn249912.aspx)) için bir makine istediğiniz yapılandırmasını bildirmek izin veren bir PowerShell aracıdır.  Örneğin, sizin deyin, "yüklü Chocolatey istediğiniz IIS'in yüklenmesini istediğiniz, açılan bağlantı noktası 80 istediğiniz, yüklü sürümü 1.0.0 sitemde istiyorum."  DSC yerel Configuration Manager (LCM'yi) bu yapılandırmasını uygular. Bir DSC çekme sunucusuna bir depo makinelerinizi için yapılandırmaların tutar. Her makinede LCM'yi düzenli aralıklarla yapılandırmasıyla depolanmış yapılandırmayı eşleşip eşleşmediğini teslim eder. Bu durum raporu veya makine hizalama depolanan yapılandırma ile sağlamak çalışır. Bir makine ya da değiştirilmiş yapılandırmasıyla hizalama içine gelmesini makineler kümesi neden çekme sunucusunda depolanan yapılandırma düzenleyebilirsiniz.

Azure Otomasyonu runbook'ları, düğümler, kimlik bilgilerini, kaynakları ve zamanlamaları ve genel değişkenler gibi varlıklar kullanarak çeşitli görevleri otomatik hale getirmek izin veren Microsoft azure'da yönetilen bir hizmettir. Azure Otomasyonu DSC PowerShell DSC araçları dahil etmek için bu Otomasyon özelliği genişletir.  Mükemmel işte [genel bakış](automation-dsc-overview.md).

DSC kaynağı olan ağ, Active Directory ya da SQL Server yönetme gibi belirli özelliklerine kod bir modüldür.  Chocolatey DSC kaynağı (diğerlerinin yanı sıra) NuGet sunucusuna erişim, paketleri indirmesine, paketleri yüklemek ve benzeri nasıl bilir.  Diğer DSC kaynakları birçok vardır [PowerShell Galerisi](http://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title).  Bu modüller, Azure Automation DSC çekme Sunucusu'na (sizin tarafınızdan) yüklü olan yapılandırmalarınızı tarafından kullanılabilmesi için.

Resource Manager şablonları altyapınızı - ağları, alt ağlar, ağ güvenliği gibi şeyleri oluşturmanın bildirim temelli bir yolunu sağlar ve yönlendirme, yük Dengeleyiciler, NIC'ler, VM'ler ve benzeri.  Burada bir [makale](../azure-resource-manager/resource-manager-deployment-model.md) Resource Manager dağıtım modeli (tanımlayıcı) Azure Hizmet Yönetimi (ASM veya Klasik) ile karşılaştırır dağıtım modeli (kesinliği) ve çekirdek kaynak sağlayıcıları, işlem, depolama açıklanır ve ağ.

Bir Resource Manager şablonu bir anahtar özellik sağlandığından gibi VM uzantısı VM'yi yükleme yeteneğidir.  VM uzantısı özel bir komut dosyası, virüsten koruma yazılımı yükleme veya bir DSC yapılandırma betiği çalıştırmasını gibi belirli özellikleri vardır.  Diğer türlerde VM uzantıları vardır.

## <a name="quick-trip-around-the-diagram"></a>Diyagram çevresinde hızlı seyahat
Üstten başlayarak, kodunuz yazma derleme, test ve sonra bir yükleme paketi oluşturun.  Chocolatey yükleme paketleri, MSI, MSU, posta gibi çeşitli türlerde işleyebilir.  Ve Chocolatey'nın yerel yetenekleri oldukça bu kadar yoksa gerçek yükleme yapmak için tam güç PowerShell vardır.  Paket nedenini bildirmeden bir yerde ulaşılabilir – bir paket deposu yerleştirin.  Bu kullanım örnek bir Azure blob storage hesabı ortak bir klasörde kullanır, ancak herhangi bir yere olabilir.  Chocolatey yerel NuGet sunucularını ve birkaç diğerleri paket meta verileri yönetimi için birlikte çalışır.  [Bu makalede](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) seçenekler açıklanmaktadır.  Bu kullanım örneği NuGet kullanır.  Bir Nuspec paketlerinizi hakkındaki meta verileri ' dir.  Nuspec's "NuPkg ait derlenmiş" ve bir NuGet sunucusunda depolanır.  Yapılandırmanıza bir paket adıyla ister ve NuGet sunucusuna başvuran Chocolatey DSC kaynağı (şimdi VM) paket alan ve sizin yerinize yükler.  Ayrıca, belirli bir paket sürümü isteyebilir.

Resmin sol alt kısmında bir Azure Resource Manager (ARM) şablonu yoktur.  Bu kullanım örnekte VM uzantısı VM Azure Otomasyonu DSC istek sunucusuyla (diğer bir deyişle, bir çekme sunucu) ile bir düğüm olarak kaydeder.  Yapılandırma çekme sunucusunda depolanır.  Aslında, iki kez depolandıktan: düz metin olarak ve (için olanlar gibi şeyler hakkında bilmeniz.) bir MOF dosyası olarak derlenmiş sonra bir kez  Portalda, MOF bir "düğümü" (aksine sadece "yapılandırma") yapılandırmadır.  Bu düğüm yapılandırmasını bilir, böylece bir düğümle ilişkili yapı gösterir.  Ayrıntılar aşağıdaki düğüm yapılandırması düğüme atama gösterir.

Büyük olasılıkla zaten üst veya çoğuna bit yaptığınız.  Nuspec oluşturma, derleme ve bir NuGet Server'da depolama küçük bir şeydir.  Ve sanal makineleri zaten yönetme.  Sonraki adım için sürekli dağıtım alma (bir kez) çekme sunucusu kurma, düğümleriniz Bununla (bir kez), kaydetme ve oluşturma ve yapılandırma vardır (başlangıçta) depolama gerektirir.  Daha sonra paketleri yükseltilmiş ve depoya dağıtılan yapılandırma ve düğüm yapılandırması çekme sunucusunda (gerektiği şekilde yineleyin) yenileyin.

ARM şablonu ile başlatıyorsanız değil, bu da normaldir.  Çekme server ve tüm REST, Vm'leri kaydetme yardımcı olmak amacıyla tasarlanmış PowerShell cmdlet'leri vardır. Bu makalede daha fazla ayrıntı için bkz: [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md)

## <a name="step-1-setting-up-the-pull-server-and-automation-account"></a>1. adım: Çekme sunucusuna ve Otomasyon hesabı ayarlama
Bir kimliği doğrulanmış (Connect-AzureRmAccount) PowerShell komut satırında: (çekme sunucunun ayarlanması birkaç dakika sürebilir)

    New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
    New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT 

Otomasyon hesabınızı aşağıdaki bölgeler (diğer adıyla konum) hiçbirine koyabilirsiniz: Doğu ABD 2, Orta Güney ABD, BİZE kamu Virginia, Batı Avrupa, Güneydoğu Asya, Japonya Doğu, Orta Hindistan ve Avustralya Güneydoğu, Orta Kanada Kuzey Avrupa.

## <a name="step-2-vm-extension-tweaks-to-the-arm-template"></a>2. adım: VM uzantısı tweaks ARM şablonu
Bu konuda sağlanan Ayrıntılar (PowerShell DSC VM uzantısı kullanılarak) VM kaydı için [Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver).  Bu adım, yeni VM DSC düğüm listesi çekme sunucusunda ile kaydeder.  Bu kayıt parçası düğüm yapılandırması düğüme uygulanacak belirtme.  Bu düğüm yapılandırması bu ilk kez burada yapılır adım 4. Tamam olacak şekilde çekme sunucusunda henüz mevcut sahip değil.  Ancak burada 2. adımda düğümün adını ve yapılandırmanın adını karar vermeniz gerekir.  Bu kullanım örnekte düğümdür 'isvbox' ve 'ISVBoxConfig' yapılandırmadır.  Bu nedenle (DeploymentTemplate.json içinde belirtilmesi) düğüm Yapılandırması 'ISVBoxConfig.isvbox' adıdır.  

## <a name="step-3-adding-required-dsc-resources-to-the-pull-server"></a>3. adım: gerekli DSC kaynakları çekme sunucusuna ekleme
Azure Otomasyon hesabınızda DSC kaynakları yüklemek için PowerShell Galerisi işaretlenir.  İstediğiniz ve "Dağıtmak için Azure Otomasyonu" düğmesini tıklatın kaynağa gidin.

![PowerShell Galerisi örneği](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Azure Portalı'na eklenen son başka bir yöntem, yeni modülleri çekme veya var olan modülleri güncelleştirmek sağlar. Otomasyon hesabı kaynak varlıklar kutucuğu tıklayın ve son olarak modülleri döşeme.  Gözat galeri simgesi galerisinde modüllerin listesini görmek, ayrıntılı bilgiler detaya ve sonuçta Otomasyon hesabınızda içeri aktarmanızı sağlar. Bu modüllerin zaman zaman güncel tutmak için harika bir yoludur. Ve hiçbir şey eşitlenmemiş alır emin olmak için diğer modüllerle bağımlılıkları içeri aktarma özelliğini denetler.

Ya da el ile yaklaşım vardır.  Bir Windows bilgisayar için bir PowerShell tümleştirme modülü klasör yapısını, Azure Automation'ın beklenen klasör yapısı biraz farklıdır.  Bu, küçük bir bölümü'uyguladıkça gerektirir.  Ancak sabit değil ve (gelecekte yükseltmek istiyor. sürece) yalnızca bir kez kaynak başına yapılır  Bu makalede PowerShell tümleştirme modülleri yazma hakkında daha fazla bilgi için bkz: [için Azure Automation tümleştirme modülleri yazma](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

* İş istasyonunuza, aşağıdaki gibi gereken modülünü yükleyin:
  * Yükleme [Windows Management Framework v5](http://aka.ms/wmf5latest) (Windows 10 için gerekli değildir)
  * `Install-Module –Name MODULE-NAME`    < — PowerShell Galerisi'nden modül alan 
* Modül klasöründen kopyalayın `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` geçici bir klasöre 
* Örnekler ve belgeler ana klasöründen silin 
* ZIP dosyasının tam olarak aynı klasörü adlandırma ana klasör zip 
* ZIP dosyası Azure depolama hesabında blob depolama gibi erişilebilir bir HTTP konuma yerleştirin.
* Bu PowerShell çalıştırın:
  
      New-AzureRmAutomationModule `
          -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
          -Name MODULE-NAME –ContentLink "https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip"

Dahil edilen örnek cChoco ve xNetworking için aşağıdaki adımları gerçekleştirir. Bkz: [notları](#notes) cChoco için özel işleme.

## <a name="step-4-adding-the-node-configuration-to-the-pull-server"></a>4. adım: düğüm yapılandırması çekme sunucusuna ekleme
Özel derleme ve çekme sunucu yapılandırmanızı alma ilk kez hakkında hiçbir şey yoktur.  Tüm sonraki alma/derlerken aynı yapılandırmaya sahip tam olarak aynı arayın.  Paketinizi güncelleştirin ve üretim için anında gerek her zaman bu adımı yaptığınız yapılandırma dosyası doğru olduktan – paketinin yeni sürümü de dahil olmak üzere.  Yapılandırma dosyası ve PowerShell şöyledir:

ISVBoxConfig.ps1:

    Configuration ISVBoxConfig 
    { 
        Import-DscResource -ModuleName cChoco 
        Import-DscResource -ModuleName xNetworking

        Node "isvbox" {   

            cChocoInstaller installChoco 
            { 
                InstallDir = "C:\choco" 
            }

            WindowsFeature installIIS 
            { 
                Ensure="Present" 
                Name="Web-Server" 
            }

            xFirewall WebFirewallRule 
            { 
                Direction = "Inbound" 
                Name = "Web-Server-TCP-In" 
                DisplayName = "Web Server (TCP-In)" 
                Description = "IIS allow incoming web site traffic." 
                DisplayGroup = "IIS Incoming Traffic" 
                State = "Enabled" 
                Access = "Allow" 
                Protocol = "TCP" 
                LocalPort = "80" 
                Ensure = "Present" 
            }

            cChocoPackageInstaller trivialWeb 
            {            
                Name = "trivialweb" 
                Version = "1.0.0" 
                Source = “MY-NUGET-V2-SERVER-ADDRESS” 
                DependsOn = "[cChocoInstaller]installChoco", 
                "[WindowsFeature]installIIS" 
            } 
        }    
    }

Yeni-ConfigurationScript.ps1:

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

Yeni bir düğüm yapılandırması bu adımları neden "çekme sunucusunda yerleştirilen ISVBoxConfig.isvbox" adlı.  Düğüm yapılandırması adı "configurationName.nodeName" oluşturulmuştur.

## <a name="step-5-creating-and-maintaining-package-metadata"></a>5. adım: Oluşturma ve paket meta verileri koruma
Paket depoya yerleştirdiğiniz her paket için açıklayan bir nuspec gerekir.  Bu nuspec derlenmiş ve NuGet sunucunuz depolanır. Bu işlem açıklanan [burada](http://docs.nuget.org/create/creating-and-publishing-a-package).  Bir NuGet sunucusu olarak MyGet.org kullanabilirsiniz.  Bu hizmet satmak, ancak Başlatıcı ücretsizdir SKU'su vardır.  NuGet.org özel paketlerinizi için kendi NuGet sunucusu yükleme hakkında yönergeler bulabilirsiniz.

## <a name="step-6-tying-it-all-together"></a>6. adım: hepsini bir araya getirmeye kadar
Her zaman bir sürüm QA geçirir ve onaylandıktan dağıtımı için paketin oluşturulduğu, nuspec ve güncelleştirilmiş ve NuGet sunucusuna dağıtılan nupkg.  Ayrıca, yeni sürüm numarasıyla kabul edin (yukarıdaki adım 4) yapılandırmasının güncelleştirilmesi gerekmektedir.  Çekme sunucusuna gönderilir ve derlenmiş gerekir.  Bu noktadan itibaren güncelleştirme çekme ve yüklemek için bu yapılandırmasına göre değişir VM'ler için olmasından.  Bu güncelleştirmelerden her birini basit - yalnızca bir çizgi veya iki PowerShell.  Visual Studio Team Services söz konusu olduğunda, bir derleme birbirine zincirlenebilir derleme görevleri bazıları kapsüllenir.  Bu [makale](https://www.visualstudio.com/en-us/docs/alm-devops-feature-index#continuous-delivery) daha fazla ayrıntı sağlar.  Bu [GitHub deposuna](https://github.com/Microsoft/vso-agent-tasks) çeşitli kullanılabilir yapı görevlerin ayrıntılarını verir.

## <a name="notes"></a>Notlar
Azure Galerisi'nden genel bir Windows Server 2012 R2 görüntüsünden bir VM ile bu kullanım örneği başlatır.  Depolanmış bir görüntüden başlatın ve DSC yapılandırması ile buradan ince ayar.  Ancak, bir görüntüye baked yapılandırmasını değiştirme DSC kullanarak yapılandırmasını dinamik olarak güncelleştirilmesi daha çok daha zordur.

Bu yöntem, VM ile birlikte kullanmak için bir ARM şablonu ve VM uzantısı kullanmanız gerekmez.  Ve Vm'lerinizi CD Yönetimi altında olacak şekilde Azure üzerinde olması gerekmez.  Tüm gerekli olan Chocolatey yüklenmesi ve çekme sunucunun olduğu bilmesi için LCM'yi VM üzerinde yapılandırılmış.  

Elbette, bir paket üretim bir VM'de güncelleştirdiğinizde, güncelleştirme yüklenirken döndürme dışında bu VM bulundurmanız gerekir.  Bunu nasıl yapacağınız yaygın olarak değişir.  Örneğin, bir Azure yük dengeleyici arkasında bir VM ile özel araştırma ekleyebilirsiniz.  VM güncelleştirilirken bir 400 dönüş araştırma uç noktası vardır.  İnce ayar bu değişikliğe neden gerekli yapılandırmanızı içinde Güncelleştirme tamamlandıktan sonra geri bir 200 döndürmek için geçiş yapmak için ince ayar yapabilirsiniz olabilir.

Bu kullanım örneğin tam kaynak konusu [bu Visual Studio projesi](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) github'da.

## <a name="related-articles"></a>İlgili Makaleler
* [Azure Otomasyonu DSC genel bakış](automation-dsc-overview.md)
* [Azure Otomasyonu DSC cmdlet'leri](https://msdn.microsoft.com/library/mt244122.aspx)
* [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md)

