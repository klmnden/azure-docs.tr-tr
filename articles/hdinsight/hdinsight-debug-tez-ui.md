---
title: "Tez UI Windows tabanlı Hdınsight ile - Azure kullanma | Microsoft Docs"
description: "Windows tabanlı Hdınsight hdınsight'ta Tez işlerinde hata ayıklamak için Tez UI kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 3889fa1c3523eb0330cbe3b7640fd8590a5ceadf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a>Windows tabanlı Hdınsight üzerinde Tez işlerinde hata ayıklamak için Tez kullanıcı arabirimini kullanma
Tez UI anlamak ve Tez yürütme altyapısı Windows tabanlı Hdınsight kümelerinde olarak kullanan işleri hata ayıklamak için kullanılan bir web sayfasıdır. Tez kullanıcı Arabirimi iş bağlı öğelerinin bir grafik olarak görselleştirme, her öğenin ayrıntısına ve istatistikler ve günlük bilgileri almasını sağlar.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Windows kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Ön koşullar
* Bir Windows tabanlı Hdınsight kümesi. Yeni küme oluşturma adımları için bkz: [Windows tabanlı Hdınsight kullanmaya başlama](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > Tez UI yalnızca 8 Şubat 2016 sonrasında oluşturulan Windows tabanlı Hdınsight kümeleri üzerinde kullanılabilir.
  >
  >
* Bir Windows tabanlı uzak masaüstü istemcisi.

## <a name="understanding-tez"></a>Tez anlama
Tez geleneksel MapReduce işleme büyük hızlarından sağlayan hadoop'ta veri işleme için genişletilebilir bir çerçevedir. Windows tabanlı Hdınsight kümeleri için Hive sorgunuzu bir parçası olarak aşağıdaki komutu kullanarak Hive için etkinleştirebilirsiniz bir isteğe bağlı altyapısıdır:

    set hive.execution.engine=tez;

İş için Tez gönderildiğinde, yönlendirilmiş Çevrimsiz grafik (yürütme iş tarafından gereken eylemlerin sırasını açıklayan DAG) oluşturur. Tek tek Eylemler köşeleri olarak adlandırılır ve genel iş parçası yürütün. Gerçek yürütme köşe tarafından açıklanan iş bir görev çağrılır ve kümedeki birden çok düğüm arasında dağıtılmış.

### <a name="understanding-the-tez-ui"></a>Tez UI anlama
Tez kullanıcı Arabirimi, bir web sayfası, çalıştırılan veya olan işlemler hakkında bilgi, daha önce Tez kullanılarak çalıştırıldı sağlar ' dir. Tez tarafından oluşturulan DAG görüntülemenize izin verir kümeler arasında nasıl dağıtıldığını sayaçları görevleri ve köşeleri ve hata bilgilerini tarafından kullanılan bellek gibi. Aşağıdaki senaryolarda yararlı bilgiler teklif edebilir:

* Uzun süre çalışan izleme harita ilerlemesini görüntüleme, işler ve görevler azaltır.
* İşleme nasıl geliştirilmiş veya neden başarısız öğrenmek başarılı veya başarısız işlemler için geçmiş verileri analiz etme.

## <a name="generate-a-dag"></a>Bir DAG oluştur
Tez UI geçmişte Tez Altyapısı şu anda çalışıyor ya da bırakıldı kullanan çalışan bir iş, yalnızca veri içermez. Basit Hive sorguları genellikle Tez, yapan filtreleme, gruplama, sıralama, birleşimler, vb. Tez genellikle gerektirir ancak daha karmaşık sorgular kullanmadan çözülebilir.

Tez kullanma yürütecek bir Hive sorgusu çalıştırmak için aşağıdaki adımları kullanın.

1. Bir web tarayıcısında https://CLUSTERNAME.azurehdinsight.net için gidin nerede **CLUSTERNAME** Hdınsight kümenizin adıdır.
2. Sayfanın üstündeki menüsünden seçin **Hive Düzenleyicisi**. Bu, aşağıdaki örnek sorgu içeren bir sayfa görüntülenir.

        Select * from hivesampletable

    Örnek sorgu silebilir ve aşağıdakiyle değiştirin.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Seçin **gönderme** düğmesi. **Proje oturumunu** sayfanın alt kısmına sorgu durumunu görüntüler. Durum değişikliklerini sonra **tamamlandı**seçin **ayrıntıları görüntüle** sonuçları görüntülemek için bağlantı. **İş çıktısı** aşağıdakine benzer olmalıdır:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a>Tez kullanıcı arabirimini kullanma
> [!NOTE]
> Baş düğümler bağlanmak için Uzak Masaüstü'nü kullanmanız gerekir böylece Tez UI yalnızca küme baş düğümler masaüstünden kullanılabilir.
>
>

1. Gelen [Azure portal](https://portal.azure.com), Hdınsight kümenize seçin. Hdınsight dikey üstten seçin **Uzak Masaüstü** simgesi. Bu Uzak Masaüstü dikey penceresinde görüntülenir

    ![Uzak Masaüstü simgesi](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Uzak Masaüstü dikey penceresinden seçin **Bağlan** küme baş düğümüne bağlanmak için. İstendiğinde, bağlantı kimliğini doğrulamak için küme Uzak Masaüstü kullanıcı adı ve parola kullanın.

    ![Uzak Masaüstü Bağlantısı simgesi](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Uzak Masaüstü bağlantısı etkin değil, bir kullanıcı adı, parola ve sona erme tarihi sağlayın, sonra seçin **etkinleştirmek** Uzak Masaüstü'nü etkinleştirmek için. Etkinleştirildikten sonra bağlanmak için önceki adımları kullanın.
   >
   >
3. Bağlantı kurulduktan sonra Internet Explorer'ı Uzak Masaüstü'nü açın, sağ üst tarafındaki tarayıcı içinde dişli simgesini seçin ve ardından **Uyumluluk Görünümü Ayarları**.
4. Aşağıdan **Uyumluluk Görünümü Ayarları**, onay kutusunu temizleyin **görüntüleme intranet sitelerini Uyumluluk Görünümü'nde** ve **kullanım Microsoft Uyumluluk listeleri**ve ardından **Kapat**.
5. Internet Explorer'da tezui/http://headnodehost:8188 / # Gözat /. Bu Tez kullanıcı arabirimini görüntüler

    ![Tez kullanıcı Arabirimi](./media/hdinsight-debug-tez-ui/tezui.png)

    Tez UI yüklediğinde, çalışmakta olan ya da silinmiş Dag'leri listesini küme üzerinde çalışan görürsünüz. Varsayılan görünüm Dag adı, kimliği, gönderen, durumu, başlangıç saati, bitiş zamanı, süresi, uygulama kimliği ve kuyruk içerir. Daha fazla sütun, sayfanın sağ taraftaki dişli simgesini kullanarak eklenebilir.

    Yalnızca bir giriş varsa, önceki bölümde çalıştırdığınız bir sorgu için olacaktır. Birden çok girdi varsa, Dag'leri yukarıda alanlarında arama ölçütü girerek arayın, ardından isabet **Enter**.
6. Seçin **Dag adı** en son DAG girişi. Bu, DAG hakkında bilgi içeren JSON dosyaları zip yükleme seçeneği yanı sıra DAG hakkında bilgi görüntüler.

    ![DAG ayrıntıları](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Yukarıdaki **DAG ayrıntıları** DAG hakkındaki bilgileri görüntülemek için kullanılan birkaç bağlantılardır.

   * **DAG sayaçları** bu DAG sayaçları bilgilerini görüntüler.
   * **Grafik görünümü** bu DAG grafik gösterimi görüntüler.
   * **Tüm köşeleri** bu DAG köşeleri listesini görüntüler.
   * **Tüm Görevler** bu DAG tüm köşeleri görevlerde listesini görüntüler.
   * **Tüm TaskAttempts** görevleri çalıştırmak için bu DAG girişimleri hakkındaki bilgileri görüntüler.

     > [!NOTE]
     > Sütun görüntü köşeleri, görevleri ve TaskAttempts kaydırırsanız, görüntülemek için bağlantıları olduğuna dikkat edin **sayaçları** ve **görüntülemek veya günlükleri indirmek** her satır için.
     >
     >

     İşi ile hatası varsa, DAG ayrıntıları durumunu başarısız oldu, başarısız görev hakkında bilgi için bağlantılar ile birlikte görüntüler. Tanılama bilgileri DAG Ayrıntılar altında görüntülenir.
8. Seçin **grafik görünümü**. Bu grafik gösterimi DAG görüntüler. Her köşe ilgili bilgileri görüntülemek için görünümünde üzerinden fare yerleştirebilirsiniz.

    ![Grafik görünümü](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Bir köşe tıklatarak yükler **köşe ayrıntıları** bu öğe için. Tıklayın **harita 1** köşe bu öğenin ayrıntılarını görüntülemek için. Seçin **Onayla** Gezinti onaylamak için.

    ![Köşe ayrıntıları](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Şimdi köşeleri ve görevlerle ilgili bağlantıları sayfanın üst kısmındaki gerektiğini unutmayın.

    > [!NOTE]
    > Bu sayfaya geri giderek de ulaşması **DAG ayrıntıları**, seçme **köşe ayrıntıları**ve ardından seçerek **harita 1** köşe.
    >
    >

    * **Köşe sayaçları** bu köşe sayaç bilgilerini görüntüler.
    * **Görevleri** bu köşe için görevleri görüntüler.
    * **Görev denemeleri** bu köşe için görevleri çalıştırma denemelerini hakkındaki bilgileri görüntüler.
    * **Kaynakları & iç havuzlar** bu köşe için iç havuzlar ve veri kaynakları görüntüler.

      > [!NOTE]
      > Olarak önceki menüsüyle, görevler, görev denemeleri ve kaynakları & Sinks__ her öğe için daha fazla bilgi için bağlantılar görüntülenecek sütun görüntü gezinebilirsiniz.
      >
      >
11. Seçin **görevleri**ve ardından adlı bir öğe seçin **00_000000**. Bu görüntüler **görev ayrıntıları** bu görev için. Bu ekranda görüntüleyebileceğiniz **görev sayaçları** ve **görev denemeleri**.

    ![Görev Ayrıntıları](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Sonraki Adımlar
Tez görünümü kullanmak öğrendiniz, daha fazla bilgi edinmek [hdınsight'ta Hive kullanarak](hdinsight-use-hive.md).

Daha ayrıntılı Tez teknik bilgi için bkz: [Hortonworks Tez sayfanın](http://hortonworks.com/hadoop/tez/).
