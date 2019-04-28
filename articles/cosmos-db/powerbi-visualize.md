---
title: Power BI öğretici için Azure Cosmos DB Bağlayıcısı
description: JSON alma, bilgilendirici raporlar oluşturabilir ve Azure Cosmos DB ile Power BI Bağlayıcısı'nı kullanarak verileri görselleştirmek için Power BI Bu öğreticiyi kullanın.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: sngun
ms.openlocfilehash: 2c58b982e596c95aa47442c1897410fe9ab6b99a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60929832"
---
# <a name="visualize-azure-cosmos-db-data-by-using-the-power-bi-connector"></a>Power BI Bağlayıcısı'nı kullanarak Azure Cosmos DB verileri Görselleştir

[Power BI](https://powerbi.microsoft.com/) oluşturduğunuz ve panolar ve raporlar çevrimiçi bir hizmettir. Power BI Desktop, yazma aracı, çeşitli veri kaynaklarından veri almanızı sağlayan bir rapordur. Azure Cosmos DB veri kaynağı, Power BI Desktop ile kullanabileceğiniz biridir. Power BI için Azure Cosmos DB Bağlayıcısı ile Azure Cosmos DB hesabı için Power BI Desktop bağlanabilirsiniz.  Power BI için Azure Cosmos DB veri içe aktardıktan sonra dönüştürmek, raporlar oluşturabilir ve raporları Power BI'a yayımlayın.   

Bu makalede, Power BI Desktop için Azure Cosmos DB hesabına bağlanmak için gereken adımlar açıklanmaktadır. Bağlanma sonra bir koleksiyonuna gidin, verileri ayıklamak, JSON verileri tablo biçimine dönüştürmek ve Power BI'da bir raporu yayımlayın.

> [!NOTE]
> Azure Cosmos DB için Power BI Bağlayıcısı, Power BI Desktop için bağlanır. Power BI Desktop'ta oluşturulan raporlar için Powerbı.com yayımlanabilir. Azure Cosmos DB verilerinin doğrudan ayıklama PowerBI.com gerçekleştirilemiyor. 

> [!NOTE]
> Power BI Bağlayıcısı ile Azure Cosmos DB'ye bağlanmanın şu anda, Azure Cosmos DB SQL API ve yalnızca Gremlin API hesapları için desteklenir.

## <a name="prerequisites"></a>Önkoşullar
Power BI öğreticideki yönergeleri izlemeden önce aşağıdaki kaynaklara erişimi olduğundan emin olun:

* [Power BI Desktop'ın en son sürümünü indirin](https://powerbi.microsoft.com/desktop).

* İndirme [örnek volkan verileri](https://github.com/Azure-Samples/azure-cosmos-db-sample-data/blob/master/SampleData/VolcanoData.json) github'dan.

* [Bir Azure Cosmos DB veritabanı hesabı oluşturma](https://azure.microsoft.com/documentation/articles/create-account/) ve volkan verileri kullanarak içeri aktarma [Azure Cosmos DB veri geçiş aracı](import-data.md). Veriler içeri aktarılırken, kaynak ve veri Geçiş Aracı'nda hedefleri için aşağıdaki ayarları göz önünde bulundurun:

   * **Kaynak parametreleri** 

       * **İçeri aktarın:** JSON dosyaları

   * **Hedef parametreleri** 

      * **Bağlantı dizesi:** `AccountEndpoint=<Your_account_endpoint>;AccountKey=<Your_primary_or_secondary_key>;Database= <Your_database_name>` 

      * **Bölüm anahtarı:**  /ülkeyi 

      * **Koleksiyon aktarım hızı:** 1000 

PowerBI.com raporlarınızda paylaşmak için Powerbı.com'daki hesabınız olmalıdır.  Power BI ve Power BI Pro hakkında daha fazla bilgi için bkz: [ https://powerbi.microsoft.com/pricing ](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Başlayalım
Şimdi bu öğreticide, dünyanın dört bir yanındaki volkanlar Çincesi bir geologist olduğunu hayal edin. Bir Azure Cosmos DB hesabını volkan veriler ve JSON belge biçimi aşağıdaki gibidir:

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

Azure Cosmos DB hesabınızdan volkan verileri almak ve etkileşimli bir Power BI rapor verileri görselleştirin.

1. Power BI Desktop'ı çalıştırın.

2. Yapabilecekleriniz **Veri Al**, bakın **son kaynaklar**, veya **açık diğer rapor** doğrudan Hoş Geldiniz ekranı. "Ekranı kapatmak için X" sağ üst köşesinde'ı seçin. **Rapor** Power BI Desktop'ın görünümü görüntülenir.
   
   ![Power BI Desktop rapor görünümü - Power BI Bağlayıcısı](./media/powerbi-visualize/power_bi_connector_pbireportview.png)

3. Seçin **giriş** şeridini ve ardından tıklayarak **Veri Al**.  **Veri Al** penceresi görüntülenmelidir.

4. Tıklayarak **Azure**seçin **Azure Cosmos DB (Beta)** ve ardından **Connect**. 

    ![Power BI Desktop, Power BI Bağlayıcısı - veri alma](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   

5. Üzerinde **bağlayıcıyı Önizleme** sayfasında **devam**. **Azure Cosmos DB** penceresi görüntülenir.

6. Aşağıda gösterildiği gibi verileri almak ister ve ardından Azure Cosmos DB hesabı uç noktasının URL'sini belirtin **Tamam**. Kendi hesabınızı kullanmak için URL URI kutusundan alabilirsiniz **anahtarları** Azure portal'ın dikey penceresi. İsteğe bağlı olarak koleksiyon adı veritabanı adı girin veya verilerin nereden geldiğini belirlemek için koleksiyon ve veritabanı seçmek için Gezgin kullanın.
   
7. Bu uç noktaya ilk kez bağlanıyorsanız, hesap anahtarı istenir. Kendi hesabınızı için anahtarını almak **birincil anahtar** kutusunda **salt okunur anahtarları** Azure portal'ın dikey penceresi. Uygun anahtarı girin ve ardından **Connect**.
   
   Rapor oluştururken salt okunur anahtarı kullanmanızı öneririz. Bu, olası güvenlik risklerini ana anahtarı gereksiz riskini engeller. Salt okunur anahtar kullanılabilir **anahtarları** Azure portal'ın dikey penceresi. 
    
8. Hesap başarıyla bağlandığında **Gezgin** bölmesi görünür. **Gezgin** hesabı altında veritabanlarının listesini gösterir.

9. ' I tıklatın ve raporu geldiği için verileri nerede seçin veritabanında genişletin **volcanodb** (veritabanı adınız farklı olabilir).   

10. Şimdi, veri almak için seçin içeren bir koleksiyon seçin **volcano1** (koleksiyon adınızı farklı olabilir).
    
    Önizleme bölmesinde bir listesini gösterir **kayıt** öğeleri.  Bir belge olarak temsil edilen bir **kayıt** Power bı'da türü. Benzer şekilde, bir belge içindeki iç içe bir JSON blok da olan bir **kayıt**.
    
    ![Azure Cosmos DB Power BI Bağlayıcısı - Gezgin penceresi için Power BI Öğreticisi](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Tıklayın **Düzenle** verileri dönüştürmek için sorgu Düzenleyicisi'nde yeni bir pencere başlatmak için.

## <a name="flattening-and-transforming-json-documents"></a>Düzleştirme ve JSON belgeleri dönüştürme
1. Power BI sorgu Düzenleyicisi penceresine geçin burada **belge** orta bölmesinde sütun.
   ![Power BI Desktop sorgu Düzenleyicisi](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Genişletici sağ tarafında tıklayın **belge** sütun başlığı.  Bağlam menüsü ile alanların bir listesi görüntülenir.  Raporunuz için gereken alanları örneği için volkan adı, ülke, bölge, konum, yükseltme, türü, durumu ve son bilmeniz patlama'ı seçin. Onay kutusunu temizleyin **ön ek olarak orijinal sütun adını kullan** kutusuna ve ardından **Tamam**.
   
    ![Power BI öğretici için Azure Cosmos DB Power BI Bağlayıcısı - belgeler genişletin](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. Orta bölmeye sonuç önizlemesi seçili alanları görüntüler.
   
    ![Power BI öğretici için Azure Cosmos DB Power BI Bağlayıcısı - sonuçları düzleştirme](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. Bizim örneğimizde, Location özelliği, bir belgedeki bir GeoJSON taşıdır.  Gördüğünüz gibi konum olarak temsil edilen bir **kayıt** Power BI Desktop'ta türü.  
5. Genişletici Document.Location sütun başlığına sağ tarafında tıklayın.  Tür ve koordinatları alanları ile bağlam menüsü görüntülenir.  Şimdi koordinatları alanı seçin, olun **ön ek olarak orijinal sütun adını kullan** seçili değilse ve tıklayın **Tamam**.
   
    ![Azure Cosmos DB Power BI Bağlayıcısı - konum kaydı için Power BI Öğreticisi](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. Orta bölmede bir koordinat sütununun gösterdiğini **listesi** türü.  Öğreticinin başında gösterildiği gibi bu öğreticideki GeoJSON veri noktası koordinatları dizide kaydedilen enlem ve boylam değerleri ile türüdür.
   
    Koordinatları [1] temsil ederken, enlem koordinatları [0] öğesi boylam temsil eder.
    ![Power BI öğretici için Azure Cosmos DB Power BI Bağlayıcısı - koordinatları listesi](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. Koordinat dizisi düzleştirmek için oluşturma bir **özel sütun** LatLong çağrılır.  Seçin **Sütun Ekle** tıklayın ve Şerit **özel sütun**.  **Özel sütun** penceresi görüntülenir.
8. Yeni bir sütun, örneğin LatLong için bir ad belirtin.
9. Ardından, yeni bir sütun özel formülünü belirtin.  Bizim örneğimizde, biz aşağıdaki formülü kullanarak aşağıda gösterildiği gibi bir virgülle ayrılmış enlem ve boylam değerleri birleştirir: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. **Tamam** düğmesine tıklayın.
   
    Üzerinde veri çözümleme ifadeleri (DAX işlevleri dahil olmak üzere DAX) daha fazla bilgi için lütfen [Power BI Desktop'ta DAX temel bilgileri](https://docs.microsoft.com/power-bi/desktop-quickstart-learn-dax-basics).
   
    ![Power BI öğretici için Azure Cosmos DB Power BI Bağlayıcısı - özel Sütun Ekle](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. Şimdi, ortadaki değerlerle doldurulan yeni LatLong sütunları gösterir.
    
    ![Azure Cosmos DB Power BI Bağlayıcısı - özel LatLong sütun için Power BI Öğreticisi](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Yeni bir sütunda bir hata alırsanız, sorgu ayarları altında uygulanan adımlar aşağıdaki şekilde eşleştiğinden emin olun:
    
    ![Uygulanan adımlar olmalıdır kaynağı, gezinti, genişletilmiş belgenin, genişletilmiş Document.Location, özel eklendi](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Adımlarınız farklıysa, ek adımlar silin ve özel sütunu yeniden eklemeyi deneyin. 

11. Tıklayın **Kapat ve Uygula** veri modeli kaydedin.
    
    ![Power BI öğretici için Azure Cosmos DB Power BI Bağlayıcısı - Kapat & Uygula](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>Raporları oluşturma
Power BI Desktop rapor görünümü, verileri görselleştirmek için rapor oluşturmaya başlayabileceğiniz ' dir.  Alanlarına sürükleyip bırakarak raporlar oluşturabilirsiniz **rapor** tuval.

![Power BI Desktop rapor görünümü - Power BI Bağlayıcısı](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

Rapor Görünümü'nde, bulmanız gerekir:

1. **Alanları** bölmesinde, veri modelleri ile raporlarınız için kullanabileceğiniz alanlar listesini görebileceğiniz budur.
2. **Görselleştirmeler** bölmesi. Bir rapor, tek bir veya birden çok görsel öğeler içerebilir.  Gereksinimlerinizi gelen sığdırma görsel türleri çekme **görselleştirmeler** bölmesi.
3. **Rapor** tuval, görsellerin raporunuz için oluşturduğunuz budur.
4. **Rapor** sayfası. Birden çok rapor sayfaları Power BI Desktop'ta ekleyebilirsiniz.

Basit bir etkileşimli harita görünümü rapor oluşturmaya yönelik temel adımlar aşağıda gösterilmiştir.

1. Bizim örneğimizde, her volkan konumunu gösteren bir harita görünümü oluşturacağız.  İçinde **görselleştirmeler** bölmesinde yukarıdaki ekran görüntüsünde vurgulanan harita görsel türüne tıklayın.  Boyarken harita görsel türü görmelisiniz **rapor** tuval.  **Görselleştirme** bölmesinde bir harita visual türüyle ilişkili özellikler kümesini de görüntülemelidir.
2. LatLong alanından artık sürükleyip **alanları** bölmesine **konumu** özelliğinde **görselleştirmeler** bölmesi.
3. Ardından, sürükleyip bırakın volkan ad alanı için **gösterge** özelliği.  
4. Yükseltme alana ardından sürükleyip **boyutu** özelliği.  
5. Harita görmelisiniz visual baloncuklar volkan yükseltilmesini ilişkilendirme Kabarcık boyutu ile her volkan konumunu belirten bir kümesi gösteriliyor.
6. Temel bir rapor oluşturdunuz.  Rapora görselleştirmeler ekleyerek daha fazla özelleştirebilirsiniz.  Bu örnekte, raporun etkileşimli hale getirmek için volkan türü dilimleyici ekledik.  
   
7. Dosya menüsünde **Kaydet** ve PowerBITutorial.pbix kaydedin.

## <a name="publish-and-share-your-report"></a>Raporunuzu paylaşmak ve yayımlama
Raporunuzu paylaşmak için Powerbı.com'daki bir hesabınızın olması gerekir.

1. Power BI Desktop'ta tıklayarak **giriş** Şerit.
2. **Yayımla**’ta tıklayın.  Olması istenir PowerBI.com hesabınız için kullanıcı adını ve parolasını girin.
3. Kimlik doğrulandıktan sonra raporu, seçtiğiniz hedef için yayımlanır.
4. Tıklayın **Power bı'da Aç 'PowerBITutorial.pbix'** görmek ve PowerBI.com üzerinde raporunuzu paylaşmak için.
   
    ![Power BI başarı yayımlayarak! Power bı'da Aç Öğreticisi](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>PowerBI.com bir pano oluşturma
Bir rapora sahipsiniz, Powerbı.com üzerinde paylaşmak sağlar.

Raporunuzu Power BI Desktop'tan PowerBI.com yayımladığınızda, onu oluşturan bir **rapor** ve **veri kümesi** PowerBI.com kiracınızdaki. Örneğin, sonra yayımladığınız adlı bir raporu **PowerBITutorial** Powerbi.com'da PowerBITutorial hem de göreceğiniz **raporları** ve **veri kümeleri** üzerinde bölümler PowerBI.com.

   ![Yeni rapor ve veri kümesi BI.com'da ekran görüntüsü](./media/powerbi-visualize/powerbi-reports-datasets.png)

Paylaşılabilir bir Pano oluşturmak için tıklayın **Canlı sayfayı sabitleme** PowerBI.com raporunuzdaki düğmesi.

   ![Yeni rapor ve veri kümesi BI.com'da ekran görüntüsü](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Ardından yönergeleri izleyin [bir rapordan kutucuk sabitleme](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) yeni bir Pano oluşturmak için. 

Ayrıca, bir Pano oluşturmadan önce rapor geçici değişiklikler yapabilirsiniz. Ancak, bu değişiklikleri uygulamak ve PowerBI.com raporu yeniden yayımlamak için Power BI Desktop kullanmanız önerilir.

<!-- ## Refresh data in PowerBI.com
There are two ways to refresh data, ad hoc and scheduled.

For an ad hoc refresh, simply click on the eclipses (…) by the **Dataset**, e.g. PowerBITutorial. You should see a list of actions including **Refresh Now**. Click **Refresh Now** to refresh the data.

![Screenshot of Refresh Now in PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

For a scheduled refresh, do the following.

1. Click **Schedule Refresh** in the action list. 

    ![Screenshot of the Schedule Refresh in PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. In the **Settings** page, expand **Data source credentials**. 
3. Click on **Edit credentials**. 
   
    The Configure popup appears. 
4. Enter the key to connect to the Azure Cosmos DB account for that data set, then click **Sign in**. 
5. Expand **Schedule Refresh** and set up the schedule you want to refresh the dataset. 
6. Click **Apply** and you are done setting up the scheduled refresh.
-->
## <a name="next-steps"></a>Sonraki adımlar
* Power BI hakkında daha fazla bilgi için bkz: [Power BI ile çalışmaya başlama](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB belge giriş sayfasının](https://azure.microsoft.com/documentation/services/cosmos-db/).

