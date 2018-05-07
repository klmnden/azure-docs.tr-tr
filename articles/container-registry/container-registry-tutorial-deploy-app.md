---
title: Azure Container Registry öğreticisi - Azure Container Registry’den web uygulamasını dağıtma
description: Coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defterinden bir kapsayıcı görüntüsü kullanarak Linux tabanlı bir web uygulamasını dağıtın. Üç bölümden oluşan bir serinin ikinci bölümü.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: c6ac8f22f128b350844af10f309fd3b93512d54d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-deploy-web-app-from-azure-container-registry"></a>Öğretici: Azure Container Registry’den web uygulaması dağıtma

Bu, üç bölümden oluşan bir öğretici serisinin ikinci bölümüdür. [Birinci bölümde](container-registry-tutorial-prepare-registry.md), özel bir coğrafi olarak çoğaltılmış kapsayıcı kayıt defteri oluşturuldu, kaynaktan bir kapsayıcı görüntüsü derlendi ve kayıt defterine gönderildi. Bu makalede, kapsayıcıyı iki farklı Azure bölgesinde iki Web App örneğine dağıtarak coğrafi olarak çoğaltılmış kayıt defterinin ağa yakın özelliğinden yararlanacaksınız.

Bu öğreticide, serinin ikinci kısmı:

> [!div class="checklist"]
> * İki *Kapsayıcılar için Web Uygulamaları* örneğine bir kapsayıcı görüntüsü dağıtma
> * Dağıtılan uygulamayı doğrulama

Henüz coğrafi olarak çoğaltılmış bir kayıt defteri oluşturmadıysanız ve kapsayıcılı örnek uygulamanın görüntüsünü kayıt defterine gönderdiyseniz, serideki [Coğrafi olarak çoğaltılmış bir Azure kapsayıcı kayıt defteri hazırlama](container-registry-tutorial-prepare-registry.md) adlı önceki öğreticiye geri dönün.

Serinin sonraki bölümünde, uygulamayı güncelleştirecek, sonra kayıt defterine yeni bir kapsayıcı görüntüsü göndereceksiniz. Son olarak, çalıştırılan her Web App örneğine göz atarak, değişikliğin her ikisinde otomatik olarak yansıtılıp yansıtılmadığına bakar ve Azure Container Registry coğrafi çoğaltma ve web kancalarını çalışır halde görürsünüz.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Kapsayıcılar için Web Uygulamalarına otomatik dağıtım

Azure Container Registry, kapsayıcılı uygulamaları doğrudan [Kapsayıcılar için Web Uygulamaları](../app-service/containers/index.yml)’na dağıtma desteği sağlar. Bu öğreticide, Azure portalını kullanarak, önceki öğreticide oluşturulan kapsayıcı görüntüsünü, farklı Azure bölgelerinde bulunan iki web uygulaması planına dağıtacaksınız.

Kayıt defterinizdeki bir kapsayıcı görüntüsünden web uygulaması dağıttığınızda ve aynı bölgede coğrafi olarak çoğaltılmış bir kayıt defteriniz olduğunda Azure Container Registry sizin için bir görüntü dağıtımı [web kancası](container-registry-webhook.md) oluşturur. Kapsayıcı deponuza yeni bir görüntü gönderdiğinizde web kancası değişikliği alır ve otomatik olarak yeni kapsayıcı görüntüsünü web uygulamanıza dağıtır.

## <a name="deploy-a-web-app-for-containers-instance"></a>Kapsayıcılar için Web App örneğini dağıtma

Bu adımda, *Batı ABD* bölgesinde bir Kapsayıcılar için Web App örneği oluşturacaksınız.

