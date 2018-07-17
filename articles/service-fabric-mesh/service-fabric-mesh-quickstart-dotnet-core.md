---
title: Hızlı Başlangıç - Azure Service Fabric kafesine uygulama dağıtma | Microsoft Docs
description: Bu hızlı başlangıçta, bir ASP.NET Core Web sitesi oluşturma ve bunu Azure Service Fabric Mesh için yayımlama işlemini göstermektedir.
services: service--fabric-mesh
documentationcenter: .net
author: tylermsft
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 93b540908391e8eedd9a1b318f3443f3e782c987
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076485"
---
# <a name="quickstart-create-and-deploy-a-web-app-to-azure-service-fabric-mesh"></a>Hızlı Başlangıç: Oluşturma ve Azure Service Fabric Mesh için bir web uygulaması dağıtma

Azure Service Fabric Mesh geliştiricilerin mikro hizmet uygulamaları sanal makineler, depolama, yönetme veya ağ dağıtmanıza olanak sağlayan tam olarak yönetilen bir hizmettir.

Bu hızlı başlangıçta ASP.NET Core web uygulaması içeren yeni bir Service Fabric Mesh uygulaması oluşturacaksınız, yerel geliştirme kümesinde çalıştırın ve Azure üzerinde çalıştırmak için yayımlama.

