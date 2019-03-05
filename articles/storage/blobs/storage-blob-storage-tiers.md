---
title: Premium, BLOB'ları - Azure depolama için sık erişimli, seyrek erişimli ve Arşiv depolama
description: Premium, Azure depolama hesapları için sık erişimli, seyrek erişimli ve Arşiv depolama.
services: storage
author: kuhussai
ms.service: storage
ms.topic: article
ms.date: 03/05/2019
ms.author: kuhussai
ms.subservice: blobs
ms.openlocfilehash: 4660a45014e6afdb091fb40b8fe7f03fdb647aab
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57339207"
---
# <a name="azure-blob-storage-premium-preview-hot-cool-and-archive-storage-tiers"></a>Azure Blob Depolama: Premium (Önizleme), sık erişimli, seyrek erişimli ve Arşiv depolama katmanları

## <a name="overview"></a>Genel Bakış

Azure depolama, Blob nesne verilerini en uygun maliyetli bir şekilde depolamanızı sağlayan farklı depolama katmanı sunuyor. Kullanılabilir katmanları mevcuttur:

- **Premium depolama (Önizleme)** sık erişilen veriler için yüksek performanslı donanımı sağlar.
 
- **Sık erişimli depolama**: sık erişilen verileri depolamak için optimize edilmiştir. 

- **Seyrek erişimli depolama** daha az sıklıkta erişilen ve en az 30 gün saklanan verileri depolamak için optimize edilmiştir.
 
- **Arşiv depolama** nadiren erişilen ve gecikme gereksinimleri esnek olan (saat bazında) en az 180 gün boyunca saklanan verileri depolamak için optimize edilmiştir.

Aşağıdaki konular farklı depolama katmanları eşlik eden:

- Arşiv depolama katmanı, yalnızca blob düzeyinde ise ve olmayan depolama hesap düzeyinde kullanılabilir.
 
- Seyrek erişimli depolama katmanındaki veriler, biraz daha düşük bir kullanılabilirliği kabul edebilir, ancak yine de yüksek dayanıklılık ve sık erişimli veriler kadar erişim süresi ve aktarım hızı benzer özelliklere gerektirir. Seyrek erişimli veriler, biraz daha düşük kullanılabilirlik SLA'sı ve yüksek erişim için sık erişimli verilere kıyasla daha düşük depolama maliyetleri için kabul edilebilir dengelemeler ücretlerdir.

- Çevrimdışı olan arşiv depolama, en düşük depolama maliyetini sunar ancak aynı zamanda en yüksek erişim maliyetine sahiptir.
 
- Hesap düzeyinde yalnızca sık ve seyrek erişimli depolama katmanları ayarlanabilir. Şu anda arşiv katmanı hesap düzeyinde ayarlanamaz.
 
- , Seyrek erişimli, seyrek ve Arşiv katmanları nesne düzeyinde ayarlanabilir.

Bulutta depolanan veriler üstel bir hızda artar. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, maliyetleri optimize etmek amacıyla erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, nasıl oluşturulduğu, işlendiği ve yaşam süresi boyunca nasıl erişildiği açısından farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında sık erişilebilir. Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir.

Bu veri senaryolarının her biri, belirli erişim düzeni için optimize edilmiş olan farklı bir depolama katmanından faydalanır. Sık erişimli, seyrek erişimli ve Arşiv depolama katmanları ile bu depolama katmanları ile Azure Blob Depolama ihtiyacına hitap farklı fiyatlandırma modelleriyle.

## <a name="storage-accounts-that-support-tiering"></a>Katman ayarlamayı destekleyen depolama hesapları

