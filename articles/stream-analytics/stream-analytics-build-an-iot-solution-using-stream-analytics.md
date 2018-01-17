---
title: "Akış analizi kullanarak bir IOT çözümü derleme | Microsoft Docs"
description: "Başlangıç Öğreticisi gişe senaryosu Stream Analytics IOT çözüm için"
keywords: "IOT çözüm, pencere işlevleri"
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/12/2018
ms.author: sngun
ms.openlocfilehash: cc84a34a410a750ddf2acb8f19b3bb809d269098
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Akış analizi kullanarak bir IOT çözüm oluşturma

## <a name="introduction"></a>Giriş
Bu öğreticide, Azure akış analizi verilerinizden gerçek zamanlı Öngörüler almak için nasıl kullanılacağını öğreneceksiniz. Geliştiriciler, kolayca geçmiş kayıtlarını veya iş öngörüleri türetmek için başvuru verileri ile tıklatın akışlar, günlükler ve cihaz tarafından oluşturulan olaylar gibi veri akışları birleştirebilirsiniz. Microsoft Azure üzerinde barındırılan bir tam olarak yönetilen, gerçek zamanlı akış hesaplama hizmet olarak Azure akış analizi yerleşik dayanıklılık, düşük gecikme süresi ve siz yukarı ve dakika içinde çalışan almak için ölçeklenebilirlik sağlar.

Bu öğreticiyi tamamladıktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* Azure Stream Analytics Portalı'yla hakkında bilgi edinin.
* Yapılandırın ve iş akışında dağıtın.
* Gerçek dünyadaki sorunları iyileştirir ve akış analizi sorgu dili kullanarak çözün.
* Stream Analytics güvenle kullanarak çözümleri müşterileriniz için akış geliştirin.
* Sorunları gidermek için izleme ve deneyimi günlüğü kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşullar gerekir:

