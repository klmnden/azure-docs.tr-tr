---
title: 'Öğretici: Service Fabric Mesh’e bir Service Fabric Mesh uygulaması dağıtma | Microsoft Docs'
description: Arka uç web hizmetiyle iletişim kuran bir ASP.NET Core web sitesini içeren bir Azure Service Fabric Mesh uygulamasını dağıtmayı öğrenin.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: f9dea759f6556bc521dda4efbd27176f1e06452b
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126584"
---
# <a name="tutorial-deploy-a-service-fabric-mesh-web-application"></a>Öğretici: Service Fabric Mesh web uygulaması dağıtma

Bir serinin üçüncü kısmı olan bu öğreticide doğrudan Visual Studio'dan bir Azure Service Fabric Mesh web uygulaması yayımlamayı öğreneceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Uygulamayı Azure’da yayımlama.
> * Uygulama dağıtım durumunu denetleme
> * Aboneliğinize dağıtılmış olan tüm uygulamaları görme
> * Uygulama günlüklerine bakma
> * Uygulama tarafından kullanılan kaynakları temizleme.

Bu öğretici serisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Service Fabric Mesh web uygulaması derleme](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Yerel ortamda uygulama hatalarını ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * Uygulamayı Azure’da yayımlama

ASP.NET web ön ucu ve ASP.NET Core Web API arka uç hizmeti olan bir Azure Service Fabric Mesh uygulaması oluşturmayı öğreneceksiniz. Ardından yerel geliştirme kümenizde uygulamanın hatalarını ayıklayacak ve uygulamayı Azure'da yayımlayacaksınız. İşlemleri tamamladığınızda bir Service Fabric Mesh uygulamasında hizmetler arası çağrı oluşturmayı gösteren basit bir yapılacak işler uygulamasına sahip olacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

* [Geliştirme ortamınızı ayarladığınızdan](service-fabric-mesh-howto-setup-developer-environment-sdk.md) ve Service Fabric çalışma zamanı, SDK, Docker ve Visual Studio 2017'yi yüklediğinizden emin olun.

## <a name="download-the-to-do-sample-application"></a>Yapılacaklar örnek uygulamasını indirme

[Bu öğretici serisinin ikinci kısmında](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md) yapılacak işler örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

Uygulama `src\todolistapp` dizininin altındadır.

## <a name="publish-to-azure"></a>Azure’da Yayımlama

Service Fabric Mesh projenizi Azure'da yayımlamak için Visual Studio'da **ServiceFabricMeshApp** girişine sağ tıklayıp **Yayımla...** öğesini seçin.

Ardından **Service Fabric Uygulamasını Yayımla** iletişim kutusunu göreceksiniz.

![Visual Studio Service Fabric Mesh yayımla iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Azure hesabınızı ve aboneliğinizi seçin. **Konum** seçin. Bu makalede **Doğu ABD** kullanılmıştır.

**Kaynak grubu** bölümünde **\<Yeni Kaynak Grubu Oluştur...>** öğesini seçin. Yeni bir kaynak grubu oluşturabileceğiniz bir iletişim kutusu açılır. Bu makalede **Doğu ABD** konumu kullanılmış ve grup **sfmeshTutorial1RG** olarak adlandırılmıştır (kuruluşunuzda aynı aboneliği kullanan birden fazla kullanıcı varsa benzersiz bir grup adı seçin).  **Oluştur**'a bastığınızda kaynak grubu oluşturulur ve yayımla iletişim kutusu açılır.

![Visual Studio Service Fabric Mesh yeni kaynak grubu iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-resource-group-dialog.png)

**Service Fabric Uygulamasını Yayımla** iletişim kutusunun **Azure Container Registry** bölümünde **\<Yeni Azure Container Registry oluştur...>** öğesini seçin. **Container Registry Oluştur** iletişim kutusunda **Container registry adı** için benzersiz bir ad girin. Bir **Konum** belirtin (bu öğreticide **Doğu ABD** kullanılmıştır). Açılan menüden önceki adımda oluşturduğunuz **Kaynak grubu** adını seçin, örneğin **sfmeshTutorial1RG**. **SKU** için **Temel** ayarını belirleyin ve ardından **Oluştur**'a basarak yayımla iletişim kutusuna dönün.

![Visual Studio Service Fabric Mesh yeni kaynak grubu iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-container-registry-dialog.png)

Aboneliğiniz için bir kaynak sağlayıcısı kaydedilmediğini belirten bir hata alırsanız gerekli kaydı yapabilirsiniz. Öncelikle kaynak sağlayıcısının aboneliğiniz için kullanılabilir durumda olup olmadığına bakın:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Container Registry sağlayıcısı (`Microsoft.ContainerRegistry`) kullanılabilir durumdaysa Powershell'den kaydedin:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

Yayımla iletişim kutusunda **Yayımla** düğmesine basarak Service Fabric uygulamanızı Azure'a dağıtın.

Azure'da ilk kez uygulama yayımladığınızda, Docker görüntüsü Azure Container Registry (ACR) konumuna iletilir ve bu da görüntünün boyutuna göre zaman alabilir. Aynı projenin sonraki yayımlama işlemleri daha hızlı olacaktır. Visual Studio **Çıktı** penceresinde **Service Fabric Araçları** bölmesini seçerek dağıtımın ilerleme durumunu izleyebilirsiniz. Dağıtım tamamlandıktan sonra **Service Fabric Araçları** çıktısı, uygulamanızın IP adresini ve bağlantı noktasını URL biçiminde gösterir.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Bir web tarayıcısı açıp URL'ye giderek Azure'da çalışan web sitenizi görebilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Kalan adımlar için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.35 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’nın son sürümünü yüklemek için bkz. [Azure CLI 2.0’ı yükleme][azure-cli-install].

## <a name="install-the-az-mesh-cli"></a>az mesh cli öğesini yükleyin
CLI isteminde

1) Azure Service Fabric Mesh CLI modülünün önceki yüklemelerini kaldırın.

```cli
az extension remove --name mesh
```

2)  Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin. Önizleme için Azure Service Fabric Mesh CLI, Azure CLI uzantısı olarak yazılmıştır ancak genel önizlemede Azure CLI ile birlikte yayımlanacaktır.

