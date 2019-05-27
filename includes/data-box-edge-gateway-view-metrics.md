---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: e02c0b86cd542b3ea12914e35a6577cf4e9b43d8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161337"
---
Ayrıca, cihaz ve cihaz sorunlarını gidermek için bazı durumlarda performansını izlemek için ölçümleri görüntüleyebilir.

Seçili cihaz ölçümler için bir grafik oluşturmak için Azure portalında aşağıdaki adımları uygulayın.

1. Azure portalında, kaynak için Git **İzleme > ölçümleri** seçip **ölçüm Ekle**.

    ![Ölçüm ekle](media/data-box-edge-gateway-view-metrics/view-metrics-1.png)

2. Kaynak otomatik olarak doldurulur.  

    ![Geçerli kaynak](media/data-box-edge-gateway-view-metrics/view-metrics-2.png)

    Başka bir kaynak belirtmek için kaynağı seçin. Üzerinde **bir kaynak seçin** dikey penceresinde, abonelik, kaynak grubu, kaynak türü ve istediğiniz ölçümleri görüntülemek ve seçmek belirli bir kaynak seçin **Uygula**.

    ![Başka bir kaynak seçin](media/data-box-edge-gateway-view-metrics/view-metrics-3.png)

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
    |**Edge işlem - bellek kullanımı**      | Bu ölçüm veri kutusu ağ geçidi için geçerlidir ve bu nedenle değil doldurulmuş değil.          |
    |**Edge işlem - CPU yüzdesi**    | Bu ölçüm veri kutusu ağ geçidi için geçerlidir ve bu nedenle değil doldurulmuş değil.         |

4. Açılır listeden bir ölçüm seçildiğinde, toplama da tanımlanabilir. Toplama, belirtilen zaman aralığı içinde toplanır. gerçek değer ifade eder. Toplanan değerler, ortalama, minimum veya maksimum değeri olabilir. Toplama, ortalama, en yüksek veya en düşük seçin.

    ![Grafiği Göster](media/data-box-edge-gateway-view-metrics/view-metrics-4.png)

5. Seçtiğiniz ölçüm birden çok örneği varsa, prosedürün seçeneği kullanılabilir. Seçin **uygulamak bölme** ve ardından istediğiniz dökümünü görmek bir değer seçin.

    ![Bölme uygula](media/data-box-edge-gateway-view-metrics/view-metrics-5.png)

6. Artık yalnızca birkaç örnekleri için dökümünü görmek istiyorsanız, verileri filtreleyebilirsiniz. Yalnızca iki bağlı ağ arabirimleri için ağ verimi, Cihazınızda görmek istiyorsanız, örneğin, bu durumda, bu arabirimleri filtre uygulayabilirsiniz. Seçin **Filtre Ekle** ve filtreleme için ağ arabirimi adı belirtin.

    ![Filtre ekle](media/data-box-edge-gateway-view-metrics/view-metrics-6.png)

7. Ayrıca kolay erişim için panoya grafik sabitleme.

    ![Panoya sabitle](media/data-box-edge-gateway-view-metrics/view-metrics-7.png)

8. Grafik verileri bir Excel elektronik tablosuna dışarı aktarma veya paylaşabileceğiniz grafik bağlantısını almak için komut çubuğundan paylaş seçeneğini seçin.

    ![Verileri dışarı aktar](media/data-box-edge-gateway-view-metrics/view-metrics-8.png)