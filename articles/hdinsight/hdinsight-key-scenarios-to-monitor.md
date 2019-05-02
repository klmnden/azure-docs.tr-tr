---
title: Küme performansını izleme - Azure HDInsight
description: Bir HDInsight kümesi için kapasite ve performans izlemeyi öğrenin.
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: arindamc
ms.openlocfilehash: 22484885663a4f9a908ae988882b87612129251a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64693216"
---
# <a name="monitor-cluster-performance"></a>Küme performansını izleme

Bir HDInsight kümesinin performans ve sistem durumu izleme, en iyi performans ve kaynak kullanımını bakımı için gereklidir. İzleme algılamanıza ve bu küme yapılandırması hataları ve kullanıcı kod sorunlarını ele da yardımcı olabilir.

Aşağıdaki bölümlerde, izlemek ve kümelerinizi Apache Hadoop YARN kuyrukları üzerindeki yükü en iyi duruma getirmek ve depolama azaltma sorunları algılamak nasıl açıklanmaktadır.

## <a name="monitor-cluster-load"></a>İzleyici küme yük

Hadoop kümeleri en iyi performans sunabilirsiniz yük kümesindeki tüm düğümlere eşit olarak dağıtılır. Bu işleme görevlerinin RAM, CPU ve disk kaynakları tek tek düğümlere tarafından kısıtlanmasını olmadan çalışmasını sağlar.

Üst düzey göz kümenizi ve bunların yükleme düğümleri almak için oturum açın [Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md), ardından **konakları** sekmesi. Konaklarınızı, tam etki alanı adlarına göre listelenir. Her ana bilgisayarın işletim durumu renkli sistem durumu göstergesi tarafından gösterilir:

| Renk | Açıklama |
| --- | --- |
| Kırmızı | Ana bilgisayarda en az bir ana bileşeni kullanılamıyor. Etkilenen bileşenleri listeler ipucunu görmek için gelin. |
| Orange | En az bir bağımlı bileşen ana bilgisayarda çalışmıyor. Etkilenen bileşenleri listeler ipucunu görmek için gelin. |
| Sarı | Ambari sunucusunun bir sinyal 3 dakikadan fazla konaktan almadı. |
| Yeşil | Normal çalışıyor durum. |

Ortalama disk kullanımı ve yük ve her konak için çekirdek sayısına ve RAM miktarını gösteren sütunları da göreceksiniz.

![Konakları sekmesi](./media/hdinsight-key-scenarios-to-monitor/hosts-tab.png)

Ayrıntılı bilgi barındıran ve bunların ölçümler üzerinde çalışan bileşenler için konak adları birini seçin. Ölçümler, CPU kullanımı, yük, disk kullanımı, bellek kullanımı, ağ kullanımı ve işlemleri sayıda seçilebilir çizelgesinin gösterilir.

![ana bilgisayar ayrıntıları](./media/hdinsight-key-scenarios-to-monitor/host-details.png)

Bkz: [yönetme HDInsight kümeleri Apache Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md) uyarılar ayarlanması ve ölçümleri görüntüleme hakkında bilgi.

## <a name="yarn-queue-configuration"></a>YARN sıra yapılandırması

Hadoop dağıtılmış platformu üzerinde çalışan çeşitli hizmetleri vardır. YARN (başka bir Resource Negotiator henüz), bu hizmetleri düzenler ve her türlü yük küme eşit olarak dağıtılır emin olmak için küme kaynaklarını ayırır.

YARN, iki Daemon'ları iki sorumlulukları Jobtracker'a, kaynak yönetimi ve iş zamanlama/izleme böler: genel bir kaynak yöneticisi yanı sıra, uygulama başına ApplicationMaster (da).

Kaynak Yöneticisi bir *saf Zamanlayıcı*ve yalnızca tüm rakip uygulamalar arasında kullanılabilir kaynaklar istemlerde. Resource Manager tüm kaynakların her zaman içinde kullanımı, kapasitesini garanti eder, SLA'ları gibi çeşitli sabitleri için iyileştirme ve benzeri olmasını sağlar. ApplicationMaster kaynakları Kaynak Yöneticisi'nden belirleyici ve kapsayıcıları ve kaynak tüketimi izlemek ve yürütmek için NodeManager(s) ile çalışır.

