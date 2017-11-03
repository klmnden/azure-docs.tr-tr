---
title: "Hızlı Başlangıç - Azure portalıyla ilk Azure kapsayıcı örnekleri kapsayıcı oluşturma"
description: "Azure Container Instances’ı dağıtma ve kullanmaya başlama"
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 0179107ece1e150246ab40836783d810425be3ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Azure Container Instances’da ilk kapsayıcınızı oluşturma

Azure Container Instances, Azure’da kapsayıcı oluşturmayı ve yönetmeyi kolaylaştırır. Bu hızlı başlangıç bir kapsayıcı oluşturmak ve genel bir IP adresi ile Internet'e kullanıma. Azure Portalı'nı kullanarak bu işlemi tamamlandı. Yalnızca birkaç tıklama ile bu tarayıcınızda görürsünüz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-container-instance"></a>Bir kapsayıcı örneği oluşturma

Seçin **yeni** > **kapsayıcıları** > **Azure kapsayıcı örnekleri (Önizleme)**.

![Azure portalında yeni bir kapsayıcı örnek oluşturmaya başla][aci-portal-01]

Aşağıdaki değerleri girin **kapsayıcı adı**, **kapsayıcı görüntü**, ve **kaynak grubu** metin kutuları. Diğer değerler varsayılan değerlerde bırakın ve ardından **Tamam**.

* Kapsayıcı adı:`mycontainer`
* Kapsayıcı görüntü:`microsoft/aci-helloworld`
* Kaynak grubu:`myResourceGroup`

![Azure portalında yeni bir kapsayıcı örnek temel ayarlarını yapılandırma][aci-portal-03]

Azure kapsayıcı örnekleri hem Windows hem de Linux kapsayıcılar oluşturabilirsiniz. Bu hızlı başlangıç varsayılan ayarını bırakacağız **Linux** biz Linux tabanlı bir kapsayıcı belirtilen bu yana (`microsoft/aci-helloworld`) önceki adımda.

Diğer ayarlarında bırakın **yapılandırma** varsayılan değerlerde, ardından **Tamam** yapılandırmayı doğrulamak için.

![Azure portalında yeni bir kapsayıcı örnek yapılandırma][aci-portal-04]

Doğrulama tamamlandığında, kapsayıcı ayarlarının bir özeti gösterilir. Seçin **Tamam** kapsayıcı dağıtım isteğinizi gönderebilirsiniz.

![Azure portalında yeni bir kapsayıcı örnek için ayar özeti][aci-portal-05]

Dağıtım başladığında, bir kutucuk dağıtımının ilerleme durumunu gösteren portal Panonuzda yerleştirilir. Dağıtım tamamlandıktan sonra kutucuğun yeni gösterecek şekilde güncelleştirilir **mycontainer myc1** kapsayıcı grubu.

![Azure portalında yeni bir kapsayıcı örnek oluşturma ilerlemesi][aci-portal-08]

Seçin **mycontainer myc1** kapsayıcı grubu özelliklerini görüntülemek için kapsayıcı grubu. Not edin **IP adresi** kapsayıcı grubunun yanı sıra **durumu** , kapsayıcının.

![Azure portalında kapsayıcı Grup genel bakış][aci-portal-06]

Kapsayıcı taşır sonra **çalıştıran** durum, yeni kapsayıcıda barındırılan uygulamayı görüntülemek için önceki adımda not ettiğiniz IP adresine gidin.

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-app-browser]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç ortak Docker hub'a depoda bir görüntüden Azure kapsayıcı örneği oluşturdu. Kendiniz bir kapsayıcı oluşturma deneyin ve Azure kapsayıcı örnekleri Azure kapsayıcı kayıt defterini kullanarak dağıtmak istiyorsanız, Azure kapsayıcı örnekleri Öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri](./container-instances-tutorial-prepare-app.md)