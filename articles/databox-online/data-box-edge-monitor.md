---
title: Azure veri kutusu Edge Cihazınızı izleyin | Microsoft Docs
description: Azure veri kutusu Ucunuzdaki izlemek için yerel web kullanıcı Arabirimi ve Azure portalını kullanmayı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 4d4cc8a3eaf2bf806d5b265be226482b23804f82
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59580400"
---
# <a name="monitor-your-azure-data-box-edge"></a>Azure veri kutusu Ucunuzdaki izleme

Bu makalede, Azure veri kutusu Edge izlemeyi öğrenin. Cihazınızı izlemek için Azure portalı veya yerel web kullanıcı Arabirimi kullanabilirsiniz. Cihaz olaylarını görüntülemek, yapılandırmak ve Uyarıları yönetme ve ölçümleri görüntülemek için Azure portalını kullanın. Yerel web kullanıcı Arabirimi, fiziksel cihazınızın çeşitli cihaz bileşenleri donanım durumunu görüntülemek için kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz olayları ve karşılık gelen Uyarıları görüntüle
> * Cihaz bileşenlerinin donanım durumunu görüntüleme
> * Cihazınız için kapasite ve işlem ölçümlerini görüntüleme
> * Yapılandırma ve Uyarıları yönetme

## <a name="view-device-events"></a>Cihaz olaylarını görüntüle

Cihaz olayı görüntülemek için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında Data Box Ucunuzdaki Git / veri kutusu ağ geçidi kaynağı ve ardından Git **İzleme > cihaz olaylarını**.
2. Bir olay seçin ve uyarı ayrıntılarını görüntüleyin. Uyarı durumu düzeltmek için ilgili eylemleri gerçekleştirin.

    ![Olay ve görünümü ayrıntıları](media/data-box-edge-monitor/view-device-events.png)

## <a name="view-hardware-status"></a>Donanım durumunu görüntüleme

Yerel web UI cihaz bileşenlerinizi donanım durumunu görüntülemek için aşağıdaki adımları uygulayın. Bu bilgiler, yalnızca bir veri kutusu Edge cihazı için kullanılabilir.

