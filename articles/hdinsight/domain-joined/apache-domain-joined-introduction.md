---
title: Hdınsight - Hdınsight kümeleri etki alanına katılmış - Azure
description: Öğrenin ....
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 6c5e32f0ed39ce2e8c1e412dcfc6c04fb0f8bd7a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715267"
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Hadoop güvenlik etki alanına katılmış Hdınsight kümeleri ile giriş

Azure HDInsight şimdiye kadar yalnızca tek bir kullanıcının yerel yönetici olmasını destekliyordu. Bu durum küçük çaplı uygulama ekipleri veya departmanları için idealdi. Daha fazla popülerliği Kurumsal kesimdeki, active directory tabanlı kimlik doğrulaması gibi Kurumsal düzeydeki özellikleri gereksinimini elde edilen iş yükleri Hadoop tabanlı olarak birden çok kullanıcı desteği ve rol tabanlı erişim denetimi giderek önemli hale geldi. Etki alanına katılmış HDInsight kümeleri kullanarak Active Directory etki alanına katılmış HDInsight kümesi oluşturabilir ve kuruluşunuzdan HDInsight kümesinde oturum açmak için Azure Active Directory üzerinden kimlik doğrulaması yapılacak kullanıcıların listesi yapılandırabilirsiniz. Kuruluş dışından kullanıcılar HDInsight kümesinde oturum açamaz veya bu kümeye erişemez. Kuruluş yöneticisi rol tabanlı erişim denetimi güvenlik Hive kullanarak yapılandırabilirsiniz [Apache bırakabilmenizi](http://hortonworks.com/apache/ranger/), böylece erişimi verileri yalnızca gerektiği kadar sınırlama. Son olarak yönetici çalışanlara göre veri erişimi ve erişim denetim ilkelerinde yapılan değişiklikler için denetim gerçekleştirebilir, bu sayede kurumsal kaynakların yüksek düzeyde yönetilebilir olmasını sağlayabilir.

> [!NOTE]
> Aşağıdaki küme türlerinde yalnızca Önizleme'de bu makalede açıklanan yeni özellikler mevcuttur: Hadoop, Spark ve etkileşimli sorgu.

> [!IMPORTANT]
> Oozie etki alanına katılmış Hdınsight üzerinde etkin değil.

## <a name="benefits"></a>Avantajlar
Kurumsal güvenlik dört ana ayaklar – çevre güvenliği, kimlik doğrulama, yetkilendirme ve şifreleme içerir.

![Etki alanına katılmış HDInsight kümelerinin avantaj katmanları](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Çevre Güvenliği
HDInsight’ta çevre güvenliği sanal ağlar ve Ağ Geçidi hizmeti kullanılarak sağlanır. Bugün, bir kuruluş yöneticisi, bir Hdınsight kümesi sanal ağ içinde oluşturup bu ağ güvenlik grupları (güvenlik duvarı kuralları) sanal ağa erişimini kısıtlamak için kullanabilirsiniz. Yalnızca gelen güvenlik duvarı kurallarında tanımlana IP adresleri HDInsight kümesiyle iletişim kurabilir ve bu sayede çevre güvenliği sağlanmış olur. Başka bir güvenlik katmanı da Ağ Geçidi hizmetinin kullanılmasıdır. Ağ geçidi ilk Hdınsight kümesi için herhangi bir gelen talep için savunma hattı olarak davranan hizmetidir. İstekleri kabul eder, doğrular ve yalnızca bu adımların ardından isteğin kümedeki diğer düğümlere iletilmesine izin verir. Bu sayede kümedeki diğer ad ve veri düğümleri için çevre güvenliğini sağlamış olur.

### <a name="authentication"></a>Kimlik Doğrulaması
Bir etki alanına katılmış Hdınsight kümesi oluşturabileceğiniz bir kuruluş yöneticisi bir [sanal ağ](https://azure.microsoft.com/services/virtual-network/). HDInsight kümesinin düğümleri kuruluş tarafından yönetilen etki alanına katılacaktır. Bu sonuç [Azure Active Directory Etki Alanı Hizmetleri](../../active-directory-domain-services/active-directory-ds-overview.md) kullanılarak elde edilir. Kümedeki tüm düğümler kuruluş tarafından yönetilen bir etki alanına katılır. Bu kurulum sayesinde kuruluş çalışanları küme düğümlerinde etki alanı kimlik bilgileriyle oturum açabilirler. Ambari görünümleri, ODBC, JDBC, PowerShell ve REST API'leri kümeyle etkileşim kurmak için gibi diğer onaylı uç kimlik doğrulaması yapmak için etki alanı kimlik bilgilerini de kullanabilirsiniz. Yönetici bu uç noktalar üzerinden kümeyle etkileşimde bulunabilecek kullanıcı sayısını sınırlayabilir.

### <a name="authorization"></a>Yetkilendirme
Birçok kuruluş tarafından kullanılan en iyi yöntemlerden birisi, her çalışana kuruluş kaynakları erişimi vermemektir. Benzer şekilde, bu sürümle birlikte, yöneticinin küme kaynaklarını için rol tabanlı erişim denetimi ilkeleri tanımlayabilirsiniz. Örneğin yönetici [Apache Ranger](http://hortonworks.com/apache/ranger/)’ı Hive için erişim denetim ilkeleri belirleyecek şekilde yapılandırabilir. Bu işlev çalışanların yalnızca işlerinde başarılı olmak için gerekli olan verilere erişmelerini sağlar. Kümeye SSH erişimi de yalnızca yöneticiyle sınırlıdır.

### <a name="auditing"></a>Denetim
HDInsight kümesini yetkisiz kullanıcılardan korumanın ve verilerin güvenliğini sağlamanın yanı sıra küme kaynaklarına ve verilere erişimi denetlemek de kaynaklara yetkisiz veya istenmeyen erişimi takip etmek için gereklidir. Yönetici görüntülemek ve tüm erişim Hdınsight küme kaynakları ve veri rapor. Yönetici ayrıca Apache Ranger destekli uç noktalarda yapılan tüm erişim denetim ilkesi değişikliklerini de görüntüleyip raporlayabilir. Etki alanına katılmış HDInsight kümesi, denetim günlüklerinde arama yapmak için bilindik Apache Ranger arabirimini kullanır. Ranger arka uçta günlükleri kaydetmek ve arama yapmak için [Apache Solr](http://hortonworks.com/apache/solr/)’ı kullanır.

### <a name="encryption"></a>Şifreleme
Verilerin korunması kuruluş güvenliğini sağlamak ve uyum gereksinimlerini karşılamak için önemlidir. Verilerin yetkisiz çalışanların erişiminden korunmasının yanı sıra şifreleme ile de gerekli güvenlik sağlanmalıdır. HDInsight kümeleri ve Azure Storage Blob veri depolarının yanı sıra Azure Data Lake Store bekleyen veriler için sunucu tarafında [veri şifreleme](../../storage/common/storage-service-encryption.md) kullanır. Güvenli Hdınsight kümeleri, rest özelliğin veri bu sunucu tarafı şifrelemesi ile sorunsuz çalışır.

## <a name="next-steps"></a>Sonraki adımlar
* [Etki alanına katılmış Hdınsight kümeleri için planlama](apache-domain-joined-architecture.md).
* [Etki alanına katılmış Hdınsight kümeleri yapılandırma](apache-domain-joined-configure.md).
* [Etki alanına katılmış Hdınsight kümelerini yönetme](apache-domain-joined-manage.md).
* [Etki alanına katılmış Hdınsight kümeleri Hive ilkeleri yapılandırma](apache-domain-joined-run-hive.md).
* [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

