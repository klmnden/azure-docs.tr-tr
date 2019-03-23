---
title: Sık erişimli, seyrek erişimli ve Arşiv BLOB'ları - Azure depolama için erişim katmanları
description: Sık erişimli, seyrek erişimli ve Arşiv Azure depolama hesapları için erişim katmanları.
services: storage
author: Xansky
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: mhopkins
ms.subservice: blobs
ms.openlocfilehash: 424ca1cccd1b82c26a801af3f85f41eb2991e2d7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370134"
---
# <a name="azure-blob-storage-hot-cool-and-archive-access-tiers"></a>Azure Blob Depolama: sık erişimli, seyrek erişimli ve Arşiv erişim katmanları

Azure depolama, blob nesne verilerini en uygun maliyetli bir şekilde depolamanızı sağlayan farklı erişim katmanı sunuyor. Kullanılabilir erişim katmanları mevcuttur:

- **Sık erişimli** - sık erişilen verileri depolamak için iyileştirilmiştir.
- **Seyrek erişimli** - daha az sıklıkta erişilen ve en az 30 gün saklanan verileri depolamak için iyileştirilmiştir.
- **Arşiv** - daha seyrek erişilen ve gecikme gereksinimleri esnek olan (saat bazında) en az 180 gün boyunca saklanan verileri depolamak için iyileştirilmiştir.

Farklı erişim katmanları için aşağıdaki maddeler geçerlidir:

- Arşiv erişim katmanı, yalnızca blob düzeyinde ve depolama hesabı düzeyinde kullanılamaz.
- Seyrek erişimli erişim katmanındaki veriler, biraz daha düşük bir kullanılabilirliği kabul edebilir, ancak yine de yüksek dayanıklılık ve sık erişimli veriler kadar erişim süresi ve aktarım hızı benzer özelliklere gerektirir. Seyrek erişimli veriler, biraz daha düşük kullanılabilirlik hizmet düzeyi sözleşmesi (SLA) ve yüksek erişim için sık erişimli verilere kıyasla daha düşük depolama maliyetleri için kabul edilebilir dengelemeler ücretlerdir.
- Çevrimdışı olan arşiv depolama, en düşük depolama maliyetini sunar ancak aynı zamanda en yüksek erişim maliyetine sahiptir.
- Hesap düzeyinde yalnızca sık ve seyrek erişimli erişim katmanları ayarlanabilir.
- Sık erişimli, seyrek erişimli ve Arşiv katmanları nesne düzeyinde ayarlanabilir.

Bulutta depolanan veriler üstel bir hızda artar. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, maliyetleri optimize etmek amacıyla erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler nasıl oluşturulan, işlenen ve yaşam süresi boyunca erişildiği açısından farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında sık erişilebilir. Bazı veriler bulutta boşta kalır ve nadiren de olsa, depolandıktan sonra her zamankinden erişilen ise.

Bu veri senaryolarının her belirli erişim düzeni için optimize edilmiştir farklı erişim katmanından faydalanır. İle çalışırken, bu gereksinimi fark yaratan erişim katmanları ayrı fiyatlandırma modelleriyle seyrek erişimli ve Arşiv erişim katmanı, Azure Blob Depolama adresi.

## <a name="storage-accounts-that-support-tiering"></a>Katman ayarlamayı destekleyen depolama hesapları

Yalnızca nesne depolama verilerinizi sık erişimli, seyrek erişimli katman veya Blob Depolama ve genel amaçlı v2 (GPv2) hesapları arşivleyin. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Ancak, var olan GPv1 veya Blob Depolama hesaplarını GPv2 hesaplarına Azure portalında bir tek tıklama işlemiyle kolayca dönüştürebilirsiniz. GPv2, bloblar, dosyalar ve Kuyruklar ve diğer yeni depolama özellikleri çok çeşitli erişim için yeni bir fiyatlandırma yapısı sağlar. Bundan sonra bazı yeni özelliklere geçmek ve fiyat indirimleri yalnızca GPv2 hesaplarda sunulur. Bu nedenle, GPv2 hesaplarını kullanmayı Değerlendirmeli ancak yalnızca tüm hizmetler için fiyatlandırma gözden geçirdikten sonra kullanmalıdır. Bazı iş yükleri, GPv1'den GPv2'de daha pahalı olabilir. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

