---
title: Azure HDInsight için - güvenlik ve iyi DevOps uygulamalarından şirket içi Apache Hadoop kümelerini geçirme
description: Güvenlik ve Azure HDInsight için geçirme şirket içi Hadoop kümeleri için en iyi DevOps öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 1a1cf731678ef7678b740020a4d61725f9a2b32a
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221991"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>Azure HDInsight için - güvenlik ve iyi DevOps uygulamalarından şirket içi Apache Hadoop kümelerini geçirme

Bu makalede, Azure HDInsight sistemlerinde güvenlik ve DevOps için öneriler sunar. Geçirme şirket içi Apache Hadoop sistemler ile Azure HDInsight için yardımcı olması için en iyi yöntemler sağlayan bir dizi gereksinimlerimizim bir parçasıdır.

## <a name="use-the-enterprise-security-package-to-secure-and-govern-the-cluster"></a>Kurumsal güvenlik paketi güvenli ve kümeyi yönetmek için kullanın

Kurumsal güvenlik paketi (ESP), Active Directory tabanlı kimlik doğrulaması, çoklu kullanıcı desteği ve rol tabanlı erişim denetimini destekler. Seçilen ESP seçeneği ile HDInsight kümesi için Active Directory etki alanına katılmış ve kuruluş yöneticisi Apache Ranger'ı kullanarak Hive güvenliği için rol tabanlı erişim denetimi (RBAC) yapılandırabilirsiniz. Yönetici ayrıca çalışanlar ve erişim denetim ilkelerinde yapılan değişiklikler göre veri erişimi denetleyebilirsiniz.

ESP özellikler şu anda Önizleme aşamasındadır ve yalnızca aşağıdaki küme türleri üzerinde kullanılabilir: Apache Hadoop, Apache Spark, Apache HBase, Apache Kafka ve Apache Interactive Query.

Etki alanına katılmış HDInsight kümesini dağıtmak için aşağıdaki adımları kullanın:

- Etki alanı adını geçirerek Azure Active Directory (AAD) dağıtma
- Azure Active Directory etki alanı Hizmetleri (AAD DS) dağıtma
- Gerekli sanal ağ ve alt ağ oluşturma
- AAD DS yönetmek için sanal ağda VM dağıtma
- VM'nin etki alanına ekleme
- Yükleme AD ve DNS araçları
- AAD DS yöneticisinin bir kuruluş birimi (OU) oluşturun
- LDAPS için AAD DS etkinleştir
- Alabilir ve böylece bir hizmet hesabı Azure Active Directory'de yetkilendirilmiş okuma ve yazma yönetici izni OU ile oluşturun. Bu hizmet hesabı daha sonra makinelerin etki alanına ve makine sorumluları OU içinde yerleştirin. Ayrıca, küme oluşturma sırasında belirttiğiniz OU içinde hizmet sorumluları de oluşturabilirsiniz.

    > [!Note]
    > Hizmet hesabı, AD etki alanı yöneticisi hesabı olması gerekmez.

- Aşağıdaki parametreleri ayarlayarak ESP HDInsight kümesi dağıtma:
    - **Etki alanı adı**: Azure AD DS ile ilişkili etki alanı adı.
    - **Etki alanı kullanıcı adı**: hizmet hesabı, örneğin önceki bölümde oluşturduğunuz Azure AD DS DC tarafından yönetilen etki alanında: `hdiadmin@contoso.onmicrosoft.com`. Bu etki alanı kullanıcısı bu HDInsight kümesinin yönetici olacaktır.
    - **Etki alanı parolası**: hizmet hesabının parolası.
    - **Kuruluş birimi**: Örneğin, HDInsight kümesi ile kullanmak istediğiniz kuruluş biriminin ayırt edici ad: `OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com`. Bu OU'ya mevcut değilse, HDInsight Küme hizmeti hesabının ayrıcalıklarını kullanarak OU oluşturmaya çalışır.
    - **LDAPS URL'si**: Örneğin, `ldaps://contoso.onmicrosoft.com:636`.
    - **Erişim kullanıcı grubu**: küme, örneğin eşitlemek istediğiniz kullanıcıları için güvenlik gruplarını: `HiveUsers`. Birden çok kullanıcı gruplarını belirtmek istiyorsanız, bunları noktalı virgülle ayırın. ';'. Gruplara ESP küme oluşturmadan önce dizinin mevcut olması gerekir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Bir etki alanına katılmış HDInsight kümeleriyle Hadoop güvenliğine giriş](../domain-joined/apache-domain-joined-introduction.md)