[Azure portalında](https://portal.azure.com) oturum açın ve önceki öğreticide oluşturduğunuz kayıt defterine gidin.

**Depolar** > **acr-helloworld** seçeneğini belirleyin, ardından **Etiketler** bölümünde **v1** etiketine tıklayın ve **Web uygulamasına dağıt**’ı seçin.

![Azure portalında uygulama hizmetine dağıtma][deploy-app-portal-01]

Görüntülenen **Kapsayıcılar için Web App** bölümünde, her bir ayar için aşağıdaki değerleri belirtin:

| Ayar | Değer |
|---|---|
| **Site Adı** | Web uygulaması için genel benzersiz bir ad. Bu örnekte, web uygulamasının içinden dağıtıldığı bölgeyi ve kayıt defterini kolayca belirlemek için `<acrName>-westus` biçimini kullanıyoruz. |
| **Kaynak Grubu** | **Var olanı kullan** > `myResourceGroup` |
| **Uygulama hizmeti planı/Konumu** | **Batı ABD** bölgesinde `plan-westus` adlı yeni bir plan oluşturun. |
| **Görüntü** | `acr-helloworld:v1`

**Oluştur**’u seçerek, web uygulamasını *Batı ABD* bölgesine sağlayın.

![Azure portalında Linux yapılandırmasındaki web uygulaması][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Dağıtılan web uygulamasını görüntüleme

Dağıtım tamamlandığında, çalıştırılan uygulamanın URL’sine tarayıcınızda giderek bu uygulamayı görüntüleyebilirsiniz.

Portalda **Uygulama Hizmetleri** seçeneğini belirleyin ve önceki adımda sağladığınız web uygulamasını seçin. Bu örnekte, web uygulaması *uniqueregistryname-westus* olarak adlandırılmıştır.

Çalıştırılan uygulamayı tarayıcınızda görüntülemek için, **App Service** genel bakışının sağ üst kısmında web uygulamasının köprülü URL’sini seçin.

![Azure portalında Linux yapılandırmasındaki web uygulaması][deploy-app-portal-04]

Coğrafi olarak çoğaltılmış kapsayıcı kayıt defterinizden Docker görüntüsü dağıtıldıktan sonra site, kapsayıcı kayıt defterini barındıran Azure bölgesini temsil eden bir görüntü gösterir.

![Bir tarayıcıda görüntülenen, dağıtılan web uygulaması][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>İkinci Kapsayıcılar için Web App örneğini dağıtma

İkinci bir web uygulamasını *Doğu ABD* bölgesine dağıtmak için önceki bölümde açıklanan yordamı kullanın. **Kapsayıcılar için Web App** bölümünde aşağıdaki değerleri belirtin:

| Ayar | Değer |
|---|---|
| **Site Adı** | Web uygulaması için genel benzersiz bir ad. Bu örnekte, web uygulamasının içinden dağıtıldığı bölgeyi ve kayıt defterini kolayca belirlemek için `<acrName>-eastus` biçimini kullanıyoruz. |
| **Kaynak Grubu** | **Var olanı kullan** > `myResourceGroup` |
| **Uygulama hizmeti planı/Konumu** | **Doğu ABD** bölgesinde `plan-eastus` adlı yeni bir plan oluşturun. |
| **Görüntü** | `acr-helloworld:v1`

**Oluştur**’u seçerek, web uygulamasını *Doğu ABD* bölgesine sağlayın.

![Azure portalında Linux yapılandırmasındaki web uygulaması][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Dağıtılan web uygulamasını görüntüleme

Daha önce olduğu gibi, çalıştırılan uygulamanın URL’sine tarayıcınızda giderek bu uygulamayı görüntüleyebilirsiniz.

Portalda **Uygulama Hizmetleri** seçeneğini belirleyin ve önceki adımda sağladığınız web uygulamasını seçin. Bu örnekte, web uygulaması *uniqueregistryname-eastus* olarak adlandırılmıştır.

Çalıştırılan uygulamayı tarayıcınızda görüntülemek için, **App Service** genel bakışının sağ üst kısmında web uygulamasının köprülü URL’sini seçin.

![Azure portalında Linux yapılandırmasındaki web uygulaması][deploy-app-portal-07]

Coğrafi olarak çoğaltılmış kapsayıcı kayıt defterinizden Docker görüntüsü dağıtıldıktan sonra site, kapsayıcı kayıt defterini barındıran Azure bölgesini temsil eden bir görüntü gösterir.

![Bir tarayıcıda görüntülenen, dağıtılan web uygulaması][deployed-app-eastus]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defterinden iki Kapsayıcılar için Web App örneğini dağıttınız. Bu öğreticideki adımları izleyerek şunları yaptınız:

> [!div class="checklist"]
> * İki *Kapsayıcılar için Web Uygulamaları* örneğine bir kapsayıcı görüntüsü dağıttınız
> * Dağıtılan uygulamayı doğruladınız

Yeni bir kapsayıcı görüntüsünü güncelleştirmek ve kapsayıcı kayıt defterine dağıtmak, ardından her iki bölgede çalıştırılan web uygulamalarının otomatik olarak güncelleştirildiğini doğrulamak için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Coğrafi olarak çoğaltılmış kapsayıcı görüntüsüne güncelleştirmeyi dağıtma](./container-registry-tutorial-deploy-update.md)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png