Yalnızca nesne depolama verilerinizi sık erişimli, seyrek erişimli veya Blob Depolama ve genel amaçlı v2 (GPv2) hesapları arşiv katmanı. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Bununla birlikte, müşteriler Azure portalında basit bir tek tıklama işlemiyle var olan GPv1 veya Blob depolama hesaplarını GPv2 hesaplarına kolayca dönüştürebilir. GPv2, bloblar, dosyalar ve kuyruklar için yeni bir fiyatlandırma yapısı ve yeni diğer birçok depolama özelliğine de erişim sağlar. Ayrıca, bazı yeni özelliklere geçmek ve fiyat indirimleri yalnızca GPv2 hesaplarda sunulur. Bu nedenle, müşteriler GPv2 hesaplarını kullanmayı değerlendirmeli ancak bazı iş yükleri GPv2’de GPv1’den daha pahalı olabileceği için bu hesapları yalnızca tüm hizmetlerin fiyatlarını gözden geçirdikten sonra kullanmalıdır. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

BLOB Depolama ve GPv2 hesapları sunmaya **erişim katmanı** varsayılan depolama katmanını sık veya seyrek erişimli ayarlanmış açık bir katmanı olmayan depolama hesabındaki tüm bloblar için olarak belirtmenizi sağlar hesap düzeyinde özniteliği Nesne düzeyinde. Katmanı nesne düzeyinde ayarlanan nesnelerde hesap katmanı geçerli olmaz. Arşiv katmanı yalnızca nesne düzeyinde uygulanabilir. Bu depolama katmanları arasında istediğiniz zaman geçiş yapabilirsiniz.

## <a name="premium-access-tier"></a>Premium erişim katmanı

Bir Premium erişim katmanı sık erişilen verilerin yüksek performanslı donanıma kullanılabilir hale getirir, önizlemede kullanılabilir. Bu katmanında depolanan veriler, daha düşük gecikme süresi ve geleneksel sabit sürücüler kıyasla daha yüksek işlem hızları için iyileştirilmiş katı hal sürücülerinde depolanır. Blok blobu depolama hesabı türü yalnızca Premium erişim katmanı kullanılabilir.

Bu katman, hızlı ve tutarlı yanıt süreleri gerektiren iş yükleri için idealdir. Etkileşimli video düzenleme, statik web içeriğini, çevrimiçi işlemler ve bunun gibi Premium erişim katmanı için iyi adaylar olduğu gibi son kullanıcılara içeren veriler. Bu katman, telemetri verilerini yakalama, Mesajlaşma ve veri dönüştürme gibi çok sayıda küçük işlemler gerçekleştiren iş yükleri için uyarlanmıştır.

## <a name="hot-access-tier"></a>Sık erişim katmanı

Sık erişimli depolama, seyrek erişimli ve depolama, ancak en düşük erişim maliyetini arşiv daha yüksek depolama maliyetleri vardır. Sık erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Etkin kullanımda olan veya sık erişilmesi beklenen (okunma ve üzerine yazılma) veriler.
* Seyrek erişimli depolama katmanına işlemeye ya da sonuçta geçişe hazırlanan veriler.

## <a name="cool-access-tier"></a>Seyrek erişim katmanı

Seyrek erişimli depolama katmanı, daha düşük depolama maliyetleri ve sık erişimli depolamayla karşılaştırıldığında daha yüksek erişim maliyetine sahiptir. Bu katman seyrek erişim katmanında en az 30 gün boyunca kalacak verilere yöneliktir. Seyrek erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Kısa süreli yedekleme ve olağanüstü durum kurtarma veri kümeleri.
* Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
* Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*Örneğin*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)

## <a name="archive-access-tier"></a>Arşiv erişim katmanı

Arşiv depolama, en düşük depolama maliyetine ve sık erişimli ve seyrek erişimli depolamaya kıyasla daha yüksek veri alma maliyetine sahiptir. Bu katman, birkaç saatlik alma gecikmesinden etkilenmeyecek ve Arşiv katmanında en az 180 gün boyunca kalacak verilere yöneliktir.

