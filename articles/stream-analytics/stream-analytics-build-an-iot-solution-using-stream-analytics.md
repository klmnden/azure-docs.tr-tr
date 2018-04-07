---
title: Azure akış analizi kullanarak bir IOT çözüm oluşturma
description: Başlangıç Öğreticisi gişe senaryosu Stream Analytics IOT çözüm için
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh, sngun
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/21/2018
ms.openlocfilehash: 80e287d09fdc5ab7157b9ee46bc830fd2db4d501
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Akış analizi kullanarak bir IOT çözüm oluşturma

## <a name="introduction"></a>Giriş
Bu çözümde, Azure akış analizi verilerinizden gerçek zamanlı Öngörüler almak için nasıl kullanılacağını öğrenin. Geliştiriciler, kolayca geçmiş kayıtlarını veya iş öngörüleri türetmek için başvuru verileri ile tıklatın akışlar, günlükler ve cihaz tarafından oluşturulan olaylar gibi veri akışları birleştirebilirsiniz. Microsoft Azure üzerinde barındırılan bir tam olarak yönetilen, gerçek zamanlı akış hesaplama hizmet olarak Azure akış analizi yerleşik dayanıklılık, düşük gecikme süresi ve siz yukarı ve dakika içinde çalışan almak için ölçeklenebilirlik sağlar.

Bu çözüm tamamladıktan sonra yapabileceksiniz:

* Azure Stream Analytics Portalı'yla hakkında bilgi edinin.
* Yapılandırın ve iş akışında dağıtın.
* Gerçek dünyadaki sorunları iyileştirir ve akış analizi sorgu dili kullanarak çözün.
* Stream Analytics güvenle kullanarak çözümleri müşterileriniz için akış geliştirin.
* Sorunları gidermek için izleme ve deneyimi günlüğü kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu çözüm tamamlamak için aşağıdaki önkoşullar gerekir:
* Bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-introduction-hello-toll"></a>Senaryo giriş: "Hello, ücretli!"
Ücretli istasyonu ortak olguya ' dir. Bunları birçok expressways, köprüleri ve tünelleri dünya genelindeki karşılaştığınız. Her Ücretli istasyon birden çok Ücretli booths sahiptir. El ile booths Ücretli bir Görevlisi ödeme durdurun. Otomatik booths üstünde her Stand algılayıcı Ücretli Stand geçirirken, araç ön için yapıştırılmış bir RFID kartı tarar. Bu Ücretli istasyonları aracılığıyla taşıtlardan geçişini ilginç işlemleri gerçekleştirilebilir olay akışının görselleştirmek kolaydır.

