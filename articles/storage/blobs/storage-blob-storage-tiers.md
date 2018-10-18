---
title: Premium, sık erişimli, seyrek erişimli ve Arşiv depolama BLOB'ları - Azure depolama için
description: Premium, sık erişimli, seyrek erişimli ve Arşiv depolama, Azure depolama hesapları için.
services: storage
author: kuhussai
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: kuhussai
ms.component: blobs
ms.openlocfilehash: 922e7ed5d55f50b2069dad71ead73d9ef4475ed0
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389913"
---
# <a name="azure-blob-storage-premium-preview-hot-cool-and-archive-storage-tiers"></a>Azure Blob Depolama: Premium (Önizleme), sık erişimli, seyrek erişimli ve Arşiv depolama katmanları

## <a name="overview"></a>Genel Bakış

Azure depolama, Blob nesne verilerini en uygun maliyetli bir şekilde depolamanızı sağlayan farklı depolama katmanı sunuyor. Kullanılabilir katmanları mevcuttur:

- **Premium depolama (Önizleme)** sık erişilen veriler için yüksek performanslı donanımı sağlar.
 
- **Sık erişimli depolama**: sık erişilen verileri depolamak için optimize edilmiştir. 

- **Seyrek erişimli depolama** daha az sıklıkta erişilen ve en az 30 gün saklanan verileri depolamak için optimize edilmiştir.
 
- **Arşiv depolama** nadiren erişilen ve gecikme gereksinimleri esnek olan (saat bazında) en az 180 gün boyunca saklanan verileri depolamak için optimize edilmiştir.

Aşağıdaki konular farklı depolama katmanları eşlik eden:

- Arşiv depolama katmanı, yalnızca blob düzeyinde kullanılabilir; depolama hesabı düzeyinde kullanılamaz.
 
- Seyrek erişimli depolama katmanındaki veriler, biraz daha düşük bir kullanılabilirliği kabul edebilir, ancak yine de sık erişimli veriler kadar erişim süresi ve verimlilik gerektirir. Seyrek erişimli veriler için, sık erişimli verilere kıyasla biraz daha düşük kullanılabilirlik SLA'sı ve yüksek erişim maliyetleri, daha düşük depolama maliyetleri için kabul edilebilir tercihlerdir.

- Çevrimdışı olan arşiv depolama, en düşük depolama maliyetini sunar ancak aynı zamanda en yüksek erişim maliyetine sahiptir.
 
- Hesap düzeyinde yalnızca sık erişimli ve seyrek erişimli depolama katmanları (arşiv değil) ayarlanabilir.
 
- Tüm katmanlarda, nesne düzeyinde ayarlanabilir.

Bulutta depolanan veriler üstel bir hızda artar. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, maliyetleri optimize etmek amacıyla erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, nasıl oluşturulduğu, işlendiği ve yaşam süresi boyunca nasıl erişildiği açısından farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında sık erişilebilir. Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir.

Bu veri senaryolarının her biri, belirli erişim düzeni için optimize edilmiş olan farklı bir depolama katmanından faydalanır. Sık erişimli, seyrek erişimli ve arşiv depolama katmanlarının kullanılmaya başlanmasıyla, Azure Blob Depolama farklı fiyatlandırma modelleriyle bu ayrılmış depolama katmanları ihtiyacına hitap ediyor.

## <a name="storage-accounts-that-support-tiering"></a>Katman ayarlamayı destekleyen depolama hesapları

Blob depolama veya Genel Amaçlı v2 (GPv2) hesaplarında nesne depolama verilerinizi yalnızca sık erişimli, seyrek erişimli ve arşiv seçeneklerine katman ayarlayabilirsiniz. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Bununla birlikte, müşteriler Azure portalında basit bir tek tıklama işlemiyle var olan GPv1 veya Blob depolama hesaplarını GPv2 hesaplarına kolayca dönüştürebilir. GPv2, bloblar, dosyalar ve kuyruklar için yeni bir fiyatlandırma yapısı ve yeni diğer birçok depolama özelliğine de erişim sağlar. Ayrıca, bazı yeni özelliklere geçmek ve fiyat indirimleri yalnızca GPv2 hesaplarda sunulur. Bu nedenle, müşteriler GPv2 hesaplarını kullanmayı değerlendirmeli ancak bazı iş yükleri GPv2’de GPv1’den daha pahalı olabileceği için bu hesapları yalnızca tüm hizmetlerin fiyatlarını gözden geçirdikten sonra kullanmalıdır. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