BLOB Depolama ve GPv2 hesapları sunmaya **erişim katmanı** özniteliği hesap düzeyinde. Bu öznitelik varsayılan erişim katmanını seyrek veya nesne düzeyinde ayarlanmış açık bir katmanı olmayan depolama hesabındaki tüm bloblar için sık erişimli olarak belirtmenizi sağlar. Katmanı nesne düzeyinde ayarlanan nesnelerde hesap katmanı geçerli olmaz. Arşiv katmanı yalnızca nesne düzeyinde uygulanabilir. Herhangi bir zamanda bu erişim katmanları arasında geçiş yapabilirsiniz.

## <a name="premium-performance-block-blob-storage"></a>Premium performans blok blob depolama

Premium performans blok blob depolama, sık erişilen verileri yüksek performanslı donanıma aracılığıyla kullanılabilir hale getirir. Bu performans katmanındaki veriler için gecikme süresi düşük ve tutarlı iyileştirilmiş katı hal sürücüleri (SSD) depolanır. SSD'ler daha yüksek işlem hızları ve geleneksel sabit sürücüler karşılaştırıldığında aktarım hızı sağlar.

Premium performans blok blob depolama hızlı ve tutarlı yanıt süreleri gerektiren iş yükleri için idealdir. Telemetri verilerini yakalama, Mesajlaşma ve veri dönüştürme gibi çok sayıda küçük işlemler gerçekleştiren iş yükleri için idealdir. Son kullanıcılar, etkileşimli video düzenleme, statik web içeriğini ve çevrimiçi işlemleri gibi içeren veri de iyi adaylardır.

Premium performans blok blob depolama, yalnızca blok blob depolama hesabı türü kullanılabilir ve şu anda sık erişimli, seyrek erişimli veya arşiv erişim katmanları katman ayarlamayı desteklemez.

## <a name="hot-access-tier"></a>Sık erişim katmanı

Sık erişimli erişim katmanını seyrek erişimli ve Arşiv katmanları daha yüksek depolama maliyetleri, ancak en düşük erişim maliyetini sahiptir. Sık erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Etkin kullanımda ya da (okunma ve üzerine yazılma) sık erişilmesi beklenen verileri.
- Seyrek erişimli katmanına işlemeye ya da sonuçta geçişe hazırlanan veriler.

## <a name="cool-access-tier"></a>Seyrek erişim katmanı

Seyrek erişimli katman, daha düşük depolama maliyetleri ve sık erişimli depolamayla karşılaştırıldığında daha yüksek erişim maliyetine sahiptir. Bu katman, seyrek erişim katmanında en az 30 gün boyunca kalacak verilere yöneliktir. Seyrek erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Kısa süreli yedekleme ve olağanüstü durum kurtarma veri kümeleri.
- Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
- Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*Örneğin*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)

## <a name="archive-access-tier"></a>Arşiv erişim katmanı

Arşiv erişim katmanı, en düşük depolama maliyetine ve sık erişimli ve seyrek erişimli katmanlar için kıyasla daha yüksek veri alma maliyetine sahiptir. Bu katman, birkaç saatlik alma gecikmesinden etkilenmeyecek ve arşiv katmanında en az 180 gün kalacak verilere yöneliktir.

Bir blob arşiv depolamasında olsa da, blob veriler çevrimdışı ve kopyalanan, üzerine veya değiştirilen okunamıyor. Arşiv depolama blob anlık görüntüsünü alamıyor. Ancak, çevrimiçi ve kullanılabilir, böylece blob ve özellikleri listelemek blob meta verilerini kalır. Arşiv bloblar için yalnızca GetBlobProperties, GetBlobMetadata, ListBlobs, SetBlobTier ve DeleteBlob işlemlerdir.

Arşiv erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Uzun vadeli yedekleme, ikincil yedekleme ve arşiv veri kümeleri
- Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*Örneğin*, kodlama diğer biçimlere dönüştürüldükten sonra ham medya dosyaları)
- Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*Örneğin*, güvenlik kamerası kayıtları, eski röntgen/MRI çekimleri sağlık kuruluşları, sesli kayıtlar ve müşteri dökümleri çağrıları için Finansal Hizmetler)

### <a name="blob-rehydration"></a>Blob yeniden doldurma

