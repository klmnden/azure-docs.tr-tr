---
title: "Hadoop bileşenleri ve sürümleri - Azure Hdınsight | Microsoft Docs"
description: "Hadoop bileşenleri ve Hdınsight ve bu Hortonworks veri platformu bulut dağıtımını kullanılabilir hizmet düzeyleri sürümlerde öğrenin."
keywords: "hadoop sürümleri, hadoop ekosistemi bileşenlerini, hadoop bileşenleri, hadoop sürümünü denetlemek nasıl"
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: bprakash
ms.openlocfilehash: 45cccb09753c85ae4a6d077d49cbd58630a9788a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="what-are-the-hadoop-components-and-versions-available-with-hdinsight"></a>Hadoop bileşenleri ve Hdınsight ile kullanılabilir sürümlerini nelerdir?

Apache Hadoop ekosistemi bileşenlerini ve kurumsal güvenlik paketinin yanı sıra Microsoft Azure Hdınsight sürümleri hakkında bilgi edinin. Ayrıca, hdınsight'ta Hadoop bileşen sürümü denetlemek öğrenin. 

Bulut dağıtım sürümünün Hortonworks veri Platformu (HDP) her Hdınsight sürümüdür.

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Hadoop bileşenleri farklı Hdınsight sürümleri ile kullanılabilir
Azure Hdınsight herhangi bir zamanda dağıtılabilir birden çok Hadoop küme sürümlerindeki destekler. Her sürüm seçimi HDP dağıtım belirli bir sürümü ve o dağıtım içinde bulunan bileşenleri kümesi oluşturur. 17 Şubat 2017'dan sonra Azure Hdınsight tarafından kullanılan varsayılan küme sürüm 3.5 ve HDP 2.5 üzerinde temel alır.

Hdınsight küme sürümleri ile ilişkili bileşen sürümü aşağıdaki tabloda listelenmiştir: 

> [!NOTE]
> Hdınsight hizmeti yönelik varsayılan sürüm verilmeksizin. Sürüm bağımlılık varsa, .NET SDK'sı, Azure PowerShell ve Azure CLI kümelerinizi oluşturduğunuzda, Hdınsight sürüm belirtin.

| Bileşen | Hdınsight 3.6 (varsayılan) | Hdınsight 3.5 | Hdınsight 3.4 | Hdınsight 3.3 | Hdınsight 3.2 | Hdınsight 3.1 | Hdınsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Veri Platformu |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop ve YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive ve HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0 (yalnızca Linux) |1.6.2 + 2.0 (yalnızca Linux) |1.6.0 (yalnızca Linux) |1.5.2 (deneysel yapı Linux yalnızca) |1.3.1 (yalnızca Windows) |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>Geçerli Hadoop bileşen sürüm bilgilerini denetleme

Hdınsight küme sürümleri ile ilişkili Hadoop ekosistemi bileşen sürümlerini güncelleştirmeleriyle Hdınsight'a değiştirebilirsiniz. Hadoop bileşenleri denetleyin ve hangi sürümlerinin bir küme için kullanılan doğrulamak için Ambari REST API kullanın. **GetComponentInformation** komutu hizmet bileşenleri hakkında bilgi alır. Ayrıntılar için bkz [Ambari belgelerine][ambari-docs].

Windows kümeleri için bileşen sürümü denetlemek için başka bir Uzak Masaüstü kullanarak bir küme için oturum açın ve C:\apps\dist\ dizinin içeriğini incelemek için yoludur.

