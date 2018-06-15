---
title: İşletim sistemi ve Azure kapsayıcı kayıt defteri yapısı ile (ACR yapı) framework düzeltme eki uygulama otomatikleştirme
description: Giriş ACR oluşturmak, kayıt defterinde Azure kapsayıcı güvenli, otomatik kapsayıcı görüntü yapı sağlayan özellikleri ve bulutta düzeltme eki uygulama paketi.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 05/01/2018
ms.author: marsma
ms.openlocfilehash: 3ef91270bceb5865bdbdf9c436e4519595a3dc09
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057783"
---
# <a name="automate-os-and-framework-patching-with-acr-build"></a>İşletim sistemi ve framework ACR yapısı ile düzeltme eki uygulama otomatikleştirme

Kapsayıcılar sanallaştırma, uygulama ve geliştirici bağımlılıklardan altyapı ve işletimsel gereksinimlerini yalıtma yeni düzeyleri sağlar. Ne kalır, ancak, bu uygulama sanallaştırma nasıl düzeltme eki adres gerek vardır.

**ACR yapı**, Azure kapsayıcı kayıt defteri, özeliklerin dizisi değil yalnızca yerel kapsayıcı görüntü yapı özelliği sağlar, ancak ayrıca otomatikleştirir [işletim sistemi ve framework düzeltme eki uygulama](#automate-os-and-framework-patching) Docker kapsayıcıları için.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="what-is-acr-build"></a>ACR yapı nedir?

Azure kapsayıcı kayıt defteri yapı bir Azure yerel kapsayıcı görüntü yapı hizmetidir. Derlemeler ACR yapı etkinleştirir iç döngü geliştirme isteğe bağlı kapsayıcı görüntü ile bulutta ve kaynak kod tamamlama ve temel görüntü üzerinde otomatik yapılarda güncelleştirin.

Tetikleyici kapsayıcı görüntü kodu bir Git deposuna işlendiğinde veya bir kapsayıcının temel görüntüsü güncelleştirildiğinde otomatik olarak oluşturur. Temel görüntü güncelleştirme tetikleyicileri ile işletim sistemi otomatik hale getirebilir ve uygulama çerçevesi değişmez kapsayıcıları ilkeleri için uymak sırasında güvenli ortamların sağlama iş akışı, düzeltme eki uygulama.

## <a name="quick-build-inner-loop-extended-to-the-cloud"></a>Hızlı derleme: iç-döngü buluta genişletilmiş

Geliştiriciler kendi ilk satırları kod tamamlama önce yaşam döngüsü yönetimi başlangıcını başlatır. ACR yapı'nın [hızlı derleme](container-registry-tutorial-quick-build.md) Özelliği Azure derlemeleri boşaltma bir tümleşik yerel iç döngü geliştirme deneyimi sağlar. Hızlı derlemeler ile kodunuzu gerçekleştirmeden önce otomatik derleme tanımları doğrulayabilirsiniz.

Bilinen kullanarak `docker build` biçimi [az acr yapı] [ az-acr-build] Azure CLI komutta yerel bir bağlam alan, ACR yapı hizmetine gönderir ve varsayılan olarak, kayıt sırasında yerleşik görüntü iter. tamamlama. ACR yapı, coğrafi olarak çoğaltılmış kayıt defterleri, en yakın çoğaltılmış kayıt defteri yararlanacağınızı dağınık geliştirme ekiplerinin etkinleştirme izler. Önizleme sırasında ACR yapı Doğu ABD ve Batı Avrupa bölgelerde kullanılabilir.

ACR yapı kapsayıcı ömrü temel tasarlanmıştır. Örneğin, ACR yapı CI/CD çözümünüze tümleştirin. Çalıştırarak [az oturum açma] [ az-login] ile bir [hizmet sorumlusu][az-login-service-principal], CI/CD çözümünüzü sonra verebilir [az acrderleme] [ az-acr-build] kazandırın için komutları yapıların görüntü.

Hızlı derlemeler ilk ACR yapı öğreticide kullanmayı öğrenin [yapı kapsayıcı görüntüleri bulutta Azure kapsayıcı kayıt defteri yapısı ile](container-registry-tutorial-quick-build.md).

## <a name="automatic-build-on-source-code-commit"></a>Kaynak kodu yürütme üzerinde otomatik derleme

Otomatik olarak bir kapsayıcı görüntüsü tetiklemek için derleme ACR kullanma kod bir Git deposuna işlendiğinde oluşturun. Derleme görevleri, Azure CLI komutu ile yapılandırılabilir [az acr yapı-görev][az-acr-build-task], bir Git deposu ve isteğe bağlı olarak bir dal ve Dockerfile belirtmenizi sağlar. Ekibinizin depoya kod onayladığında bir ACR yapı tarafından oluşturulan Web kancasını bağlantıların bulunması tanımlandı kapsayıcı görüntünün bir yapı tetikler.

Derlemeleri tetiklemek öğrenin ikinci ACR yapı öğreticide, kaynak kodu yürütme üzerinde [otomatikleştirme kapsayıcı görüntü derlemeler Azure kapsayıcı kayıt defteri yapısı ile](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>İşletim sistemi ve framework düzeltme eki uygulama otomatikleştirme

Kapsayıcı yapı hattınızı gerçekten geliştirmek için güç ACR derlemesi temel görüntü için bir güncelleştirme algılamak için kendi yeteneği gelir. Güncelleştirilmiş temel görüntü kayıt defterine gönderildiğinde ACR yapı dayalı tüm uygulama görüntüleri otomatik olarak oluşturabilirsiniz.

Kapsayıcı görüntüleri geniş kategorilere içine *temel* görüntüler ve *uygulama* görüntüler. Temel görüntülerinizi genellikle işletim sistemi ve uygulama çerçeveleri bağlı uygulamanızın, diğer özelleştirmelerin birlikte yapılandırıldığı içerir. Bu temel görüntüler kendilerini genellikle ortak Yukarı Akış görüntülerinde örneğin dayalı olan [Alpine Linux] [ base-alpine] veya [Node.js][base-node]. Birçok uygulama görüntülerinizin ortak bir temel görüntü paylaşabilen.

Bir işletim sistemi veya uygulama framework görüntü Yukarı Akış Bakımcı tarafından güncelleştirildiğinde, örneğin bir kritik işletim sistemi güvenlik düzeltme ekiyle da temel görüntülerinizi kritik düzeltme içerecek şekilde güncelleştirmeniz gerekir. Her uygulama görüntüsü sonra da artık temel yansımanıza bu Yukarı Akış düzeltmelerin dahil etmek için yeniden oluşturulması gerekir.

Bir kapsayıcı görüntüsü oluşturduğunda ACR yapı dinamik olarak temel görüntü bağımlılıkları bulur çünkü uygulama görüntünün temel görüntüsü güncelleştirildiğinde algılayabilir. Önceden yapılandırılmış biriyle [görev yapı](container-registry-tutorial-base-image-update.md#create-build-task), ACR yapı ardından **her uygulama görüntüsü otomatik olarak yeniden oluşturur** sizin için. Bu otomatik algılama ve yeniden oluşturma, ACR yapı kaydeder, zaman ve çaba normalde el ile izlemek ve her uygulama güncelleştirmek için gereken güncelleştirilmiş temel görüntünüzde başvuran görüntü ile.

İşletim sistemi ve üçüncü ACR yapı öğreticide framework düzeltme eki uygulama hakkında bilgi edinin [otomatikleştirme görüntü, Azure kapsayıcı kayıt defteri yapısı ile temel görüntü Update'te derlemeler](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Yalnızca temel ve uygulama görüntüleri aynı Azure kapsayıcı kayıt defterinde bulunuyorsa ilk önizleme için temel görüntü güncelleştirmeleri tetikleyici oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

İşletim sistemi ve kapsayıcı görüntülerinizi bulutta oluşturarak düzeltme eki uygulama çerçevesi otomatikleştirmek hazır olduğunuzda, üç ACR yapı öğretici bölümlük denetleyin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı kayıt defteri yapısı ile bulutta kapsayıcı görüntüleri oluşturma](container-registry-tutorial-quick-build.md)

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