Bir blob arşiv depolamasında olsa da, blob veriler çevrimdışı ve kopyalanan, üzerine veya değiştirilen okunamıyor. Ya da arşiv depolamasında bir blobun anlık görüntüsünü alabilirsiniz. Ancak, çevrimiçi ve kullanılabilir, böylece blob ve özellikleri listelemek blob meta verilerini kalır. Arşiv’deki bloblar için yalnızca GetBlobProperties, GetBlobMetadata, ListBlobs, SetBlobTier ve DeleteBlob işlemleri geçerlidir. 


Arşiv depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Uzun vadeli yedekleme, ikincil yedekleme ve arşiv veri kümeleri
* Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*Örneğin*, kodlama diğer biçimlere dönüştürüldükten sonra ham medya dosyaları)
* Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*Örneğin* güvenlik kamerası kayıtları, sağlık kuruluşları için eski röntgen/MRI çekimleri, finans hizmetleri için sesli kayıtlar ve müşteri görüşmesi dökümleri)

### <a name="blob-rehydration"></a>Blob yeniden doldurma
Arşiv depolama birimindeki verileri okumak için önce sık veya seyrek erişimli blob katmanı değiştirmeniz gerekir. Bu işleme yeniden doldurma denir ve tamamlanması 15 saat kadar sürebilir. En iyi performans için büyük blob boyutları önerilir. Birkaç küçük blobu aynı anda yeniden doldurmak süreyi uzatabilir.

Yeniden doldurma sırasında katmanın değişip değişmediğini onaylamak için **Arşiv Durumu** blob özelliğini kontrol edebilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **erişim katmanı** blob özelliği, yeni sık erişimli veya seyrek erişimli katmanı gösterir.  

## <a name="blob-level-tiering"></a>Blob düzeyinde katman ayarlama

Blob düzeyinde katman ayarlama, [Blob Katmanını Ayarlama](/rest/api/storageservices/set-blob-tier) adlı tek bir işlem kullanarak nesne düzeyinde verilerinizin katmanını değiştirmenize olanak verir. Sık erişimli, seyrek erişimli veya arşiv katmanları arasında bir blobun erişim katmanını kullanım şekli değiştikçe verileri hesapları arasında taşımaya gerek kalmadan kolayca değiştirebilirsiniz. Tüm katman değişiklikleri anında gerçekleşir. Ancak, bir blob Arşiv'den yeniden doldururken işlemin birkaç saat sürebilir. 

Son blob katmanı değişikliğinin zamanı, **Erişim Katmanı Değişim Zamanı** blob özelliği aracılığıyla gösterilir. Bir blob arşiv katmanındaysa nedenle aynı blob'un karşıya yüklenmesine Bu senaryoda izin verilmez, üzerine yazılamaz. Bir blobu sık erişimli veya seyrek erişimli katman, hangi durumda yeni blob yazıldı blobun katmanını devralır üzerine yazabilirsiniz.

Aynı hesapta üç farklı depolama katmanına sahip bloblar birlikte bulunabilir. Açıkça atanan bir katmanı olmayan tüm bloblar katmanı hesap erişim katmanının ayarını çıkarır. Erişim katmanı hesaptan algılanır, gördüğünüz **erişim katmanı çıkarılan** blob özelliğinin "true" ve blob ayarlandığı **erişim katmanı** blob özelliğinin hesap katmanıyla uyuştuğunu. Azure portalında, erişim katmanı alındı özelliği, blob erişim katmanı ile birlikte gösterilir (örneğin, Sık Erişilen(alındı) veya Seyrek Erişilen (alındı)).

> [!NOTE]
> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Anlık görüntüleri olan bir blok blobun katmanını da değiştiremezsiniz.