Blob depolama ve GPv2 hesapları, **Erişim Katmanı** özniteliğini nesne düzeyinde kullanıma sunar; bu ise nesne düzeyinde ayarlanmış katmanı olmayan depolama hesabındaki tüm bloblar için varsayılan depolama katmanını sık veya seyrek erişimli olarak belirtmenizi sağlar. Katmanı nesne düzeyinde ayarlanan nesnelerde hesap katmanı geçerli olmaz. Arşiv katmanı yalnızca nesne düzeyinde uygulanabilir. Bu depolama katmanları arasında istediğiniz zaman geçiş yapabilirsiniz.

## <a name="premium-access-tier"></a>Premium erişim katmanı

Bir Premium erişim katmanı sık erişilen verilerin yüksek performanslı donanıma kullanılabilir hale getirir, önizlemede kullanılabilir. Bu katmanında depolanan veriler, geleneksel sabit sürücüler için daha yüksek bir işlem fiyatları kıyasla daha düşük gecikme süresi için iyileştirilmiş katı hal sürücülerinde depolanır. Blok blobu depolama hesabı türü yalnızca Premium erişim katmanı kullanılabilir.

Bu katman, hızlı ve tutarlı yanıt süreleri gerektiren iş yükleri için idealdir. Etkileşimli video düzenleme, statik web içeriğini, çevrimiçi işlemler ve bunun gibi Premium erişim katmanı için iyi bir aday olan gibi son kullanıcılara içeren veriler. Bu katman, telemetri verilerini yakalama, Mesajlaşma ve veri dönüştürme gibi çok sayıda küçük işlemler gerçekleştiren iş yükleri için uyarlanmıştır.

Bu katmanı kullanmak için yeni bir blok blobu depolama hesabı sağlayın ve kapsayıcılar ve bloblar kullanarak oluşturmaya başlamak [Blob hizmeti REST API'si](/rest/api/storageservices/blob-service-rest-api), [AzCopy](/azure/storage/common/storage-use-azcopy), veya [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

Önizleme sırasında Premium erişim katmanı:

- Yerel olarak yedekli depolama (LRS) kullanılabilir
- Yalnızca şu bölgelerde kullanılabilir: Doğu ABD 2, ABD Orta ve ABD Batı
- Otomatik katmanlama ve veri yaşam döngüsü yönetimini desteklemez

Premium erişim katmanı önizlemesi için kaydetme hakkında bilgi için bkz: [Azure Premium Blob Depolama ile tanışın](http://aka.ms/premiumblob).

## <a name="hot-access-tier"></a>Sık erişim katmanı

Sık erişimli depolama, seyrek erişimli ve arşiv depolamaya göre daha yüksek depolama maliyetine sahiptir, ancak en düşük erişim maliyetini sunar. Sık erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Etkin kullanımda olan veya sık erişilmesi beklenen (okunma ve üzerine yazılma) veriler.
* Seyrek erişimli depolama katmanına işlemeye ya da sonuçta geçişe hazırlanan veriler.

## <a name="cool-access-tier"></a>Seyrek erişim katmanı

Seyrek erişimli depolama katmanı, sık erişimli depolamaya kıyasla daha düşük depolama maliyetine ve daha yüksek erişim maliyetine sahiptir. Bu katman, seyrek erişim katmanında en az 30 gün boyunca kalacak verilere yöneliktir. Seyrek erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Kısa süreli yedekleme ve olağanüstü durum kurtarma veri kümeleri.
* Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
* Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*Örneğin*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)

## <a name="archive-access-tier"></a>Arşiv erişim katmanı

Arşiv depolama, sık erişimli ve seyrek erişimli depolamayla karşılaştırıldığında en düşük depolama maliyetine ve daha yüksek veri alma maliyetine sahiptir. Bu katman, birkaç saatlik alma gecikmesinden etkilenmeyecek ve arşiv katmanında en az 180 gün kalacak verilere yöneliktir.

Bir blob arşiv depolamasında bulunduğu sürece çevrimdışıdır (meta veriler hariç, bunlar çevrimiçidir ve kullanılabilir), okunamaz, kopyalanamaz, üzerine yazılamaz veya değiştirilemez. Arşiv depolamasındaki blobların anlık görüntüsünü de alamazsınız. Ancak, silme, listeleme, blob özelliklerini/meta verilerini alma işlemleri için mevcut işlemleri kullanabilir ve blob katmanını değiştirebilirsiniz.

Arşiv depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Uzun vadeli yedekleme, ikincil yedekleme ve arşiv veri kümeleri
* Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*Örneğin*, kodlama diğer biçimlere dönüştürüldükten sonra ham medya dosyaları)
* Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*Örneğin* güvenlik kamerası kayıtları, sağlık kuruluşları için eski röntgen/MRI çekimleri, finans hizmetleri için sesli kayıtlar ve müşteri görüşmesi dökümleri)

