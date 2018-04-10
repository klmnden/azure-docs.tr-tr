---
title: Hızlı Başlangıç - Azure portalı ile ilk Azure Container Instances kapsayıcınızı oluşturma
description: Bu hızlı başlangıçta, Azure Container Instances’da bir kapsayıcı dağıtmak için Azure portalını kullanacaksınız
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 04/02/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: cb0c8c5f5730ae1f7a2e9b38c3ef3e04ee8cde67
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Hızlı başlangıç: Azure Container Instances’da ilk kapsayıcınızı oluşturma

Azure Container Instances, Azure’da kapsayıcı oluşturmayı ve yönetmeyi kolaylaştırır. Bu hızlı başlangıç içeriğinde, bir kapsayıcı oluşturacak ve bu kapsayıcıyı genel IP adresi ile İnternet üzerinden kullanıma sunacaksınız. Bu işlem Azure portal kullanarak tamamlanır. Yalnızca birkaç tıklama ile tarayıcınızda şunu görürsünüz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-portal-07]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-container-instance"></a>Kapsayıcı örneği oluşturma

**Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Instances** seçeneğini belirleyin.

![Azure portalında yeni bir kapsayıcı örneği oluşturmaya başlama][aci-portal-01]

**Kapsayıcı adı**, **Kapsayıcı görüntüsü** ve **Kaynak grubu** metin kutularına aşağıdaki değerleri girin. Diğer değerleri varsayılan değerlerinde bırakın ve **Tamam**’a tıklayın.

* Kapsayıcı adı: `mycontainer`
* Kapsayıcı görüntüsü: `microsoft/aci-helloworld`
* Kaynak grubu: `myResourceGroup`

![Azure portalında yeni bir kapsayıcı örneği için temel ayarları yapılandırma][aci-portal-03]

Azure Container Instances’ta hem Windows hem de Linux kapsayıcıları oluşturabilirsiniz. Bu hızlı başlangıçta, önceki adımda Linux tabanlı bir kapsayıcı (`microsoft/aci-helloworld`) belirttiğimiz için varsayılan **Linux** ayarını değiştirmeden bırakacağız.

**Yapılandırma**’da diğer ayarları varsayılan değerlerinde bırakın ve yapılandırmayı doğrulamak için **Tamam**’a tıklayın.

![Azure portalında yeni bir kapsayıcı örneği yapılandırma][aci-portal-04]

Doğrulama tamamlandığında, kapsayıcı ayarlarının bir özeti gösterilir. Kapsayıcı dağıtım isteğinizi göndermek için **Tamam**’ı seçin.

![Azure portalında yeni bir kapsayıcı örneği için ayarların özeti][aci-portal-05]

Dağıtım başlatıldığında, dağıtım ilerlemesini gösteren bir kutucuk portal panonuza yerleştirilir. Dağıtım tamamlandığında, kutucuk yeni **mycontainer-myc1** kapsayıcı grubunuzu göstermek için güncelleştirilir.

![Azure portalında yeni bir kapsayıcı örneği için oluşturma ilerlemesi][aci-portal-08]

Kapsayıcı grubu özelliklerini görüntülemek için **mycontainer-myc1** kapsayıcı grubunu seçin. Kapsayıcı grubunun **IP adresi** ve kapsayıcının **STATE** değerini not edin.

![Azure portalında kapsayıcı grubuna genel bakış][aci-portal-06]

Kapsayıcı **Çalışıyor** durumuna geçtiğinde, yeni kapsayıcınızda barındırılan uygulamayı görüntülemek için önceki adımda not ettiğiniz IP adresine gidin.

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-portal-07]

## <a name="delete-the-container"></a>Kapsayıcıyı silme
Kapsayıcıyla işiniz bittiğinde, **mycontainer-myc1** kapsayıcı grubunu seçip **Sil**’e tıklayın.

![Azure portalında kapsayıcı örneğini silme][aci-portal-09]

Bu işlem onay bir iletişim kutusu açar, seçim yapmanız istendiğinde **Evet**’e tıklayın.

![Azure portalında kapsayıcı örneği onayını silme][aci-portal-10]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
<!--[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png-->
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel bir Docker Hub deposundaki bir görüntüden bir Azure Container Instance oluşturdunuz. Kapsayıcıyı kendiniz oluşturup Azure Container Registry’yi kullanarak Azure Container Instances’a dağıtmayı denemek istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri](./container-instances-tutorial-prepare-app.md)