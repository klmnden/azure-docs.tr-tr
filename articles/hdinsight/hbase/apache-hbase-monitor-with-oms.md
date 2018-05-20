---
title: İzleme ile Azure günlük analizi - Azure Hdınsight HBase | Microsoft Docs
description: Hdınsight HBase kümelerini izlemek üzere Azure günlük analizi kullanın.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 12ec60049cdf267834d251c6c927b35e3c363a4e
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="monitor-hbase-with-log-analytics"></a>Günlük analizi ile İzleyici HBase

Hdınsight HBase izleme Azure günlük analizi, Hdınsight küme düğümlerinden Hdınsight HBase performans ölçümlerini derleme için kullanır. İzleyici HBase özgü görselleştirmeleri ve panolar, ölçümleri ve özel izleme kurallarını ve Uyarıları oluşturma olanağı aramak için araçlar sağlar. Birden çok Hdınsight HBase kümeleri için ölçümler arasında birden çok Azure aboneliği izleyebilirsiniz.

Günlük analizi olan bir hizmet olarak [Azure](../../operations-management-suite/operations-management-suite-overview.md) bulut izler ve şirket içi ortamları kendi kullanılabilirliğini ve performansını korumak için. Günlük analizi kaynaklarını Bulut ve şirket içi ortamlarında ve analiz arasında birden çok kaynak sağlamak için diğer izleme araçları tarafından oluşturulan verileri toplar.

[Günlük analizi yönetim çözümleri](../../log-analytics/log-analytics-add-solutions.md) ek veri ve çözümleme araçları sağlayan günlük analizi için işlevselliği ekleyin. Günlük analizi yönetim çözümleri için belirli bir alandaki ölçümleri sağlayan mantığı, Görselleştirme ve veri alım kuralları koleksiyonudur. Bir çözüm toplanacak yeni kayıt türleri de tanımlayabilir ve bu kayıtları günlük aramaları veya yeni kullanıcı arabirimi özellikleri ile çözümlenebilir.

