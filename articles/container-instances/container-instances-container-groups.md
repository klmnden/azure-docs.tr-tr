---
title: Azure Container Instances'a kapsayıcı grupları
description: Nasıl birden çok kapsayıcı grubunun anlamak Azure Container ınstances'da çalışma
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/20/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: f4bbea8acd447a731cf5c56f9876baf9183735ea
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59785002"
---
# <a name="container-groups-in-azure-container-instances"></a>Azure Container ınstances'da kapsayıcı grupları

Azure Container ınstances'da en üst düzey kaynak *kapsayıcı grubu*. Bu makalede, kapsayıcı grupları nedir ve tanırlar senaryo türlerini açıklar.

## <a name="how-a-container-group-works"></a>Kapsayıcı grubu nasıl çalışır?

Kapsayıcı grubu, aynı konak makinesinde zamanlanmış kapsayıcıları koleksiyonudur. Bir kapsayıcı grubundaki kapsayıcı, bir yaşam döngüsü, kaynak, yerel ağ ve depolama birimleri paylaşın. Kavramsal için benzer bir *pod* içinde [Kubernetes][kubernetes-pod].

Aşağıdaki diyagramda, birden çok kapsayıcı içeren bir kapsayıcı grubu örneği gösterilmektedir:

![Kapsayıcı grupları diyagramı][container-groups-example]

Bu örnek kapsayıcı grubu:

* Tek bir konakta zamanlanır.
* Bir DNS ad etiketi atanır.
* Tek bir genel IP adresi, bir kullanıma sunulan bağlantı noktası ile kullanıma sunar.
* İki kapsayıcı oluşur. Bir kapsayıcı 80 numaralı bağlantı noktasında, diğer dinlediği sırasında 5000 numaralı bağlantı noktasında dinler.
* İki Azure dosya paylaşımları olarak birim takar ve her kapsayıcı yerel olarak paylaşımlarından birini bağlar.