> [!NOTE]
> Premium erişim katmanında depolanan veriler olamaz şu anda katmanlı sık erişimli, seyrek erişimli veya arşiv kullanarak [Blob katmanını ayarlama](/rest/api/storageservices/set-blob-tier) veya Azure Blob Depolama Yaşam Döngüsü Yönetimi'ni kullanma. Verileri taşımak için zaman uyumlu olarak blobları Premium erişimden sık erişimli kullanarak kopyalamanız gerekir [API URL'si gelen blok yerleştirme](/rest/api/storageservices/put-block-from-url) veya AzCopy destekleyen bu API sürümü. *URL'den blok yerleştirme* kopyalar zaman uyumlu olarak, sunucu üzerindeki verileri API çağrısını tamamlar yalnızca bir kez tüm verilerin, özgün sunucu konumundan hedef konuma taşınır anlamına gelir.

### <a name="blob-lifecycle-management"></a>BLOB yaşam döngüsü yönetimi
BLOB Depolama yaşam döngüsü yönetimi (Önizleme), verilerinizi en iyi erişim katmanına geçiş yapmak ve veri yaşam döngüsü sonunda süresi dolacak şekilde kullanabileceğiniz zengin, kural tabanlı bir ilke sunar. Bkz: [Azure Blob Depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md) daha fazla bilgi için.  

### <a name="blob-level-tiering-billing"></a>Blob düzeyinde katman ayarlama faturalandırması

Bir blobu bir katmana taşındığında (sık erişilen -> seyrek erişilen, sık erişilen -> Arşiv veya seyrek erişilen -> Arşiv), işlem nerede yazma işlemi (10.000 başına) ve hedef katmanın veri yazma (GB başına) ücretleri geçerli hedef katmanın yazma işlemi olarak faturalandırılır. Bir blob daha sık erişilen bir katmana taşındığında (arşiv -> seyrek erişilen, arşiv -> sık erişilen veya seyrek erişimli -> sık erişilen), işlem nerede okuma işlemi (10.000 başına) ve kaynak katmanın veri alma (GB başına) ücretleri geçerli kaynak katmandan okuma olarak faturalandırılır. Katman değişiklikleri nasıl faturalandırılır aşağıdaki tabloda özetlenmiştir.

| | **Yazma ücretleri (işlem + erişim)** | **Okuma ücretleri (işlem + erişim)** 
| ---- | ----- | ----- |
| **SetBlobTier yönü** | Sık erişilen -> seyrek erişilen, sık erişilen, arşiv -> seyrek erişilen, arşiv -> | Arşiv -> seyrek erişilen, arşiv -> sık erişimli, seyrek erişimli -> sık erişilen

Hesap katmanını sık erişimli'den seyrek erişilene değiştirirseniz yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemleri (10.000 başına) için ücretlendirilirsiniz. Blob Depolama hesaplarında bu değişikliğin bir ücret yoktur. Blob Depolama veya GPv2 hesabına seyrek erişilenden sık erişilene değiştirirseniz hem okuma işlemleri (10.000 başına) hem de veri alma (GB başına) için ücretlendirilirsiniz. Seyrek erişimli veya arşiv katmanı dışına taşınmış tüm bloblar için erken silme ücretleri de uygulanabilir.

### <a name="cool-and-archive-early-deletion"></a>Sık Erişimli ve Arşiv erken silme

Ek olarak GB aylık ücrete, (yalnızca GPv2 hesapları) seyrek erişimli katmanına taşınan tüm bloblar bir erken silme dönemine 30 gün içinde tabidir ve Arşiv katmanına taşınan tüm bloblar bir arşiv erken silme dönemine 180 gün tabidir. Bu ücret eşit olarak bölünür. Örneğin bir blob arşive taşınır ve ardından silinir veya sık erişimli katmanına taşınırsa, 45 gün sonra 135 (180 eksi 45) eşdeğer bir erken silme ücreti ödersiniz bu blobu Arşiv'de depolama gün.

Blob özelliğini kullanarak erken silme hesaplayabilir **oluşturulma zamanı**katman değişiklikleri erişimi yok, olmuştur. Aksi takdirde erişim katmanını seyrek erişimli veya arşiv blob özelliği'ni görüntüleyerek son değiştirildiği kullanabilirsiniz: **erişim katmanını değiştirme zamanı**. Blob özellikleri hakkında daha fazla bilgi için bkz. [Blob özelliklerini alma](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties).

