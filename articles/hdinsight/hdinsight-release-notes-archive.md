---
title: "Arşivlenen Sürüm Notları - Azure hdınsight'ta Hadoop bileşenleri | Microsoft Docs"
description: "Azure Hdınsight için Hadoop bileşenleri eski sürümleri için arşivlenen sürüm notları."
services: hdinsight
documentationcenter: 
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 04278aac85171601b5801b0890d14a9076060444
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="release-notes-archive-for-hadoop-components-on-azure-hdinsight"></a>Azure hdınsight'ta Hadoop bileşenleri için sürüm notları (arşiv)

Bu makalede, hakkında bilgi sağlar. **eski** Azure Hdınsight sürüm güncelleştirmeleri. Daha yeni sürümleri hakkında daha fazla bilgi için bkz: [Hdınsight sürüm notları](hdinsight-release-notes.md).

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight sürüm makale](hdinsight-component-versioning.md).



## <a name="notes-for-08302016-release-of-hdinsight"></a>Hdınsight 30/08/2016 sürüm notları
Bu sürüm ile dağıtılan Linux tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme | Ambari derleme |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8268980 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8268980 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8269383 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Bu sürüm ile dağıtılan Windows tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1033.2559206 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1033.2559206 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1033.2559206 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1033.2559206 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1033.2559206 |2.3 |2.3.3.1-25 |


## <a name="08172016---release-of-r-server-on-hdinsight"></a>17/08/2016 - hdınsight'ta R Server sürümü
* R Server 8.0.5 - çoğunlukla bir hata düzeltmesi serbest bırakma. Bkz: [R Server sürüm notları](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) daha fazla bilgi için.
* Kenar düğümüne - AzureML paketi [bu R paketi](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) R modellerini yayımlanan ve bir Azure ML web hizmeti olarak tüketilen sağlar.  Bkz: ["Bir Model faaliyete"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) bölümünü bizim ["Genel bakış, R Server üzerinde Hdınsight"](hdinsight-hadoop-r-server-overview.md) daha fazla bilgi için makalenin.
* Linux bağımlılıkları [100 en popüler R paketleri top](https://github.com/metacran/cranlogs) -bu Linux Paket bağımlılıklarını şimdi önceden yüklenir.
* R ekleme veri düğümlerine paketleri CRAN depoyu kullanacak şekilde seçeneği. Daha fazla bilgi için bkz: ["Hdınsight'ta R Server kullanmaya başlamanıza"](hdinsight-hadoop-r-server-get-started.md).
* Kümeleri oluşturduğunuzda, R Server sağlama güvenilirlik geliştirildi.

## <a name="notes-for-08012016-release-of-hdinsight"></a>Hdınsight 08/01/2016 sürüm notları
Bu sürüm ile dağıtılan Linux tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme | Ambari derleme |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8028416 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8028416 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8053402 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Bu sürüm ile dağıtılan Windows tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1005.2488842 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1005.2488842 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1005.2488842 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1005.2488842 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1005.2488842 |2.3 |2.3.3.1-25 |

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin Spark, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Hdınsight 3.4 kümeleri yapılan değişiklikler |Aşağıdaki hive yapılandırmaları için varsayılan değerleri daha iyi performans için değiştirilir <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul> |Hizmet |Tümü |Yok |
| Bu sürümde aşağıdaki düzeltmeler bulunmaktadır |HIVE 13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933 |Hizmet |Tümü |Yok |

## <a name="notes-for-07142016-release-of-hdinsight"></a>Hdınsight 07/14/2016 sürüm notları
Bu sürüm ile dağıtılan Linux tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme | Ambari derleme |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7932505 |2.2 |2.2.9.1-11 |2.2.1.12-2 |
| 3.3 |3.3.1000.0.7932505 |2.3 |2.3.3.1-18 |2.2.1.12-2 |
| 3.4 |3.4.1000.0.7933003 |2.4 |2.4.2.0 |2.2.1.12-2 |

Bu sürüm ile dağıtılan Windows tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme |
| --- | --- | --- | --- |
| 2.1 |2.1.10.989.2441725 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.989.2441725 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.989.2441725 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.989.2441725 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.989.2441725 |2.3 |2.3.3.1-21 |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Hdınsight 07/07/2016 sürüm notları
Bu sürüm ile dağıtılan Linux tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme |
| --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7864996 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.1000.0.7864996 |2.3 |2.3.3.1-18 |
| 3.4 |3.4.1000.0.7861906 |2.4 |2.4.2.0 |

Bu sürüm ile dağıtılan Windows tabanlı Hdınsight kümeleri için tam sürüm numaraları:

| HDI | HDI kümesi sürüm | HDP | HDP derleme |
| --- | --- | --- | --- |
| 2.1 |2.1.10.977.2413853 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.977.2413853 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.977.2413853 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.977.2413853 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.977.2413853 |2.3 |2.3.3.1-21 |

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin Spark, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| [Intellij için Hdınsight araçları](hdinsight-apache-spark-intellij-tool-plugin.md) |Hdınsight Spark kümeleri için Intellij Idea eklentisi Intellij için artık Azure araç seti ile tümleşiktir. Azure SDK'sı v2.9.1, en son Java SDK'ları, destekler ve tek başına Intellij için Hdınsight eklentisi tüm özellikleri içerir. |Araçlar |Spark |Yok |
| [Eclipse için Hdınsight araçları](hdinsight-apache-spark-eclipse-tool-plugin.md) |Eclipse için Azure Araç Seti artık Hdınsight Spark kümeleri destekler. Aşağıdaki özellikleri sağlar: <ul><li>Oluşturma ve Spark uygulama kolayca Scala ve Java birinci sınıf destek IntelliSense, otomatik biçimlendirme, hata denetimi, vb. için yazma ile yazma.</li><li>Spark uygulamayı yerel olarak test etme.</li><li>Hdınsight Spark küme iş göndermek ve sonuçları alın.</li><li>Azure'da oturum açma ve Azure aboneliği ile ilişkili tüm Spark kümeleri erişebilirsiniz.</li><li>Hdınsight Spark kümenizin tüm ilişkili depolama kaynaklarını gidin.</li></ul> |Araçlar |Spark |Yok |

Bu sürüm ile başlayarak, Linux tabanlı Hdınsight kümeleri için konuk işletim sistemi düzeltme eki uygulama ilkesi değişti. Düzeltme eki uygulama nedeniyle yeniden başlatma sayısını önemli ölçüde azaltmak için yeni ilke belirtilir. Yeni ilke düzeltme ekleri sanal makinelerde (VM'ler) Linux her Pazartesi ya da verilmiş bir küme düğümleri arasında 00: 00 UTC aşamalı bir şekilde başlayarak Perşembe kümeleri. Ancak, belirli bir VM yalnızca en fazla 30 konuk işletim sistemi düzeltme eki uygulama nedeniyle günde bir kez yeniden başlatır. Ayrıca, ilk yeniden başlatma yeni oluşturulan bir küme için küme oluşturma tarihten itibaren 30 gün daha erken gerçekleşmez.

> [!NOTE]
> Bu değişiklikler yalnızca yeni oluşturulan kümeleri veya daha büyük bu sürümü için geçerlidir.
>
>

## <a name="notes-for-06062016-release-of-hdinsight"></a>Hdınsight 06/06/2016 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

| HDP | HDI sürümü | Spark sürüm | Ambari yapı numarası | HDP yapı numarası |
| --- | --- | --- | --- | --- |
| 2.3 |3.3.1000.0.7702215 |1.5.2 |2.2.1.8-2 |2.3.3.1-18 |
| 2.4 |3.4.1000.0.7702224 |1.6.1 |2.2.1.8-2 |2.4.2.0 |

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin Spark, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Hdınsight'ta Spark, genel olarak kullanılabilir |Bu sürüm, kullanılabilirlik, ölçeklenebilirlik ve kaynak hdınsight'ta Apache Spark açmak için üretkenlik iyileştirmeleri getirir. <ul><li>Endüstri lideri kullanılabilirlik % 99,9 SLA yoğun kurumsal iş yükleri için uygun olmasını sağlayan.</li><li>Azure Data Lake Store kullanarak ölçeklenebilir depolama katmanı.</li><li>Veri keşfi ve geliştirme her aşaması için üretkenlik araçları. Etkileşimli veri incelemeyi özelleştirilmiş Spark çekirdeği ile Jupyter Not etkinleştirmek, Power BI ve Tableau Qlik gibi BI panoları ile tümleştirme hızlı veri paylaşımı ve sürekli raporlama için yararlı olan, Intellij eklentisi uzun vadeli kodu için güvenilir bir seçenektir Yapı geliştirme ve hata ayıklama.</li></ul> |Hizmet |Spark |Yok |
| Intellij için Hdınsight araçları |Bu, Hdınsight Spark kümeleri için Intellij Idea bir eklentidir. Aşağıdaki özellikleri sağlar:<ul><li>Oluşturma ve Spark uygulama kolayca Scala ve Java birinci sınıf destek IntelliSense, otomatik biçimlendirme, hata denetimi, vb. için yazma ile yazma.</li><li>Spark uygulamayı yerel olarak test etme.</li><li>Hdınsight Spark küme iş göndermek ve sonuçları alın.</li><li>Azure'da oturum açma ve Azure aboneliği ile ilişkili tüm Spark kümeleri erişebilirsiniz.</li><li>Hdınsight Spark kümenizin tüm ilişkili depolama kaynaklarını gidin.</li><li>Tüm iş geçmişi ve iş bilgilerini Hdınsight Spark kümenizin gidin.</li><li>Spark işlerinin masaüstü bilgisayarınızdan uzaktan hata ayıklama.</li></ul> |Araçlar |Spark |Yok |

## <a name="notes-for-05132016-release-of-hdinsight"></a>Hdınsight 13/05/2016 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* Hdınsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* Hdınsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* Hdınsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* Hdınsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin Spark, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Spark sürüm güncelleştirmesi ve diğer hata düzeltmeleri |Bu sürüm, Hdınsight kümesinde Spark sürüm 1.6.1 için güncelleştirir ve diğer hataları düzeltir |Hizmet |Spark |Yok |

## <a name="notes-for-04112016-release-of-hdinsight"></a>Hdınsight 11/04/2016 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* Hdınsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-değişmeden)
* Hdınsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* Hdınsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* Hdınsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| HDI 3.4 için özel meta depo yükseltme sorunları |Başka bir Hdınsight kümesi daha düşük bir sürümünü daha önce kullanılan bir özel meta depo, kullandıysanız, küme oluşturma işlemi başarısız olur. Şimdi sabit bir yükseltme komut dosyası hata nedeniyle, bu |Küme oluşturma |Tümü |Yok |
| Livy kilitlenme kurtarma |Livy gönderilen herhangi bir iş için iş durumu dayanıklılık sağlar |Güvenilirlik |Linux üzerinde Spark |Yok |
| Jupyter içerik HA |Kaydetme ve Jupyter not defteri içeriği için ve kümeyle ilişkili depolama hesabından yükleme olanağı sağlar. Daha fazla bilgi için bkz: [defterlerinde Jupyter not defterleri için kullanılabilen çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Dizüstü bilgisayarlar |Linux üzerinde Spark |Yok |
| Jupyter not defterleri hiveContext kaldırılması |Kullanım `%%sql` yerine Sihirli `%%hive` Sihirli. SqlContext için hiveContext eşdeğerdir. Daha fazla bilgi için bkz: [tekrar Jupyter not defterleri için kullanılabilir](hdinsight-apache-spark-jupyter-notebook-kernels.md) |Dizüstü bilgisayarlar |Linux üzerinde Spark kümeleri |Yok |
| Eski Spark sürümlerinin kullanımdan kaldırma |Eski sürümü Spark 1.3.1 hizmetinden 5/31 üzerinde kaldırıldı |Hizmet |Windows üzerinde Spark kümeleri |Yok |

## <a name="notes-for-03292016-release-of-hdinsight"></a>Hdınsight 03/29/2016 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 değişmeden -)
* Hdınsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* Hdınsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 değişmeden -)
* Hdınsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 değişmeden -)
* Hdınsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Eklenen Hdınsight 3.4 sürüm ve tüm Hdınsight kümeleri için güncelleştirilmiş HDP sürümleri |Bu sürüm ile (HDP 2.4 üzerinde bağlı olarak) Hdınsight v3.4 eklediyseniz ve diğer HDP sürümleri de güncelleştirdiniz. HDP 2.4 sürüm notları kullanılabilir [burada](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) ve Hdınsight sürümleri hakkında daha fazla bilgi bulunabilir [burada](hdinsight-component-versioning.md). |Hizmet |Tüm Linux kümeleri |Yok |
| Hdınsight Premium |Hdınsight iki kategoride - standart ve Premium kullanıma sunulmuştur. Hdınsight Premium şu anda önizlemede ve Hadoop için kullanılabilir ve Linux üzerinde Spark kümeleri. Daha fazla bilgi için bkz: [burada](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium). |Hizmet |Hadoop ve Linux üzerinde Spark |Yok |
| Microsoft R Server |Hdınsight Premium Linux'ta Hadoop ve Spark kümeleri ile birlikte Microsoft R Server sağlar. Daha fazla bilgi için bkz: [hdınsight'ta R Server genel bakış](hdinsight-hadoop-r-server-overview.md). |Hizmet |Hadoop ve Linux üzerinde Spark |Yok |
| Spark 1.6.0 |Hdınsight 3.4 kümeleri artık Spark 1.6.0 içerir |Hizmet |Linux üzerinde Spark kümeleri |Yok |
| Jupyter not defteri geliştirmeleri |Jupyter not defterleri ile Spark kümeleri kullanılabilir ek Spark çekirdek şimdi sağlar. Bunlar ayrıca kullanımı gibi geliştirmeler %% Sihirli, otomatik Görselleştirme ve Python görselleştirme kitaplıkları (örneğin, matplotlib) ile tümleştirme. Daha fazla bilgi için bkz: [defterlerinde Jupyter not defterleri için kullanılabilen çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Hizmet |Linux üzerinde Spark kümeleri |Yok |

## <a name="notes-for-03222016-release-of-hdinsight"></a>Hdınsight 22/03/2016 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 değişmeden -)
* Hdınsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* Hdınsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 değişmeden -)
* Hdınsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 değişmeden -)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Tüm Hdınsight kümeleri için güncelleştirilmiş Hdınsight sürümleri |Bu sürümle birlikte, biz tüm Hdınsight kümeleri için Hdınsight sürümleri güncelleştirdiniz |Hizmet |Tümü |Yok |