### <a name="blob-rehydration"></a>Blob yeniden doldurma
Arşiv depolama birimindeki verileri okumak için katmanı önce sık erişimli veya seyrek erişimli blob katmanı olarak değiştirmeniz gerekir. Bu işleme yeniden doldurma denir ve tamamlanması 15 saat kadar sürebilir. En iyi performans için büyük blob boyutları önerilir. Birkaç küçük blobu aynı anda yeniden doldurmak süreyi uzatabilir.

Yeniden doldurma sırasında katmanın değişip değişmediğini onaylamak için **Arşiv Durumu** blob özelliğini kontrol edebilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.  

## <a name="blob-level-tiering"></a>Blob düzeyinde katman ayarlama

Blob düzeyinde katman ayarlama, [Blob Katmanını Ayarlama](/rest/api/storageservices/set-blob-tier) adlı tek bir işlem kullanarak nesne düzeyinde verilerinizin katmanını değiştirmenize olanak verir. Bir blob’un erişim katmanını, kullanım şekli değiştikçe verileri hesapları arasında taşımaya gerek kalmadan sık erişimli, seyrek erişimli veya arşiv katmanları arasında kolayca değiştirebilirsiniz. Tüm katman değişiklikleri anında gerçekleşir. Ancak, bir blob Arşiv'den yeniden doldururken işlemin birkaç saat sürebilir. 

Son blob katmanı değişikliğinin zamanı, **Erişim Katmanı Değişim Zamanı** blob özelliği aracılığıyla gösterilir. Bir blob arşiv katmanındaysa nedenle aynı blob'un karşıya yüklenmesine Bu senaryoda izin verilmez, üzerine yazılamaz. Durumda yeni blob yazıldı blobun katmanını devralan bir seyrek veya sık erişimli katmandaki blob üzerine yazabilirsiniz.

Aynı hesapta üç farklı depolama katmanına sahip bloblar birlikte bulunabilir. Açıkça atanan bir katmanı olmayan tüm bloblar katmanı hesap erişim katmanının ayarını çıkarır. Erişim katmanı hesaptan algılanır, gördüğünüz **erişim katmanı çıkarılan** blob özelliğinin "true" ve blob ayarlandığı **erişim katmanı** blob özelliğinin hesap katmanıyla uyuştuğunu. Azure portalında, erişim katmanı alındı özelliği, blob erişim katmanı ile birlikte gösterilir (örneğin, Sık Erişilen(alındı) veya Seyrek Erişilen (alındı)).

> [!NOTE]
> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Anlık görüntüleri olan bir blok blobun katmanını da değiştiremezsiniz.

Premium erişim katmanında depolanan veriler kullanılamaz katmanlı sık erişimli, seyrek erişimli veya arşiv kullanarak [Blob katmanını ayarlama](/rest/api/storageservices/set-blob-tier) veya Azure Blob Depolama Yaşam Döngüsü Yönetimi'ni kullanma. Verileri taşımak için zaman uyumlu olarak blobları Premium erişimden sık erişimli kullanarak kopyalamanız gerekir [API URL'si gelen blok yerleştirme](/rest/api/storageservices/put-block-from-url) veya AzCopy destekleyen bu API sürümü. *URL'den blok yerleştirme* kopyalar zaman uyumlu olarak, sunucu üzerindeki verileri API çağrısını tamamlar yalnızca bir kez tüm verilerin, özgün sunucu konumundan hedef konuma taşınır anlamına gelir.