```cli
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

## <a name="check-application-deployment-status"></a>Uygulama dağıtım durumunu denetleme

Bu noktada uygulamanız artık dağıtılmış durumdadır. `app show` komutunu kullanarak durumunu kontrol edebilirsiniz. 

Öğretici uygulamasının adı `ServiceMeshApp` olacaktır. Aşağıdaki komutla uygulama hakkında ayrıntılı bilgilere ulaşabilirsiniz:

```azurecli-interactive
az mesh app show --resource-group $rg --name ServiceMeshApp
```

## <a name="see-all-applications-currently-deployed-to-your-subscription"></a>Aboneliğinize dağıtılmış olan tüm uygulamaları görme

Aboneliğinize dağıttığınız uygulamaların listesini almak için "app list" komutunu kullanabilirsiniz.

```cli
az mesh app list --output table
```

## <a name="see-the-application-logs"></a>Uygulama günlüklerine bakma

Dağıtılan uygulamanın günlüklerini inceleyin:

```azurecli-interactive
az mesh code-package-log get --resource-group $rg --application-name ServiceMeshApp --service-name todoservice --replica-name 0 --code-package-name ServiceMeshApp
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse oluşturduğunuz tüm kaynakları silebilirsiniz. Hem ACR hem de Service Fabric Mesh hizmeti kaynaklarını barındıracak yeni bir kaynak grubu oluşturduğunuzdan bu kaynak grubunu silerek ilgili tüm kaynakların da silinmesini sağlayabilirsiniz.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Alternatif olarak kaynak grubunu [portaldan](../azure-resource-manager/resource-group-portal.md#delete-resource-group-or-resources) silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde şunları öğrendiniz:
> [!div class="checklist"]
> * Uygulamayı Azure’da yayımlama.
> * Uygulama dağıtım durumunu denetleme
> * Aboneliğinize dağıtılmış olan tüm uygulamaları görme
> * Uygulama günlüklerine bakma
> * Uygulama tarafından kullanılan kaynakları temizleme.

Azure'da bir Service Fabric Mesh uygulaması yayımlamayı tamamladığınıza göre aşağıdakileri deneyebilirsiniz:

* Hizmetler arası iletişimin farklı bir örneğini görmek için [Oy verme uygulaması örneğini](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) keşfedin.
* [Service Fabric kaynaklarını](service-fabric-mesh-service-fabric-resources.md) okuyun
* [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) hakkında bilgi edinin

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest