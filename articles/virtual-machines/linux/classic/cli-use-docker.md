---
title: Azure Linux için Docker VM uzantısı kullanma
description: Docker ve Azure sanal makine uzantıları açıklar ve program aracılığıyla sanal makineleri Azure CLI kullanarak komut satırından docker ana Azure üzerinde nasıl oluşturulacağını gösterir.
services: virtual-machines-linux
documentationcenter: ''
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: 91f7ea54afce0e94953d4bb01bbb1b33f167fe22
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Docker VM Uzantısını Azure Komut Satırı Arabirimi (Azure CLI) ile kullanma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Docker VM uzantısı ile Resource Manager modelini kullanarak hakkında daha fazla bilgi için bkz: [burada](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu konu, herhangi bir platformda, Azure CLI içinde Hizmet Yönetimi (asm) modundan Docker VM uzantısı olan bir VM oluşturmak açıklar. [Docker](https://www.docker.com/) kullanır en popüler sanallaştırma yaklaşımlardan biri [Linux kapsayıcıları](http://en.wikipedia.org/wiki/LXC) verileri yalıtarak ve bilgi işlem paylaşılan kaynakları üzerinde bir yolu olarak sanal makineleri yerine. Docker VM uzantısı kullanabilirsiniz ve [Azure Linux Aracısı](../../extensions/agent-linux.md) kapsayıcıları uygulamalarınızın Azure ile ilgili herhangi bir sayıda barındıran bir Docker VM oluşturmak için. Kapsayıcıları ve bunların avantajları üst düzey bir tartışma için bkz [Docker yüksek düzey Beyaz Tahta](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a>Docker VM uzantısı Azure ile kullanma
Docker VM uzantısı Azure ile kullanmak için bir sürümünü yüklemeniz [Azure komut satırı arabirimi](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) değerinden daha yüksek 0.8.6 (Bu yazılmasını 0.10.0 geçerli sürümü olduğu gibi). Mac, Linux ve Windows Azure CLI yükleyebilirsiniz.

Azure üzerinde Docker kullanmak için tam basit bir işlemdir:

* Azure CLI ve bağımlılıklarını Azure denetimine istediğiniz bilgisayara yükleyin (Windows, bu sanal makine olarak çalışan Linux dağıtım olacaktır)
* Bir VM Docker ana bilgisayar oluşturmak için Azure CLI Docker komutları kullanın
* Azure'da, Docker VM, Docker kapsayıcılarında yönetmek için yerel Docker komutları kullanın.

### <a name="install-the-azure-command-line-interface-azure-cli"></a>Azure yükleme komut satırı arabirimi (Azure CLI)
Azure CLI'yi yükleme ve yapılandırma için bkz: [Azure komut satırı arabirimi yükleme](../../../cli-install-nodejs.md). Yüklemeyi doğrulamak için şunu yazın `azure` , komut isteminde ve Azure CLI ASCII art gördüğünüz kısa bir süre sonra temel komutlar listelenmektedir. Yükleme doğru şekilde çalışan, yazın mümkün olmalıdır `azure help vm` ve listelenen komutlardan birini "docker" olduğunu görürsünüz.

> [!NOTE]
> Windows, docker sahip Araçları [Docker makine](https://docs.docker.com/installation/windows/), hangi Azure Vm'leri docker ana bilgisayarları olarak çalışmak için kullanabileceğiniz bir docker istemci oluşturulmasını otomatik hale getirmek için de kullanabilirsiniz.
> 
> 

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Azure CLI Azure hesabınıza bağlanın
Azure CLI kullanmadan önce Azure hesabı kimlik bilgilerinizi platformunuz üzerinde Azure CLI ile ilişkilendirmeniz gerekir. Bölüm [Azure aboneliğinize bağlanmak nasıl](/cli/azure/authenticate-azure-cli) indirmek ve içeri aktarma açıklanmaktadır, **.publishsettings** dosya veya Azure CLI bir Kurumsal kimlik ile ilişkilendirin.

> [!NOTE]
> Aşağıdakilerden birini veya farklı işlevler anlamak için yukarıdaki Belge okuduğunuzdan emin olun şekilde kimlik doğrulama, diğer yöntemleri kullanılırken davranışı bazı farklılıklar vardır.
> 
> 

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Docker yükleme ve Docker VM uzantısı için Azure kullanma
İzleyin [Docker yükleme yönergeleri](https://docs.docker.com/installation/#installation) Docker bilgisayarınıza yerel olarak yüklemek için.

Bir Azure sanal makineyle Docker kullanmak için VM için kullanılan Linux görüntüsü olması gerekir [Azure Linux VM Aracısı](../../extensions/agent-linux.md) yüklü. Şu anda bu sağlayan görüntüleri yalnızca iki tür vardır:

* Ubuntu görüntüden Azure görüntü Galerisi veya
* Azure Linux VM Aracısı ile oluşturulan özel bir Linux görüntü yüklenir ve yapılandırılır. Bkz: [Azure Linux VM Aracısı](../../extensions/agent-linux.md) Azure VM Aracısı ile özel bir Linux VM oluşturma hakkında daha fazla bilgi için.

### <a name="using-the-azure-image-gallery"></a>Azure görüntü Galerisi kullanma
Bir Bash veya Terminal oturumu, VM galerisinde yazarak kullanmak için en son Ubuntu görüntü bulmak için aşağıdaki Azure CLI komutunu kullanın

`azure vm image list | grep Ubuntu-14_04`

bir resim adları gibi seçin `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`ve bu görüntüyü kullanarak yeni bir VM oluşturmak için aşağıdaki komutu kullanın.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Burada:

* *&lt;VM buluthizmeti adı&gt;*  azure'da Docker kapsayıcısı ana bilgisayar olacak VM adı
* *&lt;Kullanıcı adı&gt;*  VM varsayılan kök kullanıcının kullanıcı adı
* *&lt;Parola&gt;*  parolası *kullanıcıadı* Azure için karmaşıklık standartları karşılar hesabı

> [!NOTE]
> Parola şu anda gerekir, en az 8 karakter olmalı bir küçük harf ve bir büyük harf karakter, sayı ve şu karakterlerden birini gibi bir özel karakter içermelidir: `!@#$%^&+=`. Hayır, önceki tümcenin sonunda nokta özel bir karakter değil.
> 
> 

Komut başarılı olduysa, kesin değişkenler ve kullandığınız seçenekler bağlı olarak aşağıdaki gibi bir şey görmeniz gerekir:

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> Sanal makine oluşturmak birkaç dakika alabilir, ancak sonra bu için hazırlanmadı (durum değeri `ReadyRole`) Docker arka plan programı (Docker hizmeti) başlatır ve Docker kapsayıcısı ana bilgisayara bağlanabilir.
> 
> 

Azure içinde oluşturduğunuz Docker VM sınamak için yazın

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

Burada *&lt;vm-adı--kullandığınız&gt;* aramanız için kullanılan sanal makine adı `azure vm docker create`. Docker ana VM çalışır durumda olduğunu gösteren aşağıdaki Azure'da çalışan ve komutları için bekleyen benzer bir şey görmeniz gerekir. 

Bilgi edinmek için docker istemcisini kullanarak bağlanmayı deneyebilirsiniz artık (Mac üzerinde gibi bazı Docker istemci kurulumlarında kullanmanız gerekebilir `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Tüm çalışma olduğundan emin olmak için Docker uzantısı VM inceleyebilirsiniz:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker ana VM kimlik doğrulaması
Docker VM oluşturmanın yanı sıra `azure vm docker create` komut Docker istemci bilgisayarınız HTTPS kullanarak Azure kapsayıcı ana bilgisayara bağlanmasına izin vermek için gerekli sertifikaları da otomatik olarak oluşturur ve sertifikalar, istemci üzerinde depolanır ve uygun şekilde ana makineler. Sonraki denemeler üzerinde var olan sertifikalar yeniden ve yeni ana bilgisayarla paylaşılan.

Sertifikaları yerleştirilir varsayılan olarak, `~/.docker`, ve Docker, bağlantı noktası üzerinde çalıştırmak için yapılandırılacak **2376**. Farklı bir bağlantı noktası veya dizin kullanmak istediğiniz sonra aşağıdakilerden birini kullanabilir `azure vm docker create` , Docker kapsayıcısı yapılandırmak için komut satırı seçenekleri bağlanan istemciler için farklı bir bağlantı veya farklı sertifikaları kullanmak üzere VM ana bilgisayarı:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Konak sunucusundaki Docker arka plan dinler ve kimlik doğrulaması tarafından oluşturulan sertifikaları kullanarak belirtilen bağlantı noktası üzerindeki istemci bağlantıları için yapılandırılmış `azure vm docker create` komutu. İstemci makine Docker ana bilgisayara erişim kazanmak için bu sertifikaların olması gerekir.

> [!NOTE]
> Bu sertifikaları olmadan çalıştıran ağa bağlı bir konak makineye bağlanmak için herkese açık olacaktır. Varsayılan yapılandırma değiştirmeden önce bilgisayarlar ve uygulamalar için risk anladığınızdan emin olun.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Gitmek hazırsınız [Docker Kullanıcı Kılavuzu] ve Docker VM kullanın. Yeni Portalı'nda Docker etkin bir VM oluşturmak için bkz [Docker VM uzantısı ile portalı kullanmak nasıl].
* Docker Geliştirici modelli bir uygulama arasında herhangi bir ortamın alın ve tutarlı bir dağıtım oluşturmak için bir bildirim temelli YAML dosyası kullanan Compose Azure Docker VM uzantısı da destekler. Bkz: [tanımlamak ve bir Azure sanal makine üzerinde birden çok kapsayıcı uygulamayı çalıştırmak için Docker ve Oluştur ile çalışmaya başlama].  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How to use the Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]:../storage-whatis-account.md
[Docker VM uzantısı ile portalı kullanmak nasıl]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker Kullanıcı Kılavuzu]:https://docs.docker.com/userguide/

[tanımlamak ve bir Azure sanal makine üzerinde birden çok kapsayıcı uygulamayı çalıştırmak için Docker ve Oluştur ile çalışmaya başlama]:../docker-compose-quickstart.md
