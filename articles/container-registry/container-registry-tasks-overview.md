---
title: İşletim sistemi ve Azure Container kayıt defteri görevleri ile (ACR) framework düzeltme eki uygulama otomatikleştirin
description: Giriş ACR görevlere güvenliğini sağlayan Azure Container Registry'de özellikleri içeren bir paketi otomatik olarak kapsayıcı görüntüsü derleme ve bulutta düzeltme eki uygulama.
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: marsma
ms.openlocfilehash: 3ba08a0c2422adcd987fa6ed601b666106e8503c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048319"
---
# <a name="automate-os-and-framework-patching-with-acr-tasks"></a>İşletim sistemi ve framework ACR görevlerle düzeltme eki uygulama otomatikleştirin

Kapsayıcılar, sanallaştırma, altyapı ve işletimsel gereksinimleri gelen uygulama ve geliştirici bağımlılıkları yalıtarak yeni düzeyleri sağlar. Ne kalır, ancak bu uygulama sanallaştırma nasıl yama adres gerekli değildir.

## <a name="what-is-acr-tasks"></a>ACR görevleri nedir?

**ACR görevleri** Azure Container Registry özeliklerin paketidir. Linux, Windows ve ARM için bulut tabanlı bir kapsayıcı görüntüsü oluşturma sağlar ve otomatik hale getirebilirsiniz [işletim sistemi ve framework düzeltme eki uygulama](#automate-os-and-framework-patching) Docker kapsayıcıları için. ACR görevleri yalnızca "İç döngü" geliştirme döngünüzün isteğe bağlı kapsayıcı görüntüsü derleme ile buluta genişletir, ancak aynı zamanda kaynak kodun işlenmesinden ya da bir kapsayıcının temel görüntü güncelleştirildiğinde otomatik derlemeler sağlar. Temel görüntü güncelleştirme tetikleyicilerle işletim sisteminizi otomatikleştirebilir ve sabit kapsayıcıların Sorumlular için bağlılığı sırasında güvenli ortamları Bakımı iş akışı, düzeltme eki uygulama çerçevesi.

Derleme ve kapsayıcı görüntüleri ACR görevlerle dört yöntemle test edin:

* [Hızlı görev](#quick-task): oluşturun ve yerel bir Docker altyapısının yüklemesine gerek kalmadan, Azure'da kapsayıcı görüntüleri isteğe bağlı olarak gönderin. Düşünme `docker build`, `docker push` bulutta. Yerel kaynak kodu veya bir Git deposu oluşturun.
* [Kaynak kod işlemesinde derleme](#automatic-build-on-source-code-commit): bir kapsayıcı görüntüsü derleme kodu bir Git deposu için kabul edilen olduğunda otomatik olarak tetikleyin.
* [Temel görüntü güncelleştirme derleme](#automate-os-and-framework-patching): Bu görüntünün temel görüntü güncelleştirildiğinde kapsayıcı görüntü derlemesi tetikleyin.
* [Çok adımlı görevler](#multi-step-tasks-preview) (Önizleme): görüntülerinizi oluşturmak için kapsayıcıları komutları çalıştırın ve bir kayıt defterine görüntü gönderme çok adımlı görevler tanımlayın. İsteğe bağlı görev yürütme ACR görevleri bu önizleme özelliğini destekler ve paralel görüntüsü derleme, test ve işlemleri gönderin.

## <a name="quick-task"></a>Hızlı görev

İç döngü geliştirme döngüsü, oluşturma ve kaynak denetimine gerçekleştirmeden önce uygulamanızı test etme kod yazma, yinelemeli işleminin gerçekten kapsayıcı yaşam döngüsü yönetimi başlangıcıdır.

Kodunuzun ilk satırını göndermeden önce kod, ACR Görevler'ın [hızlı görev](container-registry-tutorial-quick-task.md) özelliği, kapsayıcı görüntünüzü boşaltma tarafından bir tümleşik geliştirme deneyimi için Azure yapılar sağlayabilir. Hızlı Görevler ile otomatikleştirilmiş derleme tanımları ve kodunuzu uygulamadan önce olası sorunları catch doğrulayabilirsiniz.

Tanıdık kullanarak `docker build` biçimi [az acr build] [ az-acr-build] Azure CLI komutu alır bir *bağlam* (oluşturmak için dosya kümesini), bu ACR görevler gönderir ve varsayılan olarak, oluşturulan görüntüyü tamamlandıktan sonra kayıt defterine gönderir.

Aşağıdaki tabloda, ACR görevleri için desteklenen içerik konumları bazı örnekler gösterilmektedir:

| İçerik konumu | Açıklama | Örnek |
| ---------------- | ----------- | ------- |
| Yerel dosya sistemi | Yerel dosya sisteminde dosya içinde bir dizin. | `/home/user/projects/myapp` |
| GitHub, ana dal | Bir GitHub deposu dalında dosyaları ana (veya diğer varsayılan).  | `https://github.com/gituser/myapp-repo.git` |
| GitHub dal | GitHub deposunun belirli dal.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub çekme isteği | Çekme isteğinde bir GitHub deposu. | `https://github.com/gituser/myapp-repo.git#pull/23/head` |
| GitHub alt | GitHub deposunda bir alt klasör içinde dosyalar. Örnek, çekme isteği ve alt klasör belirtimi birleşimi gösterir. | `https://github.com/gituser/myapp-repo.git#pull/24/head:myfolder` |
| Uzak tarball | Sıkıştırılmış bir arşiv dosyaları uzak Web sunucusu üzerinde. | `http://remoteserver/myapp.tar.gz` |

ACR görevler, kapsayıcı yaşam temel tasarlanmıştır. Örneğin, CI/CD çözümünüze ACR görevleri tümleştirin. Yürüterek [az login] [ az-login] ile bir [hizmet sorumlusu][az-login-service-principal], CI/CD çözümünüzü ardından yayımlayabilir [azacrderlemesi] [ az-acr-build] tanıtımıyla için komutları yapıların görüntü.

Hızlı Görevler ilk ACR görevleri öğreticisinde kullanmayı öğrenin [derleme kapsayıcı görüntülerini Azure Container kayıt defteri görevler ile bulutta](container-registry-tutorial-quick-task.md).

## <a name="automatic-build-on-source-code-commit"></a>Otomatik yapı üzerinde kaynak kodu yürütme

Bir Git deposuna kod işlendiğinde otomatik olarak bir kapsayıcı görüntüsü tetiklemek için kullanım ACR Görevler oluşturun. Derleme görevleri, Azure CLI komutuyla yapılandırılabilir [az acr görev][az-acr-task], bir Git deposu ve isteğe bağlı olarak bir dal ve Dockerfile belirtmenizi sağlar. Takımınızın kod deposuna kaydeder. ACR görevler tarafından oluşturulan bir Web kancası depoda tanımlanan kapsayıcı görüntüsünün bir derleme tetikler.

> [!IMPORTANT]
> Önizleme sırasında görevlerin daha önce oluşturduysanız `az acr build-task` komutu kullanılarak yeniden oluşturulması gereken görevleri [az acr görev] [ az-acr-task] komutu.

Yapılar tetikleme hakkında bilgi edinin kaynak kod işlemesinde ikinci ACR görevleri öğreticide [otomatikleştirme kapsayıcı görüntüsü Azure Container kayıt defteri görevlerle yapılar](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>İşletim sistemi ve framework düzeltme eki uygulama otomatikleştirin

ACR görevleri gücünden gerçek anlamda kapsayıcı yapı iş geliştirmek için bir temel görüntü için bir güncelleştirme algılama özelliğini, gelir. Güncelleştirilmiş temel görüntüyü kayıt defterinize itilir, ACR görevleri otomatik olarak bunu temel alan herhangi bir uygulama görüntü oluşturabilirsiniz.

Kapsayıcı görüntüleri problem kategorilere içine *temel* görüntüleri ve *uygulama* görüntüler. Temel görüntülerinizi genellikle uygulama çerçeveleri temellendirildiği uygulamanızı, diğer özelleştirmelere yanı sıra yerleşik olarak bulunur ve işletim sistemi içerir. Bu temel görüntüleri kendilerini genellikle ortak Yukarı Akış görüntülerinde örneğin göre olan: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet], veya [Node.js][base-node]. Birçok uygulama görüntülerinizin genel bir temel görüntü paylaşabilir.

Yukarı Akış Bakımcı tarafından bir işletim sistemi veya uygulama çerçevesi görüntüsü güncelleştirildiğinde, örneğin bir kritik işletim sistemi güvenlik düzeltme ekiyle da temel görüntülerinizi kritik düzeltme içerecek şekilde güncelleştirmeniz gerekir. Her uygulama görüntüsü daha sonra da artık, temel görüntüye dahil bu Yukarı Akış düzeltmeler içerecek şekilde yeniden oluşturulması gerekir.

Bir kapsayıcı görüntüsü oluşturduğunda ACR görevleri temel görüntü bağımlılıkları dinamik olarak bulur. çünkü uygulama görüntünün temel görüntüsü güncelleştirildiğinde algılayabilir. Önceden yapılandırılmış bir [görev derleme](container-registry-tutorial-base-image-update.md#create-a-task), ardından ACR görevleri **otomatik olarak her uygulama görüntüsünü yeniden oluşturur** sizin için. Bu otomatik algılama ve yeniden oluşturma, ACR görevleri kaydeder, zaman ve çaba normalde el ile izlemek ve her uygulama güncelleştirmesi için gereken güncelleştirilmiş temel görüntünüzü başvuran görüntü ile.

İşletim sistemi ve üçüncü ACR görevleri öğreticide framework düzeltme eki uygulama hakkında [otomatikleştirme görüntü derlemeleri temel görüntü güncelleştirme Azure kapsayıcı kayıt defteri görevlerle](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Temel görüntü yalnızca hem temel sınıfını hem uygulama görüntüleri aynı Azure container Registry'de bulunabilir veya temel bir genel Docker Hub deposundaki bulunduğu tetikleyici yapılar güncelleştirir.

## <a name="multi-step-tasks-preview"></a>Çok adımlı görevler (Önizleme)

Çok adımlı görevler, bir önizleme özelliği, ACR görevleri adım tabanlı görev tanımı ve derleme, test etme ve bulutta kapsayıcı görüntülerini düzeltme eki uygulama için yürütme sağlar. Görev adımları, tek tek kapsayıcı görüntü derlemesi tanımlama ve işlemleri gönderin. Bunlar ayrıca, yürütme ortamı olarak kullanarak kapsayıcının her adımını içeren bir veya daha fazla kapsayıcı yürütülmesini tanımlayabilirsiniz.

Örneğin, aşağıdaki otomatik hale getiren çok adımlı görev oluşturabilirsiniz:

1. Bir web uygulama görüntüsü oluşturun
1. Web uygulaması kapsayıcısını çalıştırın
1. Bir web uygulamasını test görüntüsü oluşturun
1. Testleri çalıştırma karşı gerçekleştiren web uygulaması test kapsayıcısının çalıştırmak uygulama kapsayıcısı
1. Testler başarıyla geçirilirse, bir Helm grafiği arşiv paketi oluşturun
1. Gerçekleştirmek bir `helm upgrade` yeni Helm grafiği arşiv paketi kullanma

Çok adımlı görevler oluşturma, çalıştırma ve arası adım bağımlılık desteğiyle daha birleştirilebilir adımlara görüntüsü test bölmek etkinleştirin. ACR görevleri çok adımlı görevler ile görüntü oluşturma, sınama ve işletim sistemi ve iş akışları düzeltme eki uygulama çerçevesi daha ayrıntılı denetime sahip.

Çok adımlı görevler hakkında bilgi edinin [ACR görevleri çok adımlı derleme, test ve düzeltme eki görevleri çalıştırmak](container-registry-tasks-multi-step.md).

> [!IMPORTANT]
> ACR görevleri çok adımlı görev özelliği şu anda Önizleme aşamasındadır. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özellik bazı yönleri genel kullanıma (GA) açılmadan önce değişebilir

## <a name="next-steps"></a>Sonraki adımlar

İşletim sistemi ve bulutta kapsayıcı görüntülerinizi oluşturarak düzeltme eki uygulama çerçevesi otomatikleştirmek hazır olduğunuzda, üç bölümden ACR görevleri öğretici serisini denetleyin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı kayıt defteri görevler ile bulutta kapsayıcı görüntüleri oluşturma](container-registry-tutorial-quick-task.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task]: /cli/azure/acr#az-acr-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png