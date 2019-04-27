---
title: Kurumsal güvenlik paketi ile Apache Hadoop güvenliğine giriş
description: Kurumsal güvenlik paketi, Kuruluş güvenliği dört yapı taşları nasıl desteklediğini öğrenin.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 8a568302b2fd8ed0140c2ad084495d41cc09ed8f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60536866"
---
# <a name="an-introduction-to-apache-hadoop-security-with-enterprise-security-package"></a>Kurumsal güvenlik paketi ile Apache Hadoop güvenliğine giriş

Geçmişte, Azure HDInsight yalnızca tek bir kullanıcı desteklenen: yerel yönetici Bu durum küçük çaplı uygulama ekipleri veya departmanları için idealdi. Active Directory tabanlı kimlik doğrulaması, çok kullanıcılı gibi Kurumsal düzeydeki özellikleri desteği ve rol tabanlı erişim denetimi gittikçe daha önemli hale geldi gereken Kurumsal sektörde daha popüler Apache Hadoop tabanlı iş yüklerini kazanılan gibi. 

Kurumsal güvenlik paketi'ile (ESP), bir Active Directory etki alanına katılmış bir HDInsight kümesi oluşturabilirsiniz. Daha sonra oturum açmak için HDInsight kümesi için Azure Active Directory aracılığıyla doğrulayabilir çalışanların kurumsal bir listesini yapılandırabilirsiniz. Hiç bir kuruluş dışına oturum açın veya HDInsight küme erişim. 

Kuruluş yöneticisi rol tabanlı erişim denetimi (RBAC) için güvenlik Apache Hive kullanarak yapılandırabilirsiniz [Apache Ranger](https://hortonworks.com/apache/ranger/). RBAC yapılandırma, yalnızca gerekli olanla veri erişimi kısıtlar. Son olarak yönetici çalışanlara ve erişim denetim ilkelerinde yapılan değişiklikler göre veri erişimi denetleyebilirsiniz. Yönetici daha sonra bir yüksek düzeyde kurumsal kaynakların elde edebilirsiniz.

> [!NOTE]  
> Apache Oozie ESP kümelerinde şimdi etkinleştirildi. Oozie web kullanıcı Arabirimi erişmek için kullanıcıların etkinleştirmelisiniz [tünel](../hdinsight-linux-ambari-ssh-tunnel.md).

Kuruluş güvenliği dört temel yapı taşları içerir: çevre güvenliği, kimlik doğrulaması, yetkilendirme ve şifreleme.

![Kurumsal güvenlik paketi HDInsight kümeleri Kuruluş güvenliği dört yapı taşları avantajları](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

## <a name="perimeter-security"></a>Çevre güvenliği
HDInsight çevre güvenliği sanal ağlar ve Azure VPN ağ geçidi hizmeti sağlanır. Kuruluş Yöneticileri sanal ağ içindeki bir ESP kümesi oluşturabilir ve sanal ağa erişimi kısıtlamak için ağ güvenlik grupları (güvenlik duvarı kuralları) kullanın. Yalnızca gelen güvenlik duvarı kurallarında tanımlana IP adresleri HDInsight kümesiyle iletişim kurabilir. Bu yapılandırma, çevre güvenliği sağlar.

Çevre bir güvenlik katmanı VPN ağ geçidi hizmeti aracılığıyla sağlanır. Ağ geçidi, ilk savunma hattınızdır HDInsight kümesine gelen tüm istekler görür. İsteği kabul eder, doğrular ve yalnızca ardından isteğin kümedeki diğer düğümlere geçirilecek sağlar. Bu şekilde, ağ geçidi, kümedeki diğer ad ve veri düğümleri için çevre güvenliğini sağlar.

## <a name="authentication"></a>Kimlik Doğrulaması
Kuruluş Yöneticileri ESP içeren bir HDInsight kümesi oluşturma bir [sanal ağ](https://azure.microsoft.com/services/virtual-network/). HDInsight kümesinin tüm düğümlerine kuruluş tarafından yönetilen etki alanına katılır. Bu kullanımının sağlanır [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). 

Bu kurulum sayesinde kuruluş çalışanları küme düğümlerinde etki alanı kimlik bilgilerini kullanarak oturum açabilirsiniz. Bunlar ayrıca Apache Ambari Views, ODBC, JDBC, PowerShell ve REST API'leri kümeyle etkileşimde gibi diğer onaylanmış uç noktaların kimlik doğrulaması yapmak için etki alanı kimlik bilgilerini kullanabilirsiniz. Yönetici Bu uç noktalar üzerinden kümeyle etkileşime geçen kullanıcının sayısını sınırlama üzerinde tam denetime sahiptir.

## <a name="authorization"></a>Yetkilendirme
Çoğu kuruluş izleyen en iyi uygulama her çalışana kuruluş kaynakları erişimi olduğundan emin olmak. Benzer şekilde, yönetici küme kaynakları için rol tabanlı erişim denetimi ilkeleri tanımlayabilirsiniz. 

Örneğin yönetici [Apache Ranger](https://hortonworks.com/apache/ranger/)’ı Hive için erişim denetim ilkeleri belirleyecek şekilde yapılandırabilir. Bu işlev çalışanların yalnızca erişmesini sağlar. kendi işlerinde başarılı olmak için ihtiyaç duydukları kadar veri. Kümeye SSH erişimi de yalnızca yöneticiyle sınırlıdır.

## <a name="auditing"></a>Denetim
Tüm küme kaynaklarını ve veri erişim denetiminin, kaynaklara yetkisiz veya istenmeyen erişimi takip etmek gereklidir. HDInsight küme kaynaklarını yetkisiz kullanıcılardan korumanın ve verilerin güvenliğini sağlama, kadar önemlidir. 

Yönetici görüntüleyebilir ve tüm erişim veri ve HDInsight kümesi kaynakları için rapor. Yönetici de görüntüleyebilir ve tüm değişiklikleri Apache Ranger destekli uç noktalarda oluşturulan erişim denetimi ilkeleri rapor. 

Bir HDInsight kümesi ile ESP denetim günlüklerini aramak için bilindik Apache Ranger arabirimini kullanır. Ranger arka uçta kullanan [Apache Solr](https://hortonworks.com/apache/solr/) depolamak ve günlükleri aranıyor.

## <a name="encryption"></a>Şifreleme
Veri koruma, toplantı Kurumsal güvenlik ve uyumluluk gereksinimlerini için önemlidir. Erişim için verilerin yetkisiz çalışanların erişiminden korunmasının yanı sıra, bu şifreleme. 

HDInsight kümeleri--Azure Blob Depolama ve Azure Data Lake depolama Gen1/Gen2--destek saydam sunucu tarafı için her iki veri depoları [verilerin şifrelenmesi](../../storage/common/storage-service-encryption.md) bekleyen. Güvenli HDInsight kümeleri, bekleme sırasında veri sunucu tarafı şifreleme bu özellik ile sorunsuz bir şekilde çalışır.

## <a name="next-steps"></a>Sonraki adımlar

* [ESP ile HDInsight kümeleri planlama](apache-domain-joined-architecture.md)
* [ESP ile HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md)
* [ESP ile HDInsight kümelerini yönetme](apache-domain-joined-manage.md)
* [ESP ile HDInsight kümeleri için Apache Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md)
* [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)

