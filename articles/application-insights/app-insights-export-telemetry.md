---
title: "Application Insights telemetrisini sürekli verilmesini | Microsoft Docs"
description: "Microsoft Azure depolama için tanılama ve kullanım verilerini dışa aktarın ve buradan indirin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: mbullwin
ms.openlocfilehash: 7d1f648bc2c2a42cfbd668f180bce8f56ebd065b
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="export-telemetry-from-application-insights"></a>Application Insights telemetrisini dışarı aktarma
Telemetrinize standart saklama süresinden daha uzun süre tutmak mı istiyorsunuz? Veya özelleştirilmiş bir şekilde işlemek? Sürekli verme bunun için idealdir. Application Insights portalında görmek olayları JSON biçiminde Microsoft Azure depolama alanına aktarılabilir. Buradan, verilerinizi indirin ve size kod yazma işlemesi gerekir.  

Sürekli verme özelliğini kullanarak ek bir ücret maruz kalabilirsiniz. Denetleyin, [fiyatlandırma modeli](http://azure.microsoft.com/pricing/details/application-insights/).

Sürekli verme ayarlamadan önce göz önünde bulundurabilirsiniz bazı alternatifleri vardır:

* Ölçümleri veya arama dikey pencerenin üstündeki dışa aktarma düğmesi tabloları aktarmanıza olanak sağlar ve bir Excel elektronik tablosuna grafikleri.

* [Analytics](app-insights-analytics.md) telemetri için güçlü sorgu dili sağlar. Ayrıca sonuçlarını dışarı aktarabilirsiniz.
* İçin arıyorsanız [Power BI verilerinizi keşfedin](app-insights-export-power-bi.md), bunu sürekli verme kullanmadan yapabilirsiniz.
* [Veri erişim REST API](https://dev.applicationinsights.io/) siz telemetrinize programlı erişim sağlar.

Sürekli verme verilerinizi (burada, kalarak için istediğiniz sürece) depolama birimine kopyaladıktan sonra her zamanki için Application Insights'ta hala kullanılabilir [saklama dönemi](app-insights-data-retention-privacy.md).

## <a name="setup"></a>Sürekli bir dışarı aktarma oluşturma
1. Application Insights kaynağı uygulamanız için sürekli verme açın ve seçin **Ekle**:

    ![Aşağı kaydırın ve sürekli ver](./media/app-insights-export-telemetry/01-export.png)

2. Telemetriyi dışarı aktarmak istediğiniz veri türlerini seçin.

3. Oluşturma veya seçme bir [Azure depolama hesabı](../storage/common/storage-introduction.md) , verileri depolamak istediğiniz.

    > [!Warning]
    > Varsayılan olarak, depolama konumu Application Insights kaynağınıza aynı coğrafi bölgede şekilde ayarlanacak. Farklı bir bölgede depolarsanız, aktarım ödemelere maruz kalabilirsiniz.

    ![Dışarı aktarmak, hedef depolama hesabı Ekle'yi ve ardından yeni bir depolama alanı oluşturmak veya mevcut deposunu seçin](./media/app-insights-export-telemetry/02-add.png)

4. Oluşturma veya depolama alanında bir kapsayıcı seçin:

    ![Seç olay türleri](./media/app-insights-export-telemetry/create-container.png)

Dışa aktarma oluşturduktan sonra olmaya başlar. Yalnızca verme oluşturduktan sonra ulaşan Veri Al

Yaklaşık bir saat veri depolama alanında görüntülenmeden önce bir gecikme olabilir.

### <a name="to-edit-continuous-export"></a>Sürekli verme düzenlemek için

Yalnızca olay türleri daha sonra değiştirmek istiyorsanız, dışa aktarma düzenleyin:

![Seç olay türleri](./media/app-insights-export-telemetry/05-edit.png)

### <a name="to-stop-continuous-export"></a>Sürekli verme durdurmak için

Dışarı aktarma durdurmak için devre dışı bırak'ı tıklatın. Etkinleştirme tekrar tıklattığınızda, dışa aktarma yeni verilerle yeniden başlatılır. Dışarı aktarma devre dışıyken Portalı'nda gelen verileri alamazsınız.

Dışarı aktarma kalıcı olarak durdurmak için dosyayı silin. Bunun yapılması, veri depolama biriminden silmez.

### <a name="cant-add-or-change-an-export"></a>Ekleyemez veya verme değiştirmek mi?
* Eklemek veya dışarı aktarma değiştirmek için sahibi, katkıda bulunan veya uygulama Öngörüler katkıda erişim haklarına sahip olmanız gerekir. [Rolleri hakkında bilgi edinin][roles].

## <a name="analyze"></a>Hangi olayların alıyorum?
İstemci IP adresinden biz hesaplamak konum verileri eklediğimiz dışarı aktarılan verileri uygulamanızdan aldığımız ham telemetri olmasıdır.

Tarafından atılan veri [örnekleme](app-insights-sampling.md) dışarı aktarılan verileri dahil edilmez.

Hesaplanan diğer ölçümleri dahil edilmez. Örneğin, ortalama CPU kullanımı verme yoktur, ancak içinden ortalama hesaplanır ham telemetri verme.

Veriler ayrıca herhangi sonuçlarını içerir [kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md) ayarlamış olduğunuz.

> [!NOTE]
> **Örnekleme.** Uygulamanız çok miktarda veri gönderiyorsa örnekleme özelliği çalışır ve yalnızca bir kesir oluşturulan telemetri gönderin. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)
>
>

## <a name="get"></a>Verileri inceleyin.
Doğrudan portal içinde depolama inceleyebilirsiniz. Tıklatın **Gözat**, depolama hesabınızı seçin ve ardından açın **kapsayıcıları**.

Visual Studio'da Azure depolama incelemek için açık **Görünüm**, **Cloud Explorer**. (Bu komutu yoksa, Azure SDK'yı yüklemeniz gerekir: açık **yeni proje** iletişim kutusunda, Visual C# ' ı genişletin / Bulut ve **.NET için Microsoft Azure SDK almak**.)

Blob deposu açtığınızda, blob dosya kümesini içeren bir kapsayıcı göreceksiniz. Application Insights kaynağı adı, izleme anahtarı, telemetri-türü/tarih/saat türetilen her dosya URI'si. (Kaynak adının tamamen küçük harflerden ve tire izleme anahtarı atlar.)

![Uygun bir araçla blob deposu inceleyin.](./media/app-insights-export-telemetry/04-data.png)

Tarih ve saati UTC ve telemetri deposunda - değil oluşturulduğu zaman borç zaman. Veri indirmek için kod yazma varsa, bu nedenle, doğrusal olarak verilerine taşıyabilirsiniz.

Yolu şöyledir:

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

Burada

* `blobCreationTimeUtc`depolama alanı hazırlama blob dahili olarak oluşturulduğu süresi
* `blobDeliveryTimeUtc`blob verme hedef depolama için ne zaman kopyalanır zamanı

## <a name="format"></a>Veri biçimi
* Her bir blob birden çok içeren bir metin dosyasıdır ' \n'-separated satır. Bir süre kabaca yarım bir dakika boyunca işlenen telemetri içerir.
* Her satır bir istek veya sayfa görünümü gibi bir telemetri veri noktasını temsil eder.
* Her satır bir biçimlendirilmemiş bir JSON dosyasıdır. Yaslanın ve yerinde stare yapmak istiyorsanız, Visual Studio'da açın ve seçin, Gelişmiş biçim dosyasını düzenleyin:

![Uygun bir araçla telemetri görüntüleyin](./media/app-insights-export-telemetry/06-json.png)

Süreler olduğunuz nerede 10 000 işaretlerini çizgilerine içinde 1ms =. Örneğin, bu değerleri tarayıcısından alması 3ms ve 1.8s sayfasını tarayıcıda işlemek için bir istek göndermesini 1ms süresini göster:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Ayrıntılı veri özellik türleri ve değerleri için başvuru modeli.](app-insights-export-data-model.md)

## <a name="processing-the-data"></a>Veri işleme
Küçük bir ölçekte verilerinizi birbirinden çekme, elektronik tabloya okuyun ve benzeri için bazı kod yazabilirsiniz. Örneğin:

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

Daha büyük bir kod örneği için bkz: [çalışan rolü kullanarak][exportasa].

## <a name="delete"></a>Eski verileri Sil
Lütfen depolama kapasitesi yönetme ve gerekirse eski verileri silmek için sorumlu olduğunu unutmayın.

## <a name="if-you-regenerate-your-storage-key"></a>Depolama anahtarınızı yeniden oluşturursanız...
Depolama alanınızın anahtar değiştirirseniz, sürekli verme çalışmayı durdurur. Azure hesabınızda bir bildirim görürsünüz.

Sürekli Dışarı Aktar dikey penceresini açın ve dışa aktarma düzenleyin. Dışarı aktarma hedefi Düzenle, ancak yalnızca seçili aynı depolama birimi bırakın. Onaylamak için Tamam'ı tıklatın.

![Üç verme hedef düzenleme sürekli verme, açık ve kapalı.](./media/app-insights-export-telemetry/07-resetstore.png)

Sürekli verme yeniden başlatılır.

## <a name="export-samples"></a>Dışarı aktarma örnekleri

* [Akış analizi kullanarak SQL dışarı aktarma][exportasa]
* [Stream Analytics örnek 2](app-insights-export-stream-analytics.md)

Büyük ölçek üzerinde dikkate [Hdınsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop kümelerini bulutta. Hdınsight çeşitli teknolojiler yönetmek ve büyük veri çözümleme sağlar ve Application Insights ' dışarı aktarılan verileri işlemek için kullanabilirsiniz.

## <a name="q--a"></a>Soru-Cevap
* *Ancak tüm istiyorum tek seferlik bir indirme bir grafik.*  

    Evet, bunu yapabilirsiniz. Dikey pencerenin üstündeki **verileri dışa aktar**.
* *Bir verme ayarlayın, ancak my deposunda verisi yok.*

    Verme ayarladığınız bu yana Application Insights uygulamanızdan tüm telemetri aldınız mı? Yalnızca yeni verileri alırsınız.
* *I verme ayarlanmaya çalışıldı, ancak erişim reddedildi*

    Hesap, kuruluşunuz tarafından aitse sahipler veya katkıda bulunanlar grupların üyesi olmak zorunda.
* *Doğrudan kendi şirket içi depoya dışa aktarabilirsiniz?*

    Hayır, özür dileriz. Bizim verme Altyapısı şu anda yalnızca Azure storage ile şu anda çalışıyor.  
* *My deposunda put veri miktarı için herhangi bir sınır var mıdır?*

    Hayır. Dışarı aktarma silene kadar biz verisi itmesi tutmak. Biz blob storage için dış sınırına ulaşıp ancak oldukça büyük durdurmanız. Bu, kullandığınız ne kadar depolama alanı denetlemek için hazır.  
* *Kaç tane BLOB Depolama alanında ı görmeniz gerekir?*

  * (Verileri kullanılabilir ise), dışarı aktarmak için seçtiğiniz her veri türü için yeni bir blob dakikada oluşturulur.
  * Ayrıca, yüksek trafiğe sahip uygulamalar için ek bölüm birimleri ayrılır. Bu durumda her birimi dakikada bir blob oluşturur.
* *Depolama için anahtar yeniden oluşturulacak veya kapsayıcı adının değişip ve dışarı aktarma artık çalışmıyor.*

    Dışarı aktarma düzenleyin ve dışarı aktarma hedefi dikey penceresini açın. Önceki seçili aynı depolama bırakın ve onaylamak için Tamam'ı tıklatın. Dışarı aktarma yeniden başlatılır. Son birkaç gün içinde değişiklik varsa, veri kaybı olmaz.
* *Dışarı aktarma duraklatabilir miyim?*

    Evet. Devre dışı bırak'ı tıklatın.

## <a name="code-samples"></a>Kod örnekleri

* [Akış analizi örneği](app-insights-export-stream-analytics.md)
* [Akış analizi kullanarak SQL dışarı aktarma][exportasa]
* [Ayrıntılı veri özellik türleri ve değerleri için başvuru modeli.](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
