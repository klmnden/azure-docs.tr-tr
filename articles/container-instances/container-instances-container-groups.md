---
title: "Kapsayıcı grupları Azure kapsayıcı örnekleri"
description: "Kapsayıcı grupları Azure kapsayıcı durumlarda nasıl çalıştığını anlamak"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 568a99d44a5a32339d438ed1025670d12ecce791
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Azure kapsayıcı durumlarda kapsayıcı grupları

Azure kapsayıcı durumlarda en üst düzey kaynak bir kapsayıcı grubudur. Bu makalede, kapsayıcı grupları nedir ve ne tür senaryoların bunlar etkinleştirmek açıklanmaktadır.

## <a name="how-a-container-group-works"></a>Kapsayıcı grubu nasıl çalışır?

Kapsayıcı grubu, aynı ana bilgisayar makinesinde zamanlanmış ve bir yaşam döngüsü, yerel ağ ve depolama birimleri paylaşan kapsayıcıları koleksiyonudur. Kavramı, benzer bir *pod* içinde [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) ve [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

Aşağıdaki diyagramda, birden çok kapsayıcı içeren bir kapsayıcı grubu örneği gösterilmektedir.

![Kapsayıcı grupları örneği][container-groups-example]

Şunlara dikkat edin:

- Grubun tek ana makinede zamanlandı.
- Grubun tek bir ortak IP adresi, bir kullanıma sunulan bağlantı noktası ile kullanıma sunar.
- Grup iki kapsayıcıları için yapılır. Bağlantı noktası 80 sırasında diğer dinlediği bağlantı noktası 5000 bir kapsayıcı dinler.
- İki Azure dosya paylaşımları birim başlatmalar olarak grubu içerir ve her kapsayıcı paylaşımları yerel olarak birini bağlar.

### <a name="networking"></a>Ağ

Kapsayıcı grupları, bir IP adresi ve bağlantı noktası ad alanı, IP adresi üzerinde paylaşır. Grup içindeki bir kapsayıcı erişmek dış istemcileri etkinleştirmek için bağlantı noktası üzerinde IP adresi ve kapsayıcısından kullanıma gerekir. Grup kapsayıcılara bir bağlantı noktası ad alanı paylaştığından, bağlantı noktası eşlemesi desteklenmiyor. Bu bağlantı noktalarını grubun IP adresinde dışarıdan gösterilmeyen olsa bile bir grup kapsayıcılara bunlar sunulan bağlantı noktalarında localhost aracılığıyla birbirine ulaşabilirsiniz.

### <a name="storage"></a>Depolama

İçindeki bir kapsayıcı grubun bağlamak için dış birimleri belirtebilirsiniz. Tek bir grup kapsayıcılarında içindeki belirli yollara içine bu birimlerin eşleyebilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Birden çok kapsayıcı grupları tek bir işlev görev farklı ekip tarafından alınabilir ve ayrı kaynak gereksinimlerini kapsayıcı görüntüleri az sayıda içine bölmek istediğiniz durumlarda kullanışlıdır. Örnek Kullanım dahil olabilir:

- Bir uygulama kapsayıcısı ve günlüğe kaydetme kapsayıcı. Günlüğe kaydetme kapsayıcısı ana uygulama tarafından çıktısı günlükler ve ölçümleri toplar ve uzun vadeli depolama için yazar.
- Bir uygulama ve izleme kapsayıcı. İzleme kapsayıcı düzenli aralıklarla çalıştığını ve düzgün yanıt ve değilse bir uyarı başlatır emin olmak için uygulamaya istekte bulunur.
- Bir web uygulaması hizmet veren bir kapsayıcı ve en yeni içerik kaynak denetiminden çekme kapsayıcı.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [çok kapsayıcı grubu dağıtma](container-instances-multi-container-group.md) bir Azure Resource Manager şablonu ile.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png