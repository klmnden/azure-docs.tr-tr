---
title: "Blob’lar için yoğun erişimli, seyrek erişimli ve arşiv Azure depolaması | Microsoft Docs"
description: "Azure Blob depolama hesapları için yoğun erişimli, seyrek erişimli ve arşiv depolaması"
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/05/2017
ms.author: mihauss
ms.openlocfilehash: 501fc59efb8bacf58fea2825752d3a33c6ea5963
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="azure-blob-storage-hot-cool-and-archive-preview-storage-tiers"></a>Azure Blob Depolama: Yoğun erişimli, seyrek erişimli ve arşiv (önizleme) depolama katmanları

## <a name="overview"></a>Genel Bakış

Verilerinizi, nasıl kullandığınıza bağlı olarak en uygun maliyetli şekilde depolayabilmeniz için Azure Depolama, Blob nesne depolama için üç depolama katmanı sunuyor. Azure **sık erişimli depolama katmanı** sık erişimli verileri depolamak için optimize edilmiştir. Azure **seyrek erişimli depolama katmanı** daha az sıklıkta erişilen ve en az 30 gün saklanan verileri depolamak için optimize edilmiştir. **Azure arşiv depolama katmanı** (önizleme) seyrek erişilen ve en az 180 gün boyunca saklanan, esnek gecikme süresi gereksinimlerine sahip (saat bazında) verileri depolamak için optimize edilmiştir. Arşiv depolama katmanı, yalnızca blob düzeyinde kullanılabilir; depolama hesabı düzeyinde kullanılamaz. Seyrek erişimli depolama katmanındaki veriler, biraz daha düşük bir kullanılabilirliği kabul edebilir, ancak yine de sık erişimli veriler kadar erişim süresi ve verimlilik gerektirir. Seyrek erişimli veriler için, sık erişimli verilere kıyasla biraz daha düşük kullanılabilirlik SLA'sı ve yüksek erişim maliyetleri, daha düşük depolama maliyetleri için kabul edilebilir tercihlerdir. Çevrimdışı olan arşiv depolama, en düşük depolama maliyetini sunar ancak aynı zamanda en yüksek erişim maliyetine sahiptir.

Bugün, bulutta depolanan veriler büyük bir hızla artmaktadır. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, maliyetleri optimize etmek amacıyla erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, nasıl oluşturulduğu, işlendiği ve yaşam süresi boyunca nasıl erişildiği açısından farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında sık erişilebilir. Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir.

Bu veri senaryolarının her biri, belirli erişim düzeni için optimize edilmiş olan farklı bir depolama katmanından faydalanır. Sık erişimli, seyrek erişimli ve arşiv depolama katmanlarının kullanılmaya başlanmasıyla, Azure Blob Depolama farklı fiyatlandırma modelleriyle bu ayrılmış depolama katmanları ihtiyacına hitap ediyor.

## <a name="blob-storage-accounts"></a>Blob Storage hesapları

**Blob Storage hesapları**, yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage’da depolamanıza yönelik özel depolama hesaplarıdır. Blob depolama hesaplarıyla artık hesap düzeyinde seyrek erişimli veya sık erişimli depolama katmanları arasından, blob düzeyinde ise seyrek erişimli, sık erişimli ve arşiv katmanları arasından seçim yapabilirsiniz. Nadiren, seyrek olarak veya sıkça erişilen verileri; sık erişimli, seyrek erişimli ve arşiv depolama katmanları arasından uygun olanda depolayarak maliyetleri optimize edin. Blob depolama hesapları, mevcut genel amaçlı depolama hesaplarınıza benzer ve blok blobları ve ilave blobları için yüzde yüz API tutarlığı dahil günümüzde kullandığınız tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır.

> [!NOTE]
> Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez.

Blob depolama hesapları, **Erişim Katmanı** özniteliğini hesap düzeyinde açığa çıkarır ve böylece varsayılan depolama hesabı katmanı **Sık erişimli** veya **Seyrek erişimli** olarak tanımlanır. Varsayılan depolama hesabı katmanı, blob düzeyinde ayarlanmış açık bir katmanı olmayan tüm bloblara uygulanır. Verilerinizin kullanım düzeninde bir değişiklik olursa herhangi bir zamanda bu depolama katmanları arasında geçiş yapabilirsiniz. **Arşiv katmanı** (önizleme) yalnızca blob düzeyinde kullanılabilir.