### <a name="blob-lifecycle-management"></a>BLOB yaşam döngüsü yönetimi
BLOB Depolama yaşam döngüsü yönetimi (Önizleme), verilerinizi en iyi erişim katmanına geçiş yapmak ve veri yaşam döngüsü sonunda süresi dolacak şekilde kullanabileceğiniz zengin, kural tabanlı bir ilke sunar. Bkz: [Azure Blob Depolama yaşam döngüsünü yönetme](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts) daha fazla bilgi için.  

### <a name="blob-level-tiering-billing"></a>Blob düzeyinde katman ayarlama faturalandırması

Bir blobu bir katmana taşındığında (sık erişilen -> seyrek erişilen, sık erişilen -> Arşiv veya seyrek erişilen -> Arşiv), işlem nerede yazma işlemi (10.000 başına) ve hedef katmanın veri yazma (GB başına) ücretleri geçerli hedef katmanın yazma işlemi olarak faturalandırılır. Bir blob daha sık erişilen bir katmana taşınırsa (arşiv->seyrek erişilen, arşiv->sık erişilen veya seyrek erişilen->sık erişilen), bu işlem kaynak katmandan okuma olarak faturalandırılır ve kaynak katmanın okuma işlemi (10.000 işlem başına) ile veri alma (GB başına) ücretleri uygulanır.

Hesap katmanını sık erişilenden seyrek erişilene değiştirirseniz yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemleri (10.000 işlem başına) ücretlendirilir. Blob Depolama hesaplarında bu değişikliğin bir ücret yoktur. Blob depolama veya GPv2 hesabınızı seyrek erişilenden sık erişilene değiştirirseniz hem okuma işlemleri (10.000 işlem başına) hem de veri alma (GB başına) için ücretlendirilirsiniz. Seyrek erişim veya arşiv katmanı dışına taşınmış tüm bloblar için erken silme ücretleri de uygulanabilir.

### <a name="cool-and-archive-early-deletion"></a>Seyrek erişimli ve arşiv erken silme

GB başına aylık ücrete ek olarak seyrek erişimli katmana taşınan tüm bloblar (yalnızca GPv2 hesapları) 30 günlük seyrek erişim erken silme süresine tabidir ve arşiv katmanına taşınan tüm bloblar da 180 günlük arşiv erken silme süresine tabidir. Bu ücret eşit olarak bölünür. Örneğin, bir blob arşive taşınır ve ardından silinir veya 45 gün sonra sık erişimli katmana taşınırsa sizden o blobu arşivde 135 (180 eksi 45) gün depolamaya eşdeğer bir erken silme ücreti istenir.

## <a name="comparison-of-the-storage-tiers"></a>Depolama katmanlarının karşılaştırması

Aşağıdaki tabloda, sık erişimli, seyrek erişimli ve arşiv depolama katmanlarının karşılaştırması gösterilmiştir.

| | **Sık erişimli depolama katmanı** | **Seyrek erişimli depolama katmanı** | **Arşiv depolama katmanı**
| ---- | ----- | ----- | ----- |
| **Kullanılabilirlik** | %99,9 | %99 | Yok |
| **Kullanılabilirlik** <br> **(RA-GRS okumaları)**| %99,99 | %99,9 | Yok |
| **Kullanım ücretleri** | Yüksek depolama maliyeti, düşük erişim ve işlem maliyetleri | Düşük depolama maliyeti, yüksek erişim ve işlem maliyetleri | En düşük depolama maliyeti, en yüksek erişim ve işlem maliyetleri |
| **En düşük nesne boyutu** | Yok | Yok | Yok |
| **En az depolama süresi** | Yok | 30 gün (yalnızca GPv2) | 180 gün
| **Gecikme süresi** <br> **(İlk bayta kadar süre)** | milisaniye | milisaniye | < 15 sa
| **Ölçeklenebilirlik ve Performans Hedefleri** | Genel amaçlı depolama hesaplarıyla aynı | Genel amaçlı depolama hesaplarıyla aynı | Genel amaçlı depolama hesaplarıyla aynı |

> [!NOTE]
> Blob Storage hesapları, genel amaçlı depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

* GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme.
* GPv2 veya Blob depolama hesabında blobun katmanını değiştirme.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.

4. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabında blobun katmanını değiştirin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınızdaki blobunuza gitmek için Tüm Kaynaklar’ı seçin, depolama hesabınızı seçin, kapsayıcınızı seçin ve ardından blobunuzu seçin.

