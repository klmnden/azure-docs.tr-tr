---
title: Linux için Docker VM uzantısı kullanılarak | Microsoft Docs
description: Docker ve Azure sanal makine uzantıları ve docker ana Klasik dağıtım modelinde Azure CLI kullanarak bir Azure sanal makineler oluşturmak nasıl açıklar.
services: virtual-machines-linux
documentationcenter: ''
author: squillace
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
ms.openlocfilehash: 76497f58678e5ecfbab7d263b3adb4c475763cd8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34653595"
---
# <a name="using-the-docker-vm-extension-with-the-azure-portal"></a>Azure portalıyla Docker VM uzantısı kullanma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

[Docker](https://www.docker.com/) kullanır en popüler sanallaştırma yaklaşımlardan biri [Linux kapsayıcıları](http://en.wikipedia.org/wiki/LXC) verileri yalıtarak ve bilgi işlem paylaşılan kaynakları üzerinde bir yolu olarak sanal makineleri yerine. Tarafından yönetilen Docker VM uzantısı kullanabilirsiniz [Azure Linux Aracısı] kapsayıcıları uygulamalarınızın Azure ile ilgili herhangi bir sayıda barındıran bir Docker VM oluşturmak için.

> [!NOTE]
> Bu konuda, Azure portalından Docker VM oluşturmayı açıklar. Komut satırında bir Docker VM oluşturma hakkında bilgi için bkz: [Docker VM uzantısı gelen Azure komut satırı arabirimi (Azure CLI) kullanma]. Kapsayıcıları ve bunların avantajları üst düzey bir tartışma için bkz [Docker yüksek düzey Beyaz Tahta](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).
> 
> 

## <a name="create-a-new-vm-from-the-image-gallery"></a>Görüntü galeriden yeni VM oluşturma
İlk adım bir Azure VM Ubuntu 14.04 LTS görüntünün görüntü Galerisi'nden bir örnek sunucu görüntüsünü ve Ubuntu 14.04 masaüstü istemci olarak kullanarak Docker VM uzantısı destekleyen Linux görüntüsünden gerektirir. Portalı'nda tıklatın **+ yeni** yeni bir VM örneği oluşturun ve Ubuntu 14.04 LTS görüntü seçim yok ya da tam görüntü Galerisi, aşağıda gösterildiği gibi seçin.

> [!NOTE]
> Şu anda yalnızca Temmuz 2014'den daha yeni Ubuntu 14.04 LTS görüntüleri Docker VM uzantısı destekler.
> 
> 

![Yeni bir Ubuntu görüntüsü oluşturma](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Docker sertifikaları oluşturma
VM oluşturulduktan sonra Docker'ın istemci bilgisayarınızda yüklü olduğundan emin olun. (Ayrıntılar için bkz [Docker yükleme yönergeleri](https://docs.docker.com/installation/#installation).)

Docker iletişim göre için sertifika ve anahtar dosyaları oluşturma [Https ile çalışan Docker] ve bunların içine yerleştirin **`~/.docker`** istemci bilgisayarınızda dizin.

> [!NOTE]
> Docker VM uzantısı'nda portal şu anda base64 ile kodlanmış kimlik bilgileri gerektirir.
> 
> 

Komut satırında kullanın **`base64`** veya base64 ile kodlanmış Konular oluşturmak için başka bir sık kullanılan kodlama aracı. Basit bir sertifika ve anahtar dosyaları kümesiyle bunu aşağıdakine benzer görünür:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Docker VM uzantısı Ekle
Docker VM uzantısı eklemek için oluşturduğunuz VM örneği bulun ve ekranı aşağı kaydırarak **uzantıları** ve aşağıda gösterildiği gibi VM uzantıları getirmek için tıklatın.

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a>Bir uzantı ekleme
Tıklatın **+ Ekle** olası VM eklemek için bu VM Uzantıları'nı görüntülemek için.

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-the-docker-vm-extension"></a>Docker VM uzantısı seçin
Docker açıklama ve önemli bağlantılar getirir, Docker VM uzantısı seçin ve ardından **oluşturma** yükleme yordamı başlamak için alttaki.

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a>Sertifika ve anahtar dosyaları ekleyin:
Form alanlarını, CA sertifikası, sunucu sertifikası ve sunucu anahtarınızı base64 ile kodlanmış sürümleri aşağıdaki grafikte gösterildiği gibi girin.

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> (Önceki görüntü olduğu gibi) 2376 varsayılan olarak dolu olup olmadığını unutmayın. Herhangi bir uç nokta burada girebilirsiniz, ancak eşleşen uç nokta ayarlamayı açmak için sonraki adım olacaktır. Varsayılan değiştirirseniz, sonraki adımda eşleşen endpoint yukarı açtığınızdan emin olun.
> 
> 

## <a name="add-the-docker-communication-endpoint"></a>Docker iletişim uç nokta ekleyin
Oluşturduğunuz kaynak grubunu görüntülerken, VM ile ilişkilendirilmiş ağ güvenlik grubu seçin ve tıklatın **gelen güvenlik kuralları** aşağıda gösterildiği gibi kurallarını görüntülemek için.

![](media/portal-use-docker/AddingEndpoint.png)

Tıklatın **+ Ekle** başka bir kural ekleyin ve varsayılan durumda uç nokta için bir ad girin (Bu örnekte, **Docker**) ve 2376 'hedef bağlantı noktası aralığı'. Protokol değerini gösteren ayarlamak **TCP**, tıklatıp **Tamam** kuralı oluşturmak için.

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a>Docker istemci ve Azure Docker ana test
Bulun ve VM etki alanı adını ve türünü, istemci bilgisayarın komut satırında kopyalama `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (burada *dockerextension* ile değiştirilir alt etki alanı için VM).

Sonuç şuna benzer görünmelidir:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Yukarıdaki adımları tamamladıktan sonra artık uzaktan diğer istemcilerinden bağlanması için yapılandırılan bir Azure VM üzerinde çalışan tam olarak işlevsel bir Docker ana bilgisayarı vardır.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
Gitmek hazırsınız [Docker Kullanıcı Kılavuzu] ve Docker VM kullanın. Komut satırı arabirimi aracılığıyla Azure vm'lerinde oluşturma Docker konağına otomatikleştirmek istiyorsanız, bkz: [Docker VM uzantısı gelen Azure komut satırı arabirimi (Azure CLI) kullanma]

<!--Anchors-->
[Create a new VM from the Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add the Docker VM Extension]:#adddockerextension
[Test Docker Client and Azure Docker Host]:#testclientandserver
[Next steps]:#next-steps

<!--Image references-->
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[6]:./media/markdown-template-for-new-articles/pretty49.png
[7]:./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Docker VM uzantısı gelen Azure komut satırı arabirimi (Azure CLI) kullanma]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Aracısı]:../../extensions/agent-linux.md
[Link 3 to another azure.microsoft.com documentation topic]:../storage-whatis-account.md

[Https ile çalışan Docker]:http://docs.docker.com/articles/https/
[Docker Kullanıcı Kılavuzu]:https://docs.docker.com/userguide/