Arşiv depolama birimindeki verileri okumak için katmanı önce sık erişimli veya seyrek erişimli blob katmanı olarak değiştirmeniz gerekir. Bu işleme yeniden doldurma denir ve tamamlanması 15 saat kadar sürebilir. En iyi performans için büyük blob boyutları önerilir. Birkaç küçük blobu aynı anda yeniden doldurmak süreyi uzatabilir.

Yeniden doldurma sırasında katmanın değişip değişmediğini onaylamak için **Arşiv Durumu** blob özelliğini kontrol edebilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.  

## <a name="blob-level-tiering"></a>Blob düzeyinde katman ayarlama

Blob düzeyinde katman ayarlama, [Blob Katmanını Ayarlama](/rest/api/storageservices/set-blob-tier) adlı tek bir işlem kullanarak nesne düzeyinde verilerinizin katmanını değiştirmenize olanak verir. Bir blob’un erişim katmanını, kullanım şekli değiştikçe verileri hesapları arasında taşımaya gerek kalmadan sık erişimli, seyrek erişimli veya arşiv katmanları arasında kolayca değiştirebilirsiniz. Tüm katman değişiklikleri anında gerçekleşir. Ancak, bir blob Arşiv'den yeniden doldururken işlemin birkaç saat sürebilir.

Son blob katmanı değişikliğinin zamanı, **Erişim Katmanı Değişim Zamanı** blob özelliği aracılığıyla gösterilir. Bir blob arşiv katmanındaysa nedenle aynı blob'un karşıya yüklenmesine Bu senaryoda izin verilmez, üzerine yazılamaz. Durumda yeni blob yazıldı blobun katmanını devralan bir seyrek veya sık erişimli katmandaki blob üzerine yazabilirsiniz.

Aynı hesapta üç erişim katmanları tüm bloblar bulunabilir. Açıkça atanan bir katmanı olmayan tüm bloblar katmanı hesap erişim katmanının ayarını çıkarır. Erişim katmanı hesaptan algılanır, gördüğünüz **erişim katmanı çıkarılan** blob özelliğinin "true" ve blob ayarlandığı **erişim katmanı** blob özelliğinin hesap katmanıyla uyuştuğunu. Azure portalında erişim katmanını özelliği blob erişim katmanı ile görüntülenen çıkarılan (örneğin, **etkin (çıkarılan)** veya **seyrek erişilen (alındı)**).

> [!NOTE]
> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Anlık görüntüleri olan bir blok blobun katmanını da değiştiremezsiniz.

> [!NOTE]
> Bir blok blob depolama hesabında depolanan verileri olamaz şu anda katmanlı sık erişimli, seyrek erişimli veya arşiv kullanarak [Blob katmanını ayarlama](/rest/api/storageservices/set-blob-tier) veya Azure Blob Depolama Yaşam Döngüsü Yönetimi'ni kullanma.
> Verileri taşımak için zaman uyumlu olarak blobları blok blob'u depolama hesabından farklı bir hesap kullanarak sık erişim katmanı kopyalamanız gerekir [API URL'si gelen blok yerleştirme](/rest/api/storageservices/put-block-from-url) veya AzCopy destekleyen bu API sürümü.
> *URL'den blok yerleştirme* kopyalar zaman uyumlu olarak, sunucu üzerindeki verileri API çağrısını tamamlar yalnızca bir kez tüm verilerin, özgün sunucu konumundan hedef konuma taşınır anlamına gelir.

### <a name="blob-lifecycle-management"></a>BLOB yaşam döngüsü yönetimi

BLOB Depolama yaşam döngüsü yönetimi (Önizleme), verilerinizi en iyi erişim katmanına geçiş yapmak ve veri yaşam döngüsü sonunda süresi dolacak şekilde kullanabileceğiniz zengin, kural tabanlı bir ilke sunar. Bkz: [Azure Blob Depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md) daha fazla bilgi için.  

### <a name="blob-level-tiering-billing"></a>Blob düzeyinde katman ayarlama faturalandırması

Bir blobu bir katmana taşındığında (sık erişilen -> seyrek erişilen, sık erişilen -> Arşiv veya seyrek erişilen -> Arşiv), işlem nerede yazma işlemi (10.000 başına) ve hedef katmanın veri yazma (GB başına) ücretleri geçerli hedef katmanın yazma işlemi olarak faturalandırılır.

