---
title: Azure HDInsight için - güvenlik ve iyi DevOps uygulamalarından şirket içi Apache Hadoop kümelerini geçirme
description: Güvenlik ve Azure HDInsight için geçirme şirket içi Hadoop kümeleri için en iyi DevOps öğrenin.
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 1d13b52d253562a24946e6df2fc069f41b485fef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64707899"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>Azure HDInsight için - güvenlik ve iyi DevOps uygulamalarından şirket içi Apache Hadoop kümelerini geçirme

Bu makalede, Azure HDInsight sistemlerinde güvenlik ve DevOps için öneriler sunar. Geçirme şirket içi Apache Hadoop sistemler ile Azure HDInsight için yardımcı olması için en iyi yöntemler sağlayan bir dizi gereksinimlerimizim bir parçasıdır.

## <a name="secure-and-govern-cluster-with-enterprise-security-package"></a>Güvenli ve kurumsal güvenlik paketi ile küme yönetme

Kurumsal güvenlik paketi (ESP), Active Directory tabanlı kimlik doğrulaması, çoklu kullanıcı desteği ve rol tabanlı erişim denetimini destekler. Seçilen ESP seçeneği ile HDInsight kümesi için Active Directory etki alanına katılmış ve kuruluş yöneticisi Apache Ranger'ı kullanarak Apache Hive güvenliği için rol tabanlı erişim denetimi (RBAC) yapılandırabilirsiniz. Yönetici ayrıca çalışanlar ve erişim denetim ilkelerinde yapılan değişiklikler göre veri erişimi denetleyebilirsiniz.

ESP aşağıdaki küme türleri üzerinde kullanılabilir: Apache Hadoop, Apache Spark, Apache HBase, Apache Kafka ve etkileşimli sorgu (LLAP Hive). 

Etki alanına katılmış HDInsight kümesini dağıtmak için aşağıdaki adımları kullanın:

- Azure Active Directory (AAD), etki alanı adını geçirerek dağıtın.
- Azure Active Directory etki alanı Hizmetleri (AAD DS) dağıtın.
- Gerekli sanal ağ ve alt ağ oluşturun.
- AAD DS yönetmek için sanal ağda VM dağıtın.
- VM'yi etki alanına ekleyin.
- Yükleme AD ve DNS araçları.
- AAD DS yöneticisinin bir kuruluş birimi (OU) oluşturun sağlayın.
- LDAPS AAD DS için etkinleştirin.
- Alabilir ve böylece bir hizmet hesabı Azure Active Directory'de yetkilendirilmiş okuma ve yazma yönetici izni OU ile oluşturun. Bu hizmet hesabı daha sonra makinelerin etki alanına ve makine sorumluları OU içinde yerleştirin. Ayrıca, küme oluşturma sırasında belirttiğiniz OU içinde hizmet sorumluları de oluşturabilirsiniz.


    > [!Note]
    > Hizmet hesabı, AD etki alanı yöneticisi hesabı olması gerekmez.


- Aşağıdaki parametreleri ayarlayarak ESP HDInsight kümesi dağıtma:
    - **Etki alanı adı**: Azure AD DS ile ilişkili etki alanı adı.
    - **Etki alanı kullanıcı adı**: Hizmet hesabı, örneğin önceki bölümde oluşturduğunuz Azure AD DS DC tarafından yönetilen etki alanında: `hdiadmin@contoso.onmicrosoft.com`. Bu etki alanı kullanıcısı bu HDInsight kümesinin yönetici olacaktır.
    - **Etki alanı parolası**: Hizmet hesabı parolası.
    - **Kuruluş birimi**: Örneğin, HDInsight kümesi ile kullanmak istediğiniz kuruluş biriminin ayırt edici ad: `OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com`. Bu OU'ya mevcut değilse, HDInsight Küme hizmeti hesabının ayrıcalıklarını kullanarak OU oluşturmaya çalışır.
    - **LDAPS URL'si**: Örneğin, `ldaps://contoso.onmicrosoft.com:636`.
    - **Erişim kullanıcı grubu**: Küme, örneğin eşitlemek istediğiniz kullanıcıları için güvenlik gruplarını: `HiveUsers`. Birden çok kullanıcı gruplarını belirtmek istiyorsanız, bunları noktalı virgülle ayırın. ';'. Gruplara ESP küme oluşturmadan önce dizinin mevcut olması gerekir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Bir etki alanına katılmış HDInsight kümeleriyle Apache Hadoop güvenliğine giriş](../domain-joined/apache-domain-joined-introduction.md)

