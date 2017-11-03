---
title: "Küme performansı - Azure Hdınsight izleme | Microsoft Docs"
description: "Bir Hdınsight kümesi için kapasite ve performans izleme yapma."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: maxluk
ms.openlocfilehash: f2333202cdccc82edea649ff1c295752a414c8b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-cluster-performance"></a>Küme performansını izleme

Hdınsight kümesinin performans ve sistem durumu izleme, en yüksek performans ve kaynak kullanımı bakımı için gereklidir. İzleme de adresi olası kodlama veya küme yapılandırma hataları yardımcı olabilir.

Aşağıdaki bölümlerde yüklenirken küme en iyi duruma getirme, YARN sıra verimliliği ve depolama erişilebilirlik açıklanmaktadır.

## <a name="cluster-loading"></a>Küme yükleme

Hadoop kümeleri, tüm küme düğümlerine yüklenmesi dengelemeniz. Bu Dengeleme işleme görevlerini RAM, CPU ve disk kaynakları tarafından kısıtlı engeller.

Kümenizi ve bunların yükleme düğümlerinin en üst düzey bilgi almak için oturum [Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md)seçeneğini belirleyip **ana** sekmesi. Konaklarınızın, tam etki alanı adlarına göre listelenir. Her ana bilgisayarın işletim durumu renkli durum göstergesi tarafından gösterilir:

| Renk | Açıklama |
| --- | --- |
| Kırmızı | Ana bilgisayarda en az bir ana bileşeni kullanılamıyor. Etkilenen bileşenleri listeleri bir araç ipucu görmek için gelin. |
| Orange | En az bir bağımlı bileşen ana bilgisayarda çalışmıyor. Etkilenen bileşenleri listeleri bir araç ipucu görmek için gelin. |
| Sarı | Ambari sunucusu bir sinyal 3 dakikadan fazla ana bilgisayardan almadı. |
| Yeşil | Normal çalışıyor durum. |

Ayrıca her ana bilgisayar için çekirdek sayısı ve RAM miktarını gösteren sütunları görürsünüz ve disk kullanımı ve yük ortalaması.

![Konakları sekmesi](./media/hdinsight-key-scenarios-to-monitor/hosts-tab.png)

Bileşenlerini barındıran ve bunların ölçümleri çalıştıran ayrıntılı bir bakış için konak adları birini seçin. Ölçümleri CPU kullanımı, yük, disk kullanımı, bellek kullanımı, ağ kullanımını ve işlemler sayıda seçilebilir olarak zaman çizelgesi gösterilir.

![ana bilgisayar ayrıntıları](./media/hdinsight-key-scenarios-to-monitor/host-details.png)

Bkz: [Hdınsight kümelerini yönetme Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md) uyarıları ayarlama ve ölçümleri görüntüleme hakkında bilgi.

## <a name="yarn-queue-configuration"></a>YARN sıra yapılandırması

Hadoop, dağıtılmış bir platformda çalışan çeşitli hizmetler sahiptir. YARN (henüz başka bir kaynak Uzlaştırıcı) bu hizmetleri koordinatları, küme kaynakları ayırır ve ortak bir veri kümesi erişimini yönetir.

YARN iki Daemon Jobtracker'a, kaynak yönetimi ve iş zamanlama/monitoring iki sorumluluklarını böler: Genel ResourceManager ve uygulama başına ApplicationMaster (AM).

ResourceManager olan bir *saf Zamanlayıcı*ve yalnızca tüm rakip uygulamalar arasında kullanılabilir kaynakları istemlerde. ResourceManager tüm kaynakların her zaman kullanın, SLA'ları gibi çeşitli sabitleri için kapasite güvence altına alır, en iyi duruma getirme ve benzeri olmasını sağlar. ApplicationMaster ResourceManager kaynaklardan görüşür ve yürütün ve kapsayıcılar ve bunların kaynak tüketimini izlemek için NodeManager(s) ile çalışır.