> [!NOTE]
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve faturalama](#pricing-and-billing) bölümüne bakın.

### <a name="hot-access-tier"></a>Sık erişim katmanı

Sık erişimli depolama, seyrek erişimli ve arşiv depolamaya göre daha yüksek depolama maliyetine sahiptir, ancak en düşük erişim maliyetini sunar. Sık erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Etkin kullanımda olan veya sık erişilmesi beklenen (okunma ve üzerine yazılma) veriler.
* Seyrek erişimli depolama katmanına işlemeye ya da sonuçta geçişe hazırlanan veriler.

### <a name="cool-access-tier"></a>Seyrek erişim katmanı

Seyrek erişimli depolama katmanı, sık erişimli depolamaya kıyasla daha düşük depolama maliyetine ve daha yüksek erişim maliyetine sahiptir. Bu katman, seyrek erişim katmanında en az 30 gün boyunca kalacak verilere yöneliktir. Seyrek erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Kısa süreli yedekleme ve olağanüstü durum kurtarma veri kümeleri.
* Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
* Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*Örneğin*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)

### <a name="archive-access-tier-preview"></a>Arşiv erişim katmanı (önizleme)

Arşiv depolama, sık erişimli ve seyrek erişimli depolamayla karşılaştırıldığında en düşük depolama maliyetine ve daha yüksek veri alma maliyetine sahiptir. Bu katman, birkaç saatlik alma gecikmesinden etkilenmeyecek ve arşiv katmanında en az 180 gün kalacak verilere yöneliktir.

Bir blob arşiv depolamasında bulunduğu sürece çevrimdışıdır (meta veriler hariç, bunlar çevrimiçidir ve kullanılabilir), okunamaz, kopyalanamaz, üzerine yazılamaz veya değiştirilemez. Arşiv depolamasındaki blobların anlık görüntüsünü de alamazsınız. Ancak, silme, listeleme, blob özelliklerini/meta verilerini alma işlemleri için mevcut işlemleri kullanabilir ve blob katmanını değiştirebilirsiniz.

#### <a name="blob-rehydration"></a>Blob yeniden doldurma
Arşiv depolama birimindeki verileri okumak için katmanı önce sık erişimli veya seyrek erişimli blob katmanı olarak değiştirmeniz gerekir. Bu işleme yeniden doldurma denir ve 50 GB’tan küçük bloblar için 15 saat kadar sürebilir. Daha büyük bloblar için gereken ek süre blob aktarım hızı sınırına göre değişir.

Yeniden doldurma sırasında katmanın değişip değişmediğini onaylamak için "archive status" blob özelliğini kontrol edebilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra "archive status" blob özelliği kaldırılır ve"access tier" blob özelliği seyrek veya sık erişimli katmanı gösterir.  

Arşiv depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Uzun süreli yedekleme, arşivleme ve olağanüstü durum kurtarma veri kümeleri.
* Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*Örneğin*, kodlama diğer biçimlere dönüştürüldükten sonra ham medya dosyaları)
* Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*Örneğin* güvenlik kamerası kayıtları, sağlık kuruluşları için eski röntgen/MRI çekimleri, finans hizmetleri için sesli kayıtlar ve müşteri görüşmesi dökümleri)

### <a name="recommendations"></a>Öneriler

