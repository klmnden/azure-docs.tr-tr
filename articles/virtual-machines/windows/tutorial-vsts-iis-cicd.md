---
title: Team Services ile azure'da CI/CD işlem hattı oluşturma | Microsoft Docs
description: Sürekli tümleştirme ve IIS üzerinde bir Windows VM için bir web uygulamasına dağıtır teslimat için bir Visual Studio Team Services potansiyel oluşturmayı öğrenin
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
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
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: cf6e3013d4dfc7e18d96a717a76b591cde939139
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Visual Studio Team Services ve IIS ile sürekli tümleştirme işlem hattı oluşturma
Derleme, test ve dağıtım aşamaları uygulama geliştirme otomatikleştirmek için sürekli tümleştirme ve dağıtım (CI/CD) ardışık düzen kullanabilirsiniz. Bu öğreticide, Visual Studio Team Services ve Windows sanal makine (VM), IIS çalıştıran Azure kullanarak bir CI/CD işlem hattı oluşturun. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Team Services proje için ASP.NET web uygulaması yayımlama
> * Kod tamamlama tarafından tetiklenen bir yapı tanımı oluşturun
> * IIS yükle ve azure'da bir sanal makinede Yapılandır
> * Bir dağıtım grubuna Team Services IIS örneği ekleme
> * Bir yayın oluşturmak tanımı yeni web yayımlama IIS paketlerini dağıtma
> * CI/CD ardışık düzen test

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).


