---
title: Azure'da özel Docker kapsayıcısı kayıt defterleri
description: Bulut tabanlı, yönetilen, özel Docker kayıt defterleri sağlayan Azure Container Kayıt Defteri hizmetine giriş.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview
ms.date: 05/08/2018
ms.author: stevelas
ms.custom: mvc
ms.openlocfilehash: 394297e87ef03541725aad0689f11bca17c05ed9
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576309"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Azure'da özel Docker kapsayıcısı kayıt defterlerine giriş

Azure Container Kayıt Defteri, açık kaynak Docker Registry 2.0’ı temel alan bir yönetilen [Docker kayıt defteri](https://docs.docker.com/registry/) hizmetidir. Özel [Docker kapsayıcısı](https://www.docker.com/what-docker) görüntülerinizi depolamak ve yönetmek için Azure kapsayıcısı kayıt defterleri oluşturun ve bunların bakımını yapın.

Azure’da mevcut kapsayıcı geliştirme ve dağıtım işlem hatlarınızla kapsayıcı kayıt defterleri kullanın. Azure Container Registry Oluşturması (ACR Build) kullanarak Azure’da kapsayıcı görüntüleri oluşturun. İsteğe bağlı olarak oluşturun veya kaynak kodu yürütme ve temel görüntü güncelleştirmesi oluşturma tetikleyicileri ile yapıları tamamen otomatik hale getirin.

Docker ve kapsayıcılarla ilgili arka plan bilgileri için bkz. [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/).

## <a name="use-cases"></a>Uygulama alanları

Azure kapsayıcısı kayıt defterinden çeşitli dağıtım hedeflerine görüntü çekme:

* [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) ve [Kubernetes](http://kubernetes.io/docs/) dahil olmak üzere konak kümeleri arasında kapsayıcı haline getirilmiş uygulamaları yöneten **ölçeklenebilir düzenleme sistemleri** .
* [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) ve diğerleri gibi uygun ölçekte uygulama oluşturulmasını ve çalıştırılmasını destekleyen **Azure hizmetleri**.

Geliştiriciler bir kapsayıcı geliştirme iş akışı kapsamında bir kapsayıcı kayıt defterine de öğe itebilir. Örneğin, [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) veya [Jenkins](https://jenkins.io/) gibi bir sürekli tümleştirme ve dağıtım aracından bir kapsayıcı kayıt defteri hedeflenebilir.

Uygulama görüntülerini, temel görüntüleri güncelleştirildiğinde otomatik olarak yeniden oluşturan [ACR Build](#azure-container-registry-build) oluşturma görevlerini yapılandırın. Ekibiniz bir Git deposunda kod yürüttüğünde görüntü oluşturmalarını otomatikleştirmek için ACR Build kullanın. *ACR Build şu anda önizlemededir.*

## <a name="key-concepts"></a>Önemli kavramlar

* **Kayıt Defteri** - Azure aboneliğinizde bir veya daha fazla kapsayıcı kayıt defteri oluşturun. Kayıt defterleri üç SKU'da kullanılabilir: [Temel, Standart ve Premium](container-registry-skus.md). Tümü de web kancası tümleştirmesi, Azure Active Directory ile kayıt defteri kimlik doğrulaması ve silme işlevi desteğine sahiptir. Kapsayıcı görüntülerinizin yerel, kapalı bir ağda depolanmasının avantajlarından yararlanmak için dağıtımlarınızla aynı Azure konumunda bir kayıt defteri oluşturun. Gelişmiş çoğaltma ve kapsayıcı görüntüsü dağıtma senaryoları için Premium kayıt defterlerinin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanın. Tam kayıt defteri adı `myregistry.azurecr.io` biçimindedir.

  Azure Active Directory destekli bir [hizmet sorumlusunu](../active-directory/develop/app-objects-and-service-principals.md) veya sağlanan bir yönetici hesabını kullanarak kapsayıcı kayıt defterine [erişimi denetlersiniz](container-registry-authentication.md). Bir kayıt defteriyle kimlik doğrulamak için standart `docker login` komutunu çalıştırın.

* **Depo** -Bir kayıt defteri, kapsayıcı görüntüleri grubu olan bir veya daha çok depo içerir. Azure Container Kayıt Defteri, çok düzeyli depo ad alanlarını destekler. Çok düzeyli ad alanlarıyla, belirli bir uygulamayla veya uygulama koleksiyonuyla ilişkili görüntü koleksiyonlarını belirli geliştirme veya işlem ekipleri halinde gruplandırabilirsiniz. Örnek:

  * `myregistry.azurecr.io/aspnetcore:1.0.1`, kurum çapında bir görüntüyü temsil eder
  * `myregistry.azurecr.io/warrantydept/dotnet-build`, garanti departmanı genelinde paylaşılan .NET uygulamaları oluşturmak için kullanılan bir görüntüyü temsil eder
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web`, garanti departmanına ait olan müşteri gönderileri uygulamasında gruplandırılan bir web görüntüsünü temsil eder

* **Görüntü** - Bir depoda depolanan görüntülerin her biri, bir Docker kapsayıcısının salt okunur anlık görüntüsüdür. Azure kapsayıcısı kayıt defterleri hem Windows hem de Linux görüntüleri içerebilir. Tüm kapsayıcı dağıtımlarınız için görüntü adlarını siz denetlersiniz. Bir depoya görüntü itmek ya da bir depodan görüntü çekmek için standart [Docker komutlarını](https://docs.docker.com/engine/reference/commandline/) kullanın.

* **Kapsayıcı** - Kapsayıcı, bir yazılım uygulamasını ve uygulamanın bağımlılıklarını kod, çalışma zamanı, sistem araçları ve kitaplıkları içeren eksiksiz bir dosya sistemi şeklinde sarmalanmış bir halde tanımlar. Docker kapsayıcılarını bir kapsayıcı kayıt defterinden çektiğiniz Windows veya Linux görüntülerine bağlı olarak çalıştırın. Tek bir makinede çalışan kapsayıcılar işletim sistemi çekirdeğini paylaşır. Docker kapsayıcıları tüm büyük Linux dağıtımlarına, macOS ve Windows'a tümüyle taşınabilir.

## <a name="azure-container-registry-build-preview"></a>Azure Container Registry Build (Önizleme)

[Azure Container Registry Build](container-registry-build-overview.md) (ACR Build), Azure’da kolaylaştırılmış ve verimli Docker kapsayıcı görüntüsü derlemeleri sağlayan bir Azure Container Registry özellik paketidir. `docker build` işlem yüklerini Azure’a boşaltarak geliştirme iç döngünüzü buluta genişletmek için ACR Build kullanın. Kapsayıcı işletim sisteminizi ve çerçeve düzeltme eki uygulama işlem hattınızı otomatikleştirmek ve ekibiniz kaynak denetiminde kod yürüttüğünde otomatik olarak görüntü oluşturmak için oluşturma görevleri yapılandırın.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-portal.md)
* [Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md)
* [ACR Build ile işletim sistemi ve çerçeve düzeltme eki uygulamayı otomatikleştirme](container-registry-build-overview.md) (Önizleme)