![Ücretli booths adresindeki araba resmi](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Gelen veriler
Bu çözüm iki veri akışları ile çalışır. Giriş ve çıkış Ücretli istasyonları yüklü algılayıcılar ilk akış üretir. İkinci araç kayıt verileri içeren bir statik arama dataset akışıdır.

### <a name="entry-data-stream"></a>Girdi veri akışı
Ücretli istasyonları girerken giriş veri akışı araba hakkında bilgiler içerir. Çıkış veri örnek uygulamasında bulunan bir Web uygulamasından bir olay hub'ı kuyruğuna akışı Canlı olaylardır.

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
Çıkış veri akışı Ücretli istasyon bırakarak araba hakkında bilgiler içerir. Çıkış veri örnek uygulamasında bulunan bir Web uygulamasından bir olay hub'ı kuyruğuna akışı Canlı olaylardır.

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
Çözüm ticari araç kayıt veritabanı statik bir anlık görüntü kullanır. Bu veriler örnek dahil, Azure blob depolama alanına bir JSON dosyası olarak kaydedilir.

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
Bu çözüm tamamlamak için bir Microsoft Azure aboneliği gerekir. Bir Azure hesabınız yoksa, şunları yapabilirsiniz [ücretsiz deneme sürümü isteği](http://azure.microsoft.com/pricing/free-trial/).

Böylece Azure kredi en iyi kullanımı yapabilirsiniz bu makalenin sonunda "Azure hesabınızı temizleme" bölümündeki adımları takip ettiğinizden emin olun.

## <a name="deploy-the-sample"></a>Örnek dağıtma 
Birkaç tıklama ile birlikte bir kaynak grubunda kolayca dağıtılabilir çeşitli kaynaklar vardır. Çözüm tanımı github deposunda barındırılan [ https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp ](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp).

### <a name="deploy-the-tollapp-template-in-the-azure-portal"></a>Azure portalında TollApp şablonu dağıtma
1. TollApp ortam Azure'a dağıtmak için bu bağlantıyı kullanmak [TollApp Azure şablon dağıtımı](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-stream-analytics%2Fmaster%2FSamples%2FTollApp%2FVSProjects%2FTollAppDeployment%2Fazuredeploy.json).

2. İstenirse Azure portalında oturum açın.

3. İçinde çeşitli kaynakları faturalandırılır abonelik seçin.

4. Yeni bir kaynak grubu ile benzersiz bir ad, örneğin belirtin `MyTollBooth`. 

5. Bir Azure konumu seçin.

6. Belirtin bir **aralığı** saniye sayısı. Bu değer, örnek web uygulamasında nasıl sık olay Hub'ına veri göndermek kullanılır. 

7. **Denetleme** hüküm ve koşulları kabul edin.

8. Seçin **panoya Sabitle** böylece kaynakları daha sonra kolayca bulun.

9. Seçin **satın alma** örnek şablonu dağıtmak için.

10. Birkaç dakika sonra onaylamak için bir bildirim görüntülenir **dağıtım başarılı**.

### <a name="review-the-azure-stream-analytics-tollapp-resources"></a>Azure Stream Analytics TollApp kaynakları gözden geçirin
1. Azure portalında oturum açma

2. Önceki bölümde adlı kaynak grubunu bulun.

3. Aşağıdaki kaynaklar kaynak grubunda listelendiğini doğrulayın:
   - Bir Cosmos DB hesabı
   - Bir Azure akış analizi işi
   - Bir Azure depolama hesabı
   - Bir Azure olay hub'ı
   - Two Web Apps

## <a name="examine-the-sample-tollapp-job"></a>Örnek TollApp iş inceleyin 
1. Önceki bölümde kaynak grubunda başlayarak seçin adı ile başlayan Stream Analytics akış işi **tollapp** (adının benzersizliğini rastgele karakterler içeriyor).

2. Üzerinde **genel bakış** sayfası dikkat edin işin **sorgu** kutusunu sorgu sözdizimini görüntülemek için.

   ```sql
   SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
   INTO CosmosDB
   FROM EntryStream TIMESTAMP BY EntryTime
   GROUP BY TUMBLINGWINDOW(minute, 3), TollId
   ```

   Sorgu amacı paraphrase için ücretli Stand girin taşıtlardan sayısını gerektiğini varsayalım. Otoyol Ücretli Stand girme taşıtlardan sürekli akışı olduğundan, hiçbir zaman durdurur bir akışa giriş olayları benzer izinlerdir. Bir "süre" tanımlamak zorunda akış ölçmek için üzerinden ölçmek için. "Kaç taşıtlardan Ücretli Stand üç dakikada için enter?" sorusu Ayrıca, şimdi daraltın Bu genellikle dönen sayım olarak adlandırılır.

   Gördüğünüz gibi Azure akış analizi gibi SQL ve sorgu zaman ilgili yönlerini belirlemek için birkaç uzantıları ekler bir sorgu dili kullanır.  Hakkında daha fazla ayrıntı için okuma [zaman Yönetimi](https://msdn.microsoft.com/library/azure/mt582045.aspx) ve [Pencereleme](https://msdn.microsoft.com/library/azure/dn835019.aspx) sorguda kullanılan yapılar.

3. TollApp örnek iş girişleri inceleyin. Yalnızca EntryStream giriş geçerli sorgusunda kullanılır.
   - **EntryStream** girdidir, bir araba girer bir gişe Otoyol üzerinde her zaman temsil eden veri kuyruklar bir Event Hub bağlantısı. Olaylar örnek bölümü olan bir web uygulaması oluşturma ve bu verileri bu Event Hub'ında sıraya alınmış. Bu giriş akış sorgusunun FROM yan tümcesinde sorgulanır unutmayın.
   - **ExitStream** girdidir, bir araba çıkar bir gişe Otoyol üzerinde her zaman temsil eden veri kuyruklar bir Event Hub bağlantısı. Bu akış girişi sorgu söz dizimi sonraki varyasyonları kullanılır.
   - **Kayıt** giriş gerekirse aramalar için kullanılan bir statik registration.json dosyasına işaret eden bir Azure Blob Depolama bağlantısı olabilir. Bu başvuru veri girişi sorgu söz dizimi sonraki varyasyonları kullanılır.

4. TollApp örnek iş çıktısı inceleyin.
   - **Cosmos DB** çıkış çıkış havuzu olayları alan bir derlemesidir Cosmos veritabanı. Bu çıktı, akış sorgu yan TÜMCESİNE kullanılır.

## <a name="start-the-tollapp-streaming-job"></a>TollApp akış işi Başlat
İş akışında başlatmak için aşağıdaki adımları izleyin:

1. Üzerinde **genel bakış** sayfası seçin işin **Başlat**.

2. Üzerinde **başlangıç işi** bölmesinde, **şimdi**.

3. İş, üzerinde çalışmaya başladıktan sonra birkaç dakika sonra **genel bakış** iş akışında, görünüm sayfası **izleme** grafik. Grafik birkaç bin giriş olaylarını ve çıkış olaylarındaki onlarca göstermelidir.

## <a name="review-the-cosmosdb-output-data"></a>CosmosDB çıktı verileri gözden geçirin
1. TollApp kaynakları içeren kaynak grubunu bulun.

2. Ad deseni Azure Cosmos DB hesabıyla seçin **tollapp<random>-cosmos**.

3. Seçin **Veri Gezgini** başlık Veri Gezgini sayfasını açın.

4. Genişletme **tollAppDatabase** > **tollAppCollection** > **belgeleri**.

5. Çıktı olarak kullanılabilir olduğunda kimlikleri listesinde birkaç belgeleri gösterilir.

6. JSON belgesini gözden geçirmek için her kimliği seçin. Her tollid windowend fark zaman ve o penceresinden araba sayısı.

7. Ek üç bir dakika sonra başka bir dört belge kümesini, kullanılabilir tollid başına tek bir belge. 


## <a name="report-total-time-for-each-car"></a>Her araba toplam süresi raporu
Bir araba Ücretli geçirmek için gerekli olan ortalama süre değerlendirmeniz işlemi ve müşteri deneyimi yardımcı olur.

Toplam süre bulmak için ExitTime akış olan EntryTime akış katılın. İki giriş akışları sütunlarda eşit eşleşen TollId ve LicencePlate katılın. **Katılma** işleci birleştirilmiş olayları arasındaki kabul edilebilir zaman farkı açıklar zamana bağlı leeway belirtmenizi gerektirir. Kullanım **DATEDIFF** işlevi olayları birbirinden en fazla 15 dakika gerektiğini belirtin. Ayrıca uygulama **DATEDIFF** çıkmak için işlevi ve gerçek zaman hesaplamak için giriş saatleri bir araba harcadığı Ücretli istasyonu. Kullanımı farkı Not **DATEDIFF** içinde kullanıldığında bir **seçin** deyimi yerine **katılma** koşulu.

```sql
SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute, EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN ExitStream TIMESTAMP BY ExitTime
ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15
```

### <a name="to-update-the-tollapp-streaming-job-query-syntax"></a>İş sorgu sözdizimi akış TollApp güncelleştirmek için:

1. Üzerinde **genel bakış** sayfası seçin işin **durdurmak**.

2. İş durduruldu bildirim için birkaç dakika bekleyin.

3. İş TOPOLOJİSİ başlığı altında seçin **< > sorgu**

4. Ayarlanan akış SQL sorgusu yapıştırın.

5. Seçin **kaydetmek** sorguyu kaydetmek için. Onayla **Evet** değişiklikleri kaydedin.

6. Üzerinde **genel bakış** sayfası seçin işin **Başlat**.

7. Üzerinde **başlangıç işi** bölmesinde, **şimdi**.

### <a name="review-the-total-time-in-the-output"></a>Toplam süre çıkışı gözden geçirin
İş akışında CosmosDB çıktı verilerini gözden geçirmek için önceki bölümdeki adımları yineleyin. En son JSON belgelerini gözden geçirin. 

Örneğin, bu belgede bir örnek araba belirli bir lisans kalıbı, entrytime ve çıkış zamanı ve Ücretli Stand süresi iki dakika olarak gösteren DATEDIFF hesaplanan dakika cinsiden süre alanı gösterilmektedir: 
```JSON
{
    "tollid": 4,
    "entrytime": "2018-04-05T06:51:39.0491173Z",
    "exittime": "2018-04-05T06:53:09.0491173Z",
    "licenseplate": "JVR 9425",
    "durationinminutes": 2,
    "id": "ff52eb25-d580-7566-2879-1f52bba6601e",
    "_rid": "+8E4AI1DZgBjAAAAAAAAAA==",
    "_self": "dbs/+8E4AA==/colls/+8E4AI1DZgA=/docs/+8E4AI1DZgBjAAAAAAAAAA==/",
    "_etag": "\"ad02f6b8-0000-0000-0000-5ac5c8330000\"",
    "_attachments": "attachments/",
    "_ts": 1522911283
}
```

## <a name="report-vehicles-with-expired-registration"></a>Süresi dolan kayıt ile rapor araçları
Azure Stream Analytics, zamana bağlı veri akışları ile katılmak için başvuru verileri statik anlık görüntülerini kullanabilirsiniz. Bu özelliği tanıtmak için aşağıdaki örnek soru kullanın. Kayıt giriş lisans etiketlerin süre sonu listeleyen bir statik blob json dosyasıdır. Lisans makinesinde birleştirerek, başvuru verileri her araç Ücretli her iki geçirme karşılaştırılır. 

Bir ticari araç Ücretli şirketle kaydedilmişse Ücretli Stand İnceleme için durdurulmadan geçirebilirsiniz. Kayıt arama tablosu kayıtlar süresi dolmuş tüm ticari araçları tanımlamak için kullanın.

```sql
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

1. İş sorgu sözdizimi akış TollApp güncelleştirmek için önceki bölümdeki adımları yineleyin.

2. İş akışında CosmosDB çıktı verilerini gözden geçirmek için önceki bölümdeki adımları yineleyin. 

Örnek çıktı:
```json
    {
        "entrytime": "2018-04-05T08:01:28.0252168Z",
        "licenseplate": "GMT 3221",
        "tollid": 1,
        "registrationid": "763220582",
        "id": "47db0535-9716-4eb2-db58-de7886966cbf",
        "_rid": "y+F8AJ9QWACSAQAAAAAAAA==",
        "_self": "dbs/y+F8AA==/colls/y+F8AJ9QWAA=/docs/y+F8AJ9QWACSAQAAAAAAAA==/",
        "_etag": "\"88007d8d-0000-0000-0000-5ac5d7e20000\"",
        "_attachments": "attachments/",
        "_ts": 1522915298
    }
```

## <a name="scale-out-the-job"></a>Projeyi ölçeklendirme
Büyük miktarda veriyi işlemek azure Stream Analytics özellikler esnek ölçek için tasarlanmıştır. Azure Stream Analytics sorgu kullanabileceğiniz bir **bölüm tarafından** sistem bu adımı çıkışı ölçeklendirir bildirmek için yan tümcesi. **PartitionID** (olay hub) giriş bölüm Kimliğini eşleştirilecek sistemi ekleyen özel bir sütundur.

Bölümler sorguya genişletmek için sorgu söz dizimi aşağıdaki koda düzenleyin:
```sql
SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
INTO CosmosDB
FROM EntryStream 
TIMESTAMP BY EntryTime 
PARTITION BY PartitionId
GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId
```

Daha fazla akış birimleri için iş akışında ölçeklendirin için:

1. **Durdur** geçerli iş. 

2. Sorgu sözdizimi güncelleştirme **< > sorgu** sayfasında ve değişiklikleri kaydedin.

3. İş akışında yapılandırma başlığı altında seçin **ölçek**.
   
4. Slayt **akış birimleri** kaydırıcı 1-6. Akış birimleri, iş alabilir işlem güç miktarını tanımlayın. **Kaydet**’i seçin.

5. **Başlat** ek ölçek göstermek için iş akışında. Azure Stream Analytics işini daha fazla işlem kaynağı dağıtır ve PARTITION BY yan tümcesinde belirtilen sütun kullanarak kaynaklarına arasında iş bölümleme daha iyi verim elde etmek. 

## <a name="monitor-the-job"></a>İş izleme
**İZLEYİCİ** alanı içeren çalışan iş hakkındaki istatistiklerdir. (Bu belgenin kalan gibi ad Ücretli) aynı bölgede depolama hesabı kullanmak için ilk kez bir yapılandırma gerekmez.   

![İzleyici'nin ekran görüntüsü](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

Erişebileceğiniz **etkinlik günlükleri** işi panodan **ayarları** de alanı.

## <a name="clean-up-the-tollapp-resources"></a>TollApp kaynakları temizlemek
1. Azure portalında Stream Analytics işi durdurun.

2. TollApp şablona ilgili sekiz kaynakları içeren kaynak grubunu bulun.

3. **Kaynak grubunu sil**'i seçin. Silme işlemini onaylamak için kaynak grubu adını yazın.

## <a name="conclusion"></a>Sonuç
Bu çözüm Azure akış analizi hizmetine kullanıma sunuldu. Girişleri ve çıkışları Stream Analytics işi için nasıl yapılandırılacağı gösterilmektedir. Ücretli veri senaryoyu kullanarak, çözümü karşılaşılan Azure akış analizi basit SQL benzeri sorguları hareket ve bunların nasıl çözülebilir verilerle alanındaki çıkan sorunları açıklanmıştır. Çözüm zamana bağlı verilerle çalışmak için SQL uzantısı yapıları açıklanmaktadır. Veri akışları nasıl, statik başvuru verileri ile veri akışı zenginleştirmek nasıl ve daha yüksek verimlilik elde etmek için bir sorgu ölçeklendirmek nasıl oluşturulacağını gösterir.

Bu çözüm iyi giriş sağlasa da, herhangi bir yolla tam değil. Daha fazla sorgu desenlerine SAQL dil desteğini kullanarak bulabilirsiniz [sorgu örnekler için ortak Stream Analytics kullanım desenlerini](stream-analytics-stream-analytics-query-patterns.md).