## <a name="create-project-in-team-services"></a>Team Services içinde projesi oluşturma
Şirket içi kodu yönetim çözümünü korumadan kolay işbirliği ve geliştirme için Visual Studio Team Services sağlar. Team Services kodu bulut test, yapı ve application ınsights sağlar. Sürüm denetim deposunu ve kod geliştirme en uygun IDE seçebilirsiniz. Bu öğretici için ücretsiz bir hesap, temel ASP.NET web uygulaması ve CI/CD ardışık düzen oluşturmak için kullanabilirsiniz. Bir Team Services hesabı zaten yoksa [oluşturmak](http://go.microsoft.com/fwlink/?LinkId=307137).

Kod yürütme işlemini yönetmek, yapı tanımları ve tanımları yayın için Team Services içinde bir proje gibi oluşturun:

1. Team Services panonuz bir web tarayıcısında açın ve seçin **yeni proje**.
2. Girin *mywebapp şeklindedir* için **proje adı**. Tüm diğer varsayılan değerleri bırakın *Git* sürüm denetimi ve *Çevik* iş öğesi işlemi.
3. Seçeneği için **paylaşmak** *ekip üyelerinin*seçeneğini belirleyip **oluşturma**.
5. Projeniz oluşturulduktan sonra seçeneği **bir benioku veya gitignore ile başlatmak**, ardından **başlatma**.
6. Yeni projeniz içinde seçin **panolar** üstte seçip **Visual Studio'da Aç**.


## <a name="create-aspnet-web-application"></a>ASP.NET web uygulaması oluşturma
Önceki adımda, bir proje Team Services ile oluşturulmuş. Son adım, yeni projeniz Visual Studio'da açılır. Kod tamamlama yönetmek **Takım Gezgini** penceresi. Yeni projeniz yerel bir kopyasını oluşturun, sonra bir ASP.NET web uygulaması gibi bir şablondan oluşturun:

1. Seçin **kopya** Team Services projenizin yerel git deposu oluşturmak için.
    
    ![Team Services projeden depoyu kopyalama](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. Altında **çözümleri**seçin **yeni**.

    ![Web uygulaması çözümü oluşturun](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Seçin **Web** şablonları ve ardından **ASP.NET Web uygulaması** şablonu.
    1. Gibi uygulamanız için bir ad girin *mywebapp şeklindedir*ve kutusunun işaretini kaldırın **çözüm için dizin oluştur**.
    2. Seçeneği kullanılabilir durumdaysa için kutunun işaretini **projeye Application Insights Ekle**. Application Insights web uygulamanızı Azure Application Insights ile yetkilendirmek gerektirir. Bu öğreticide basit tutmak için bu işlem atlayın.
    3. **Tamam**’ı seçin.
4. Seçin **MVC** şablonu listesinden.
    1. Seçin **kimlik doğrulamayı Değiştir**, seçin **doğrulaması yok**seçeneğini belirleyip **Tamam**.
    2. Seçin **Tamam** çözümünüzü oluşturmak için.
5. İçinde **Takım Gezgini** penceresinde, seçin **değişiklikleri**.

    ![Team Services git deposuna yerel değişiklikleri](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Yürütme metin kutusuna bir ileti gibi girin *ilk yürütme*. Seçin **Commit tüm ve eşitleme** açılır menüsünden.


## <a name="create-build-definition"></a>Yapı tanımı oluşturun
Team Services içinde uygulamanızı nasıl yerleşik ana hatlarını belirlemek için derleme tanımını kullanın. Bu öğreticide, bir IIS sunucusunda web uygulamasını çalıştırmak kullanırız paketi bizim kaynak kodu çözüm oluşturur, ardından web oluşturur alır dağıtmak bir temel tanımı oluşturun.

1. Team Services projenizde seçin **yapı & yayın** üstte seçip **derlemeler**.
3. Seçin **+ yeni tanımı**.
4. Seçin **ASP.NET (Önizleme)** şablonu ve select **Uygula**.
5. Tüm varsayılan görev değerleri bırakın. Altında **alma kaynakları**, emin *mywebapp şeklindedir* depo ve *ana* şube seçilir.

    ![Yapı tanımı Team Services projesi oluşturun](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Üzerinde **Tetikleyicileri** sekmesinde, kaydırıcıyı için **Bu tetikleyici etkinleştirmek** için *etkin*.
7. Yapı tanımı ve sıra seçerek yeni bir yapı Kaydet **sıraya & Kaydet** , ardından **sıraya & Kaydet** yeniden. Seçin ve varsayılan değerleri bırakın **sıra**.

Derleme üzerinde barındırılan bir aracısı, zamanlanmış olarak izleme sonra oluşturmaya başlar. Çıktı aşağıdaki örneğe benzer:

![Team Services projesinin başarılı derleme](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Sanal makine oluşturma
ASP.NET web uygulamanızı çalıştırmak için bir platform sağlamak için IIS çalışan bir Windows sanal makine gerekir. Team Services kod tamamlama ve derlemeleri tetiklenen IIS örneği ile etkileşim kurmak için bir aracı kullanır.

Kullanarak bir Windows Server 2016 VM oluşturma [bu komut dosyası örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json). VM oluşturma ve çalıştırmak için komut dosyası için birkaç dakika sürer. VM oluşturulduktan sonra bağlantı noktası 80 web trafiği için açık [Ekle AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) gibi:

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

VM'nize bağlanmak için ortak IP adresi ile elde [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) gibi:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

VM'yi Uzak Masaüstü oturumu oluşturun:

```cmd
mstsc /v:<publicIpAddress>
```

VM, açık bir **yönetici PowerShell** komut istemi. IIS ve gerekli .NET özellikleri gibi yükleyin:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Dağıtım grubu oluşturma
Göndermek için web dağıtım paketi için IIS sunucusunu, Team Services içinde bir dağıtım grubu tanımlayın. Bu grup, hangi yeni yapılar hedef Team Services için kod tamamlama ve derlemeleri tamamlandı olarak sunucularıdır belirtmenizi sağlar.

1. Team Services içinde seçin **yapı & yayın** ve ardından **dağıtım grupları**.
2. Seçin **eklemek dağıtım grubu**.
3. Grup için bir ad girin *myIIS*seçeneğini belirleyip **oluşturma**.
4. İçinde **kaydetmek makineler** bölümünde, olun *Windows* seçilir ve ardından kutuyu **kişisel erişim belirteci komut dosyasındaki kimlik doğrulaması için kullanmak**.
5. Seçin **betik Panoya Kopyala**.


### <a name="add-iis-vm-to-the-deployment-group"></a>IIS VM dağıtım grubuna Ekle
Dağıtım grubu oluşturulan her IIS örneği grubuna ekleyin. Team Services yükler ve yeni web alan VM'de bir aracı yapılandıran bir komut dosyası oluşturur paketlerini dağıtma sonra IIS için geçerlidir.

1. Geri **yönetici PowerShell** , VM oturum yapıştırın ve Team Services kopyalanan komut dosyasını çalıştırın.
2. Aracı için etiketler yapılandırmak isteyip istemediğiniz sorulduğunda seçin *Y* ve girin *web*.
3. Kullanıcı hesabı için istendiğinde basın *dönmek* Varsayılanları kabul etmek için.
4. Komut dosyasını içeren bir ileti son bekleyin *hizmet vstsagent.account.computername başarıyla başlatıldı*.
5. İçinde **dağıtım grupları** sayfasında **yapı & yayın** menüsünde, açık *myIIS* dağıtım grubu. Üzerinde **makineler** sekmesinde, VM'yi listelenmiş olduğunu doğrulayın.

    ![VM Team Services dağıtım grubuna başarıyla eklendi](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Yayın tanımı oluşturun
Derlemeleriniz yayımlamak için bir yayın tanımı Team Services içinde oluşturun. Bu tanım, uygulamanızın başarılı bir yapı tarafından otomatik olarak başlatılır. Web göndermek için dağıtım grubu seçin paketini dağıtmak ve uygun IIS ayarlarını tanımlayın.

1. Seçin **yapı & yayın**seçeneğini belirleyip **derlemeler**. Bir önceki adımda oluşturduğunuz derleme tanımını seçin.
2. Altında **yakın zamanda tamamlandı**, en son yapı seçin ve ardından **sürüm**.
3. Seçin **Evet** bir yayın tanımı oluşturmak için.
4. Seçin **boş** şablonu, ardından **sonraki**.
5. Proje ve kaynak yapı tanımı, projenizi ile doldurulur doğrulayın.
6. Seçin **sürekli dağıtım** onay kutusunu işaretleyin ve ardından **oluşturma**.
7. Açılan kutuyu işaretleyin **+ görev ekleme** ve *bir dağıtım grubu aşaması eklemek*.
    
    ![Team Services tanımında serbest bırakmak için görev ekleme](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Seçin **Ekle** yanına **IIS Web uygulama Deploy(Preview)**seçeneğini belirleyip **Kapat**.
9. Seçin **dağıtım grubu üzerinde çalışan** üst görev.
    1. İçin **dağıtım grubu**, daha önce oluşturduğunuz dağıtım grubunu seçin gibi *myIIS*.
    2. İçinde **makine etiketleri** kutusunda **Ekle** ve *web* etiketi.
    
    ![IIS için yayın tanımı dağıtım grubu görevi](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Seçin **Dağıt: IIS Web uygulamasını dağıtmak** görev aşağıdaki gibi IIS örneği ayarlarını yapılandırmak için:
    1. İçin **Web sitesi adı**, girin *varsayılan Web sitesi*.
    2. Diğer tüm varsayılan ayarları bırakın.
12. Seçin **kaydetmek**seçeneğini belirleyip **Tamam** iki kez.


## <a name="create-a-release-and-publish"></a>Sürüm oluşturma ve yayımlama
Şimdi web gönderebilir paket yeni bir sürüm olarak dağıtın. Bu adımı, dağıtım grubunun parçası olduğundan, web iter her örneği Aracısı ile iletişim kurar paket dağıtın ve ardından güncelleştirilmiş web uygulamasını çalıştırmak üzere IIS'yi yapılandırır.

1. Yayın tanımınızı seçin **+ yayın**, ardından **oluşturma yayın**.
2. İle birlikte en son sürüme aşağı açılan listesinde seçili olduğunu doğrulayın **dağıtım otomatik: sürüm oluşturulduktan sonra**. **Oluştur**’u seçin.
3. Küçük bir başlık yayın tanımınızı üstte gibi görünen *yayın 'Sürüm 1' oluşturulan*. Yayın bağlantıyı seçin.
4. Açık **günlükleri** yayın ilerleme durumunu izlemek için sekmesi.
    
    ![Paket gönderme dağıtımı ve başarılı Team Services sürüm](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Yayın tamamlandıktan sonra bir web tarayıcısı açın ve VM ortak işlenen madde adresini girin. ASP.NET web uygulamanız çalışıyor.

    ![IIS VM üzerinde çalışan ASP.NET web uygulaması](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a>Tüm CI/CD ardışık düzen test
IIS üzerinde çalışan web uygulamanız ile tüm CI/CD ardışık düzen şimdi deneyin. Visual Studio ve güncelleştirilmiş web sürümü tetikleyen bir yapı kodunuzu tetiklenir yürütme değişikliği yaptıktan sonra paketi IIS dağıtın:

1. Visual Studio'da açın **Çözüm Gezgini** penceresi.
2. Gidin ve açmak *mywebapp şeklindedir | Görünümleri | Giriş | Index.cshtml*
3. Satır okumak için 6'yı düzenleyin:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Dosyayı kaydedin.
5. Açık **Takım Gezgini** penceresinde, seçin *mywebapp şeklindedir* proje sonra seçin **değişiklikleri**.
6. Bir kaydetme iletisi gibi girin *sınama CI/CD ardışık düzen*, ardından seçin **tamamlama tüm ve eşitleme** açılan menüsünden.
7. Team Services çalışma alanında, yeni bir yapı kod tamamlama tetiklenir. 
    - Seçin **yapı & yayın**seçeneğini belirleyip **derlemeler**. 
    - Yapı tanımı seçin ve ardından **sıraya alınan & çalışan** yapı ilerledikçe izlemek için yapı.
9. Derleme başarılı olduktan sonra yeni bir sürüm tetiklenir.
    - Seçin **yapı & yayın**, ardından **sürümleri** IIS VM'nize gönderilen paketini dağıtmak web görmek için. 
    - Seçin **yenileme** durumunu güncelleştirmek için simge. Zaman *ortamları* sütunda yeşil bir onay işareti görüntülenir, yayın IIS başarıyla dağıtıldığını.
11. Uygulanan yaptığınız değişiklikleri görmek için IIS Web sitesini bir tarayıcıda yenileyin.

    ![IIS VM'de CI/CD ardışık düzen tarafından çalıştırılan ASP.NET web uygulaması](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir ASP.NET web uygulamasını Team Services içinde oluşturulur ve yapı yapılandırılmış sürüm tanımlarını yeni web dağıtımı paketleri ve IIS her kod tamamlama dağıtabilirsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir Team Services proje için ASP.NET web uygulaması yayımlama
> * Kod tamamlama tarafından tetiklenen bir yapı tanımı oluşturun
> * IIS yükle ve azure'da bir sanal makinede Yapılandır
> * Bir dağıtım grubuna Team Services IIS örneği ekleme
> * Bir yayın oluşturmak tanımı yeni web yayımlama IIS paketlerini dağıtma
> * CI/CD ardışık düzen test

Bir SQL yükleme konusunda bilgi almak için sonraki öğretici için öncelikli&#92;IIS&#92;.NET yığında Windows VM'ler çifti.

> [!div class="nextstepaction"]
> [SQL&#92;IIS&#92;.NET yığını](tutorial-iis-sql.md)