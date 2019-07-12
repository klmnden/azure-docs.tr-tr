---
title: Azure Container Registry etiketi ve sürüm görüntüleri
description: Kapsayıcı görüntüleri etiketleme ve sürüm oluşturma Docker için en iyi yöntemler
services: container-registry
author: stevelasker
ms.service: container-registry
ms.topic: article
ms.date: 07/10/2019
ms.author: steve.lasker
ms.openlocfilehash: bd00fd4f8dd247c766eb34849ecf9de603c5171b
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800399"
---
# <a name="recommendations-for-tagging-and-versioning-container-images"></a>Kapsayıcı görüntüleri etiketleme ve sürüm oluşturma için öneriler

Kapsayıcı kayıt defteri ve ardından bunları dağıtmak için dağıtma kapsayıcı görüntüleri gönderme, görüntü etiketleme ve sürüm oluşturma için bir strateji gerekir. Bu makalede, iki yaklaşım ve burada her kapsayıcı yaşam döngüsü boyunca uygun açıklanır:

* **Kararlı etiketleri** -ana belirtin veya ikincil sürüm gibi örneğin, yeniden etiketleri *mycontainerimage:1.0*.
* **Benzersiz etiket** -, anında bir kayıt defterine gibi her bir görüntü için farklı bir etiket *mycontainerimage:abc123*.

## <a name="stable-tags"></a>Kalıcı etiketler

**Öneri**: Kararlı etiketleri korumak için kullanmak **temel görüntüleri** kapsayıcınızı yapıları için. Dağıtımları kararlı etiketlerle kaçının, çünkü bu etiketleri güncelleştirmeleri almaya devam etmek ve üretim ortamlarında tutarsızlıklar ortaya çıkarabilir.

*Kararlı etiketleri* bir geliştirici ya da bir yapı sistemi devam edebilirsiniz güncelleştirmeleri almaya devam belirli bir etikete çekme anlamına gelir. Kararlı içeriği dondurulmuş anlamına gelmez. Bunun yerine, kararlı görüntüsü bu sürümün amacı için kararlı anlamına gelir. "Kararlı" kalmak için bu güvenlik düzeltme ekleri veya framework güncelleştirmeleri uygulamak için hizmet.

### <a name="example"></a>Örnek

Framework takım sürüm 1.0 ile birlikte gelir. Alışık oldukları küçük güncelleştirmeler dahil güncelleştirmeleri sevk. Kalıcı etiketler için belirli bir ana ve alt sürüm desteklemek için iki kararlı etiketlerin sahiptirler.

* `:1` – ana sürüm için kararlı bir etiket. `1` "Yeni" veya "en yeni" 1.* sürümünü temsil eder.
* `:1.0`-kararlı bir sürüm 1.0, 1.0 güncelleştirmeleri bağlama ve yayınlanmasının İleri 1.1 olarak toplanan değil, bir geliştiricinin vererek etiketi.

Ayrıca takım kullanır `:latest` hangi geçerli ana sürümü ne olursa olsun en son kararlı etiketin işaret etiketi.

Ne zaman temel görüntü güncelleştirmeleri kullanılabilir veya kararlı etiketlerle görüntüleri framework'ün sürüm hizmet verme her türlü sürümü en son kararlı sürüm temsil eden yeni Özet güncelleştirilir.

Bu durumda, büyük ve küçük etiketleri sürekli olarak bakımda. Temel görüntü senaryodan, bu hizmet verilen görüntüleri sağlamak görüntü sahibi sağlar.

## <a name="unique-tags"></a>Benzersiz etiketleri

**Öneri**: Benzersiz etiketleri kullanmak **dağıtımları**, özellikle birden çok düğümde ölçeklendirme bir ortamda. Büyük olasılıkla bilinçli dağıtımları bileşenleri sürümünün tutarlı olmasını istersiniz. Daha fazla örnek, kapsayıcı yeniden ya bir orchestrator ölçeklenen, konaklarınız yanlışlıkla, diğer düğümlerle tutarsız daha yeni bir sürümü çekme olmaz.

Benzersiz etiketleme yalnızca her resim bir kayıt defterine gönderilmiştir benzersiz etiket olduğu anlamına gelir. Etiketleri yeniden değil. Dahil olmak üzere, benzersiz bir etiket oluşturmak için izlemeniz gereken birkaç desen vardır:

* **Tarih ve saat damgası** -görüntü zaman oluşturulmuş açıkça söyleyebilirsiniz ortak oldukça, bu yaklaşım olduğu. Ancak, yapı sisteminizi yeniden ilişkilendirmek nasıl? Aynı anda tamamlanan derleme bulmak zorunda mıyım? Ne zaman dilimi içinde misiniz? Tüm derleme sistemler için UTC kalibre?
* **Git işleme** – temel görüntü güncelleştirmeleri destekleyen başlatana kadar bu yaklaşım çalışır. Temel görüntü güncelleştirme olursa, yapı sisteminizi önceki derleme olarak aynı Git işleme ile devreye devre dışı. Ancak, temel görüntü yeni içeriğe sahip. Genel olarak, Git işleme sağlar bir *yarı*-kararlı etiketi.
* **Bildirim özeti** -container registry'ye gönderildi her kapsayıcı görüntüsünün ilişkilendirilen bir bildirime sahip bir benzersiz SHA-256 karma veya Özet tanımlanmış. Benzersiz karşın, Özet uzun, okunması zor ve derleme ortamınızla uncorrelated içindir.
* **Derleme kimliği** -olası olduğundan bu seçenek en iyi yöntem olabilir artımlı ve tüm yapıtlar ve günlükleri bulmak için belirli yapı geri için bağıntısını kurmanızı sağlar. Ancak, bir bildirim Özet gibi bir insan okumak zor olabilir.

  Kuruluşunuz birkaç yapı sistemi varsa, bu seçenek bir değişim etiket yapı sistem adı ile önek olur: `<build-system>-<build-id>`. Örneğin, API takımın Jenkins derleme sistemi derleme ayırt ve sistem web takımın Azure işlem hatları oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki kavramları daha ayrıntılı bir açıklaması için blog gönderisine bakın [Docker etiketleme: Docker görüntüleri etiketleme ve sürüm oluşturma için en iyi yöntemler](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/).

Azure kapsayıcı kayıt defterinizin uygun maliyetli kullanımı ve performansı en üst düzeye çıkarmak için bkz: [en iyi uygulamalar için Azure Container Registry](container-registry-best-practices.md).

<!-- IMAGES -->


<!-- LINKS - Internal -->

