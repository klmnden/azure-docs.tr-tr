---
title: Power BI - Azure kullanarak Uzaktan izleme verilerini Görselleştirme | Microsoft Docs
description: Bu öğretici Power BI Desktop ve Cosmos DB integerate verileri, bir uzaktan izleme çözümü özelleştirilmiş bir görsel öğe kullanır. Bu şekilde, kullanıcılar kendi özel panolar oluşturun ve bunları çözümü değil, kullanıcılara paylaşın.
services: iot-suite
suite: iot-suite
author: asdonald
manager: hegate
ms.author: asdonald
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 55feb56008a54676bd0af332e251da94a9653aaf
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="visualize-remote-monitoring-data-using-power-bi"></a>Power BI kullanarak Uzaktan izleme verileri Görselleştir

Bu öğretici, Uzaktan izleme çözümü verilerinizi Power bı'da CosmosDB takın konusunda size yol gösterecek. Bu bağlantı kuruldu, daha sonra kendi özel panolar oluşturabilir ve bunları geri Uzaktan izleme çözümü panonuz ekleyin. Bu iş akışı olanları kutunun dışında ek olarak oluşturulması daha özel grafikler sağlar. Bu öğreticide daha sonra Uzaktan izleme çözümü dışında kullanılması özel panolar yapı veya diğer veri akışları ile tümleştirmek için de kullanabilirsiniz. Power BI panoları oluşturma belirli parça seçerken birbiriyle etkileşim her paneli de yapabilirsiniz anlamına gelir. Örneğin, benzetimli kamyonlar hakkında tüm bilgiler gösterilmiştir bir filtre olabilir ve panonuz her parçasının kamyon bilgileri yalnızca benzetimli göstermek için etkileşimde bulunur. Power BI dışında bir aracı kullanmak istiyorsanız, seçeceğiniz görselleştirme araç kullanın ve, bir oluşturduysanız Cosmos veritabanı veya özel veritabanı kanca için aşağıdaki adımları da genişletebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

- Şu anda çalışan bir uzaktan izleme çözümü olması gerekir
- Erişiminiz olmalıdır [Azure Portal](https://portal.azure.com) ve aboneliğiniz üzerinde IOT hub'ı ve çözüm çalışıyor
- Bilmeniz gereken [Power BI desktop](https://powerbi.microsoft.com) yüklü herhangi bir sürümü ne yapacağını


## <a name="information-needed-from-azure-portal"></a>Azure Portalı'ndan gerekli bilgiler

1. Gidin [Azure Portal](https://portal.azure.com) ve gerekirse oturum açın

2. Sol panelde kaynak Gruplar'ı tıklatın.

    ![Yan paneli Nav](./media/iot-accelerators-integrate-data-powerbi/side_panel.png)

3. Hangi IOT çözümünüzü çalıştığı kaynak grubuna gidin ve bu kaynak grubunun genel bakış sayfasına ulaşmak için tıklatın. 

4. Bu genel bakış sayfasında türü "Azure Cosmos DB hesabı" öğesini tıklatın, ardından Cosmos DB akışı genel bakış sayfasına bu IOT çözüm için gerçekleştirilecek.

    ![Kaynak Grubu](./media/iot-accelerators-integrate-data-powerbi/resource_groups.png)

5. Sol panelindeki "Anahtarlar" bölümüne tıklayın ve Powerbı kullanılması için aşağıdaki değerleri not edin:

    - URI
    - Birincil Anahtar

    ![keys](./media/iot-accelerators-integrate-data-powerbi/keys.png)

## <a name="setting-up-the-stream-in-power-bi"></a>Power BI akışında ayarlama
  
1. Power BI Masaüstü uygulamasını açın ve sol üst köşeden "Veri Al"'i tıklatın. 

    ![Veri Al](./media/iot-accelerators-integrate-data-powerbi/get_data.png)

2. Veri girmek için sorulduğunda, "Azure Cosmos VT" için arama seçin ve bu bağlayıcıyı seçin. Bu bağlayıcı temelde doğrudan Azure IOT çözümünüzün cosmos veritabanından veri çeker
  
    ![Cosmos DB](./media/iot-accelerators-integrate-data-powerbi/cosmos_db.png)
  
3. Yukarıdaki kaydettiğiniz bilgileri girin:

    * URI
    * Birincil Anahtar

4. Power BI içeri aktarılacak tüm tabloları seçin. Bu eylem verileri yüklenirken kapalı tetiklersiniz. Çözümünüzü bir uzun süre çalışan, uzun, veri yüklemek alabilir (kadar birkaç saat). 

    ![Tabloları](./media/iot-accelerators-integrate-data-powerbi/import_tables.png)

5. Veri tamamlandıktan sonra yükleme, Power BI en üstündeki satırda "sorgular Düzenle"'ı tıklatın ve her tablo için sarı çubuğu okları tıklatarak tüm tabloları genişletin. Bu temelde tüm sütunları gösterecek şekilde genişletir. Nasıl veri nem, süresini hızlandırmak, vb. gibi şeyler için doğru türde olmayan fark edeceksiniz.

    ![Yeni bir sütun](./media/iot-accelerators-integrate-data-powerbi/new_column.png)
  
    Örneğin, Power BI'a gelen veriler bağlayıcı üzerinden gelen zaman UNIX zaman içinde değiştirildi. Bu dönüştürme için ayarlamak için ileriye dönük yeni bir sütun oluşturup bu denklemi tarih saat biçiminde almak için kullanabilirsiniz: 

    ```text
    #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [Document.device.msg.received]/1000)
    ```

    ![Güncelleştirilmiş tablo](./media/iot-accelerators-integrate-data-powerbi/updated_table.png)
  
    Document.Device.msg.Received UNIX biçimlendirmeye sahip sütunlar biridir ve dönüştürme gereken diğer kişilerle değiştirdi. 
  
    Diğer veri noktaları dize çiftleri değiştirilebilir türü ya da nerede uygun aynı kullanarak olarak yukarıdaki adımları Int dönüştürüldü.

## <a name="creating-a-dashboard"></a>Bir pano oluşturma

Akış bağlandıktan sonra kişiselleştirilmiş panolar oluşturmaya hazırsınız! Pano, telemetri immmited bizim sanal cihazlar tarafından olmasına ve farklı gösteren çevresinde gibi döner alma örneğidir: 

* Aygıt konumu bir harita (sağdaki)
* Kendi durum ve önem derecesi olan cihazlar. (sol üst)
* Kurallar, yerinde aygıtlarıyla ve bunları (sol alt) giderek uyarıları varsa

![Powerbı Görselleştirme](./media/iot-accelerators-integrate-data-powerbi/visual_data.png)

## <a name="publishing-the-dashboard-and-refreshing-the-data"></a>Pano yayımlama ve verileri yenileme

Panolarınızı başarıyla oluşturduktan sonra öneririz, [Power BI panolarınızı yayımlama](https://docs.microsoft.com/en-us/power-bi/desktop-upload-desktop-files) kişilerle paylaşmak için.

Ayrıca istersiniz [veri yenileme](https://docs.microsoft.com/en-us/power-bi/refresh-data) yayımlanan Panoda en son veri kümesi olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Power BI'ı kullanarak Uzaktan izleme verilerini görselleştirmek hakkında öğrendiniz

Uzaktan izleme çözümü özelleştirme hakkında daha fazla bilgi için bkz:

* [Uzaktan izleme çözümü UI Özelleştirme](iot-accelerators-remote-monitoring-customize.md)
* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

