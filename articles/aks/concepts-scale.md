---
title: Kavramlar - Azure Kubernetes Hizmetleri (AKS) uygulamaları ölçeklendirme
description: Azure Kubernetes hizmeti (yatay pod otomatik ölçeklendiricinin, küme ölçeklendiriciyi ve Azure Container Instances Bağlayıcısı gibi AKS), ölçeklendirme hakkında bilgi edinin.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: zarhoads
ms.openlocfilehash: 2070c79a6ce0627280b1793e412002783f385cc0
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074032"
---
# <a name="scaling-options-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) uygulamaları için ölçeklendirme seçenekleri

Azure Kubernetes Service (AKS) uygulamaları çalıştırma gibi artırın veya azaltın, bilgi işlem kaynaklarının miktarını gerekebilir. Uygulama örnekleri sayısı arttıkça, temel alınan Kubernetes düğümleri de değiştirmeniz gerekebilir sayısını değiştirme gerekir. Hızlı bir şekilde çok sayıda ek uygulama örnekleri sağlamak gerekebilir.

Bu makalede çekirdek tanıtılır yardımcı kavramları, AKS içinde uygulamaları ölçeklendirme:

- [El ile ölçeklendirme](#manually-scale-pods-or-nodes)
- [Yatay pod otomatik ölçeklendiricinin (HPA)](#horizontal-pod-autoscaler)
- [Otomatik ölçeklendiricinin küme](#cluster-autoscaler)
- [AKS ile Azure Container örneği (ACI) Tümleştirmesi](#burst-to-azure-container-instances)

## <a name="manually-scale-pods-or-nodes"></a>Pod'ların veya düğümleri el ile ölçeklendirme

Çoğaltmalar (pod) ve kullanılabilir kaynaklar ve durum değişikliği için uygulamanızın nasıl yanıt vereceğini test etmek için düğümleri elle ölçeklendirebilirsiniz. Kaynakları el ile ölçeklendirme, belirli bir düğüm sayısı gibi sabit bir maliyete korumak için kullanılacak kaynakları tanımlamak da sağlar. El ile ölçeklendirmek için çoğaltma veya düğüm sayısı ve ek pod'lar oluşturmak veya düğüm boşaltma Kubernetes API zamanlama tanımlayın.

Pod'ların ve düğümleri görür el ile ölçeklendirme ile kullanmaya başlamak için [ölçeklendirme uygulamaları AKS][aks-scale].

## <a name="horizontal-pod-autoscaler"></a>Yatay pod otomatik ölçeklendiricinin

Kubernetes kaynak talebi izlemek ve çoğaltmalarının sayısı otomatik olarak ölçeklendirmek için yatay pod otomatik ölçeklendiricinin (HPA) kullanır. Varsayılan olarak, yatay pod otomatik ölçeklendiricinin gerekli değişiklikler yineleme sayısı her 30 saniyede ölçümler API denetler. Değişiklikleri gerekli olduğunda, yineleme sayısını artırabilir veya buna göre azalan. Kubernetes için 1.8 + ölçümleri sunucu dağıtmış olduğunuz AKS kümeleriyle yatay pod otomatik ölçeklendiricinin çalışır.

![Kubernetes yatay pod otomatik ölçeklendirmeyi](media/concepts-scale/horizontal-pod-autoscaling.png)

Belirli bir dağıtım için yatay pod otomatik ölçeklendiricinin yapılandırdığınızda çalıştırabilirsiniz çoğaltmaları minimum ve maksimum sayısını tanımlayın. Ölçüm, izleme ve ölçeklendirme herhangi bir karar, CPU kullanımı gibi temel de tanımlarsınız.

Yatay pod otomatik ölçeklendiricinin aks'deki kullanmaya başlamak için bkz. [otomatik pod ölçeklendirmeyi AKS][aks-hpa].

### <a name="cooldown-of-scaling-events"></a>Olayları ölçeklendirmenin dakikaysa

Yatay pod otomatik ölçeklendiricinin ölçümleri API'si her 30 saniyede denetler gibi başka bir onay yapılır önce önceki ölçek olayları başarıyla tamamlanmamış olabilir. Bu davranış önceki ölçek olayı uygulama iş yükünü ve buna uygun olarak ayarlamak için kaynak taleplerini alma oluşturabildi önce yineleme sayısını değiştirmek yatay pod otomatik ölçeklendiricinin neden olabilir.

Bu yarış olayları en aza indirmek için dakikaysa veya gecikme değerler olarak ayarlanabilir. Yatay pod otomatik ölçeklendiricinin başka bir ölçek olay tetiklenebilir önce bir ölçek olayından sonra ne kadar beklemesi gerekir, bu değerleri tanımlayın. Bu davranış, yeni çoğaltma sayısı etkili olması için ve ölçümler API'si yansıtılmıştır dağıtılmış iş yükündeki sağlar. Varsayılan olarak, Ölçek olayları gecikmesi 3 dakikadır ve olayları aşağı ölçeğinde gecikme 5 dakikadır

Bu dakikaysa değerleri ayarlamak gerekebilir. Varsayılan dakikaysa değerleri yatay pod otomatik ölçeklendiricinin çoğaltma sayısını yeterince hızlı ölçeklendirme değil izlenim verebilir. Daha hızlı bir şekilde kullanımda çoğaltmaların sayısı artırmak için örneğin, azaltmak `--horizontal-pod-autoscaler-upscale-delay` yatay pod otomatik ölçeklendiricinin tanımlarını kullanarak oluştururken `kubectl`.

## <a name="cluster-autoscaler"></a>Otomatik ölçeklendiricinin küme

Değişen taleplere pod düğüm havuzundaki istenen işlem kaynaklarını göre düğüm sayısını ayarlayan bir küme ölçeklendiriciyi (şu anda önizlemede aks'deki) Kubernetes gerekir. Varsayılan olarak, küme ölçeklendiriciyi API sunucusu için gerekli değişiklikleri düğüm sayısı 10 saniyede denetler. Küme otomatik ölçeklendirme bir değişiklik gerekli olduğunu belirlerse, AKS kümenizdeki düğüm sayısını artırabilir veya uygun şekilde azalır. Küme otomatik ölçeklendiricinin çalışan Kubernetes AKS RBAC özellikli kümeleriyle çalışır 1.10.x veya üzeri.

![Kubernetes küme ölçeklendiriciyi](media/concepts-scale/cluster-autoscaler.png)

Küme ölçeklendirici, genellikle yatay pod otomatik ölçeklendiricinin kullanılır. Yatay pod otomatik ölçeklendiricinin birleştirildiğinde artırır veya uygulamanın talebine bağlı pod'ların sayısını azaltır ve bu ek pod'lar uygun şekilde çalışması için gerekli olarak küme ölçeklendiriciyi düğüm sayısını ayarlar.

Küme ölçeklendiriciyi tek düğüm havuzu AKS kümeleriyle'te önizlemesi yalnızca test edilmelidir.

Küme otomatik ölçeklendiricinin aks'deki kullanmaya başlamak için bkz. [AKS üzerinde Küme Ölçeklendiriciyi][aks-cluster-autoscaler].

### <a name="scale-up-events"></a>Olayları ölçeklendirin

İstenen bir pod çalıştırmak için yeterli işlem kaynakları bir düğüm yoksa, bu pod planlama sürecinde ilerleme olamaz. İçinde düğüm havuzu ek işlem kaynakları kullanılabilir sürece pod başlatılamıyor.

Küme otomatik ölçeklendiricinin düğüm havuzu kaynak kısıtlamaları nedeniyle zamanlanamaz pod'ların istediğinde, ek işlem kaynakları sağlamak için düğüm havuzdaki düğüm sayısını artar. Bu ek düğümler, başarıyla dağıtılan ve düğüm havuzu içinde kullanmak için kullanılabilir olduğunda, pod'ların ardından bunlar üzerinde çalışmak üzere zamanlanır.

Hızlı bir şekilde ölçeklendirme uygulamanız gerekiyorsa, bazı pod'ları kümesi ölçeklendiriciyi tarafından dağıtılan ek düğümler zamanlanmış pod'ların kabul edebilir kadar zamanlanması için bekleyen bir durumda kalabilir. Yüksek patlama taleplerine sahip uygulamalar için sanal düğümü ve Azure Container Instances ile ölçeklendirebilirsiniz.

### <a name="scale-down-events"></a>Olayları ölçeklendirin

Küme otomatik ölçeklendiricinin durumunu kısa bir süre önce yeni bir zamanlama istekleri etmemiş olan düğümler için zamanlama pod de izler. Bu senaryo, düğüm havuza gerekli olandan daha fazla işlem kaynaklara sahip olduğunu ve düğüm sayısını azaltılabilir gösterir.

Varsayılan olarak 10 dakika için artık gerekli bir eşiğini geçen düğüm silinmek üzere zamanlandı. Bu durumda, düğüm havuzdaki diğer düğümlerde çalıştırılacak pod'ların zamanlanır ve küme ölçeklendiriciyi düğüm sayısını azaltır.

Küme otomatik ölçeklendiricinin düğümlerinin sayısını azalttığında pod'ların farklı düğümlere zamanlandığı gibi bazı kesintisi uygulamalarınızı karşılaşabilirsiniz. Uğramasını azaltmak için tek pod örneği kullanan uygulamaları kaçının.

## <a name="burst-to-azure-container-instances"></a>Azure Container Instances'a veri bloğu

AKS kümenizi hızlı bir şekilde ölçeklendirmek için Azure Container Instances'a (ACI) ile tümleştirebilirsiniz. Kubernetes, çoğaltma ve düğüm sayısını ölçeklendirme için yerleşik bileşeni vardır. Ancak, hızlı bir şekilde ölçeklendirme uygulamanız gerekiyorsa, yatay pod otomatik ölçeklendiricinin mevcut düğüm havuzu bilgi işlem kaynaklarının sağladığı çok daha fazla pod zamanlayabilir. Yapılandırdıysanız, bu senaryo sonra düğüm havuzu içinde ek düğümler dağıtmak için küme ölçeklendiriciyi tetikleyecektir ancak başarıyla sağlama ve Kubernetes Zamanlayıcı üzerlerinde pod'ların çalışmasına izin vermek bu düğümleri birkaç dakika sürebilir.

![ACI'ya ölçeklendirme Kubernetes veri bloğu](media/concepts-scale/burst-scaling.png)

ACI ek altyapı işleriyle container Instances hızlı bir şekilde dağıtmanıza olanak tanır. AKS ile bağladığınızda, güvenli, mantıksal bir uzantı AKS kümenizin ACI olur. AKS kümenizde ACI sanal Kubernetes düğüm olarak gösterir Virtual Kubelet bileşeni yüklenir. Kubernetes pod'ların olarak değil sanal düğümleri aracılığıyla ACI örnekleri olarak VM düğümlerinin doğrudan AKS kümenizde çalışan pod'ların ardından zamanlayabilirsiniz. Sanal düğümü şu anda, AKS ön izleme aşamasındalar.

Uygulamanızı sanal düğümü kullanmak için hiçbir değişiklik gerektirir. ACI ve AKS dağıtımları ölçeklendirebilirsiniz ve küme olarak gecikme olmadan yeni düğümler AKS kümenizde otomatik ölçeklendiricinin dağıtır.

Sanal düğümler, aynı sanal ağda AKS kümenizin ek bir alt ağa dağıtılır. ACI ve AKS korunması arasındaki trafik bu sanal ağ yapılandırması sağlar. Bir AKS kümesi gibi ACI örneği, diğer kullanıcıların yalıtılmış bir güvenli, mantıksal işlem kaynağıdır.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamaları ölçeklendirme ile çalışmaya başlamak için öncelikle izleyin [Azure CLI ile bir AKS kümesi oluşturma Hızlı başlangıcı][aks-quickstart]. El ile veya otomatik olarak, AKS kümesinde uygulamaları ölçeklendirmek başlatabilirsiniz:

- El ile ölçeklendirme [pod'ların] [ aks-manually-scale-pods] veya [düğümleri][aks-manually-scale-nodes]
- Kullanım [yatay pod otomatik ölçeklendiricinin][aks-hpa]
- Kullanım [ölçeklendiriciyi küme][aks-cluster-autoscaler]

Çekirdek Kubernetes hakkında daha fazla bilgi ve AKS kavramlar için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümesi ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS erişim ve kimlik][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]

<!-- LINKS - external -->

<!-- LINKS - internal -->
[aks-quickstart]: kubernetes-walkthrough.md
[aks-hpa]: tutorial-kubernetes-scale.md#autoscale-pods
[aks-scale]: tutorial-kubernetes-scale.md
[aks-manually-scale-pods]: tutorial-kubernetes-scale.md#manually-scale-pods
[aks-manually-scale-nodes]: tutorial-kubernetes-scale.md#manually-scale-aks-nodes
[aks-cluster-autoscaler]: autoscaler.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-network]: concepts-network.md