> [!IMPORTANT]
> Linux üzerinde Hdınsight sürüm 3.4 veya üstü kullanılan yalnızca işletim sistemidir. Daha fazla bilgi için bkz: [Windows devre dışı bırakma hdınsight'ta](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Sürüm notları

Bkz: [Hdınsight sürüm notları](hdinsight-release-notes.md) Hdınsight'in en son sürümleri üzerinde ek sürüm notları.

## <a name="supported-hdinsight-versions"></a>Desteklenen Hdınsight sürümleri
Aşağıdaki tabloda Azure portalında şu anda kullanılabilir Hdınsight sürümleri listelenmiştir. Her Hdınsight sürümüne karşılık gelen HDP sürümleri ürün sürüm tarihleri ile birlikte listelenir. Bilinen zaman destek sona erme ve sona erme tarihleri de sağlanır.

> [!NOTE]
> Bir sürümünün süresi doldu için destek sonra Microsoft Azure Portalı aracılığıyla kullanılamayabilir. Ancak, küme sürümlerindeki kullanılabilir kullanarak devam `Version` Windows PowerShell parametresinde [yeni AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) komut ve sona erme tarihi sürüm kadar .NET SDK'sı.
> 
> İki baş düğümler ile yüksek oranda kullanılabilir küme Hdınsight sürüm 2.1 ve üzeri için varsayılan olarak dağıtılır. Hdınsight sürüm 1.6 kümeler için kullanılamaz.

| Hdınsight sürümü | HDP sürüm | VM İŞLETİM SİSTEMİ | Yüksek kullanılabilirlik | Sürüm tarihi | Azure portalındaki kullanılabilirliği | Destek sona erme tarihi | Sona erme tarihi |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Hdınsight 3.6 |HDP 2.6 |Ubuntu 16 |Evet |4 Nisan 2017 |Evet | | |
| Hdınsight 3.5 |HDP 2,5 |Ubuntu 16 |Evet |30 Eylül 2016 |Evet |5 Eylül 2017 |31 May 2018 |
| Hdınsight 3.4 |2.4 HDP |Ubuntu 14.0.4 LTS |Evet |29 Mart 2016 |Evet |29 Aralık 2016 |9 Ocak 2018 |
| Hdınsight 3.3 |2.3 HDP |Windows Server 2012 R2 |Evet |2 aralık 2015 |Evet |27 Haziran 2016 |31 Temmuz 2018 |
| Hdınsight 3.3 |2.3 HDP |Ubuntu 14.0.4 LTS |Evet |2 aralık 2015 |Evet |27 Haziran 2016 |31 Temmuz 2017 |
| Hdınsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS veya Windows Server 2012 R2 |Evet |18 Şubat 2015 |Hayır |1 Mart 2016 |1 Nisan 2017 |
| Hdınsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |Evet |24 Haziran 2014 |Hayır |18 Mayıs 2015 |30 Haziran 2016 |
| Hdınsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |Evet |11 Şubat 2014 |Hayır |17 Eylül 2014 |30 Haziran 2015 |
| Hdınsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |Evet |28 Ekim 2013 |Hayır |12 Mayıs 2014 |31 Mayıs 2015 |
| Hdınsight 1.6 |HDP 1.1 | |Hayır |28 Ekim 2013 |Hayır |26 Nisan 2014 |31 Mayıs 2015 |


## <a name="enterprise-security-package-for-hdinsight"></a>Hdınsight için Kurumsal güvenlik paketi

Azure Hdınsight oluşturma küme iş akışının bir parçası, Hdınsight kümesinde ekleyebileceğiniz isteğe bağlı bir pakettir. Kurumsal güvenlik paketi destekler:

- Kimlik doğrulaması için Active Directory ile tümleştirme.

    Geçmişte, bir yerel yönetici kullanıcı ve yerel bir SSH kullanıcı Hdınsight kümeleri yalnızca oluşturabilirsiniz. Yerel yönetici kullanıcı, tüm dosyaları, klasörleri, tablolar ve sütunlar erişebilir.  Kurumsal güvenlik paketiyle Hdınsight kümeleri kendi Active Directory ile tümleştirme tarafından rol tabanlı erişim denetimi etkinleştirebilirsiniz içeren Active Directory, Azure Active Directory etki alanı Hizmetleri veya Iaas üzerinde Active Directory şirket içi sanal makine. Etki alanı yöneticisi küme üzerinde kullanıcıların kendi şirket (etki alanı) kullanıcı adı ve parola kümeye erişmek kullanmasına izin verebilirsiniz. 

    Daha fazla bilgi için bkz.

    - [Hadoop güvenlik etki alanına katılmış Hdınsight kümeleri ile giriş](./domain-joined/apache-domain-joined-introduction.md)
    - [Hdınsight'ta Azure etki alanına katılmış Hadoop kümeleri planlama](./domain-joined/apache-domain-joined-architecture.md)
    - [Etki alanına katılmış sandbox ortamını yapılandırma](./domain-joined/apache-domain-joined-configure.md)
    - [Azure Active Directory etki alanı Hizmetleri kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Veriler için yetkilendirme

    - Hive, Spark SQL ve Yarn Kuyrukları için yetkilendirme için Apache bırakabilmenizi ile tümleştirme.
    - Dosyalar ve klasörler üzerinde erişim denetimi ayarlayabilirsiniz.

    Daha fazla bilgi için bkz.

    - [Etki alanına katılmış Hdınsight'ta Hive ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)

- İzleyici erişir ve yapılandırılmış ilkeler için denetim günlüklerini görüntüleyin. 

### <a name="supported-cluster-types"></a>Desteklenen küme türleri

Şu anda yalnızca şu küme türleri Kurumsal güvenlik paketi destekler:

- Hadoop (yalnızca Hdınsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-store"></a>Azure Data Lake Store desteği

Azure Data Lake Store birincil depolama ve ek depolama Kurumsal güvenlik paketi kullanılmasını destekler.

### <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA
Kurumsal güvenlik paketi için fiyatlandırma ve SLA hakkında bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hdinsight-windows-retirement"></a>HDInsight Windows'un devre dışı bırakılması
Microsoft Azure Hdınsight sürüm 3.3 Windows'da Hdınsight son sürümü oluştu. Windows'da Hdınsight için devre dışı bırakma 31 Temmuz 2018 tarihidir. Tüm Hdınsight kümeleri Windows 3.3 veya önceki sürümlerde varsa (Hdınsight sürüm 3.5 veya sonrasını) Linux'ta Hdınsight 31 Temmuz 2018 önce geçiş yapmanız gerekir. Linux işletim sistemine geçiş oluşturmak ya da Hdınsight kümelerinizi yeniden boyutlandırma özelliği tutmanıza olanak sağlar. Hdınsight sürüm 3.3 Windows için destek, 27 Haziran 2016 tarihinde süresi doldu.

Hdınsight sürüm 3.4 ile başlayarak, Microsoft Hdınsight yalnızca, Linux işletim sistemine yayımladı. Sonuç olarak, bazı bileşenleri Hdınsight içinde yalnızca Linux için kullanılabilir. Apache bırakabilmenizi, Kafka, etkileşimli sorgu, Spark, Hdınsight uygulamaları, bunlar ve Azure Data Lake Store birincil dosya sistemi olarak. Hdınsight'ın gelecek sürümlerinde, yalnızca Linux işletim sisteminde kullanılabilir. Hiçbir gelecek sürümlerde Windows'da hdınsight olacaktır. 

## <a name="faqs"></a>SSS

### <a name="what-is-the-timeline-for-retiring-hdinsight-on-windows"></a>Windows'da Hdınsight devre dışı bırakma için zaman çizelgesi nedir?
31 Temmuz 2018 Windows'da Hdınsight için devre dışı bırakma tarihidir. Planlanan tarihten bölgeniz için farklı olması durumunda, ayrı olarak bildirilir. 

### <a name="what-is-the-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>Hdınsight Windows üzerinde var olan müşteriler için devre dışı bırakma etkisi nedir?
Windows'da Hdınsight kullanımdan kaldırıldıktan sonra yeni bir Hdınsight Windows Küme oluşturun veya var olan bir Hdınsight Windows kümesine yeniden boyutlandırma olamaz. Hdınsight sürüm 3.3 desteği, 27 Haziran 2016 tarihinde süresi doldu. Bu nedenle, destek veya hata düzeltmeleri Hdınsight 3.3 veya önceki sürümlerinde yoktur. Hdınsight'ın gelecek sürümlerinde, yalnızca Linux işletim sisteminde kullanılabilir. Hiçbir gelecek sürümlerde Windows'da hdınsight olacaktır.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Windows'da Hdınsight'ın hangi sürümleri etkilenen?
Azure Hdınsight sürüm 3.3 Hdınsight için Windows son sürümüdür. Windows'da Hdınsight kullanımdan önce tüm Hdınsight Windows kümeleri sürüm 3.3 veya önceki sürüm 3.5 veya sonrasını Linux'ta Hdınsight geçirilmelidir. Linux'ta Hdınsight kümelerinizi geçme yeni kümeler oluşturun veya var olan kümeleri yeniden boyutlandırma yeteneği tutmanıza olanak sağlar. 

### <a name="what-do-i-need-to-do"></a>Yapmak neler gerekir?
Hdınsight Windows kümelerinizi 31 Temmuz 2018 önce desteklenen bir Hdınsight Linux kümeye geçirin. Daha fazla bilgi edinin [Hdınsight geçiş belge](hdinsight-migrate-from-windows-to-linux.md). Azure Hdınsight sürümleri hakkında daha fazla ayrıntı için listesi için bkz [desteklenen sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions). 

### <a name="where-do-i-find-the-cluster-os-type"></a>Küme işletim sistemi türü nereden bulabilirim?
Azure Portal'da Hdınsight kümesi genel bakış sayfasına gidin ve bulun **küme türü** altında **Essentials**. Küme işletim sistemi türü, bu sayfada listelenir. 

### <a name="i-cant-migrate-to-an-hdinsight-linux-cluster-by-july-31-2018-what-is-the-impact-to-my-hdinsight-windows-cluster"></a>Bir Hdınsight Linux kümeye 31 Temmuz 2018 tarafından geçişi yapamazsınız. My Hdınsight Windows küme üzerindeki etkisi nedir?
Hdınsight Windows Küme çalışır-olduğu, ancak yeni bir Hdınsight Windows Küme oluşturun veya var olan bir Hdınsight Windows kümesine yeniden boyutlandırma olamaz. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>My küme .NET bağımlılığa sahiptir. Bu bağımlılık Linux'ta nasıl giderebilirim?
Kullanarak Linux küme bağımlılığı çözümleyebilir [Mono proje](http://www.mono-project.com/). Bu açık kaynak uygulaması .NET, Hdınsight Linux kümeleri için kullanılabilir. Daha fazla bilgi edinin [Hdınsight geçiş belge](hdinsight-migrate-from-windows-to-linux.md). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>Windows'da Hdınsight için yeni bir müşteri ben. Bir Hdınsight Windows Küme nasıl oluşturabilir miyim?
3 Temmuz 2017 itibariyle yalnızca varolan Hdınsight Windows müşterileri yeni Hdınsight Windows kümeleri oluşturabilirsiniz. Yeni müşteriler PowerShell veya SDK kullanarak Azure portalında bir Hdınsight Windows Küme oluşturulamaz. Yeni müşteriler Linux Hdınsight kümesi oluşturmanızı öneririz. Mevcut müşterilerin yeni Hdınsight Windows kadar Windows'da Hdınsight tarihten kümeleri oluşturabilirsiniz. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-to-hdinsight-on-linux"></a>Linux'ta Hdınsight Hdınsight'ta Windows taşıma ile ilişkili bir fiyatlandırma etki var mı?
Hayır, fiyatlandırma ya da OS Hdınsight için aynıdır. 

### <a name="what-are-the-customer-advantages-associated-with-the-move-to-only-using-hdinsight-on-linux"></a>Taşıma işlemi yalnızca Hdınsight Linux üzerinde kullanma ile ilgili müşteri yararları nelerdir?
* Zaman-açık kaynak büyük veri teknolojileri Hdınsight hizmeti aracılığıyla market
* Büyük topluluk ve destek için ekosistemi
* Açık kaynak topluluğu tarafından etkin geliştirme Hadoop ve diğer büyük veri teknolojileri için çalışma olanağı

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>Linux'ta Hdınsight Windows'da hdınsight'ta kullanılabilenleri ötesinde ek işlevsellik sağlar mı?
Hdınsight sürüm 3.4 ile başlayarak, Microsoft Hdınsight yalnızca, Linux işletim sistemine yayımladı. Sonuç olarak, bazı bileşenleri Hdınsight içinde yalnızca Linux için kullanılabilir. Apache bırakabilmenizi, Kafka, etkileşimli sorgu, Spark, Hdınsight uygulamaları, bunlar ve Azure Data Lake Store birincil dosya sistemi olarak. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Hdınsight küme sürümleri için hizmet düzeyi sözleşmesi
Hizmet düzeyi sözleşmesi (SLA) cinsinden tanımlı bir _destek penceresi_. Bir Hdınsight kümesi sürüm Microsoft Müşteri Hizmetleri ve desteği tarafından desteklenmektedir süreyi destek penceredir. Sürüm varsa, bir _destek sona erme tarihi_ , geçtikten, Hdınsight kümesi desteği dışında bir penceredir. Listesi için desteklenen sürümleri hakkında daha fazla bilgi için bkz [desteklenen Hdınsight küme sürümleri](hdinsight-migrate-from-windows-to-linux.md). Belirtilen bir Hdınsight sürüm (yeni bir X + 1 sürümünü kullanıma sunulduktan sonra) X destek sona erme tarihini sonraki hesaplanır biri:  

* Formül 1: Hdınsight küme sürümü X serbest bırakıldığında tarih 180 gün ekleyin.
* Formül 2: ne zaman Hdınsight küme sürümü X + 1 Azure portalında kullanılabilir hale getirileceğini tarih 90 gün ekleyin.

_Tarihten_ geçtikten sonra küme sürümü oluşturulamıyor Hdınsight'ta tarihidir. 31 Temmuz 2017 başlayarak, o tarihten sonra Hdınsight kümesi boyutlandırılamaz. 

> [!NOTE]
> Hdınsight Windows kümeleri (2.1, 3.0, 3.1, 3.2 ve 3.3 dahil olmak üzere sürümler) Azure konuk işletim sistemi ailesi sürüm 4, Windows Server 2012 R2'in 64-bit sürümünü kullanan çalıştırın. Azure konuk işletim sistemi ailesi sürüm 4, .NET Framework sürüm 4.0, 4.5, 4.5.1 ve 4.5.2 destekler.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks sürüm notları Hdınsight sürümleri ile ilişkili

Bölüm, Hdınsight ile kullanılan Apache bileşenleri ve Hortonworks veri platformu dağıtımları için sürüm notlarına bağlantılar sağlar.
* Hdınsight küme sürümü 3.6 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* Hdınsight kümesi sürüm 3.5 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). Hdınsight kümesi sürüm 3.5 _varsayılan_ Azure portalında oluşturulan Hadoop kümesi.
* Hdınsight kümesi sürüm 3.4 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* Hdınsight küme sürümü 3.3 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Apache Storm sürüm notları](https://storm.apache.org/2015/11/05/storm0100-released.html) Apache Web sitesinde kullanılabilir.
  * [Apache Hive sürüm notları](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) Apache Web sitesinde kullanılabilir.
* Hdınsight kümesi sürüm 3.2 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.2][hdp-2-2].

  * Kullanılabilir aşağıdaki gibi olan belirli Apache bileşenleri için sürüm notları: [0.14 Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [ortak](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), ve [Oozie 4.1.0'da](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* Hdınsight kümesi sürüm 3.1 kullanan temel alan bir Hadoop dağıtım [Hortonworks veri platformu 2.1.7][hdp-2-1-7]. Kasım, 7, 2014 önce oluşturulan Hdınsight 3.1 kümeleri temel [Hortonworks veri platformu 2.1.1][hdp-2-1-1].
* Hdınsight kümesi sürüm 3.0 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.0][hdp-2-0-8].
* Hdınsight kümesi sürüm 2.1 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.3][hdp-1-3-0].
* Hdınsight kümesi sürüm 1.6 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.1][hdp-1-1-0].






## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Kümeler için varsayılan düğümü yapılandırması ve sanal makine boyutları
Aşağıdaki tablolar, Hdınsight kümeleri için varsayılan sanal makine (VM) boyutları listeler.

> [!IMPORTANT]
> Bir kümede 32'den fazla alt düğüm gerekirse, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir.
> 
> 

* Desteklenen tüm bölgeler Brezilya Güney ve Japonya Batı dışında:

  | Küme türü | Hadoop | HBase | Interactive Query | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: varsayılan VM boyutu |D3 v2 |D3 v2 | D13, D14 |A3 |D12 v2 |D12 v2 |
  | HEAD: VM boyutları önerilir |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |A3, A4, A5 |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Çalışan: varsayılan VM boyutu |D3 v2 |D3 v2  | D13, D14 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Çalışan: VM boyutları önerilir |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2 D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2 D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | ZooKeeper: varsayılan VM boyutu | |A3 | |A2 | | |
  | ZooKeeper: VM boyutları önerilir | |A3, A4, A5 | | A2, A3, A4 | | |
  | Edge: varsayılan VM boyutu | | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Kenar: VM boyutu önerilir | | | | | |Windows: D12 v2 D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Brezilya Güney ve yalnızca Japonya Batı (v2 boyutları):

  | Küme türü | Hadoop | HBase | Interactive Query |Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: varsayılan VM boyutu |D3 |D3  | D13, D14 |A3 |D12 |D12 |
  | HEAD: VM boyutları önerilir |D3, D4, D12 |D3, D4, D12  | D13, D14 |A3, A4, A5 |D12, D13, D14 |D12, D13, D14 |
  | Çalışan: varsayılan VM boyutu |D3 |D3  | D13, D14 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Çalışan: VM boyutları önerilir |D3, D4, D12 |D3, D4, D12  | D13, D14 |D3, D4, D12 |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |
  | ZooKeeper: varsayılan VM boyutu | |A2 | | A2 | | |
  | ZooKeeper: VM boyutları önerilir | |A2, A3, A4 | |A2, A3, A4 | | |
  | Edge: varsayılan VM boyutları | | | | | |Windows: D12; Linux: D4 |
  | Edge: VM boyutları önerilir | | | | | |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |

> [!NOTE]
> - HEAD olarak bilinir *Nimbus* Storm için küme türü.
> - Çalışan olarak bilinir *Supervisor* Storm için küme türü.
> - Çalışan olarak bilinir *bölge* HBase için küme türü.

## <a name="next-steps"></a>Sonraki adımlar
- [Hadoop, Spark ve Hdınsight hakkında daha fazla bilgi için Kurulum küme](hdinsight-hadoop-provision-linux-clusters.md)
- [Bir Windows PC Hdınsight'ta Hadoop ile çalışma](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