Birden çok kiracının büyük bir küme paylaştığınızda, kümenin kaynak rekabetini yoktur. CapacityScheduler kaynak tarafından sıraya alma isteği'kurmak paylaşımı yönetmenize yardımcı olan takılabilir bir zamanlayıcı var. Ayrıca CapacityScheduler destekler *hiyerarşik kuyrukları* diğer uygulamaları kuyruklar ücretsiz kaynakları kullanmak için izin verilmeden önce bir kuruluşun alt kuyrukları arasında paylaşılan kaynaklar emin olmak için.

YARN bu kuyruklar için kaynak ayırmayı kurmamızı sağlayan ve tüm kullanılabilir kaynaklarınız atanmış olup olmadığını gösterir. Kuyruklarınızı hakkındaki bilgileri görüntülemek için Ambari Web kullanıcı Arabirimi için oturum açın ve ardından **YARN Kuyruk yöneticisi** üstteki menüden.

![YARN Kuyruk Yöneticisi](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager.png)

YARN Kuyruk Yöneticisi sayfası her birine atanan kapasite yüzdesini yanı sıra sola Kuyruklarınızı listesini gösterir.

![YARN Kuyruk yöneticisi Ayrıntıları sayfası](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Ambari panosundan Kuyruklarınızı daha ayrıntılı bilgi için seçin **YARN** sol taraftaki listeden hizmet. Altında **hızlı bağlantılar** açılır menüsünde, select **kaynak yöneticisi kullanıcı Arabirimi** , etkin düğüm altında.

![Kaynak Yöneticisi kullanıcı Arabirimi menü bağlantısı](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

Kaynak Yöneticisi Arabiriminde seçin **Zamanlayıcı** sol taraftaki menüden. Kuyruklarınızı altında listesini *uygulama sıraları*. Kaynak kısıtlı olup tüm işler ve her ne kadar iyi, bunlar arasında dağıtılmış işleri Kuyruklarınızı için kullanılan kapasite burada görebilirsiniz.

![Kaynak Yöneticisi kullanıcı Arabirimi menü bağlantısı](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui.png)

## <a name="storage-throttling"></a>Depolama alanı azaltma

Bir kümenin performans sorunu depolama düzeyinde oluşabilir. Bu tür bir performans sorunu nedeniyle en sık olan *engelleme* giriş/çıkış (GÇ) işlemi, depolama hizmeti işleyebileceğinden daha fazla g/ç çalışmakta olan görevlerin gönderdiğinizde hangi. Bu engelleme işlemiyle, geçerli bir IOs işlendikten sonra kadar işlenmeyi bekleyen g/ç isteklerinin bir kuyruk oluşturur. Şu nedenle taşlarıdır *depolama azaltma*, fiziksel bir sınır değil, ancak bunun yerine bir sınırı uygulanmaktadır depolama hizmeti tarafından bir hizmet düzeyi sözleşmesi (SLA). Bu sınır, hizmet, tek bir istemci ya da Kiracı tekeline sağlar. SLA'sı IOs sayısı (IOPS) için saniyede Azure depolama - Ayrıntılar için sınırlar, bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Azure depolama, depolama ile ilgili sorunları izleme hakkında bilgi kullanıyorsanız, dahil olmak üzere azaltma, bkz [izleme, tanılama ve sorun giderme Microsoft Azure depolama](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Azure Data Lake Storage (ADLS) kümenizin yedekleme deposu ise, azaltma nedeniyle bant genişliği sınırlarını kaynaklanıyor olabilir. Azaltma bu durumda görevi günlüklerde gözlemci azaltma hataları tarafından tanımlanabilir. ADLS için şu makalelere uygun hizmet için azaltma bölümüne bakın:

* [Apache Hive HDInsight ve Azure Data Lake Store için performans ayarlama Kılavuzu](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [HDInsight ve Azure Data Lake Storage MapReduce için performans ayarlama Kılavuzu](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [HDInsight ve Azure Data Lake Store üzerinde Apache Storm için performans ayarlama Kılavuzu](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="next-steps"></a>Sonraki adımlar

Kümeleri izleme ve sorun giderme hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [HDInsight günlüklerini çözümleme](hdinsight-debug-jobs.md)
* [Apache Hadoop YARN günlükleri ile uygulama hatalarını ayıklama](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Linux tabanlı HDInsight üzerinde Apache Hadoop Hizmetleri için yığın dökümlerini etkinleştirme](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
