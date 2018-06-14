---
title: Bir Linux VM üzerinde rayları Web sitesinde bir Ruby konak | Microsoft Docs
description: Ayarlama ve Ruby kullanarak bir Linux sanal makine azure'da rayları tabanlı Web sitesinde ana bilgisayar.
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: fa19f3dc7dded712102d4ba9b66dd4df1bfd20dd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29397606"
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Azure VM’de Ruby on Rails Web uygulaması
Bu öğretici, Linux sanal makine kullanarak azure'da rayları Web sitesinde bir Ruby barındırmak nasıl gösterir.  

Bu öğretici, Ubuntu Server 14.04 LTS kullanılarak doğrulandı. Başka bir Linux dağıtım kullanırsanız, rayları yüklemek için adımları değiştirmeniz gerekebilir.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md).  Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]
>

## <a name="create-an-azure-vm"></a>Bir Azure VM oluşturma
Bir Linux görüntüsüyle bir Azure VM oluşturarak başlayın.

VM oluşturmak için Azure portalında veya Azure komut satırı arabirimi (CLI) kullanabilirsiniz.

### <a name="azure-portal"></a>Azure portalına
1. Oturum [Azure portalı](https://portal.azure.com)
2. Tıklatın **kaynak oluşturma**, arama kutusuna "Ubuntu Server 14.04" yazın. Araması tarafından döndürülen girdiyi tıklatın. Dağıtım modelini seçin **Klasik**, "Oluştur" seçeneğine tıklayın.
3. Temel bilgiler dikey penceresinde gerekli alanlar için değerleri girin: adı (VM), kullanıcı adı, kimlik doğrulama türü ve ilgili kimlik bilgileri, Azure abonelik, kaynak grubunu ve konumu.

   ![Yeni bir Ubuntu görüntüsü oluşturma](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. VM sağlandıktan sonra VM adına tıklayın ve tıklayın **uç noktaları** içinde **ayarları** kategorisi. Altında listelenen SSH uç nokta bulunamadı **tek başına**.

   ![Varsayılan uç noktası](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure CLI
Adımları [çalıştıran bir sanal makine Linux oluşturma][vm-instructions].

VM sağlandıktan sonra aşağıdaki komutu çalıştırarak SSH uç noktası alabilirsiniz:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Ruby rayları üzerinde yükle
1. SSH VM'e bağlanmak için kullanın.
2. SSH oturumundan Ruby VM yüklemek için aşağıdaki komutları kullanın:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > The brightbox repository contains the current Ruby distribution.

    Yükleme birkaç dakika sürebilir. Tamamlandığında, Ruby yüklendiğini doğrulamak için aşağıdaki komutu kullanın:

        ruby -v

3. Rayları yüklemek için aşağıdaki komutu kullanın:

        sudo gem install rails --no-rdoc --no-ri -V

    Kullanımı daha hızlıdır belgeleri yükleme atlamak için yok rdoc ve--Hayır RI bayrakları.
    -V ekleme yükleme ilerleme durumu hakkında bilgi görüntüler bu komut büyük olasılıkla yürütmek için uzun bir süre gidersiniz.

## <a name="create-and-run-an-app"></a>Oluşturma ve bir uygulamayı çalıştırma
SSH yoluyla hala oturum açmış olsa da, aşağıdaki komutları çalıştırın:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

[Yeni](http://guides.rubyonrails.org/command_line.html#rails-new) komut yeni bir rayları uygulaması oluşturur. [Server](http://guides.rubyonrails.org/command_line.html#rails-server) komut rayları ile birlikte gelen WEBrick web sunucusu başlatır. (Üretim kullanımı için büyük olasılıkla Unicorn veya yolcu gibi farklı bir sunucu kullanmak istediğiniz.)

Aşağıdakine benzer bir çıktı görmeniz gerekir.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Bir uç nokta ekleme
1. [Azure portalı] gidin [https://portal.azure.com] ve VM'yi seçin.

2. Seçin **uç noktaları** içinde **ayarları** sayfa sol kenar.

3. Tıklatın **ekleme** sayfanın üst kısmındaki.

4. İçinde **uç nokta ekleme** iletişim sayfasında, aşağıdaki bilgileri girin:

   * **Ad**: HTTP
   * **Protocol**: TCP
   * **Genel bağlantı noktası**: 80
   * **Özel bağlantı noktası**: 3000
   * **PI adresi kayan**: devre dışı
   * **Erişim denetimi listesi - sipariş**: Bu erişim kuralı önceliği ayarlar 1001 ya da başka bir değer.
   * **Erişim denetimi listesi - adı**: allowHTTP
   * **Erişim denetimi listesi - eylem**: izin ver
   * **Erişim denetimi listesi - uzak alt**: 1.0.0.0/16

     Bu uç noktaya trafiğini burada rayları sunucu dinliyor özel bağlantı noktası 3000 için yönlendirecek 80 genel bir bağlantı noktası var. Erişim denetimi listesi kuralı bağlantı noktası 80 üzerinde ortak trafiğine izin verir.

     ![Yeni uç noktası](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Uç nokta kaydetmek için Tamam'ı tıklatın.

6. Bildiren bir ileti görüntülenmelidir **sanal makine uç noktası kaydediliyor**. Bu ileti kaybolur sonra uç nokta etkindir. Şimdi, sanal makinenin DNS adı giderek uygulamanızı test. Web sitesi aşağıdakine benzer görünmelidir:

    ![Varsayılan rayları sayfası][default-rails-cloud]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide adımları çoğunu el ile yaptınız. Bir üretim ortamında, uygulama geliştirme makinenizde yazmak ve Azure VM'ye dağıtın. Ayrıca, çoğu üretim ortamlarında Apache veya yürüten rayları uygulamasının birden çok örneği Yönlendirme ve statik kaynakları hizmet isteği NginX gibi başka bir sunucu işlemi ile birlikte rayları uygulama ana bilgisayar. Daha fazla bilgi için http://rubyonrails.org/deploy/ bakın.

Ruby rayları üzerinde hakkında daha fazla bilgi için [rayları kılavuzları üzerinde Söyleniş][rails-guides].

Söyleniş uygulamanızdan Azure hizmetleri kullanmak için bkz:

* [BLOB'ları kullanarak yapılandırılmamış veri depolayın][blobs]
* [Mağaza anahtar/değer çiftlerinin tabloları kullanma][tables]
* [Yüksek bant genişliği içeriğin bulunduğu içerik teslim ağı][cdn-howto]

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal-classic.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
