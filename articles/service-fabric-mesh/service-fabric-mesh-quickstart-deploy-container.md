---
title: Hızlı Başlangıç - Merhaba Dünya’yı Azure Service Fabric Mesh’e Dağıtma | Microsoft Docs
description: Bu hızlı başlangıçta, bir Service Fabric Mesh uygulamasının Azure Service Fabric Mesh’e nasıl dağıtıldığı gösterilir.
services: service-fabric-mesh
keywords: SEO uzmanınıza danışmadan anahtar sözcük eklemeyin veya anahtar sözcükleri düzenlemeyin.
author: dkkapur
ms.author: dekapur
ms.date: 11/27/2018
ms.topic: quickstart
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 5ca622602c71976917a07005bf349dd98086327c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810919"
---
# <a name="quickstart-deploy-hello-world-to-service-fabric-mesh"></a>Hızlı Başlangıç: Merhaba Dünya uygulamasını Service Fabric Mesh’e dağıtma

[Service Fabric Mesh](service-fabric-mesh-overview.md), sanal makine sağlamaya gerek kalmadan Azure’da mikro hizmet uygulamalarının oluşturulmasını ve yönetilmesini kolaylaştırır. Bu hızlı başlangıçta, Azure’da Merhaba Dünya uygulamasını oluşturacak ve bunu İnternette kullanıma sunacaksınız. Bu işlem tek bir komutla tamamlanır. Yalnızca birkaç dakika içinde tarayıcınızda bu görünümü görürsünüz:

![Tarayıcıdaki Merhaba Dünya uygulaması][sfm-app-browser]

[!INCLUDE [preview note](./includes/include-preview-note.md)]

Henüz bir Azure hesabınız yoksa, başlamadan önce [ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama 
Bu hızlı başlangıcı tamamlamak için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. Şu [yönergeleri](service-fabric-mesh-howto-setup-cli.md) izleyerek Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
Uygulamanın dağıtılacağı kaynak grubunu oluşturun. Mevcut bir kaynak grubunu kullanabilir ve bu adımı atlayabilirsiniz. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma
`az mesh deployment create` komutunu kullanarak kaynak grubunda uygulamanızı oluşturun.  Aşağıdakileri çalıştırın:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/helloworld/helloworld.linux.json --parameters "{'location': {'value': 'eastus'}}" 
```

Önceki komutu kullanarak bir Linux uygulama dağıtır [linux.json şablon](https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/helloworld/helloworld.linux.json). Bir Windows uygulamasını dağıtmak istiyorsanız, kullanın [windows.json şablon](https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/helloworld/helloworld.windows.json). Windows kapsayıcı görüntüleri Linux kapsayıcı görüntülerinden büyüktür ve dağıtılması daha uzun sürebilir.

Bu komut, aşağıda gösterilen bir JSON parçacığı oluşturur. Altında ```outputs``` JSON çıkışını kopyalama bölümünü ```publicIPAddress``` özelliği.

```json
"outputs": {
    "publicIPAddress": {
    "type": "String",
    "value": "40.83.78.216"
    }
}
```

Bu bilgiler geldiği ```outputs``` ARM şablonu bölümünde. Bu bölümde, aşağıda gösterildiği gibi ağ geçidi kaynağının genel IP adresini Getir başvuruyor. 

```json
  "outputs": {
    "publicIPAddress": {
      "value": "[reference('helloWorldGateway').ipAddress]",
      "type": "string"
    }
  }
```

## <a name="open-the-application"></a>Uygulamayı açma
Uygulama başarıyla dağıtıldıktan sonra, CLI çıkışından hizmet uç noktası için genel IP adresini kopyalayın. Web tarayıcısında IP adresini açın. Azure Service Fabric Mesh logosunun yer aldığı bir web sayfası görüntülenir.

## <a name="check-the-application-details"></a>Uygulama ayrıntılarını denetleme
`az mesh app show` komutunu kullanarak uygulamanın durumunu denetleyebilirsiniz. Bu komut, takip edebileceğiniz yararlı bilgiler sağlar.

Bu hızlı başlangıçta uygulama adı olarak `helloWorldApp` kullanılır; uygulamada ayrıntıları toplamak için aşağıdaki komutu yürütün:

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name helloWorldApp
```

## <a name="see-the-application-logs"></a>Uygulama günlüklerine bakma

`az mesh code-package-log get` komutunu kullanarak dağıtılan uygulamanın günlüklerini inceleyin:

```azurecli-interactive
az mesh code-package-log get --resource-group myResourceGroup --application-name helloWorldApp --service-name helloWorldService --replica-name 0 --code-package-name helloWorldCode
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulamayı silmeye hazır olduğunuzda, kaynak grubunu, uygulamayı ve uygulamanın içerdiği ağ kaynaklarını kaldırmak için [az group delete][az-group-delete] komutunu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh uygulamalarını oluşturma ve dağıtma hakkında daha fazla bilgi edinmek için öğreticiye devam edin.
> [!div class="nextstepaction"]
> [Çok hizmet sunan bir web uygulaması oluşturma ve dağıtma](service-fabric-mesh-tutorial-create-dotnetcore.md)

<!-- Images -->
[sfm-app-browser]: ./media/service-fabric-mesh-quickstart-deploy-container/HelloWorld.png

<!-- Links / Internal -->
[az-group-delete]: /cli/azure/group
[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
