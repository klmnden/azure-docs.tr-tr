---
title: Özel Docker kapsayıcısı kayıt defterleri Azure - genel bakış
description: Bulut tabanlı, yönetilen, özel Docker kayıt defterleri sağlayan Azure Container Kayıt Defteri hizmetine giriş.
services: container-registry
author: stevelas
ms.service: container-registry
ms.topic: overview
ms.date: 06/28/2019
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: b8b4b5fc3ec15d921ff5580aff4d0202be1d38b9
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797909"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Azure'da özel Docker kapsayıcısı kayıt defterlerine giriş

Azure Container kayıt defteri, açık kaynaklı Docker kayıt defteri 2.0 üzerinde alan bir yönetilen, özel Docker kayıt defteri hizmeti ' dir. Oluşturun ve özel Docker kapsayıcı görüntülerinizi yönetmek için Azure kapsayıcısı kayıt defterleri güncelleştirin.

Azure kapsayıcısı kayıt defterleri, mevcut kapsayıcı geliştirme ve dağıtım işlem hatlarınızla veya azure'da kapsayıcı görüntülerini oluşturmak için Azure Container kayıt defteri görevleri kullanın. İsteğe bağlı olarak oluşturun veya tam kaynak kodu işlemeler gibi tetikleyicilerle otomatikleştirme ve temel görüntü güncelleştirme.

Docker ve kayıt defteri kavramları hakkında daha fazla bilgi için bkz. [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/) ve [kayıt defterleri, depolar ve görüntüleri hakkında](container-registry-concepts.md).

## <a name="use-cases"></a>Uygulama alanları

Azure kapsayıcısı kayıt defterinden çeşitli dağıtım hedeflerine görüntü çekme:

* [Kubernetes](https://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/) ve [Docker Swarm](https://docs.docker.com/swarm/) dahil olmak üzere konak kümeleri arasında kapsayıcı haline getirilmiş uygulamaları yöneten **ölçeklenebilir düzenleme sistemleri**.
* [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) ve diğerleri gibi uygun ölçekte uygulama oluşturulmasını ve çalıştırılmasını destekleyen **Azure hizmetleri**.

Geliştiriciler bir kapsayıcı geliştirme iş akışı kapsamında bir kapsayıcı kayıt defterine de öğe itebilir. Örneğin, bir sürekli tümleştirme ve teslim aracından bir kapsayıcı kayıt defteri gibi hedef [Azure işlem hatları](/azure/devops/pipelines/get-started/what-is-azure-pipelines.md) veya [Jenkins](https://jenkins.io/).

ACR görevleri, kendi temel görüntüleri güncelleştirildiğinde uygulama görüntüleri otomatik olarak yeniden yapılandırma ya da ekibinizin kod bir Git deposuna onayladığında görüntü oluşturmayı otomatikleştirme. Oluşturma, test etme ve bulutta paralel birden çok kapsayıcı görüntülerini düzeltme otomatik hale getirmek için çok adımlı Görevler oluşturun.

Azure, Azure komut satırı arabirimi, Azure portal ve API desteği, Azure kapsayıcısı kayıt defterleri yönetme gibi araçları sağlar. İsteğe bağlı olarak yükleme [Visual Studio Code için Docker uzantısını](https://code.visualstudio.com/docs/azure/docker) ve [Azure hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısı, Azure kapsayıcısı kayıt defterleri ile çalışma. Çekme ve bir Azure container registry'ye görüntüleri gönderme veya Visual Studio Code içinde tüm ACR görevler çalıştırabilirsiniz.

## <a name="key-features"></a>Önemli özellikler

* **Kayıt defteri SKU'ları** -Azure aboneliğinizde bir veya daha fazla kapsayıcı kayıt defterleri oluşturun. Kayıt defterleri üç Sku'da kullanılabilir: [Temel, standart ve Premium](container-registry-skus.md), Web kancası tümleştirmesi, kayıt defteri kimlik doğrulamasını Azure Active Directory ve silme işlevlerini destekleyen her biri. Kapsayıcı görüntülerinizin yerel, kapalı bir ağda depolanmasının avantajlarından yararlanmak için dağıtımlarınızla aynı Azure konumunda bir kayıt defteri oluşturun. Gelişmiş çoğaltma ve kapsayıcı görüntüsü dağıtma senaryoları için Premium kayıt defterlerinin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanın. 

  [Erişimini](container-registry-authentication.md) kullanarak bir Azure kimlik Azure Active Directory destekli bir kapsayıcı kayıt defterine [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md), ya sağlanan yönetici hesabı. Azure CLI veya standardını kullanarak kayıt defterini oturum `docker login` komutu.

* **Desteklenen görüntüleri ve yapıtları** -bir depoda gruplandırılmış, her uyumlu Docker kapsayıcısının salt okunur anlık görüntüsüdür. Azure kapsayıcısı kayıt defterleri hem Windows hem de Linux görüntüleri içerebilir. Tüm kapsayıcı dağıtımlarınız için görüntü adlarını siz denetlersiniz. Bir depoya görüntü itmek ya da bir depodan görüntü çekmek için standart [Docker komutlarını](https://docs.docker.com/engine/reference/commandline/) kullanın. Docker kapsayıcı görüntüleri ek olarak, Azure Container Registry depolar [ilgili içerik biçimlerini](container-registry-image-formats.md) gibi [Helm grafikleri](container-registry-helm-repos.md) ve yerleşik görüntüleri [açık kapsayıcı girişim (OCI) görüntüsü Biçim belirtimi](https://github.com/opencontainers/image-spec/blob/master/spec.md).

* **Azure kapsayıcı kayıt defteri görevleri** -kullanım [Azure kapsayıcı kayıt defteri görevleri](container-registry-tasks-overview.md) (ACR oluşturma, test etme, gönderme ve azure'da görüntüleri dağıtma kolaylaştırmak için görevler). Örneğin, boşaltarak, geliştirme iç döngü buluta genişletmek için ACR görevleri kullanın `docker build` azure'a işlemleri. Kapsayıcı işletim sisteminizi ve çerçeve düzeltme eki uygulama işlem hattınızı otomatikleştirmek ve ekibiniz kaynak denetiminde kod yürüttüğünde otomatik olarak görüntü oluşturmak için oluşturma görevleri yapılandırın.

  [Çok adımlı görevler](container-registry-tasks-overview.md#multi-step-tasks) oluşturmaya, test etme ve bulutta kapsayıcı görüntülerini düzeltme adım tabanlı görev tanımı ve yürütme sağlayın. Görev adımları, tek tek kapsayıcı derleme ve gönderme işlemlerini tanımlar. Ayrıca, her adımın kapsayıcıyı kendi yürütme ortamı olarak kullanmasıyla bir veya daha fazla kapsayıcının yürütülmesini de tanımlayabilirler.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-portal.md)
* [Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md)
* [Kapsayıcı derleme ve bakım görevleri ACR ile otomatikleştirme](container-registry-tasks-overview.md)
