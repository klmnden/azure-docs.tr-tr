---
title: "Geliştirme ve Test ortamları için yayımlamak için Windows PowerShell betikleri kullanma | Microsoft Docs"
description: "Windows PowerShell komut dosyalarını Visual Studio'dan yayımlamak için geliştirme ve test ortamları için nasıl kullanılacağını öğrenin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 92753860ec820172e46f483831eb0c1cf1acb038
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Geliştirme ve test ortamlarında yayımlama için Windows PowerShell betiklerini kullanma
Visual Studio'da bir web uygulaması oluşturduğunuzda, daha sonra Azure Web sitenizi yayımlama bir Web uygulamasını Azure App Service veya bir sanal makine olarak otomatikleştirmek için kullanabileceğiniz bir Windows PowerShell komut dosyası oluşturabilirsiniz. Düzenle ve Windows PowerShell komut dosyasını gereksinimlerinize uyacak şekilde Visual Studio düzenleyicisinde genişletin veya komut dosyasını varolan derleme, test ve komut dosyaları yayımlama ile tümleştirin.

Bu komut dosyalarını kullanarak, siteniz geçici kullanım için özelleştirilmiş sürümleri (geliştirme ve test ortamları olarak da bilinir) sağlayabilirsiniz. Örneğin, Web sitenizi bir Azure sanal makine ya da test paketi çalıştırmak, bir hatayı yeniden, önerilen bir değişikliğin hata düzeltmesi, deneme test veya bir tanıtım veya sunumu için özel bir ortam ayarlamak için bir Web sitesinde hazırlama yuvası belirli bir sürümü ayarlayabilir. Projenizi yayımlayan bir komut dosyası oluşturduktan sonra aynı ortamları betik gerektiği gibi yeniden çalıştırarak yeniden veya kendi yapı testi için özel bir ortam oluşturmak için web uygulamanızın komut dosyasını çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor
* Azure SDK 2.3 veya üzeri. Bkz: [Visual Studio indirmeleri](http://go.microsoft.com/fwlink/?LinkID=624384) daha fazla bilgi için.

Web projeleri için komut dosyaları oluşturmak için Azure SDK'sını gerekmez. Bu özellik web projeleri, olmayan bulut Hizmetleri web rolleri için kullanılabilir.

* Azure PowerShell 0.7.4 veya sonraki bir sürümü. Bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) daha fazla bilgi için.
* [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) veya sonraki bir sürümü.