## <a name="comparison-of-the-storage-tiers"></a>Depolama katmanlarının karşılaştırması

Sık erişimli bir karşılaştırmasını aşağıdaki tabloda gösterilmektedir, seyrek erişimli ve Arşiv depolama katmanları.

| | **Sık erişimli depolama katmanı** | **Seyrek erişimli depolama katmanı** | **Arşiv depolama katmanı**
| ---- | ----- | ----- | ----- |
| **Kullanılabilirlik** | %99,9 | %99 | Yok |
| **Kullanılabilirlik** <br> **(RA-GRS okumaları)**| %99,99 | %99,9 | Yok |
| **Kullanım ücretleri** | Daha yüksek depolama maliyetleri, daha düşük erişim ve işlem maliyetleri | Daha düşük depolama maliyetleri, daha yüksek erişim ve işlem maliyetleri | En düşük depolama maliyeti, yüksek erişim ve işlem maliyetleri |
| **En düşük nesne boyutu** | Yok | Yok | Yok |
| **En az depolama süresi** | Yok | 30 gün (yalnızca GPv2) | 180 gün
| **Gecikme süresi** <br> **(İlk bayta kadar süre)** | milisaniye | milisaniye | < 15 sa
| **Ölçeklenebilirlik ve Performans Hedefleri** | Genel amaçlı depolama hesaplarıyla aynı | Genel amaçlı depolama hesaplarıyla aynı | Genel amaçlı depolama hesaplarıyla aynı |

> [!NOTE]
> Blob Storage hesapları, genel amaçlı depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için [Azure Storage ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). 

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

* GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme.
* GPv2 veya Blob depolama hesabında blobun katmanını değiştirme.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.

4. Gereksinimlerinize uygun depolama katmanını seçin: Ayarlama **erişim katmanı** ya da **seyrek erişimli** veya **etkin**.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabında blobun katmanını değiştirin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınızdaki blobunuza gitmek için Tüm Kaynaklar’ı seçin, depolama hesabınızı seçin, kapsayıcınızı seçin ve ardından blobunuzu seçin.

3. Blob özellikleri dikey penceresinde, **Erişim katmanı** açılan menüsüne tıklayarak **Sık Erişilen**, **Seyrek Erişilen** veya **Arşiv** depolama katmanını seçin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Tüm depolama hesapları Blob Depolama her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Aşağıdaki fatura değerlendirmeleri göz önünde bulundurun:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti depolama katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.
* **Veri erişim maliyetleri**: Düştükçe veri erişimi artış ücretleri. Verileri seyrek erişimli ve Arşiv depolama katmanı için okuma gigabayt başına veri erişim ücreti ödersiniz.
* **İşlem maliyetleri**: Tüm katmanlar için düştükçe artıran işlem başına ücret yoktur.
* **Coğrafi çoğaltma veri aktarımı maliyetleri**: Bu ücret, yalnızca coğrafi yapılandırılmış, GRS ve RA-GRS dahil olmak üzere çoğaltma ile hesapları için geçerlidir. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.
* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı, gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.
* **Depolama katmanının değiştirilmesi**: Hesap Depolama katmanını seyrek erişimliden sık erişimli'den değiştirme depolama hesabında varolan tüm verilerin okunmasına eşit bir ücret doğurur. Ancak, hesap depolama katmanını sık erişimli'den seyrek Erişimliye değiştirmek, tüm verileri seyrek erişilen katmana (yalnızca GPv2 hesapları) yazma eşit bir ücret doğurur.

> [!NOTE]
> Blob Depolama hesapları için fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

## <a name="faq"></a>SSS

