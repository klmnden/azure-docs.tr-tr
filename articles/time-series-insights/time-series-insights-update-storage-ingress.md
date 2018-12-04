---
title: Veri depolama ve Azure Time Series Insights (Önizleme) içinde giriş | Microsoft Docs
description: Veri depolama ve Azure Time Series Insights (Önizleme) içinde giriş anlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 6635558d1b7cf664084c103e3b3b37b44c022fa6
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856995"
---
# <a name="data-storage-and-ingress-in-the-azure-time-series-insights-preview"></a>Veri depolama ve Azure Time Series Insights (Önizleme) içinde giriş

Bu makalede, Azure Time Series Insights (Önizleme) verileri depolama ve giriş değişiklikleri açıklar. Dosya biçimi, temel alınan depolama yapısı kapsar ve **zaman serisi kimliği** özelliği. Ayrıca, temel alınan giriş işlemi, aktarım hızı ve sınırlamalar açıklanır.

## <a name="data-storage"></a>Veri depolama

Time Series Insights güncelleştirme oluştururken (**PAYG Sku**) ortamında, iki kaynak oluşturmakta olduğunuz:

1. Bir Azure TSI ortamı.
1. Bir Azure depolama genel amaçlı V1 hesabı verilerin depolanacağı.

(Önizleme) TSI Parquet dosya türü ile Azure Blob Depolama kullanır. Azure TSI, dizin oluşturma ve Azure depolama hesabındaki verileri bölümleme BLOB'ları oluşturma da dahil olmak üzere tüm veri işlemlerini yönetir. Bu BLOB'ları kullanarak bir Azure depolama hesabı oluşturulur. Tüm olayları yüksek performanslı bir şekilde sorgulanabilir sağlamak için. TSI güncelleştirme Azure depolama genel amaçlı V1 ve V2 "etkin" yapılandırma seçeneklerini destekler.  

Diğer Azure depolama blobu gibi okuma ve yazma farklı tümleştirme senaryolarını desteklemek için Azure TSI oluşturulan BLOB'ları için.

> [!TIP]
> TSI performansı olumsuz yönde okuma veya çok sık, bloblar için yazma etkilenebilir unutmamak önemlidir.

