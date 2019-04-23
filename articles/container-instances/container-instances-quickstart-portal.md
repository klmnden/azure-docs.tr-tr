---
title: Hızlı Başlangıç - Azure Container Instances'a - Portal Docker kapsayıcısı dağıtma
description: Bu hızlı başlangıçta, bir yalıtılmış bir Azure kapsayıcı örneğinde çalışan kapsayıcılı web uygulaması hızlı bir şekilde dağıtmak için Azure portalını kullanın
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 04/17/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 008d6d2a9a4a20e9fd083e9e2f009396a7f14df2
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995924"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak azure'da bir kapsayıcı örneği dağıtma

Azure Container Instances, kolay ve hızlı ile Azure'da sunucusuz Docker kapsayıcıları çalıştırmak için kullanın. Azure Kubernetes hizmeti gibi bir tam kapsayıcı düzenleme platformunu ihtiyacınız kalmadığında bir kapsayıcı örneği isteğe bağlı bir uygulamayı dağıtın.

Bu hızlı başlangıçta, yalıtılmış bir Docker kapsayıcısı dağıtma ve uygulamayı bir tam etki alanı adı (FQDN) ile kullanılabilir hale getirmek için Azure portalını kullanın. Birkaç ayarı yapılandırdıktan ve kapsayıcıyı dağıttıktan sonra çalışan uygulamaya göz atabilirsiniz:

![Azure Container Instances hizmetine dağıtılmış uygulamanın tarayıcıdaki görüntüsü][aci-portal-07]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-free-account] oluşturun.

## <a name="create-a-container-instance"></a>Kapsayıcı örneği oluşturma

**Kaynak oluştur** > **Kapsayıcılar** > **Container Instances** seçeneğini belirleyin.

![Azure portalında yeni bir kapsayıcı örneği oluşturmaya başlama][aci-portal-01]

Üzerinde **Temelleri** sayfasında, aşağıdaki değerleri girin **kaynak grubu**, **kapsayıcı adı**, ve **kapsayıcı görüntüsü** metin kutuları. Diğer değerleri varsayılan değerlerinde bırakın ve **Tamam**’ı seçin.

* Kaynak grubu: **Yeni Oluştur** > `myresourcegroup`
* Kapsayıcı adı: `mycontainer`
* Kapsayıcı görüntüsü: `mcr.microsoft.com/azuredocs/aci-helloworld`

![Azure portalında yeni bir kapsayıcı örneği için temel ayarları yapılandırma][aci-portal-03]

Bu Hızlı Başlangıç için kullandığınız varsayılan **görüntü türü** ayarıyla **genel** genel Microsoft dağıtmak için `aci-helloworld` görüntü. Bu bir Linux görüntüsü statik bir HTML sayfası görevi görür node.js'de yazılmış küçük bir web uygulamasını paketler.

Üzerinde **ağ** sayfasında, belirtin bir **DNS ad etiketi** kapsayıcınız için. Ad, kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Kapsayıcınız `<dns-name-label>.<region>.azurecontainer.io` konumunda genel kullanıma sunulacaktır. "DNS ad etiketi kullanılamıyor" hata iletisiyle karşılaşırsanız farklı bir DNS ad etiketi deneyin.

![Azure portalında yeni bir kapsayıcı örneği yapılandırma][aci-portal-04]

Diğer ayarları değerlerinde bırakın ve ardından **gözden geçir + Oluştur**.

Doğrulama tamamlandığında, kapsayıcı ayarlarının bir özeti gösterilir. Seçin **Oluştur** kapsayıcı dağıtım isteğinizi gönderebilirsiniz.

![Azure portalında yeni bir kapsayıcı örneği için ayarların özeti][aci-portal-05]

Dağıtım başladığında devam ettiğini gösteren bir bildirim görüntülenir. Kapsayıcı grubu dağıtıldığında yeni bir bildirim daha görüntülenir.

Kapsayıcı grubu için genel bakış giderek açın **kaynak grupları** > **myresourcegroup** > **mycontainer**. Kapsayıcı örneğinin **Durum**’u ile birlikte **FQDN**’sini (tam etki alanı adı) not edin.

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

Bu hızlı başlangıçta, genel bir Microsoft görüntüden bir Azure container örneği oluşturdu. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

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