3. Blob özellikleri dikey penceresinde, **Erişim katmanı** açılan menüsüne tıklayarak **Sık Erişilen**, **Seyrek Erişilen** veya **Arşiv** depolama katmanını seçin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Tüm depolama hesapları Blob Depolama her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Aşağıdaki fatura değerlendirmeleri göz önünde bulundurun:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti depolama katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.
* **Veri erişimi maliyetleri**: Katmanın erişimi sıklığı düştükçe veri erişimi ücretleri artar. Seyrek erişimli depolama ve arşiv depolama katmanındaki verilerde, okuma işlemleri için erişilen gigabayt veri başına ücretlendirilirsiniz.
* **İşlem maliyetleri**: Tüm katmanlarda, erişim sıklığı düştükçe artan bir işlem başına ücret uygulanır.
* **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu ücret, GRS ve RA-GRS dahil olmak üzere yalnızca coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.
* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.
* **Depolama katmanını değiştirme**: Hesap depolama katmanını seyrek erişimliden sık erişimliye değiştirmek, depolama hesabında mevcut tüm verilerin okunmasına eşit bir ücret doğurur. Ancak, hesap depolama katmanını sık erişilenden seyrek erişilene değiştirmek, tüm verileri seyrek erişilen katmana yazma (yalnızca GPv2 hesapları) maliyetine eşit bir ücret yansıtır.

> [!NOTE]
> Blob Depolama hesapları için fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

## <a name="faq"></a>SSS

**Verilerime katman ayarlamak istediğimde Blob depolamayı mı yoksa GPv2 hesaplarını mı kullanmalıyım?**

Katman ayarlama için Blob depolama hesapları yerine GPv2 kullanmanızı öneririz. GPv2, Blob depolama hesaplarının desteklediği tüm özelliklerin yanı sıra başka birçoğunu da destekler. Blob depolama ile GPv2 ücretleri neredeyse aynıdır, ancak bazı yeni özellikler ve fiyat indirimleri yalnızca GPv2 hesaplarında kullanılabilir. GPv1 hesapları katman ayarlamayı desteklemez.

GPv1 ve GPv2 hesapları arasındaki fiyat yapısı farklıdır ve müşteriler GPv2 hesaplarını kullanmaya karar vermeden önce her ikisini de dikkatle değerlendirmelidir. Mevcut bir Blob depolama veya GPv1 hesabını tek tıklamada basit bir işlemle kolayca GPv2’ye dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

**Nesneleri aynı hesapta üç depolama katmanının (sık erişimli, seyrek erişimli ve arşiv) üçüne de depolayabilir miyim?**

Evet. Hesap düzeyinde ayarlanan **Erişim Katmanı** özniteliği, o hesapta bulunan ve ayarlanmış açık bir katmanı olmayan tüm nesneler için varsayılan katman olur. Ancak blob düzeyinde katman ayarlama, hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanını açık olarak ayarlamanıza olanak tanır. Aynı hesapta, üç depolama katmanının (sık erişilen, seyrek erişilen veya arşiv) tümüne ait bloblar bulunabilir.

**Blob veya GPv2 depolama hesabımın varsayılan depolama katmanını değiştirebilir miyim?**

Evet, depolama hesabındaki **Erişim katmanı** özniteliğini ayarlayarak varsayılan depolama katmanını değiştirebilirsiniz. Depolama katmanının değiştirilmesi, hesabta depolanmış ve açıkça belirlenmiş bir katmanı olmayan tüm nesneler için geçerli olur. Sık erişimli depolama katmanını seyrek erişimli olarak değiştirmek, yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemi maliyetleri (10.000 işlem başına), seyrek erişimliden sık erişimliye geçmek de Blob depolama ve GPv2 hesaplarındaki tüm bloblar için hem okuma işlemi (10.000 işlem başına) hem de veri alma (GB başına) maliyetleri doğurur.

**Varsayılan hesap erişim katmanımı arşiv olarak ayarlayabilir miyim?**

Hayır. Yalnızca sık ve seyrek erişimli depolama katmanları varsayılan hesap erişim katmanı olarak ayarlanabilir. Arşiv yalnızca nesne düzeyinde ayarlanabilir.

**Sık erişimli, seyrek erişimli ve arşiv depolama katmanları hangi bölgelerde kullanılabilir ?**

