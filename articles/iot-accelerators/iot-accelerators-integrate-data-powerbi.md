---
title: Power BI - Azure'ı kullanarak Uzaktan izleme verilerini Görselleştirme | Microsoft Docs
description: Bu öğreticide Power BI Desktop ve Cosmos DB integerate verileri, bir uzaktan izleme çözümü özelleştirilmiş görselleştirmemizdeki kullanılır. Bu şekilde, kullanıcılar, kendi özel panolar oluşturma ve kullanıcılara çözüm üzerinde değil paylaşın.
author: asdonald
manager: hegate
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: 3398c6d318e0e3c51d3f6cfe8af651a6e3f55c9c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61448158"
---
# <a name="visualize-remote-monitoring-data-using-power-bi"></a>Uzaktan izleme verilerini Power BI kullanarak Görselleştirme

Bu öğreticide, Uzaktan izleme çözümü verilerinizi Power bı'a CosmosDB takın konusunda size yol gösterir. Bu bağlantı kuruldu, ardından kendi özel panolar oluşturabilir ve bunları, Uzaktan izleme çözümü panosuna dönün ekleyin. Hazır ek olarak oluşturulması daha özel grafikler bu iş akışı sağlar. Bu öğreticide daha sonra diğer veri akışları ile tümleştirin veya Uzaktan izleme çözümünüzü dışında kullanılması için özel panolar oluşturmak için de kullanabilirsiniz. Power bı'da panolar oluşturmaya belirli parçaları seçerken birbiriyle etkileşim kuran her paneli de yapabilirsiniz anlamına gelir. Örneğin, yalnızca, sanal kamyon ilgili bilgilerin gösteren bir filtre olabilir ve panonuz her parça yalnızca kamyon bilgi benzetimli göstermek için etkileşimde. Power BI dışındaki bir aracı kullanmak istiyorsanız, Cosmos veritabanı veya özel bir veritabanı, bir oluşturduysanız kanca ve tercih ettiğiniz görselleştirme aracı kullanmak için bu adımları da genişletebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

- Şu anda çalışan bir uzaktan izleme çözümü olmalıdır
- Erişiminiz olmalıdır [Azure portalı](https://portal.azure.com) ve aboneliğinizi üzerinde IOT hub'ı ve çözüm çalışıyor
- Olmalıdır [Power BI desktop](https://powerbi.microsoft.com) herhangi bir sürümü yüklü yapar


## <a name="information-needed-from-azure-portal"></a>Azure Portalı'ndan gerekli bilgiler

1. Gidin [Azure portalı](https://portal.azure.com) ve gerekirse oturum

2. Sol panelde, kaynak gruplar

    ![Yan paneli Nav](./media/iot-accelerators-integrate-data-powerbi/side_panel.png)

3. Hangi IOT çözümünüzü çalıştığı kaynak grubuna gidin ve bu kaynak grubunun genel bakış sayfasına ulaşmak için tıklayın. 

4. Bu genel bakış sayfasında türü "Azure Cosmos DB hesabı" öğesini tıklatın, ardından Cosmos DB akışa Genel Bakış sayfasına bu IOT çözüm için alınır.

    ![Kaynak Grubu](./media/iot-accelerators-integrate-data-powerbi/resource_groups.png)

5. Sol panelde, "Anahtarlar" bölümüne tıklayın ve Power BI hizmetinde kullanılması için aşağıdaki değerleri not alın:

   - URI
   - Birincil anahtar

     ![keys](./media/iot-accelerators-integrate-data-powerbi/keys.png)

## <a name="setting-up-the-stream-in-power-bi"></a>Power bı'da Stream ayarlama
  
1. Power BI Masaüstü uygulamasını açın ve sol üst köşedeki "Veri Al"'a tıklayın. 

    ![Veri alma](./media/iot-accelerators-integrate-data-powerbi/get_data.png)

2. Veri girmeniz istendiğinde, "Azure Cosmos DB" için arama seçin ve bu bağlayıcıyı seçin. Bu bağlayıcı, temelde doğrudan cosmos veritabanı, Azure IOT çözümünüzün veri çeker
  
    ![Cosmos DB](./media/iot-accelerators-integrate-data-powerbi/cosmos_db.png)
  
3. Yukarıda kaydettiğiniz bilgileri girin:

    * URI
    * Birincil anahtar

4. Power BI'a alınması için tüm tabloları seçin. Bu eylem verileri yüklenmesini devre dışı başlatır. Çözümünüze bir uzun süre çalışan, daha uzun, veri yüklemek sürebilir (en fazla birkaç saat). 

    ![Tabloları İçeri Aktar](./media/iot-accelerators-integrate-data-powerbi/import_tables.png)

5. Veri tamamlandıktan sonra yükleme, Power BI'ın üst satırdaki "sorguları Düzenle"'a tıklayın ve sarı çubuk her tablo için okları tıklayarak tüm tabloları genişletin. Bu temelde tüm sütunları gösterecek şekilde genişletir. Nasıl veri nem süresini hızlandırmak, vb. gibi şeyler için doğru türde olmadığı fark edeceksiniz.

    ![Yeni bir sütun](./media/iot-accelerators-integrate-data-powerbi/new_column.png)
  
    Örneğin, UNIX zamanına bağlayıcı aracılığıyla zaman geldiğini Power BI'a gelen verileri değiştirildi. Bu dönüştürme için ayarlamak için bundan sonra yeni bir sütun oluşturup bu eşitlik tarih saat biçiminde almak için kullanabilirsiniz: 

    ```text
    #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [Document.device.msg.received]/1000)
    ```

    ![Güncelleştirilmiş tablo](./media/iot-accelerators-integrate-data-powerbi/updated_table.png)
  
    Document.Device.msg.Received sadece UNIX biçimlendirmeye sahip sütunları biridir ve dönüştürme gereken diğer kullanıcılarla değiştirdi. 
  
    Diğer veri noktası türü dize çiftler değiştirilebilir veya burada uygun olarak yukarıdaki adımları kullanarak Int dönüştürüldü.

## <a name="creating-a-dashboard"></a>Bir pano oluşturma

Akış Bağlantı kurulduktan sonra kişiselleştirilmiş panolar oluşturmaya hazırsınız! Pano aşağıdaki bizim sanal cihazlar ve çevresinde gibi farklı özetlere gösteren tarafından yayılan telemetri alma, bir örnek verilmiştir: 

* Cihaz konumu (sağdaki) bir haritada
* Cihaz durum ve önem derecesi. (sol üst)
* Bir yerde kuralları ile cihazları ve bunların (sol alt) giderek herhangi bir uyarı varsa

![Power BI Görselleştirme](./media/iot-accelerators-integrate-data-powerbi/visual_data.png)

## <a name="publishing-the-dashboard-and-refreshing-the-data"></a>Panoyu yayımlama ve veri yenileme

Panolarınızı başarıyla oluşturduktan sonra olmasını öneririz, [Power BI panolarınızı yayımlama](https://docs.microsoft.com/power-bi/desktop-upload-desktop-files) başkalarıyla paylaşmak için.

Ayrıca isteyebilirsiniz [veri yenileme](https://docs.microsoft.com/power-bi/refresh-data) yayımlanan Panoda en yeni veri kümesi olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Power BI'ı kullanarak Uzaktan izleme verileri görselleştirme hakkında bilgi edindiniz

Uzaktan izleme çözümü özelleştirme hakkında daha fazla bilgi için bkz:

* [Uzaktan izleme çözüm kullanıcı arabirimini özelleştirme](iot-accelerators-remote-monitoring-customize.md)
* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

