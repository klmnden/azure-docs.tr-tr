---
title: Azure HDInsight sanal ağ mimarisi
description: Azure sanal ağ'daki bir HDInsight kümesi oluşturduğunuzda kullanılabilir kaynakları öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
origin.date: 03/26/2019
ms.date: 04/29/2019
ms.author: v-yiso
ms.openlocfilehash: 6d92273298c0448d7377acab6f3b8ea1cc1ed908
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60484888"
---
# <a name="azure-hdinsight-virtual-network-architecture"></a>Azure HDInsight sanal ağ mimarisi

Bu makalede, özel bir Azure sanal ağına bir HDInsight kümesi dağıtırken var olan kaynakları açıklanmaktadır. Bu bilgiler, Azure HDInsight kümenizdeki şirket içi kaynaklara bağlanmak için yardımcı olur. Azure sanal ağları hakkında daha fazla bilgi için bkz. [Azure sanal ağ nedir?](../virtual-network/virtual-networks-overview.md).

## <a name="resource-types-in-azure-hdinsight-clusters"></a>Azure HDInsight kümelerinde kaynak türleri

Azure HDInsight kümeleri farklı türlerde sanal makineler veya düğümler var. Her düğüm türü, sistem işleminde bir rol oynar. Bu düğüm türleri ve kümedeki kendi rolleri aşağıdaki tabloda özetlenmiştir.

| Tür | Açıklama |
| --- | --- |
| Baş düğüm |  Apache Storm dışındaki tüm küme türleri için baş düğümler dağıtılmış uygulamanın yürütülmesini yönetme işlemleri barındırır. Baş düğüm, ayrıca içine SSH yapabileceğiniz düğümüdür ve küme kaynakları üzerinde çalıştırılacak düzenlenir uygulamalar çalıştırın. Baş düğüm sayısını iki tüm küme türleri sabitlenmiştir. |
| ZooKeeper düğümü | Zookeeper veri işleme yapıyor düğümler arasında görevleri koordine eder. Ayrıca baş düğümün öncü seçimi yapar ve hangi baş düğüm, belirli bir hizmet ana çalışan izler. ZooKeeper düğümleri sayısı iki sabit. |
| Çalışan düğümü | Veri işleme işlevleri destekleyen düğümleri temsil eder. Çalışan düğümlerinin eklenip hesaplama yeteneği ölçeklendirin ve maliyetleri yönetmek için kümesinden kaldırılabilir. |
| R Server kenar düğümüne | R Server kenar düğümüne SSH uygulayın yapabilecekleriniz düğümünü temsil eder ve ardından küme kaynakları üzerinde çalıştırılacak düzenlenir uygulamalarını yürütmek. Bir kenar düğümü küme içindeki veri analizi katılmak değil. Bu düğüm, bir tarayıcı kullanarak R uygulamayı çalıştırmayı sağlayan R Studio Server da barındırır. |
| Bölge düğümü | HBase kümesi türü için bölge sunucusu (veri düğümü olarak da bilinir) bölge düğüm çalıştırır. Bölge sunucuları, hizmet ve HBase tarafından yönetilen veri bölümünü yönetin. Bölge düğümler eklenebilir veya hesaplama yeteneği ölçeklendirin ve maliyetleri yönetmek için kümeden kaldırılmış.|
| Nimbus düğümü | Storm küme türü için Nimbus düğümü, baş düğüm için benzer işlevsellik sağlar. Nimbus düğümü, çalışan Storm topolojilerini koordinatları Zookeeper aracılığıyla kümedeki diğer düğümlere görevler atar. |
| Gözetmen düğümü | Storm küme türü için istenen işlemi gerçekleştirmek için Nimbus düğümü tarafından sağlanan yönergeleri gözetmen düğüm yürütür. |

## <a name="basic-virtual-network-resources"></a>Temel sanal ağ kaynakları

Aşağıdaki diyagramda HDInsight düğümleri ile ağ kaynaklarını yerleşimini Azure'da gösterilmektedir.

![Oluşturulan özel Azure VNET'te HDInsight varlıklar diyagramı](./media/hdinsight-virtual-network-architecture/vnet-diagram.png)

Varsayılan kaynakları HDInsight bir Azure sanal ağına dağıtıldığında mevcut dışında ağlar ve sanal ağ arasında iletişimi destekleyen ağ aygıtlarının yanı sıra önceki tabloda, belirtilen küme düğümü türlerini içerir.

HDInsight, özel bir Azure sanal ağına bağlanmasıdır dağıtıldığında, oluşturulan dokuz küme düğümleri aşağıdaki tabloda özetlenmiştir.

| Kaynak türü | Mevcut bir sayı | Ayrıntılar |
| --- | --- | --- |
|Baş düğüm | iki |    |
|Zookeeper düğümü | üç | |
|Çalışan düğümü | iki | Bu sayı, küme yapılandırması ve ölçeklendirme göre değişebilir. En az üç çalışan düğümü için Apache Kafka gereklidir.  |
|Ağ geçidi düğümü | iki | Ağ geçidi düğümleri, Azure sanal makineler Azure'da oluşturulur, ancak aboneliğinizde görünmez olur. Bu düğümlerini yeniden başlatmanız gerekiyorsa desteğe başvurun. |

Aşağıdaki ağ kaynakları mevcut HDInsight ile kullanılan sanal ağ içinde otomatik olarak oluşturulur:

| Ağ kaynak | Mevcut bir sayı | Ayrıntılar |
| --- | --- | --- |
|Yük dengeleyici | üç | |
|Ağ Arabirimleri | dokuz | Bu değer, her düğüm, kendi ağ arabirimine sahip olduğu normal bir kümede temel alır. Dokuz için iki baş düğüm, üç zookeeper düğümleri, iki çalışan düğümü ve iki ağ geçidi düğümleri önceki tabloda sözü edilen arabirimdir. |
|Genel IP Adresleri | iki |    |

## <a name="endpoints-for-connecting-to-hdinsight"></a>HDInsight için bağlanmak için uç noktaları

HDInsight kümenizi üç yolla erişebilir:

- Sanal ağ dışındaki bir HTTPS uç noktası `CLUSTERNAME.azurehdinsight.net`.
- Doğrudan baş düğümüne bağlanmak için bir SSH uç noktası `CLUSTERNAME-ssh.azurehdinsight.net`.
- Bir HTTPS uç noktası sanal ağ içindeki `CLUSTERNAME-int.azurehdinsight.net`. Bildirim "-int" Bu URL. Bu uç nokta, bu sanal ağ özel bir IP çözülür ve genel internet'ten erişilebilir durumda değil.

Bu üç uç noktalar her bir yük dengeleyiciye atanır.

Genel IP adresleri sanal ağın dışında bağlantı sağlayan iki uç nokta için de sağlanır.

1. Bir genel IP atanır yük dengeleyici tam etki alanı adı (FQDN) için internet'ten kümeye bağlanırken kullanılacak `CLUSTERNAME.azurehdinsight.net`.
1. İkinci genel IP adresi için SSH yalnızca etki alanı adı kullanılır `CLUSTERNAME-ssh.azurehdinsight.net`.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel uç nokta ile bir sanal ağdaki HDInsight kümelerine gelen trafiğin güvenliğini sağlama](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)