[Insight & Analytics](https://azure.microsoft.com/pricing/details/insight-analytics/) günlük analizi platformda inşa edilmiştir. Kullanmayı tercih günlük analizi yetenekleri ve hizmete alınan GB başına ödeme veya çalışma alanınızı Insight & Analytics katmana geçmek ve hizmet tarafından yönetilen düğüm başına ücret ödersiniz. Insight & Analytics, bir üst günlük analizi tarafından sunulan yetenekleri sunar. HBase izleme çözüm ya da günlük analizi veya Insight & Analizi ile kullanılabilir.

Bir Hdınsight HBase izleme çözümü sağlamak için günlük analizi çalışma alanı oluşturun. Her çalışma alanı, kendi veri deposu, veri kaynakları ve çözümlerle benzersiz bir günlük analizi ortamı olarak. Aboneliğinizde test ve üretim gibi birden çok ortamı desteklemek için birden çok çalışma alanı oluşturabilirsiniz.

## <a name="provision-hdinsight-hbase-monitoring"></a>Hazırlama Hdınsight HBase izleme

1. Oturum [Azure portal](https://portal.azure.com) Azure aboneliğinizi kullanarak.
2. İçinde **yeni** bölmesi altında **Market**seçin **izleme + Yönetim**.
3. İçinde **izleme + Yönetim** bölmesinde, **tümünü görmek**.

    ![İzleme + yönetim bölmesi](./media/apache-hbase-monitor-with-oms/monitoring-management-blade.png)  

4. Listesindeki arayın **yönetim çözümleri** bant. Sağındaki **yönetim çözümleri**seçin **daha fazla**.

    ![İzleme + yönetim bölmesi](./media/apache-hbase-monitor-with-oms/management-solutions.png) 

5. İçinde **yönetim çözümleri** bölmesinde, bir çalışma alanına eklemek için Hdınsight HBase izleme yönetim çözümü seçin.

    ![Yönetim çözümleri bölmesi](./media/apache-hbase-monitor-with-oms/hbase-solution.png)  
6. Yönetim çözümü bölmesinde, yönetim çözümü hakkındaki bilgileri gözden geçirin ve ardından **oluşturma**. 
7. İçinde *yönetim çözümü adı* bölmesinde yönetim çözümle ilişkilendirin veya yeni bir günlük analizi çalışma alanı oluşturmak için varolan bir çalışma alanını seçin ve ardından seçin.
8. Azure abonelik, kaynak grubunu ve konumu uygun şekilde çalışma ayarlarını değiştirin. 
    ![çözüm çalışma alanı](./media/apache-hbase-monitor-with-oms/solution-workspace.png)  
9. **Oluştur**’u seçin.  
10. Bu yeni yönetim çözümü kendi çalışma alanında kullanmak için gidin **günlük analizi** > ***çalışma alanı adı*** > **çözümleri**. Yönetimi çözümünüz için bir giriş listede görüntülenir. Çözüme gitmek için bir girişi seçin.

    ![Günlük analizi çözümleri](./media/apache-hbase-monitor-with-oms/log-analytics-solutions.png)  

11. Hdınsight HBase izleme çözümünüz için bölmesinde görünür.

    ![Hdınsight HBase çözümleri bölmesi](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

12. Günlük analizi veri göndermek için Hdınsight HBase kümesi henüz yapılandırmadınız çünkü Özet döşeme herhangi bir veri gösterme.

## <a name="connect-hdinsight-hbase-cluster-to-log-analytics"></a>Hdınsight HBase kümesi günlük Analizi'ne bağlayın

Hdınsight HBase izleme tarafından sağlanan araçları kullanmak için böylece kendi bölge sunucu, baş düğümler ve ZooKeeper düğümleri ölçümleri günlük analizi için iletir kümenizi yapılandırmanız gerekir. Bu yapılandırma, Hdınsight HBase kümesi karşı bir betik eylemi çalıştırılarak yapılır.

### <a name="get-log-analytics-workspace-id-and-workspace-key"></a>Günlük analizi çalışma alanı kimliği ve çalışma alanı anahtarı edinin

Günlük analizi çalışma alanı kimliği ve çalışma alanı anahtarı kümenizdeki düğümlerin etkinleştirmek için günlük analizi ile kimlik doğrulaması gerekir. Bu değerleri almak için:

1. Genel bakış, HBase izleme bölmesinde Azure portalında seçin.

    ![HBase izleme çözümü bölmesi](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

2. Yeni bir tarayıcı sekmesinde veya penceresinde OMS Portalı'nı başlatmak için OMS portalı seçin.

    ![OMS portalı seçin](./media/apache-hbase-monitor-with-oms/select-oms-portal.png) 

3. OMS portalı giriş ayarlarını seçin.

    ![OMS Portalı](./media/apache-hbase-monitor-with-oms/oms-portal-settings.png) 

4. Kaynak, Linux sunucuları Seç bağlı.

    ![Bağlı kaynak ardından Linux sunucuları seçin](./media/apache-hbase-monitor-with-oms/select-linux-servers.png) 

5. Bu betik eylemi için gereken değerleri olduğu gibi görüntülenir, çalışma alanı kimliği ve birincil anahtar değerlerine dikkat edin.

### <a name="run-the-script-action"></a>Betik eylemi Çalıştır

Hdınsight HBase kümeden veri toplamayı etkinleştirmek için kümedeki tüm düğümlere karşı bir betik eylemi çalıştırın:

1. Azure portalında, Hdınsight HBase kümesi için bölmesine gidin.
2. Seçin **betik eylemleri**.

    ![Betik eylemleri](./media/apache-hbase-monitor-with-oms/script-actions.png) 

3. Seçin **gönderme yeni**.

    ![Yeni gönderme](./media/apache-hbase-monitor-with-oms/script-actions-submit-new.png)  

4. İçinde **komut gönderme** eylem komut dosyası türü ayarlamak `"- Custom"`.
5. Bu komut dosyası için bir ad sağlayın.
6. İçin **betik URI Bash**, aşağıdaki URI'de yapıştırın:

        https://raw.githubusercontent.com/hdinsight/HDInsightOMS/master/monitoring/script2.sh 

7. İçin **düğüm türleri**, tüm üç seçin (**Head**, **bölge**, **ZooKeeper**).
8. İçinde **parametreleri** metin kutusu, çalışma alanı Kimliğiniz ve çalışma alanı anahtarı girin, her kapsayan değeri tırnak işaretleri içine ve boşlukla ayrılmış.

        "<Workspace ID>" "<Workspace Key>"

9. Seçin **bu betik eylemini Sürdür** eylem kümeye yeni düğümler eklendiğinde yeniden için.

    ![Betik eylem ayarları](./media/apache-hbase-monitor-with-oms/submit-script-action.png)  

10. **Oluştur**’u seçin.
11. Betik eylemi çalıştırmak için birkaç dakika sürer. Betik eylemleri bölmesinde durumunu izleyebilirsiniz.

    ![Çalışan betik eylemi](./media/apache-hbase-monitor-with-oms/script-action-running.png)  

12. Betik eylemi tamamladığında, listenin betik adın yanında yeşil bir onay işareti görmeniz gerekir.

    ![Betik eylem tamamlandı](./media/apache-hbase-monitor-with-oms/script-action-done.png)  

## <a name="view-the-hdinsight-hbase-monitoring-solution"></a>Hdınsight HBase izlemesi çözümü görüntüleyin

Betik eylemi tamamlandıktan sonra birkaç dakika içinde veri izleme çözümüne görmeniz gerekir.

1. Azure portalını Hdınsight HBase çözümünüzün bölmesine gidin.
2. Seçin **genel bakış** sekmesi.
3. Altında **Özet**, izlenmekte olan bölge sunucu sayısını belirten bir kutucuk görürsünüz.

    ![İzleme Özet kutucuğu](./media/apache-hbase-monitor-with-oms/monitoring-summary-tile.png)  

4. Bölge sunucu sayısını içeren kutucuğa seçin. Ana Panodaki görüntülenir.
5. Bu panoyu bölgeler, kullanımdaki yazma tamamlanan günlük (NİLEME) sayısı, günlük analizi aramaları koleksiyonu hakkındaki istatistiklerdir erişim sağlar (gibi bölge sunucu günlükleri, Phoenix günlükleri ve özel durumları) ve ilgili görselleştirmek için popüler grafikleri koleksiyonu Ölçümleri. 

    ![Ana Pano](./media/apache-hbase-monitor-with-oms/main-dashboard.png)  

6. Bunlardan herhangi birini seçerek burada sorguyu daraltmak ve daha ayrıntılı verileri araştırmak günlük arama görünümü detaya.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlük analizi uyarıları oluşturma](../../log-analytics/log-analytics-alerts-creating.md)
* [Azure günlük analizi günlük aramaları verilerle Bul](../../log-analytics/log-analytics-log-searches.md).