> [!NOTE]
> Çok kapsayıcılı grupları şu anda yalnızca Linux kapsayıcıları destekler. Windows kapsayıcıları için Azure Container Instances, yalnızca tek bir örnek dağıtılmasını destekler. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışıyoruz, ancak geçerli platform farklılıklarını hizmetinde bulabilirsiniz [genel bakış](container-instances-overview.md#linux-and-windows-containers).

## <a name="deployment"></a>Dağıtım

Çok kapsayıcılı bir grup dağıtmak için iki ortak yollar şunlardır: kullanan bir [Resource Manager şablonu] [ resource-manager template] veya [YAML dosyası][yaml-file]. Ek Azure hizmet kaynakları dağıtmak için ihtiyacınız olduğunda Resource Manager şablonu önerilir (örneğin, bir [Azure dosyaları paylaşım][azure-files]) kapsayıcı örnekleri dağıttığınızda. Container Instances dağıtımınızda olduğunda YAML formatın daha kısa niteliği nedeniyle bir YAML dosyası önerilir.

Bir kapsayıcı grubun yapılandırmayı korumak için yapılandırmayı bir YAML dosyası için Azure CLI komutunu kullanarak dışa aktarabilirsiniz [az container dışarı aktarma][az-container-export]. Dışarı aktarma "yapılandırma" kod olarak sürüm denetiminde kapsayıcı grubu yapılandırmalarınızı depolamanıza olanak tanır Veya yeni bir YAML yapılandırmasında geliştirirken dışarı aktarılan dosya başlangıç noktası olarak kullanın.

## <a name="resource-allocation"></a>Kaynak ayırma

Azure Container Instances, CPU, bellek gibi kaynakları ayırır ve isteğe bağlı olarak [Gpu'lar] [ gpus] (Önizleme) bir kapsayıcı grubuna ekleyerek [kaynak isteklerini] [ resource-requests] grubunda örnekleri. İki örnek, istekte bulunan her 1 ile bir kapsayıcı grubu oluşturursanız, örnek olarak, CPU kaynaklarını alma CPU ve kapsayıcı grubunun 2 CPU ayrılır.

Kapsayıcı grubu için mevcut olan en fazla kaynaklara bağımlı [Azure bölgesi] [ region-availability] dağıtımı için kullanılır.

### <a name="container-resource-requests-and-limits"></a>Kapsayıcı kaynak isteklerini ve sınırlar

* Varsayılan olarak, bir çalışma grubundaki kapsayıcı örnekleri, istenen kaynak grubunun paylaşın. Bir gruptaki iki isteyen her 1 örnekler CPU, bütün olarak grubun, 2 CPU erişimi olduğundan. Her örneği için 2 CPU kullanabilir ve çalıştıkları sırada örnekler için CPU kaynağı rekabet.

* Bir gruptaki bir örnek tarafından kaynak kullanımını sınırlamak için isteğe bağlı olarak ayarlanmış bir [kaynak sınırı] [ resource-limits] örneği. İki örnekli bir gruptaki 1 isteyen CPU kapsayıcılarınızı biri diğerinden çalıştırmak daha fazla CPU gerekebilir.

  Bu senaryoda, 0,5 kaynak sınırı ayarlayabilirsiniz CPU bir örneği ve ikinci 2 CPU sınırı. Bu yapılandırma 0,5 ilk kapsayıcınızın kaynak kullanımını sınırlar CPU, ikinci kapsayıcıyı varsa tam 2 CPU'ları kullanmasını sağlar.

Daha fazla bilgi için [ResourceRequirements] [ resource-requirements] kapsayıcı özelliğinde REST API gruplandırır.

### <a name="minimum-and-maximum-allocation"></a>En düşük ve en fazla ayırma

* Ayırma bir **minimum** 1 CPU ve bellek bir kapsayıcı grubu için 1 GB. Bir grup içindeki tek bir container Instances ile 1'den küçük sağlanabilir CPU ve bellek 1 GB. 

* İçin **maksimum** kapsayıcı grubundaki kaynaklara bakın [kaynak kullanılabilirliğini] [aci-bölge-kullanılabilirliği] dağıtım bölgesinde Azure Container Instances için.

## <a name="networking"></a>Ağ

Kapsayıcı grupları, bir IP adresi ve IP adresi üzerinde bağlantı noktası ad alanı paylaşın. Bir kapsayıcı grubu içinde erişmek dış istemcilere etkinleştirmek için IP adresini ve kapsayıcı bağlantı noktasını açığa çıkarmalıdır. Kapsayıcı grubu içinde bir bağlantı noktası ad alanını paylaştığından, bağlantı noktası eşlemesi desteklenmiyor. Bile bu bağlantı noktalarını dışarıdan grubun IP adresinde kullanıma sunulmaz bir grup içindeki kapsayıcılar sunduğunuz, bağlantı noktalarında localhost aracılığıyla birbirine ulaşabilirsiniz.

İsteğe bağlı olarak gruplar halinde kapsayıcı dağıtma bir [Azure sanal ağı] [ virtual-network] güvenli sanal ağdaki diğer kaynaklarla iletişim kurabilmesi kapsayıcılar izin vermek için (Önizleme).

## <a name="storage"></a>Depolama

Kapsayıcı grubu içinde bağlamak için dış birimleri belirtebilirsiniz. Bu birimleri, bir grup içindeki ayrı kapsayıcıları içinde belirli yollar içine eşleyebilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Birden çok kapsayıcı grubunun tek bir işlev görev az sayıda kapsayıcı görüntülerini bölmek istediğiniz durumlarda kullanışlıdır. Bu görüntüler, ardından farklı ekipler tarafından sunulan ve ayrı kaynak gereksinimlerine sahip.

Örnek Kullanım içerebilir:

* Bir web uygulaması hizmet veren bir kapsayıcı ve kaynak denetiminden en son içeriği çekme kapsayıcı.
* Bir uygulama kapsayıcısı ve günlüğe kaydetme kapsayıcı. Günlük kapsayıcı tarafından ana uygulama günlükleri ve ölçümleri çıkış toplar ve uzun vadeli depolamaya yazar.
* Bir uygulama kapsayıcısı ve izleme kapsayıcı. İzleme kapsayıcı, düzenli aralıklarla bu çalıştığını ve düzgün yanıt ve değilse bir uyarı başlatır emin olmak için uygulamaya bir istek gönderir.
* Bir ön uç kapsayıcısı ve bir arka uç kapsayıcı. Ön uç, verileri almak için bir hizmeti çalıştıran arka ucu ile bir web uygulaması hizmet edebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Resource Manager şablonu ile bir çok kapsayıcılı kapsayıcı grubu dağıtmayı öğrenin:

> [!div class="nextstepaction"]
> [Kapsayıcı grubu dağıtma][resource-manager template]

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/

<!-- LINKS - Internal -->
[resource-manager template]: container-instances-multi-container-group.md
[yaml-file]: container-instances-multi-container-yaml.md
[region-availability]: container-instances-region-availability.md
[resource-requests]: /rest/api/container-instances/containergroups/createorupdate#resourcerequests
[resource-limits]: /rest/api/container-instances/containergroups/createorupdate#resourcelimits
[resource-requirements]: /rest/api/container-instances/containergroups/createorupdate#resourcerequirements
[azure-files]: container-instances-volume-azure-files.md
[virtual-network]: container-instances-vnet.md
[gpus]: container-instances-gpu.md
[az-container-export]: /cli/azure/container#az-container-export
