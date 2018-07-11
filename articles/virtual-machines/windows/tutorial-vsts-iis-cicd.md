---
title: Öğretici - azure'da Team Services ile CI/CD işlem hattı oluşturma | Microsoft Docs
description: Bu öğreticide, bir Visual Studio Team Services sürekli tümleştirme ve azure'da Windows sanal makinesi üzerinde IIS'ye bir web uygulaması dağıtır teslim işlem hattı oluşturma konusunda bilgi edinin.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b23cec90573c4be73a73daf0bc0e793da012585c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932101"
---
# <a name="tutorial-create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Öğretici: Visual Studio Team Services ve IIS ile sürekli tümleştirme işlem hattı oluşturma
Derleme, test ve uygulama geliştirme için dağıtım aşamaları otomatikleştirmek için bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı kullanabilirsiniz. Bu öğreticide, IIS çalıştıran ve Azure Visual Studio Team Services ve Windows sanal makinesi (VM) kullanarak bir CI/CD işlem hattı oluşturun. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Team Services projesine ASP.NET web uygulaması yayımlama
> * Kod tamamlama tarafından tetiklenen bir yapı tanımı oluşturun
> * Yükleme ve azure'daki bir sanal makinede IIS yapılandırma
> * IIS örneği, Team Services'da bir dağıtım grubu ekleyin.
> * Bir yayın oluştur yeni web yayımlama için tanım paket IIS'ye dağıtma
> * CI/CD işlem hattı test

Bu öğretici, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).


## <a name="create-project-in-team-services"></a>Team Services'de projesi oluşturma
Visual Studio Team Services, şirket içi kod yönetim çözümünü korumadan kolay işbirliği ve geliştirme olanağı sağlar. Team Services, bulut kodu test, yapı ve application ınsights sağlar. Sürüm denetimi deposu ve kod geliştirme için en uygun IDE'yi seçebilirsiniz. Bu öğreticide, bir basit bir ASP.NET web uygulaması ve CI/CD işlem hattı oluşturmak için ücretsiz bir hesap kullanabilirsiniz. Bir Team Services hesabı zaten yoksa [oluşturmak](http://go.microsoft.com/fwlink/?LinkId=307137).

Kod yürütme işlemini yönetmek, derleme tanımları ve yayın tanımları için Team Services içinde bir proje şu şekilde oluşturun:

1. Team Services panonuzu bir web tarayıcısı açın ve seçin **yeni proje**.
2. Girin *mywebapp şeklindedir* için **proje adı**. Kullanılacak diğer tüm varsayılan değerleri bırakın *Git* sürüm denetimi ve *Çevik* iş öğesi işlemi.
3. Seçeneği için **paylaşmak** *takım üyeleri*, ardından **Oluştur**.
5. Projeniz oluşturulduktan sonra seçeneğini **Benioku ya da gitignore ile başlatın**, ardından **başlatmak**.
6. Yeni projeniz seçin **panolar** üstte seçip **Visual Studio'da Aç**.


## <a name="create-aspnet-web-application"></a>ASP.NET web uygulaması oluşturun
Önceki adımda, Team Services içinde bir proje oluşturdunuz. Son adım, yeni projenizi Visual Studio'da açılır. Kod işlemelerinizi yönettiğiniz **Takım Gezgini** penceresi. Yeni projeniz yerel bir kopyasını oluşturun ve ardından ASP.NET web uygulaması gibi bir şablondan oluşturun:

