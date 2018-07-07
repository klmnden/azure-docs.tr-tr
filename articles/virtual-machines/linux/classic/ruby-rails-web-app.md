---
title: Ruby on Rails, bir Linux VM'de Web sitesi barındırma | Microsoft Docs
description: Ayarlanmış ve Ruby on Rails tabanlı bir Web sitesinde bir Linux sanal makinesini kullanarak azure'da barındırın.
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
ms.openlocfilehash: 6ea1d249b7f9aec3a45923b162a97ce7f83d0d31
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901162"
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Azure VM’de Ruby on Rails Web uygulaması
Bu öğretici, bir Linux sanal makinesi kullanarak Ruby on Rails Web sitesi azure'da barındırmak nasıl gösterir.  

Bu öğreticide, Ubuntu Server 14.04 LTS kullanılarak doğrulandı. Farklı bir Linux dağıtımı kullanıyorsanız, Rails yüklemek için adımları değiştirmeniz gerekebilir.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md).  Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]
>

## <a name="create-an-azure-vm"></a>Bir Azure VM oluşturma
Bir Linux görüntüsü ile bir Azure VM oluşturarak başlayın.

VM oluşturmak için Azure portal veya Azure komut satırı arabirimi (CLI) kullanabilirsiniz.

### <a name="azure-portal"></a>Azure portalına
1. Oturum [Azure portalı](https://portal.azure.com)
2. Tıklayın **kaynak Oluştur**, arama kutusuna "Ubuntu Server 14.04" yazın. Arama sonucunda döndürülen girişe tıklayın. Dağıtım modeli için seçin **Klasik**, ardından "Oluştur" a tıklayın.
3. Temel bilgiler dikey penceresi gerekli alanlar için değerler sağlayın: (VM için) ad, kullanıcı adı, kimlik doğrulama türü ve karşılık gelen kimlik bilgileri, Azure aboneliği, kaynak grubunu ve konumu.

   ![Yeni bir Ubuntu görüntüsünü oluşturma](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. VM sağlandıktan sonra sanal makine adına tıklayın ve tıklayın **uç noktaları** içinde **ayarları** kategorisi. SSH uç noktası altında listelenen bulma **tek başına**.

   ![Varsayılan uç nokta](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure CLI
Bağlantısındaki [çalıştıran bir sanal makine Linux oluşturma][vm-instructions].

VM sağlandıktan sonra aşağıdaki komutu çalıştırarak SSH uç noktası alabilirsiniz:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Ruby on Rails yükleyin
1. VM'ye bağlanmak için SSH kullanın.
2. SSH oturumundan, VM'de Ruby yüklemek için aşağıdaki komutları kullanın:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > The brightbox repository contains the current Ruby distribution.

    Yükleme birkaç dakika sürebilir. Tamamlandığında, Ruby yüklendiğini doğrulamak için aşağıdaki komutu kullanın:

        ruby -v

3. Rails yüklemek için aşağıdaki komutu kullanın:

        sudo gem install rails --no-rdoc --no-ri -V

    Kullanımı daha hızlı olan belgeleri, yüklemeyi atlamak için no-rdoc ve--no-RI bayrakları.
    Ekleme -V yükleme ilerleme durumu hakkında bilgi görüntüler bu komut, büyük olasılıkla yürütmek için uzun sürer.

## <a name="create-and-run-an-app"></a>Oluşturma ve uygulama çalıştırma
SSH yoluyla oturumunuz hala açıkken aşağıdaki komutları çalıştırın:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

[Yeni](http://guides.rubyonrails.org/command_line.html#rails-new) komut yeni bir Rails uygulaması oluşturur. [Sunucu](http://guides.rubyonrails.org/command_line.html#rails-server) komut Rails ile birlikte gelen WEBrick web sunucusu başlatır. (Üretimde kullanıma yönelik, büyük olasılıkla Unicorn veya yolcu gibi farklı bir sunucu kullanmak isteyebilirsiniz.)

Aşağıdakine benzer bir çıktı görmeniz gerekir.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Bir uç nokta ekleme
1. [Azure portalı] [https://portal.azure.com] ve VM'nizi seçin.

2. Seçin **uç noktaları** içinde **ayarları** sol kenar sayfası.

3. Tıklayın **ekleme** sayfanın üstünde.

4. İçinde **uç noktası ekleme** iletişim sayfasında, aşağıdaki bilgileri girin:

   * **Ad**: HTTP
   * **Protokol**: TCP
   * **Genel bağlantı noktası**: 80
   * **Özel bağlantı noktası**: 3000
   * **PI adresi kayan**: devre dışı
   * **Erişim denetim listesi - sipariş**: Bu kuralın önceliğini ayarlar 1001 veya başka bir değer.
   * **Erişim denetim listesi - adı**: allowHTTP
   * **Erişim denetim listesi - eylem**: izin ver
   * **Erişim denetim listesi - uzak alt**: 1.0.0.0/16

     Bu uç noktaya trafik, Rails sunucusunu nereye dinliyor özel bağlantı noktası 3000 için yönlendirilecek 80 numaralı genel bağlantı noktasını sahiptir. Erişim denetim listesi kuralına 80 numaralı bağlantı noktasında ortak trafiğine izin verir.

     ![Yeni uç nokta](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Uç nokta kaydetmek için Tamam'a tıklayın.

6. Bildiren bir ileti görünmelidir **sanal makine uç noktası kaydediliyor**. Uç nokta, bu ileti kaybolur sonra etkinleştirilir. Şimdi sanal makinenizi DNS adını giderek uygulamanızı test. Web sitesi, aşağıdakine benzer görünmelidir:

    ![Varsayılan rails sayfası][default-rails-cloud]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide adımlarının çoğunun el ile vermedi. Bir üretim ortamında, geliştirme makinenizde uygulamanızı yazma ve Azure VM dağıtın. Ayrıca, çoğu üretim ortamlarında, Apache veya Ngınx, Rails uygulamasını birden çok örneğini Yönlendirme ve statik kaynakları sunan yürüten istek gibi başka bir sunucu işlemi ile birlikte Rails uygulamasını barındırır. Daha fazla bilgi için http://guides.rubyonrails.org/routing.html.

Ruby on Rails hakkında daha fazla bilgi edinmek için [Ruby on Rails kılavuzları][rails-guides].

Ruby uygulamanızı Azure hizmetlerini kullanmak için bkz:

* [BLOB'ları kullanarak yapılandırılmamış verileri Store][blobs]
* [Store anahtar/değer çiftleri tablolarını kullanma][tables]
* [Content Delivery Network ile yüksek bant genişlikli içeriğe hizmet][cdn-howto]

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
