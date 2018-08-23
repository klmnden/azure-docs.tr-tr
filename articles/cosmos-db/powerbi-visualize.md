---
title: Azure Cosmos DB Bağlayıcısı için Power BI Öğreticisi | Microsoft Docs
description: JSON alma, bilgilendirici raporlar oluşturabilir ve Azure Cosmos DB ile Power BI Bağlayıcısı'nı kullanarak verileri görselleştirmek için Power BI Bu öğreticiyi kullanın.
keywords: power BI öğretici, verileri, power BI Bağlayıcısı görselleştirin
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: sngun
ms.openlocfilehash: 63ea7e384f9bc5713a41f6c5537ec5548810e5d9
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42061738"
---
# <a name="power-bi-tutorial-for-azure-cosmos-db-visualize-data-using-the-power-bi-connector"></a>Azure Cosmos DB için Power BI öğretici: Power BI Bağlayıcısı'nı kullanarak verileri Görselleştir
[PowerBI.com](https://powerbi.microsoft.com/) burada oluşturmak ve paylaşmak için kullanabileceğiniz panolar ve raporlar, siz ve kuruluşunuz için önemli olan verilerle çevrimiçi bir hizmettir.  Power BI Desktop, yazma, çeşitli veri kaynaklarından veri almak, birleştirmek ve veri dönüştürme, güçlü raporlar ve görselleştirmeler oluşturma ve raporları Power BI'da Yayımla sağlayan aracı adanmış bir rapordur.  Power BI Desktop'ın en son sürümüyle artık Azure Cosmos DB bağlayıcısı aracılığıyla Azure Cosmos DB hesabınız için Power BI bağlanabilirsiniz.   

Power BI Bu öğreticide, biz Power BI Desktop'ta bir Azure Cosmos DB hesabına bağlanma, Gezgin'i kullanarak verileri ayıklamak için istediğimiz bir koleksiyonuna gidin, JSON verileri Power BI Desktop sorgu Düzenleyicisi'ni kullanarak tablo biçimine dönüştürmek için adımlarında yol , derleme ve bir raporu powerbi.com üzerinde yayımlayın.

Bu Power BI öğreticiyi tamamladıktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:  

* Nasıl ı Power BI Desktop'ı kullanarak Azure Cosmos DB'den verileri içeren raporlar oluşturabilirsiniz?
* Power BI Desktop'ta bir Azure Cosmos DB hesabına nasıl bağlanabilir miyim?
* Nasıl ı Power BI Desktop'ta bir koleksiyondan veri alabilir?
* Power BI Desktop'ta iç içe geçmiş JSON verileri nasıl dönüştürme?
* Nasıl yayımlamak ve PowerBI.com raporumda paylaşmak?

> [!NOTE]
> Azure Cosmos DB için Power BI Bağlayıcısı, ayıklama ve veri dönüştürme için Power BI Desktop için bağlanır. Power BI Desktop'ta oluşturulan raporlar için Powerbı.com yayımlayabilirsiniz. Powerbı.com'da doğrudan ayıklama ve Azure Cosmos DB veri dönüştürme gerçekleştirilemiyor. 

> [!NOTE]
> Power BI Bağlayıcısı ile Azure Cosmos DB'ye bağlanma, şu anda Azure Cosmos DB SQL ve yalnızca MongoDB API'si hesapları için desteklenir. Azure Cosmos DB MongoDB API'sini kullanarak Power BI'a bağlamak için kullanmalısınız [Simba MongoDB ODBC sürücüsü](http://www.simba.com/drivers/mongodb-odbc-jdbc/).

## <a name="prerequisites"></a>Önkoşullar
Power BI öğreticideki yönergeleri izlemeden önce aşağıdaki kaynaklara erişimi olduğundan emin olun:

* [Power BI Desktop'ın en son sürümünü](https://powerbi.microsoft.com/desktop).
* Tanıtım hesabı ve Azure Cosmos DB hesabınızdaki veriler, belgelerimizin erişim.
  * Tanıtım hesabı, bu öğreticide gösterilen volkan verilerle doldurulur. Bu Tanıtım hesap tarafından SLA bağlı değil ve yalnızca tanıtım amacıyla tasarlanmıştır.  Biz bu tanıtım hesabı dahil olmak üzere değişiklik yapma hakkını saklı tutarız ancak bunlarla sınırlı olmamak hesap sonlandırma, anahtarının değiştirilmesi, değiştirme, erişimini ve veri bulunuruz veya neden olmadan dilediğiniz zaman silin.
    * URL: `https://analytics.documents.azure.com`
    * Salt okunur anahtarı: `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`
  * Veya kendi hesabınızı oluşturmak için bkz. [Azure portalını kullanarak bir Azure Cosmos DB veritabanı hesabı oluşturma](https://azure.microsoft.com/documentation/articles/create-account/). Ardından, örnek volkan almak için gerekenler benzer veri Bu öğreticide kullanılan (ancak GeoJSON blokları içermiyor), bkz [NOAA site](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) ve ardından kullanarak verileri içeri aktarma [Azure Cosmos DB veri geçiş aracı](import-data.md).

PowerBI.com raporlarınızda paylaşmak için Powerbı.com'daki hesabınız olmalıdır.  Ücretsiz ve Power BI Pro için Power BI hakkında daha fazla bilgi edinmek için [ https://powerbi.microsoft.com/pricing ](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Başlayalım
Şimdi bu öğreticide, dünyanın dört bir yanındaki volkanlar Çincesi bir geologist olduğunu hayal edin.  Bir Azure Cosmos DB hesabını volkan veriler ve aşağıdaki örnek belgeyi gibi JSON belgelerinin bakın.

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

Azure Cosmos DB hesabınızdan volkan verileri almak ve aşağıdaki rapor gibi etkileşimli bir Power BI raporundaki verileri görselleştirmek istediğiniz.

![Bu Power BI Bağlayıcısı ile Power BI öğreticiyi izleyerek, Power BI Desktop volkan rapor ile verileri görselleştirme mümkün olacaktır](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Bir denemeye hazır mısınız? Haydi başlayalım.

1. Power BI Desktop, iş istasyonunda çalışır.
2. Power BI Desktop başlatıldıktan sonra bir *Hoş Geldiniz* ekranı görüntülenir.
   
    ![Power BI Desktop Hoş Geldiniz ekranı - Power BI Bağlayıcısı](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. Yapabilecekleriniz **Veri Al**, bkz: **son kaynaklar**, veya **açık diğer rapor** doğrudan *Hoş Geldiniz* ekran.  Ekranı kapatmak için sağ üst köşesindeki X tıklayın. **Rapor** Power BI Desktop'ın görünümü görüntülenir.
   
    ![Power BI Desktop rapor görünümü - Power BI Bağlayıcısı](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Seçin **giriş** şeridini ve ardından tıklayarak **Veri Al**.  **Veri Al** penceresi görüntülenmelidir.
5. Tıklayarak **Azure**seçin **Azure Cosmos DB (Beta)** ve ardından **Connect**. 

    ![Power BI Desktop, Power BI Bağlayıcısı - veri alma](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. Üzerinde **bağlayıcıyı Önizleme** sayfasında **devam**. **Azure Cosmos DB** penceresi görüntülenir.
7. Aşağıda gösterildiği gibi verileri almak ister ve ardından Azure Cosmos DB hesabı uç noktasının URL'sini belirtin **Tamam**. Kendi hesabınızı kullanmak için URL URI kutusundan alabilirsiniz **[anahtarları](manage-account.md#keys)** Azure portal'ın dikey penceresi. Tanıtım hesabı kullanmak için girin `https://analytics.documents.azure.com` URL. 
   
    Bu alanlar isteğe bağlı olarak veritabanı adı, koleksiyon adı ve SQL deyimi boş bırakın.  Bunun yerine, verilerin nereden geldiğini belirlemek için veritabanı ve koleksiyonu seçmek için Gezgin kullanacağız.
   
    ![Azure Cosmos DB Power BI Bağlayıcısı - Desktop penceresine bağlanmak için Power BI Öğreticisi](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. Bu uç noktaya ilk kez bağlanıyorsanız, hesap anahtarı istenir. Kendi hesabınızı için anahtarını almak **birincil anahtar** kutusunda **[salt okunur anahtarları](manage-account.md#keys)** Azure portal'ın dikey penceresi. Tanıtım, anahtar hesabıdır `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Uygun anahtarı girin ve ardından **Connect**.
   
    Rapor oluştururken salt okunur anahtarı kullanmanızı öneririz.  Bu, olası güvenlik risklerini ana anahtarı gereksiz riskini engeller. Salt okunur anahtar kullanılabilir [anahtarları](manage-account.md#keys) yukarıda sağlanan tanıtım hesap bilgilerini Azure portalında veya ın dikey penceresini kullanabilir.
   
    ![Power BI öğretici için Azure Cosmos DB Power BI Bağlayıcısı - hesap anahtarı](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > "Belirtilen veritabanı bulunamadı." diyen bir hata alırsanız geçici çözüm bu adımlarına bakın [Power BI sorunu](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. Hesap başarıyla bağlandığında **Gezgin** bölmesi görünür.  **Gezgin** hesabı altında veritabanlarının listesini gösterir.
10. ' A tıklayın ve tanıtım hesabı kullanıyorsanız, rapor, gelir için verileri nerede seçin veritabanında genişletin **volcanodb**.   
11. Artık, alınacak verileri içeren bir koleksiyon seçin. Tanıtım hesabı kullanıyorsanız, seçin **volcano1**.
    
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
   
    Üzerinde veri çözümleme ifadeleri (DAX işlevleri dahil olmak üzere DAX) daha fazla bilgi için lütfen [Power BI Desktop'ta DAX temel](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
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
   
    ![Azure Cosmos DB için Power BI öğreticinin tamamlandıktan sonra son Power BI Desktop raporunun ekran görüntüsü](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)
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