Sık ve seyrek erişimli depolama katmanları blob düzeyinde katman ayarlama ile birlikte tüm bölgelerde kullanılabilir. Arşiv depolama, başlangıçta yalnızca seçilmiş bölgelerde kullanılabilir. Tam liste için bkz. [Bölgelere göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/regions/services/).

**Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanındakilerden farklı mı davranır?**

Sık erişimli depolama katmanındaki bloblar GPv1, GPv2 ve Blob depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek erişimli depolama katmanındaki bloblar GPv1, GPv2 ve Blob depolama hesaplarındaki bloblara benzer gecikme süresine (milisaniye olarak) sahiptir. Arşiv depolama katmanındaki bloblar, GPv1, GPv2 ve Blob depolama hesaplarında birkaç saatlik gecikme süresine sahiptir.

Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanında depolanan bloblara göre daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [depolama SLA’sı](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**İşlemler sık erişimli, seyrek erişimli ve arşiv katmanları arasında aynı mıdır?**

Evet. Sık erişimli ve seyrek erişimli arasında tüm işlemleri %100 tutarlıdır. Blob özelliklerini/meta verileri silme, listeleme, alma ve blob katmanını ayarlama dahil olmak üzere tüm geçerli arşiv işlemleri sık ve seyrek erişim arasında %100 tutarlıdır. Bir blob arşiv katmanındayken okunamaz veya değiştirilemez.

**Blobu arşiv katmanından sık erişimli veya seyrek erişimli katmana yeniden doldururken işlemin ne zaman tamamlandığını nasıl bilebilirim?**

Yeniden doldurma işlemi sırasında, katman değişikliğinin ne zaman tamamlandığını onaylamak üzere **Arşiv Durumu** özniteliğini yoklamak için blob özelliklerini alma işlemini kullanabilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.  

**Blobun katmanını ayarladıktan sonra buna uygun fiyattan fatura almaya ne zaman başlarım?**

Her blob her zaman blobun tarafından belirtilen katmana göre faturalandırılır **erişim katmanı** özelliği. Bir blob için yeni bir katman ayarlarken **erişim katmanı** özelliği, tüm geçişler için yeni katmanı anında yansıtır. Ancak, bir blobu arşiv katmanından sık erişimli veya seyrek erişimli katmana yeniden doldururken işlemin birkaç saat sürebilir. Bu durumda, bu noktada, yeniden doldurma tamamlanana kadar arşiv fiyatlarından faturalandırılır **erişim katmanı** özelliği yeni katmanı yansıtır. Bu noktada sık erişimli veya seyrek erişimli fiyatı, bir blob için faturalandırılırsınız.

**Seyrek erişimli veya arşiv katmanındaki bir blobu silerken veya dışarı taşırken erken silme ücreti ödeyip ödemeyeceğimi nasıl anlarım?**

Sırasıyla 30 ve 180 günden önce seyrek erişimli (yalnızca GPv2 hesapları) ya da arşiv katmanından silinen veya dışarı taşınan tüm bloblar eşit dağıtılmış bir erken silme ücreti ödenmesini gerektirir. Nasıl bir blob seyrek erişimli veya arşiv katmanında denetleyerek süredir belirleyebilirsiniz **erişim katmanı değişim zamanı** blob son katman değişikliğinin bir damgasını sağlayan özelliği. Daha fazla ayrıntı için bkz. [Seyrek erişim ve arşiv erken silme](#cool-and-archive-early-deletion) bölümü.

**Hangi Azure araçları ve SDK’lar blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekliyor?**

Azure portalı, PowerShell ve CLI araçları ile .NET, Java, Python ve Node.js istemci kitaplıklarının tümü blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekler.  

**Sık, seyrek ve arşiv katmanlarına ne kadar veri depolayabilirim?**

Diğer sınırlarla birlikte veri depolama da depolama katmanına göre değil hesap düzeyinde ayarlanır. Bu nedenle, tamamını tek bir katmanda veya üç katmanın hepsinde kullanmayı seçebilirsiniz. Daha fazla bilgi için [Azure Storage ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>GPv2 Blob depolama hesaplarında sık erişimli, seyrek erişimli ve arşiv seçeneklerini değerlendirme

[Bölgeye göre sık, seyrek ve arşiv kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Blob Depolama yaşam döngüsünü yönetme](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md)

[Blob depolama ve GPv2 hesaplarında bölgeye göre sık erişimli, seyrek erişimli ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