## <a name="notes-for-03102016-release-of-hdinsight"></a>Hdınsight 03/10/2016 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* Hdınsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 değişmeden -)
* Hdınsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* Hdınsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Tüm Hdınsight kümeleri için güncelleştirilmiş Hdınsight sürümleri |Bu sürümle birlikte, biz tüm Hdınsight kümeleri için Hdınsight sürümleri güncelleştirdiniz |Hizmet |Tümü |Yok |

## <a name="notes-for-01272016-release-of-hdinsight"></a>Hdınsight 27/01/2016 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* Hdınsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 değişmeden -)
* Hdınsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* Hdınsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Tüm Hdınsight kümeleri için güncelleştirilmiş Hdınsight sürümleri |Bu sürümle birlikte, biz tüm Hdınsight kümeleri için Hdınsight sürümleri güncelleştirdiniz |Hizmet |Tümü |Yok |

## <a name="notes-for-12022015-release-of-hdinsight"></a>Hdınsight 02/12/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 değişmeden -)
* Hdınsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* Hdınsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 değişmeden -)
* Hdınsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Eklenen Hdınsight 3.3 Sürüm ve tüm Hdınsight kümeleri için güncelleştirilmiş HDP sürümleri |Bu sürüm ile (HDP 2.3 üzerinde bağlı olarak) Hdınsight v3.3 eklediyseniz ve diğer HDP sürümleri de güncelleştirdiniz. HDP 2.3 sürüm notları kullanılabilir [burada](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) ve Hdınsight sürümleri hakkında daha fazla bilgi bulunabilir [burada](hdinsight-component-versioning.md). |Hizmet |Tümü |Yok |

## <a name="notes-for-11302015-release-of-hdinsight"></a>Hdınsight 11/30/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* Hdınsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Tüm Hdınsight kümeleri ve Hdınsight 3.2 kümeleri (Windows ve Linux) için HDP sürümleri için güncelleştirilmiş Hdınsight sürümleri |Bu sürümle birlikte, Hdınsight ve HDP sürümleri güncelleştirildi |Hizmet |Tümü |Yok |

## <a name="notes-for-10272015-release-of-hdinsight"></a>Hdınsight 27/10/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 değişmeden -)
* Hdınsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* Hdınsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Güncelleştirilmiş Hdınsight sürümleri tüm Hdınsight kümeleri (Windows ve Linux) |Bu sürümle birlikte, Hdınsight ve HDP sürümleri güncelleştirildi |Hizmet |Tümü |Yok |
| Büyük harf kümeleriyle sabit Jupyter için Windows Spark kümeleri |DNS adlarını büyük harflerle belirtilen sahip küme kaynağı isteği onay nedeniyle Jupyter not defterleri ile ilgili sorunları vardı. Küçük harflere Jupyter'ın yapılandırma için DNS adını değiştirmek için düzeltme oluştu. |Hizmet |Hdınsight Spark (Windows) |Yok |

## <a name="notes-for-10202015-release-of-hdinsight"></a>Hdınsight 20/10/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* Hdınsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* Hdınsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Varsayılan HDP sürüm HDP 2.2 değiştirildi |Hdınsight Windows kümeleri yönelik varsayılan sürüm HDP 2.2 değiştirilir. Hdınsight sürüm 3.2 (HDP 2.2) Şubat 2015 tarihinden itibaren içinde genellikle kullanılmaktadır. Açık bir seçim Azure portalı, PowerShell cmdlet'leri veya SDK'yı kullanarak küme hazırlama sırasında değil yapıldığında bu değişiklik, yalnızca varsayılan küme sürümü çevirir. |Hizmet |Tümü |Yok |
| Birden çok Hdınsight Linux kümeleri tek bir sanal ağ üzerinde dağıtmak için VM adı biçiminde değişiklikler |Tek bir sanal ağda birden çok Hdınsight Linux kümeleri dağıtmak için destek bu sürümde eklenir. Güncelleştirmesinin bir parçası olarak, headnode kümedeki sanal makine adları biçimi değişmiştir\*, workernode\* ve zookeepernode\* hn için\*, aşağı\*ve zk\* sırasıyla. Bu değiştirilebilir olduğundan doğrudan bağımlılık sanal makine adları biçiminin almak için önerilen bir yöntem değil. "Ana bilgisayar adı -f", konakları ve ana bileşenlerini eşleme listesini belirlemek için yerel makine veya Ambari API kullanın. Daha fazla bilgi konumunda bulabilirsiniz [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) ve [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). |Hizmet |Linux'ta Hdınsight kümeleri |Yok |
| Yapılandırma değişiklikleri |Hdınsight 3.1 kümeler için aşağıdaki yapılandırmaları şimdi etkinleştirilir: <ul><li>Tez.yarn.ATS.Enabled ve yarn.log.server.url. Bu uygulama zaman çizelgesi sunucusu ve günlük sunucusu günlüklerini sunmak sağlar.</li></ul>Hdınsight 3.2 kümeler için aşağıdaki yapılandırmaları değiştirildi: <ul><li>mapreduce.fileoutputcommitter.algorithm.Version 2 olarak ayarlanmış. Bu FileOutputCommitter V2 sürümünü kullanımını etkinleştirir.</li></ul> |Hizmet |Tümü |Yok |

