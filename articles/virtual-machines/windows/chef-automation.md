---
title: Chef ile Azure sanal makine dağıtımı | Microsoft Docs
description: Otomatik sanal makine dağıtımı ve yapılandırması Microsoft Azure üzerinde Chef kullanmayı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: gwallace
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 07/09/2019
ms.author: diviso
ms.openlocfilehash: 74b92c277b1d6eaa0984e55a70459bad59c2bf84
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719283"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Chef ile Azure sanal makine dağıtımını otomatikleştirme

Chef, Otomasyon sunmaya yönelik harika bir araçtır ve istenen durum yapılandırmaları.

En son bulut API sürümüyle, Chef, sağlama ve yapılandırma durumları tek bir komut ile dağıtım olanağı sağlayan Azure ile sorunsuz tümleştirme sağlar.

Bu makalede, Azure sanal makineleri sağlayın ve rehberlik bir ilke veya "Kılavuzu" oluşturma ve ardından bir Azure sanal makinesi için bu kılavuzu dağıtmak için Chef ortamınızı ayarlayın.

## <a name="chef-basics"></a>Chef temelleri
Başlamadan önce [Chef temel kavramlarını gözden](https://www.chef.io/chef).

Aşağıdaki diyagramda, üst düzey Chef mimarisi gösterilmektedir.

![][2]

Chef, üç ana mimari bileşeni vardır: Chef sunucusu, Chef istemci (node) ve Chef iş istasyonu.

Chef sunucusu yönetim noktasıdır ve Chef sunucu için iki seçenek vardır: barındırılan bir çözüm ya da şirket içi bir çözüm.

Chef istemci (node) yönettiğiniz sunucularında yer alan aracısıdır.

Chef adı için her iki yönetim iş istasyonu, burada ilkeleri oluşturun ve yönetimi komutları ve Chef Araçları'nın yazılım paketi yürütmek iş istasyonu.

Genellikle, gördüğünüz _istasyonunuzu_ eylemler gerçekleştirmek burada konumu olarak ve _Chef iş istasyonu_ yazılım paketi için.
Örneğin, bir parçası olarak Bıçağı komutu indirirsiniz _Chef iş istasyonu_, Bıçak komutları çalıştırmanız _istasyonunuzu_ altyapısını yönetme.

Chef, ayrıca "Tarif kitapları" ve etkili bir şekilde ilkeler tanımlayın ve sunucular için geçerli olan "tarif" kullanır.

## <a name="preparing-your-workstation"></a>İş istasyonunuzu hazırlama

İlk olarak, Chef yapılandırma dosyalarını ve tarif kitapları depolamak için bir dizin oluşturarak iş istasyonunuzu hazırlama.

C:\Chef adlı bir dizin oluşturun.

En son yükleyip [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sürüm istasyonunuzu açın.

## <a name="configure-azure-service-principal"></a>Azure hizmet sorumlusunu yapılandırma

İçinde bir hizmet koşulları ve Azure hizmet sorumlusu basit hesabıdır.   Bizim Chef iş istasyonundan Azure kaynaklarını oluşturmanıza yardımcı olmak için size bir hizmet sorumlusu kullanıyor olacaksınız.  Gerekli izinlerle ilgili hizmet sorumlusu oluşturmak için şu PowerShell içinde aşağıdaki komutları çalıştırmanız gerekir:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Lütfen bir Subscriptionıd, Tenantıd, ClientID ve istemci gizli anahtarı (yukarıda belirlenen parola) not edin, bu daha sonra ihtiyacınız olacak. 

## <a name="setup-chef-server"></a>Chef Sunucusu Kurulumu

Bu kılavuzda, barındırılan Chef için abone, varsayılmaktadır.

Chef sunucusu zaten kullanıyorsanız, şunları yapabilirsiniz:

* Kaydolun [barındırılan Chef](https://manage.chef.io/signup), Chef ile başlamanın en hızlı yolu olduğu.
* Tek başına Chef sunucusu aşağıdaki linux tabanlı makinesine SCVMM yüklemek [yükleme yönergeleri](https://docs.chef.io/install_server.html) gelen [Chef belgeleri](https://docs.chef.io/).

### <a name="creating-a-hosted-chef-account"></a>Chef barındırılan hesabı oluşturma

Chef barındırılan hesap için kaydolun [burada](https://manage.chef.io/signup).

Kayıt işlemi sırasında yeni bir kuruluş oluşturun istenir.

![][3]

Kuruluşunuz oluşturulduktan sonra Başlangıç setini indirin.

![][4]

> [!NOTE]
> Anahtarlarınızı sıfırlanacak uyarı bir istem alırsanız, sahip olduğumuz olarak henüz yapılandırılmamış olan herhangi bir mevcut altyapı olarak devam etmek uygundur.
>

Bu başlangıç Seti zip dosyası kuruluş yapılandırma dosyalarını ve kullanıcı anahtarı içeren `.chef` dizin.

`organization-validator.pem` Ayrı ayrı, özel anahtarı olduğundan ve özel anahtarları Şef sunucusunda değil depolanmalıdır indirilmelidir. Gelen [Chef yönetme](https://manage.chef.io/)Yönetim bölümüne gidin ve "Sıfırlama doğrulama ayrı olarak indirmek dosyayı sağlayan anahtarı" seçin. Dosyayı c:\chef için kaydedin.

### <a name="configuring-your-chef-workstation"></a>Chef istasyonunuzu yapılandırma

C:\chef için chef starter.zip içeriğini ayıklayın.

Chef starter\chef deposu altındaki tüm dosyaları kopyalayın\.c:\chef dizininize chef.

Kopyalama `organization-validator.pem` c:\Downloads kaydedilmişse c:\chef için dosya

Dizininizi aşağıdaki örnek gibi görünmelidir.

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

Artık c:\chef kökündeki beş dosya ve dört dizinleri (boş bir chef deposu dizini dahil) olmalıdır.

### <a name="edit-kniferb"></a>Edit knife.rb

PEM dosyaları kuruluşunuz ve iletişim için yönetim özel anahtarları içeren ve Bıçak yapılandırmanızı knife.rb dosyası içerir. Knife.rb dosyasını düzenlemeniz gerekir.

Knife.rb dosyayı dilediğiniz düzenleyicide açın. Değiştirilmemiş dosya gibi görünmelidir:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Aşağıdaki bilgiler için knife.rb ekleyin:

validation_client_name   "myorg-validator"

validation_key           "#{current_dir}/myorg.pem"

knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"

knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"

knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"

knife[:azure_client_secret] =     "#1234p$wdchef19"


Bu satırlar Bıçak c:\chef\cookbooks altında tarif kitapları dizine başvurur ve ayrıca Azure işlemleri sırasında oluşturulan Azure hizmet sorumlusu kullanan sağlayacaktır.

Knife.rb dosyanız artık aşağıdaki örneğe benzer görünmelidir:

![][14]

<!--- Giant problem with this section: Chef 12 uses a config.rb instead of knife.rb
// However, the starter kit hasn't been updated
// So, I don't think this will even work on the modern Chef -->

<!--- update image [6] knife.rb -->

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Chef iş istasyonu yükleyin

Ardından, [yükleyip](https://downloads.chef.io/chef-workstation/) Chef iş istasyonu.
Chef iş istasyonunun varsayılan konumu yükleyin. Bu yükleme birkaç dakika sürebilir.

Masaüstünde, bir "FA Chef ürünlerle etkileşime geçmek için gerekir aracı ile yüklenen bir ortam olan PowerShell" görürsünüz. FA PowerShell yeni geçici komutlar gibi kullanılabilir hale getirir `chef-run` gibi de Geleneksel Chef CLI komutları `chef`. Chef iş istasyonu ve Chef Araçlar'ın yüklü sürümü görmek `chef -v`. Chef iş istasyonu uygulamadan "Hakkında Chef iş istasyonu"'i seçerek, iş istasyonu sürümünüzü kontrol edebilirsiniz.

`chef --version` şunun gibi döndürülmesi gerekir:

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> Yolun sırası önemlidir! Değilse, opscode yolları doğru sırayla sorunları olacaktır.
>

Devam etmeden önce iş istasyonunuzu yeniden başlatın.

### <a name="install-knife-azure"></a>Bıçak Azure'ı yükleme

Bu öğreticide, sanal makineyle etkileşim kurmak için Azure Resource Manager kullanmakta olduğunuz varsayılır.

Bıçak Azure uzantıyı yükleyin. Bu, "Azure eklentisiyle" Bıçak sağlar.

Aşağıdaki komutu çalıştırın.

    chef gem install knife-azure ––pre

> [!NOTE]
> En son dizi API'si erişim sağlayan Bıçak Azure eklentisi en son RC sürümünü alıyorsunuz – öncesi bağımsız değişkeni sağlar.
>
>

Bağımlılık sayısı da aynı anda yüklenecek olasıdır.

![][8]

Her şeyin doğru şekilde yapılandırıldığından emin olmak için aşağıdaki komutu çalıştırın.

    knife azurerm server list

Her şeyin doğru şekilde yapılandırıldıysa, kullanılabilir Azure görüntüleri kaydırın listesini görürsünüz.

Tebrikler. İş istasyonunuzu ayarlayın!

## <a name="creating-a-cookbook"></a>Kılavuzu oluşturma

Kılavuzu Chef tarafından yönetilen istemcinizdeki yürütmek istediğiniz komutları kümesini tanımlamak için kullanılır. Kılavuzu oluşturma basit, tek **chef Oluşturma Kılavuzu** Kitapçığı şablonu oluşturmak için komutu. Bu kılavuzu IIS otomatik olarak dağıtan bir web sunucusu için ' dir.

C:\Chef dizininiz altında aşağıdaki komutu çalıştırın.

    chef generate cookbook webserver

Bu komut bir dizi C:\Chef\cookbooks\webserver dizin altında dosya oluşturur. Ardından, Chef istemcisi yönetilen bir sanal makinede yürütülecek komut kümesini tanımlar.

Komut dosyası default.rb içinde depolanır. Bu dosyada, IIS yükler, IIS başlayıp wwwroot klasörü için bir şablon dosyası kopyalar komut kümesini tanımlar.

C:\chef\cookbooks\webserver\recipes\default.rb dosyasını değiştirin ve aşağıdaki satırları ekleyin.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

İşiniz bittiğinde dosyayı kaydedin.

## <a name="creating-a-template"></a>Şablon oluşturma
Bu adımda, default.html sayfası olarak kullanılan için bir şablon dosyası oluşturacaksınız.

Şablon oluşturmak için aşağıdaki komutu çalıştırın:

    chef generate template webserver Default.htm

Gidin `C:\chef\cookbooks\webserver\templates\default\Default.htm.erb` dosya. Bazı basit bir "Merhaba Dünya" HTML kod ekleyerek dosyasını düzenleyin ve ardından dosyayı kaydedin.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Kılavuzu Chef sunucuya yükleyin.
Bu adımda, bir kopyasını Kılavuzu, yerel makinede oluşturdunuz ve Chef barındırılan sunucuya yükleyin oluşturun. Karşıya sonra Kitapçığı altında görünür **ilke** sekmesi.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Bıçak Azure ile bir sanal makine dağıtma
Bir Azure sanal makinesine dağıtın ve IIS web hizmeti ve varsayılan web sayfasını yükleyen "Web sunucusu" kılavuzu uygulayın.

Bunu yapmak için kullanmanız **Bıçak azurerm sunucusu oluşturma** komutu.

Örnek komut, sonraki görünür.

    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"


Yukarıdaki örnekte, Batı ABD bölgesinde yüklü Windows Server 2016 ile Standard_DS2_v2 sanal makine oluşturacaksınız. Belirli değişkenleri değiştirin ve çalıştırın.

> [!NOTE]
> Komut satırı aracılığıyla ben ayrıca uç ağ filtresi kuralları – tcp uç noktaları parametresini kullanarak otomatikleştirme. 80 ve web sayfası ve RDP oturumu erişim sağlamak için 3389 numaralı bağlantı noktalarını açık.
>
>

Komutu çalıştırdıktan sonra makinenizin sağlamak başlamak görmek için Azure portalına gidin.

![][15]

Komut istemi görünür.

![][16]

Dağıtım tamamlandıktan sonra dağıtım tamamlandığında yeni bir sanal makinenin genel IP adresi görüntülenir, bu kopyalayabilir ve bir web tarayıcısına yapıştırın ve dağıttığınız Web sitesini görüntülemek. Şu sanal makine dağıtılırken dışarıdan kullanılabilir olması, böylece biz 80 numaralı bağlantı noktasını açıldı.   

![][11]

Bu örnek, creative HTML kodu kullanır.

Ayrıca düğümün durumu görüntüleyebilirsiniz [Chef yönetme](https://manage.chef.io/). 

![][17]

Bağlantı noktası 3389 üzerinden Azure portalından bir RDP oturumu aracılığıyla bağlanabilirsiniz unutmayın.

Teşekkür ederiz! Git ve altyapınızı Azure ile kod yolculuğu olarak bugün başlayın!

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png
[14]: media/chef-automation/14.png
[15]: media/chef-automation/15.png
[16]: media/chef-automation/16.png
[17]: media/chef-automation/17.png


<!--Link references-->
