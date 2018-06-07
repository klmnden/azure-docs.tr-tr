---
title: Azure Cosmos DB bağlayıcı için Power BI Öğreticisi | Microsoft Docs
description: JSON almak için ayrıntılı raporlar oluşturun ve Azure Cosmos DB ve Power BI Bağlayıcısı'nı kullanarak verileri Görselleştir için bu Power BI öğreticiyi kullanın.
keywords: power BI öğretici, verileri, power BI Bağlayıcısı görselleştirin
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: sngun
ms.openlocfilehash: 67ea7a9ea1a1be4fd0780f8b8ce22f1a133615e0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34615878"
---
# <a name="power-bi-tutorial-for-azure-cosmos-db-visualize-data-using-the-power-bi-connector"></a>Azure Cosmos DB için Power BI Öğreticisi: Power BI Bağlayıcısı'nı kullanarak verileri Görselleştir
[Powerbı.com adresinde](https://powerbi.microsoft.com/) nerede oluşturmak ve paylaşmak için kullanabileceğiniz panolar ve raporlar ve kuruluşunuz için önemli olan verileri ile çevrimiçi bir hizmettir.  Power BI Desktop yazma çeşitli veri kaynaklarından veri almak, birleştirme ve verileri dönüştürmek, güçlü raporları ve görselleştirmeleri oluşturmak ve Power BI raporları yayınlamak olanak tanıyan aracı adanmış bir rapordur.  Power BI Desktop en son sürümü ile artık Azure Cosmos DB bağlayıcısı aracılığıyla Azure Cosmos DB hesabınız için Power BI bağlanabilirsiniz.   

Bu Power BI öğreticide biz Power BI Desktop'ta Azure Cosmos DB hesabınıza bağlanın, biz Gezgin kullanarak verileri ayıklamak istediğiniz yere bir koleksiyona gidin, JSON verilerini Power BI Desktop sorgu Düzenleyicisi'ni kullanarak tablo biçimine dönüştürmek için izleyeceğiniz adımlarda size yol , derleme ve bir raporu Powerbi.com'da yayımlayın.

Bu Power BI öğreticiyi tamamladıktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:  

* Nasıl ı veri raporlarla Azure Cosmos DB'den Power BI Desktop kullanarak oluşturabilir miyim?
* Power BI Desktop'ta Azure Cosmos DB hesabına nasıl bağlanabilir miyim?
* Nasıl Power BI Desktop'ta koleksiyonundan ı verileri alabilir?
* İç içe geçmiş JSON verilerini Power BI Desktop'ta nasıl dönüştürme?
* Nasıl yayımlama ve paylaşma raporlarım powerbı.com'da?

> [!NOTE]
> Azure Cosmos DB için Power BI Bağlayıcısı ayıklama ve veri dönüştürme için Power BI Desktop bağlanır. Power BI Desktop'ta oluşturulan raporlar sonra Powerbi.com'da yayımlanabilir. Powerbi.com'u doğrudan ayıklama ve Azure Cosmos DB veri dönüştürme gerçekleştirilemiyor. 

> [!NOTE]
> Power BI MongoDB API kullanarak Azure Cosmos DB bağlanmak için kullanmanız gerekir [Simba MongoDB ODBC sürücüsü](http://www.simba.com/drivers/mongodb-odbc-jdbc/).

## <a name="prerequisites"></a>Önkoşullar
Bu Power BI öğreticide yönergeleri izlemeden önce aşağıdaki kaynaklara erişimi olduğundan emin olun:

* [Power BI Desktop'ın en son sürümünü](https://powerbi.microsoft.com/desktop).
* Bizim demo hesabını ve Azure Cosmos DB hesabınızdaki verilere erişemezsiniz.
  * Demo hesabını, bu öğreticide gösterilen volcano verilerle doldurulur. Bu demo hesap tarafından herhangi bir SLA bağlı değil ve yalnızca tanıtım amacıyla tasarlanmıştır.  Biz bu demo hesabı da dahil olmak üzere değişiklikleri yapma hakkı yedek ancak bunlarla sınırlı olmamak hesap sonlandırma anahtarının değiştirilmesi, değiştirme, erişimi kısıtlamak için ve verileri önceden veya neden olmadan herhangi bir zamanda silin.
    * URL'Sİ: https://analytics.documents.azure.com
    * Salt okunur anahtar: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR + YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw ==
  * Veya, kendi hesabınızı oluşturmak için bkz: [Azure portalını kullanarak bir Azure Cosmos DB veritabanı hesabı oluşturma](https://azure.microsoft.com/documentation/articles/create-account/). Ardından, örnek volcano almak için ne benzer veri Bu öğreticide kullanılan (ancak GeoJSON blokları içermiyor), bkz: [NOAA site](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) ve ardından verileri kullanarak içeri [Azure Cosmos DB veri geçiş aracı](import-data.md).

Powerbı.com'daki raporlarınızı paylaşmak için Powerbi.com'u bir hesabınızın olması gerekir.  Ücretsiz ve Power BI Pro için Power BI hakkında daha fazla bilgi için [ https://powerbi.microsoft.com/pricing ](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Başlayalım
Bu öğreticide, şimdi dünyanın volkanlar kavramlarını geologist olduğunuzu varsayalım.  Bir Azure Cosmos DB hesabında volcano veriler ve aşağıdaki örnek belge gibi JSON belgeleri bakın.

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

Azure Cosmos DB hesabından volcano verileri almak ve bir etkileşimli Power BI raporu aşağıdaki rapor gibi verileri görselleştirmek istiyor.

![Power BI Bağlayıcısı ile bu Power BI öğreticiyi izleyerek, Power BI Desktop volcano raporuyla görselleştirmek kullanabileceksiniz](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Bir denemelisiniz hazır mısınız? Haydi başlayalım.

1. Power BI Desktop iş istasyonunuza çalıştırın.
2. Power BI Desktop başlatıldıktan sonra bir *Hoş Geldiniz* ekranı görüntülenir.
   
    ![Power BI Desktop Hoş Geldiniz ekranı - Power BI Bağlayıcısı](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. Yapabilecekleriniz **Veri Al**, bkz: **son kaynakları**, veya **açık diğer rapor** doğrudan *Hoş Geldiniz* ekran.  Ekranı kapatmak için sağ üst köşedeki X'i tıklatın. **Rapor** Power BI Desktop görünümü görüntülenir.
   
    ![Power BI Desktop rapor görünümü - Power BI Bağlayıcısı](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Seçin **giriş** Şerit sonra tıklayın **Veri Al**.  **Veri Al** penceresi görünmelidir.
5. Tıklayın **Azure**seçin **Azure Cosmos DB (Beta)** ve ardından **Bağlan**. 

    ![Power BI Desktop, Power BI Bağlayıcısı - veri alma](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. Üzerinde **Önizleme bağlayıcı** sayfasında, **devam**. **Azure Cosmos DB** penceresi görüntülenir.
7. Aşağıda gösterildiği gibi verileri almak ister ve ardından Azure Cosmos DB hesap uç noktasının URL'sini belirtin **Tamam**. Kendi hesabınızı kullanmak için URL URI kutusundan alabilirsiniz **[anahtarları](manage-account.md#keys)** Azure portalı dikey. Demo hesabını kullanmak için girin `https://analytics.documents.azure.com` URL. 
   
    Bu alanlar isteğe bağlı olarak veritabanı adı, koleksiyon adı ve SQL deyimi boş bırakın.  Bunun yerine, verileri nereden geldiğini tanımlamak için veritabanı ve koleksiyonu seçmek için Gezgin kullanacağız.
   
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - masaüstü penceresinde bağlanmak için](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. Bu uç noktaya ilk kez bağlanıyorsanız hesap anahtarı için istenir. Kendi hesabınızı için anahtarından almak **birincil anahtar** kutusunda **[salt okunur anahtarları](manage-account.md#keys)** Azure portalı dikey. Gösteri, anahtar hesabıdır `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Uygun anahtarını girin ve ardından **Bağlan**.
   
    Raporları oluştururken salt okunur anahtarı kullanmanızı öneririz.  Bu, olası güvenlik risklerini ana anahtarın nızı gereksiz ihlallerden engeller. Salt okunur anahtar kullanılabilir [anahtarları](manage-account.md#keys) Azure portal veya dikey penceresinde, yukarıda verilen demo hesap bilgileri kullanabilirsiniz.
   
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - hesap anahtarı](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > "Belirtilen veritabanı bulunamadı." diyen bir hata iletisi alırsanız geçici çözüm bu adımlarına bakın [Power BI sorunu](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. Hesap başarıyla bağlandığında **Gezgini** bölmesinde görünür.  **Gezgini** hesabı altında veritabanlarının bir listesini gösterir.
10. ' I tıklatın ve veri demo hesabını kullanıyorsanız, rapor, geldiği için select where veritabanını genişletin **volcanodb**.   
11. Şimdi, almak için gerekli verileri içeren bir koleksiyon seçin. Demo hesabını kullanıyorsanız, seçin **volcano1**.
    
    Önizleme bölmesinde bir listesini gösterir **kaydı** öğeleri.  Bir belge olarak temsil edilen bir **kaydı** Power bı'da türü. Benzer şekilde, bir belge içindeki iç içe geçmiş bir JSON blok da olan bir **kaydı**.
    
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - Gezgini penceresi](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Tıklatın **Düzenle** verileri dönüştürmek için sorgu Düzenleyicisi'nde yeni bir pencere başlatmak için.

## <a name="flattening-and-transforming-json-documents"></a>Düzleştirme ve JSON belgelerini dönüştürme
1. Geçiş için Power BI sorgu Düzenleyicisi penceresini, burada **belge** Orta bölmede sütun.
   ![Power BI Desktop sorgu Düzenleyicisi](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Genişletici sağ tarafında tıklayın **belge** sütun başlığı.  Bağlam menüsü ile bir alanlar listesi görüntülenir.  Raporunuz için gereksinim duyduğunuz alanları örneği için Volcano adı, ülke, bölge, konum, yükseltme, türü, durumu ve son bilmeniz patlama seçin. İşaretini **önek olarak orijinal sütun adını kullan** kutusuna ve ardından **Tamam**.
   
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı için - belgeleri genişletin](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. Orta bölmede seçili alanlarla sonuç önizlemesini görüntülenir.
   
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı için - sonuçları düzleştirme](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. Bizim örneğimizde, bir belge GeoJSON bloğunda konumu özelliğidir.  Gördüğünüz gibi konumu olarak temsil edilen bir **kaydı** Power BI Desktop'ta türü.  
5. Genişletici Document.Location sütun başlığını sağ tarafındaki'i tıklatın.  Tür ve koordinatları alanları ile bağlam menüsü görüntülenir.  Şimdi koordinatları alanı seçin, olun **önek olarak orijinal sütun adını kullan** seçili değilse ve tıklatın **Tamam**.
   
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - konumu kaydı](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. Orta bölmede şimdi koordinatları sütunu gösterir **listesi** türü.  Öğreticinin başında gösterildiği gibi Bu öğreticide GeoJSON veri noktası koordinatları dizisinde kaydedilen enlem ve boylam değerlerle türüdür.
   
    [1] koordinatları temsil ederken, enlem koordinatları [0] öğesi boylam temsil eder.
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - koordinatları listesi](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. Koordinatları dizi düzleştirmek için oluşturma bir **özel sütun** LatLong çağrılır.  Seçin **Sütun Ekle** Şerit ve tıklayın **özel sütun**.  **Özel sütun** penceresi görüntülenir.
8. Yeni bir sütun, örneğin LatLong için bir ad sağlayın.
9. Ardından, yeni bir sütun için özel formülü belirtin.  Bizim örneğimizde, biz aşağıdaki formül kullanılarak aşağıda gösterildiği gibi virgülle ayrılmış enlem ve boylam değerlerini birleştirme: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. **Tamam**’a tıklayın.
   
    Üzerinde veri analizi ifadeleri (DAX işlevleri dahil olmak üzere DAX) daha fazla bilgi için lütfen ziyaret [DAX temel Power BI Desktop'ta](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - özel sütun eklemek için](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. Şimdi, Orta bölmede değerlerle doldurulan yeni LatLong sütunları gösterir.
    
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - özel LatLong sütun için](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Yeni bir sütun bir hata alırsanız, sorgu ayarları altında uygulanan adımlar aşağıdaki şekilde eşleştiğinden emin olun:
    
    ![Uygulanan adımlar olması gereken kaynak, gezinti, genişletilmiş belgenin, genişletilmiş Document.Location, eklenen özel](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Adımlarınızı farklıysa, ek adımlar silin ve özel sütun yeniden eklemeyi deneyin. 

11. Tıklatın **kapatın ve geçerli** veri modeli kaydetmek için.
    
    ![Power BI öğretici Azure Cosmos DB Power BI Bağlayıcısı - Kapat & Uygula](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>Raporları oluşturma
Power BI Desktop rapor görünümü verileri görselleştirmek için raporları oluşturma başlayabileceğiniz ' dir.  Sürükleme ve bırakma alanlarına göre raporlar oluşturabilirsiniz **rapor** tuvale.

![Power BI Desktop rapor görünümü - Power BI Bağlayıcısı](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

Rapor görünümünde, bulmanız gerekir:

1. **Alanları** bölmesinde, veri modelleri raporlarınız için kullanabileceğiniz alanlarla listesini görebileceğiniz budur.
2. **Görselleştirmeleri** bölmesi. Bir raporu, tek bir veya birden çok görselleştirmeleri içerebilir.  Gereksinimlerinize gelen sığdırma visual türleri çekme **görselleştirmeleri** bölmesi.
3. **Rapor** tuvale, burada, rapor için görsel yapı budur.
4. **Rapor** sayfası. Power BI Desktop'ta birden çok rapor sayfaları ekleyebilirsiniz.

Aşağıdaki basit bir etkileşimli harita görünümü rapor oluşturma temel adımları gösterir.

1. Bizim örneğimizde, her volcano konumunu gösteren bir harita görünüm oluşturacağız.  İçinde **görselleştirmeleri** bölmesinde, yukarıdaki ekran görüntüsünde vurgulanan harita görsel türü'ı tıklatın.  Üzerinde boyandığında harita görsel türü görmelisiniz **rapor** tuvale.  **Görselleştirme** bölmesinde harita visual türüyle ilişkili özellikler kümesi de görüntülenmelidir.
2. LatLong alanından şimdi, sürükleyip **alanları** bölmesine **konumu** özelliğinde **görselleştirmeleri** bölmesi.
3. Ardından, sürüklenip Volcano ad alanına **gösterge** özelliği.  
4. Yükseltme alanına daha sonra sürükleyip **boyutu** özelliği.  
5. Harita görmelisiniz visual balonları volcano ayrıcalıkların bağıntı Kabarcık boyutunu her volcano konumla belirten bir dizi gösterme.
6. Temel bir rapor oluşturdunuz.  Daha fazla görselleştirmeleri ekleyerek daha ayrıntılı raporu özelleştirebilirsiniz.  Örneğimizde, raporun etkileşimli yapmak için Volcano türü dilimleyici eklendi.  
   
    ![Azure Cosmos DB için Power BI öğreticinin tamamlandıktan sonra son Power BI Desktop raporunun ekran görüntüsü](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)
7. Dosya menüsünde **kaydetmek** ve dosyayı PowerBITutorial.pbix kaydedin.

## <a name="publish-and-share-your-report"></a>Yayımlama ve raporunuzu paylaşma
Raporunuzu paylaşmak için Powerbi.com'u bir hesabınızın olması gerekir.

1. Power BI Desktop'ta tıklayın **giriş** Şerit.
2. **Yayımla**’ta tıklayın.  Olması istenir PowerBI.com hesabınız için kullanıcı adını ve parolasını girin.
3. Kimlik bilgileri doğrulandıktan sonra raporu seçtiğiniz Hedefinizi yayımlanır.
4. Tıklatın **Power bı'da Aç 'PowerBITutorial.pbix'** görüp raporunuzda PowerBI.com paylaşın.
   
    ![Power BI başarı için yayımlama! Power bı'da Aç Öğreticisi](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Powerbi.com'u bir pano oluşturun
Bir rapor sahip olduğunuza göre işlem Powerbı.com adresinde paylaşmak olanak tanır

Raporunuza Power BI masaüstünden PowerBI.com yayımladığınızda, bunu oluşturan bir **rapor** ve **Dataset** PowerBI.com kiracınızda. Örneğin, sonra yayımladığınız adlı bir raporu **PowerBITutorial** Powerbı.com adresine PowerBITutorial hem de göreceğiniz **raporları** ve **veri kümeleri** PowerBI.com bölümleri.

   ![Yeni rapor ve veri kümesi powerbı.com'da ekran görüntüsü](./media/powerbi-visualize/powerbi-reports-datasets.png)

Paylaşılabilir bir Pano oluşturmak için tıklatın **PIN Canlı sayfa** PowerBI.com raporunuzu düğmesinde.

   ![Yeni rapor ve veri kümesi powerbı.com'da ekran görüntüsü](./media/powerbi-visualize/power-bi-pin-live-tile.png)

' Ndaki yönergeleri izleyin [bir raporundan bir kutucuğu sabitleyin](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) yeni bir Pano oluşturmak için. 

Bir Pano oluşturmadan önce rapor geçici değişiklikler da yapabilirsiniz. Ancak, değişiklikler yapabilir ve PowerBI.com raporu yeniden yayımlamanız için Power BI Desktop kullanmanız önerilir.

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

