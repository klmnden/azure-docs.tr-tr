---
title: Application Insights telemetrisini sürekli dışarı aktarma | Microsoft Docs
description: Microsoft Azure depolama için tanılama ve kullanım verileri dışarı aktarma ve buradan indirin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: mbullwin
ms.openlocfilehash: 71e70962a8c55d397b6261571cfef4a126d3e8b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60899426"
---
# <a name="export-telemetry-from-application-insights"></a>Application Insights'tan telemetriyi dışarı aktarma
Telemetrinizi standart saklama süresinden daha uzun süre tutmak mı istiyorsunuz? Veya özel bir şekilde işlemek? Sürekli dışarı aktarma için idealdir. Application Insights portalında gördüğünüz olayları için Microsoft Azure Depolama'da JSON biçiminde dışarı aktarılabilir. Buradan verilerinizi indirin ve, kod yazma, işlemeniz gerekir.  

Sürekli dışarı aktarmayı ayarlamadan önce bazı alternatifleri düşünün isteyebileceğiniz vardır:

* Dışarı Aktar düğmesini ölçümleri veya arama dikey penceresinin üst tablolar aktarmanıza olanak tanır ve bir Excel elektronik tablosuna grafikleri.

* [Analytics](../../azure-monitor/app/analytics.md) telemetri için güçlü bir sorgu dili sağlar. Ayrıca sonuçlarını dışarı aktarabilirsiniz.
* İçin arıyorsanız [verilerinizi Power bı'da araştırma](../../azure-monitor/app/export-power-bi.md ), sürekli dışarı aktarma dosyası olmadan bunu yapabilirsiniz.
* [Veri erişimi REST API](https://dev.applicationinsights.io/) , telemetrinizi programlı bir şekilde erişmenize olanak tanır.
* Kurulum da erişebilirsiniz [Powershell aracılığıyla sürekli dışarı aktarma](https://docs.microsoft.com/powershell/module/az.applicationinsights/new-azapplicationinsightscontinuousexport).

Sürekli dışarı aktarma (burada, kalarak için istediğiniz sürece) depolama alanına veri kopyaladıktan sonra her zamanki için Application Insights'da hala kullanılabilir [saklama süresi](../../azure-monitor/app/data-retention-privacy.md).

## <a name="continuous-export-advanced-storage-configuration"></a>Sürekli dışarı aktarma gelişmiş depolama yapılandırması

Sürekli dışarı aktarma **desteklemediği** aşağıdaki Azure depolama özellikleri/yapılandırmalar:

* Kullanım [VNET/Azure depolama güvenlik duvarları](https://docs.microsoft.com/azure/storage/common/storage-network-security) Azure Blob Depolama ile birlikte okunmalıdır.

* [Sabit depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) Azure Blob Depolama için.

* [Azure Data Lake depolama Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction).

## <a name="setup"></a> Sürekli dışarı aktarma oluştur
1. Application Insights kaynağı uygulamanız için sürekli dışarı aktarma açın ve seçin **Ekle**:

2. Telemetriyi dışarı aktarmak istediğiniz veri türlerini seçin.

3. Oluşturma veya seçme bir [Azure depolama hesabı](../../storage/common/storage-introduction.md) istediğiniz verileri depolamak. Depolama fiyatlandırma seçenekleri hakkında daha fazla bilgi için ziyaret [fiyatlandırma sayfası resmi](https://azure.microsoft.com/pricing/details/storage/).

    > [!Warning]
    > Varsayılan olarak, Application Insights kaynağınıza aynı coğrafi bölgede için depolama konumu da ayarlanır. Farklı bir bölgede depolamak, aktarımı ücrete tabi olabilirsiniz.

    ![Dışarı aktarma, hedef depolama hesabı Ekle'a tıklayın ve yeni bir depo oluşturun veya var olan bir depo seçin](./media/export-telemetry/02-add.png)

4. Oluşturun veya depolama birimindeki kapsayıcı seçin:

    ![Choose olay türleri](./media/export-telemetry/create-container.png)

Dışarı aktarma oluşturduktan sonra kullanmaya başlar. Yalnızca bir dışarı aktarma oluşturduktan sonra gelen verileri elde edersiniz.

Veri depolamada görünmeden önce bir saatlik bir gecikme olabilir.

### <a name="to-edit-continuous-export"></a>Sürekli dışarı aktarma düzenlemek için

Olay türleri daha sonra değiştirmek istiyorsanız, yalnızca dışarı aktarmayı düzenleyin:

![Choose olay türleri](./media/export-telemetry/05-edit.png)

### <a name="to-stop-continuous-export"></a>Sürekli dışarı aktarma durdurmak için

Dışarı aktarma durdurmak için devre dışı bırak'a tıklayın. Etkinleştirme yeniden'e tıkladığınızda, dışarı aktarma yeni verilerle yeniden başlatılır. Dışarı aktarma devre dışıyken portalda gelen verileri elde etmezsiniz.

Dışarı aktarma kalıcı olarak durdurmak için silin. Bunun yapılması, veri depolama alanından silinmesine neden olmaz.

### <a name="cant-add-or-change-an-export"></a>Ekleyemez veya bir dışarı aktarma Değiştir?
* Ekleme veya değiştirme dışarı aktarmaları için erişim haklarına sahip, katkıda bulunan veya Application Insights katkıda bulunan gerekir. [Rolleri hakkında bilgi edinin][roles].

## <a name="analyze"></a> Hangi olayların elde ederim?
Şu hesapla konum verileri istemci IP adresinden eklediğimiz dışarı aktarılan verileri uygulamanızdan aldığımız ham telemetri olmasıdır.

Tarafından atılan veri [örnekleme](../../azure-monitor/app/sampling.md) dışarı aktarılan verileri dahil değildir.

Diğer hesaplanmış ölçüler dahil edilmez. Örneğin, ortalama CPU kullanımı biz vermeyin ancak biz içinden ortalaması hesaplanır ham telemetriyi dışarı aktarma.

Veriler ayrıca sonuçları herhangi içerir [kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md) ayarlamış olduğunuz.

> [!NOTE]
> **Örnekleme.** Uygulamanız çok miktarda veri gönderiyorsa örnekleme özelliği çalışır ve yalnızca bir kesir oluşturulan telemetri gönderin. [Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)
>
>

## <a name="get"></a> Verileri İnceleme
Doğrudan portalda depolama inceleyebilirsiniz. Tıklayın **Gözat**, depolama hesabınızı seçin ve ardından açın **kapsayıcıları**.

Visual Studio'da Azure depolama incelemek için açık **görünümü**, **Cloud Explorer**. (Bu menü komutu sahip değilseniz Azure SDK'yı yüklemeniz gerekir: Açık **yeni proje** iletişim kutusunda Visual genişletin C#/bulut seçip **.NET için Microsoft Azure SDK alma**.)

Blob deponuza açtığınızda, bir dizi blob dosyalarını içeren bir kapsayıcıya görürsünüz. Application Insights kaynak adınız, izleme anahtarı, telemetri-türü/tarih/saat türetilen her dosya URI'si. (Kaynak adı tamamen küçük harflerden ve kısa çizgi izleme anahtarı atlar.)

![Uygun bir aracı ile blob deposu inceleyin](./media/export-telemetry/04-data.png)

Tarih ve saati UTC ve telemetri deposunda - oluşturulduğu saat borç zaman. Verileri indirmek için kod yazma, bu nedenle, doğrusal olarak verilerine taşıyabilirsiniz.

Yol biçimi şu şekildedir:

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

Konum

* `blobCreationTimeUtc` Depolama blob dahili olarak oluşturulduğu zaman zaman hazırlama
* `blobDeliveryTimeUtc` süresi ne zaman blob dışarı aktarma hedef depolama alanına kopyalanır.

## <a name="format"></a> Veri biçimi
* Her blob, birden çok içeren bir metin dosyasıdır ' \n'-separated satır. Bu işleme yaklaşık yarım bir dakikalık bir zaman aralığında toplanan telemetriyi içerir.
* Her satır bir istek veya sayfa görüntüleme gibi bir telemetri veri noktasını temsil eder.
* Her satır bir biçimlendirilmemiş bir JSON belgesidir. Yaslanın ve adresinden stare istiyorsanız, Visual Studio'da açın ve seçin, Gelişmiş biçim dosyasını düzenleyin:

![Uygun bir araç telemetri görüntüleme](./media/export-telemetry/06-json.png)

Süreler olduğundan burada 10 000 işaretlerini dalgalanmasındaki = 1 ms. Örneğin, bu değerleri 1 süresini göster ms ve 1.8 almak için tarayıcıdan 3 ms istek göndermek için s sayfasını tarayıcıda işlemek için:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Ayrıntılı veri özellik türleri ve değerleri için başvuru model.](export-data-model.md)

## <a name="processing-the-data"></a>Veri işleme
Küçük bir ölçekte verilerinizi uzaklıkta çekme, elektronik tabloya okuyun ve benzeri için bazı kod yazabilirsiniz. Örneğin:

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

Daha büyük bir kod örneği için bkz. [bir çalışan rolü kullanarak][exportasa].

## <a name="delete"></a>Eski verilerinizi silme
Depolama kapasitesini yönetmek ve gerekirse eski verileri silmek için sorumlu olursunuz.

## <a name="if-you-regenerate-your-storage-key"></a>Depolama anahtarı yeniden oluşturursanız...
Depolama anahtarı değiştirirseniz, sürekli dışarı aktarma çalışmayı durdurur. Azure hesabınızda bir bildirim görürsünüz.

Sürekli dışarı aktarma dikey penceresini açın ve dışarı aktarma düzenleyin. Dışarı aktarma hedefi düzenlemek, ancak yalnızca seçilen aynı depolama bırakın. Onaylamak için Tamam'a tıklayın.

![Üç rol dışarı hedef düzenleme sürekli dışarı aktarma, açık ve kapalı.](./media/export-telemetry/07-resetstore.png)

Sürekli dışarı aktarmayı yeniden başlatılır.

## <a name="export-samples"></a>Dışarı aktarma örnekleri

* [Stream Analytics kullanarak SQL'e aktarma][exportasa]
* [Stream Analytics örneği 2](export-stream-analytics.md)

Büyük ölçekleri üzerinde düşünün [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop kümelerini bulutta. Çeşitli teknolojiler yönetmek ve büyük veri analizi için HDInsight sağlar ve Application Insights'tan dışarı aktarılan verileri işlemek için kullanabilirsiniz.

## <a name="q--a"></a>Soru-Cevap
* *Ancak tüm istiyorum grafik tek seferlik bir indirme.*  

    Evet, bunu yapabilirsiniz. Dikey pencerenin en üstünde tıklayın **verileri dışarı aktarma**.
* *Bir dışarı aktarma ayarlarım ancak my deposunda veri yoktur.*

    Dışarı aktarma ' olarak ayarlandığı Application Insights, uygulamanızdan hiç telemetri aldınız? Yalnızca yeni verileri de alacaksınız.
* *Ben bir dışarı aktarma ayarlanmaya çalışıldı ancak erişim reddedildi*

    Hesap kuruluşunuza ait, sahip veya katkıda bulunan grupların bir üyesi olmanız gerekir.
* *Doğrudan kendi şirket içi deposuna dışa aktarabilir miyim?*

    Hayır, özür dileriz. Dışarı aktarma altyapımız şu anda yalnızca Azure depolama ile şu anda çalışır.  
* *My deposunda koyduğunuz veri miktarı için herhangi bir sınır var mıdır?*

    Hayır. Dışarı aktarma silene kadar biz veri gönderme saklamak. Biz blob depolama için dış sınırlarına gelindiğinde, ancak oldukça büyük olan durdurmanız. Bunun ne kadar depolama alanı kontrol etmek için size aittir.  
* *Depolama alanında kaç blobları görüyorum?*

  * (Veri varsa) dışarı aktarmak için seçtiğiniz her veri türü için yeni bir blob dakikada oluşturulur.
  * Ayrıca, yüksek trafiğe sahip uygulamalar için ek bölüm birimleri ayrılır. Bu durumda, her bir birimi dakikada bir blob oluşturur.
* *İçin depolama anahtarı yeniden oluşturuldu veya kapsayıcının adı değiştirilmiş ve dışarı aktarma artık çalışmaz.*

    Dışarı aktarmayı düzenleyin ve dışarı aktarma hedefi dikey penceresini açın. Aynı depolama alanı olarak önce seçili bırakın ve onaylamak için Tamam'a tıklayın. Dışarı aktarma yeniden başlatılır. Son birkaç gün içinde değişiklik olduysa, veri kaybı olmaz.
* *Dışarı aktarma duraklatarak miyim?*

    Evet. Devre dışı bırak'a tıklayın.

## <a name="code-samples"></a>Kod örnekleri

* [Stream Analytics örnek](export-stream-analytics.md)
* [Stream Analytics kullanarak SQL'e aktarma][exportasa]
* [Ayrıntılı veri özellik türleri ve değerleri için başvuru model.](export-data-model.md)

<!--Link references-->

[exportasa]: ../../azure-monitor/app/code-sample-export-sql-stream-analytics.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