## <a name="notes-for-09092015-release-of-hdinsight"></a>Hdınsight 09/09/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 değişmeden -)
* Hdınsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 değişmeden -)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Tüm Hdınsight kümeleri için güncelleştirilmiş Hdınsight sürümleri |Bu sürümle birlikte, Hdınsight sürümleri güncelleştirildi |Hizmet |Tümü |Yok |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Hdınsight 31/07/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 değişmeden -)
* Hdınsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 değişmeden -)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Spark küme düğümü yeniden görüntüsünü oluşturuyor iş akışı Düzelt |Spark küme düğümleri olmayan yeniden görüntü oluşturma sonra kurtarmak neden olan bir hata sabit |Hizmet |Spark |Yok |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Hdınsight 31/07/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 değişmeden -)
* Hdınsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 değişmeden -)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Tüm Hdınsight kümeleri için güncelleştirilmiş Hdınsight sürümleri |Bu sürümle birlikte, Hdınsight sürümleri güncelleştirildi |Hizmet |Tümü |Yok |

## <a name="notes-for-07072015-release-of-hdinsight"></a>Hdınsight 07/07/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 değişmeden -)
* Hdınsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

| Başlık | Açıklama | Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı) | Küme türü (örneğin, Hadoop, HBase veya Storm) | JIRA (varsa) |
| --- | --- | --- | --- | --- |
| Hdınsight 3.2 kümeleri için güncelleştirilmiş HDP sürümleri |Bu sürümle birlikte, Hdınsight 3.2 HDP 2.2.6.1-0012 dağıtır. |Hizmet |Tümü |Yok |

## <a name="notes-for-06262015-release-of-hdinsight"></a>Hdınsight 06/26/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 değişmeden -)
* Hdınsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Hdınsight 3.2 kümeleri için güncelleştirilmiş HDP sürümleri</td>
<td>Bu sürümle birlikte, Hdınsight 3.2 HDP 2.2.6.1 dağıtır.</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Hdınsight 18/06/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* Hdınsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Açılan ek HTTPS bağlantı noktaları</td>
<td>Bulut hizmeti şimdi küme örn 5 bağlantı noktaları için 8001 8005 açar https:// adresindeki<clustername>.azurehdinsight.net:8001/. Bu URL'leri isteklerine 443 numaralı bağlantı noktası aynı temel kimlik doğrulaması parola mekanizması kullanılarak doğrulanır. Bu bağlantı noktalarının aynı bağlantı noktasında etkin headnode bağlar. Betik eylemleri, bu bağlantı noktalarına headnode ve küme dışında bir yol dinleme Müşteri Hizmetleri yapmak için kullanılabilir.</td>
<td>Bulut hizmeti</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Hdınsight 3.2 aralıklı MapReduce karışık sorunun</td>
<td>Harita azaltmak karışık büyük kümelerinde bazen görev hatalarına kaynaklanan bir nadir, aralıklı yarış durumu düzeltin. Bkz: <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> daha fazla bilgi için.</td>
<td>Hadoop çekirdek</td>
<td>Tümü</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>
<tr>
<td>Taşıma için en son Azure Java SDK 2.2 Hdınsight 3.2 için</td>
<td>WASB sürücü tarafından kullanılan Java için Azure SDK'sının en son sürüme taşındı. En yeni SDK'ya birkaç düzeltmeleri sahip ve aynı için sürüm notları https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt kullanılabilir.</td>
<td>Hadoop çekirdek</td>
<td>Tümü</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>
<tr>
<td>Hdınsight 3.1 kümeleri için HDP 2.1.15 gitme</td>
<td>Yayın için Hortonworks sürüm notları kullanılabilir <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">burada</a>.</td>
<td>HDP</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Hdınsight 06/04/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 değişmeden -)
* Hdınsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Storm kümeleri için 502 hatalı ağ geçidi hata düzeltme</td>
<td>Bu sürüm iş gönderme aşağı bir yeniden başlatmadan sonra olacak şekilde Web sitesi neden API etkileyen bir hata düzeltmeleri.</td>
<td>Hizmet</td>
<td>Storm</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Hdınsight 06/01/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 değişmeden -))
* Hdınsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 değişmeden -)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Çeşitli hata düzeltmeleri</td>
<td>Bu sürüm küme sağlamak için ilgili hataları düzeltir.</td>
<td>Hizmet</td>
<td>Tüm küme türleri</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Hdınsight 27/05/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Diğer küme sürümleri ve SDK bu sürümünün bir parçası dağıtılmaz.

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>HDP 2.2 güncelleştirme</td>
<td>Bu Hdınsight 3.2 sürümünde HDP 2.2.6 içerir ve bazı önemli hata düzeltmeleri Hdınsight'a getirir. Tam sürüm notlarında kullanılabilir <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 sürüm notları</a>.</td>
<td>HDP</td>
<td>Tüm küme türleri</td>
<td>Yok</td>
</tr>
<tr>
<td>Varsayılan Yarn kapsayıcı bellek yapılandırmayı değiştirme</td>
<td>Bu güncelleştirme, düğüm Yöneticisi tarafından başlatılan varsayılan kullanılabilir bellek (yarn.nodemanager.resource.memory mb ve yarn.scheduler.maximum ayırma mb) YARN kapsayıcılara yükseltilmiştir 5632 MB. Daha önce bu 4608 MB azaltılmış olsa da, çeşitli iş çalışır dayalı, yeni değer daha iyi sağlaması gerekir güvenilirliğini ve performansını çoğu işleri, bu nedenle daha iyi bir varsayılan olan. Bu bellek yapılandırmasına kritik bağımlılığı varsa, her zamanki gibi açıkça küme oluşturulurken ayarlayın.</td>
<td>HDP</td>
<td>Tüm küme türleri</td>
<td>Yok</td>
</tr>
<tr>
<td>HBase ve Storm kümeleri için varsayılan yapılandırma eşlik</td>
<td>Bu güncelleştirme YARN yapılandırmalar aynı değerlerini Hadoop kümeleri olarak kullanmak için Hbase ve Storm kümelerini geri yükler. Bu, tüm küme türlerine eşlik için gerçekleştirilir.</td>
<td>HDP</td>
<td>HBase, Storm</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Hdınsight 20/05/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* Hdınsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>SCP.NET EventHub desteği</td>
<td>Hdınsight Storm güncelleştirilmiş küme paketleri SCP.NET için yeni özellikler duruma getirin. Şimdi EventHubSpout veya Java Spout'lar kullanmayı kolaylaştıran yeni API'ler topoloji Oluşturucu erişebilirsiniz. SCP.NET istemciniz sözleşmeleri güncelleştirilmiş gibi yeni kümeleriyle çalışmak için SDK güncelleştirmeniz gerekir. Yeni API'ler hakkında daha fazla bilgi için kullanım ve sürüm notları (hata düzeltmeleri de dahil olmak üzere) SCP.NET NuGet paketine dahil Benioku dosyasına bakın.</td>
<td>VS Tooling</td>
<td>Storm Hdınsight 3.2 kümeleri</td>
<td>Yok</td>
</tr>
<tr>
<td>JDBC sürücüsü güncelleştirme</td>
<td>Sürücü sqljdbc_4.1.5605.100 desteklenen bir SQL Server sürümünde güncelleştirildi.</td>
<td>Meta depo</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Hdınsight 27/04/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 değişmeden -)
* Hdınsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK'SI 1.5.8

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>DLL bağımlılık Düzelt</td>
<td>Birim testi çerçevesi Hdınsight bağlılığı kaldırır.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Yarış durumu için hata düzeltmesi</td>
<td>Küme durumunu yoklama önce kabul edilmesi için PUT isteğinde isteği şimdi bekler oluşturma</td>
<td>SDK</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Hdınsight 14/04/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 değişmeden -)
* Hdınsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK'SI 1.5.6

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Tez hata düzeltmeleri</td>
<td>Apache TEZ 2214 ve TEZ 1923 için düzeltmeler bu HDI 3.2 sürümünde bulunmaktadır. Bunlar veri önemli miktarda karışık gerektiren belirli Hive sorguları için Tez üzerinde gerekli.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Hdınsight 06/04/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 değişmeden -)
* Hdınsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 değişmeden -)
* Hdınsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 değişmeden -)
* Hdınsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK'SI 1.5.6

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Hdınsight .NET SDK'sı 1.5.6</td>
<td>Linux'ta Hdınsight için bazı iç sınıflar kaldırmak üzere güncelleştirir.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Avro kitaplığı 1.5.6</td>
<td>Eklenen <b>KnownTypeAttribute</b> yöntemi için <b>GetAllKnownTypes</b>. Bir tür için GetAllKnownTypes yöntemi null olduğunda sabit NullReferenceException.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Hata düzeltmeleri</td>
<td>Hizmetine çeşitli hata düzeltmeleri</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Hdınsight 01/04/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* Hdınsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* Hdınsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* Hdınsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK'SI 1.5.5

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Uzak Masaüstü kimlik bilgilerinin .NET SDK'sı üzerinden Windows kümelerde etkinleştir/devre dışı bırak yeteneği</td>
<td>RDP kimlik bilgileri Windows kümelerde devre dışı bırakma veya etkinleştirme programlama desteği.</td>
<td>SDK</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Sağlanacak sırada kümelerde Uzak Masaüstü kimlik bilgileri sağlamak için özelliği</td>
<td>Küme oluşturma sırasında Uzak Masaüstü kimlik bilgilerini etkinleştirmek için programlama desteği. Bu, ilk küme hazırlama ve Uzak Masaüstü'nü etkinleştirmek için iki adımlı işlem kaldırır.</td>
<td>SDK</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Yükseltilen Python 2.7.8 için</td>
<td>Hdınsight sürüm 2.1, 3.0, 3.1 ve 3.2 için bazı önemli güvenlik içeren yükseltilmiş Python Python 2.7.8, Hdınsight kümelerine üzerinde düzeltmeler</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>YARN yapılandırma değişikliği</td>
<td>Değiştirilen YARN yapılandırma yarn.resourcemanager.max tamamlandı-uygulamalar-1000 Hdınsight sürüm 3.1 ve 3.2 tüm küme türleri. Bu değer yalnızca YARN kullanıcı arabiriminde tamamlanmış uygulamaların listesini denetler. Kullanıcı Arabirimi üzerindeki gösterilen uygulamalar listesini önce gönderilen uygulamalar hakkında bilgi almak için geçmiş sunucusuna doğrudan gidebilirsiniz.</td>
<td>YARN</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Bir HBase kümesi düğüm yeniden boyutlandırma</td>
<td>HBase kümeleri artık düğümlerinin (yukarı ve aşağı) Hdınsight sürüm 3.1 ve 3.2 için yeniden boyutlandır</td>
<td>Hizmet</td>
<td>HBase</td>
<td>Yok</td>
</tr>
<tr>
<td>JDBC yükseltme</td>
<td>SQL JDBC sürücüsü sürüm sqljdbc_4.0.2206.100 Hdınsight sürüm 3.2 yükseltilir. Bu sürüm önemli güvenlik geliştirmeleri içerir.</td>
<td>HDP</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>JVM yapılandırmasını güncelleştirme</td>
<td>Güncelleştirilmiş JVM yapılandırma networkaddress.cache.ttl 300 saniye varsayılan değerinden Hdınsight sürüm 3.1 ve 3.2-1. Bu yapılandırma değeri önbellek ilkesi adı hizmetinden başarılı adı aramaları için denetler. Bu büyüyen ve HBase kümeleri küçültme ilgili bir hata düzeltmeleri.</td>
<td>Hizmet</td>
<td>HBase</td>
<td>Yok</td>
</tr>
<tr>
<td>Azure depolama Java 2.0 için SDK yükseltme</td>
<td>Hdınsight sürüm 3.2 Java için Azure depolama SDK'ın en son sürümünü kullanacak şekilde yükseltilir. Bu, geçerli 0.6.0 bazı önemli hata düzeltmeleri içerir sürümü.</td>
<td>HDP</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Apache WASB kaynak koduna yükseltme</td>
<td>Hdınsight sürüm 3.2 Apache Hadoop WASB dosya sistemi sürücüsünden için kullandığı en son kod artık. Bu değişiklikle WASB sürücü ayrı bir jar şimdi paketlenmiştir. Bu yalnızca bir paketleme değişiklik ve WASB sürücüsünün davranışını değişiklikleri içermiyor. Hadoop azure 2.6.0.jar bu JAR dosyasının adıdır.</td>
<td>HDP</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Hdınsight 3.2 dosya adı güncelleştirmelerinde jar</td>
<td>Bu güncelleştirme Hdınsight sürüm 3.2 için çeşitli hata düzeltmeleri içerir ve HDP bir parçası olarak paketlenmiş birkaç iç Kavanoz yükseltildi. Bu JAR filesare HDP pakete ve müşteri uygulamalar tarafından doğrudan kullanıma yönelik iç. Müşteri uygulamalar HDP iç Kavanoz yükseltme değil bölün böylece uygulamaları kendi Kavanoz sürümü paketi.</td>
<td>HDP</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Hdınsight 03/03/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 değişmeden -)
* Hdınsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 değişmeden -)
* Hdınsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 değişmeden -)
* Hdınsight 3.2.3.488.1375841 (HDP-2.2.10.0-değişmeden 2340 -)
* SDK 1.5.0 (değiştirmeden)

