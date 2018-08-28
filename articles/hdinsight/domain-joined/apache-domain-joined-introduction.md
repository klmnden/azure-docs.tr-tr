---
title: Bir etki alanına katılmış Azure HDInsight kümeleriyle Hadoop güvenliğine giriş
description: Bilgi nasıl etki alanına katılmış Azure HDInsight kümeleri, Kuruluş güvenliği dört yapı taşları destekler.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.openlocfilehash: c13fd979562cc89831d031c5050fe9dbb184267b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041141"
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Bir etki alanına katılmış HDInsight kümeleriyle Hadoop güvenliğine giriş

Geçmişte, Azure HDInsight yalnızca tek bir kullanıcı desteklenen: yerel yönetici Bu durum küçük çaplı uygulama ekipleri veya departmanları için idealdi. Active Directory tabanlı kimlik doğrulaması gibi çok kullanıcılı Kurumsal düzeydeki özellikleri desteği ve rol tabanlı erişim denetimi gittikçe daha önemli hale geldi gereken Kurumsal sektörde daha popüler Hadoop tabanlı iş yüklerini kazanılan gibi. 

Bir Active Directory etki alanına katılmış bir HDInsight kümesi oluşturabilirsiniz. Ardından HDInsight kümesinde oturum açmak için Azure Active Directory aracılığıyla doğrulayabilir çalışanların kurumsal bir listesini yapılandırabilirsiniz. Kuruluş dışından kullanıcılar oturum açamıyor veya HDInsight küme erişim. 

Kuruluş yöneticisi rol tabanlı erişim denetimi (RBAC) Hive güvenliği kullanarak yapılandırabilirsiniz [Apache Ranger](http://hortonworks.com/apache/ranger/). RBAC yapılandırma, yalnızca gerekli olanla veri erişimi kısıtlar. Son olarak yönetici çalışanlara ve erişim denetim ilkelerinde yapılan değişiklikler göre veri erişimi denetleyebilirsiniz. Yönetici daha sonra bir yüksek düzeyde kurumsal kaynakların elde edebilirsiniz.

> [!NOTE]
> Bu makalede açıklanan yeni özellikler yalnızca şu küme türlerini üzerinde önizleme modunda kullanılabilir: Hadoop, Spark ve etkileşimli sorgu. Oozie etki alanına katılmış kümeleri şimdi etkinleştirildi. Oozie web kullanıcı Arabirimi erişmek için kullanıcıların etkinleştirmelisiniz [tünel](../hdinsight-linux-ambari-ssh-tunnel.md).

Kuruluş güvenliği dört temel yapı taşları içerir: çevre güvenliği, kimlik doğrulaması, yetkilendirme ve şifreleme.

![Etki alanına katılmış HDInsight kümeleri Kuruluş güvenliği dört yapı taşları, avantajları](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

## <a name="perimeter-security"></a>Çevre güvenliği
HDInsight çevre güvenliği sanal ağlar ve Azure VPN ağ geçidi hizmeti sağlanır. Kuruluş Yöneticileri, bir sanal ağ içindeki bir HDInsight kümesi oluşturma ve sanal ağa erişimi kısıtlamak için ağ güvenlik grupları (güvenlik duvarı kuralları) kullanın. Yalnızca gelen güvenlik duvarı kurallarında tanımlana IP adresleri HDInsight kümesiyle iletişim kurabilir. Bu yapılandırma, çevre güvenliği sağlar.

Çevre bir güvenlik katmanı VPN ağ geçidi hizmeti aracılığıyla sağlanır. Ağ geçidi, ilk savunma hattınızdır HDInsight kümesine gelen tüm istekler görür. İsteği kabul eder, doğrular ve yalnızca ardından isteğin kümedeki diğer düğümlere geçirilecek sağlar. Bu şekilde, ağ geçidi, kümedeki diğer ad ve veri düğümleri için çevre güvenliğini sağlar.

## <a name="authentication"></a>Kimlik Doğrulaması
Kuruluş Yöneticileri etki alanına katılmış bir HDInsight kümesi oluşturabilirsiniz. bir [sanal ağ](https://azure.microsoft.com/services/virtual-network/). HDInsight kümesinin tüm düğümlerine kuruluş tarafından yönetilen etki alanına katılır. Bu kullanımının sağlanır [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). 

Bu kurulum sayesinde kuruluş çalışanları küme düğümlerinde etki alanı kimlik bilgilerini kullanarak oturum açabilir. Bunlar ayrıca, etki alanı kimlik Ambari Views, ODBC, JDBC, PowerShell ve REST API'leri kümeyle etkileşimde gibi diğer onaylanmış uç noktaların kimlik doğrulaması yapmak için kullanabilirsiniz. Yönetici Bu uç noktalar üzerinden kümeyle etkileşime geçen kullanıcının sayısını sınırlama üzerinde tam denetime sahiptir.

## <a name="authorization"></a>Yetkilendirme
Çoğu kuruluş izleyen en iyi uygulama her çalışana kuruluş kaynakları erişimi olduğundan emin olmak. Benzer şekilde, yönetici küme kaynakları için rol tabanlı erişim denetimi ilkeleri tanımlayabilirsiniz. 

Örneğin yönetici [Apache Ranger](http://hortonworks.com/apache/ranger/)’ı Hive için erişim denetim ilkeleri belirleyecek şekilde yapılandırabilir. Bu işlev çalışanların yalnızca erişmesini sağlar. kendi işlerinde başarılı olmak için ihtiyaç duydukları kadar veri. Kümeye SSH erişimi de yalnızca yöneticiyle sınırlıdır.

## <a name="auditing"></a>Denetim
Tüm küme kaynaklarını ve veri erişim denetiminin, kaynaklara yetkisiz veya istenmeyen erişimi takip etmek gereklidir. HDInsight küme kaynaklarını yetkisiz kullanıcılardan korumanın ve verilerin güvenliğini sağlama, kadar önemlidir. 

Yönetici görüntüleyebilir ve tüm erişim veri ve HDInsight kümesi kaynakları için rapor. Yönetici de görüntüleyebilir ve tüm değişiklikleri Apache Ranger destekli uç noktalarda oluşturulan erişim denetimi ilkeleri rapor. 

Bir etki alanına katılmış HDInsight kümesi, Denetim günlüklerinde arama yapma için bilindik Apache Ranger arabirimini kullanır. Ranger arka uçta kullanan [Apache Solr](http://hortonworks.com/apache/solr/) depolamak ve günlükleri aranıyor.

## <a name="encryption"></a>Şifreleme
Veri koruma, toplantı Kurumsal güvenlik ve uyumluluk gereksinimlerini için önemlidir. Erişim için verilerin yetkisiz çalışanların erişiminden korunmasının yanı sıra, bu şifreleme. 

HDInsight kümeleri--Azure Blob Depolama ve Azure Data Lake depolama Gen1--destek saydam sunucu tarafı için her iki veri depoları [verilerin şifrelenmesi](../../storage/common/storage-service-encryption.md) bekleyen. Güvenli HDInsight kümeleri, bekleme sırasında veri sunucu tarafı şifreleme bu özellik ile sorunsuz bir şekilde çalışır.

## <a name="next-steps"></a>Sonraki adımlar
* [Etki alanına katılmış HDInsight kümeleri planlama](apache-domain-joined-architecture.md)
* [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md)
* [Etki alanına katılmış HDInsight kümelerini yönetme](apache-domain-joined-manage.md)
* [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md)
* [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)

