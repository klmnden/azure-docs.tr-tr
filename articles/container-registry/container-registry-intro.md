---
title: "Azure’da özel Docker kapsayıcısı kayıt defterleri | Microsoft Docs"
description: "Bulut tabanlı, yönetilen, özel Docker kayıt defterleri sağlayan Azure Container Kayıt Defteri hizmetine giriş."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: fd0356286be46f99fd9ab8eabc53256103038407
ms.contentlocale: tr-tr
ms.lasthandoff: 08/21/2017

---
# <a name="introduction-to-private-docker-container-registries"></a>Özel Docker kapsayıcısı kayıt defterlerine giriş


Azure Container Kayıt Defteri, açık kaynak Docker Registry 2.0’ı temel alan bir yönetilen [Docker kayıt defteri](https://docs.docker.com/registry/) hizmetidir. Özel [Docker kapsayıcısı](https://www.docker.com/what-docker) görüntülerinizi depolamak ve yönetmek için Azure kapsayıcısı kayıt defterleri oluşturun ve bunların bakımını yapın. Azure’da mevcut kapsayıcı geliştirme ve dağıtım işlem hatlarınızla kapsayıcı kayıt defterleri kullanın ve Docker topluluğunun uzmanlığından yararlanın.

Docker ve kapsayıcılarla ilgili arka plan bilgileri için bkz.

* [Docker kullanıcı kılavuzu](https://docs.docker.com/engine/userguide/)




## <a name="use-cases"></a>Uygulama alanları
Azure kapsayıcısı kayıt defterinden çeşitli dağıtım hedeflerine görüntü çekme:

* [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) ve [Kubernetes](http://kubernetes.io/docs/) dahil olmak üzere konak kümeleri arasında kapsayıcı haline getirilmiş uygulamaları yöneten **ölçeklenebilir düzenleme sistemleri** .
* [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) ve diğerleri gibi uygun ölçekte uygulama oluşturulmasını ve çalıştırılmasını destekleyen **Azure hizmetleri**.

Geliştiriciler bir kapsayıcı geliştirme iş akışı kapsamında bir kapsayıcı kayıt defterine de öğe itebilir. Örneğin, [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) veya [Jenkins](https://jenkins.io/) gibi bir sürekli tümleştirme ve dağıtım aracından bir kapsayıcı kayıt defteri hedeflenebilir.





## <a name="key-concepts"></a>Önemli kavramlar
* **Kayıt Defteri** - Azure aboneliğinizde bir veya daha fazla kapsayıcı kayıt defteri oluşturun. Her kayıt defteri aynı konumdaki standart bir Azure [depolama hesabı](../storage/common/storage-introduction.md) tarafından desteklenir. Kapsayıcı görüntülerinizin yerel, kapalı bir ağda depolanmasının avantajlarından yararlanmak için dağıtımlarınızla aynı Azure konumunda bir kayıt defteri oluşturun. Tam kayıt defteri adı `myregistry.azurecr.io` biçimindedir.

  Azure Active Directory destekli bir [hizmet sorumlusunu](../active-directory/active-directory-application-objects.md) veya sağlanan bir yönetici hesabını kullanarak kapsayıcı kayıt defterine [erişimi denetlersiniz](container-registry-authentication.md). Bir kayıt defteriyle kimlik doğrulamak için standart `docker login` komutunu çalıştırın.

* **Yönetilen Kayıt Defteri** - Üç SKU’da (Temel, Standart ve Premium) kayıt defterleri için ek özellikler sunan bir katmandır. Bu SKU’lardaki görüntüler, Azure Container Registry hizmeti tarafından yönetilen Depolama Hesaplarında depolanır. Böylece güvenilirlik artar ve yeni özellikler etkinleştirilir. Yeni özellikler; web kancası tümleştirme, Azure Active Directory ile depo kimlik doğrulaması ve silme işlemi için desteği içerir. Kullanıcılar yeni bir kayıt defteri oluştururken, yönetilen kayıt defterleri ve kendi Depolama Hesapları tarafından desteklenen bir kayıt defteri arasında seçim yapabilirler.

* **Depo** -Bir kayıt defteri, kapsayıcı görüntüleri grubu olan bir veya daha çok depo içerir. Azure Container Kayıt Defteri, çok düzeyli depo ad alanlarını destekler. Bu özellik, belirli bir uygulamayla veya uygulama koleksiyonuyla ilişkili görüntü koleksiyonlarını belirli geliştirme veya işlem ekipleri halinde gruplandırmanıza imkan tanır. Örneğin:

  * `myregistry.azurecr.io/aspnetcore:1.0.1`, kurum çapında bir görüntüyü temsil eder
  * `myregistry.azurecr.io/warrantydept/dotnet-build`, garanti departmanı genelinde paylaşılan .NET uygulamaları oluşturmak için kullanılan bir görüntüyü temsil eder
  * `myregistry.azrecr.io/warrantydept/customersubmissions/web`, garanti departmanına ait olan müşteri gönderileri uygulamasında gruplandırılan bir web görüntüsünü temsil eder

* **Görüntü** - Bir depoda depolanan görüntülerin her biri, bir Docker kapsayıcısının salt okunur anlık görüntüsüdür. Azure kapsayıcısı kayıt defterleri hem Windows hem de Linux görüntüleri içerebilir. Tüm kapsayıcı dağıtımlarınız için görüntü adlarını siz denetlersiniz. Bir depoya görüntü itmek ya da bir depodan görüntü çekmek için standart [Docker komutlarını](https://docs.docker.com/engine/reference/commandline/) kullanın.

* **Kapsayıcı** - Kapsayıcı, bir yazılım uygulamasını ve uygulamanın bağımlılıklarını kod, çalışma zamanı, sistem araçları ve kitaplıkları içeren eksiksiz bir dosya sistemi şeklinde sarmalanmış bir halde tanımlar. Docker kapsayıcılarını bir kapsayıcı kayıt defterinden çektiğiniz Windows veya Linux görüntülerine bağlı olarak çalıştırın. Tek bir makinede çalışan kapsayıcılar işletim sistemi çekirdeğini paylaşır. Docker kapsayıcıları tüm büyük Linux dağıtımlarına, Mac’e ve Windows’a tümüyle taşınabilir.




## <a name="next-steps"></a>Sonraki adımlar
* [Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-portal.md)
* [Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md)
* [Docker CLI’yı kullanarak ilk görüntünüzü itme](container-registry-get-started-docker-cli.md)
* Visual Studio Team Services, Azure Container Service ve Azure Container Kayıt Defteri’ni kullanarak sürekli tümleştirme ve dağıtım iş akışı oluşturmak için [bu öğreticiye](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md) bakın.
* Azure’da kendi Docker özel kayıt defterinizi ayarlamak (genel bir uç noktası olmadan) istiyorsanız bkz. [Azure’da Kendi Özel Docker Kayıt Defterinizi Dağıtma](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).