Bu sürüm aşağıdaki güncelleştirmeyi içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA</th>
</tr>
<tr>
<td>Güvenilirlik geliştirmeleri</td>
<td>Biz, küme oluşturma göre artan yük ile daha iyi ölçeklendirme hizmet izin düzeltmeleri yapılan.</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Hdınsight 18/02/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 değişmeden -)
* Hdınsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 değişmeden -)
* Hdınsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 değişmeden -)
* Hdınsight 3.2.3.471.1342507 (HDP 2.2.10.0 2340)
* SDK'SI 1.5.0

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Hdınsight 3.2 kümeleri</td>
<td>Hadoop 2.6/HDP2.2 Hdınsight 3.2 kümeleriyle kullanılabilir. Tüm açık kaynak bileşenlerini önemli güncelleştirmeleri içerir. Daha fazla bilgi için bkz: Hdınsight'ta yenilikler ve <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 sürüm notları</a>.</td>
<td>Açık kaynaklı yazılım</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Hdınsight Linux (Önizleme)</td>
<td>Ubuntu Linux üzerinde çalışan kümeleri dağıtılabilir. Daha fazla bilgi için Linux'ta Hdınsight ile çalışmaya başlama bakın.</td>
<td>Hizmet</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Storm genel kullanılabilirlik</td>
<td>Apache Storm kümeleri genel olarak kullanılabilir. Daha fazla bilgi için bkz: Get started Hdınsight'ta Storm kullanarak.</td>
<td>Hizmet</td>
<td>Storm</td>
<td>Yok</td>
</tr>
<tr>
<td>Sanal makine boyutları</td>
<td>Azure Hdınsight, daha fazla sanal makine türler ve boyutlarında kullanılabilir. Hdınsight A7 boyutlarını için genel amaçlar için A2 kullanabilir; Katı hal sürücüleri (SSD) ve yüzde 60 daha hızlı işlemci özellik D-serisi düğümleri; ve hızlı ağ için InfiniBand sahip A8 ve A9 boyutlarını destekler.</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Küme ölçeklendirme</td>
<td>Silme veya yeniden oluşturmak zorunda kalmadan, çalışan bir Hdınsight kümesine veri düğüm sayısını değiştirebilirsiniz. Şu anda, yalnızca Hadoop sorgu ve Apache Storm küme türleri küme türü yakında izlemektir Apache HBase için destek ancak bu özellik yok. Daha fazla bilgi için bkz: yönetmek Hdınsight kümeleri.</td>
<td>Hizmet</td>
<td>Hadoop, Storm</td>
<td>Yok</td>
</tr>
<tr>
<td>Visual Studio Araçları</td>
<td>Apache Storm için tam araç yanı sıra Apache Hive Visual Studio için araç deyim tamamlama, yerel doğrulama ve hata ayıklama için geliştirilmiş destek içerecek şekilde güncelleştirildi. Daha fazla bilgi için Visual Studio için Hdınsight Hadoop araçları ile çalışmaya başlama bakın.</td>
<td>Araçları</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<td>Azure Cosmos DB için Hadoop Bağlayıcısı</td>
<td>Azure Cosmos DB Hadoop bağlayıcısıyla Azure Cosmos DB koleksiyonları arasında veya veritabanı hesapları arasında saklanan şema daha az JSON belgeleri üzerinden karmaşık toplamalar, analiz ve işlemeleri gerçekleştirebilirsiniz. Daha fazla bilgi ve bir öğretici için Azure Cosmos DB ve Hdınsight kullanarak çalıştırmak Hadoop işlerini bakın.</td>
<td>Hizmet</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Hata düzeltmeleri</td>
<td>Biz Hdınsight Hizmetleri için çeşitli küçük hata düzeltmeleri yapıldı. Hiçbir müşteri dönük davranış değişiklikleri beklenir.</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Hdınsight 06/02/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 değişmeden -)
* Hdınsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 değişmeden -)
* Hdınsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK'SI YOK

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Hata düzeltmeleri</td>
<td>Biz Hdınsight Hizmetleri için çeşitli küçük hata düzeltmeleri yapıldı. Hiçbir müşteri dönük davranış değişiklikleri beklenir.</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>HDP 2.1 bakım güncelleştirmesi</td>
<td>Hdınsight 3.1 HDP 2.1.10.0 dağıtmak üzere güncelleştirilir. Daha fazla bilgi için bkz: <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">sürüm notları HDP-2.1.10</a>. </td>
<td>Açık kaynaklı yazılım</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>HDP ikili güncelleştirmeleri</td>
<td>HBase için hangi dosya adları güncelleştirilmiş birkaç JAR dosyalarını vardır. Müşteriler bu JAR dosyalarını adları bir bağımlılık olması beklenmez şekilde bu JAR dosyaları HBase tarafından dahili olarak kullanılır. Bunlar:
<ul>
<li>./lib/jetty-6.1.26.hwx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/jetty-Util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>Açık kaynaklı yazılım</td>
<td>HBase</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Hdınsight 29/1/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 değişmeden -)
* Hdınsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 değişmeden -)
* Hdınsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 değişmeden -)
* SDK'SI YOK