Birden çok kiracıya büyük bir küme paylaştığınızda, kümenin kaynaklar için rekabet yoktur. CapacityScheduler istekleri yukarı çağrısı tarafından Paylaşımı kaynak yönetmenize yardımcı olan takılabilir bir Zamanlayıcı ' dir. CapacityScheduler de destekler *hiyerarşik sıraları* diğer uygulamaları kuyruklar kaynakları serbest bırakmak kullanmasına izin verilen önce bir kuruluş alt sıralar arasında paylaşılan kaynaklar sağlamak için.

YARN bu sıraların kaynakları tahsis olanak tanır ve kullanılabilir kaynaklarınızı atanmış olup olmadığını gösterir. Kuyruklar hakkındaki bilgileri görüntülemek için Ambari Web kullanıcı Arabirimi için oturum açın ve ardından **YARN sıra yöneticisi** üstteki menüden.

![YARN sıra Yöneticisi](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager.png)

YARN sıra Yöneticisi sayfası, solda, her birine atanan kapasitenin yüzdesi yanı sıra, kuyrukların listesini gösterir.

![YARN sıra yöneticisi Ayrıntıları sayfası](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Ambari panosundan, kuyruklar daha ayrıntılı bir bakış seçin **YARN** sol taraftaki listeden hizmet. Altında **hızlı bağlantılar** açılır menüsünde, select **ResourceManager UI** etkin düğüm altında.

![ResourceManager UI menü bağlantısı](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

ResourceManager Arabiriminde seçin **Zamanlayıcı** sol taraftaki menüden. Altında kuyruklar listesini görmek *uygulama sıraları*. İşlerini bunlar arasında ne kadar iyi dağıtılır, sıranın her biri için kullanılan kapasite burada görebilirsiniz ve olup tüm işleri kaynak kısıtlı.

![ResourceManager UI menü bağlantısı](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui.png)

## <a name="storage-throttling"></a>Depolama azaltma

Bir kümenin performans düşüklüğü depolama düzeyinde oluşabilir. Bu performans sorunu en sık nedeniyle türünde *engelleme* giriş/çıkış, çalışan görevleri depolama hizmeti işleyebileceğinden daha fazla GÇ gönderdiğinizde hangi (IO) işlemleri. Bu engelleme geçerli IOs işlendikten sonra kadar işlenmeyi bekleyen g/ç istek kuyruğu oluşturur. Taşlarıdır nedeniyle *depolama azaltma*, fiziksel bir sınır olmayan, ancak bunun yerine bir sınır uygulanmaz depolama hizmeti tarafından bir hizmet düzeyi sözleşmesi (SLA). Bu sınır, tek bir istemci ya da Kiracı hizmet kullanmasını olduğunu sağlar. SLA için saniye başına (IOPS) Azure Storage - Ayrıntılar için IOs sayısını sınırlayan bkz [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Depolama ile ilgili sorunları izleme hakkında bilgi için Azure Storage kullanıyorsanız, da dahil olmak üzere azaltma, bkz: [izleme, tanılama ve Microsoft Azure Storage sorun giderme](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Kümenizin yedekleme deposu Azure Data Lake depolamak (ADLS), azaltma bant genişliği sınırlarını nedeni büyük olasılıkla ise. Azaltma bu durumda görev günlükleri gözlemci azaltma hatalar nedeniyle tanımlanamadı. ADLS için bu makaleler uygun hizmet için azaltma bölümüne bakın:

* [Azure Data Lake Store ve HDInsight’ta Hive için performans ayarlama kılavuzu](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [Azure Data Lake Store ve HDInsight’ta MapReduce için performans ayarlama kılavuzu](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [Azure Data Lake Store ve HDInsight’ta Storm için performans ayarlama kılavuzu](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme ve kümelerinizi izleme hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [HDInsight günlüklerini çözümleme](hdinsight-debug-jobs.md)
* [YARN günlükleri ile uygulama hatalarını ayıklama](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Linux tabanlı hdınsight'ta Hadoop Hizmetleri için yığın dökümleri etkinleştir](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