Bir Azure aboneliğine sahip olmanız gerekir. Yoksa, ücretsiz bir Azure aboneliği kolayca oluşturabilirsiniz [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce. Ayrıca gerekecektir [, geliştirici ortamı Kurulumu](service-fabric-mesh-howto-setup-developer-environment-sdk.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="create-a-service-fabric-mesh-project"></a>Service Fabric Mesh proje oluşturma

Visual Studio açıp seçin **dosya** > **yeni** > **proje...**

İçinde **yeni proje** iletişim **arama** kutusunun üstündeki türü `mesh`. Seçin **Service Fabric Mesh uygulaması** şablonu. (Şablon görmüyorsanız Mesh SDK'sı yüklü ve açıklandığı gibi VS araçları Önizleme emin olun [geliştirme ortamınızı ayarlama](service-fabric-mesh-howto-setup-developer-environment-sdk.md). 

İçinde **adı** kutusuna **ServiceFabricMesh1** ve **konumu** kutusunda, proje için dosyaların depolanacağı klasörü yolunu ayarlayın.

Emin olun **çözüm için dizin oluştur** denetlenir ve tıklayın **Tamam** Service Fabric Mesh projeyi oluşturmak için.

![Visual studio yeni Service Fabric Mesh proje iletişim kutusu](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-project.png)

### <a name="create-a-service"></a>Hizmet oluşturma

Tıkladıktan sonra **Tamam**, **yeni Service Fabric hizmeti** iletişim kutusu görüntülenir. Seçin **ASP.NET Core** emin olun, proje türü **kapsayıcı işletim sistemi** ayarlanır **Windows** basın **Tamam** ASP.NET Core projesi oluşturmak için . 

![Visual studio yeni Service Fabric Mesh proje iletişim kutusu](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-service-fabric-service.png)

**Yeni ASP.NET Core Web uygulaması** iletişim kutusu görüntülenir. Seçin **Web uygulaması** ve tuşuna **Tamam**.

![Visual studio yeni ASP.NET core uygulaması](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-aspnetcore-app.png)

Visual Studio, hem Service Fabric Mesh uygulaması projesi ve ASP.NET Core projesi oluşturur.

## <a name="build-and-publish-to-your-local-cluster"></a>Derleme ve yerel kümenize yayımlama

Bir Docker görüntüsü otomatik olarak yerleşik ve yerel kümenize projenizi yükler hemen sonra yayınlanmış. Bu işlem biraz zaman alabilir. Service Fabric araçları ilerlemesini izleyebilirsiniz **çıkış** istiyorsanız bölmesi. Seçin **Service Fabric Araçları** bölmesindeki bir öğeye. Docker görüntüsü dağıtılırken çalışmaya devam edebilirsiniz.

Proje oluşturulduktan sonra basın **F5** hizmetinizi yerel olarak hata ayıklamak için. Yerel Dağıtım tamamlandı ve Visual Studio projeniz çalışırken, bir tarayıcı penceresi ile bir örnek Web sayfası açılır.

Bitirdiğinizde dağıtılan hizmet tarama, projenizi tuşuna basarak hata ayıklama durdurabilirsiniz **Shift + F5** Visual Studio'da.

## <a name="publish-to-azure"></a>Azure’da Yayımlama

Azure'da Service Fabric Mesh projenizi yayımlamak için sağ **Service Fabric Mesh proje** Visual studio seçip **Yayımla...**

![Visual studio Service Fabric Mesh projeye sağ tıklayın](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-right-click-publish.png)

Göreceğiniz bir **Service Fabric uygulamasını Yayımla** iletişim.

![Visual studio Service Fabric Mesh yayımlama iletişim kutusu](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-dialog.png)

Azure hesabınızı ve aboneliğinizi seçin. Seçin bir **konumu**. Bu makalede **Doğu ABD**.

Altında **kaynak grubu**seçin  **\<yeni kaynak grubu oluştur... >**. Yeni bir kaynak grubu oluşturacağı bir iletişim kutusu görüntülenir. Bu makalede **Doğu ABD** konumuna ve grup adları **sfmeshTutorial1RG** (Kuruluşunuz birden çok kişi aynı aboneliği kullanarak varsa, benzersiz bir grup adı seçin).  Tuşuna **Oluştur** Yayımla iletişim kutusuna dönmek ve kaynak grubu oluşturun.

![Visual studio Service Fabric Mesh yeni kaynak grubu iletişim kutusu](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-resource-group-dialog.png)

Geri **Service Fabric uygulamasını Yayımla** iletişim altında **Azure Container Registry**seçin  **\<yeni Container Registry oluştur... >**. İçinde **Container Registry oluşturma** iletişim için benzersiz bir ad kullanın **kapsayıcı kayıt defteri adı**. Belirtin bir **konumu** (Bu öğreticide **Doğu ABD**). Seçin **kaynak grubu** açılan önceki adımda örneğin oluşturduğunuz **sfmeshTutorial1RG**. Ayarlama **SKU** için **temel** ve tuşuna **Oluştur** Yayımla iletişim kutusuna dönün.

Bir hata alırsanız bir kaynak sağlayıcısı kaydedilmemiş aboneliğiniz için bunu kaydedebilirsiniz. Önce aboneliğinizin kaynak sağlayıcısı olup olmadığına bakın:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Kapsayıcı kayıt defteri sağlayıcısı (`Microsoft.ContainerRegistry`) kullanılabilir, Powershell'den kaydedin:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

![Visual studio Service Fabric Mesh yeni kaynak grubu iletişim kutusu](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-container-registry-dialog.png)

Yayımla iletişim kutusunda basın **Yayımla** Service Fabric Mesh uygulamanızı azure'a dağıtmak için düğme.

Azure'a ilk kez yayımladığınızda, docker görüntüsü için Azure Container Registry (görüntü boyutuna bağlı olarak zaman alan ACR) gönderilir. Sonraki yayınlar aynı projeyi daha hızlı olacaktır. Seçerek dağıtım ilerlemesini izleyebilirsiniz **Service Fabric Araçları** bölmesinde Visual Studio **çıkış** penceresi. Dağıtım tamamlandıktan sonra **Service Fabric Araçları** çıkış IP adresini ve bağlantı noktası, uygulamanızın bir URL biçiminde görüntülenir.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMesh1\Web1\bin\Any CPU\Release\netcoreapp2.0\Web1.dll
Uploading the images to Azure Container Registry...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://...
```

Bir web tarayıcısı açın ve Azure'da çalışan Web sitesi görmek için URL'ye gidin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu hızlı başlangıç için oluşturduğunuz tüm kaynakları silin. ACR hem Service Fabric Mesh hizmet kaynakları barındırmak için yeni bir kaynak grubu oluşturduktan sonra güvenle ilişkili tüm kaynakları silmek için kolay bir yolu bu kaynak grubunu silebilirsiniz.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Alternatif olarak, kaynak grubunu silebilirsiniz [Azure portalından](https://portal.azure.com).

## <a name="next-steps"></a>Sonraki adımlar
Oluşturma ve Service Fabric Mesh uygulamaları dağıtma hakkında daha fazla bilgi için öğreticiye geçin.
> [!div class="nextstepaction"]
> [Bir hizmet birden çok web uygulaması oluşturma ve dağıtma](service-fabric-mesh-tutorial-create-dotnetcore.md)
