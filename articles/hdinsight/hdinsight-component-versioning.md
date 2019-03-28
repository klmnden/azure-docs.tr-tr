---
title: Apache Hadoop bileşenlerinin ve sürümlerinin - Azure HDInsight
description: Apache Hadoop bileşenleri ve sürümleri HDInsight ve bu Hortonworks Data Platform bulut dağıtımlarında kullanılabilir hizmet düzeyleri öğrenin.
keywords: hadoop sürümlerini, hadoop ekosistemi bileşenleri, hadoop bileşenleri, hadoop sürüm denetimi yapma
services: hdinsight
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: hrasheed
ms.openlocfilehash: 1783bf51c33a1dec84572b76149771a9723fe209
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519655"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>Apache Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilen nelerdir?

Hakkında bilgi edinin [Apache Hadoop](https://hadoop.apache.org/) ekosistemi bileşenleri ve Microsoft Azure HDInsight yanı sıra Kurumsal güvenlik paketi sürümleri. Ayrıca, HDInsight Hadoop bileşeni sürümlerinde denetleme konusunda bilgi edinin. 

Her bir HDInsight sürüm, Hortonworks Data Platform (HDP) sürümünün bir bulut dağıtımıdır.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Apache Hadoop bileşenleri farklı HDInsight sürümleri ile kullanılabilir
Azure HDInsight, herhangi bir zamanda dağıtılabilir birden çok Hadoop küme sürümleri destekler. Her sürüm seçimi HDP dağıtım belirli bir sürümünü ve dağıtımı içinde bulunan bileşenler kümesi oluşturur. 4 Nisan 2017'den itibaren Azure HDInsight tarafından kullanılan varsayılan küme sürümü 3.6 olduğu ve HDP 2.6 üzerinde temel alır.

HDInsight küme sürümleri ile ilişkili bileşen sürümü aşağıdaki tabloda listelenmiştir: 

> [!NOTE]  
> HDInsight hizmeti için varsayılan sürüm verilmeksizin. .NET SDK'sı ile Azure PowerShell ve klasik Azure CLI ile kümeleri oluşturduğunuzda, bir sürüm bağımlılığı varsa, HDInsight sürüm belirtin.

| Bileşen | HDInsight (Önizleme) 4.0 | HDInsight 3.6 (varsayılan) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 | HDInsight 3.1 | HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Veri Platformu |3.0 |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop ve YARN |3.1.1 |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.9.1 |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive ve HCatalog |-|1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive |3.1.0 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 |-| 0.8.4 |-|-|-|-|-|-|
| Apache Ranger |1.1.0 |0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |2.0.1 |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.7 |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.3.1 |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.2.1 |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |-|0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |5 |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.3.1 |2.3.0, 2.2.0, 2.1.0 |1.6.2, 2.0 |1.6.0 |1.5.2 |1.3.1 (yalnızca Windows) |-|-|
| Apache Livy |0,5 |0.4 |0.3 |0.3 |0.2 |-|-|-|
| Apache Kafka | 1.1 |1.1, 1.0 * (aşağıdaki nota bakın) | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.7.0 |2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.8.0 |0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |4.2.1 |3.2.8 |-|-|-|
| Apache kaydırıcı |-| 0.92.0 |-|-|-|-|-|-|

> [!NOTE]
> Sistem performans değerlendirmeleri nedeniyle Mart 2019 ' sürüm 0.10 Kafka için destek süresi sona erdi.

## <a name="check-for-current-hadoop-component-version-information"></a>Geçerli Hadoop bileşeni sürüm bilgileri için onay

HDInsight küme sürümleri ile ilişkili Hadoop ekosistemi bileşen sürümü, HDInsight için güncelleştirmeleri ile değiştirebilirsiniz. Hadoop bileşenleri denetimi ve bir küme için hangi sürümlerinin kullanıldığını doğrulamak için Ambari REST API'yi kullanın. **GetComponentInformation** komutu, hizmet bileşenleri hakkında bilgi alır. Ayrıntılar için bkz [Apache Ambari belgeleri][ambari-docs].

> [!IMPORTANT]    
> Linux üzerinde HDInsight sürüm 3.4 veya üzeri kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight üzerinde Windows emeklilik](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Sürüm notları

Bkz: [HDInsight sürüm notları](hdinsight-release-notes.md) HDInsight'ın en son sürümlerinde ek sürüm notları.

## <a name="supported-hdinsight-versions"></a>Desteklenen HDInsight sürümleri
HDInsight sürümleri aşağıdaki tablolarda listelenmiştir. HDInsight her sürüme karşılık HDP sürümleri ürün yayın tarihleri ile birlikte listelenir. Bunlar bilinen olduğunda destek sona erme ve sona erme tarihleri de sağlanır.

### <a name="available-versions"></a>Kullanılabilir sürümler

Aşağıdaki tabloda, PowerShell ve .NET SDK'sı gibi diğer dağıtım yöntemleri yanı sıra, Azure portalında kullanılabilen HDInsight sürümleri listelenmiştir.

| HDInsight sürümü | HDP sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek sona erme tarihi | Sona erme tarihi | Yüksek kullanılabilirlik |  Azure Portal kullanılabilirliği | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 <br> (Önizleme) |HDP 3.0 |Ubuntu 16.0.4 LTS |24 Eylül 2018 | | |Evet |Evet |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |4 Nisan 2017 | | |Evet |Evet |
| HDInsight 3.5 <br> (Spark)\* |HDP 2.6 |Ubuntu 16.0.4 LTS |30 Eylül 2016 |13 Mart 2019 |13 Mart 2019 |Evet |Evet |

*&ast; Spark küme türleri için yalnızca HDInsight 3.5 desteği genişletildi*

> [!NOTE]  
> Bir sürümünün süresi doldu için destek sonra onu Microsoft Azure Portalı aracılığıyla kullanılabilir olmayabilir. Ancak, küme sürümleri kullanılabilir kullanarak devam `Version` Windows PowerShell parametresi [yeni AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) komut ve .NET SDK'sı sürüm devre dışı bırakılacağı tarihten kadar.
>

### <a name="retired-versions"></a>Devre dışı bırakılan sürümleri

Aşağıdaki tabloda, HDInsight sürümleri listelenmiştir **değil** Azure portalında kullanılabilir.

| HDInsight sürümü | HDP sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek sona erme tarihi | Sona erme tarihi | Yüksek kullanılabilirlik |  Azure Portal kullanılabilirliği | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 <br> (Spark olmayan) |HDP 2.5 |Ubuntu 16.0.4 LTS |30 Eylül 2016 |5 Eylül 2017 |28 Haziran 2018 |Evet |Hayır |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29 Mart 2016 |29 Aralık 2016 |9 Ocak 2018 |Evet |Hayır |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 aralık 2015 |27 Haziran 2016 |31 Temmuz 2018 |Evet |Hayır |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2 aralık 2015 |27 Haziran 2016 |31 Temmuz 2017 |Evet |Hayır |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS veya Windows Server 2012 R2 |18 Şubat 2015 |1 Mart 2016 |1 Nisan 2017 |Evet |Hayır |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |24 Haziran 2014'e |18 Mayıs 2015 |30 Haziran 2016 |Evet |Hayır |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |11 Şubat 2014 |17 Eylül 2014'ten |30 Haziran 2015 |Evet |Hayır |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |28 Ekim 2013 |12 Mayıs 2014 |31 Mayıs 2015 |Evet |Hayır |
| HDInsight 1.6 |HDP 1.1 | |28 Ekim 2013 |26 Nisan 2014 |31 Mayıs 2015 |Hayır |Hayır |

> [!NOTE]  
> İki baş düğümü ile yüksek oranda kullanılabilir kümeleri, HDInsight sürüm 2.1 ve üzeri için varsayılan olarak dağıtılır. HDInsight sürüm 1.6 kümelerinde kullanılamaz.

## <a name="enterprise-security-package-for-hdinsight"></a>HDInsight için Kurumsal güvenlik paketi

Kurumsal güvenlik HDInsight kümenizi oluşturma küme iş akışının parçası olarak ekleyebileceğiniz isteğe bağlı bir pakettir. Kurumsal güvenlik paketi destekler:

- Kimlik doğrulaması için Active Directory ile tümleştirme.

    Geçmişte, yerel bir yönetici ve yerel bir SSH kullanıcısı ile HDInsight kümeleri yalnızca oluşturabilirsiniz. Yerel yönetici kullanıcı, tüm dosyaları, klasörleri, tablolar ve sütunlar erişebilirsiniz.  Kurumsal güvenlik paketi ile HDInsight kümeleri kendi Active Directory ile tümleştirerek rol tabanlı erişim denetimi etkinleştirebilirsiniz içeren Active Directory, Azure Active Directory etki alanı Hizmetleri veya ıaas Active Directory şirket sanal makine. Kullanıcılar kendi Kurumsal (etki alanı) kullanıcı adı ve parola kümeye erişmek için kullanılacak etki alanı yöneticisi küme üzerinde verebilirsiniz. 

    Daha fazla bilgi için bkz.

    - [Bir etki alanına katılmış HDInsight kümeleriyle Apache Hadoop güvenliğine giriş](./domain-joined/apache-domain-joined-introduction.md)
    - [HDInsight, Azure etki alanına katılmış Apache Hadoop kümeleri planlama](./domain-joined/apache-domain-joined-architecture.md)
    - [Etki alanına katılmış korumalı alan ortamını yapılandırma](./domain-joined/apache-domain-joined-configure.md)
    - [Azure Active Directory Domain Services'ı kullanarak etki alanına katılmış HDInsight kümelerini yapılandırma](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Veriler için yetkilendirme

  - Hive, Spark SQL ve Yarn kuyruklar için yetkilendirme için Apache Ranger tümleştirme.
  - Dosyalar ve klasörler üzerindeki erişim denetimi ayarlayabilirsiniz.

    Daha fazla bilgi için bkz.

  - [Etki alanına katılmış HDInsight Apache Hive ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)

- İzleyici erişir ve yapılandırılmış ilkeler için denetim günlüklerini görüntüleyin. 

### <a name="supported-cluster-types"></a>Desteklenen küme türleri

Şu anda yalnızca şu küme türlerini Kurumsal güvenlik paketi destekler:

- Hadoop (yalnızca HDInsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-storage"></a>Azure Data Lake depolama desteği

Birincil depolama alanı ve ek depolama Azure Data Lake Storage kullanarak kurumsal güvenlik paketi destekler.

### <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA
Kurumsal güvenlik paketi için fiyatlandırma ve SLA hakkında ek bilgi için bkz: [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hdinsight-windows-retirement"></a>HDInsight Windows'un devre dışı bırakılması
Microsoft Azure HDInsight sürüm 3.3 Windows üzerinde HDInsight son sürümünü oluştu. 31 Temmuz 2018'den Windows üzerinde HDInsight için kullanımdan kaldırma tarihtir. Tüm HDInsight kümeleri Windows 3.3 veya önceki sürümleri varsa, (HDInsight sürüm 3.5 veya sonraki sürümler) Linux'ta HDInsight için 31 Temmuz 2018'den önce geçirmeniz gerekir. Linux işletim sistemine geçiş oluşturun ya da HDInsight kümeleri yeniden boyutlandırma özelliği korumak sağlar. HDInsight sürümü 3.3 Windows üzerinde desteği, 27 Haziran 2016 tarihinde süresi doldu.

HDInsight sürüm 3.4 ile başlayarak, Microsoft HDInsight yalnızca Linux işletim sisteminde kullanıma sundu. Sonuç olarak, bazı bileşenlerin HDInsight içinde yalnızca Linux için kullanılabilir. Bunlar [Apache Ranger](https://ranger.apache.org/), [Apache Kafka](https://kafka.apache.org/), etkileşimli sorgu [Apache Spark](https://spark.apache.org/), HDInsight uygulamalarını ve Azure Data Lake Storage birincil dosya sistemi olarak. HDInsight'ın gelecek sürümlerini, yalnızca Linux işletim sisteminde kullanılabilir. Hiçbir Windows üzerinde HDInsight'ın gelecek sürümlerini olacaktır. 

## <a name="faqs"></a>SSS

### <a name="what-is-the-timeline-for-retiring-hdinsight-on-windows"></a>HDInsight üzerinde Windows devre dışı bırakma zaman çizelgesi nedir?
31 Temmuz 2018'den, Windows üzerinde HDInsight emeklilik tarihtir. Planlanan o tarihten bölgeniz için farklı ise, ayrı olarak bildirilir. 

### <a name="what-is-the-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>HDInsight üzerinde Windows mevcut müşteriler için devre dışı bırakma etkisi nedir?
Windows üzerinde HDInsight kullanımdan kaldırıldıktan sonra yeni bir HDInsight Windows kümesi oluşturma veya var olan HDInsight Windows kümesi yeniden boyutlandırma olamaz. HDInsight sürümü 3.3 desteği, 27 Haziran 2016 tarihinde süresi doldu. Bu nedenle, HDInsight 3.3 veya önceki sürümleri için hata düzeltmeleri veya desteği yoktur. HDInsight'ın gelecek sürümlerini, yalnızca Linux işletim sisteminde kullanılabilir. Hiçbir Windows üzerinde HDInsight'ın gelecek sürümlerini olacaktır.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Windows üzerinde HDInsight'ın hangi sürümleri etkilenir?
Azure HDInsight sürüm 3.3 HDInsight için Windows son sürümüdür. Windows üzerinde HDInsight kullanımdan önce tüm HDInsight Windows kümeleri sürüm 3.3 veya önceki sürüm 3.5 veya sonraki sürümler Linux'ta HDInsight geçirilmelidir. Linux üzerinde HDInsight kümelerinizi geçirme, yeni kümeler oluşturun veya var olan kümeleri yeniden boyutlandırma yeteneği korumak sağlar. 

### <a name="what-do-i-need-to-do"></a>Ne yapmam gerekiyor?
HDInsight Windows kümeleri, 31 Temmuz 2018'den önce desteklenen bir HDInsight Linux kümeye geçirme. Daha fazla bilgi [HDInsight dokumentu migrace](hdinsight-migrate-from-windows-to-linux.md). Azure HDInsight sürümleri hakkında daha fazla ayrıntı için bkz: listesini [desteklenen sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions). 

### <a name="where-do-i-find-the-cluster-os-type"></a>Küme işletim sistemi türü nerede bulabilirim?
Azure portalında, HDInsight kümesine Genel Bakış sayfasına gidin ve bulmak **küme türü** altında **Essentials**. Küme işletim sistemi türleri, ilgili sayfada listelenir. 

### <a name="i-cant-migrate-to-an-hdinsight-linux-cluster-by-july-31-2018-what-is-the-impact-to-my-hdinsight-windows-cluster"></a>Bir HDInsight Linux kümesi için 31 Temmuz 2018 tarihine kadar geçişi yapamazsınız. Windows HDInsight kümem için etkisi nedir?
HDInsight Windows kümesi olarak çalıştırır-olduğunu, ancak yeni bir HDInsight Windows kümesi oluşturma, veya var olan HDInsight Windows kümesi yeniden boyutlandırın. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>Kümem bir .NET bağımlılığı vardır. Bu bağımlılık Linux üzerinde nasıl giderebilirim?
Kullanarak, Linux küme bağımlılık çözümleyebilir [Mono projesi](https://www.mono-project.com/). Bu açık kaynak uygulaması .NET HDInsight Linux kümeleri için kullanılabilir. Daha fazla bilgi [HDInsight dokumentu migrace](hdinsight-migrate-from-windows-to-linux.md). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>Windows üzerinde HDInsight için yeni bir müşteri ortağıyım. Bir Windows HDInsight kümesine nasıl oluşturabilirim?
3 Temmuz 2017'den itibaren yalnızca var olan HDInsight Windows müşterilerin yeni HDInsight Windows kümeleri oluşturabilirsiniz. Yeni müşteriler PowerShell veya SDK'sını kullanarak Azure portalında bir Windows HDInsight kümesi oluşturulamıyor. Yeni müşteriler Linux HDInsight kümesi oluşturmanızı öneririz. Mevcut müşterileri yeni HDInsight Windows HDInsight kadar Windows üzerinde devre dışı bırakılacağı tarihten kümeler oluşturabilirsiniz. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-to-hdinsight-on-linux"></a>HDInsight Linux üzerinde HDInsight Windows üzerinde taşıma ile ilişkili bir fiyatlandırma etkisi var mı?
Hayır, fiyatlandırma ya da OS HDInsight için aynıdır. 

### <a name="what-are-the-customer-advantages-associated-with-the-move-to-only-using-hdinsight-on-linux"></a>Taşıma işlemi yalnızca Linux üzerinde HDInsight kullanma ile ilgili müşteri üstün olan yönleri nelerdir?
* Saati-açık kaynaklı büyük veri teknolojileri HDInsight hizmeti aracılığıyla market
* Büyük topluluk ve destek için bir ekosistem
* Açık kaynak topluluğu tarafından etkin olarak geliştirme Hadoop ve diğer büyük veri teknolojileri için çalışma olanağı

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>Linux üzerinde HDInsight sistemlerdekinden Windows üzerinde HDInsight içinde ek işlevsellik sağlar mı?
HDInsight sürüm 3.4 ile başlayarak, Microsoft HDInsight yalnızca Linux işletim sisteminde kullanıma sundu. Sonuç olarak, bazı bileşenlerin HDInsight içinde yalnızca Linux için kullanılabilir. Apache Ranger, Kafka, Interactive Query, Spark, HDInsight uygulamaları, bunlar ve birincil dosya sistemi olarak Azure Data Lake Storage. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight küme sürümleri için hizmet düzeyi sözleşmesi
Hizmet düzeyi sözleşmesi (SLA) de tanımlanan bir _destek penceresi_. Destek, Microsoft Müşteri Hizmetleri ve desteği tarafından desteklenen bir HDInsight kümesi sürüm süre penceredir. Sürüm varsa, bir _destek sona erme tarihi_ geçirilen, HDInsight kümesi desteği penceresi dışında. Desteklenen sürümler hakkında daha fazla bilgi için bkz: listesini [desteklenen HDInsight küme sürümleri](hdinsight-migrate-from-windows-to-linux.md). Belirtilen bir HDInsight sürüm (yeni bir X + 1 sürümü kullanıma sunulduktan sonra) X desteği sona erme tarihini sonraki hesaplanır biri:  

* Formül 1: 180 gün boyunca HDInsight kümesi sürüm X serbest bırakıldığında tarihe ekler.
* Formül 2: Olduğunda HDInsight kümesi sürüm X + 1 Azure portalında kullanılabilir hale getirileceğini tarihinden 90 gün ekleyin.

_Devre dışı bırakılacağı tarihten_ sonra küme sürümünü oluşturulamıyor HDInsight üzerinde tarihtir. 31 Temmuz 2017'den itibaren o tarihten sonra bir HDInsight kümesi yeniden boyutlandıramazsınız. 

> [!NOTE]  
> Azure konuk işletim sistemi ailesi sürüm 4, Windows Server 2012 R2 64 bit sürümünü kullanan Windows HDInsight kümeleri (2.1, 3.0, 3.1, 3.2 ve 3.3 dahil olmak üzere sürümler) çalıştırın. Azure konuk işletim sistemi ailesi 4 sürümü, .NET Framework sürüm 4.0, 4.5, 4.5.1 ve 4.5.2'yi destekler.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks sürüm notları HDInsight sürümleri ile ilişkili

Bölüm sürüm notları için HDInsight ile kullanılan Apache bileşenleri ve Hortonworks Data Platform dağıtımları için bağlantılar sağlar.
* HDInsight kümesi sürüm 4.0 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html)
* HDInsight küme sürümü 3.6 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.6](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* HDInsight kümesi sürüm 3.5 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.5](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). HDInsight kümesi sürüm 3.5 _varsayılan_ Azure portalında oluşturduğunuz Hadoop kümesi.
* HDInsight küme sürümü 3.4 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.4](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* HDInsight kümesi sürüm 3.3 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Apache Storm sürüm notları](https://storm.apache.org/2015/11/05/storm0100-released.html) Apache Web sitesinde mevcuttur.
  * [Apache Hive sürüm notları](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) Apache Web sitesinde mevcuttur.
* HDInsight kümesi sürüm 3.2 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.2][hdp-2-2].

  * Belirli Apache bileşenler için sürüm notları gibi kullanılabilir: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [ortak](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), ve [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight kümesi sürüm 3.1 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. Kasım, 7, 2014'ten önce oluşturulmuş 3.1 HDInsight kümeleri dayanır [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight kümesi sürüm 3.0 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight kümesi sürüm 2.1 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.3][hdp-1-3-0].
* HDInsight kümesi sürüm 1.6 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.1][hdp-1-1-0].



## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Kümeler için varsayılan düğüm yapılandırması ve sanal makine boyutları
Aşağıdaki tablolar, HDInsight kümeleri için varsayılan sanal makine (VM) boyutları listeler.  Bu grafik, HDInsight kümeleri dağıtmak için PowerShell veya Azure CLI betikleri oluştururken kullanılacak VM boyutlarını anlama gereklidir.

> [!IMPORTANT]  
> Bir kümedeki 32'den fazla alt düğüme ihtiyacınız varsa, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir.

* Brezilya Güney ve Japonya Batı dışındaki tüm desteklenen bölgeler:

|Küme türü|Hadoop|HBase|Interactive Query|Storm|Spark|ML Server|Kafka|
|---|---|---|---|---|---|---|---|
|HEAD: varsayılan VM boyutu|D12 v2|D12 v2|D13 v2|A3|D12 v2|D12 v2|D3v2|
|HEAD: Önerilen VM boyutları|D3 v2|D3 v2|D13|A4 v2|D12 v2|D12 v2|A2M v2|
||D4 v2|D4 v2|D14|A8 v2|D13 v2|D13 v2|D3 v2|
||D12 v2|D12 v2|E16 v3|A2m v2|D14 v2|D14 v2|D4 v2|
||E4 v3|E4 v3|E32 v3|E4 v3|E4 v3|E4 v3|D12 v2|
|Çalışan: varsayılan VM boyutu|D4 v2|D4 v2|D14 v2|D3 v2|D13 v2|D4 v2|Aracı başına 2 S30 disklerle D12v2 4|
|Çalışan: Önerilen VM boyutları|D3 v2|D3 v2|D13|D3 v2|D4 v2|D4 v2|D13 v2|
||D4 v2|D4 v2|D14|D4 v2|D12 v2|D12 v2|DS12 v2|
||D12 v2|D12 v2|E16 v3|D12 v2|D13 v2|D13 v2|DS13 v2|
||E4 v3|E4 v3|E20 v3|E4 v3|D14 v2|D14 v2|E4 v3|
||||E32 v3||E16 v3|E16 v3|ES4 v3|
||||E64 v3||E20 v3|E20 v3|E8 v3|
||||||E32 v3|E32 v3|ES8 v3|
||||||E64 v3|E64 v3||
|ZooKeeper: varsayılan VM boyutu||A4 v2|A4 v2|A4 v2||A2 v2|D3v2|
|ZooKeeper: Önerilen VM boyutları||A4 v2||A2 v2|||A2M v2|
|||A8 v2||A4 v2|||D3 v2|
|||A2m v2||A8 v2|||E8 v3|
|Edge: varsayılan VM boyutu||||||D4 v2||
|Sınırı: Önerilen VM boyut||||||D4 v2||
|||||||D12 v2||
|||||||D13 v2||
|||||||D14 v2||
|||||||E16 v3||
|||||||E20 v3||
|||||||E32 v3||
|||||||E64 v3||

* Brezilya Güney ve yalnızca Japonya Batı (v2 boyutları):

  | Küme türü | Hadoop | HBase | Interactive Query |Storm | Spark | ML Services |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: varsayılan VM boyutu |D12 |D12  | D13 |A3 |D12 |D12 |
  | HEAD: Önerilen VM boyutları |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |A3<br/> A4<br/> A5 |D12,<br/> D13,<br/> D14 |D12,<br/> D13,<br/> D14 |
  | Çalışan: varsayılan VM boyutu |D4 |D4  |  D14 |D3 |D13 |D4 |
  | Çalışan: Önerilen VM boyutları |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |D3,<br/> D4,<br/> D12 |D4,<br/> D12,<br/> D13,<br/> D14 | D4,<br/> D12,<br/> D13,<br/> D14 |
  | ZooKeeper: varsayılan VM boyutu | |A4 v2 | A4 v2| A4 v2 | | A2 v2|
  | ZooKeeper: Önerilen VM boyutları | |A2<br/> A3<br/> A4 | |A2<br/> A3<br/> A4 | | |
  | Edge: varsayılan VM boyutları | | | | | |D4 |
  | Edge: Önerilen VM boyutları | | | | | |D4,<br/> D12,<br/> D13,<br/> D14 |

> [!NOTE]
> - HEAD olarak bilinen *Nimbus* Storm için küme türü.
> - Çalışan olarak bilinen *gözetmen* Storm için küme türü.
> - Çalışan olarak bilinen *bölge* için HBase küme türü.

## <a name="next-steps"></a>Sonraki adımlar
- [Apache Hadoop, Spark ve HDInsight hakkında daha fazla bilgi için Kurulum küme](hdinsight-hadoop-provision-linux-clusters.md)
- [Bir Windows PC ile gelen HDInsight üzerinde Apache Hadoop çalışma](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: https://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: https://zookeeper.apache.org/