Bir blob daha sık erişilen bir katmana taşındığında (arşiv -> seyrek erişilen, arşiv -> sık erişilen veya seyrek erişimli -> sık erişilen), işlem nerede okuma işlemi (10.000 başına) ve kaynak katmanın veri alma (GB başına) ücretleri geçerli kaynak katmandan okuma olarak faturalandırılır. Katman değişiklikleri nasıl faturalandırılır aşağıdaki tabloda özetlenmiştir.

| | **Yazma ücretleri (işlem + erişim)** | **Okuma ücretleri (işlem + erişim)**
| ---- | ----- | ----- |
| **SetBlobTier yönü** | sık erişilen -> seyrek erişilen, sık Arşiv, seyrek erişilen -> Arşiv -> | Arşiv -> seyrek erişilen, arşiv -> sık erişimli, seyrek erişimli -> sık erişilen

Hesap katmanını sık erişilenden seyrek erişilene değiştirirseniz yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemleri (10.000 işlem başına) ücretlendirilir. Blob Depolama hesaplarında bu değişikliğin bir ücret yoktur. Blob depolama veya GPv2 hesabınızı seyrek erişilenden sık erişilene değiştirirseniz hem okuma işlemleri (10.000 işlem başına) hem de veri alma (GB başına) için ücretlendirilirsiniz. Seyrek erişim veya arşiv katmanı dışına taşınmış tüm bloblar için erken silme ücretleri de uygulanabilir.

### <a name="cool-and-archive-early-deletion"></a>Seyrek erişimli ve arşiv erken silme

GB başına aylık ücrete ek olarak seyrek erişimli katmana taşınan tüm bloblar (yalnızca GPv2 hesapları) 30 günlük seyrek erişim erken silme süresine tabidir ve arşiv katmanına taşınan tüm bloblar da 180 günlük arşiv erken silme süresine tabidir. Bu ücret eşit olarak bölünür. Örneğin, bir blob arşive taşınır ve ardından silinir veya 45 gün sonra sık erişimli katmana taşınırsa sizden o blobu arşivde 135 (180 eksi 45) gün depolamaya eşdeğer bir erken silme ücreti istenir.

Blob özelliğini kullanarak erken silme hesaplayabilir **oluşturulma zamanı**katman değişiklikleri erişimi yok, olmuştur. Aksi takdirde erişim katmanı son seyrek erişimli veya arşiv blob özelliği görüntüleyerek değiştirildiği kullanabilirsiniz: **erişim katmanını değiştirme zamanı**. Blob özellikleri hakkında daha fazla bilgi için bkz. [Blob özelliklerini alma](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties).

## <a name="comparing-block-blob-storage-options"></a>Blok blobu depolama seçeneklerini karşılaştırma

Aşağıdaki tabloda gösterilen karşılaştırması premium performans blok blob depolama ve sık erişimli, seyrek erişimli ve Arşiv katmanları erişin.

|                                           | **Premium performans** | **Sık erişimli katmanı** | **Seyrek erişimli katman** | **Arşiv katmanı**
| ----------------------------------------- | ---------------- | ------------ | ----- | ----- |
| **Kullanılabilirlik**                          | %99,9            | %99,9        | %99 | Yok |
| **Kullanılabilirlik** <br> **(RA-GRS okumaları)**  | Yok              | %99,99       | %99,9 | Yok |
| **Kullanım ücretleri**                         | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyeti | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyetleri | Daha düşük depolama maliyetleri, daha yüksek erişim ve işlem maliyetleri | En düşük depolama maliyeti, yüksek erişim ve işlem maliyetleri |
| **En düşük nesne boyutu**                   | Yok | Yok | Yok | Yok |
| **En az depolama süresi**              | Yok | Yok | 30 gün (yalnızca GPv2) | 180 gün
| **Gecikme süresi** <br> **(İlk bayta kadar süre)** | Tek basamaklı milisaniye | milisaniye | milisaniye | < 15 sa