Azure Blob depolamanın genel bakış için okuma [depolama blobları giriş](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

Parquet dosya türü hakkında daha fazla bilgi için gözden [desteklenen dosya türleri Azure Depolama'da](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#Parquet-format).

## <a name="parquet-file-format"></a>Parquet dosyası biçimi

Parquet tasarlanma sütun odaklı veri dosyası biçimi şöyledir:

* Birlikte çalışabilirlik
* Alan verimliliğini
* Sorgu verimliliği artar

Azure TSI Parquet verimli veri sıkıştırma sağlar ve karmaşık veri toplu işleme için performans ile kodlama düzenleriyle Gelişmiş seçtiniz.

Hangi Parquet dosya biçimi olduğuna iyi anlaşılmasını için attıktan [resmi Parquet sayfa](https://parquet.apache.org/documentation/latest/).

## <a name="event-structure-in-parquet"></a>Parquet içindeki olay yapısı

Aşağıdaki biçimlerde Azure TSI tarafından oluşturulan BLOB'ları iki kopya depolanır:

1. İlk olarak, bir ilk kopyalama geliş saati tarafından ayrılmış olmalıdır:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI internal suffix>.parquet`
    * Bölümlenmiş geliş saati ile BLOB'ları için BLOB oluşturma zamanı.

1. İkincisi, repartitioned kopyalama, zaman serisi kimliği dinamik gruplandırarak bölümlere ayrılması:

    • `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_< TSI internal suffix >.parquet`
    * Min olay zaman damgası tarafından zaman serisi bölümlenmiş BLOB'ları için BLOB kimliği

> [!NOTE]
> * `<YYYY>` yıl eşlenir.
> * `<MM>` ay eşlenir.
> * `<YYYYMMDDHHMMSSfff>` milisaniye cinsinden tam zaman damgası eşlenir.

Azure TSI olayları Parquet dosya içerikleri için şu şekilde eşlenir:

* Her olay için tek bir satır eşler.
* Yerleşik **zaman damgası** bir olay zaman damgası içeren sütun. **Zaman damgası** özelliği null hiçbir zaman.  Varsayılan **olay kaynağı sıraya saati** varsa **zaman damgası** özelliği, olay kaynağı belirtilmedi.  **Zaman damgası** UTC biçiminde olan.  
* Sütunlar için diğer tüm özellikler eşlemesi ile sona erecek `_string` (dize) `_bool` (boolean) `_datetime` (TarihSaat), ve `_double` özellik türüne bağlı olarak (çift).
* Bu dosya biçiminin ilk sürümüdür ve olarak diyoruz ***V = 1**.  Bu özellik gelişse bile adı buna göre artırılır için **V = 2**, **V = 3**ve benzeri.

## <a name="how-to-partition"></a>Nasıl yapılır bölüm

Her Azure TSI (Önizleme) ortama sahip olmanız gerekir bir **zaman serisi kimliği** özelliği ve **zaman damgası** benzersiz şekilde tanımlamak özelliği. **Zaman serisi kimliği** verileriniz için mantıksal bir bölümü olarak görev yapar ve veriler fiziksel bölümler arasında dağıtmak için doğal bir sınır ile Azure TSI (Önizleme) ortamı sağlar. Fiziksel bölüm yönetimi, bir Azure depolama hesabında Azure TSI (Önizleme) tarafından yönetilir.

Azure TSI, bırakarak ve bölümleri yeniden depolama kullanımı ve sorgu performansını iyileştirmek için dinamik bölümlemeyi kullanır. TSI (Önizleme) dinamik bölümleme algoritma, verileri birden çok farklı mantıksal bölümleri olan tek bir fiziksel bölüm önlemek üstlenmeye çalışır. Veya başka bir deyişle bölümleme algoritma'nın hedefi tek bir ilgili tüm verileri tutmak için **zaman serisi kimliği** diğer aralıklı olmadan Parquet dosyasında özel olarak bulunması **zaman serisi kimlikleri**. Olayları tek bir özgün sıralamasını korumak dinamik bölümleme algoritması da üstlenmeye çalışır **zaman serisi kimliği**.

Giriş zamanında tarafından veri başlangıçta bölümlenen **zaman damgası** için belirtilen zaman aralığı içinde mantıksal, tek bir bölüm birden çok fiziksel bölümler arasında yayılıyor olabilir. Tek bir fiziksel bölüm çoğu veya tüm mantıksal bölümler de içerebilir.  BLOB boyutu sınırlamaları nedeniyle, bile en uygun bölümleme tek bir mantıksal bölüm birden çok fiziksel bölüme kaplayabilir.

> [!NOTE]
> **Zaman damgası** değerdir ileti **sıraya zaman** varsayılan olarak yapılandırılan olay kaynağınızdaki.  

Geçmiş verileri veya toplu iletileri yüklüyorsanız belirlemeniz gerekir **zaman damgası** uygun eşleşen veri özelliği **zaman damgası** ile verilerinizi depolamak istediğiniz değer.  **Zaman damgası** özelliği duyarlıdır. Daha fazla bilgi için okuma [zaman serisi modeli makale](./time-series-insights-update-tsm.md).

## <a name="physical-partition"></a>Fiziksel bölüm

Bir fiziksel bölüm, Azure Depolama'da depolanan bir blok blobudur. Anında iletme oranına bağlı olarak BLOB'ları yaklaşık 20-50 MB boyutunda olmasını bekliyoruz ancak blobları gerçek boyutuna göre değişir. Bu beklentiler nedeniyle TSI takım 20 MB'lık boyut sorgu performansını iyileştirmek için seçildi. Bu, dosya boyutu ve hızı veri girişine göre zaman içinde değişebilir.

> [!NOTE]
> * Bloblar, 20 MB boyutlandırılır.
> * Azure BLOB'ları bazen daha iyi performans için olarak bırakılan ve yeniden yeniden bölümlenebildiği.
> * Ayrıca aynı TSI verileri birden çok bloblar bulunabilir unutmayın.

## <a name="logical-partition"></a>Mantıksal bölüm

Bir mantıksal bölüm, bir tek bölüm anahtarı değeri ile ilişkili tüm verileri depolayan bir fiziksel bölüm içindeki bir bölümdür. TSI (Önizleme), mantıksal olarak iki özelliğe göre her bir blob bölüm:

1. **Zaman serisi kimliği** -olay akışını ve modeli içindeki tüm TSI veriler için bölüm anahtarı.
1. **Zaman damgası** - ilk girişine göre.

Bu iki özelliklerine bağlı, yüksek performanslı sorgular Azure TSI (Önizleme) sağlar. Bu iki özellik de TSI verileri hızlı bir şekilde sunmaya yönelik en etkili yöntem sağlar.

Uygun bir seçmeniz önemlidir **zaman serisi kimlikleri**, sabit bir özelliği olarak.  Lütfen [seçerek zaman serisi kimlikleri](./time-series-insights-update-how-to-id.md) daha fazla bilgi için.

## <a name="your-azure-storage-account"></a>Azure depolama hesabınız

### <a name="storage"></a>Depolama

TSI PAYG ortam oluşturduğunuzda, iki kaynak – TSI ortamı ve bir Azure depolama genel amaçlı V1 hesabı verilerin depolanacağı oluşturun. Gelecekte Azure depolama genel amaçlı V1 hesapları var olan Azure müşterilerine sınırlı olacaktır, dolayısıyla varsayılan olarak bir Azure depolama genel amaçlı V2 hesabına 'Sık erişimli' TSI (Önizleme) ortamı hazırlama azure'a tüm yeni müşteriler oluşturur.  Azure depolama genel amaçlı V1 yapmak seçtik, birlikte çalışabilirlik, fiyat ve Performans nedeniyle varsayılan.  

Azure TSI, Azure depolama hesabınızdaki her bir olay en fazla iki kopyasını yayımlar. İlk kopyayı diğer hizmetleri kullanarak performantly sorgu emin olmak için her zaman korunur. Bu nedenle, Spark, Hadoop ve tanıdık araçlardan kolayca genelinde kullanılabilir **zaman serisi kimlikleri** ham Parquet dosya olduğundan bu altyapılar temel destek dosyaları ada göre filtre uygula. Blobları yıl ve aya göre gruplandırma, özel bir iş için belirli bir zaman aralığı içinde blob listesi toplamak kullanışlıdır.  

Ayrıca, TSI Azure TSI API'leri için en iyi duruma getirme Parquet dosyalarını yeniden bölümlenip ve en son repartitioned dosya da kaydedilir.

Genel Önizleme sırasında veriler Azure depolama hesabınızdaki süresiz olarak depolanır. Verileri silme olanağı sağlayan yüksek düzeyde denetim eski verileri yönetme gelecekte eklenecektir.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Yazma ve Time Series Insights BLOB'ları düzenleme

Sorgu performansı ve veri kullanılabilirliğini sağlamak için düzenleyemeyeceği ya TSI tarafından oluşturulan tüm blobları silin.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Erişim ve zaman serisi Öngörülerinden (Önizleme) verileri dışarı aktarma

Azure TSI diğer hizmetler ile birlikte kullanmak için Gezgini (Önizleme) içinde depolanan verilere isteyebilirsiniz. Örneğin, Azure Machine Learning Studio'yu kullanarak makine öğrenimi için Power BI veya bir not defteri uygulama Jupyter not defterleri, vb. raporlama için verilerinizi kullanmak isteyebilirsiniz.

Verilerinize erişmek için üç genel yol vardır:

* (Önizleme) Azure TSI Gezgini.
* Azure TSI (Önizleme) API'ları.
* Doğrudan bir Azure depolama hesabından.

### <a name="from-the-time-series-insights-preview-explorer"></a>Time Series Insights (Önizleme) Gezgini'nden

TSI Gezgini (Önizleme) bir CSV dosyasından olarak verileri dışarı aktarabilirsiniz. Daha fazla bilgi edinin [TSI Gezgini (Önizleme)](./time-series-insights-update-explorer.md).

### <a name="from-the-time-series-insights-preview-apis"></a>Time Series Insights (Önizleme) API'lerinden

API uç noktası adresinden ulaşılabilir `/getRecorded`. Bu API hakkında daha fazla bilgi okuyun [zaman serisi sorgu](./time-series-insights-update-tsq.md).

### <a name="from-an-azure-storage-account"></a>Bir Azure depolama hesabından

1. Ne olursa olsun hesabına TSI verilerinize erişmek için kullanacağınız verilen okuma erişimi olması gerekir. Azure Blob depolamaya okuma erişimi verme hakkında daha fazla bilgi edinmek için [depolama kaynaklara erişimi yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources).

1. Azure Blob depolama alanından verileri okumak için doğrudan yollar hakkında daha fazla bilgi için okuma [için ve Azure Depolama'dan veri taşıma](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

1. Verileri bir Azure depolama hesabından dışarı aktarma:

    * İlk verileri dışarı aktarmak için karşılanması gereken gereksinimleri, hesabınızın olduğundan emin olun. Okuma [depolama içeri ve dışarı aktarma gereksinimleri](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements) daha fazla bilgi için.
    * Verileri Azure depolama hesabınızı sık ziyaret ederek dışarı aktarmak için diğer yollar hakkında bilgi edinin [depolama almak ve bloblarından veri dışarı aktarma](https://docs.microsoft.com/azure/storage/common/storage-import-export-data-from-blobs)

### <a name="blob-storage-considerations"></a>BLOB Depolama konuları

* Azure depolama alanına sahip okuma ve yazma yükün TSI (Önizleme) kullanımınızı olduğuna göre sınırlar.  
* Henüz Azure TSI özel Önizleme Parquet blob meta veri işleme dış sistemler desteklemek için mağaza içi herhangi bir türden sağlamaz. Ancak biz bunu Araştırıyor ve desteği gelecekte ekleyebilirsiniz.  
* Müşterilerin verileri işlemek için zaman tarafından bölümlenmiş Azure BLOB'ları okuma gerekir.
* Azure TSI özel önizlemesi, blob verilerini daha iyi performans için dinamik yeniden bölümlenmesi gerçekleştirir. Bu, bırakarak ve BLOB'ları yeniden gerçekleştirilir. Hizmetlerin çoğu, özgün dosyalarını kullanarak en iyi alabilecektir.  
* TSI (Önizleme) verilerinizi BLOB'ları arasında çoğaltılmış olabilir.

### <a name="data-deletion"></a>Veri silme

Azure TSI özel Önizleme veri silme işlemi şu anda desteklememektedir ancak gelecekte. GA tarafından ancak büyük olasılıkla daha çabuk desteklemek bekliyoruz. Veri silme destekliyoruz, kullanıcıların bilgilendireceğiz.

Time Series Insights (Önizleme) TSI güncelleştirme içinde BLOB'ları hakkında meta veriler tutar beri blobları silmeyin.

## <a name="ingress"></a>Giriş

### <a name="azure-time-series-insights-ingress-policies"></a>Azure zaman serisi görüşleri giriş ilkeleri

Azure TSI özel önizlemesi, aynı olay kaynakları ve şu anda dosya türlerini destekler.

Desteklenen olay kaynakları şunlardır:

* Azure IoT Hub
* Azure Event Hubs
  * Not: Kafka örnekleri Azure olay hub'ı destekler.

Desteklenen dosya türleri şunlardır:

* JSON
  * Desteklenen JSON şekilleri üzerinde size daha fazla işlemek için bkz: [zaman serisi sorgu](./time-series-insights-update-tsq.md) belgeleri.

### <a name="data-availability"></a>Veri kullanılabilirliği

Azure TSI özel Önizleme blob boyutu iyileştirme stratejisi kullanarak verilerin dizinini oluşturur. Başka bir deyişle, veriler (hangi veri miktarını ve hangi hız uygun yayımlanacak göre) dizinlendikten sonra sorgu kullanılabilir. Biz genel Önizleme yaklaştıkça, yeni olaylar için (Bu, veri sorguları daha hızlı ve daha güvenilir için kullanılabilir hale getirir) her birkaç saniyede aramak için mantıksal eklenir.

> [!IMPORTANT]
> * Genel Önizleme TSI veri 60 saniye içinde bunu bir olay kaynağı ulaşmaktan kullandıracağınız.  
> * Daha uzun bir süre önce verileri görmek için bekliyoruz özel Önizleme sırasında kullanılabilir hale getirilir.  
>   * Önemli bir gecikme yaşıyorsanız, lütfen bizimle iletişime geçin.

### <a name="scale"></a>Ölçek

Her ortam 6Mbps kadar desteklemek için Azure TSI özel Önizleme bekliyoruz. Gelişmiş ölçeklendirme destek gelecekteki sürümlerde sunulması planlanmaktadır.

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Yeni hakkında okuyun [zaman serisi modeli](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-storage-ingress/storage-architecture.png
[2]: media/v2-update-storage-ingress/parquet-files.png
[3]: media/v2-update-storage-ingress/blob-storage.png