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
ms.openlocfilehash: 3a6fbc8410dbc5aec4522f0972a29c67527edb23
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42059526"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Chef ile Azure sanal makine dağıtımını otomatikleştirme
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef, Otomasyon sunmaya yönelik harika bir araçtır ve istenen durum yapılandırmaları.

En son bulut API Sürüm, Chef, sağlama ve yapılandırma durumları tek bir komut ile dağıtım olanağı sağlayan Azure ile sorunsuz tümleştirme sağlar.

Bu makalede, Azure sanal makineleri sağlayın ve rehberlik bir ilke veya "Kılavuzu" oluşturma ve ardından bir Azure sanal makinesi için bu kılavuzu dağıtmak için Chef ortamınızı ayarlayın.

Haydi başlayalım!

## <a name="chef-basics"></a>Chef temelleri
Başlamadan önce [Chef temel kavramlarını gözden](http://www.chef.io/chef). 

Aşağıdaki diyagramda, üst düzey Chef mimarisi gösterilmektedir.

![][2]

Chef, üç ana mimari bileşeni vardır: Chef sunucusu, Chef istemci (node) ve Chef iş istasyonu.

Chef sunucusu yönetim noktasıdır ve Chef sunucu için iki seçenek vardır: barındırılan bir çözüm ya da şirket içi bir çözüm. Biz bir barındırılan çözümün kullanacaklardır.

Chef istemci (node) yönettiğiniz sunucularında yer alan aracısıdır.

Chef iş istasyonu yönetim istasyonudur burada ilkeleri oluşturma ve yönetim komutları yürütün. Biz çalıştırma **Bıçak** altyapısını yönetmek için Chef istasyonundan komutu.

"Tarif kitapları" ve "Tarif" kavramı yoktur. Bunlar etkili bir şekilde ilkeler tanımlayın ve sunucular için geçerli olur.

## <a name="preparing-the-workstation"></a>İş istasyonu hazırlama
İlk olarak, iş istasyonu hazırlama olanak tanır. Standart bir Windows iş istasyonu kullanıyorum. Biz tarif kitapları ve yapılandırma dosyalarını depolamak için bir dizin oluşturmanız gerekir.

İlk C:\chef adlı bir dizin oluşturun.

Ardından c:\chef\cookbooks adlı ikinci bir dizin oluşturun.

Biz artık Chef ile Azure aboneliğini iletişim kurabilmesi Azure ayarları dosyasını indirmeniz gerekir.

İndirme, yayımlama ayarları PowerShell Azure kullanarak [Get-AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) komutu. 

Yayımlama ayarları dosyası içinde C:\chef kaydedin.

## <a name="creating-a-managed-chef-account"></a>Yönetilen bir Chef hesap oluşturma
Barındırılan bir Chef hesabı için kaydolun [burada](https://manage.chef.io/signup).

Kaydolma işlemi sırasında yeni bir kuruluş oluşturun istenir.

![][3]

Kuruluşunuz oluşturulduktan sonra Başlangıç setini indirin.

![][4]

> [!NOTE]
> Anahtarlarınızı sıfırlanacak uyarı istemi alır, sahip olduğumuz olarak henüz yapılandırılmamış olan herhangi bir mevcut altyapı olarak devam etmek için Tamam demektir.
> 
> 

Bu başlangıç Seti zip dosyası, kuruluş yapılandırma dosyaları ve anahtarları içerir.

## <a name="configuring-the-chef-workstation"></a>Chef iş istasyonu yapılandırma
C:\chef için chef starter.zip içeriğini ayıklayın.

Chef starter\chef deposu altındaki tüm dosyaları kopyalayın\.c:\chef dizininize chef.

Dizininizi aşağıdaki örnek gibi görünmelidir.

![][5]

Şimdi Azure yayımlama dosya c:\chef kökündeki dahil olmak üzere dört dosyaları olmalıdır.

Knife.rb dosya Bıçak yapılandırmanızı içerirken PEM dosyalar kuruluşunuz ve yönetici iletişim özel anahtarı içerir. Knife.rb dosyasını düzenlemeniz gerekir.

Tercih ettiğiniz düzenleyicide dosyayı açıp kaldırarak "cookbook_path" değiştirmek /... / sonraki gösterildiği gibi görünecek şekilde yolu.

    cookbook_path  ["#{current_dir}/cookbooks"]

Ayrıca aşağıdakileri ekleyerek Azure adını yansıtacak şekilde satırı yayımlama ayarları dosyası.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Knife.rb dosyanız artık aşağıdaki örneğe benzer görünmelidir.

![][6]

Bu satırlar Bıçak c:\chef\cookbooks altında tarif kitapları dizine başvurur ve ayrıca Azure işlemleri sırasında bizim Azure yayımlama ayarları dosyasını kullanır sağlayacaktır.

## <a name="installing-the-chef-development-kit"></a>Chef Geliştirme Seti yükleniyor
Sonraki [yükleyip](http://downloads.getchef.com/chef-dk/windows) ChefDK (Seti Chef iş istasyonunuzu ayarlamak için Chef geliştirme).

![][7]

C:\opscode varsayılan konuma yükler. Bu yükleme yaklaşık 10 dakika sürer.

YOL değişkeninize C:\opscode\chefdk\bin için girişler doğrulayın; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Bunlar yoksa, bu yollar eklediğinizden emin olun!

*YOLUN SIRALAMASI ÖNEM TAŞIMAKTADIR UNUTMAYIN!* Değilse, opscode yolları doğru sırayla sorunları olacaktır.

Devam etmeden önce iş istasyonunuzu yeniden başlatın.

Ardından, Bıçak Azure uzantısı yüklenir. Bu, "Azure eklentisiyle" Bıçak sağlar.

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

Tebrikler. İş istasyonu ayarlama!

## <a name="creating-a-cookbook"></a>Kılavuzu oluşturma
Kılavuzu Chef tarafından yönetilen istemcinizdeki yürütmek istediğiniz komutları kümesini tanımlamak için kullanılır. Kılavuzu oluşturmak kolaydır ve kullandığımız **chef Oluşturma Kılavuzu** Kitapçığı şablonu oluşturmak için komutu. IIS otomatik olarak dağıtan bir ilkedir istiyorum gibi miyim Kılavuzu web sunucusunu çağırma.

C:\Chef dizininiz altında aşağıdaki komutu çalıştırın.

    chef generate cookbook webserver

Bu C:\Chef\cookbooks\webserver dizin altında dosya kümesi oluşturur. Şimdi yönetilen bir sanal makine üzerinde yürütmek için Chef istemci istiyoruz komutları kümesini tanımlamak ihtiyacımız var.

Komut dosyası default.rb içinde depolanır. Bu dosyada, ben IIS yükler, IIS başlayıp wwwroot klasörü için bir şablon dosyası kopyalar komut kümesini tanımlama.

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
Daha önce de belirttiğimiz gibi biz default.html sayfa olarak kullanılacak bir şablon dosyası oluşturmanız gerekir.

Şablon oluşturmak için aşağıdaki komutu çalıştırın.

    chef generate template webserver Default.htm

Artık C:\chef\cookbooks\webserver\templates\default\Default.htm.erb dosyasına gidin. Bazı basit bir "Merhaba Dünya" HTML kod ekleyerek dosyasını düzenleyin ve ardından dosyayı kaydedin.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Kılavuzu Chef sunucuya yükleyin.
Bu adımda, biz yerel makinede oluşturduk Kitapçığı bir kopyasını almak ve Chef barındırılan sunucuya yükleniyor. Karşıya sonra Kitapçığı altında görünür **ilke** sekmesi.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Bıçak Azure ile bir sanal makine dağıtma
Artık bir Azure sanal makinesi dağıtma eder ve IIS web hizmeti ve varsayılan web sayfasını yükleyecektir "Web sunucusu" kılavuzu uygulayın.

Bunu yapmak için kullanmanız **Bıçak azure sunucusu oluşturma** komutu.

Örnek komut, ardından görüntülenen yaşıyorum.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametreler büyük/küçük harf açıklamalıdır. Belirli değişkenleri değiştirin ve çalıştırın.

> [!NOTE]
> Komut satırı aracılığıyla ben ayrıca uç ağ filtresi kuralları – tcp uç noktaları parametresini kullanarak otomatikleştirme. 80 ve benim bir web sayfası ve RDP oturumu erişim sağlamak için 3389 numaralı bağlantı noktalarını açık.
> 
> 

Komutu çalıştırdıktan sonra Azure portalına gidin ve sağlamayı başlamak makinenize görürsünüz.

![][13]

Komut istemi görünür.

![][10]

Dağıtım tamamlandıktan sonra biz size Bıçak Azure komutu ile sanal makine sağladığında bağlantı noktası açmış olsaydık gibi web hizmetine bağlantı noktası 80 üzerinden bağlanabilir olmalıdır. Bu sanal makine bulut hizmetimde yalnızca sanal makineyi olduğu gibi ben bulut hizmeti url ile bağlanırsınız.

![][11]

Gördüğünüz gibi HTML kodum ile yaratıcı aldım.

Biz de 3389 numaralı bağlantı noktası üzerinden Azure portalından bir RDP oturumu aracılığıyla bağlanabilirsiniz unutmayın.

Bu yardımcı olmuştur umarım! Git ve altyapınızı Azure ile kod yolculuğu olarak bugün başlayın!

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
