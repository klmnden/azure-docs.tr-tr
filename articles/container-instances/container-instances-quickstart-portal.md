---
title: Hızlı başlangıç - Azure Container Instances hizmetinde uygulama çalıştırma
description: Bu hızlı başlangıçta, Azure portalı kullanarak Docker kapsayıcısında çalışan bir uygulamayı Azure Container Instances hizmetine dağıtacaksınız
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 6ab7680559400c0a270142563ecf7e251c2cd1b8
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857729"
---
# <a name="quickstart-run-an-application-in-azure-container-instances"></a>Hızlı başlangıç: Azure Container Instances hizmetinde uygulama çalıştırma

Docker kapsayıcılarınızı kolay ve hızlı bir şekilde Azure'da çalıştırmak için Azure Container Instances hizmetini kullanın. Sanal makine dağıtmanız veya Kubernetes gibi tam kapsamlı bir düzenleme platformu kullanmanız gerekmez. Bu hızlı başlangıçta, Azure portalı kullanarak Azure’da bir kapsayıcı oluşturacak ve bu uygulamasını tam etki alanı adı (FQDN) ile kullanıma sunacaksınız. Birkaç ayarı yapılandırdıktan ve kapsayıcıyı dağıttıktan sonra çalışan uygulamaya göz atabilirsiniz:

![Azure Container Instances hizmetine dağıtılmış uygulamanın tarayıcıdaki görüntüsü][aci-portal-07]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-free-account] oluşturun.

## <a name="create-a-container-instance"></a>Kapsayıcı örneği oluşturma

**Kaynak oluştur** > **Kapsayıcılar** > **Container Instances** seçeneğini belirleyin.

![Azure portalında yeni bir kapsayıcı örneği oluşturmaya başlama][aci-portal-01]

**Kapsayıcı adı**, **Kapsayıcı görüntüsü** ve **Kaynak grubu** metin kutularına aşağıdaki değerleri girin. Diğer değerleri varsayılan değerlerinde bırakın ve **Tamam**’ı seçin.

* Kapsayıcı adı: `mycontainer`
* Kapsayıcı görüntüsü: `microsoft/aci-helloworld`
* Kaynak grubu: **Yeni oluştur** > `myResourceGroup`

![Azure portalında yeni bir kapsayıcı örneği için temel ayarları yapılandırma][aci-portal-03]

Azure Container Instances’ta hem Windows hem de Linux kapsayıcıları oluşturabilirsiniz. Bu hızlı başlangıçta, Linux tabanlı `microsoft/aci-helloworld` görüntüsünü dağıtmak için **Linux**’un ayarını varsayılan şekilde bırakın.

**Yapılandırma** altında, kapsayıcınız için bir **DNS ad etiketi** belirtin. Ad, kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Kapsayıcınız `<dns-name-label>.<region>.azurecontainer.io` konumunda genel kullanıma sunulacaktır.

**Yapılandırma**’da diğer ayarları varsayılan değerlerinde bırakın ve yapılandırmayı doğrulamak için **Tamam**’ı seçin.

![Azure portalında yeni bir kapsayıcı örneği yapılandırma][aci-portal-04]

Doğrulama tamamlandığında, kapsayıcı ayarlarının bir özeti gösterilir. Kapsayıcı dağıtım isteğinizi göndermek için **Tamam**’ı seçin.

![Azure portalında yeni bir kapsayıcı örneği için ayarların özeti][aci-portal-05]

Dağıtım başladığında devam ettiğini gösteren bir bildirim görüntülenir. Kapsayıcı grubu dağıtıldığında yeni bir bildirim daha görüntülenir.

![Azure portalında yeni bir kapsayıcı örneği için oluşturma ilerlemesi][aci-portal-08]

**Kaynak Grupları** > **myResourceGroup** > **mycontainer** yolunu izleyerek kapsayıcı grubunun genel bakış sayfasını açın. Kapsayıcı örneğinin **Durum**’u ile birlikte **FQDN**’sini (tam etki alanı adı) not edin.

![Azure portalında kapsayıcı grubuna genel bakış][aci-portal-06]

*Çalışıyor* **Durumunda** iken tarayıcınızda kapsayıcının FQDN’sine gidin.

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-portal-07]

Tebrikler! Yalnızca birkaç ayarı yapılandırarak, Azure Container Instances’ta genel olarak erişilebilir bir uygulama dağıttınız.

## <a name="view-container-logs"></a>Kapsayıcı günlüklerini görüntüleme

Kapsayıcı örneğinin günlüklerini görüntülemek, kapsayıcınızın veya çalıştırdığı uygulamanın sorunlarını gidermede yardımcı olur.

Kapsayıcının günlüklerini görüntülemek için **Ayarlar** altında **Kapsayıcılar**’ı ve ardından **Günlükler**’i seçin. Uygulamayı tarayıcınızda görüntülediğinizde HTTP GET isteğinin oluşturulduğunu görmeniz gerekir.

![Azure portalında kapsayıcı günlükleri][aci-portal-11]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcıyla işiniz bittiğinde, *mycontainer* kapsayıcı örneğine ait **Genel bakış**’ı ve ardından **Sil**’i seçin.

![Azure portalında kapsayıcı örneğini silme][aci-portal-09]

Onay iletişim kutusu görüntülendiğinde **Evet**’i seçin.

![Azure portalında kapsayıcı örneği onayını silme][aci-portal-10]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel bir Docker Hub deposundaki bir görüntüden bir Azure kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/