Depolama hesapları hakkıında daha fazla bilgi için bkz. [Azure Storage hesapları hakkında](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Yalnızca blok veya ilave blobu depolaması gerektiren uygulamalar için, katmanlı depolamanın farklı fiyat avantajlarından faydalanmak üzere Blob Storage hesapları kullanılmasını öneriyoruz. Ancak, bunun aşağıdaki gibi, genel amaçlı depolama hesaplarının kullanılması gerektiği belirli koşullar altında mümkün olmayabileceğini de anlıyoruz:

* Tablo, kuyruk veya dosyaları kullanmanız gerekmesi ve bloblarınızın aynı depolama hesabında saklanmasını istemeniz. Bunları aynı hesapta depolamanın, aynı paylaşılan anahtara sahip olma dışında teknik bir avantajı yoktur.

* Hala Klasik dağıtım modeli kullanmanız gerekmesi. Blob Storage hesapları yalnızca Azure Resource Manager dağıtım modeli aracılığıyla kullanılabilir.

* Sayfa blobları kullanmanız gerekmesi. Blob Storage hesapları sayfa bloblarını desteklemez. Sayfa bloblarını kullanmaya özel olarak ihtiyacınız yoksa, genelde blok bloblarını kullanılmasını öneriyoruz.

* 2014-02-14 tarihinden önceki [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) sürümünü veya 4.x’ten düşük bir istemci kitaplığı sürümü ile kullanmanız ve uygulamanızı güncelleştirememeniz.

> [!NOTE]
> Blob depolama hesapları şu anda çoğu Azure bölgesinde desteklenmektedir.


## <a name="blob-level-tiering-feature-preview"></a>Blob düzeyinde katmanlama özelliği (önizleme)

Blob düzeyinde katman ayarlama, [Blob Katmanını Ayarlama](/rest/api/storageservices/set-blob-tier) adlı tek bir işlem kullanarak nesne düzeyinde verilerinizin katmanını değiştirmenize olanak verir. Bir blob’un erişim katmanını, kullanım şekli değiştikçe verileri hesapları arasında taşımaya gerek kalmadan sık erişimli, seyrek erişimli veya arşiv katmanları arasında kolayca değiştirebilirsiniz. Tüm katman değişiklikleri, bir blob arşivden yeniden doldurulma sırasında olmadığı sürece hemen gerçekleşir. Son blob katmanı değişikliğinin zamanı, blob özelliklerindeki **Erişim Katmanı Değişim Zamanı** özniteliği aracılığıyla gösterilir. Blob arşiv katmanındaysa üzerine yazılamayabilir ve bu nedenle aynı blob’un karşıya yüklenmesine bu senaryoda izin verilmez. Sık veya seyrek erişimli blob’ların üzerine yazabilirsiniz ve bu durumda yeni blob, üzerine yazılan eski blobun katmanını devralır.

Aynı hesapta üç farklı depolama katmanına sahip bloblar birlikte bulunabilir. Açıkça atanan bir katmanı olmayan tüm bloblar katmanı hesap erişim katmanının ayarını çıkarır. Erişim katmanı hesaptan alınıyorsa, **Erişim Katmanı Alındı** özniteliğinin “true” olarak ayarlandığını ve blob **Erişim Katmanı** özniteliğinin hesap katmanıyla uyuştuğunu görürsünüz. Azure portalında, erişim katmanı alındı özelliği, blob erişim katmanı ile birlikte gösterilir (örneğin, Sık Erişilen(alındı) veya Seyrek Erişilen (alındı)).

> [!NOTE]
> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Anlık görüntüleri olan bir blok blobun katmanını da değiştiremezsiniz.

### <a name="blob-level-tiering-billing"></a>Blob düzeyinde katman ayarlama faturalandırması

Bir blob daha seyrek erişilen bir katmana taşındığında (Sık Erişilen->Seyrek Erişilen, Sık Erişilen->Arşiv veya Seyrek Erişilen->Arşiv), bu işlem hedef katmana yazma olarak faturalandırılır ve hedef katmanın yazma işlemi (10.000 başına) ile veri yazma (GB başına) ücretleri uygulanır. Bir blob daha sık erişilen bir katmana taşınırsa (Arşiv->Seyrek Erişilen, Arşiv->Sık Erişilen veya Seyrek Erişilen->Sık Erişilen), bu işlem kaynak katmandan okuma olarak faturalandırılır ve kaynak katmanın okuma işlemi (10.000 başına) ile veri alma (GB başına) ücretleri uygulanır.

Önizleme sürümündeki bu özellikleri kullanmak için [Azure Arşiv ve Blob Düzeyi Katmanlama blog duyurusundaki](https://azure.microsoft.com/blog/announcing-the-public-preview-of-azure-archive-blob-storage-and-blob-level-tiering) yönergeleri izleyin.

Blob düzeyi katmanlama için önizleme sırasında geçerli olan bazı kısıtlamalar şunlardır:

* ABD Doğu 2, ABD Doğu veya ABD Batı'da başarılı önizleme kayıt destek arşiv depolama sonrası oluşturulan yalnızca yeni blob depolama hesapları.

* Ortak bölgelerde başarılı önizleme kayıt destek blob düzeyi katmanlama sonrası oluşturulan yalnızca yeni blob depolama hesapları.

* Blob düzeyinde katmanlama ve arşiv depolaması yalnızca [LRS] (../ common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#locally-redundant-storage) depolaması içinde desteklenir. [GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#geo-redundant-storage) ve [RA-GRS](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#read-access-geo-redundant-storage) gelecekte desteklenecektir.

* Anlık görüntüleri olan blobların katmanını değiştiremezsiniz.

* Arşiv depolamasındaki bir blobu kopyalayamaz ve anlık görüntüsünü alamazsınız.

## <a name="comparison-of-the-storage-tiers"></a>Depolama katmanlarının karşılaştırması

Aşağıdaki tabloda, sık erişimli ve seyrek erişimli depolama katmanlarının karşılaştırması gösterilmiştir. Arşiv blob düzeyi katmanı önizleme sürümünde olduğundan SLA’sı yoktur.

| | **Sık erişimli depolama katmanı** | **Seyrek erişimli depolama katmanı** | **Arşiv depolama katmanı**
| ---- | ----- | ----- | ----- |
| **Kullanılabilirlik** | %99,9 | %99 | Yok |
| **Kullanılabilirlik** <br> **(RA-GRS okumaları)**| %99,99 | %99,9 | Yok |
| **Kullanım ücretleri** | Yüksek depolama maliyeti, düşük erişim ve işlem maliyetleri | Düşük depolama maliyeti, yüksek erişim ve işlem maliyetleri | En düşük depolama maliyeti, en yüksek erişim ve işlem maliyetleri |
| **En düşük nesne boyutu** | Yok | Yok | Yok |
| **En az depolama süresi** | Yok | Yok | 180 gün
| **Gecikme süresi** <br> **(İlk bayta kadar süre)** | milisaniye | milisaniye | < 15 sa
| **Ölçeklenebilirlik ve Performans Hedefleri** | Genel amaçlı depolama hesaplarıyla aynı | Genel amaçlı depolama hesaplarıyla aynı | Genel amaçlı depolama hesaplarıyla aynı |

> [!NOTE]
> Blob Storage hesapları, genel amaçlı depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).


## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Blob depolama hesapları, her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Bir Blob Storage hesabı kullanırken, aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti depolama katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.

* **Veri erişimi maliyetleri**: Katmanın erişimi sıklığı düştükçe veri erişimi ücretleri artar. Seyrek erişimli depolama ve arşiv depolama katmanındaki verilerde, okuma işlemleri için erişilen gigabayt veri başına ücretlendirilirsiniz.

* **İşlem maliyetleri**: Tüm katmanlarda, erişim sıklığı düştükçe artan bir işlem başına ücret uygulanır.

* **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu, yalnızca GRS ve RA-GRS dahil, coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.

* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.

* **Depolama katmanını değiştirme**: Hesap depolama katmanını seyrek erişimliden sık erişimliye değiştirmek, depolama hesabında mevcut tüm verilerin okunmasına eşit bir ücret doğurur. Ancak hesap depolama katmanının sık erişilenden seyrek erişilene geçirildiğinde, tüm verileri seyrek erişilen katmana yazma maliyetine eşit bir ücret yansıtılır.

> [!NOTE]
> Blob depolama hesaplarına ilişkin fiyatlandırma modeli hakkında daha fazla bilgi için bkz. [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için bkz. [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfası.

## <a name="quick-start"></a>Hızlı başlangıç

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

* Blob Storage hesabı oluşturma.
* Blob Storage hesabı yönetme.

Bu ayar tüm depolama hesabına uygulandığından aşağıdaki örneklerde erişim katmanı arşiv olarak ayarlayamazsınız. Arşiv katmanını yalnızca belirli bir blob için ayarlayabilirsiniz.

### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Azure portalı kullanarak Blob depolama hesabı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Hub menüsünde, **Yeni** > **Veri + Depolama** > **Depolama hesabı**’nı seçin.

3. Depolama hesabınız için bir ad girin.

    Bu ad genel olarak benzersiz olmalıdır; depolama hesabındaki nesnelere erişmek için kullanılan URL’nin bir parçası olarak kullanılır.  

4. Dağıtım modeli olarak **Kaynak Yöneticisi**’ni seçin.

    Katmanlı depolama yalnızca Resource Manager depolama hesaplarıyla birlikte kullanılabilir; yeni kaynaklar için önerilen dağıtım modeli budur. Daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).  

5. Hesap Türü açılır listesinden **Blob Depolama** seçeneğini belirleyin.

    Depolama hesabının türünü buradan seçebilirsiniz. Katmanlı depolama genel amaçlı depolamada kullanılamaz; yalnızca Blob depolama türündeki hesapta kullanılabilir.     

    Bunu seçtiğinizde performans katmanı Standart olarak ayarlanır. Katmanlı depolama, Premium performans katmanı ile kullanılamaz.

6. Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**, **GRS** veya **RA-GRS**. Varsayılan seçenek **RA-GRS**’dir.

    LRS = yerel olarak yedekli depolama; GRS = coğrafi olarak yedekli depolama (iki bölge); RA-GRS okuma erişimli, coğrafi olarak yedekli depolama (ikincisine okuma erişiminin bulunduğu 2 bölge).

    Azure Depolama çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Depolama çoğaltma](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin. Varsayılan seçenek **Sık Erişimli**’dir.

8. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.

9. Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../azure-resource-manager/resource-group-overview.md).

10. Depolama hesabınız için bölge seçin.

11. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Azure portalı kullanarak Blob depolama hesabındaki depolama katmanını değiştirme

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.

4. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>Azure portalı kullanarak blobun depolama katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınızdaki blobunuza gitmek için Tüm Kaynaklar’ı seçin, depolama hesabınızı seçin, kapsayıcınızı seçin ve ardından blobunuzu seçin.

3. Blob özellikleri dikey penceresinde **Erişim Katmanı** açılan menüsüne tıklayarak **Sık Erişilen**, **Seyrek Erişilen** veya **Arşiv** depolama katmanını seçin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

> [!NOTE]
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve Faturalama](#pricing-and-billing) bölümüne bakın.


## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Blob depolama hesaplarını değerlendirme ve geçiş yapma
Bu bölümün amacı kullanıcıların Blob depolama hesapları kullanmaya sorunsuz bir geçiş yapmasına yardımcı olmaktır. İki kullanıcı senaryosu vardır:

* Genel amaçlı bir depolama hesabınız var ve doğru depolama katmanı ile bir Blob depolama hesabına geçişi değerlendirmek istiyorsunuz.
* Bir Blob depolama hesabı kullanmaya karar verdiğiniz veya zaten bir tane var ve sık erişimli ya da seyrek erişimli depolama katmanı kullanıp kullanmayacağınızı değerlendirmek istiyorsunuz.

Her iki durumda da ilk iş sırası bir Blob depolama hesabında depolanan verileriniz depolama ve erişim maliyetinin tahmin edilmesi ve bu maliyetin mevcut maliyetlerinizle karşılaştırılmasıdır.

## <a name="evaluating-blob-storage-account-tiers"></a>Blob depolama hesabı katmanlarını değerlendirme

Bir Blob depolama hesabına depolanan verilerinizin depolama ve erişim maliyetini tahmin etmek için var olan kullanım modelinizi ve beklediğiniz kullanım modelini yaklaşık olarak değerlendirmeniz gerekir. Genel olarak, şunları bilmek istersiniz:

* Depolama tüketiminiz – Ne kadar veri depolanıyor ve bu aylık olarak nasıl değişiyor?

* Depolama erişim modelleriniz - Hesaptan ne kadar veri okunuyor ve hesaba ne kadar veri yazılıyor (yeni veriler dahil)? Veri erişimi için kaç tane işlem kullanılıyor ve bunlar ne tür işlemler?

## <a name="monitoring-existing-storage-accounts"></a>Var olan depolama hesaplarını izleme

Var olan depolama hesaplarınızı izlemek ve bu verileri toplamak için, bir depolama hesabına yönelik günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler sağlayan Azure Depolama Analizi hizmetinden yararlanabilirsiniz. Storage Analytics hem genel amaçlı depolama hesapları hem de Blob depolama hesapları için toplu işlem istatistiklerini içerebilen ölçümleri ve Blob depolama hizmetine yapılan isteklere ilişkin kapasite verilerini depolayabilir. Bu veriler aynı depolama hesabındaki iyi bilinen tablolara depolanır.

Daha fazla bilgi için bkz. [Storage Analytics Ölçümleri hakkında](https://msdn.microsoft.com/library/azure/hh343258.aspx) ve [Storage Analytics Ölçüm Tablosu Şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob depolama hesapları, tablo hizmeti uç noktasını yalnızca ilgili hesabın ölçüm verilerini depolamak ve bunlara erişmek için ortaya çıkarır.

Blob depolama hizmetinin depolama tüketimini izlemek için kapasite ölçümlerini etkinleştirmeniz gerekir.
Bu özellik etkinleştirildiğinde bir depolama hesabının Blob hizmeti için kapasite verileri günlük olarak kaydedilir ve aynı depolama hesabı içindeki *$MetricsCapacityBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir.

Blob depolama hizmetinin veri erişim modelini izlemek için saatlik işlem ölçümlerini bir API düzeyinde etkinleştirmeniz gerekir. Bu özellik etkinleştirildiğinde API başına işlemler saatte bir toplanır ve aynı depolama hesabındaki *$MetricsHourPrimaryTransactionsBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir. RA-GRS depolama hesapları kullanılırken *$MetricsHourSecondaryTransactionsBlob* tablosu, işlemleri ikincil uç noktaya kaydeder.

> [!NOTE]
> Blok genelindeki sayfa blob’larını ve sanal makine disklerini engelleme ve ekleme blob verileriyle birlikte depoladığınız genel amaçlı bir depolama hesabınız varsa bu tahmin işlemi geçerli değildir. Bunun nedeni, yalnızca bir Blob depolama hesabına geçirilebilecek engelleme ve ekleme blob’ları için blob türüne göre kapasite ve işlem ölçümlerini ayırt etmenin bir yolunun olmamasıdır.

Veri tüketim ve erişim modelinizi yaklaşık olarak tahmin etmek için, ölçümler için düzenli kullanımınızı temsil eden bir elde tutma süresi seçmeniz ve tahmin etmeniz önerilir. Seçeneklerden biri son yedi güne ait ölçüm verilerinin tutulması ve verilerin ay sonunda analiz için haftada bir toplanmasıdır. Diğer bir seçenek ise son 30 güne ait ölçüm verilerinin tutulması ve verilerin 30 günlük süre sonunda toplanıp çözümlenmesidir.

Ölçüm verilerini etkinleştirme, toplama ve görüntüleme hakkında bilgi için bkz. [Azure Depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Analiz verilerinin depolanması, erişimi ve indirilmesi de normal kullanıcı verileri gibi ücretlendirilir.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Kullanım ölçümlerinden yararlanarak maliyetleri tahmin etme

### <a name="storage-costs"></a>Depolama maliyetleri

*$MetricsCapacityBlob* kapasite ölçüm tablosunda *'data'* satır anahtarını içeren en son giriş, kullanıcı verilerinin harcadığı kapasiteyi gösterir. *$MetricsCapacityBlob* kapasite ölçüm tablosunda *'analytics'* satır anahtarını içeren en son giriş, analiz günlüklerinin harcadığı kapasiteyi gösterir.

Hem kullanıcı verileri hem de analiz günlükleri (etkinse) tarafından kullanılan bu toplam kapasite, verileri depolama hesabına depolama maliyetini tahmin etmek için kullanılabilir. Aynı yöntem ayrıca engelleme ve ekleme blob’larının genel amaçları depolama hesaplarına depolanma maliyetlerini tahmin etmek için kullanılabilir.

### <a name="transaction-costs"></a>İşlem maliyetleri

İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalBillableRequests'* toplamı, ilgili API’nin toplam işlem sayısını belirtir. *Örneğin*, belirli bir süre içindeki *'GetBlob'* işlemlerinin toplam sayısı *'user;GetBlob'* satır anahtarını içeren tüm girişlere yönelik toplam faturalandırılabilir isteklerin toplamına göre hesaplanabilir.

Blob depolama hesaplarına ilişkin işlem maliyetlerini tahmin etmek için, farklı şekilde fiyatlandırıldıkları için işlemleri üç gruba ayırmanız gerekir.

* *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* ve *'CopyBlob'* gibi yazma işlemleri.
* *'DeleteBlob'* ve *'DeleteContainer'* gibi silme işlemleri.
* Diğer tüm işlemler.

Genel amaçlı depolama hesaplarının işlem maliyetlerini tahmin etmek için işlemden/API’den bağımsız olarak tüm işlemleri toplamanız gerekir.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Veri erişimi ve coğrafi çoğaltma veri aktarımı maliyetleri

Storage Analytics bir depolama hesabından okunan ve depolama hesabına yazılan veri miktarını belirtmese de, işlem ölçümleri tablosuna bakılarak bu değer kabaca tahmin edilebilir. İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalIngress'* toplamı, ilgili API’nin toplam giriş verileri miktarını bayt cinsinden belirtir. Benzer şekilde, *'TotalEgress'* toplamı toplam çıkış verileri miktarını bayt cinsinden belirtir.

Blob depolama hesaplarına ilişkin veri erişimi maliyetlerini hesaplamak için işlemleri iki gruba ayırmanız gerekir.

* Depolama hesabından alınan veri miktarı birincil olarak *'GetBlob'* ve *'CopyBlob'* işlemleri için *'TotalEgress'* toplamına bakılarak tahmin edilebilir.

* Depolama hesabına yazılan veri miktarı birincil olarak *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* ve *'AppendBlock'* işlemleri için *'TotalIngress'* toplamına bakılarak tahmin edilebilir.

Blob depolama hesaplarında coğrafi çoğaltma veri aktarımı maliyeti de bir GRS veya RA-GRS depolama hesabı kullanılırken yazılan veri miktarı tahmini kullanılarak hesaplanabilir.

> [!NOTE]
> Seyrek veya sık erişimli bir depolama katmanını kullanma maliyetlerini hesaplama hakkında daha ayrıntılı bir örnek için *'Sık ve Seyrek Erişimli erişim katmanları nelerdir ve hangisinin kullanılacağını nasıl belirlemeliyim?'* başlıklı SSS bölümüne bakın bkz. [Azure Depolama Fiyatlandırma Sayfası](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Mevcut verileri geçirme

Bir Blob Storage hesabı yalnızca blok ve ilave bloblarının depolanmasına yöneliktir. Blobların yanı sıra tablo, kuyruk, dosya ve diskleri de depolamanızı sağlayan mevcut genel amaçlı depolama hesapları Blob depolama hesaplarına dönüştürülemez. Depolama katmanlarını kullanmak için, yeni Blob depolama hesapları oluşturmanız ve mevcut verilerinizi yeni oluşturulan hesaplara taşımanız gerekir.

Mevcut verileri şirket içi depolama aygıtlarından, üçüncü taraf bulut depolama sağlayıcılardan ya da Azure’daki mevcut genel amaçlı depolama hesaplarınızdan Blob depolama hesaplarına geçirmek için aşağıdaki yöntemleri kullanabilirsiniz:

### <a name="azcopy"></a>AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy yardımcı programını, verileri genel amaçlı depolama hesaplarınızdan Blob Storage hesabınıza kopyalamak ya da şirket içi depolama aygıtlarınızdaki verileri Blob Storage hesabınıza yüklemek için kullanabilirsiniz.

Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Veri hareketi kitaplığı

.NET için Azure Storage veri hareketi kitaplığı AzCopy’yi çalıştıran çekirdek veri hareketi altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. Bu, AzCopy’nin dış örneklerini çalıştırmanıza ve izlemenize gerek kalmadan, AzCopy tarafından uygulamanızda yerel olarak sağlanan özelliklerden tam olarak faydalanmanızı sağlar.

Daha fazla ayrıntı için bkz. [.Net için Azure Storage Veri Hareketi Kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı

Azure istemci kitaplıklarından birini ya da Azure Storage hizmetleri REST API’sini kullanarak verilerinizi Blob Storage hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Daha fazla bilgi için,bkz. [Azure Blob Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Bloblar, blobla depolanan istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. Tüm kopyalama mekanizmalarının blob verilerinin ve özellikle şifrelemeyle ilgili meta verilerin korunduğundan emin olması kesinlikle önemlidir. Blobları bu meta veriler olmadan kopyalarsanız, blob içeriği tekrar alınamaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Depolama İstemci Tarafı Şifrelemesi](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="faq"></a>SSS

1. **Mevcut depolama hesapları hâlâ kullanılabilir mi?**

    Evet, var olan depolama hesapları hala kullanılabilir ve fiyatlandırma veya işlev açısından bir farklılık göstermez.  Bunlar depolama katmanı seçme olanağına sahip değildir ve gelecekte katmanlama özelliğine sahip olmayacaktır.

2. **Neden ve ne zaman Blob depolama hesapları kullanmaya başlamalıyım?**

    Blob Storage hesapları, blobları depolamak ve yeni blob merkezli özellikleri sunmak içindir. Dahası, bu hesap türüne göre hiyerarşik depolama ve katmanlama gibi özellikler gelecekte sunulacağından, Blob Storage hesapları blobları depolamak için önerilen yoldur. Ancak, ne zaman geçiş yapacağınız iş gereksinimleriniz temelinde size bağlıdır.

3. **Mevcut depolama hesabımı Blob depolama hesabına dönüştürebilir miyim?**

    Hayır. Blob depolama hesabı farklı türde bir depolama hesabıdır ve yeni bir tane oluşturmanız ve daha önce açıklandığı şekilde verilerinizi taşımanız gereklidir.

4. **Nesneleri aynı hesaptaki iki depolama katmanında depolayabilir miyim?**

    Evet. Hesap düzeyinde ayarlanan *“Erişim Katmanı”* özniteliği, bu hesapta bulunan ve açkıkça bir katman belirlenmemiş olan tüm nesneler için varsayılan katman olur. Ancak blob düzeyinde katman ayarlama (önizleme), hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanı ayarlamanıza olanak tanır. Aynı hesapta, üç depolama katmanının (sık erişilen, seyrek erişilen veya arşiv) tümüne ait bloblar bulunabilir.

5. **Blob depolama hesabımdaki depolama katmanını değiştirebilir miyim?**

    Evet, depolama hesabındaki *“Erişim Katmanı”* özniteliğini ayarlayarak depolama katmanını değiştirebilirsiniz. Depolama katmanının değiştirilmesi, hesabta depolanmış ve açıkça belirlenmiş bir katmanı olmayan tüm nesneler için geçerli olur. Sık erişilen olan depolama katmanının seyrek erişilen olarak değiştirilmesi, hem yazma işlemi (10.000 başına) hem de veri yazma (GB başına) maliyetleri doğurur (yalnızca blob depolama hesaplarında). Seyrek erişilen olan depolama katmanının sık erişilen olarak değiştirilmesi ise hesaptaki tüm verinin okunması için hem okuma işlemi (10.000 başına) hem de veri alma (GB başına) maliyetleri doğurur.

6. **Blob depolama hesabımdaki depolama katmanını hangi sıklıkta değiştirebilirim?**

    Depolama katmanını değiştirme sıklığına ilişkin bir sınırlama koymuyoruz, ancak depolama katmanını seyrek erişimliden sık erişimliye değiştirmenin büyük maliyetler doğurduğuna dikkat edin. Depolama katmanını sık değiştirmeniz önerilmez.

7. **Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanındakilerden farklı mı davranır?**

    Sık erişimli depolama katmanındaki bloblar genel amaçlı depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek erişimli depolama katmanındaki bloblar genel amaçlı depolama hesaplarındaki bloblarla benzer gecikme süresine (milisaniye olarak) sahiptir. Arşiv depolama katmanındaki bloblar, birkaç saatlik gecikme süresine sahiptir.

    Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanında depolanan bloblara göre daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [Depolama için SLA](https://azure.microsoft.com/support/legal/sla/storage).

8. **Sayfa bloblarını ve sanal makine disklerini Blob depolama hesaplarında depolayabilir miyim?**

    Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez. Azure Virtual Machine diskleri sayfa blobları tarafından yedeklenir ve bu nedenle sanal makine disklerini depolamak için Blob Storage hesapları kullanılamaz. Ancak, sanal makine disklerinin yedeklerini blok blobları olarak Blob Storage hesabında depolamak mümkündür.

9. **Blob depolama hesaplarını kullanmak için mevcut uygulamalarımı değiştirmem gerekir mi?**

    Blob Storage hesapları, blok ve ilave blobları için genel amaçlı depolama hesaplarıyla % 100 API tutarlıdır. Uygulamanız blok veya ilave bloblarını kullandığı ve [Depolama Hizmetleri REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx)’nin 2014-02-14 sürümünü veya üstünü kullandığınız sürece, uygulamanız çalışmaya devam edecektir. Protokolün daha eski bir sürümünü kullanıyorsanız, her iki tür depolama hesabıyla sorunsuz çalışarak yeni sürümü kullanmak için uygulamanızı güncelleştirmeniz gerekir. Genel olarak, hangi depolama hesabını kullandığınızdan bağımsız olarak her zaman en son sürümü kullanmanızı öneriyoruz.

10. **Kullanıcı deneyiminde bir değişiklik olur mu?**

    Blob Storage hesapları blok ve ilave bloblarını depolamak için genel amaçlı depolama hesaplarına çok benzer ve yüksek dayanıklılık ve kullanılabilirlik, ölçeklenebilirlik, performans ve güvenlik dahil olmak üzere Azure Storage’ın tüm anahtar özelliklerini destekler. Blob depolama hesaplarına özgü özellikler ve kısıtlamalar ve yukarıda bahsedilen depolama katmanları dışındaki her şey aynı kalır.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-blob-storage-accounts"></a>Blob Storage hesaplarını değerlendirme

[Bölgeye göre Blob depolama hesaplarının kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Bölgeye göre Blob depolama fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Blob Storage hesaplarını kullanmaya başlama

[Azure Blob depolamayı kullanmaya başlama](storage-dotnet-how-to-use-blobs.md)

[Azure Depolama’ya ve Azure Depolama’da veri taşıma](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Depolama hesaplarınıza göz atma ve bu hesapları keşfetme](http://storageexplorer.com/)
