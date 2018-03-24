---
title: Azure kapsayıcı örnekleri kapsayıcı grupları
description: Kapsayıcı grupları Azure kapsayıcı durumlarda nasıl çalıştığını anlamak
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 03/20/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 3b1eeebacb55ffc7af4e2014f26dd9d5643f5478
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Azure kapsayıcı durumlarda kapsayıcı grupları

Azure kapsayıcı durumlarda en üst düzey kaynak *kapsayıcı grubu*. Bu makalede, kapsayıcı grupları nelerdir ve bunlar etkinleştirme senaryoları türleri açıklanmaktadır.

## <a name="how-a-container-group-works"></a>Kapsayıcı grubu nasıl çalışır?

Kapsayıcı grubu, aynı ana bilgisayar makinesinde zamanlanmış kapsayıcıları koleksiyonudur. Kapsayıcı grubu kapsayıcılarında bir yaşam döngüsü, yerel ağ ve depolama birimleri paylaşır. Kavramsal için benzer bir *pod* içinde [Kubernetes] [ kubernetes-pod] ve [DC/OS][dcos-pod].

Aşağıdaki diyagramda, birden çok kapsayıcı içeren bir kapsayıcı grubu örneği gösterilmektedir:

![Kapsayıcı grupları diyagramı][container-groups-example]

Bu örnek kapsayıcı grubu:

* Tek ana makinede zamanlandı.
* Bir DNS ad etiketi atanır.
* Tek bir ortak IP adresi, bir kullanıma sunulan bağlantı noktası ile kullanıma sunar.
* İki kapsayıcıları için oluşur. Bağlantı noktası 80 sırasında diğer dinlediği bağlantı noktası 5000 bir kapsayıcı dinler.
* İki içeren Azure dosya paylaşımları birim başlatmalar ve her kapsayıcı paylaşımları yerel olarak birini bağlar.

> [!NOTE]
> Birden çok kapsayıcı grupları Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="deployment"></a>Dağıtım

Kapsayıcı *grupları* 1 vCPU ve 1 GB bellek en düşük kaynak ayırma vardır. Tek tek *kapsayıcıları* Grup değerinden 1 vCPU ve 1 GB belleği olan bir kapsayıcıda sağlanabilir. Bir kapsayıcı grubu içindeki kaynakların dağıtılması kapsayıcı grubu düzeyinde belirlenen sınırlar içinde birden çok kapsayıcı için özelleştirilebilir. Örneğin, iki kapsayıcı her 1 vCPU ayırdığı bir kapsayıcı grubunda bulunan 0,5 vCPU ile.

## <a name="networking"></a>Ağ

Kapsayıcı grupları, bir IP adresi ve bağlantı noktası ad alanı, IP adresi üzerinde paylaşır. Grup içindeki bir kapsayıcı erişmek dış istemcileri etkinleştirmek için bağlantı noktası üzerinde IP adresi ve kapsayıcısından kullanıma gerekir. Grup kapsayıcılara bir bağlantı noktası ad alanı paylaştığından, bağlantı noktası eşlemesi desteklenmiyor. Bu bağlantı noktalarını grubun IP adresinde dışarıdan gösterilmeyen olsa bile bir grup kapsayıcılara bunlar sunulan bağlantı noktalarında localhost aracılığıyla birbirine ulaşabilirsiniz.

## <a name="storage"></a>Depolama

İçindeki bir kapsayıcı grubun bağlamak için dış birimleri belirtebilirsiniz. Tek bir grup kapsayıcılarında içindeki belirli yollara içine bu birimlerin eşleyebilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Birden çok kapsayıcı grupları tek bir işlev görev küçük bir kapsayıcı görüntüleri numarada bölmek istediğiniz durumlarda kullanışlıdır. Bu görüntüler farklı ekip tarafından alınabilir ve ayrı kaynak gereksinimleri vardır.

Örnek Kullanım dahil olabilir:

* Bir uygulama kapsayıcısı ve günlüğe kaydetme kapsayıcı. Günlüğe kaydetme kapsayıcısı ana uygulama tarafından çıktısı günlükler ve ölçümleri toplar ve uzun vadeli depolama için yazar.
* Bir uygulama kapsayıcısı ve izleme kapsayıcı. İzleme kapsayıcı düzenli aralıklarla bu çalıştığını ve düzgün yanıt ve değilse bir uyarı başlatır emin olmak için uygulamaya istekte bulunur.
* Bir web uygulaması hizmet veren bir kapsayıcı ve en yeni içerik kaynak denetiminden çekme kapsayıcı.

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure Resource Manager şablonu ile birden çok kapsayıcı kapsayıcı grubu dağıtmayı öğrenin:

> [!div class="nextstepaction"]
> [Kapsayıcı grubu dağıtma](container-instances-multi-container-group.md)

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