## <a name="additional-tools"></a>Ek araçlar
Ek araçlar ve kaynaklar Azure geliştirme için Visual Studio de PowerShell ile çalışmak için kullanılabilir. Bkz: [Visual Studio için PowerShell Araçları](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Yayımlama betikleri oluşturma
Yayımlama betikleri izleyerek yeni bir proje oluşturduğunuzda, Web sitenizi barındıran bir sanal makine için oluşturabileceğiniz [bu yönergeleri](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Ayrıca [oluşturmak betikler web uygulamaları için Azure App Service'te yayımlama](app-service/app-service-web-get-started-dotnet.md).

## <a name="scripts-that-visual-studio-generates"></a>Visual Studio'nun oluşturduğu komut dosyaları
Visual Studio'nun oluşturduğu adlı bir çözüm düzeyi klasör **PublishScripts** iki Windows PowerShell dosyaları, sanal makinenize veya Web sitesi ve kullanabileceğiniz işlevleri içeren bir modül için bir yayımlama komut dosyasını içerir komut dosyaları. Visual Studio, aynı zamanda, dağıttığınız proje ayrıntılarını belirtir JSON biçiminde bir dosya oluşturur.

### <a name="windows-powershell-publish-script"></a>Windows PowerShell komut dosyası yayımlama
Yayımla komut dosyası için bir Web sitesi veya sanal makine dağıtımı için belirli Yayımla adımları içerir. Visual Studio için Windows PowerShell geliştirme renklendirme sözdizimi sağlar. İşlevlerin kullanılabilir ve değişen gereksinimlerinize uyacak şekilde betik işlevlerde serbestçe düzenleyebilirsiniz için Yardım.

### <a name="windows-powershell-module"></a>Windows PowerShell Modülü
Visual Studio'nun oluşturduğu Windows PowerShell modülü Yayımla komut dosyası kullanan işlevler içerir. Bunlar Azure PowerShell işlevleri ve değiştirilecek amaçlanmamıştır. Bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) daha fazla bilgi için.

### <a name="json-configuration-file"></a>JSON yapılandırma dosyası
JSON dosyası oluşturulur **yapılandırmaları** klasör ve Azure'a dağıtmak için tam olarak hangi kaynaklara belirtir yapılandırma verilerini içerir. Proje-adı-WAWS-bir sanal makine oluşturduysanız, bir Web sitesi ya da proje adı-VM-dev.json oluşturduysanız dev.json Visual Studio'nun oluşturduğu dosyasının adıdır. Bir Web sitesi oluşturduğunuzda oluşturan bir JSON yapılandırma dosyası bir örneği burada verilmiştir. Değerleri çoğu kendinden açıklamalıdır. Projenizin adına eşleşmeyebilir şekilde Web sitesi adı Azure tarafından oluşturulur.

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```
Bir sanal makine oluşturduğunuzda, JSON yapılandırma dosyası aşağıdakine benzer. Bir bulut hizmeti, sanal makine için bir kapsayıcı olarak oluşturulduğunu unutmayın. Sanal makine için yerel makine, Uzak Masaüstü ve Windows PowerShell Web sitesine yayımlamak imkan tanıyan Web dağıtımı, uç nokta yanı sıra her zamanki uç noktaları için HTTP ve HTTPS üzerinden web access içerir.

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Yayımlama betikleri çalıştırdığınızda neler değiştirmek için JSON yapılandırma düzenleyebilirsiniz. `cloudService` Ve `virtualMachine` bölümleri gereklidir, ancak silebilirsiniz `databases` ihtiyaç duymuyorsanız bölüm. Visual Studio'nun oluşturduğu varsayılan yapılandırma dosyası boş özellikleri isteğe bağlıdır; varsayılan yapılandırma dosyasında değerleri olan gereklidir.

Azure'da tek üretim site yerine birden çok dağıtım ortamı (yuvaları da bilinir) sahip bir Web sitesi varsa, Web sitesi adına yuva adı JSON yapılandırma dosyasında içerebilir. Adlı bir Web sitesi varsa, örneğin, **Sitem** ve onu adlı yuvasını **test** URI Sitem test.cloudapp.net olmakla birlikte yapılandırma dosyasında kullanılacak doğru adı mysite(test) sonra. Bu, Web sitesi yalnızca gerçekleştirebilir ve yuvaları aboneliğinizde zaten mevcut. Yoksa, Web sitesi oluşturma yuva belirtmeden betiği çalıştırarak, ardından yuvada oluşturun [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885), ve bundan sonra komut dosyasının değiştirilmiş Web sitesi adı ile çalıştırın. Web uygulamaları için dağıtım yuvası hakkında daha fazla bilgi için bkz: [hazırlık Azure App Service'deki web uygulamaları için ortamları ayarlama](app-service/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Yayımlama betikleri çalıştırma
Önce bir Windows PowerShell Betiği çalıştırılmadı, önce çalıştırılacak betikleri etkinleştirmek için yürütme ilkesi ayarlamanız gerekir. Bu kullanıcıların kötü amaçlı yazılım veya komut dosyaları yürütme içeren virüslere karşı savunmasız, Windows PowerShell komut dosyalarını çalıştırmasını engellemek için bir güvenlik özelliğidir.

### <a name="run-the-script"></a>Komut dosyasını çalıştır
1. Projeniz için Web dağıtımı paketi oluşturun. Bir Web Dağıtımı Web sitesi ya da sanal makineyi kopyalamak istediğiniz dosyaları içeren sıkıştırılmış bir arşiv (.zip dosyası) paketidir. Visual Studio'da Web dağıtımı paketleri için herhangi bir web uygulaması oluşturabilirsiniz.

![Web oluşturma paketini dağıtma](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Daha fazla bilgi için bkz: [nasıl yapılır: Visual Studio'da bir Web dağıtım paketi oluşturma](https://msdn.microsoft.com/library/dd465323.aspx). Web dağıtımı paketi oluşturulmasını bölümde açıklandığı gibi otomatikleştirebilirsiniz **özelleştirme ve yayımlama betikleri genişletme** bu konuda daha sonra.

1. İçinde **Çözüm Gezgini**betik bağlam menüsünü açın ve ardından **PowerShell ISE ile açmak**.
2. Bu bilgisayarda Windows PowerShell komut dosyalarını çalıştırmak ilk kez kullanıyorsanız, yönetici ayrıcalıklarıyla bir komut istemi penceresi açın ve aşağıdaki komutu yazın:

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. Aşağıdaki komutu kullanarak Azure'da oturum açın.

    ```powershell
    Add-AzureAccount
    ```

    İstendiğinde, kullanıcı adı ve parola sağlayın.

    Komut dosyası Otomasyon gerçekleştirirken Azure kimlik bilgileri sağlama bu yöntem çalışmaz unutmayın. Bunun yerine, kimlik bilgilerini sağlamak için .publishsettings dosyasını kullanmanız gerekir. Komut, size yalnızca bir kez kullanın **Get-AzurePublishSettingsFile** Azure'dan dosyasını indirin ve bundan sonra kullanmak için **Import-AzurePublishSettingsFile** dosya içeri aktarılamıyor. Ayrıntılı yönergeler için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

4. (İsteğe bağlı) Sanal makine gibi Azure kaynakları oluşturmak istiyorsanız, veritabanı ve web uygulamanızı yayımlamayı olmadan Web sitesi kullanmak **Yayımla WebApplication.ps1** komutunu **-yapılandırma**bağımsız değişkenini JSON yapılandırma dosyasına ayarlayın. Bu komut satırı JSON yapılandırma dosyası oluşturmak için hangi kaynak belirlemek için kullanır. Diğer komut satırı bağımsız değişkenleri için varsayılan ayarları kullandığından, kaynakları oluşturur, ancak web uygulamanızı yayımlamak değil. Verbose seçeneği neler hakkında daha fazla bilgi sağlar.

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. Kullanım **Yayımla WebApplication.ps1** komutu bir komut dosyası çağırma ve web uygulamanızı yayımlamak için aşağıdaki örnekleri gösterildiği gibi. Abonelik adı gibi diğer bağımsız değişkenlerden biri için varsayılan ayarları geçersiz kılacak gerekiyorsa paket adı, sanal makine kimlik bilgileri veya veritabanı sunucusu kimlik bilgileri yayımlama, bu parametreleri belirtebilirsiniz. Kullanım **– ayrıntılı** yayımlama işleminin ilerleme durumu hakkında daha fazla bilgi için seçeneği.

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    Bir sanal makine oluşturuyorsanız, komut aşağıdaki gibi görünür. Bu örnek ayrıca birden çok veritabanı için kimlik bilgilerini belirleme konusunda gösterir. Bu komut dosyaları oluşturmak sanal makineler'ne için SSL sertifikası bir güvenilen kök yetkilisi değil. Bu nedenle, kullanmanıza gerek **– AllowUntrusted** seçeneği.

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    Komut dosyası veritabanları oluşturabilirsiniz, ancak veritabanı sunucuları oluşturmaz. Bir veritabanı sunucusu oluşturmak istiyorsanız, kullanabileceğiniz **yeni AzureSqlDatabaseServer** Azure modülünde işlevi.

## <a name="customizing-and-extending-the-publish-scripts"></a>Özelleştirme ve yayımlama betikleri genişletme
Yayımla betiği ve JSON yapılandırma dosyası özelleştirebilirsiniz. Windows PowerShell modülünü işlevlerde **AzureWebAppPublishModule.psm1** değiştirilecek amaçlanmamıştır. Yalnızca farklı bir veritabanı belirtin veya sanal makinenin özelliklerini değiştirmek istiyorsanız, JSON yapılandırma dosyasını düzenleyin. Derleme ve dağıtım projenizi sınamayı otomatikleştirmek için komut dosyası işlevselliğini genişletmek istiyorsanız, işlevi saplamalar uygulayabilirsiniz **Yayımla WebApplication.ps1**.

Projenizi derleme otomatikleştirmek için MSBuild için çağıran kodu eklemek `New-WebDeployPackage` Bu kod örneğinde gösterildiği gibi. MSBuild komut yolu yüklediğiniz Visual Studio sürümüne bağlı olarak farklı olur. Doğru yolu almak için işlevini kullanabilirsiniz **Get-MSBuildCmd**, bu örnekte gösterildiği gibi.

### <a name="to-automate-building-your-project"></a>Projenizi derleme otomatik hale getirmek için
1. Ekleme `$ProjectFile` genel param bölümünde parametresi.

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. Copy işlevi `Get-MSBuildCmd` komut dosyanızı içine.

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. Değiştir `New-WebDeployPackage` ile aşağıdaki kod ve satır oluştururken yer tutucuları değiştirmek `$msbuildCmd`. Visual Studio 2015 için kodudur. Visual Studio 2013 kullanıyorsanız, değiştirme **VisualStudioVersion** aşağıda özelliğine `12.0`.

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function to build and package your web application
    ```

    Web uygulamanızı oluşturmak için MsBuild.exe kullanın. MSBuild komut satırı başvurusu, Yardım için bkz: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-the-build-command"></a>Yapı komutu yürütme Başlat

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. Çağrı `New-WebDeployPackage` bu satırı önce işlevin: `$Config = Read-ConfigFile $Configuration` web uygulamaları için veya `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` sanal makineler için.

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. Özelleştirilmiş komut dosyası geçirme kullanarak komut satırından çağırma `$Project` olduğu gibi aşağıdaki örnek komut satırı bağımsız değişkeni.

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    Uygulamanızın veya testleri otomatikleştirmek için kodu ekleyin `Test-WebApplication`. Satırları açıklamadan çıkarın özen **Yayımla WebApplication.ps1** burada bu işlevler adı verilir. Uygulaması sağlamazsanız, projenizi Visual Studio ile el ile oluşturun ve Azure'a yayımlamak için Yayımla betiğini çalıştırın.

## <a name="publishing-function-summary"></a>Özet yayımlama işlevi
Windows PowerShell komut isteminde kullanabileceğiniz işlevleri için Yardım almak için komut kullanmak `Get-Help function-name`. Yardım parametre Yardım ve örnekler içerir. Aynı Yardım metni de betik kaynak dosyaları, **AzureWebAppPublishModule.psm1** ve **Yayımla WebApplication.ps1**. Komut dosyası ve Yardım Visual Studio dilinizde yerelleştirilmiştir.

**AzureWebAppPublishModule**

| İşlev adı | Açıklama |
| --- | --- |
| Ekleme AzureSQLDatabase |Yeni bir Azure SQL veritabanı oluşturur. |
| Ekleme AzureSQLDatabases |Azure SQL veritabanları değerlerin Visual Studio'nun oluşturduğu JSON yapılandırma dosyası oluşturur. |
| Add-AzureVM |Bir Azure sanal makine oluşturur ve dağıtılan VM URL'sini döndürür. İşlev önkoşulların ayarlar ve ardından çağırır **New-AzureVM** yeni bir sanal makine oluşturmak için işlevi (Azure Modülü). |
| Ekleme AzureVMEndpoints |Yeni giriş uç noktaları bir sanal makineye ekler ve yeni uç nokta ile sanal makine döndürür. |
| Ekleme AzureVMStorage |Yeni bir Azure depolama hesabı geçerli abonelikte oluşturur. Hesap adını "benzersiz bir alfasayısal dize tarafından izlenen devtest" ile başlar. İşlevi yeni depolama hesabı adını döndürür. Bir konumu veya yeni depolama hesabı için bir benzeşim grubu belirtmeniz gerekir. |
| Ekleme AzureWebsite |Bir Web sitesi belirtilen adı ve konumu ile oluşturur. Bu işlev çağrılarını **yeni AzureWebsite** Azure modülünde işlevi. Abonelik zaten belirtilen adda bir Web sitesi içermiyorsa, bu işlev Web sitesi oluşturur ve bir Web sitesi nesnesi döndürür. Aksi takdirde, döndürür `$null`. |
| Yedekleme abonelik |Geçerli Azure aboneliğindeki kaydeder `$Script:originalSubscription` betik kapsamında değişken. Bu işlev geçerli Azure aboneliği kaydeder (ile alınan `Get-AzureSubscription -Current`) ve depolama hesabı ve bu komut dosyası tarafından değiştirilen abonelik (değişkeninde depolanan `$UserSpecifiedSubscription`) ve betik kapsamında, depolama hesabı. Değerleri kaydederek bir işlev gibi kullanabileceğiniz `Restore-Subscription`, geçerli durum değiştiyse özgün geçerli abonelik ve depolama hesabı geçerli durumuna geri yüklemek için. |
| Bul-AzureVM |Belirtilen Azure sanal makine alır. |
| Biçim DevTestMessageWithTime |Tarih ve saat için bir ileti başına. Bu işlev, hata ve ayrıntı akışlara yazılan iletileri için tasarlanmıştır. |
| Get-AzureSQLDatabaseConnectionString |Bir Azure SQL veritabanına bağlanmak için bir bağlantı dizesi derler. |
| Get-AzureVMStorage |İlk Depolama hesabı adı deseni ile adını döndürür "devtest*" (büyük küçük harfe duyarlı) belirtilen konum veya benzeşim grubu. Varsa "devtest*" depolama hesabı konumu ya da benzeşim grubu eşleşmiyor, işlevi yok sayar. Bir konumu veya bir benzeşim grubu belirtmeniz gerekir. |
| Get-MSDeployCmd |MsDeploy.exe aracını çalıştırmak için bir komut döndürür. |
| AzureVMEnvironment yeni |Bulur veya bir sanal makine JSON yapılandırma dosyasındaki değerleri eşleşen abonelik oluşturur. |
| Yayımlama WebPackage |MsDeploy.exe kullanır ve bir web paketi yayımlayın. Kaynakları bir Web sitesine dağıtmak için zip dosyası. Bu işlev, herhangi bir çıktı üretmez. MSDeploy.exe çağrısı başarısız olursa, işlev özel durum oluşturur. Daha ayrıntılı çıktı elde etmek için kullanın **-Verbose** seçeneği. |
| Yayımlama WebPackageToVM |Parametre değerleri doğrular ve ardından çağırır **Yayımla WebPackage** işlevi. |
| Okuma ConfigFile |JSON yapılandırma dosyası doğrular ve seçili değerlerinin bir karma tablosu döndürür. |
| Geri yükleme-abonelik |Geçerli aboneliğe özgün abonelik sıfırlar. |
| Test-AzureModule |Döndürür `$true` yüklü Azure Modül sürümü 0.7.4 ise veya sonraki bir sürümü. Döndürür `$false` Modülü yüklü değil veya önceki bir sürümü. Bu işlev hiç parametre yok. |
| Test-AzureModuleVersion |Döndürür `$true` Azure modülü sürümü 0.7.4 ise veya sonraki bir sürümü. Döndürür `$false` Modülü yüklü değil veya önceki bir sürümü. Bu işlev hiç parametre yok. |
| Test-HttpsUrl |Giriş URL'SİNİN System.Uri nesnesine dönüştürür. Döndürür `$True` URL mutlak ve kendi şeması https ise. Döndürür `$false` URL göreli kendi şeması HTTPS değil veya giriş dizesini bir URL dönüştürülemiyor. |
| Test-üyesi |Döndürür `$true` bir özelliği veya yöntemi nesnenin üyesiyse. Aksi takdirde, döndürür `$false`. |
| Yazma ErrorWithTime |Geçerli saati ile önekli bir hata iletisi yazar. Bu işlev çağrılarını **biçimi DevTestMessageWithTime** ileti hata akışı yazmadan önce saat başına işlevi. |
| Yazma HostWithTime |Ana bilgisayar programı bir ileti yazar (**Write-Host**) ile geçerli saati öneki. Ana bilgisayar programı yazma etkisini değişir. Çoğu program barındıran Windows PowerShell için standart çıktı bu iletiler yazma. |
| Yazma VerboseWithTime |Geçerli saati ile önek ayrıntılı bir ileti yazar. Çağırır çünkü **Write-Verbose**, iletisini görüntüler yalnızca zaman komut dosyasını çalıştırır **ayrıntılı** parametresi veya ne zaman **VerbosePreference** tercih içinayarlama **Devam**. |

**Yayımlama WebApplication**

| İşlev adı | Açıklama |
| --- | --- |
| AzureWebApplicationEnvironment yeni |Bir Web sitesi veya sanal makine gibi Azure kaynakları oluşturur. |
| WebDeployPackage yeni |Bu işlev uygulanmadı. Projenizi yapılandırmak için bu işlevde komutları ekleyebilirsiniz. |
| Yayımlama AzureWebApplication |Azure web uygulaması yayımlar. |
| Yayımlama WebApplication |Oluşturur ve Web uygulamaları, sanal makineler, SQL veritabanları ve depolama hesapları için bir Visual Studio web projesini dağıtır. |
| Test-WebApplication |Bu işlev uygulanmadı. Uygulamanızı test etmek için bu işlevde komutları ekleyebilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar
PowerShell okuyarak komut dosyası hakkında daha fazla bilgi [ile Windows PowerShell komut dosyası](https://technet.microsoft.com/library/bb978526.aspx) ve diğer Azure PowerShell komut dosyalarını en [Komut Merkezi](https://azure.microsoft.com/documentation/scripts/).