> [!NOTE]
> Ölçeklenebilirlik ve performans hedefleri için bkz: [depolama hesapları için Azure depolama ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

- GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme.
- GPv2 veya Blob depolama hesabında blobun katmanını değiştirme.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

1. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.

1. Gereksinimleriniz için doğru erişim katmanını seçin: Ayarlama **erişim katmanı** ya da **seyrek erişimli** veya **etkin**.

1. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob Depolama hesabında blobun katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Depolama hesabınızdaki blobunuza gitmek için **tüm kaynakları**, depolama hesabınızı seçin, kapsayıcınızı seçin ve ardından blobunuzu seçin.

1. İçinde **Blob özellikleri** dikey penceresinde **erişim katmanı** seçmek için açılır menü **etkin**, **seyrek erişimli**, veya **arşiv**  erişim katmanı.

1. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Tüm depolama hesapları Blob Depolama her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Aşağıdaki fatura değerlendirmeleri göz önünde bulundurun:

- **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti erişim katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.
- **Veri erişim maliyetleri**: Düştükçe veri erişimi artış ücretleri. Seyrek erişimli ve Arşiv erişim katmanındaki veriler için okuma gigabayt başına veri erişim ücreti ödersiniz.
- **İşlem maliyetleri**: Tüm katmanlar için düştükçe artıran işlem başına ücret yoktur.
- **Coğrafi çoğaltma veri aktarımı maliyetleri**: Bu ücret, yalnızca coğrafi yapılandırılmış, GRS ve RA-GRS dahil olmak üzere çoğaltma ile hesapları için geçerlidir. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.
- **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı, gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.
- **Erişim katmanını değiştirme**: Hesap erişim katmanını seyrek erişimliden sık erişimliye değiştirmek depolama hesabında varolan tüm verilerin okunmasına eşit bir ücret doğurur. Ancak, hesap erişim katmanını sık erişilenden seyrek tüm verileri seyrek erişilen katmana (yalnızca GPv2 hesapları) yazma eşit bir ücret doğurur.

> [!NOTE]
> Blob Depolama hesapları için fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

## <a name="faq"></a>SSS

**Verilerime katman ayarlamak istediğimde Blob depolamayı mı yoksa GPv2 hesaplarını mı kullanmalıyım?**

Katman ayarlama için Blob depolama hesapları yerine GPv2 kullanmanızı öneririz. GPv2, Blob depolama hesaplarının desteklediği tüm özelliklerin yanı sıra başka birçoğunu da destekler. Blob depolama ile GPv2 ücretleri neredeyse aynıdır, ancak bazı yeni özellikler ve fiyat indirimleri yalnızca GPv2 hesaplarında kullanılabilir. GPv1 hesapları katman ayarlamayı desteklemez.

GPv1 ve GPv2 hesapları arasındaki fiyat yapısı farklıdır ve müşteriler GPv2 hesaplarını kullanmaya karar vermeden önce her ikisini de dikkatle değerlendirmelidir. Mevcut bir Blob depolama veya GPv1 hesabını tek tıklamada basit bir işlemle kolayca GPv2’ye dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

**Nesneleri üçüne de depolayabilir miyim (sık erişimli, seyrek erişimli ve Arşiv) erişim katmanları aynı hesaptaki?**

Evet. Hesap düzeyinde ayarlanan **Erişim Katmanı** özniteliği, o hesapta bulunan ve ayarlanmış açık bir katmanı olmayan tüm nesneler için varsayılan katman olur. Ancak blob düzeyinde katman ayarlama, hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanını açık olarak ayarlamanıza olanak tanır. Üç erişim katmanları bloblar (sık erişimli, seyrek erişimli veya arşiv) aynı hesap içinde bulunabilir.

**Blob veya GPv2 depolama hesabımın varsayılan erişim katmanını değiştirebilirim?**

Evet, ayarlayarak varsayılan erişim katmanını değiştirebilir miyim **erişim katmanı** depolama hesabındaki özniteliği. Erişim katmanını değiştirme ayarlanmış açık bir katmanı olmayan hesapta depolanan tüm nesnelere uygulanır. Erişim katmanı sık erişimli olarak değiştirmek yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemleri (10.000 başına) doğurur, seyrek erişimliden sık erişimliye hem okuma işlemleri (10.000 başına) maliyetleri doğurur ve Blob depolama alanındaki tüm bloblar için veri alma (GB başına) ücretleri ve GPv2 hesapları.

**Varsayılan hesap erişim katmanımı arşiv olarak ayarlayabilir miyim?**

Hayır. Yalnızca sık ve seyrek erişimli erişim katmanları varsayılan hesap erişim katmanı olarak ayarlanabilir. Arşiv yalnızca nesne düzeyinde ayarlanabilir.

**Hangi bölgeler sık erişimli, seyrek erişimli ve Arşiv katmanları erişim kullanılabilir?**

Sık ve seyrek erişimli erişim katmanları blob düzeyinde katman ayarlama ile birlikte tüm bölgelerde kullanılabilir. Arşiv depolama, başlangıçta yalnızca seçilmiş bölgelerde kullanılabilir. Tam liste için bkz. [Bölgelere göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/regions/services/).

**Seyrek erişimli erişim katmanındaki bloblar sık erişimli erişim katmanındaki yapılandırılanlardan farklı mı davranır?**

Sık erişimli erişim katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarındaki bloblarla aynı gecikme süresine sahip. Seyrek erişimli erişim katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarında benzer gecikme süresine (milisaniye cinsinden) bloblar sahip. Arşiv erişim katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarında birkaç saatlik gecikme süresine sahiptir.

Seyrek erişimli erişim katmanındaki bloblar, bir biraz daha düşük kullanılabilirlik hizmet düzeyine (SLA) sık erişimli erişim katmanında depolanan bloblara sahiptir. Daha fazla bilgi için bkz. [depolama SLA’sı](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**İşlemler sık erişimli, seyrek erişimli ve arşiv katmanları arasında aynı mıdır?**

Evet. Sık erişimli ve seyrek erişimli arasında tüm işlemleri %100 tutarlıdır. Blob özelliklerini/meta verileri silme, listeleme, alma ve blob katmanını ayarlama dahil olmak üzere tüm geçerli arşiv işlemleri sık ve seyrek erişim arasında %100 tutarlıdır. Bir blob arşiv katmanındayken okunamaz veya değiştirilemez.

**Blobu arşiv katmanından sık erişimli veya seyrek erişimli katmana yeniden doldururken işlemin ne zaman tamamlandığını nasıl bilebilirim?**

Yeniden doldurma işlemi sırasında, katman değişikliğinin ne zaman tamamlandığını onaylamak üzere **Arşiv Durumu** özniteliğini yoklamak için blob özelliklerini alma işlemini kullanabilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.  

**Blobun katmanını ayarladıktan sonra buna uygun fiyattan fatura almaya ne zaman başlarım?**

Her blob her zaman blobun tarafından belirtilen katmana göre faturalandırılır **erişim katmanı** özelliği. Bir blob için yeni bir katman ayarlarken **erişim katmanı** özelliği, tüm geçişler için yeni katmanı anında yansıtır. Ancak arşiv katmanındaki bir blobun sık veya seyrek erişim katmanına alınması birkaç saat sürebilir. Bu durumda, bu noktada, yeniden doldurma tamamlanana kadar arşiv fiyatlarından faturalandırılır **erişim katmanı** özelliği yeni katmanı yansıtır. Bu noktada sık erişimli veya seyrek erişimli fiyatı, bir blob için faturalandırılırsınız.

**Seyrek erişimli veya arşiv katmanındaki bir blobu silerken veya dışarı taşırken erken silme ücreti ödeyip ödemeyeceğimi nasıl anlarım?**

Sırasıyla 30 ve 180 günden önce seyrek erişimli (yalnızca GPv2 hesapları) ya da arşiv katmanından silinen veya dışarı taşınan tüm bloblar eşit dağıtılmış bir erken silme ücreti ödenmesini gerektirir. Nasıl bir blob seyrek erişimli veya arşiv katmanında denetleyerek süredir belirleyebilirsiniz **erişim katmanı değişim zamanı** blob son katman değişikliğinin bir damgasını sağlayan özelliği. Daha fazla bilgi için [seyrek erişimli ve Arşiv erken silme](#cool-and-archive-early-deletion).

**Hangi Azure araçları ve SDK’lar blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekliyor?**

Azure portalı, PowerShell ve CLI araçları ile .NET, Java, Python ve Node.js istemci kitaplıklarının tümü blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekler.  

**Sık, seyrek ve arşiv katmanlarına ne kadar veri depolayabilirim?**

Diğer sınırlarla birlikte veri depolama hesap düzeyinde ve erişim katmanı başına ayarlanır. Bu nedenle, tamamını tek bir katmanda veya üç katmanın hepsinde kullanmayı seçebilirsiniz. Daha fazla bilgi için [Azure depolama ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>GPv2 Blob depolama hesaplarında sık erişimli, seyrek erişimli ve arşiv seçeneklerini değerlendirme

[Bölgeye göre sık, seyrek ve arşiv kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Blob Depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md)

[Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)