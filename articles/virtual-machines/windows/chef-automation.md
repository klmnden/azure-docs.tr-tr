---
title: Azure sanal makine dağıtımı Chef ile | Microsoft Docs
description: Otomatik sanal makine dağıtımı ve yapılandırması Microsoft Azure üzerinde yapmak için Chef kullanmayı öğrenin
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
ms.openlocfilehash: 36293c41219a1b42d75850fa66d3c631637bb855
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Chef ile Azure sanal makine dağıtımını otomatikleştirme
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef Otomasyon teslim etmek için harika bir araçtır ve durumu yapılandırmaları istenen.

En son API bulut sürüm, Chef sağlamak ve tek bir komut yapılandırma durumlarında dağıtma olanağı verip, Azure ile sorunsuz tümleştirme sağlar.

Bu makalede, Azure sanal makineler sağlamak ve bir ilke veya "Kılavuzu" oluşturma ve ardından bu kılavuzu bir Azure sanal makine dağıtma rehberlik Chef ortamınızı ayarlayın.

Başlayalım!

## <a name="chef-basics"></a>Chef temelleri
Başlamadan önce [Chef temel kavramlarını gözden](http://www.chef.io/chef). 

Aşağıdaki diyagramda, üst düzey Chef mimari gösterilmektedir.

![][2]

Chef üç ana mimari bileşeni vardır: Chef sunucusu, Chef istemcisi (düğüm) ve Chef iş istasyonu.

Yönetim noktası Chef sunucusudur ve Chef sunucusu için iki seçenek vardır: barındırılan bir çözüm ya da bir şirket içi çözüm. Biz, barındırılan bir çözümü kullanarak.

Chef (düğüm) yönettiğiniz sunucularda bulunur Aracısı istemcidir.

Chef iş istasyonu burada biz ilkeleri oluşturun ve yönetim komutları yürütün yönetim iş istasyonu ' dir. Biz çalıştırmak **Bıçak** altyapısını yönetmek için Chef istasyonundan komutu.

"Cookbooks" ve "Tarif" kavramı yoktur. Bunlar etkili bir şekilde tanımlamak ve sunucular için geçerli ilkelerdir.

## <a name="preparing-the-workstation"></a>İş istasyonu hazırlama
İlk olarak, iş istasyonu prep olanak sağlar. Standart bir Windows iş istasyonu kullanıyorum. Biz cookbooks ve yapılandırma dosyaları depolamak için bir dizin oluşturmanız gerekir.

İlk C:\chef adlı bir dizin oluşturun.

Daha sonra c:\chef\cookbooks adlı ikinci bir dizin oluşturun.

Şimdi Chef Azure aboneliği ile iletişim kurabilmesi Azure ayarları dosyasını karşıdan yüklemek ihtiyacımız.

Karşıdan yükle, yayımlama ayarları PowerShell Azure kullanarak [Get-AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) komutu. 

Yayımlama ayarları dosyası C:\chef kaydedin.

## <a name="creating-a-managed-chef-account"></a>Yönetilen bir Chef hesap oluşturma
Barındırılan Chef hesap için kaydolabilirsiniz [burada](https://manage.chef.io/signup).

Kaydolma işlemi sırasında yeni bir kuruluş oluşturun istenir.

![][3]

Kuruluşunuz oluşturulduktan sonra başlangıç paketi yükleyin.

![][4]

> [!NOTE]
> Anahtarlarınızı sıfırlanacak uyarı bir uyarı alırsanız, henüz yapılandırılmış hiçbir mevcut altyapısına sahip olduğumuz olarak devam etmek için Tamam.
> 
> 

Bu starter kit zip dosyası kuruluş yapılandırma dosyalarını ve anahtarlarını içerir.

## <a name="configuring-the-chef-workstation"></a>Chef iş istasyonu yapılandırma
C:\chef için chef starter.zip içeriğini ayıklayın.

Chef starter\chef depodaki altındaki tüm dosyaları kopyalayın\.c:\chef dizininize chef.

Dizininizi aşağıdaki örnek gibi görünmelidir.

![][5]

Azure yayımlama dosya c:\chef kök dizininde dahil olmak üzere dört dosyaları şimdi olmalıdır.

Knife.rb dosya Bıçak yapılandırmanızı içerirken PEM dosyaları kuruluşunuz ve yönetici iletişimi için özel anahtarları içerir. Knife.rb dosyasını düzenlemeniz gerekir.

Seçim düzenleyicinizde dosyasını açın ve "cookbook_path" kaldırarak değiştirin /... / sonraki gösterildiği gibi görünmesi yolundan.

    cookbook_path  ["#{current_dir}/cookbooks"]

Ayrıca aşağıdaki ekleyin, Azure ad yansıtma satır yayımlama ayarları dosyası.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Knife.rb dosyanızı şimdi aşağıdaki örneğe benzemelidir.

![][6]

Bu satırlar Bıçak c:\chef\cookbooks cookbooks dizininde başvuruyor ve ayrıca bizim Azure yayımlama ayarları dosyası Azure işlemleri sırasında kullanır, güvence altına alır.

## <a name="installing-the-chef-development-kit"></a>Chef Geliştirme Seti yükleme
Sonraki [yükleyip](http://downloads.getchef.com/chef-dk/windows) ChefDK (Kit Chef iş istasyonunuzu ayarlamak için Chef geliştirme).

![][7]

C:\opscode varsayılan konumda yükleyin. Bu yükleme yaklaşık 10 dakika sürer.

PATH değişkeni C:\opscode\chefdk\bin için girdiler içeriyor doğrulayın; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Bunlar yoksa, bu yollar eklediğinizden emin olun!

*YOLUN SIRASI ÖNEMLİDİR UNUTMAYIN!* Opscode yolları doğru sırada değilse sorunları sahip olur.

Devam etmeden önce iş istasyonunuzu yeniden başlatın.

Ardından, sisteme Bıçak Azure uzantısı yüklenir. Bu, "Azure eklentisi" ile Bıçak sağlar.

Aşağıdaki komutu çalıştırın.

    chef gem install knife-azure ––pre

> [!NOTE]
> – Öncesi bağımsız değişkeni, en son dizi API erişim sağlayan Bıçak Azure eklentisi son RC sürümünü alma sağlar.
> 
> 

Bağımlılık sayısı da aynı anda yüklenecek olasıdır.

![][8]

Her şeyin doğru şekilde yapılandırıldığından emin olmak için aşağıdaki komutu çalıştırın.

    knife azure image list

Her şeyin doğru şekilde yapılandırıldıysa, mevcut Azure görüntüleri kaydırın listesini görürsünüz.

Tebrikler. İş istasyonu ayarlama!

## <a name="creating-a-cookbook"></a>Bir kılavuzu oluşturma
Bir kılavuzu Chef tarafından yönetilen istemci üzerinde çalıştırmak istediğiniz komut kümesini tanımlamak için kullanılır. Bir kılavuzu oluşturma basit ve kullanırız **chef oluşturmak Kılavuzu** Kılavuzu şablonu oluşturmak için komutu. IIS otomatik olarak dağıtan bir ilke ister misiniz gibi ı Kılavuzu web sunucusunu çağırma.

C:\Chef dizini altında aşağıdaki komutu çalıştırın.

    chef generate cookbook webserver

Bu C:\Chef\cookbooks\webserver dizin altında dosya kümesi oluşturur. Şimdi yönetilen sanal makineyi yürütmek için Chef istemci isteriz komut kümesini tanımlamak ihtiyacımız.

Komut dosyası default.rb depolanır. Bu dosyada, ı IIS yükler, IIS başlayıp bir şablon dosyası wwwroot klasörüne kopyalar komut kümesini tanımlama.

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

Tamamladıktan sonra dosyayı kaydedin.

## <a name="creating-a-template"></a>Şablon oluşturma
Daha önce belirtildiği gibi default.html sayfası olarak kullanılacak bir şablon dosyası oluşturmak gerekir.

Şablon oluşturmak için aşağıdaki komutu çalıştırın.

    chef generate template webserver Default.htm

Şimdi C:\chef\cookbooks\webserver\templates\default\Default.htm.erb dosyasına gidin. Bazı basit "Hello World" HTML kod ekleyerek dosyasını düzenleyin ve ardından dosyayı kaydedin.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Kılavuzu Chef sunucusuna yükleyin
Bu adımda, biz yerel makinede oluşturduk Kılavuzu bir kopyasını almak ve Chef barındırılan sunucusuna yükleniyor. Karşıya sonra Kılavuzu altında görünür **İlkesi** sekmesi.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Bir sanal makine Bıçak Azure ile dağıtma
Biz şimdi bir Azure sanal makinesi dağıtır ve IIS web hizmeti ve varsayılan web sayfası yükleyecek "Web" kılavuzu uygulayın.

Bunu yapmak için kullanılması **Bıçak azure sunucusu oluşturmak** komutu.

Örnek komut, ardından görüntülenen istiyorum.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametreleri kendinden açıklamalıdır. Belirli değişkenleri değiştirin ve çalıştırın.

> [!NOTE]
> Üzerinden komut satırı ı de uç nokta ağ filtre kuralları – tcp uç noktaları parametresini kullanarak otomatikleştirme. Yukarı bağlantı noktası 80 ve my web sayfası ve RDP oturumu erişim sağlamak için 3389 açmış olduğunuz.
> 
> 

Komutu çalıştırdıktan sonra Azure portalına gidin ve sağlamak başlamak makinenizi görürsünüz.

![][13]

Komut istemi sonraki görüntülenir.

![][10]

Dağıtım tamamlandıktan sonra biz biz Bıçak Azure komutu ile sanal makine sağladığında bağlantı noktası açıldı gibi web hizmetine bağlantı noktası 80 üzerinden bağlanabiliyor olmalıdır. Bu sanal makine yalnızca bir sanal makine my bulut hizmetinde olduğu gibi ı bulut hizmeti url ile bağlanırsınız.

![][11]

Gördüğünüz gibi HTML kodumu yaratıcı aldım.

Biz de 3389 numaralı bağlantı noktası aracılığıyla Azure portalından bir RDP oturumu aracılığıyla bağlanabilirsiniz unutmayın.

Bu yardımcı olmuştur ı umuyoruz! Git ve altyapınızı Azure ile kod gezisine olarak bugün başlayın!

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