- [HDInsight, Azure etki alanına katılmış Hadoop kümeleri planlama](../domain-joined/apache-domain-joined-architecture.md)
- [Azure Active Directory Domain Services'ı kullanarak bir etki alanına katılmış HDInsight kümesi yapılandırma](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)
- [Azure Active Directory kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)
- [İçinde etki alanına katılmış HDInsight Hive ilkelerini yapılandırma](../domain-joined/apache-domain-joined-run-hive.md)
- [Apache Oozie etki alanına katılmış HDInsight Hadoop kümeleri çalıştırın.](../domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)

## <a name="implement-end-to-end-enterprise-security-management"></a>Uçtan uca Kurumsal güvenlik yönetimini uygulama

Son Kurumsal güvenlik sonuna aşağıdaki denetimleri kullanarak gerçekleştirilebilir:

- **Özel ve korumalı veri işlem hattı** (çevre düzeyi güvenlik):
    - Azure sanal ağları, ağ güvenlik grupları ve ağ geçidi hizmeti çevre düzeyi güvenliği sağlanabilir.

- **Kimlik doğrulaması ve veri erişimi için yetkilendirme**
    - Azure Active Directory Domain Services'ı kullanarak etki alanına katılmış HDI kümesi oluşturma. (Kurumsal güvenlik paketi)
    - AD kullanıcıları için küme kaynakları için rol tabanlı erişim sağlamak için Ambari kullanın
    - Erişim denetimi ilkeleri için Hive tablosunda ayarlamak için Apache Ranger'ı kullanma / sütun / satır düzeyi.
    - Kümeye SSH erişimi yöneticinin yalnızca sınırlı olabilir.

- **Denetim**
    - Tüm veri ve HDInsight kümesi kaynakları için erişim görünümünü ve raporunu.
    - Görüntüleme ve tüm değişiklikleri rapor için erişim denetim ilkeleri

- **Şifreleme**
    - Microsoft tarafından yönetilen anahtarlar veya müşteri tarafından yönetilen anahtarlar kullanılarak saydam sunucu tarafı şifrelemesi.
    - İstemci tarafı şifreleme, https ve TLS aktarım sırasında şifreleme kullanma

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

- En son kullanılabilir HDI sürümü kullanan yeni bir TEST HDI kümesi oluşturun.
- İşler ve iş yüklerini beklendiği gibi çalıştığından emin olmak için yeni kümede test edin.
- İşleri veya uygulamalarınız veya iş yükleriniz gerektiği gibi değiştirin.
- Küme düğümleri üzerinde yerel olarak depolanan tüm geçici verileri yedekleyin.
- Var olan kümeyi silin.
- HDInsight en son sürümünün bir küme aynı varsayılan veri ve meta depo önceki küme kullanarak aynı sanal ağ alt ağı oluşturun.
- Yedeklenen herhangi bir geçici veri içeri aktarın.
- Başlangıç işleri/yeni küme kullanarak işleme devam edin.

Daha fazla bilgi için bkz: [yeni bir sürüme yükseltme HDInsight kümesi](../hdinsight-upgrade-cluster.md)

## <a name="patch-cluster-operating-systems"></a>Küme işletim sistemi düzeltme eki

Yönetilen bir Hadoop hizmeti olan HDInsight HDInsight kümeleri tarafından kullanılan sanal makinelerin işletim sistemi düzeltme eki uygulama üstlenir.

Daha fazla bilgi için bkz: [HDInsight için işletim sistemi düzeltme eki uygulama](../hdinsight-os-patching.md)

## <a name="post-migration"></a>Geçiş sonrası

1. **Uygulamaları düzeltme** - yinelemeli olarak işler, işlemler ve betikler gerekli değişiklikleri yapın.
2. **Testleri gerçekleştirmek** - çalıştırmalarınızı işlevsel çalışma ve performans testleri
3. **En iyi duruma getirme** - yukarıdaki test sonuçlarına göre performans sorunları giderin ve performans iyileştirmeleri onaylamak için yeniden sınayın.

## <a name="appendix-gathering-details-to-prepare-for-a-migration"></a>Ek: bir geçişe hazırlamak için ayrıntıları toplanıyor

Bu bölüm hakkında önemli bilgiler toplamak amacıyla şablon soru sağlar:

- Şirket içi dağıtım.
- Proje Ayrıntıları.
- Azure gereksinimleri.

### <a name="on-premises-deployment-questionnaire"></a>Şirket içi dağıtım anketi

| **Soru** | **Örnek** | **Yanıt** |
|---|---|---|
|**Konu**: **ortamı**|||
|Küme dağıtım türü|Hortonworks, Cloudera, MapR| |
|Küme dağıtım sürümü|2.6.5, CDH 5.7 HDP|
|Büyük veri ekonomik sistemi bileşenleri|HDFS, Yarn, Hive, LLAP, Impala, Kudu, HBase, Spark, MapReduce, Kafka, Zookeeper, Solr, Sqoop, Oozie, Ranger, Atlas, Falcon, Zeppelin, R|
|Küme türleri|Hadoop, Spark, Confluent Kafka, Storm, Solr|
|Küme sayısı|4|
|Ana düğüm sayısı|2|
|Çalışan düğümü sayısı|100|
|Edge düğüm sayısı| 5|
|Toplam Disk alanı|100 TB|
|Ana düğüm yapılandırması|m/y, cpu, disk, vs.|
|Veri düğümleri yapılandırma|m/y, cpu, disk, vs.|
|Kenar düğümleri yapılandırma|m/y, cpu, disk, vs.|
|HDFS şifreleme?|Evet|
|Yüksek Kullanılabilirlik|HDFS HA, HA meta veri deposu|
|Olağanüstü Durum Kurtarma / yedekleme|Yedekleme kümesi?|  
|Kümede bağımlı sistemleri|SQL Server, Teradata, Power BI, MongoDB|
|Üçüncü taraf tümleştirme|Tableau, GridGain Qubole, Informatica, Splunk|
|**Konu**: **güvenlik**|||
|Çevre güvenliği|Güvenlik Duvarları|
|Küme kimlik doğrulaması ve yetkilendirme|Active Directory, Ambari, Cloudera Yöneticisi, kimlik doğrulama yok|
|HDFS erişim denetimi|  El ile ssh kullanıcıları|
|Hive kimlik doğrulaması ve yetkilendirme|Sentry, LDAP, Kerberos Ranger AD|
|Denetim|Ambari, Cloudera Gezgin Ranger|
|İzleme|Grafit, toplanan statsd, Telegraf, InfluxDB|
|Uyarı|Datadog Kapacitor, Prometheus,|
|Veri tutma süresi| 3 yıl, 5 yıl|
|Küme yöneticileri|Birden çok yöneticisi olan tek yönetici|

### <a name="project-details-questionnaire"></a>Proje Ayrıntıları anketi

|**Soru**|**Örnek**|**Yanıt**|
|---|---|---|
|**Konu**: **iş yükleri ve sıklığı**|||
|MapReduce işleri|10 işleri--günde iki kez||
|Hive işleri|100 iş--her saat||
|Spark toplu işler|50 işleri--15 dakikada bir||
|Spark akış işleri|5 iş--3 dakikada bir||
|Yapılandırılmış akış işleri|5 iş--dakika başı||
|ML Model eğitim işleri|bir hafta içerisinde bir kez 2 iş--||
|Programlama Dilleri|Python, Scala ve Java||
|Komut dosyaları|Shell, Python||
|**Konu**: **veri**|||
|Veri kaynakları|Düz dosyaları, Json, Kafka, RDBMS||
|Verileri düzenleme|Oozie iş akışları, hava akışı||
|Bellek arama|Apache Ignite, ignite Redis||
|Veri hedefleri|HDFS, RDBMS, Kafka, MPP ||
|**Konu**: **Meta verileri**|||
|Hive veritabanı türü|MySQL, Postgres||
|Hayır. Hive meta depolar|2||
|Hayır. Hive tabloları|100||
|Hayır. Ranger ilkeleri|20||
|Hayır. Oozie iş akışları|100||
|**Konu**: **ölçek**|||
|Çoğaltma dahil olmak üzere veri hacmi|100 TB||
|Günlük alımı hacmi|50 GB||
|Veri büyüme oranı|yıl başına %10||
|Küme düğümleri büyüme oranı|%5 yılda
|**Konu**: **küme kullanımı**|||
|Ortalama CPU yüzdesi|60%||
|Ortalama bellek yüzdesi|% 75||
|Kullanılan disk alanı|% 75||
|Ortalama ağ yüzdesi|%25
|**Konu**: **ekibi**|||
|Hayır. Yöneticileri|2||
|Hayır. Geliştiriciler|10||
|Hayır. Son kullanıcıları|100||
|Beceriler|Hadoop, Spark||
|Hayır. Geçiş çalışmalarında kullanılabilir kaynakları|2||
|**Konu**: **sınırlamaları**|||
|Geçerli sınırlamalar|Gecikmesi||
|Geçerli zorlukları|Eşzamanlılık sorunu||

### <a name="azure-requirements-questionnaire"></a>Azure gereksinimleri anketi

|**Konu**: **altyapı** |||
|---|---|---|
|**Soru**|**Örnek**|**Yanıt**|
| Tercih edilen bölge|ABD Doğu||
|Tercih edilen VNet?|Evet||
|HA / DR gerekli?|Evet||
|Diğer bulut hizmetleriyle tümleştirme?|ADF, CosmosDB||
|**Konu**: **veri taşıma**  |||
|İlk yükleme tercihi|DistCp, veri, ADF, WANDisco kutusu||
|Veri aktarımı değişim|DistCp, AzCopy||
|Sürekli artımlı veri aktarımı|DistCp, Sqoop||
|**Konu**: **izleme ve uyarı** |||
|Azure izleme ve uyarı Vs tümleştirme üçüncü taraf izleme kullanın|İzleme ve uyarı Azure kullanın||
|**Konu**: **güvenlik tercihleri** |||
|Özel ve korumalı veri işlem hattı?|Evet||
|Etki alanına katılmış kümeye (ESPP)?|     Evet||
|Böylece, şirket içinde AD eşitleme buluta?|     Evet||
|Hayır. AD kullanıcıları eşitleme?|          100||
|Bulut için parola eşitlemeyi Tamam?|    Evet||
|Kullanıcılar yalnızca bulut?|                 Evet||
|MFA gerekli?|                       Hayır|| 
|Veri yetkilendirme gereksinimleri var mı?|  Evet||
|Rol tabanlı erişim denetimi?|        Evet||
|Denetim gerekli?|                  Evet||
|Bekleyen verileri şifreleme?|          Evet||
|Aktarımdaki verileri şifreleme?|       Evet||
|**Konu**: **Re mimarisi tercihleri** |||
|Tek küme vs belirli küme türlerinin|Belirli küme türlerinin||
|Birlikte bulunan depolama Vs Uzak Depolama?|Uzaktaki Depolama Birimi||
|Veri olarak daha küçük bir küme boyutu uzaktan depolanır?|Daha küçük bir küme boyutu||
|Tek bir büyük küme yerine birden çok daha küçük bir küme kullanılır?|Birden çok daha küçük bir küme kullanın||
|Uzak meta veri deposu kullanabilir?|Evet||
|Paylaşım meta depolar farklı sunucular arasında?|Evet||
|İş yükleri Ayrıştır?|Spark işlerinde Hive işlerini değiştirin||
|ADF veri düzenlemesi için kullanılır?|Hayır||
|HDI vs Iaas HDP?|HDI||

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [HDInsight 4.0](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-introduction)