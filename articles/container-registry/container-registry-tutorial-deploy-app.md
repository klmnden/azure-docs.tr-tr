---
title: "Azure kapsayıcı kayıt defteri Öğreticisi - Azure kapsayıcı kayıt defterinden web uygulaması dağıtma"
description: "Coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defterinden bir kapsayıcı görüntüsü kullanarak Linux tabanlı bir web uygulamasını dağıtın. Bölümü iki üç bölümlük olması gerekir."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: neilpeterson
tags: acr, azure-container-registry, geo-replication
keywords: "Docker, kapsayıcıları, kayıt defteri, Azure"
ms.service: container-registry
ms.devlang: 
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 90d4b51dfaad409298f72887480dfaf827aef9f0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="deploy-web-app-from-azure-container-registry"></a>Azure kapsayıcı kayıt defterinden Web uygulaması dağıtma

Bu iki üç bölümlü öğretici serisinde bir parçasıdır. İçinde [bölüm bir](container-registry-tutorial-prepare-registry.md), özel, coğrafi olarak çoğaltılmış kapsayıcı kayıt defteri oluşturuldu ve bir kapsayıcı görüntüsü kaynağından yerleşik ve kayıt defterine gönderilir. Bu makalede, iki Web uygulaması örneği coğrafi olarak çoğaltılmış kayıt defteri ağ Kapat yönünü yararlanmak için iki farklı Azure bölgelerinde içine kapsayıcı dağıtın.

Bu öğreticide, iki serideki Kısım:

> [!div class="checklist"]
> * Bir kapsayıcı görüntü iki dağıtmak *kapsayıcıları için Web Apps* örnekleri
> * Dağıtılmış uygulamanın doğrulayın

Coğrafi olarak çoğaltılmış bir kayıt defteri henüz oluşturmadıysanız ve kayıt defteri kapsayıcılı örnek uygulamaya görüntüsü gönderilen serisi önceki öğreticide dönün. [coğrafi olarak çoğaltılmış Azure kapsayıcı kayıt defteri hazırlama](container-registry-tutorial-prepare-registry.md).

Serinin sonraki bölümü, uygulamayı güncelleştirme ardından yeni bir kapsayıcı görüntüsü için kayıt anında. Son olarak, otomatik olarak her ikisinde de, yansıtılan değişikliği görmek için çalışan her Web uygulaması örneği Azure kapsayıcı kayıt defteri coğrafi çoğaltma ve Web kancalarını eylemde gösteren göz atın.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Kapsayıcıları için Web uygulamaları için otomatik dağıtım

Azure kapsayıcı kayıt defterini doğrudan kapsayıcılı uygulamalarını dağıtmak için destek sağlar [kapsayıcıları için Web Apps](../app-service/containers/index.yml). Bu öğreticide, farklı Azure bölgelerinde bulunan iki web uygulaması planlara önceki öğreticideki oluşturulan kapsayıcı görüntüsü dağıtmak için Azure Portalı'nı kullanın.

Kayıt defterinde bir kapsayıcı görüntüsünden bir web uygulaması dağıtma ve coğrafi olarak çoğaltılmış bir kayıt defteri aynı bölgede olması, bir görüntü dağıtımı Azure kapsayıcı kayıt defteri oluşturur [Web kancası](container-registry-webhook.md) sizin için. Yeni bir görüntü kapsayıcısı deponuza bastığınızda, Web kancası değişiklik seçer ve otomatik olarak yeni kapsayıcı görüntüsü, web uygulamanızın dağıtır.

## <a name="deploy-a-web-app-for-containers-instance"></a>Kapsayıcıları örneği için bir Web uygulaması dağıtma

Bu adımda, bir Web uygulaması kapsayıcıları örneği için oluşturduğunuz *Batı ABD* bölge.

