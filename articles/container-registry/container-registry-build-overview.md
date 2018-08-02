---
title: İşletim sistemi ve Azure Container kayıt defteri Build ile (ACR Build) framework düzeltme eki uygulama otomatikleştirin
description: Giriş ACR Build, güvenliğini sağlayan Azure Container Registry'de özellikleri içeren bir paketi otomatik olarak kapsayıcı görüntüsü derleme ve bulutta düzeltme eki uygulama.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 08/01/2018
ms.author: marsma
ms.openlocfilehash: 63bbd9b5711330207c34ac4aa05aac3a71304653
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413588"
---
# <a name="automate-os-and-framework-patching-with-acr-build"></a>İşletim sistemi ve framework ACR derlemesi ile düzeltme eki uygulama otomatikleştirin

Kapsayıcılar, sanallaştırma, altyapı ve işletimsel gereksinimleri gelen uygulama ve geliştirici bağımlılıkları yalıtarak yeni düzeyleri sağlar. Ne kalır, ancak bu uygulama sanallaştırma nasıl yama adres gerekli değildir.

**ACR Build** Azure Container Registry özeliklerin paketidir. Linux, Windows ve ARM için bulut tabanlı bir kapsayıcı görüntüsü oluşturma sağlar ve otomatik hale getirebilirsiniz [işletim sistemi ve framework düzeltme eki uygulama](#automate-os-and-framework-patching) Docker kapsayıcıları için.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="what-is-acr-build"></a>ACR Build nedir?

Azure kapsayıcı kayıt defteri oluşturmak bir Azure yerel kapsayıcı görüntüsü derleme hizmetidir. ACR Build etkinleştirir iç döngü geliştirme bulutta isteğe bağlı kapsayıcı görüntüsü derler ve kaynak kod tamamlama ve temel görüntüye otomatik derlemeler güncelleştirin.

Tetikleyici kapsayıcı görüntüsü, bir Git deposuna kod işlendiğinde veya bir kapsayıcının temel görüntüsü güncelleştirildiğinde otomatik olarak oluşturur. Temel görüntü güncelleştirme tetikleyicilerle işletim sisteminizi otomatikleştirebilir ve sabit kapsayıcıların Sorumlular için bağlılığı sırasında güvenli ortamları Bakımı iş akışı, düzeltme eki uygulama çerçevesi.

## <a name="quick-build-inner-loop-extended-to-the-cloud"></a>Hızlı derleme: iç buluta genişletilmiş döngü

Geliştiriciler kendi ilk kod satırlarını göndermeden önce yaşam döngüsü yönetimi başına başlatır. ACR Build'ın [hızlı derleme](container-registry-tutorial-quick-build.md) özellik yapıları azure'a aktarmasını bir tümleşik yerel iç döngü geliştirme deneyimi sağlar. Hızlı derlemeler ile kodunuzu uygulamadan önce otomatik derleme tanımlarını doğrulayabilirsiniz.

Tanıdık kullanarak `docker build` biçimi [az acr build] [ az-acr-build] Azure CLI komutu alır bir **bağlam** (oluşturmak için dosya kümesi) için ACR Build service gönderen Ayrıca, varsayılan olarak, kendi kayıt tamamlandıktan sonra oluşturulan görüntüyü gönderir.

Aşağıdaki tabloda, ACR derlemesi için desteklenen içerik konumları bazı örnekler gösterilmektedir:

| İçerik konumu | Açıklama | Örnek |
| ---------------- | ----------- | ------- |
| Yerel dosya sistemi | Yerel dosya sisteminde dosya içinde bir dizin. | `/home/user/projects/myapp` |
| GitHub, ana dal | Bir GitHub deposu dalında dosyaları ana (veya diğer varsayılan).  | `https://github.com/gituser/myapp-repo.git` |
| GitHub dal | GitHub deposunun belirli dal.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub çekme isteği | Çekme isteğinde bir GitHub deposu. | `https://github.com/gituser/myapp-repo.git#pull/23/head` |
| GitHub alt | GitHub deposunda bir alt klasör içinde dosyalar. Örnek, çekme isteği ve alt klasör belirtimi birleşimi gösterir. | `https://github.com/gituser/myapp-repo.git#pull/24/head:myfolder` |
| Uzak tarball | Sıkıştırılmış bir arşiv dosyaları uzak Web sunucusu üzerinde. | `http://remoteserver/myapp.tar.gz` |

ACR Build Ayrıca, coğrafi çoğaltmalı kayıt defterleri, dağınık geliştirme takımları yakın çoğaltılmış bir kayıt defteri yararlanmak etkinleştirme izler.

ACR Build kapsayıcı yaşam temel tasarlanmıştır. Örneğin, ACR Build CI/CD çözümünüze tümleştirin. Yürüterek [az login] [ az-login] ile bir [hizmet sorumlusu][az-login-service-principal], CI/CD çözümünüzü ardından yayımlayabilir [azacrderlemesi] [ az-acr-build] tanıtımıyla için komutları yapıların görüntü.

Hızlı derlemeler ilk ACR Build öğreticisinde kullanmayı öğrenirsiniz [bulutta kapsayıcı görüntülerini Azure Container kayıt defteri yapısı ile yapı](container-registry-tutorial-quick-build.md).

## <a name="automatic-build-on-source-code-commit"></a>Otomatik yapı üzerinde kaynak kodu yürütme

Kullanım, otomatik olarak bir kapsayıcı görüntüsü tetiklemek için ACR derlemesi bir Git deposuna kod işlendiğinde derleme. Derleme görevleri, Azure CLI komutuyla yapılandırılabilir [az acr derleme görevi][az-acr-build-task], bir Git deposu ve isteğe bağlı olarak bir dal ve Dockerfile belirtmenizi sağlar. Takımınızın kod deposuna kaydeder. ACR derlemesi tarafından oluşturulan bir Web kancası depoda tanımlanan kapsayıcı görüntüsünün bir derleme tetikler.

Yapılar tetikleme hakkında bilgi edinin kaynak kod işlemesinde ikinci ACR Build öğreticide [otomatikleştirme kapsayıcı görüntüsü Azure Container kayıt defteri Build ile oluşturur](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>İşletim sistemi ve framework düzeltme eki uygulama otomatikleştirin

Kapsayıcı derleme işlem hattınızı gerçek anlamda geliştirmek için gücünü ACR Build temel görüntü için bir güncelleştirme algılama özelliğini, gelir. ACR Build güncelleştirilmiş temel görüntüyü kayıt defterinize itilir, otomatik olarak bunu temel alan herhangi bir uygulama görüntü oluşturabilirsiniz.

Kapsayıcı görüntüleri problem kategorilere içine *temel* görüntüleri ve *uygulama* görüntüler. Temel görüntülerinizi genellikle uygulama çerçeveleri temellendirildiği uygulamanızı, diğer özelleştirmelere yanı sıra yerleşik olarak bulunur ve işletim sistemi içerir. Bu temel görüntüleri kendilerini genellikle ortak Yukarı Akış görüntülerinde örneğin göre olan: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet], veya [Node.js][base-node]. Birçok uygulama görüntülerinizin genel bir temel görüntü paylaşabilir.

Yukarı Akış Bakımcı tarafından bir işletim sistemi veya uygulama çerçevesi görüntüsü güncelleştirildiğinde, örneğin bir kritik işletim sistemi güvenlik düzeltme ekiyle da temel görüntülerinizi kritik düzeltme içerecek şekilde güncelleştirmeniz gerekir. Her uygulama görüntüsü daha sonra da artık, temel görüntüye dahil bu Yukarı Akış düzeltmeler içerecek şekilde yeniden oluşturulması gerekir.

Bir kapsayıcı görüntüsü oluşturduğunda ACR Build dinamik olarak temel görüntü bağımlılıkları bulur çünkü uygulama görüntünün temel görüntüsü güncelleştirildiğinde algılayabilir. Önceden yapılandırılmış bir [görev derleme](container-registry-tutorial-base-image-update.md#create-build-task), ACR Build ardından **otomatik olarak her uygulama görüntüsünü yeniden oluşturur** sizin için. Bu otomatik algılama ve yeniden oluşturma, ACR Build kaydeder, zaman ve çaba normalde el ile izlemek ve her uygulama güncelleştirmesi için gereken güncelleştirilmiş temel görüntünüzü başvuran görüntü ile.

İşletim sistemi ve üçüncü ACR Build öğreticisinde framework düzeltme eki uygulama hakkında [otomatikleştirme görüntü Azure kapsayıcı kayıt defteri Build ile temel görüntü güncelleştirme derlemeleri](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Yalnızca temel ve uygulama görüntüleri aynı Azure kapsayıcı kayıt defteri veya genel olarak erişilebilir Docker Hub depoları bulunuyorsa ilk önizleme için temel görüntü güncelleştirme tetikleyici oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

İşletim sistemi ve bulutta kapsayıcı görüntülerinizi oluşturarak düzeltme eki uygulama çerçevesi otomatikleştirmek hazır olduğunuzda, üç bölümden ACR oluşturma Öğreticisi dizisinin denetleyin.

> [!div class="nextstepaction"]
> [Azure Container Registry Derlemesi ile bulutta kapsayıcı görüntüleri derleme](container-registry-tutorial-quick-build.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