1. Seçin **kopya** Team Services projenize bir yerel git deposu oluşturmak için.
    
    ![Team Services projeden deposunu kopyalayın](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. Altında **çözümleri**seçin **yeni**.

    ![Web uygulaması çözümünü oluşturun](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Seçin **Web** şablonları seçip **ASP.NET Web uygulaması** şablonu.
    1. Gibi uygulamanız için bir ad girin *mywebapp şeklindedir*ve kutusunun işaretini kaldırın **çözüm için dizin oluştur**.
    2. Seçenek kullanılabilir değilse, için kutuyu temizleyin **projeye Application Insights Ekle**. Application Insights web uygulamanızı Azure Application Insights ile yetkilendirme gerektirir. Bu öğreticide basit tutmak için bu işlem atlayın.
    3. **Tamam**’ı seçin.
4. Seçin **MVC** şablon listesinden.
    1. Seçin **kimlik doğrulamayı Değiştir**, seçin **kimlik doğrulaması yok**, ardından **Tamam**.
    2. Seçin **Tamam** çözümünüzü oluşturmak için.
5. İçinde **Takım Gezgini** penceresinde seçin **değişiklikleri**.

    ![Team Services git deposu için yerel değişiklikleri](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. İşleme metin kutusuna gibi bir ileti girin *ilk işleme*. Seçin **tüm işleme ve eşitleme** aşağı açılan menüden.


## <a name="create-build-definition"></a>Yapı tanımı oluşturun
Team Services içinde nasıl uygulamanızın derlenmesi gereken ana hatlarını belirlemek için bir yapı tanımı kullanın. Bu öğreticide, bir IIS sunucusunda web uygulamasını çalıştırmak için kullanabiliriz paket kaynak kodumuzu çözümü oluşturur, ardından web oluşturur alır dağıttığınız bir temel tanımı oluştururuz.

1. Team Services projenize seçin **derleme ve yayınlama** üstte seçip **yapılar**.
3. Seçin **+ yeni tanım**.
4. Seçin **ASP.NET (Önizleme)** şablonu seçip alt **Uygula**.
5. Varsayılan görev değerleri bırakın. Altında **alma kaynakları**, emin *mywebapp şeklindedir* depo ve *ana* dal seçilir.

    ![Team Services projede derleme tanımı oluşturun](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Üzerinde **Tetikleyicileri** sekmesinde, için kaydırıcıyı **bu tetikleyiciyi etkinleştir** için *etkin*.
7. Derleme tanımını kaydet ve kuyruğa al yeni bir yapıyı seçerek **Kaydet ve kuyruğa** , ardından **Kaydet ve kuyruğa** yeniden. Seçin ve varsayılan değerleri bırakın **kuyruk**.

Barındırılan bir aracıda build zamanlandığında watch ardından oluşturmaya başlar. Çıktı aşağıdaki örneğe benzer:

![Team Services projenin başarılı derleme](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Sanal makine oluşturma
ASP.NET web uygulamanızı çalıştırmak için bir platform sağlamak için IIS çalıştıran bir Windows sanal makine gerekir. Team Services, kod tamamlama ve derleme tetiklendi IIS örneği ile etkileşim kurmak için bir aracı kullanır.

İle Windows Server 2016 VM oluşturma [yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVM* içinde *Doğu ABD* konumu. Kaynak grubu *myResourceGroupVSTS* ve destekleyici ağ kaynakları da oluşturulur. TCP bağlantı noktası olan Web trafiğine izin verecek şekilde *80* VM'ye açılır. İstendiğinde, bir kullanıcı adı ve sanal makine için oturum açma kimlik bilgileri olarak kullanılacak bir parola sağlayın:

```powershell
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine
New-AzureRmVM `
  -ResourceGroupName "myResourceGroupVSTS" `
  -Name "myVM" `
  -Location "East US" `
  -ImageName "Win2016Datacenter" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNetworkSecurityGroup" `
  -PublicIpAddressName "myPublicIp" `
  -Credential $cred `
  -OpenPorts 80
```

VM'nize bağlanmak için genel IP adresiyle elde [Get-Azurermpublicıpaddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) gibi:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Sanal makinenize Uzak Masaüstü oturumu oluşturun:

```cmd
mstsc /v:<publicIpAddress>
```

VM, açık bir **yönetici PowerShell** komut istemi. IIS ve gerekli .NET özellikleri aşağıda gösterildiği gibi yükleyin:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Dağıtım grubu oluşturun
Blokların IIS sunucusuna web dağıtım paketi, Team Services içinde bir dağıtım grubu tanımlayın. Bu gruba hangi sunucuların Team Services için kod tamamlama ve tamamlanan derlemeleri gibi yeni derleme hedef olduğunu belirtmenizi sağlar.

1. Team Services'ı seçin **derleme ve yayınlama** seçip **dağıtım grupları**.
2. Seçin **ekleme dağıtım grubu**.
3. Grup için bir ad girmeniz *myIIS*, ardından **Oluştur**.
4. İçinde **kaydetme makineler** bölümünde, olun *Windows* seçilir ve ardından kutuyu **kimlik doğrulaması için betikte kişisel erişim belirteci kullanmak**.
5. Seçin **betiği Panoya Kopyala**.


### <a name="add-iis-vm-to-the-deployment-group"></a>IIS VM dağıtım grubuna ekleyin.
Dağıtım grubu ile oluşturulan her IIS örneği grubuna ekleyin. Team Services indirir ve yeni web alan sanal makinede bir aracı yapılandıran bir betik oluşturur paketlerini dağıtma sonra IIS için geçerlidir.

1. Geri **yönetici PowerShell** VM'nize oturum yapıştırın ve Team Services kopyalanan betiği çalıştırın.
2. Aracı için etiketleri yapılandırmak için istendiğinde, *Y* girin *web*.
3. Kullanıcı hesabı için istendiğinde basın *dönüş* Varsayılanları kabul etmek için.
4. Komut dosyasının bir ileti görüntüler bekleyin *hizmet vstsagent.account.computername başarıyla başlatıldı*.
5. İçinde **dağıtım grupları** sayfasının **derleme ve yayınlama** menüsünde, açık *myIIS* dağıtım grubu. Üzerinde **makineler** sekmesinde, VM'nizi listelendiğini doğrulayın.

    ![VM Team Services dağıtım grubuna başarıyla eklendi](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Yayın tanımı oluşturma
Derlemelerinizi yayımlamak için Team Services içinde bir sürüm tanımı oluşturun. Bu tanım, uygulamanızın başarılı bir derleme tarafından otomatik olarak başlatılır. Dağıtım grubu, web göndermeyi seçtiğiniz paketi dağıtma ve uygun IIS ayarlarını tanımlayın.

1. Seçin **derleme ve yayınlama**, ardından **yapılar**. Bir önceki adımda oluşturulan derleme tanımı seçin.
2. Altında **son Tamamlananlar**, en son derlemeyi seçin ve ardından seçin **yayın**.
3. Seçin **Evet** bir yayın tanımı oluşturma.
4. Seçin **boş** şablonu seçip **sonraki**.
5. Proje ve kaynak derleme tanımı projenizle doldurulur doğrulayın.
6. Seçin **sürekli dağıtım** onay kutusunu işaretleyin ve ardından **Oluştur**.
7. Açılan kutuyu işaretleyin **+ Ekle görevleri** ve *bir dağıtım grubu aşaması Ekle*.
    
    ![Yayın tanımı Team Services için görev ekleyin](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Seçin **Ekle** yanındaki **IIS Web uygulamasını Deploy(Preview)**, ardından **Kapat**.
9. Seçin **dağıtım grubunda Çalıştır** üst görev.
    1. İçin **dağıtım grubu**, daha önce oluşturduğunuz dağıtım grubunu seçin gibi *myIIS*.
    2. İçinde **makine etiketleri** kutusunda **Ekle** ve *web* etiketi.
    
    ![IIS için yayın tanımı dağıtım grubu görevi](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Seçin **Dağıt: IIS Web uygulamasını dağıtma** görev aşağıdaki gibi IIS örneği ayarlarını yapılandırmak için:
    1. İçin **Web sitesi adı**, girin *varsayılan Web sitesi*.
    2. Diğer tüm varsayılan ayarları bırakın.
12. Seçin **Kaydet**, ardından **Tamam** iki kez.


## <a name="create-a-release-and-publish"></a>Bir sürüm oluşturma ve yayımlama
Artık web gönderebilmek için paket yeni bir sürüm olarak dağıtın. Bu adımı, dağıtım grubunun parçası olduğundan, web gönderim her örneği Aracısı ile iletişim kurar paket dağıtın ve ardından güncelleştirilen web uygulamasını çalıştırmak üzere IIS'i yapılandırır.

1. Yayın tanımınızı seçin **+ yayın**, ardından **yayın oluştur**.
2. İle birlikte en son sürüme aşağı açılan listeden, seçili olduğunu doğrulayın **otomatik dağıtım: yayın oluşturulduktan sonra**. **Oluştur**’u seçin.
3. Küçük bir başlık gibi yayın tanımınızı üst kısmında görünür *yayın 'Release-1' oluşturuldu*. Yayın bağlantıyı seçin.
4. Açık **günlükleri** yayın ilerleme durumunu izlemek için sekmesinde.
    
    ![Başarılı Team Services sürüm ve web paketi anında dağıtın](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Yayın tamamlandıktan sonra bir web tarayıcısı açın ve sanal makinenizin genel işlenen madde adresini girin. ASP.NET web uygulamanız çalışıyor.

    ![IIS sanal makinesinde çalışan ASP.NET web uygulaması](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a>CI/CD işlem hattının tamamını test
IIS üzerinde çalışan web uygulamanız ile CI/CD işlem hattının tamamını hemen deneyin. Visual Studio ve ardından, güncelleştirilen web sürümü tetikleyen bir derleme, kodunuzun tetiklenir işleme bir değişiklik yaptıktan sonra paketi IIS dağıtın:

1. Visual Studio'da açın **Çözüm Gezgini** penceresi.
2. Gidin ve açmak *mywebapp şeklindedir | Görünümleri | Giriş | Index.cshtml*
3. Okunacak 6 satırı düzenleyin:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Dosyayı kaydedin.
5. Açık **Takım Gezgini** penceresinde *mywebapp şeklindedir* proje ve ardından **değişiklikleri**.
6. Bir işleme iletisi girin *test CI/CD işlem hattı*, ardından **tümünü işle ve Eşitle** aşağı açılan menüden.
7. Team Services çalışma alanında, yeni bir yapı kod işlemeden diğerine tetiklenir. 
    - Seçin **derleme ve yayınlama**, ardından **yapılar**. 
    - Yapı tanımınızı seçin ve ardından **sıraya alınan & çalışıyor** sürerken izlemek için derleme.
9. Derleme başarılı olduktan sonra yeni bir yayın tetiklenir.
    - Seçin **derleme ve yayınlama**, ardından **yayınlar** web dağıtım paketi IIS sanal makinenizde gönderilen görmek için. 
    - Seçin **Yenile** durumunu güncelleştirmek için simge. Zaman *ortamları* sütununda yeşil bir onay işareti gösterir, IIS sürümü başarıyla dağıtılan.
11. Uygulanan yaptığınız değişiklikleri görmek için IIS Web sitesini bir tarayıcıda yenileyin.

    ![CI/CD işlem hattından IIS VM'de çalışan ASP.NET web uygulaması](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir ASP.NET web uygulamasını Team Services içinde oluşturulan ve derleme yapılandırılmış ve yayın tanımlarını yeni web dağıtımı paketleri IIS her kod işlemesinde dağıtın. Şunları öğrendiniz:

> [!div class="checklist"]
> * Team Services projesine ASP.NET web uygulaması yayımlama
> * Kod tamamlama tarafından tetiklenen bir yapı tanımı oluşturun
> * Yükleme ve azure'daki bir sanal makinede IIS yapılandırma
> * IIS örneği, Team Services'da bir dağıtım grubu ekleyin.
> * Bir yayın oluştur yeni web yayımlama için tanım paket IIS'ye dağıtma
> * CI/CD işlem hattı test

Bir SQL yükleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin&#92;IIS&#92;bir çift sanal makinelerinin Windows üzerinde .NET yığını.

> [!div class="nextstepaction"]
> [SQL&#92;IIS&#92;.NET yığını](tutorial-iis-sql.md)