**Verilerime katman ayarlamak istediğimde Blob depolamayı mı yoksa GPv2 hesaplarını mı kullanmalıyım?**

Katman ayarlama için Blob depolama hesapları yerine GPv2 kullanmanızı öneririz. GPv2, Blob depolama hesaplarının desteklediği tüm özelliklerin yanı sıra başka birçoğunu da destekler. Blob depolama ile GPv2 ücretleri neredeyse aynıdır, ancak bazı yeni özellikler ve fiyat indirimleri yalnızca GPv2 hesaplarında kullanılabilir. GPv1 hesapları katman ayarlamayı desteklemez.

GPv1 ve GPv2 hesapları arasındaki fiyat yapısı farklıdır ve müşteriler GPv2 hesaplarını kullanmaya karar vermeden önce her ikisini de dikkatle değerlendirmelidir. Mevcut bir Blob depolama veya GPv1 hesabını tek tıklamada basit bir işlemle kolayca GPv2’ye dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

**Üç (sık erişimli, seyrek erişimli ve Arşiv) farklı depolama katmanına aynı hesaptaki nesneleri depolayabilir?**

Evet. Hesap düzeyinde ayarlanan **Erişim Katmanı** özniteliği, o hesapta bulunan ve ayarlanmış açık bir katmanı olmayan tüm nesneler için varsayılan katman olur. Ancak blob düzeyinde katman ayarlama, hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanını açık olarak ayarlamanıza olanak tanır. Aynı hesapta üç depolama katmanları (sık erişimli, seyrek erişimli veya arşiv) bloblar bulunabilir.

**Blob veya GPv2 depolama hesabımın varsayılan depolama katmanını değiştirebilir miyim?**

Evet, depolama hesabındaki **Erişim katmanı** özniteliğini ayarlayarak varsayılan depolama katmanını değiştirebilirsiniz. Depolama katmanının değiştirilmesi, hesabta depolanmış ve açıkça belirlenmiş bir katmanı olmayan tüm nesneler için geçerli olur. Sık erişimli depolama katmanını seyrek erişimliye değiştirmek yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemleri (10.000 başına) doğurur, seyrek erişimliden sık erişimli'den hem okuma işlemleri (10.000 başına) maliyetleri doğurur ve Blob depolama alanındaki tüm bloblar için veri alma (GB başına) ücretleri ve GPv2 hesapları.

**My varsayılan hesap erişim katmanı Arşiv'e ayarlayabilir miyim?**

Hayır. Yalnızca sık ve seyrek erişimli depolama katmanları varsayılan hesap erişim katmanı olarak ayarlanabilir. Arşiv yalnızca nesne düzeyinde ayarlanabilir.

**Hangi bölgelerde sık erişimli, seyrek erişimli ve Arşiv depolama katmanları kullanılabilir?**

Sık ve seyrek erişimli depolama katmanları blob düzeyinde katman ayarlama ile birlikte tüm bölgelerde kullanılabilir. Arşiv depolama, başlangıçta yalnızca seçilmiş bölgelerde kullanılabilir. Tam liste için bkz. [Bölgelere göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/regions/services/).

**Seyrek erişimli depolama katmanındaki bloblar sık erişimli depolama katmanındaki yapılandırılanlardan farklı mı davranır?**

Sık erişimli depolama katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarındaki bloblarla aynı gecikme süresine sahip. Seyrek erişimli depolama katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarında benzer gecikme süresine (milisaniye cinsinden) bloblar sahip. Arşiv depolama katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarında birkaç saatlik gecikme süresine sahiptir.