Oturum [Azure portal](https://portal.azure.com) ve önceki öğreticide oluşturduğunuz kayıt defteri gidin.

Seçin **depoları** > **acr helloworld**, sonra sağ tıklayın **v1** altında etiketi **etiketleri** seçip**Web uygulamasına dağıtma**.

![Azure portalında app Service'e dağıtma][deploy-app-portal-01]

Altında **kapsayıcıları için Web uygulaması** görüntülenen, her ayar için aşağıdaki değerleri belirtin:

| Ayar | Değer |
|---|---|
| **Site adı** | Web uygulaması için genel benzersiz bir ad. Bu örnekte, kullanırız biçimi `<acrName>-westus` bölge web uygulaması dağıtılır ve kayıt defteri kolay tanımlamak için. |
| **Kaynak Grubu** | **Varolanı kullanma** > `myResourceGroup` |
| **App service planı/konumu** | Adlı yeni bir plan oluşturmak `plan-westus` içinde **Batı ABD** bölge. |
| **Görüntü** | `acr-helloworld:v1`

Seçin **oluşturma** web uygulaması'na sağlamak için *Batı ABD* bölge.

![Web uygulaması Linux yapılandırma Azure portalında][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Dağıtılan web uygulamasının görüntüleyin

Dağıtım tamamlandığında, çalışan uygulama URL'sini tarayıcınızın giderek görüntüleyebilirsiniz.

Portalı'nda seçin **uygulama hizmetleri**, web uygulaması önceki adımda sağlanan sonra. Bu örnekte, web uygulaması adlı *uniqueregistryname westus*.

Sağ üst içinde köprülü URL web uygulamasının seçin **uygulama hizmeti** çalışan uygulama tarayıcınızda görüntülemek için genel bakış.

![Web uygulaması Linux yapılandırma Azure portalında][deploy-app-portal-04]

Coğrafi olarak çoğaltılmış kapsayıcı kayıt defterinden Docker görüntü dağıtıldığında, siteyi kapsayıcı kayıt defteri barındırma Azure bölgesini temsil eden bir görüntü görüntüler.

![Bir tarayıcıda görüntülenen dağıtılan web uygulaması][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Kapsayıcıları örneği için ikinci Web uygulaması dağıtma

İkinci bir web uygulamasına dağıtmak için önceki bölümde özetlenen yordamı kullanın *Doğu ABD* bölge. Altında **kapsayıcıları için Web uygulaması**, aşağıdaki değerleri belirtin:

| Ayar | Değer |
|---|---|
| **Site adı** | Web uygulaması için genel benzersiz bir ad. Bu örnekte, kullanırız biçimi `<acrName>-eastus` bölge web uygulaması dağıtılır ve kayıt defteri kolay tanımlamak için. |
| **Kaynak Grubu** | **Varolanı kullanma** > `myResourceGroup` |
| **App service planı/konumu** | Adlı yeni bir plan oluşturmak `plan-eastus` içinde **Doğu ABD** bölge. |
| **Görüntü** | `acr-helloworld:v1`

Seçin **oluşturma** web uygulaması'na sağlamak için *Doğu ABD* bölge.

![Web uygulaması Linux yapılandırma Azure portalında][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Dağıtılan web uygulamasının görüntüleyin

Olarak daha önce çalışan uygulama URL'sini tarayıcınızın giderek görüntüleyebilirsiniz.

Portalı'nda seçin **uygulama hizmetleri**, web uygulaması önceki adımda sağlanan sonra. Bu örnekte, web uygulaması adlı *uniqueregistryname eastus*.

Sağ üst içinde köprülü URL web uygulamasının seçin **App Service'e genel bakış** çalışan uygulama tarayıcınızda görüntülemek için.

![Web uygulaması Linux yapılandırma Azure portalında][deploy-app-portal-07]

Coğrafi olarak çoğaltılmış kapsayıcı kayıt defterinden Docker görüntü dağıtıldığında, siteyi kapsayıcı kayıt defteri barındırma Azure bölgesini temsil eden bir görüntü görüntüler.

![Bir tarayıcıda görüntülenen dağıtılan web uygulaması][deployed-app-eastus]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure kapsayıcı coğrafi olarak çoğaltılmış bir kayıt defteri kapsayıcıları örnekleri için iki Web uygulaması dağıtılmış. Bu öğreticide adımları izleyerek:

> [!div class="checklist"]
> * Bir kapsayıcı görüntüsü iki dağıtılan *kapsayıcıları için Web Apps* örnekleri
> * Dağıtılmış uygulamanın doğrulandı

Güncelleştirme ve ardından yeni bir kapsayıcı görüntüsü kapsayıcı kayıt defterine dağıtma sonraki öğretici ilerletmek sonra her iki bölgelerde çalışan web uygulamaları otomatik olarak güncelleştirildiğini doğrulayın.

> [!div class="nextstepaction"]
> [Coğrafi olarak çoğaltılmış kapsayıcı görüntü için bir güncelleştirme dağıtın](./container-registry-tutorial-deploy-update.md)

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