Bu sürüm aşağıdaki güncelleştirmeyi içerir:

<table border="1">

<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Etkilenen alan (örneğin, hizmet, bileşen veya SDK'sı)</p></th>
<th>Küme türü (örneğin, Hadoop, HBase veya Storm)</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Hata düzeltmeleri</td>
<td>Biz Azure yükseltmeler sırasında Hdınsight kümeleri güvenilirliğini geliştirmeye birkaç önemli hata düzeltmeleri yapıldı.</td>
<td>Hizmet</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-152015-release-of-hdinsight"></a>Hdınsight 5/1/2015 sürüm notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları:

* Hdınsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 değişmeden -)
* Hdınsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 değişmeden -)
* Hdınsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 değişmeden -)

Bu sürüm aşağıdaki güncelleştirmeleri içerir:

<table border="1">

<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Bileşen</th>
<th>Küme Türü</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Twitter eğilim analizi ve Mahout tabanlı film önerileri için örnek</td>
<td><p>Bu sürümde, Hdınsight sorgu konsol iki ek örnekler vardır:</p>

<p><b>Twitter eğilim analizi</b><br>
Twitter gibi siteler tarafından sağlanan ortak API'ler verileri çözümlemek ve popüler eğilimleri anlamak için yararlı bir kaynaktır. Bu öğreticide, Hive belirli bir sözcük içeren çoğu tweet'leri gönderilen Twitter kullanıcıların listesini almak için nasıl kullanılacağını öğrenin. </p>

<p><b>Mahout film önerisi</b><br>
Apache Mahout learning kitaplığı, Apache Hadoop bir makinedir. Mahout (örneğin, filtreleme, Sınıflandırma ve kümeleme) veri işlemeye algoritmalarını içerir. Bu öğreticide, bir öneri altyapısı arkadaşlarınızın gördünüz filmler üzerinde temel film önerileri oluşturma için kullanın.</p></td>
<td>Sorgu konsol</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Hive yapılandırması için varsayılan değer ile değiştirin: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Bu boyut yapılandırmasından otomatik olarak dönüştürülen harita birleştirmeler için geçerlidir. Değer belleğe sığmayacak hash eşlemleri dönüştürülüp tablolar boyutları toplamını temsil eder. Önceki bir sürümde, bu değer 10 MB varsayılan değerinden 128 MB olarak artar. Ancak, yeni değer 128 MB işlerinin nedeniyle bellek yetersizliği nedeniyle başarısız olmasına neden olmaktadır. Bu sürüm, varsayılan değer 10 MB olarak geri döner. Müşteriler, bu değer, kendi sorgu ve tablo boyutları verilen küme oluşturma sırasında geçersiz kılmak hala seçebilirsiniz. Bu ayar ve onu geçersiz kılmanıza hakkında daha fazla bilgi için bkz: <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">en iyi duruma getirme otomatik katılma dönüştürme</a> Hortonworks belgelerinde. </p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Hdınsight 23/12/2014 sürümünün notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları şunlardır:

* Hdınsight 2.1.10.420.1246783 (HDP sürüm değişmeden)
* Hdınsight 3.0.6.420.1246783 (HDP sürüm değişmeden)
* Hdınsight 3.1.1.420.1246783 (HDP sürüm değişmeden)

Bu sürüm aşağıdaki güncelleştirmeyi içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Bileşen</th>
<th>Küme Türü</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Aşırı yük nedeniyle aralıklı küme oluşturma hataları</td>
<td><p>Küme oluşturma sırasında HDP Paketleri indirmeye için geliştirilmiş algoritması aşırı yük nedeniyle hataları daha sağlam işlenmesini sağlar.</p></td>
<td>Hizmet</td>
<td>Hadoop, Hbase, Storm</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Hdınsight 18/12/2014 sürümünün notları
Bu sürüm aşağıdaki bileşen güncelleştirmeyi içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Bileşen</th>
<th>Küme Türü</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Genel kullanılabilirlik kümesi özelleştirme</a></td>
<td><p>Özelleştirme için Azure Hdınsight kümelerinizi Apache Hadoop ekosistemi kullanılabilir projelerle özelleştirmenize olanak sağlayabilir. Bu yeni özellik ile denemeler ve Azure Hdınsight Hadoop projeleri dağıtın. Bu üzerinden etkinleştirilir **betik eylemi** özelliği Hadoop kümeleri özel komut dosyaları kullanarak rastgele şekilde değiştirebilirsiniz. Bu özelleştirme Hdınsight kümeleri Hadoop, HBase ve Storm dahil olmak üzere tüm türleri kullanılabilir. Bu özellik gücünü göstermek için biz popüler yükleme işlemini belgelenen <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>, ve <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a>modüller. Bu sürüm ayrıca özelliği müşterilerin kendi özel betik eylemi Azure portalı üzerinden belirtmek kılavuzları ve en iyi uygulamaları yardımcı yöntemler kullanarak özel betik eylemleri oluşturma hakkında ve komut dosyasını test etme hakkında yönergeler sağlar ekler Eylem. </p></td>
<td>Özellik genel kullanılabilirlik</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Hdınsight 12/05/2014 sürümünün notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları şunlardır:

* Hdınsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* Hdınsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* Hdınsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* Hdınsight SDK'sı yok

Bu sürüm aşağıdaki bileşen güncelleştirmeleri içerir:

<table border="1">
<tr>
<th>Başlık</th>
<th>Açıklama</th>
<th>Bileşen</th>
<th>Küme Türü</th>
<th>JIRA (varsa)</th>
</tr>
<tr>
<td>Hata düzeltmesi: çok sayıda bölüm Hive DDL tablosuna eklenirken aralıklı hata oluştu </td>
<td><p>Hive meta depo veritabanı aralıklı bağlantısı hatasıyla ise bir Hive tablosu bölümlerin çok eklerken, Hive DDL başarısız olabilir. Bu sorun oluşursa Hive hata günlüğünde aşağıdaki deyimi isseen: </p><p>"Hata [ana]: ql. Sürücü (SessionState.java:printError(547)) - başarısız oldu: yürütme hatası, org.apache.hadoop.hive.ql.exec.DDLTask dönüş koddan 1. MetaException (message:java.lang.RuntimeException: commitTransaction openTransactionCalls çağrıldı = 0. Bu büyük olasılıkla openTransaction/commitTransaction dengesiz çağrıları olduğunu gösterir)"</p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>HIVE 482 (dışarıdan quoted edilemez şekilde bir iç JIRA budur. Burada başvurusunu not almıştınız.)</td>
</tr>
<tr>
<td>Hata düzeltmesi: Hdınsight sorgu konsolunda zaman askıda kalabilir</td>
<td>Bu durumda, aşağıdaki deyim WebHCat Başlatıcısı işi için WebHCat günlüğünde görülebilir: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): geçmiş dosyası {wasb url geçmiş dosyasına} yüklenemedi "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>HIVE 482 (dışarıdan quoted edilemez şekilde bir iç JIRA budur. Burada başvurusunu not almıştınız.)</td>
</tr>
<tr>
<td>Hata düzeltmesi: zaman ani artış Hbase sorguları gecikme süresi</td>
<td>Bu durumda, kullanıcılar, Hbase sorgularının gecikme 3 saniyelik zaman zaman bir depo fark edeceksiniz. </td>
<td>Hdınsight kümesi ağ geçidi</td>
<td>HBase</td>
<td>Yok</td>
</tr>
<tr>
<td>Dosya adı değişiklikleri HDP JAR</td>
<td>HDI için sürüm 3.0, burada birkaç değişiklik HDP tarafından yüklenen iç JAR dosyaları için küme. jetty 6.1.26.jar jetty 6.1.26.hwx.jar ile değiştirilmiştir. jetty kul 6.1.26.jar kul jetty 6.1.26.hwx.jar ile değiştirilmiştir. Bu değişiklikler, Hadoop, Mahout, WebHCat ve Oozie projeler için geçerlidir.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-11212014-release-of-hdinsight"></a>Hdınsight 21/11/2014 sürümünün notları
Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları şunlardır:

* Hdınsight 2.1.9.382.1169709 (11/14/2014 hiçbir değişiklik)
* Hdınsight 3.0.5.382.1169709 (11/14/2014 hiçbir değişiklik)
* Hdınsight 3.1.1.382.1169709 (11/14/2014 hiçbir değişiklik)
* Hdınsight SDK 1.4.0

Bu sürüm aşağıdaki bileşen güncelleştirmeleri içerir:

<table border="1">
<tr><th>Başlık</th><th>Açıklama</th><th>Bileşen</th><th>Küme Türü</th><th>JIRA (varsa)</th></tr>
<tr>
<td>Erişen uygulama günlükleri</td>
<td>Program aracılığıyla listeleme kümelerinizi üzerinde çalışan uygulamalar ve sorunlu uygulamalarda hata ayıklama yardımcı olmak için ilgili uygulamaya özgü veya kapsayıcı özgü günlükleri indirmek için yeteneği.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Bölge adı IHdInsightClient.DeleteCluster içinde belirtme özelliği </td>
<td>Azure Hdınsight SDK'sını kullanırken bir bölge adı belirtme olanağı sağlar **DeleteCluster**. Bu, farklı bölgelerde aynı ada sahip iki kaynakları var ve bunlardan birini silemiyor müşteriler engellemesini yardımcı olur.</td>
<td>SDK</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>ClusterDetails.DeploymentId</td>
<td>**ClusterDetails** nesnesi döndürür bir **Deploymentıd** alanı küme için benzersiz bir tanımlayıcıyı temsil eder. Küme oluşturma girişimleri aynı adı taşıyan arasında benzersiz olması sağlanır.</td>
<td>SDK</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Hdınsight 11/14/2014 sürümünün notları

Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları şunlardır:

