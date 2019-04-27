---
title: İşleç en iyi uygulamalar - kapsayıcı görüntüsü Yönetimi Azure Kubernetes Hizmetleri (AKS)
description: Yönetme ve güvenli kapsayıcı görüntülerini Azure Kubernetes Service (AKS) için küme işleci en iyi uygulamaları öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: iainfou
ms.openlocfilehash: 1cc91f55d3895f06176875cb9ae620685dc09a26
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60464816"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Kapsayıcı görüntü yönetimi ve güvenliği Azure Kubernetes Service (AKS) için en iyi uygulamalar

Kapsayıcılar ve kapsayıcı görüntüleri güvenliğini geliştirmek ve Azure Kubernetes Service (AKS) uygulamaları çalıştırma gibi anahtar bir noktadır. Eski içeren kapsayıcı görüntülerini temel veya yüklenmemiş bir uygulama çalışma zamanını bir güvenlik riski ve olası bir saldırı vektörü tanıtır. Bu riskleri azaltmak için tarayın ve çalışma zamanı yanı sıra derleme zamanını kapsayıcılarınızı sorunları düzeltmek araçları tümleşmelidir. Güvenlik Açığı veya güncel temel görüntü olarak yakalanır işlemde daha önce küme daha güvenli hale getirin. Bu makalede, *kapsayıcıları* kapsayıcı kayıt defteri ve çalışan kapsayıcıları depolanan her iki kapsayıcı görüntülerini anlamına gelir.

Bu makalede aks'deki kapsayıcılarınızı güvenliğini nasıl ele alınmaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Tarama ve resim güvenlik açıklarını düzeltin
> * Güvenilir bir kayıt defteri ile imzalanmış kapsayıcı görüntülerini kullanma
> * Otomatik olarak tetikleyin ve temel görüntüsü güncelleştirildiğinde kapsayıcı görüntülerini yeniden dağıtma

Ayrıca, en iyi yöntemler için okuyabilirsiniz [küme güvenlik] [ best-practices-cluster-security] ve [pod güvenlik][best-practices-pod-security].

## <a name="secure-the-images-and-run-time"></a>Görüntüleri güvenli ve çalışma zamanı

**En iyi uygulama kılavuzunu** - kapsayıcı görüntülerinizi güvenlik açıkları için tarayın ve yalnızca doğrulama başarılı görüntüleri dağıtma. Düzenli olarak uygulama çalışma zamanı ve temel görüntüleri güncelleştirin, sonra AKS kümesinde iş yükleri yeniden dağıtın.

Bir kapsayıcı tabanlı iş yüklerini benimsenmesini içeriğiyle görüntüleri ve kendi uygulamalarınızı oluşturmak için kullanılan çalışma zamanı güvenliğini doğruluyor. Nasıl güvenlik açıklarını dağıtımlarınızı açmadığınızdan emin olabilirim? Dağıtım iş akışı gibi araçları kullanarak kapsayıcı görüntülerini taramak için bir işlem içermelidir [Twistlock] [ twistlock] veya [Açık Deniz Mavisi][aqua]ve ardından yalnızca dağıtılacak doğrulanmış görüntüler sağlar.

![Tarama ve düzeltme kapsayıcı görüntüleri, doğrulamak ve dağıtma](media/operator-best-practices-container-security/scan-container-images-simplified.png)

Gerçek hayatta kullanılan örnekte, görüntü tarama, doğrulama ve dağıtımı otomatik hale getirmek için bir sürekli tümleştirme ve sürekli dağıtım (CI/CD) işlem hattı kullanabilirsiniz. Azure Container Registry açıklarından tarama özellikleri içerir.

## <a name="use-a-trusted-registry"></a>Güvenilir bir kayıt defteri kullanma

**En iyi uygulama kılavuzunu** - sınırlamak, pod'ların görüntü kayıt defterleri ve dağıtımları kullanabilirsiniz. Yalnızca Burada doğrulayın ve kullanılabilir resimlerini denetleme güvenilen kayıt defterleri izin verir.

