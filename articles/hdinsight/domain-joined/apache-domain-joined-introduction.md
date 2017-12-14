---
title: "Hadoop güvenliği - etki alanına katılmış HDInsight kümeleri - Azure | Microsoft Belgeleri"
description: "Öğrenin ...."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/31/2016
ms.author: saurinsh
ms.openlocfilehash: 0a3558973014e47d470ef89d5d0f7c9ac15cb4d9
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Hadoop güvenlik etki alanına katılmış Hdınsight kümeleri ile giriş

Azure HDInsight şimdiye kadar yalnızca tek bir kullanıcının yerel yönetici olmasını destekliyordu. Bu durum küçük çaplı uygulama ekipleri veya departmanları için idealdi. Hadoop tabanlı iş yükleri kurumsal sektörde daha popüler hale geldikçe Active Directory tabanlı kimlik doğrulaması, çoklu kullanıcı desteği ve rol tabanlı erişim gibi kurumsal düzey özellikler daha önemli hale geldi. Etki alanına katılmış HDInsight kümeleri kullanarak Active Directory etki alanına katılmış HDInsight kümesi oluşturabilir ve kuruluşunuzdan HDInsight kümesinde oturum açmak için Azure Active Directory üzerinden kimlik doğrulaması yapılacak kullanıcıların listesi yapılandırabilirsiniz. Kuruluş dışından kullanıcılar HDInsight kümesinde oturum açamaz veya bu kümeye erişemez. Kuruluş yöneticisi [Apache Ranger](http://hortonworks.com/apache/ranger/)’ı kullanarak Hive güvenliği için rol tabanlı erişim denetimi yapılandırarak verilere erişimi ihtiyaca göre sınırlandırabilir. Son olarak yönetici çalışanlara göre veri erişimi ve erişim denetim ilkelerinde yapılan değişiklikler için denetim gerçekleştirebilir, bu sayede kurumsal kaynakların yüksek düzeyde yönetilebilir olmasını sağlayabilir.

> [!NOTE]
> Bu makalede açıklanan yeni özellikler yalnızca aşağıdaki küme türlerinde kullanılabilir: Hadoop, Spark ve etkileşimli sorgu. Gibi diğer iş yüklerinin, HBase, Storm ve Kafka, gelecek sürümlerde etkinleştirilecek.

> [!IMPORTANT]
> Oozie etki alanına katılmış Hdınsight üzerinde etkin değil.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="benefits"></a>Avantajlar
Kuruluş Güvenliği dört ana katmandan oluşur: Çevre Güvenliği, Kimlik Doğrulaması, Yetkilendirme ve Şifreleme.

![Etki alanına katılmış HDInsight kümelerinin avantaj katmanları](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Çevre Güvenliği
HDInsight’ta çevre güvenliği sanal ağlar ve Ağ Geçidi hizmeti kullanılarak sağlanır. Günümüzde kuruluş yöneticileri sanal ağ üzerinde HDInsight kümesi oluşturup Ağ Güvenlik Grupları’nı (gelen veya giden güvenlik duvarı kuralları) kullanarak sanal ağa erişimi sınırlayabilir. Yalnızca gelen güvenlik duvarı kurallarında tanımlana IP adresleri HDInsight kümesiyle iletişim kurabilir ve bu sayede çevre güvenliği sağlanmış olur. Başka bir güvenlik katmanı da Ağ Geçidi hizmetinin kullanılmasıdır. Ağ Geçidi hizmeti, HDInsight kümesine gelen tüm istekler için birinci savunma hattı olarak çalışır. İstekleri kabul eder, doğrular ve yalnızca bu adımların ardından isteğin kümedeki diğer düğümlere iletilmesine izin verir. Bu sayede kümedeki diğer ad ve veri düğümleri için çevre güvenliğini sağlamış olur.

### <a name="authentication"></a>Kimlik Doğrulaması
Bir etki alanına katılmış Hdınsight kümesi oluşturabileceğiniz bir kuruluş yöneticisi bir [sanal ağ](https://azure.microsoft.com/services/virtual-network/). HDInsight kümesinin düğümleri kuruluş tarafından yönetilen etki alanına katılacaktır. Bu sonuç [Azure Active Directory Etki Alanı Hizmetleri](../../active-directory-domain-services/active-directory-ds-overview.md) kullanılarak elde edilir. Kümedeki tüm düğümler kuruluş tarafından yönetilen bir etki alanına katılır. Bu kurulum sayesinde kuruluş çalışanları küme düğümlerinde etki alanı kimlik bilgileriyle oturum açabilirler. Kullanıcılar ayrıca etki alanı kimlik bilgilerini kullanarak Hue, Ambari Views, ODBC, JDBC, PowerShell ve REST API’leri gibi diğer onaylanmış uç noktaların kimlik doğrulamasından geçebilir ve kümeyle etkileşimde bulunabilirler. Yönetici bu uç noktalar üzerinden kümeyle etkileşimde bulunabilecek kullanıcı sayısını sınırlayabilir.

### <a name="authorization"></a>Yetkilendirme
Birçok kuruluş tarafından kullanılan en iyi yöntemlerden birisi, her çalışana kuruluş kaynakları erişimi vermemektir. Bu sürümde yönetici küme kaynakları için benzer şekilde rol tabanlı erişim denetim ilkeleri tanımlayabilir. Örneğin yönetici [Apache Ranger](http://hortonworks.com/apache/ranger/)’ı Hive için erişim denetim ilkeleri belirleyecek şekilde yapılandırabilir. Bu işlev çalışanların yalnızca işlerinde başarılı olmak için gerekli olan verilere erişmelerini sağlar. Kümeye SSH erişimi de yalnızca yöneticiyle sınırlıdır.

### <a name="auditing"></a>Denetim
HDInsight kümesini yetkisiz kullanıcılardan korumanın ve verilerin güvenliğini sağlamanın yanı sıra küme kaynaklarına ve verilere erişimi denetlemek de kaynaklara yetkisiz veya istenmeyen erişimi takip etmek için gereklidir. Yönetici görüntülemek ve tüm erişim Hdınsight küme kaynakları ve veri rapor. Yönetici ayrıca Apache Ranger destekli uç noktalarda yapılan tüm erişim denetim ilkesi değişikliklerini de görüntüleyip raporlayabilir. Etki alanına katılmış HDInsight kümesi, denetim günlüklerinde arama yapmak için bilindik Apache Ranger arabirimini kullanır. Ranger arka uçta günlükleri kaydetmek ve arama yapmak için [Apache Solr](http://hortonworks.com/apache/solr/)’ı kullanır.

### <a name="encryption"></a>Şifreleme
Verilerin korunması kuruluş güvenliğini sağlamak ve uyum gereksinimlerini karşılamak için önemlidir. Verilerin yetkisiz çalışanların erişiminden korunmasının yanı sıra şifreleme ile de gerekli güvenlik sağlanmalıdır. HDInsight kümeleri ve Azure Storage Blob veri depolarının yanı sıra Azure Data Lake Store bekleyen veriler için sunucu tarafında [veri şifreleme](../../storage/common/storage-service-encryption.md) kullanır. Güvenli HDInsight kümeleri bekleyen verileri de kapsayan sunucu tarafı veri şifreleme özelliği ile sorunsuz bir şekilde çalışacaktır.

## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış HDInsight kümesini yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md).
* Etki alanına katılmış HDInsight kümesini yönetmek için bkz. [Etki alanına katılmış HDInsight kümelerini yönetme](apache-domain-joined-manage.md).
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