* Hdınsight 2.1.9.382.1169709
* Hdınsight 3.0.5.382.1169709
* Hdınsight 3.1.1.382.1169709

Bu sürüm aşağıdaki yeni özellikleri, bileşen güncelleştirmeleri ve hata düzeltmelerini içerir.

<table border="1">
<tr><th>Başlık</th><th>Açıklama</th><th>Bileşen</th><th>Küme Türü</th><th>JIRA (varsa)</th></tr>
<tr>
<td>Betik eylemi (Önizleme)</td>
<td>Hadoop değiştirilmesini etkinleştirir kümesi özelleştirme özelliği önizlemesini özel komut dosyalarını kullanarak rastgele yollarla kümeleri. Bu özellik ile kullanıcılar deneme ve Azure Hdınsight kümelerine Apache Hadoop ekosistemi kullanılabilir olan projeler dağıtın. Bu özelleştirme özellik Hdınsight kümeleri, Hadoop, HBase ve Storm dahil olmak üzere tüm türleri kullanılabilir.</td>
<td>Yeni özellik</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>Azure Web siteleri ve depolama günlük analizi için önceden oluşturulmuş işleri</td>
<td>Hdınsight sorgu konsol verilerinizi veya örnek veriler üzerinde çalışmak çözümlerini destekler başlarken bir galeri sahiptir.
<p>**Verileriniz üzerinde çalışan çözümler**:<br>
İşlerini kendi çözümleri oluşturmak için bir başlangıç noktası sağlamak için en yaygın veri analizi senaryolardan bazıları için oluşturduk. Nasıl çalıştığını görmek için iş ile verilerinizi kullanabilirsiniz. Hazır olduğunuzda, önceden oluşturulmuş işinden sonra modellenir bir çözüm oluşturmak öğrendiniz kullanın.</p>
<p>**Örnek veriler üzerinde çalışan çözümler**:<br>
(Web günlükleri ve algılayıcı verilerini analiz etme gibi) bazı temel senaryolar üzerinden adım adım ilerlemenizi sağlayarak Hdınsight ile çalışma konusunda bilgi edinin. Bu tür verileri çözümlemek için Hdınsight kullanmayı ve bu verileri nasıl diğer uygulamalara ve hizmetlere bağlanabilir öğrenin. Microsoft Excel'e bağlanarak veri görselleştirme bu yaklaşım gücünü gösteren bir örnek sağlar.</p></td>
<td>Sorgu konsol</td>
<td>Hadoop</td>
<td>Yok</td>
</tr>
<tr>
<td>Templeton bellek sızıntısı düzeltme</td>
<td>İkinci bir ele kümeleri uzun süre çalışan vardı veya işin 100s gönderme müşteriler etkilenen Templeton bir bellek sızıntısı ister. Hizmetini yeniden başlatmak için Templeton 5xx hataları ve geçici bildirilmiş sorun oluştu. Geçici çözüm artık gerekli değildir.</td>
<td>Templeton</td>
<td>Tümü</td>
<td>https://issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>

> [!NOTE]
> Küme özelleştirme tarafından kullanılabilir hale yeni özellikleri göstermek için bir kümede Spark ve R modüllerini yüklemek için betik eylemi kullanarak yordamları belgelenmiştir. Daha fazla bilgi için bkz.

* [Yükleme ve Hdınsight kümelerinde Spark 1.0 kullanma](hdinsight-hadoop-spark-install.md)
* [Yükleme ve Hdınsight Hadoop kümeleri üzerinde R kullanma](hdinsight-hadoop-r-scripts.md)

## <a name="notes-for-11072014-release-of-hdinsight"></a>Hdınsight 07/11/2014 sürümünün notları

Bu sürüm ile dağıtılan Hdınsight kümeleri için tam sürüm numaraları şunlardır:

* Hdınsight 2.1 2.1.9.374.1153876
* Hdınsight 3.0 3.0.5.374.1153876
* Hdınsight 3.1 3.1.1.374.1153876

Bu sürüm aşağıdaki bileşen güncelleştirmeleri içerir:

<table border="1">
<tr><th>Başlık</th><th>Açıklama</th><th>Bileşen</th><th>Küme Türü</th><th>JIRA (varsa)</th></tr>
<tr>
<td>HDP 2.1.7</td>
<td>Bu sürüm Hortonworks veri Platformu (HDP) 2.1.7 temel alır. Daha fazla bilgi için bkz: <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">HDP 2.1.7 için sürüm notları</a>.</td>
<td>HDP</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
<tr>
<td>YARN zaman çizelgesi sunucu</td>
<td>Varsayılan olarak etkin YARN zaman çizelgesi sunucu (olarak da bilinen genel uygulama geçmişi). Zaman Çizelgesi sunucu tamamlanan uygulamalar (örneğin, uygulama kimliği, uygulama adı, uygulama durumu, uygulama gönderme zamanı ve uygulama tamamlama süresi) hakkında genel bilgi sağlar.

Bu uygulama bilgilerini baş düğümünden http://headnodehost:8188 URI erişerek veya YARN komutunu çalıştırarak alınabilir: yarn uygulama-- appStates listesinde tüm.

Bu bilgiler ayrıca uzaktan bir REST API olsa https://{ClusterDnsName adresindeki alınabilir}. azurehdinsight.NET/ws/v1/applicationhistory/.

Daha ayrıntılı bilgi için bkz: <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN zaman çizelgesi sunucu</a>.</td>
<td>Hizmet, YARN</td>
<td>Hadoop, HBase</td>
<td>Yok</td>
</tr>
<tr>
<td>Küme dağıtım kimliği</td>
<td>SDK sürüm 1.3.3.1.5426.29232 ile başlayarak, Hdınsight verilen her küme için benzersiz bir kimliği kullanıcılar erişebilir. Bu bir DNS adı yeniden zaman kümelerin benzersiz örnekleri şekil sağlar çapraz oluşturun veya bırakma senaryoları.</td>
<td>SDK</td>
<td>Tümü</td>
<td>Yok</td>
</tr>
</table>

> [!NOTE]
> Bu sürümde tam sürüm numarasını portalında yukarı gösteren ya da Windows PowerShell veya SDK tarafından döndürülen engelledi hata düzeltildi.

## <a name="notes-for-10152014-release"></a>15/10/2014 sürüm notları

Bu düzeltme sürüm Templeton ağır kullanıcılarının etkilenen Templeton içinde bellek sızıntısı sabit. Bazı durumlarda, istekler, çalıştırmak için yeterli belleğe sahip değil çünkü 500 hata kodları bildirilen hataları Templeton yoğun kullandı kullanıcılar görür. Templeton hizmetini yeniden başlatmak için bu sorun için geçici çözüm bulunuyordu. Bu sorun düzeltilmiştir.

## <a name="notes-for-1072014-release"></a>10/7/2014 sürüm notları

* Ambari kullanırken uç noktası, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *host_name* alan döndürür yalnızca ana bilgisayar adı yerine düğümün tam etki alanı adı (FQDN). Örneğin, döndürme yerine "**headnode0**", FQDN Al"**headnode0. { ClusterDNS} .azurehdinsight .net**". Bu değişikliği burada birden çok küme türleri (örneğin, HBase ve Hadoop) bir sanal ağ içinde dağıtılabilir senaryolarını kolaylaştırmak için gereklidir. Bu, örneğin, Hadoop için bir arka uç platform olarak HBase kullanarak olur.

* Hdınsight kümesi varsayılan dağıtımını yeni bellek ayarlarını sağladık. Önceki varsayılan bellek ayarlarını yeterli dağıtılan CPU çekirdeği sayısı için kılavuz için dikkate almamıştır. Bu yeni bellek ayarları (Hortonworks öneriler) uygun şekilde daha iyi Varsayılanları sağlamalıdır. Bunları değiştirmek için küme yapılandırmasının değiştirilmesi hakkında SDK başvurusu belgelerine başvurun. Varsayılan 4 CPU Çekirdeği (8 kapsayıcısı) Hdınsight küme tarafından kullanılan yeni bellek ayarlarını aşağıdaki tabloda listelenmektedir. (Bu sürümünden önce kullanılan değerleri de parenthetically sağlanır.)

