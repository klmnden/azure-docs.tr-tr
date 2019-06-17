---
title: Chef ile Azure sanal makine dağıtımı | Microsoft Docs
description: Otomatik sanal makine dağıtımı ve yapılandırması Microsoft Azure üzerinde Chef kullanmayı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: jeconnoc
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: 9cb7172fb529d8f0cd8650db7c06a78176ef342d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64729543"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Chef ile Azure sanal makine dağıtımını otomatikleştirme
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

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

C:\chef adlı bir dizin oluşturun.

Azure PowerShell indirme [yayımlama ayarları](https://docs.microsoft.com/dynamics-nav/how-to--download-and-import-publish-settings-and-subscription-information).

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

`organization-validator.pem` Ayrı ayrı, özel anahtarı olduğundan ve özel anahtarları Şef sunucusunda değil depolanmalıdır indirilmelidir. Gelen [Chef yönetme](https://manage.chef.io/) ve "Sıfırlama doğrulama ayrı olarak indirmek dosyayı sağlayan anahtarı"'i seçin. Dosyayı c:\chef için kaydedin.

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

Ayrıca aşağıdakileri ekleyerek Azure adını yansıtacak şekilde satırı yayımlama ayarları dosyası.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Kaldırarak "cookbook_path" değiştirmek /... / olarak görünecek şekilde yolu:

    cookbook_path  ["#{current_dir}/cookbooks"]

Bu satırlar Bıçak c:\chef\cookbooks altında tarif kitapları dizine başvurur ve ayrıca Azure işlemleri sırasında bizim Azure yayımlama ayarları dosyasını kullanır sağlayacaktır.

Knife.rb dosyanız artık aşağıdaki örneğe benzer görünmelidir:

![][6]

<!--- Giant problem with this section: Chef 12 uses a config.rb instead of knife.rb
// However, the starter kit hasn't been updated
// So, I don't think this will even work on the modern Chef -->

<!--- update image [6] knife.rb -->

```rb
knife.rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "mynode"
client_key               "#{current_dir}/user.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
validation_client_name   "myorg-validator"
validation_key           ""#{current_dir}/myorg.pem"
cookbook_path            ["#{current_dir}/cookbooks"]
knife[:azure_publish_settings_file] = "yourfilename.publishsettings"
```

## <a name="install-chef-workstation"></a>Chef iş istasyonu yükleyin

Ardından, [yükleyip](https://downloads.chef.io/chef-workstation/) Chef iş istasyonu.
Chef iş istasyonunun varsayılan konumu yükleyin. Bu yükleme birkaç dakika sürebilir.

Masaüstünde, bir "FA Chef ürünlerle etkileşime geçmek için gerekir aracı ile yüklenen bir ortam olan PowerShell" görürsünüz. FA PowerShell yeni geçici komutlar gibi kullanılabilir hale getirir `chef-run` gibi de Geleneksel Chef CLI komutları `chef`. Chef iş istasyonu ve Chef Araçlar'ın yüklü sürümü görmek `chef -v`. Chef iş istasyonu uygulamadan "Hakkında Chef iş istasyonu"'i seçerek, iş istasyonu sürümünüzü kontrol edebilirsiniz.

`chef --version` şunun gibi döndürülmesi gerekir:

```
Chef Workstation: 0.2.29
  chef-run: 0.2.2
  Chef Client: 14.6.47x
  delivery-cli: master (6862f27aba89109a9630f0b6c6798efec56b4efe)
  berks: 7.0.6
  test-kitchen: 1.23.2
  inspec: 3.0.12
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

    knife azure image list

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

Bunu yapmak için kullanmanız **Bıçak azure sunucusu oluşturma** komutu.

Örnek komut, sonraki görünür.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametreler büyük/küçük harf açıklamalıdır. Belirli değişkenleri değiştirin ve çalıştırın.

> [!NOTE]
> Komut satırı aracılığıyla ben ayrıca uç ağ filtresi kuralları – tcp uç noktaları parametresini kullanarak otomatikleştirme. 80 ve benim bir web sayfası ve RDP oturumu erişim sağlamak için 3389 numaralı bağlantı noktalarını açık.
>
>

Komutu çalıştırdıktan sonra makinenizin sağlamak başlamak görmek için Azure portalına gidin.

![][13]

Komut istemi görünür.

![][10]

Dağıtım tamamlandıktan sonra Bıçak Azure komutu ile sanal makine sağladığında bağlantı noktasının açık olduğundan, web hizmetine bağlantı noktası 80 üzerinden bağlanabilir olmalıdır. Bu sanal makine yalnızca sanal makineyi bu bulut hizmetinde olduğu gibi bulut hizmeti url ile bağlanabilir.

![][11]

Bu örnek, creative HTML kodu kullanır.

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


<!--Link references-->