Ek güvenlik için uygulama kodunuzun yalnızca dijital olarak imzalayabilirsiniz gibi kapsayıcı görüntülerinizi de dijital olarak imzalayabilirsiniz. Ardından yalnızca imzalı görüntülerini dağıtmak için AKS izin. Bu işlem, yalnızca dijital olarak imzalanmış ve size, bir güvenlik açığı denetimi başarılı değil yalnızca görüntüleri güvenilen görüntüleri çekmek için AKS sınırlamak, ek bir güvenlik katmanı sağlar. Ayrıca kapsayıcı görüntüsünü edilmemiş değiştirilmiş ve tam aynı ada sahip bir görüntü yerine emin emin olun.

Dijital olarak imzalanmış kapsayıcı görüntülerini sağlayan güvenilir kayıt defterleri karmaşıklığı ortamınıza ekleyebilirsiniz, ancak belirli bir ilke veya yönetmeliklere uygunluk için gerekli olabilir. Azure Container Registry güvenilen kayıt defterleri destekler ve görüntüleri imzalanmış.

Dijital olarak imzalanmış görüntüleri hakkında daha fazla bilgi için bkz. [içerik Azure Container Registry güvende][acr-content-trust].

## <a name="automatically-build-new-images-on-base-image-update"></a>Yeni görüntü temel görüntü güncelleştirme otomatik olarak oluşturma

**En iyi uygulama kılavuzunu** - temel görüntü güncelleştirilirken yeni görüntüleri oluşturmak için kullanımı Otomasyon uygulama görüntüleri için temel görüntüleri olarak. Bu temel görüntüleri genellikle güvenlik düzeltmelerini içeren herhangi bir aşağı akış uygulaması kapsayıcı görüntülerini güncelleştirin.

Temel görüntü her güncelleştirildiğinde tüm aşağı akış kapsayıcı görüntülerini de güncelleştirilmesi gerekir. Doğrulama ve dağıtım işlem hatları gibi bu yapı işlemini tümleşik [Azure işlem hatları] [ azure-pipelines] veya Jenkins. Bu işlem hatları uygulamalarınızı güncelleştirilmiş temel görüntüleri üzerinde çalışmaya devam ettiğinden emin yapar. Uygulama kapsayıcı görüntülerinizi doğrulandıktan sonra AKS dağıtımları daha sonra en son, güvenli görüntülerini çalıştırmak için güncelleştirilebilir.

Temel görüntü güncelleştirildiğinde azure kapsayıcı kayıt defteri görevleri kapsayıcı görüntülerini otomatik olarak güncelleştirebilirsiniz. Bu özellik, az sayıda temel görüntüleri oluşturmak ve düzenli olarak hata ve güvenlik düzeltmeleri ile güncel kalmasını sağlar.

Temel görüntü güncelleştirmeleri hakkında daha fazla bilgi için bkz. [otomatikleştirme görüntü derlemeleri temel görüntü güncelleştirme Azure kapsayıcı kayıt defteri görevlerle][acr-base-image-update].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kapsayıcılarınızı güvenli nasıl odaklanır. Bu alanlardan bazıları uygulamak için aşağıdaki makalelere bakın:

* [Azure kapsayıcı kayıt defteri görevleri olan temel görüntü güncelleştirme görüntü oluşturmayı otomatikleştirme][acr-base-image-update]
* [Azure Container Registry'de içeriğine güven][acr-content-trust]

<!-- EXTERNAL LINKS -->
[azure-pipelines]: /azure/devops/pipelines/?view=vsts
[twistlock]: https://www.twistlock.com/
[aqua]: https://www.aquasec.com/

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[acr-content-trust]: ../container-registry/container-registry-content-trust.md
[acr-base-image-update]: ../container-registry/container-registry-tutorial-base-image-update.md
