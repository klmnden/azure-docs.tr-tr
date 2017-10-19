---
title: "Azure'da özel Docker kapsayıcısı kayıt defterleri"
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
ms.date: 09/11/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 664696d2f355609c76477765c2238c6d62253482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Azure'da özel Docker kapsayıcısı kayıt defterlerine giriş

Azure Container Kayıt Defteri, açık kaynak Docker Registry 2.0’ı temel alan bir yönetilen [Docker kayıt defteri](https://docs.docker.com/registry/) hizmetidir. Özel [Docker kapsayıcısı](https://www.docker.com/what-docker) görüntülerinizi depolamak ve yönetmek için Azure kapsayıcısı kayıt defterleri oluşturun ve bunların bakımını yapın. Azure’da mevcut kapsayıcı geliştirme ve dağıtım işlem hatlarınızla kapsayıcı kayıt defterleri kullanın ve Docker topluluğunun uzmanlığından yararlanın.

Docker ve kapsayıcılarla ilgili arka plan bilgileri için bkz. [Docker kullanıcı kılavuzu](https://docs.docker.com/engine/userguide/).

## <a name="use-cases"></a>Uygulama alanları
Azure kapsayıcısı kayıt defterinden çeşitli dağıtım hedeflerine görüntü çekme:

* [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) ve [Kubernetes](http://kubernetes.io/docs/) dahil olmak üzere konak kümeleri arasında kapsayıcı haline getirilmiş uygulamaları yöneten **ölçeklenebilir düzenleme sistemleri** .
* [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) ve diğerleri gibi uygun ölçekte uygulama oluşturulmasını ve çalıştırılmasını destekleyen **Azure hizmetleri**.

Geliştiriciler bir kapsayıcı geliştirme iş akışı kapsamında bir kapsayıcı kayıt defterine de öğe itebilir. Örneğin, [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) veya [Jenkins](https://jenkins.io/) gibi bir sürekli tümleştirme ve dağıtım aracından bir kapsayıcı kayıt defteri hedeflenebilir.

## <a name="key-concepts"></a>Önemli kavramlar
* **Kayıt Defteri** - Azure aboneliğinizde bir veya daha fazla kapsayıcı kayıt defteri oluşturun. Her kayıt defteri aynı konumdaki standart bir Azure [depolama hesabı](../storage/common/storage-introduction.md) tarafından desteklenir. Kapsayıcı görüntülerinizin yerel, kapalı bir ağda depolanmasının avantajlarından yararlanmak için dağıtımlarınızla aynı Azure konumunda bir kayıt defteri oluşturun. Tam kayıt defteri adı `myregistry.azurecr.io` biçimindedir.

  Azure Active Directory destekli bir [hizmet sorumlusunu](../active-directory/active-directory-application-objects.md) veya sağlanan bir yönetici hesabını kullanarak kapsayıcı kayıt defterine [erişimi denetlersiniz](container-registry-authentication.md). Bir kayıt defteriyle kimlik doğrulamak için standart `docker login` komutunu çalıştırın.

* **Yönetilen Kayıt Defteri** - Yönetilen kayıt defteri veya kayıt defteri oluştururken kendi depolama hesabınızda yedeklenecek bir kayıt defteri oluşturabilirsiniz. Yönetilen kayıt defterleri Temel, Standart ve Premium olmak üzere üç SKU'da ek özellikler sunar. Bu SKU'lardaki görüntüler, Azure Container Registry hizmeti tarafından yönetilen Azure Depolama hesaplarında depolanır. Böylece güvenilirlik artar ve yeni özellikler etkinleştirilir. Yeni özellikler; web kancası tümleştirme, Azure Active Directory ile depo kimlik doğrulaması ve silme işlemi için desteği içerir.

* **Depo** -Bir kayıt defteri, kapsayıcı görüntüleri grubu olan bir veya daha çok depo içerir. Azure Container Kayıt Defteri, çok düzeyli depo ad alanlarını destekler. Çok düzeyli ad alanlarıyla, belirli bir uygulamayla veya uygulama koleksiyonuyla ilişkili görüntü koleksiyonlarını belirli geliştirme veya işlem ekipleri halinde gruplandırabilirsiniz. Örneğin:

  * `myregistry.azurecr.io/aspnetcore:1.0.1`, kurum çapında bir görüntüyü temsil eder
  * `myregistry.azurecr.io/warrantydept/dotnet-build`, garanti departmanı genelinde paylaşılan .NET uygulamaları oluşturmak için kullanılan bir görüntüyü temsil eder
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web`, garanti departmanına ait olan müşteri gönderileri uygulamasında gruplandırılan bir web görüntüsünü temsil eder

* **Görüntü** - Bir depoda depolanan görüntülerin her biri, bir Docker kapsayıcısının salt okunur anlık görüntüsüdür. Azure kapsayıcısı kayıt defterleri hem Windows hem de Linux görüntüleri içerebilir. Tüm kapsayıcı dağıtımlarınız için görüntü adlarını siz denetlersiniz. Bir depoya görüntü itmek ya da bir depodan görüntü çekmek için standart [Docker komutlarını](https://docs.docker.com/engine/reference/commandline/) kullanın.

* **Kapsayıcı** - Kapsayıcı, bir yazılım uygulamasını ve uygulamanın bağımlılıklarını kod, çalışma zamanı, sistem araçları ve kitaplıkları içeren eksiksiz bir dosya sistemi şeklinde sarmalanmış bir halde tanımlar. Docker kapsayıcılarını bir kapsayıcı kayıt defterinden çektiğiniz Windows veya Linux görüntülerine bağlı olarak çalıştırın. Tek bir makinede çalışan kapsayıcılar işletim sistemi çekirdeğini paylaşır. Docker kapsayıcıları tüm büyük Linux dağıtımlarına, macOS ve Windows'a tümüyle taşınabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-portal.md)
* [Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md)
* [Docker CLI’yı kullanarak ilk görüntünüzü itme](container-registry-get-started-docker-cli.md)
* Visual Studio Team Services, Azure Container Service ve Azure Container Registry kullanarak sürekli tümleştirme ve dağıtım iş akışı oluşturmak için bkz. [Docker Swarm ve VSTS ile CI/CD](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md).