<table border="1">
<tr><th>Bileşen</th><th>Bellek ayırma</th></tr>
<tr><td> yarn.Scheduler.minimum ayırma</td><td>768 MB (daha önce 512 MB)</td></tr>
<tr><td> yarn.Scheduler.Maximum ayırma</td><td>6144 (değiştirmeden) MB</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 (değiştirmeden) MB</td></tr>
<tr><td>Mapreduce.Map.Memory</td><td>768 MB (daha önce 512 MB)</td></tr>
<tr><td>mapreduce.Map.Java.opts</td><td>= Xmx512m (daha önce - Xmx410m) kabul eder</td></tr>
<tr><td>Mapreduce.reduce.Memory</td><td>1536 MB (daha önce 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>= Xmx1024m (daha önce - Xmx819m) kabul eder</td></tr>
<tr><td>yarn.app.mapreduce.AM.Resource</td><td>768 MB (daha önce 1024 MB)</td></tr>
<tr><td>yarn.app.mapreduce.AM.Command</td><td>= Xmx512m (daha önce - Xmx819m) kabul eder</td></tr>
<tr><td>mapreduce.Task.io.sort</td><td>256 MB (daha önce 200 MB)</td></tr>
<tr><td>Tez.AM.Resource.Memory</td><td>1536 MB (değiştirmeden)</td></tr>
</table>

YARN ve MapReduce Hdınsight tarafından kullanılan Hortonworks veri platformu üzerinde tarafından kullanılan bellek yapılandırma ayarları hakkında daha fazla bilgi için bkz: [HDP bellek yapılandırma ayarlarını belirlemek](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks uygun bellek ayarlarını hesaplamak için bir aracı de sağlanır.

Azure PowerShell ve Hdınsight SDK hata iletisi ile ilgili: "*küme HTTP Hizmetleri erişim için yapılandırılmamış*":

* Bu bilinen bir hatadır [uyumluluk sorunu](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) Hdınsight SDK veya Azure PowerShell sürümünü ve küme sürümü fark nedeniyle oluşabilir. 8/15 veya daha sonra oluşturulan kümeleri sanal ağlara yeni sağlama özelliği için destek vardır. Ancak bu özellik Hdınsight SDK veya Azure PowerShell eski sürümleri tarafından doğru Yorumlanmamış. Bazı iş gönderme işlemleri içinde bir hata oluşur. Hdınsight SDK API'leri veya Azure PowerShell cmdlet'leri kullanıyorsanız (**kullanım AzureRmHDInsightCluster** veya **Invoke-AzureRmHDInsightHiveJob**) işlerini göndermek için bu işlemler "hatailetisiylebaşarısızolabilir *Küme <clustername> HTTP Hizmetleri erişim için yapılandırılmamış*. " Veya (işlemine bağlı olarak), diğer hata iletileri gibi alabilirsiniz "*kümeye bağlanamıyor*".
* Bu uyumluluk sorunları Hdınsight SDK ve Azure PowerShell son sürümlerinde çözümlenir. Hdınsight SDK sürüm 1.3.1.6 veya üstü ve Azure PowerShell araçlarını 0.8.8 sürümüne güncelleştirme öneririz veya sonraki bir sürümü. En son Hdınsight SDK erişmek [NugGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started) ve Azure PowerShell Araçları [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Hdınsight 3.1 12/9/2014 sürümünün notları
* Bu sürüm Hortonworks veri Platformu (HDP) 2.1.5 temel alır. Bu sürümde düzeltilen listesi için bkz: [bu sürümde sabit](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) Hortonworks site sayfasında.
* Pig kitaplıkları klasöründe "avro mapred 1.7.4.jar" dosyası "avro-mapred-1.7.4-hadoop2.jar için." değiştirildi Bu dosyanın içeriğini bölünemez küçük bir hata düzeltme içerir. Müşterilerin doğrudan bağımlılık dosyaları yeniden adlandırıldığında sonundan kaçınmak için JAR dosyasının adına yapmayın önerilir.

## <a name="notes-for-8212014-release"></a>21/8/2014 sürüm notları
* Templeton denetleyicisi proje için varsayılan bellek sınırı 1 GB'lık ayarlar aşağıdaki WebHCat yapılandırma (HIVE-7155) ekliyoruz. (Önceki varsayılan değer 512 MB idi.)

     templeton.mapper.memory.mb (1024 =)

  * Bu değişikliği daha düşük bellek sınırlamaları nedeniyle bazı Hive sorguları aşağıdaki hatayla giderir: "Kapsayıcı fiziksel bellek sınırlarının dışına çalışıyor."
  * Eski varsayılan ayarlara dönmek için bu yapılandırma değeri 512 Azure PowerShell aracılığıyla küme oluşturma sırasında şu komutu kullanarak ayarlayabilirsiniz:

      Ekleme AzureRmHDInsightConfigValues-çekirdek @{"templeton.mapper.memory.mb"="512";}
* Zookeeper rolünün ana bilgisayar adı değiştirildi *zookeeper*. Bu ad çözümlemesi küme içindeki etkiler, ancak dış REST API'leri etkilemez. Kullanan bileşenleri varsa *zookeepernode* ana bilgisayar adı, bunları yeni adını kullanacak şekilde güncellemeniz gerekir. Üç zookeeper düğümleri için yeni adları şunlardır:

  * zookeeper0
  * zookeeper1
  * zookeeper2
* HBase sürüm destek matrisi güncelleştirilir. Yalnızca Hdınsight sürüm 3.1 (HBase sürüm 0,98) üretim HBase iş yükleri için desteklenir. (Önizleme için kullanılabilir) sürüm 3.0 taşıma İleri desteklenmiyor.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>15/8/2014 önce oluşturulan kümeleri ile ilgili notlar
Bir Azure PowerShell veya Hdınsight SDK hata iletisi "Küme <clustername> HTTP Hizmetleri erişim için yapılandırılmamış" (veya işlemine bağlı olarak diğer hata iletileri gibi: "kümeye bağlanamıyor") bir sürüm fark nedeniyle karşılaştı Azure PowerShell veya Hdınsight SDK'sı ile bir küme arasında. 8/15 veya daha sonra oluşturulan kümeleri sanal ağlara yeni sağlama özelliği için destek vardır. Bu özellik, Azure PowerShell veya iş gönderme işlemlerinin hatalarına Hdınsight SDK eski sürümleri tarafından doğru yorumlanması değil. İşlerini göndermek için Hdınsight SDK API'leri veya Azure PowerShell cmdlet'lerini (örneğin, kullanım AzureRmHDInsightCluster veya Invoke-AzureRmHDInsightHiveJob) kullanıyorsanız, bu işlemler açıklanan hata iletilerinden biri ile başarısız olabilir.

Bu uyumluluk sorunları Hdınsight SDK ve Azure PowerShell son sürümlerinde çözümlenir. Hdınsight SDK sürüm 1.3.1.6 veya üstü ve Azure PowerShell araçlarını 0.8.8 sürümüne güncelleştirme öneririz veya sonraki bir sürümü. En son Hdınsight SDK erişmek [NuGet][nuget-link]. Azure PowerShell araçları kullanarak erişebilirsiniz [Microsoft Web Platformu yükleyicisi][webpi-link].

## <a name="notes-for-7282014-release"></a>28/7/2014 sürüm notları
* **Yeni bölgelerdeki Hdınsight**: biz Hdınsight coğrafi varlığı üç bölgelere genişletilmiş. Hdınsight müşteriler, bu bölgelerde kümeleri oluşturabilirsiniz:
  * Doğu Asya
  * Orta Kuzey ABD
  * Orta Güney ABD
* Hdınsight sürüm 1.6 (HDP 1.1 ve Hadoop 1.0.3) ve Hdınsight sürüm 2.1 (HDP1.3 ve Hadoop 1.2) Azure portalından kaldırılıyor. Azure PowerShell cmdlet'ini kullanarak bu sürümleri için Hadoop kümeleri oluşturma olanağına [yeni AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) veya kullanarak [Hdınsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx). Daha fazla bilgi için bkz: [Hdınsight bileşen sürümü oluşturma](hdinsight-component-versioning.md).
* Bu sürümde Hortonworks veri Platformu (HDP) değişiklikleri:

<table border="1">
<tr><th>HDP</th><th>Değişiklikleri</th></tr>
<tr><td>1.3 HDP / HDI 2.1</td><td>Değişiklik yok</td></tr>
<tr><td>2.0 HDP / HDI 3.0</td><td>Değişiklik yok</td></tr>
<tr><td>2.1 HDP / HDI 3.1</td><td>zookeeper: ['3.4.5.2.1.3.0-1948'] ['3.4.5.2.1.3.2-0002'] -></td></tr>
</table>

## <a name="notes-for-6242014-release"></a>24/6/2014 sürüm notları
Bu sürüm Hdınsight hizmeti geliştirmeler içerir:

* **HDP 2.1 kullanılabilirlik**: (HDP 2.1 içeren) Hdınsight 3.1 genel olarak kullanılabilir ve yeni küme için varsayılan sürümdür.
* **HBase - Azure portal geliştirmeleri**: HBase kümeleri önizlemede kullanılabilen yapıyoruz. Portal yalnızca birkaç tıklama ile HBase kümeleri oluşturabilirsiniz.

HBase ile milyonlarca uç noktadan gelen algılayıcı ve telemetri verilerini depolayan Hizmetleri için büyük veri kümeleriyle çalışmak etkileşimli Web sitelerinde Hdınsight üzerinde gerçek zamanlı iş yüklerinin oluşturabilirsiniz. Bu iş yükleri Hadoop işleriyle verileri analiz etmek için sonraki adım olabilir ve bu Azure PowerShell ve Hive küme Panosu aracılığıyla hdınsight'ta mümkündür.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout Hdınsight 3.1 önceden yüklenmiş
 [Mahout](http://hortonworks.com/hadoop/mahout/) Mahout işleri ek küme yapılandırması gerek kalmadan çalıştırabilmeniz için Hdınsight 3.1 Hadoop kümeleri üzerinde önceden yüklenir. Örneğin, uzak bir Hadoop kümesine Uzak Masaüstü Protokolü (RDP) kullanarak yapabilecekleriniz ve ek adımlar olmadan, aşağıdaki Hello World Mahout komutu çalıştırabilirsiniz:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Bu yordamı daha eksiksiz bir açıklaması için belgelerine bakın [Breiman örnek](https://mahout.apache.org/users/classification/breiman-example.html) Apache Mahout Web sitesinde.

### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Hdınsight 3.1 Tez Hive sorgularını kullanabilirsiniz
Hive 0,13 Hdınsight 3.1 kullanılabilir ve önemli performans geliştirmeleri yararlanılabilir Tez kullanarak sorguları çalıştırabilen olur.
Tez Hive sorguları için varsayılan olarak etkin değildir. Kullanmak için de etmeniz gerekir. Aşağıdaki kod parçacığını çalıştırarak Tez etkinleştirebilirsiniz:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks ile Tez Hive sorgusu performans geliştirmeleri ayrıntılı bir dökümünü içinde standart kıyaslamaları teslim olarak yayımlanır. Ayrıntılar için bkz [Apache Hive 13 değerlendirmesi'Kurumsal Hadoop için](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Daha fazla bilgi için bkz: [üzerinde Tez Hive](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

### <a name="global-availability"></a>Genel kullanılabilirlik
Hdınsight'ta Hadoop 2.2 sürümle birlikte, Microsoft Hdınsight kullanılabilir Azure kullanılabilir olduğu tüm ana coğrafi bölgelerde yapmıştır. Özellikle, Batı Avrupa ve Güneydoğu Asya veri merkezleri çevrimiçi bir araya getirilmiştir. Bu, müşterilerin kümeleri yakın olan bir veri merkezinde ve büyük olasılıkla bir bölgede benzer uyumluluk gereksinimlerinin bulun olanak verir.

### <a name="dos--donts-between-cluster-versions"></a>DOS & Küme sürümleri arasında yapılmaması gerekenler
**Oozie meta deponuz Hdınsight 3.1 kümesi ile kullanılan Hdınsight 2.1 kümeleri ile geriye dönük olarak uyumlu değildir ve bu önceki sürümüyle kullanılamaz**.

Bir Hdınsight 3.1 kümesi ile dağıtılmış bir özel Oozie meta depo veritabanı bir Hdınsight 2.1 kümeyle yeniden kullanılamaz. Bir Hdınsight 2.1 kümeyle meta depo kaynaklanan olsa bile bu geçerlidir. Meta depo şeması artık Hdınsight 2.1 kümeleri tarafından gerekli meta depo ile uyumlu olacak şekilde bir Hdınsight 3.1 kümesi ile kullanıldığında yükseltilmiş çünkü bu senaryo desteklenmiyor. Bir Hdınsight 3.1 kümesi ile kullanılan bir Oozie meta depo yeniden denemesi işleme Hdınsight 2.1 küme yaramaz.

**Oozie meta deponuz kümeler arasında paylaşılamaz.**

Oozie meta deponuz belirli kümelerine bağlı olan ve kümeler arasında paylaşılamaz.

### <a name="breaking-changes"></a>Yeni değişiklikler
**Sözdizimi önek**: yalnızca "wasb: / /" söz dizimi Hdınsight 3.1 ve 3.0 kümeleri desteklenir. Eski "asv: / /" söz dizimi Hdınsight 2.1 ve 1.6 kümeleri desteklenir, ancak Hdınsight 3.1 veya 3.0 kümeleri desteklenmez. Bir Hdınsight 3.1 veya 3.0 için gönderilen bir işin kullanır, açıkça küme yani "asv: / /" sözdizimi başarısız olur. "Wasb: / /" söz dizimi yerine kullanılmalıdır. Ayrıca, tüm Hdınsight 3.1 veya kullanarak kaynaklarına açık başvuruları içeren mevcut bir meta depo ile oluşturulan 3.0 kümesine gönderilen işler "asv: / /" sözdizimi başarısız olur. Bu meta deponuz kullanarak yeniden oluşturulması gerekecek "wasb: / /" adresi kaynaklarına sözdizimi.

**Bağlantı noktaları**: Hdınsight hizmeti tarafından kullanılan bağlantı noktaları değiştirilmiştir. Kullanılmakta bağlantı noktası numaralarını Windows işletim sisteminin kısa ömürlü bağlantı noktası aralığı içinde yoktu. Bağlantı noktaları kısa süreli Internet Protokol tabanlı iletişimleri için önceden tanımlanmış kısa ömürlü bir aralıktan otomatik olarak ayrılır. Yeni izin verilen Hortonworks veri Platformu (HDP) hizmet bağlantı noktası numaralarını kümesidir karşılaşmadan baş düğüm üzerinde çalışan hizmetleri tarafından kullanılan bağlantı noktaları ile meydana gelebilecek çakışmaları önlemek için bu aralığın dışında. Yeni bağlantı noktası numaralarını en son değişiklikleri neden olmamalıdır. Kullanılan numaralar aşağıdaki gibidir:

 **Hdınsight 1.6 (HDP 1.1)**

<table border="1">
<tr><th>Ad</th><th>Değer</th></tr>
<tr><td>DFS.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.History.Server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table>

 **Hdınsight 3.1 ve 3.0 (HDP 2.1 ve 2. 0)**

<table border="1">
<tr><th>Ad</th><th>Değer</th></tr>
<tr><td>DFS.namenode.HTTP adresi</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.HTTPS adresi</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.HTTP adresi</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table>

### <a name="dependencies"></a>Bağımlılıklar
Aşağıdaki bağımlılıkları Hdınsight'ta eklenen 3.x (HDP2.x):

* guice servlet
* optiq çekirdek
* javax.inject
* Etkinleştirme
* jsr305
* geronimo-jaspic_1.0_spec
* Tem slf4j
* Java xmlbuilder
* ant
* Commons derleyici
* jdo API
* Commons math3
* paranamer
* jaxb impl
* stringtemplate
* eigenbase xom
* bölgesi servlet
* Commons Yönet
* jaxb API
* jetty tüm sunucu
* janino
* xercesImpl
* optiq avatica
* jta
* eigenbase özellikleri
* Tüm Modaya uygun
* hamcrest çekirdek
* Posta
* linq4j
* jpam
* İstemci bölgesi
* aopalliance
* geronimo-annotation_1.0_spec
* ant Başlatıcısı
* bölgesi guice
* XML API'leri
* stax API
* asm commons
* asm-ağacı
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni tüm
* hız
* jettison
* java snappy
* jetty tüm
* Commons dbcp

Aşağıdaki bağımlılıkları Hdınsight'ta artık mevcut 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* Sarmaşık
* aspectjrt
* JSON
* çekirdek
* jdo2 API
* avro mapred
* datanucleus Geliştirici
* jsp
* Commons günlük kaydı API
* Commons matematik
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* snappy

### <a name="version-changes"></a>Sürüm değişiklikleri
Aşağıdaki sürüm arasında Hdınsight değişikliklerinin yapıldığı 2.x (HDP1.x) ve Hdınsight 3.x (HDP2.x):

* Ölçümleri çekirdekli: ['2.1.2'] ['3.0.0'] ->
* derbynet: ['10.4.2.0'] ['10.10.1.1'] ->
* datanucleus: ['rdbms-3.0.8'] -> ['rdbms-3.2.9']
* jasper derleyici: ['5.5.12'] ['5.5.23'] ->
* log4j: ['1.2.15', '1.2.16'] -> ['1.2.16', '1.2.17']
* derbyclient: ['10.4.2.0'] ['10.10.1.1'] ->
* httpcore: ['4.2.4'] ['4.2.5'] ->
* hsqldb: ['1.8.0.10'] ['2.0.0'] ->
* jets3t: ['0.6.1'] ['0.9.0'dan'] ->
* protobuf java: ['2.4.1'] ['2.5.0'] ->
* Derby: ['10.4.2.0'] ['10.10.1.1'] ->
* jasper: [' çalışma zamanı-5.5.12'] -> [' çalışma zamanı-5.5.23']
* Commons arka plan programı: ['1.0.1'] ['1.0.13'] ->
* datanucleus çekirdekli: ['3.0.9'] ['3.2.10'] ->
* datanucleus API jdo: ['3.0.7'] ['3.2.6'] ->
* zookeeper: ['3.4.5.1.3.9.0-01320'] ['3.4.5.2.1.3.0-1948'] ->
* bonecp: ['0.7.1.RELEASE'] -> ['
* 0.8.0.RELEASE']

### <a name="drivers"></a>Sürücüler
SQL Server için Java veritabanı bağlantısı (JDBC) sürücüsü Hdınsight tarafından dahili olarak kullanılır ve dış işlemleri için kullanılmaz. Açık veritabanı bağlantısı (ODBC) kullanarak Hdınsight'a bağlanmak istiyorsanız, lütfen Microsoft Hive ODBC sürücüsü kullanın. Daha fazla bilgi için bkz: [Hdınsight Microsoft Hive ODBC sürücüsü ile bağlanma Excel'e](hdinsight-connect-excel-hive-odbc-driver.md).

### <a name="bug-fixes"></a>Hata düzeltmeleri
Bu sürümle birlikte, biz çeşitli hata düzeltmeleri aşağıdaki Hdınsight sürümleriyle yenileyene:

* Hdınsight 2.1 (HDP 1.3)
* Hdınsight 3.0 (HDP 2.0)
* Hdınsight 3.1 (HDP 2.1)

## <a name="hortonworks-release-notes"></a>Hortonworks sürüm notları
Hortonworks veri Hdınsight sürüm kümeleri tarafından kullanılan platformları (HDPs) için sürüm notları aşağıdaki konumlarda kullanılabilir:

* Hdınsight sürüm 3.1 kullanan temel alan bir Hadoop dağıtım [Hortonworks veri platformu 2.1.7][hdp-2-1-7]. 7/11/2014 sonra Azure portalını kullanarak oluşturulan varsayılan Hadoop kümesi budur. 11/7/2014 temel alarak önce oluşturulan Hdınsight 3.1 kümeleri [Hortonworks veri platformu 2.1.1][hdp-2-1-1]
* Hdınsight sürüm 3.0 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.0][hdp-2-0-8].
* Hdınsight sürüm 2.1 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.3][hdp-1-3-0].
* Hdınsight sürüm 1.6 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