Seyrek erişimli depolama katmanındaki bloblar, bir biraz daha düşük kullanılabilirlik hizmet düzeyine (SLA) sık erişimli depolama katmanında depolanan bloblara sahiptir. Daha fazla bilgi için bkz. [depolama SLA’sı](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**İşlemler sık erişimli, seyrek erişimli ve Arşiv katmanları arasında aynı mı?**

Evet. Sık ve seyrek erişimli arasında tüm işlemleri % 100 tutarlıdır ' dir. Silme de dahil olmak üzere, tüm geçerli arşiv işlemleri sık ve seyrek erişimli ile tutarlı %100 listesi, get blob özelliklerini/meta verilerini ve blob katmanını ayarlama olan. Bir blob okunabilir veya arşiv katmanındayken içinde değiştirilebilir.

**Yeniden doldurma işlemi tamamlandıktan sonra bir blobu arşiv katmanından sık erişimli veya seyrek erişimli katman için yeniden doldururken işlemin ne nasıl bilebilirim?**

Yeniden doldurma işlemi sırasında, katman değişikliğinin ne zaman tamamlandığını onaylamak üzere **Arşiv Durumu** özniteliğini yoklamak için blob özelliklerini alma işlemini kullanabilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **erişim katmanı** blob özelliği, yeni sık erişimli veya seyrek erişimli katmanı gösterir.  

**Blobun katmanını ayarladıktan sonra buna uygun fiyattan fatura almaya ne zaman başlarım?**

Her blob her zaman blobun tarafından belirtilen katmana göre faturalandırılır **erişim katmanı** özelliği. Bir blob için yeni bir katman ayarlarken **erişim katmanı** özelliği, tüm geçişler için yeni katmanı anında yansıtır. Ancak, bir blobu arşiv katmanından sık erişimli veya seyrek erişimli katman için yeniden doldururken işlemin birkaç saat sürebilir. Bu durumda, bu noktada, yeniden doldurma tamamlanana kadar arşiv fiyatlarından faturalandırılır **erişim katmanı** özelliği yeni katmanı yansıtır. Bu noktada bu bir blobu sık erişimli veya seyrek erişimli oranı için faturalandırılırsınız.

**Bir erken silme ücreti siliniyor veya bir blobun seyrek erişimli veya arşiv katmanı taşıma ödemem, nasıl belirlerim?**

Silinen veya seyrek erişimli (yalnızca GPv2 hesapları) ya da arşiv katmanından ve 180 günden önce 30 dışında sırasıyla taşınan tüm bloblar eşit dağıtılmış bir erken silme ücreti ödenmesini gerektirir. Nasıl bir blob seyrek erişimli veya arşiv katmanında denetleyerek süredir belirleyebilirsiniz **erişim katmanı değişim zamanı** blob son katman değişikliğinin bir damgasını sağlayan özelliği. Daha fazla bilgi için [seyrek erişimli ve Arşiv erken silme](#cool-and-archive-early-deletion).

**Hangi Azure Araçları ve SDK'lar blob düzeyinde katman ayarlamayı ve Arşiv depolamayı destekliyor?**

Azure portalı, PowerShell ve CLI araçları ve .NET, Java, Python ve Node.js istemci kitaplıklarının tümü blob düzeyinde katman ayarlamayı ve Arşiv depolama desteği.  

**Ne kadar veri ben sık erişimli, seyrek erişimli ve Arşiv katmanları depolayabilir miyim?**

Diğer sınırlarla birlikte veri depolama da depolama katmanına göre değil hesap düzeyinde ayarlanır. Bu nedenle, tamamını tek bir katmanda veya üç katmanın hepsinde kullanmayı seçebilirsiniz. Daha fazla bilgi için [Azure Storage ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>GPv2 Blob Depolama hesaplarında sık erişimli, seyrek erişimli ve Arşiv değerlendir

[Sık erişimli, seyrek erişimli ve Arşiv bölgelere göre kullanılabilirliğini denetleyin](https://azure.microsoft.com/regions/#services)

[Azure Blob Depolama yaşam döngüsünü yönetme](storage-lifecycle-management-concepts.md)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md)

[Sık erişimli, seyrek erişimli ve Arşiv Blob Depolama ve GPv2 hesaplarında bölgeye göre fiyatlandırma denetleyin](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