* En son sürümünü [Azure PowerShell](/powershell/azure/overview)
* 2015, Visual Studio 2017 veya ücretsiz [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
* Bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/)
* Bilgisayarda yönetim ayrıcalıkları
* İndirme [TollApp.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) Microsoft İndirme Merkezi'nden
* İsteğe bağlı: Kaynak kodu TollApp olay üreteci için [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Senaryo giriş: "Hello, ücretli!"
Ücretli istasyonu ortak olguya ' dir. Bunları birçok expressways, köprüleri ve tünelleri dünya genelindeki karşılaştığınız. Her Ücretli istasyon birden çok Ücretli booths sahiptir. El ile booths Ücretli bir Görevlisi ödeme durdurun. Otomatik booths üstünde her Stand algılayıcı Ücretli Stand geçirirken, araç ön için yapıştırılmış bir RFID kartı tarar. Bu Ücretli istasyonları aracılığıyla taşıtlardan geçişini ilginç işlemleri gerçekleştirilebilir olay akışının görselleştirmek kolaydır.

![Ücretli booths adresindeki araba resmi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Gelen veriler
Bu öğretici, iki veri akışları ile çalışır. Giriş ve çıkış Ücretli istasyonları yüklü algılayıcılar ilk akış üretir. İkinci araç kayıt verileri içeren bir statik arama dataset akışıdır.

### <a name="entry-data-stream"></a>Girdi veri akışı
Ücretli istasyonları girerken giriş veri akışı araba hakkında bilgiler içerir.

| TollID | EntryTime | LicensePlate | Durum | Yapma | Model | VehicleType | VehicleWeight | Ücretli | Etiket |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |CT |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |CT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4x4 |1 |0 |6 |321987654 |

Aşağıda, sütunların kısa bir açıklaması verilmiştir:

| Sütun | Açıklama |
| --- | --- |
| TollID |Ücretli Stand benzersiz olarak tanıtan Ücretli Stand kimliği |
| EntryTime |Tarih ve saati UTC Ücretli Stand için araç girdisi |
| LicensePlate |Aracın Lisans kalıbı sayısı |
| Durum |Amerika Birleşik Devletleri bir durumda |
| Yapma |Otomobil üreticisi |
| Model |Otomobil model numarası |
| VehicleType |Yolcu taşıtlardan veya ticari araçları için 2 ya da 1 |
| WeightType |Araç ağırlık ton cinsinden; yolcu araçları için 0 |
| Ücretli |ABD Doları Ücretli değeri |
| Etiket |E-etiketinde ödeme otomatikleştirir otomobil; Ödeme el ile yapılan burada boş |

### <a name="exit-data-stream"></a>Çıkış veri akışı
Çıkış veri akışı Ücretli istasyon bırakarak araba hakkında bilgiler içerir.

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000Z |CDE 1007 |

Aşağıda, sütunların kısa bir açıklaması verilmiştir:

| Sütun | Açıklama |
| --- | --- |
| TollID |Ücretli Stand benzersiz olarak tanıtan Ücretli Stand kimliği |
| ExitTime |Aracın çıkış Ücretli Stand UTC'saat ve tarihi |
| LicensePlate |Aracın Lisans kalıbı sayısı |

### <a name="commercial-vehicle-registration-data"></a>Ticari araç kayıt verileri
Öğretici, bir statik veritabanının anlık görüntüsü bir ticari araç kayıt kullanır.

| LicensePlate | RegistrationId | Süresi Doldu |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| BAC 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Aşağıda, sütunların kısa bir açıklaması verilmiştir:

| Sütun | Açıklama |
| --- | --- |
| LicensePlate |Aracın Lisans kalıbı sayısı |
| RegistrationId |Aracın 's kayıt kimliği |
| Süresi Doldu |Aracın kayıt durumunu: araç kayıt etkinse 0 kayıt süresi 1 |

## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Azure akış analizi için ortamını ayarlama
Bu öğreticiyi tamamlamak için bir Microsoft Azure aboneliği gerekir. Microsoft, ücretsiz deneme sürümü için Microsoft Azure hizmetleri sunar.

Bir Azure hesabınız yoksa, şunları yapabilirsiniz [ücretsiz deneme sürümü isteği](http://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Ücretsiz deneme için kaydolun metin iletileri ve geçerli bir kredi kartı alabileceği bir mobil aygıtı gerekir.
> 
> 

Böylece Azure kredi en iyi kullanımı yapabilirsiniz bu makalenin sonunda "Azure hesabınızı temizleme" bölümündeki adımları takip ettiğinizden emin olun.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Öğretici için gerekli Azure kaynaklarını hazırlama
Bu öğretici almak için iki olay hub gerektirir *girişi* ve *çıkmak* veri akışları. Azure SQL veritabanı Stream Analytics işlerine sonuçlarını çıkarır. Azure depolama araç kayıtlar hakkında başvuru verileri depolar.

Tüm gerekli kaynakları oluşturmak için GitHub TollApp klasöründe Setup.ps1 komut dosyasını kullanabilirsiniz. Zaman açısından bu çalıştırmanızı öneririz. Bu kaynakları Azure portalında yapılandırma hakkında daha fazla bilgi edinmek istiyorsanız, "Azure portalında öğretici kaynaklarını yapılandırma" eke bakın.

Karşıdan yükle ve destekleyici Kaydet [TollApp](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) klasör ve dosyaları.

Açık bir **Microsoft Azure PowerShell** penceresi *yönetici olarak*. Azure PowerShell henüz yoksa,'ndaki yönergeleri izleyin [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview) yükleyin.

Windows otomatik olarak .ps1, .dll ve .exe dosyaları engellediğinden, komut dosyasını çalıştırmadan önce yürütme ilkesini ayarlamanız gerekir. Azure PowerShell penceresi çalıştığından emin olun *yönetici olarak*. Çalıştırma **Set-ExecutionPolicy unrestricted**. İstendiğinde yazın **Y**.

!["Set-Kısıtlanmamış Azure PowerShell penceresinde çalıştıran ExecutionPolicy" ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Çalıştırma **Get-ExecutionPolicy** komutu çalıştığından emin olmak için.

!["Get-ExecutionPolicy Azure PowerShell penceresinde Çalıştırma" ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Komut dosyaları ve oluşturucu uygulama sahip dizinine gidin.

!["Azure PowerShell penceresinde çalıştıran cd .\TollApp\TollApp" ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Tür **.\\ Setup.ps1** Azure hesabınızı kurmak için oluşturmak ve gerekli tüm kaynaklarını yapılandırmak ve olayları Başlat. Komut dosyasını bir bölgesini kaynaklarınızı oluşturmak için rastgele seçer. Bir bölge açıkça belirtmek için geçirebilirsiniz **-konum** aşağıdaki örnekteki gibi parametre:

**. \\Setup.ps1-konum "Orta ABD"**

![Azure oturum açma sayfasının ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Komut dosyasını açar **oturum** Microsoft Azure için sayfa. Hesap kimlik bilgilerinizi girin.

> [!NOTE]
> Birden çok abonelik hesabınıza erişimi varsa öğretici için kullanmak istediğiniz abonelik adı girmeniz istenir.
> 
> 

Komut dosyasını çalıştırmak için birkaç dakika sürebilir. Sona erdikten sonra çıkışı aşağıdaki ekran görüntüsü gibi görünmelidir.

![Azure PowerShell penceresinde komut dosyası çıkışını ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Ayrıca, aşağıdaki ekran görüntüsüne benzer başka bir pencere görürsünüz. Bu uygulamayı Azure Event öğretici çalıştırmak için gerekli olan Hubs'a, olayları gönderiyor. Bu nedenle, uygulamayı durdurun veya değil öğretici tamamlanana kadar bu pencereyi kapatın.

!["Gönderme olay hub'ı veri" ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Artık Azure portalında kaynaklarınızı görüyor olmalısınız. Git <https://portal.azure.com>ve hesabı kimlik bilgilerinizle oturum açın. Şu anda bazı işlevler Klasik Portalı'nı kullanan olduğunu unutmayın. Bu adımları açıkça gösterilir.

### <a name="azure-event-hubs"></a>Azure Event Hubs

Azure portalından tıklatın **daha fazla hizmet** sol yönetim bölmesinin üzerinde. Tür **olay hub'ları** sağlanan alana ile başlayan yeni bir olay hub'ı ad alanı görebilirsiniz **tolldata**. Bu namesapce Setup.ps1 betiği tarafından oluşturulur. Adlı iki olay hub görürsünüz **girişi** ve **çıkmak** bu ad alanında oluşturuldu.

### <a name="azure-storage-container"></a>Azure depolama kapsayıcısı
Azure portalından, depolama hesaplarınıza göz atın, ile başlayan bir depolama hesabı görmelisiniz **tolldata**. Tıklatın **tolldata** araç kayıt verileri karşıya yüklenen JSON dosyaları görmek için kapsayıcı.

### <a name="azure-sql-database"></a>Azure SQL Database
1. Tarayıcıda açılan ilk sekme Azure Portalı'na geri gidin. Tıklatın **SQL veritabanları** öğreticide kullanılır ve SQL veritabanı görmek için Azure portalının sol tarafında **tolldatadb**.
   
    ![SQL veritabanı oluşturuldu ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)
2. Sunucu adı bağlantı noktası numarası olmadan kopyalayın (*servername*. Örneğin database.windows.net).
    ![Oluşturulan SQL veritabanı db ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15a.png)

## <a name="connect-to-the-database-from-visual-studio"></a>Visual Studio'dan veritabanına bağlan
Visual Studio çıktı veritabanı sorgu sonuçlarında erişmek için kullanın.

Visual Studio'dan (hedef) SQL veritabanına bağlan:

1. Visual Studio'yu açın ve ardından **Araçları** > **veritabanına bağlan**.
2. İstenirse, tıklatın **Microsoft SQL Server** veri kaynağı olarak.
   
    ![Değişikliği veri kaynağı iletişim kutusu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)
3. İçinde **sunucu adı** alanında, önceki bölümde Azure portalından kopyalandığından adını yapıştırın (diğer bir deyişle, *servername*. database.windows.net).
4. Tıklatın **SQL Server kimlik doğrulaması kullanmak**.
5. Girin **tolladmin** içinde **kullanıcı adı** alan ve **123toll!** içinde **parola** alan.
6. Tıklatın **seçin veya bir veritabanı adı girin**seçip **TollDataDB** veritabanı olarak.
   
    ![Bağlantı Ekle iletişim kutusu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)
7. **Tamam**’a tıklayın.
8. Sunucu Gezgini'ni açın.
   
    ![Sunucu Gezgini](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)
9. TollDataDB veritabanında dört tablolara bakın.
   
    ![TollDataDB veritabanındaki tabloları](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Olay Oluşturucusu: TollApp örnek proje
PowerShell Betiği TollApp örnek uygulama programı kullanarak olayları göndermek otomatik olarak başlar. Ek adımlar gerçekleştirmeniz gerekmez.

Uygulama Ayrıntıları ilgileniyorsanız, ancak TollApp uygulamanın kaynak kodu Github'da bulabilirsiniz [samples/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Visual Studio'da görüntülenen örnek kodunu ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma
1. Azure portalında yeni bir Stream Analytics işi oluşturmak için sayfanın sol üst köşedeki yeşil artı işaretine tıklayın. Seçin **Intelligence + analiz** ve ardından **Stream Analytics işi**.
   
    ![Yeni düğmesi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)
2. Bir iş adı sağlayın, abonelik doğrulama düzeltin ve sonra olay hub'ı depolama ile aynı bölgede yeni bir kaynak grubu oluşturun (varsayılan, Orta Güney ABD komut dosyası için).
3. Tıklatın **panoya Sabitle** ve ardından **oluşturma** sayfanın sonundaki.
   
    ![Akış analizi işi seçeneği oluşturun](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Giriş kaynağı tanımlayın
1. İşi oluşturmak ve işi sayfasını açın. Veya portal panosunda oluşturulan analizi işi'ı tıklatın.

2. Tıklatın **GİRİŞLERİ** veri kaynağını tanımlamak için sekmesini.
   
    ![Giriş sekmesi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.png)
3. Tıklatın **bir giriş eklemek**.
   
    ![Bir giriş seçeneği Ekle](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)
4. Girin **EntryStream** olarak **giriş diğer adı**.
5. Kaynak türü **veri akışı**
6. Kaynak **olay hub'ı**.
7. **Hizmet veri yolu ad alanı** aşağı açılan TollData biri olmalıdır.
8. **Olay hub'ı adı** ayarlanmalı **girişi**.
9. **Olay hub'ı ilke adı*olan **RootManageSharedAccessKey** (varsayılan değer).
10. Seçin **JSON** için **OLAYI seri hale getirme biçimi** ve **UTF8** için **kodlama**.
   
    Ayarlarınızı gibi görünür:
   
    ![Olay hub'ı ayarları](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

10. Tıklatın **oluşturma** Sihirbazı tamamlamak için sayfanın altındaki.
    
    Giriş akışı oluşturduğunuza göre çıkış akışı oluşturmak için aynı adımları izleyeceksiniz. Aşağıdaki ekran görüntüsü üzerinde değerleri olarak girdiğinizden emin olun.
    
    ![Çıkış akışı için ayarları](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)
    
    İki giriş akışları tanımladınız:
    
    ![Azure Portalı'nda tanımlanan giriş akışları](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.png)
    
    Sonra başvuru veri girişi araba kayıt verileri içeren blob dosyası için ekleyeceksiniz.
11. Tıklatın **ekleme**ve ardından akış girişleri için aynı süreci izleyin ancak seçin **başvuru verileri** yerine **veri akışı** ve **giriş diğer adı** olan **kayıt**.

12. ile başlayan depolama hesabı **tolldata**. Kapsayıcı adı olmalıdır **tolldata**ve **yol DESENİ** olmalıdır **registration.json**. Bu dosya adı büyük/küçük harfe duyarlıdır ve olmalıdır **küçük**.
    
    ![Blog depolama ayarları](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)
13. Tıklatın **oluşturma** Sihirbazı tamamlamak için.

Şimdi tüm girişleri tanımlanır.

## <a name="define-output"></a>Çıktı tanımlayın
1. Akış analizi işi'ne genel bakış bölmesinde seçin **ÇIKIŞLARI**.
   
    !["Çıktı Ekle" seçeneğini ve Çıkış sekmesi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.png)
2. **Ekle**'ye tıklayın.
3. Ayarlama **çıkış diğer adları** 'output' için ve ardından **havuzu** için **SQL veritabanı**.
3. Makalenin "Bağlanmak için veritabanı gelen Visual Studio" bölümünde kullanılan sunucu adını seçin. Veritabanı adı **TollDataDB**.
4. Girin **tolladmin** içinde **kullanıcıadı** alanı **123toll!** içinde **parola** alan ve **TollDataRefJoin** içinde **tablo** alan.
   
    ![SQL veritabanı ayarları](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.png)
5. **Oluştur**’a tıklayın.

## <a name="azure-stream-analytics-query"></a>Azure Stream analytics sorgusu
**Sorgu** sekmesi gelen verileri dönüştüren bir SQL sorgusu içerir.

![Sorgu sekmesi eklenen bir sorgu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.png)

Bu öğretici, Azure akış analizi ilgili yanıt sağlamak için kullanılabilir verileri ve yapıları Stream Analytics sorgu hat ilgili birkaç iş soruları yanıtlamak çalışır.

İlk Stream Analytics işiniz başlamadan önce bazı senaryolar ve sorgu söz dizimi inceleyelim.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Azure Stream Analytics sorgu dili giriş
- - -
Ücretli Stand girin taşıtlardan sayısını gerektiğini varsayalım. Bu olayların sürekli akışı olduğundan, bir "süre." tanımlamak zorunda "Kaç taşıtlardan üç dakikada Ücretli Stand girin?" olarak soru değiştirelim. Bu genellikle dönen sayım olarak adlandırılır.

Bu soruya yanıt veren Azure akış analizi sorgu bakalım:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Gördüğünüz gibi Azure akış analizi gibi SQL ve sorgu zaman ilgili yönlerini belirlemek için birkaç uzantıları ekler bir sorgu dili kullanır.

Hakkında daha fazla ayrıntı için okuma [zaman Yönetimi](https://msdn.microsoft.com/library/azure/mt582045.aspx) ve [Pencereleme](https://msdn.microsoft.com/library/azure/dn835019.aspx) MSDN'den sorguda kullanılan yapılar.

## <a name="testing-azure-stream-analytics-queries"></a>Azure Stream Analytics sorgu test etme
İlk Azure akış analizi sorgu yazılmış, TollApp klasörü şu yol içinde yer alan örnek veri dosyalarını kullanarak test etmek için zamanı:

**..\\TollApp\\TollApp\\veri**

Bu klasör, aşağıdaki dosyaları içerir:

* Entry.json
* Exit.json
* Registration.json

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Soru 1: Ücretli Stand girme taşıtlardan sayısı
1. Azure Portalı'nı açın ve oluşturulan Azure Stream Analytics işiniz gidin. Tıklatın **sorgu** sekmesinde ve önceki bölümde sorgudan yapıştırın.

2. Bu örnek verileri sorgusu doğrulamak için veri EntryStream giriş tıklayarak yükleme... simge ve seçme **dosyasından örnek verileri yükleme**.

    ![Entry.json dosyasının ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)
3. Görünür bölmesinde yerel makinenizde (Entry.json) dosyasını seçin ve **Tamam**. **Test** simgesi artık karanl ve tıklatılabilir.
   
    ![Entry.json dosyasının ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.png)
3. Sorgu çıktısı beklendiği gibi olduğunu doğrulayın:
   
    ![Test sonuçları](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.png)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Soru 2: Rapor toplam süre, her araba Ücretli Stand geçirmek için
Bir araba Ücretli geçirmek için gerekli olan ortalama süre değerlendirmeniz işlemi ve müşteri deneyimi yardımcı olur.

Toplam süre bulmak için ExitTime akış olan EntryTime akış katılmak için gerekir. Akışlar TollId ve LicencePlate sütunlarda katılır. **Katılma** işleci birleştirilmiş olayları arasındaki kabul edilebilir zaman farkı açıklar zamana bağlı leeway belirtmenizi gerektirir. Kullanacağınız **DATEDIFF** işlevi olayları birbirinden en fazla 15 dakika gerektiğini belirtin. Ayrıca uygulanacak **DATEDIFF** çıkmak için işlevi ve gerçek zaman hesaplamak için giriş saatleri bir araba harcadığı Ücretli istasyonu. Kullanımı farkı Not **DATEDIFF** içinde kullanıldığında bir **seçin** deyimi yerine **katılma** koşulu.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Bu sorguyu sınamak için üzerinde sorguyu güncelleştirmek **sorgu** iş için. İçin test dosyası ekleme **ExitStream** olduğu gibi **EntryStream** yukarıda girildi.
   
2. Tıklatın **Test**.

3. Sorguyu sınamak ve çıktı görüntülemek için onay kutusunu seçin:
   
    ![Test çıkışı](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Soru 3: süresi dolan kayıt ile tüm ticari araçları rapor
Azure Stream Analytics, zamana bağlı veri akışları ile katılmaya verilerinin statik anlık görüntülerini kullanabilirsiniz. Bu özelliği tanıtmak için aşağıdaki örnek soru kullanın.

Bir ticari araç Ücretli şirketle kaydedilmişse Ücretli Stand İnceleme için durdurulmadan geçirebilirsiniz. Ticari araç kayıt arama tablosu kayıtlar süresi dolmuş tüm ticari araçları tanımlamak için kullanır.

```
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

Başvuru verileri kullanarak bir sorguyu sınamak için bir giriş kaynağı yapmış olduğunuz başvuru verileri için tanımlamanız gerekir.

Bu sorguyu sınamak için Query'ye yapıştırın **sorgu** sekmesini tıklatın, **Test**, iki giriş kaynağı ve kayıt örnek veri ve tıklatın belirtin **Test**.  
   
![Test çıkışı](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

## <a name="start-the-stream-analytics-job"></a>Stream Analytics işi Başlat
Şimdi yapılandırmayı tamamlamak ve iş başlangıç zamanı geldi. Soru 3 sorgusu, hangi şeması eşleşen bir çıktı oluşturacak **TollDataRefJoin** çıktı tablosu.

Proje Git **PANO**, tıklatıp **Başlat**.

![Proje panosunda Başlat düğmesinin Ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.png)

Açılan iletişim kutusunda, değiştirmek **Başlat çıktı** için zaman **özel zaman**. Önce geçerli saati bir saat için saat değiştirin. Bu değişiklik, öğreticiyi başında olaylar üretmek başlatıldığından bu yana olay hub'ı tüm olayları işlenir sağlar. Şimdi **Başlat** iş başlamak için Başlat.

![Özel süre seçimi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

İşin başlangıç birkaç dakika sürebilir. Akış analizi için üst düzey sayfasında durumunu görebilirsiniz.

![İşin durumunun ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.png)

## <a name="check-results-in-visual-studio"></a>Visual Studio'da denetim sonuçları
1. Visual Studio sunucu Gezgini'ni açın ve sağ **TollDataRefJoin** tablo.
2. Tıklatın **Show Table Data** işinizi çıkışını görmek için.
   
    ![Sunucu Gezgini içinde "Tablo verileri göster" seçimi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Out Azure Stream Analytics işlerini ölçeklendirme
Azure Stream Analytics özellikler esnek ölçek çok miktarda veri işleyebilecek şekilde tasarlanmıştır. Azure Stream Analytics sorgu kullanabileceğiniz bir **bölüm tarafından** sistem bu adımı çıkışı ölçeklenir bildirmek için yan tümcesi. **PartitionID** (olay hub) giriş bölüm Kimliğini eşleştirilecek sistemi ekleyen özel bir sütundur.

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Geçerli işi durdurma, sorguda güncelleştirme **sorgu** sekmesini tıklatın ve açmak **ayarları** gear proje panosunda. Tıklatın **ölçek**.
   
    **Akış birimleri** iş alabilir işlem gücü miktarına tanımlayın.
2. 1. 6'dan açılan değiştirin.
   
    ![6 akış birimleri seçme ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.png)
3. Git **ÇIKIŞLARI** sekmesinde ve SQL tablo adını değiştirin **TollDataTumblingCountPartitioned**.

İş başlatırsanız şimdi, Azure akış analizi iş üzerinde daha fazla işlem kaynağı dağıtabilir ve daha iyi verim elde etmek. Lütfen TollApp uygulama tarafından TollId bölümlenmiş olayları gönderiyor unutmayın.

## <a name="monitor"></a>İzleme
**İZLEYİCİ** alanı içeren çalışan iş hakkındaki istatistiklerdir. Yapılandırma depolama kullanmak için gereken ilk kez hesabı ile aynı bölgede (Bu belgenin kalan gibi ad Ücretli).   

![İzleyici'nin ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

Erişebileceğiniz **etkinlik günlükleri** işi panodan **ayarları** de alanı.


## <a name="conclusion"></a>Sonuç
Bu öğretici Azure akış analizi hizmetine kullanıma sunuldu. Girişleri ve çıkışları Stream Analytics işi için nasıl yapılandırılacağı gösterilmektedir. Ücretli veri senaryoyu kullanarak, öğretici Azure akış analizi basit SQL benzeri sorguları hareket ve bunların nasıl çözülebilir verilerle alanındaki çıkan sorunları genel türleri açıklanmıştır. Öğretici, zamana bağlı verilerle çalışmak için SQL uzantısı yapıları açıklanmaktadır. Veri akışları nasıl, statik başvuru verileri ile veri akışı zenginleştirmek nasıl ve daha yüksek verimlilik elde etmek için bir sorgu ölçeklendirmek nasıl oluşturulacağını gösterir.

Bu öğretici bir iyi giriş sağlasa da, herhangi bir yolla tam değil. Daha fazla sorgu desenlerine SAQL dil desteğini kullanarak bulabilirsiniz [sorgu örnekler için ortak Stream Analytics kullanım desenlerini](stream-analytics-stream-analytics-query-patterns.md).
Başvurmak [çevrimiçi belgeleri](https://azure.microsoft.com/documentation/services/stream-analytics/) Azure Stream Analytics hakkında daha fazla bilgi edinmek için.

## <a name="clean-up-your-azure-account"></a>Azure hesabınızı Temizle
1. Azure portalında Stream Analytics işi durdurun.
   
    Setup.ps1 komut dosyası, iki olay hub'ları ve bir SQL veritabanı oluşturur. Aşağıdaki yönergeler temiz kaynakları öğreticinin sonunda yardımcı olur.
2. Bir PowerShell penceresinde yazın **.\\ CleanUp.ps1** öğreticide kullanılan kaynakları siler komut dosyasını başlatmak için.
   
   > [!NOTE]
   > Kaynakları adına göre tanımlanır. Dikkatle kaldırma onaylamadan önce her bir öğeyi gözden emin olun.
   > 
   > 