- [HDInsight, Azure etki alanına katılmış Apache Hadoop kümeleri planlama](../domain-joined/apache-domain-joined-architecture.md)
- [Azure Active Directory Domain Services'ı kullanarak bir etki alanına katılmış HDInsight kümesi yapılandırma](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)
- [Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)
- [Etki alanına katılmış HDInsight Apache Hive ilkelerini yapılandırma](../domain-joined/apache-domain-joined-run-hive.md)
- [Apache Oozie etki alanına katılmış HDInsight Hadoop kümeleri çalıştırın.](../domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)

## <a name="implement-end-to-end-enterprise-security"></a>Uçtan uca Kurumsal güvenlik uygulamaları

Son Kurumsal güvenlik sonuna aşağıdaki denetimleri kullanarak gerçekleştirilebilir:

- **Özel ve korumalı veri işlem hattı (çevre düzeyi güvenlik)**
    - Çevre düzeyi güvenlik, Azure sanal ağları, ağ güvenlik grupları ve ağ geçidi hizmeti gerçekleştirilebilir.

- **Kimlik doğrulaması ve veri erişimi için yetkilendirme**
    - Azure Active Directory Domain Services'ı kullanarak etki alanına katılmış HDInsight kümesi oluşturma. (Kurumsal güvenlik paketi).
    - Ambari, AD kullanıcıları için küme kaynakları için rol tabanlı erişim sağlamak için kullanın.
    - Erişim denetimi ilkeleri için Hive tablosunda ayarlamak için Apache Ranger'ı kullanma / sütun / satır düzeyi.
    - Kümeye SSH erişimi yöneticinin yalnızca sınırlı olabilir.

- **Denetim**
    - Tüm veri ve HDInsight kümesi kaynakları için erişim görünümünü ve raporunu.
    - Görüntüleme ve tüm değişiklikler için erişim denetimi ilkeleri bildirin.

- **Şifreleme**
    - Microsoft tarafından yönetilen anahtarlar veya müşteri tarafından yönetilen anahtarlar kullanılarak saydam sunucu tarafı şifrelemesi.
    - İstemci tarafı şifreleme, https ve TLS aktarım sırasında şifreleme kullanarak.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure sanal ağlarına genel bakış](../../virtual-network/virtual-networks-overview.md)
- [Azure ağ güvenlik gruplarına genel bakış](../../virtual-network/security-overview.md)
- [Azure sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md)
- [Azure depolama Güvenlik Kılavuzu](../../storage/common/storage-security-guide.md)
- [Bekleme sırasında Azure depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)

## <a name="use-monitoring--alerting"></a>Kullanımı izleme ve uyarı

Daha fazla bilgi için bkz:

[Azure İzleyiciye Genel Bakış](../../azure-monitor/overview.md)

## <a name="upgrade-clusters"></a>Küme yükseltme

Düzenli olarak en son özelliklerden yararlanmak için en son HDInsight sürümüne yükseltin. Aşağıdaki adımlar, kümenin en son sürüme yükseltmek için kullanılabilir:

1. En son kullanılabilir HDInsight sürümünü kullanan yeni bir TEST HDInsight kümesi oluşturun.
1. İşler ve iş yüklerini beklendiği gibi çalıştığından emin olmak için yeni kümede test edin.
1. İşleri veya uygulamalarınız veya iş yükleriniz gerektiği gibi değiştirin.
1. Küme düğümleri üzerinde yerel olarak depolanan tüm geçici verileri yedekleyin.
1. Var olan kümeyi silin.
1. HDInsight en son sürümünün bir küme aynı varsayılan veri ve meta depo önceki küme kullanarak aynı sanal ağ alt ağı oluşturun.
1. Yedeklenen herhangi bir geçici veri içeri aktarın.
1. Başlangıç işleri/yeni küme kullanarak işleme devam edin.

Daha fazla bilgi için bkz: [Yeni bir sürüme yükseltme HDInsight küme](../hdinsight-upgrade-cluster.md).

## <a name="patch-cluster-operating-systems"></a>Küme işletim sistemi düzeltme eki

Yönetilen bir Hadoop hizmeti olan HDInsight HDInsight kümeleri tarafından kullanılan sanal makinelerin işletim sistemi düzeltme eki uygulama üstlenir.

Daha fazla bilgi için bkz: [HDInsight için işletim sistemi düzeltme eki uygulama](../hdinsight-os-patching.md).

## <a name="post-migration"></a>Geçiş sonrası

1. **Uygulamaları düzeltme** - yinelemeli olarak işler, işlemler ve betikler gerekli değişiklikleri yapın.
2. **Testleri gerçekleştirmek** - çalıştırmalarınızı işlevsel çalışma ve performans testleri.
3. **En iyi duruma getirme** - yukarıdaki test sonuçlarına göre performans sorunları giderin ve performans iyileştirmeleri onaylamak için yeniden sınayın.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [HDInsight 4.0](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-introduction).