1. Yerel web kullanıcı Arabirimine cihazınızın bağlanın.
2. Git **Bakım > donanım durumunu**. Çeşitli cihaz bileşenlerinin durumunu görüntüleyebilirsiniz.

    ![Donanım durumunu görüntüleme](media/data-box-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Ölçümleri görüntüle

Ayrıca, cihaz ve cihaz sorunlarını gidermek için bazı durumlarda performansını izlemek için ölçümleri görüntüleyebilir.

Seçili cihaz ölçümler için bir grafik oluşturmak için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında, kaynak için Git **İzleme > ölçümleri** seçip **ölçüm Ekle**.

    ![Ölçüm ekleme](media/data-box-edge-monitor/view-metrics-1.png)

2. Kaynak otomatik olarak doldurulur.  

    ![Geçerli kaynak](media/data-box-edge-monitor/view-metrics-2.png)

    Başka bir kaynak belirtmek için kaynağı seçin. Üzerinde **bir kaynak seçin** dikey penceresinde, abonelik, kaynak grubu, kaynak türü ve istediğiniz ölçümleri görüntülemek ve seçmek belirli bir kaynak seçin **Uygula**.

    ![Başka bir kaynak seçin](media/data-box-edge-monitor/view-metrics-3.png)

3. Açılan listeden Cihazınızı izlemek için bir ölçüm seçin. Ölçümler olabilir **kapasite ölçümleri** veya **işlem ölçümlerini**. Cihazın kapasitesi için kapasite ölçümlerini ilgilidir. İşlem ölçümlerini okuma için ilgili ve Azure depolama alanına yazma işlemleri.

    |Kapasite ölçümleri                     |Açıklama  |
    |-------------------------------------|-------------|
    |**Kullanılabilir kapasite**               | Cihaza yazılan veri boyutunu ifade eder. Diğer bir deyişle, cihazda kullanılabilir kapasite budur. <br></br>Hem cihaz hem de bulut üzerindeki bir kopyasına sahip dosyaların yerel kopyasını silerek cihaz kapasite boşaltabilirsiniz.        |
    |**Toplam Kapasite**                   | Toplam bayt veri yazmak için cihazda ifade eder. Bu ayrıca yerel önbelleğin toplam boyutu adlandırılır. <br></br> Veri diski ekleyerek, mevcut bir sanal cihazın kapasitesini şimdi artırabilirsiniz. VM için hiper yönetici Yönetimi aracılığıyla veri diski ekleyin ve sonra VM'yi yeniden başlatın. Ağ geçidi cihazın yerel depolama havuzuna yeni eklenen veri diski uyum sağlayacak şekilde genişletir. <br></br>Daha fazla bilgi için Git [Hyper-V sanal makinesi için bir sabit sürücü ekleme](https://www.youtube.com/watch?v=EWdqUw9tTe4). |
    
    |İşlem ölçümleri              | Açıklama         |
    |-------------------------------------|---------|
    |**Karşıya yüklenen bayt (cihaz) bulut**    | Cihazınızdaki tüm paylaşımlar arasında tüm baytların toplamından karşıya yüklendi        |
    |**Karşıya yüklenen bayt (paylaşım) bulut**     | Bayt paylaşım karşıya yüklendi. Bu olabilir: <br></br> Olan ortalama (tüm baytların toplamından karşıya paylaşım / sayı paylaşım),  <br></br>En fazla bayt sayısı en fazla bir paylaşımdan karşıya yüklendi <br></br>Min, en az bir paylaşımdan karşıya yüklenen bayt sayısı      |
    |**Bulut indirme aktarım hızı (paylaşım)**| Paylaşım bayt indirildi. Bu olabilir: <br></br> Olan ortalama (tüm baytların toplamından okuyun veya bir paylaşım için indirilen / sayı paylaşım) <br></br> En fazla bayt sayısı en fazla bir paylaşımdan indirildi<br></br> ve minimum bayt sayısı en az bir paylaşımdan indirilir.  |
    |**Aktarım hızı bulut okuyun**            | Tüm baytlar toplamına, cihazınızdaki tüm paylaşımlar arasında buluttan okuyun.     |
    |**Bulut karşıya yükleme verimi**          | Buluta arasında tüm paylaşımları Cihazınızda yazılan tüm baytların toplamından     |
    |**Bulut karşıya yükleme verimi (paylaşım)**  | Buluta bir paylaşımdan yazılan tüm baytların toplamından / # paylaşımlarının average, max ve Paylaşım başına min      |
    |**Okuma aktarım hızı (ağ)**           | Sistem ağ aktarım hızı için buluttan okuma bayt içerir. Bu görünüm, paylaşımları için kısıtlı olmayan veriler içerebilir. <br></br>Bölme, cihazdaki tüm ağ bağdaştırıcıları üzerinden trafiği gösterir. Bu, bağlı veya etkin olmayan bağdaştırıcıları içerir.      |
    |**Aktarım hızı (ağ) yazma**       | Sistem ağ verimliliği buluta yazılan bayt içerir. Bu görünüm, paylaşımları için kısıtlı olmayan veriler içerebilir. <br></br>Bölme, cihazdaki tüm ağ bağdaştırıcıları üzerinden trafiği gösterir. Bu, bağlı veya etkin olmayan bağdaştırıcıları içerir.          |
    |**Edge işlem - bellek kullanımı**      | Veri kutusu Ucunuzdaki için IOT Edge cihazı için bellek kullanımı. Yüksek kullanım görürseniz ve cihaz performansını dağıtmış olan geçerli iş yükleri tarafından etkileniyorsa, sonraki adımları belirlemek için Microsoft Support başvurun. <br></br>Bu ölçüm, ağ geçidi veri kutusu doldurulmamış.          |
    |**Edge işlem - CPU yüzdesi**    | CPU kullanımı, veri kutusu Edge için IOT Edge cihazı. Yüksek kullanım görürseniz ve cihaz performansını dağıtmış olan geçerli iş yükleri tarafından etkileniyorsa, sonraki adımları belirlemek için Microsoft Support başvurun. <br></br>Bu ölçüm, ağ geçidi veri kutusu doldurulmamış.        |
1. Açılır listeden bir ölçüm seçildiğinde, toplama da tanımlanabilir. Toplama, belirtilen zaman aralığı içinde toplanır. gerçek değer ifade eder. Toplanan değerler, ortalama, minimum veya maksimum değeri olabilir. Toplama, ortalama, en yüksek veya en düşük seçin.

    ![Grafiği Göster](media/data-box-edge-monitor/view-metrics-4.png)

5. Seçtiğiniz ölçüm birden çok örneği varsa, prosedürün seçeneği kullanılabilir. Seçin **uygulamak bölme** ve ardından istediğiniz dökümünü görmek bir değer seçin.

    ![Bölme uygula](media/data-box-edge-monitor/view-metrics-5.png)

6. Artık yalnızca birkaç örnekleri için dökümünü görmek istiyorsanız, verileri filtreleyebilirsiniz. Yalnızca iki bağlı ağ arabirimleri için ağ verimi, Cihazınızda görmek istiyorsanız, örneğin, bu durumda, bu arabirimleri filtre uygulayabilirsiniz. Seçin **Filtre Ekle** ve filtreleme için ağ arabirimi adı belirtin.

    ![Filtre ekle](media/data-box-edge-monitor/view-metrics-6.png)

7. Ayrıca kolay erişim için panoya grafik sabitleme.

    ![Panoya sabitle](media/data-box-edge-monitor/view-metrics-7.png)

8. Grafik verileri bir Excel elektronik tablosuna dışarı aktarma veya paylaşabileceğiniz grafik bağlantısını almak için komut çubuğundan paylaş seçeneğini seçin.

    ![Verileri dışarı aktarma](media/data-box-edge-monitor/view-metrics-8.png)

## <a name="manage-alerts"></a>Uyarıları yönetme

Cihazınızı şirket kaynaklarının kullanımını ilgili uyarı koşulları hakkında bilgilendirmek için uyarı kuralları yapılandırın. Uyarı koşulları için Cihazınızı izlemek için uyarı kuralları yapılandırabilirsiniz. Uyarılar hakkında daha ayrıntılı bilgi için şuraya gidin [oluşturun, görüntüleyin ve yönetin, Azure İzleyici ölçüm uyarıları](../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Sonraki adımlar 

[Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.