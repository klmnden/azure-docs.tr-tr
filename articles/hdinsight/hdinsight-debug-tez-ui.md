---
title: Tez kullanıcı Arabirimi, Windows tabanlı HDInsight ile - Azure kullanın
description: Tez kullanıcı Arabirimi, Windows tabanlı HDInsight HDInsight üzerinde Tez işlerinin hatalarını ayıklamak için kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: f54cc60f9490b8a5ca1872a290c3895ea8b0c5e4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590910"
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a>Tez kullanıcı Arabirimi, Windows tabanlı HDInsight üzerinde Tez işlerinin hatalarını ayıklamak için kullanın
Tez kullanıcı Arabirimi, yürütme altyapısı Tez kullanan Hive işlerinin hatalarını ayıklamak için kullanılabilir. Bir grafik bağlı öğelerin her öğenin ayrıntısına ve istatistikleri ve günlük kaydı bilgilerini alma Tez kullanıcı Arabirimi iş görselleştirir.

> [!IMPORTANT]
> Bu belgedeki adımlarda Windows kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Önkoşullar
* Bir Windows tabanlı HDInsight kümesi. Yeni küme oluşturma adımları için bkz [Windows tabanlı HDInsight kullanmaya başlama](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > Tez kullanıcı Arabirimi, yalnızca 8 Şubat 2016'dan sonra oluşturulan Windows tabanlı HDInsight kümelerinde kullanılabilir.
  >
  >
* Bir Windows tabanlı uzak masaüstü istemcisi.

## <a name="understanding-tez"></a>Tez anlama
Tez Hadoop veri işlemeye yönelik genişletilebilir bir çerçevesidir ve geleneksel MapReduce işleme büyük işleyebileceğiniz sağlar. Tez Hive sorgusu bir parçası olarak aşağıdaki metin dahil olmak üzere etkinleştirebilirsiniz:

    set hive.execution.engine=tez;

Tez yönlendirilmiş Çevrimsiz graf (yürütme iş için gereken eylemlerin sırasını açıklayan DAG) oluşturur. Bireysel eylemleri köşeler verilir ve genel projenin parçası yürütün. Gerçek yürütmesi, bir köşe tarafından açıklanan iş bir görev çağrılır ve kümedeki birden fazla düğümde dağıtılmış.

### <a name="understanding-the-tez-ui"></a>Tez kullanıcı Arabirimi anlama
Tez kullanıcı Arabirimi, bir web sayfası Tez kullanan işlemler hakkında bilgi sağlar. ' dir. Bunu, aşağıdaki senaryolarda yararlı bilgiler teklif edebilir:

* Uzun süre çalışan izleme eşleme ilerleme durumunu görüntüleme, işler ve görevler azaltır.
* İşleme nasıl geliştirilmiş veya başarısız olma nedenine öğrenmek başarılı veya başarısız işlemler için geçmiş verilerini analiz etme.

## <a name="generate-a-dag"></a>Bir DAG oluştur
Tez kullanıcı Arabirimi, geçmişte Tez Altyapısı şu anda çalışıyor veya devre dışı bırakıldı kullanan çalışan bir iş, verileri içerir. Basit bir Hive sorguları genellikle Tez kullanmadan çözümlenebilir. Filtreleme yapmak daha karmaşık sorgular, Tez gruplandırma, sıralama, birleştirme, vb. gerektirir.

Tez kullanan Hive sorgusu çalıştırmak için aşağıdaki adımları kullanın.

1. Bir web tarayıcısında gidin https://CLUSTERNAME.azurehdinsight.netburada **CLUSTERNAME** HDInsight kümenizin adıdır.
2. Sayfanın üst kısmındaki menüden **Hive Düzenleyicisi**. Bu, aşağıdaki örnek sorgu içeren bir sayfa görüntüler.

        Select * from hivesampletable

    Örnek sorgu silebilir ve aşağıdakiyle değiştirin.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Seçin **Gönder** düğmesi. **Proje oturumunu** sayfanın alt kısmındaki bölümde sorgu durumunu görüntüler. Durumu değiştikten sonra **tamamlandı**seçin **ayrıntıları** sonuçlarını görüntülemek için bağlantı. **İş çıktısı** aşağıdakine benzer olmalıdır:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a>Tez kullanıcı arabirimi
> [!NOTE]
> Tez kullanıcı Arabirimi yalnızca baş düğümlerine bağlanmak için Uzak Masaüstü kullanmalısınız, ve kümenin baş düğümlerinden masaüstünden kullanılabilir.
>
>

1. Gelen [Azure portalında](https://portal.azure.com), HDInsight kümenizi seçin. HDInsight dikey pencerenin üst **Uzak Masaüstü** simgesi. Bu bağlantı Uzak Masaüstü dikey penceresinde görüntüler.

    ![Uzak Masaüstü simgesi](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Uzak Masaüstü dikey penceresinden seçin **Connect** küme baş düğümüne bağlanmak için. İstendiğinde, bağlantı kimliğini doğrulamak için küme Uzak Masaüstü kullanıcı adı ve parola kullanın.

    ![Uzak Masaüstü bağlanmak simge](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Uzak Masaüstü Bağlantısı etkinleştirmediyseniz, bir kullanıcı adı, parola ve sona erme tarihi sağlayın, sonra seçin **etkinleştirme** Uzak Masaüstü'nü etkinleştirmek için. Etkinleştirildikten sonra bağlanmak için önceki adımları kullanın.
   >
   >
3. Bağlantı kurulduktan sonra uzak masaüstünde Internet Explorer'ı açın, tarayıcının sağ üst kısımdaki dişli simgesini seçin ve ardından **Uyumluluk Görünümü Ayarları**.
4. Aşağıdan **Uyumluluk Görünümü Ayarları**, onay kutusunu temizleyin **Intranet sitelerini Uyumluluk Görünümü'nde görüntülemek** ve **kullanımı Microsoft uyumluluk listesi**, ve ardından **Kapat**.
5. Internet Explorer'da Gözat http://headnodehost:8188/tezui/#/. Bu, Tez kullanıcı Arabirimi görüntüler

    ![Tez kullanıcı Arabirimi](./media/hdinsight-debug-tez-ui/tezui.png)

    Tez kullanıcı Arabirimi yüklediğinde, çalışmakta olan veya silinmiş Dag'leri listesini küme üzerinde çalışan bakın. Varsayılan görünüm, DAG adı, kimliği, gönderen, durumu, başlangıç zamanı, bitiş zamanı, süre, uygulama kimliği ve kuyruk içerir. Daha fazla sütun, sayfanın sağ taraftaki dişli simgesini kullanarak eklenebilir.

    Yalnızca bir giriş varsa, önceki bölümde çalıştırdığınız sorguyu içindir. Birden çok girişi varsa, arama alanlarını Dag'leri yukarıda arama ölçütü girerek, ardından isabet **Enter**.
6. Seçin **Dag adı** en son DAG girişi. Bu bağlantı seçeneği bir zip DAG hakkında bilgiler içeren JSON dosyalarını indirmek için yanı sıra DAG hakkında bilgi görüntüler.

    ![DAG ayrıntıları](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Yukarıda **DAG ayrıntıları** DAG hakkındaki bilgileri görüntülemek için kullanılan birkaç bağlantı mevcuttur.

   * **DAG sayaçları** bu DAG sayaçları bilgilerini görüntüler.
   * **Grafik görünümü** bu DAG grafik gösterimi görüntülenir.
   * **Tüm köşeleri** bu DAG köşeleri listesini görüntüler.
   * **Tüm Görevler** bu DAG tüm köşeler için görevleri listesini görüntüler.
   * **Tüm TaskAttempts** görevleri çalıştırmak için bu DAG girişimleri hakkındaki bilgileri görüntüler.

     > [!NOTE]
     > Sütun görünen köşeler, görevleri ve TaskAttempts kaydırırsanız görüntülemek için bağlantıları olduğuna dikkat edin **sayaçları** ve **görüntüleme veya indirme günlükleri** her satır için.
     >
     >

     İş ile ilgili hata varsa, DAG ayrıntıları başarısız oldu, başarısız görev hakkındaki bilgiler için bağlantılarla birlikte durumunu görüntüler. Tanılama bilgilerini olması görüntülenir DAG ayrıntıları altında.
8. Seçin **grafik görünümü**. Bu, DAG grafiksel bir temsilini görüntüler. Her köşe ilgili bilgileri görüntülemek için görünümde üzerine fare yerleştirebilirsiniz.

    ![Grafik görünümü](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Bir köşe tıklayarak yükler **köşe ayrıntılarını** o öğe için. Tıklayarak **harita 1** bu öğenin ayrıntılarını görüntülemek için köşe. Seçin **Onayla** Gezinti onaylamak için.

    ![Köşe ayrıntılarını](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Artık köşeler ve görevlerle ilgili bağlantılar sayfasının üstündeki gerektiğini unutmayın.

    > [!NOTE]
    > Bu sayfaya geri giderek de ulaşması **DAG ayrıntıları**u seçerek **köşe ayrıntılarını**seçip **harita 1** köşe.
    >
    >

    * **Köşe sayaçları** bu köşe sayacı bilgilerini görüntüler.
    * **Görevleri** bu köşe için görevleri görüntüler.
    * **Görev denemesi** bu köşe için görevleri çalıştırma girişimlerini hakkındaki bilgileri görüntüler.
    * **Kaynakları ve havuzlarını** bu köşe için iç havuzları ve veri kaynaklarını görüntüler.

      > [!NOTE]
      > Olarak önceki menü, görevler, görev denemesi ve kaynakları ve Sinks__ her öğe için daha fazla bilgi için bağlantılar görüntülenecek sütun görünümünü kaydırabilir.
      >
      >
11. Seçin **görevleri**ve ardından adlı bir öğe seçin **00_000000**. Bu bağlantı görüntüler **görev ayrıntıları** bu görev için. Bu ekrandan görüntüleyebilirsiniz **görev sayaçları** ve **görev denemesi**.

    ![Görev ayrıntıları](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Sonraki Adımlar
Tez görünümü kullanmak öğrendiniz, daha fazla bilgi edinin [HDInsight üzerinde Hive'ı kullanarak](hadoop/hdinsight-use-hive.md).

Daha ayrıntılı Tez üzerinde teknik bilgi için bkz. [Hortonworks Tez sayfanın](http://hortonworks.com/hadoop/tez/).
