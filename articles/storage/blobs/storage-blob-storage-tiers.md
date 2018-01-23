---
title: "Blob’lar için yoğun erişimli, seyrek erişimli ve arşiv Azure depolaması | Microsoft Docs"
description: "Azure depolama hesapları için sık erişimli, seyrek erişimli ve arşiv depolama."
services: storage
documentationcenter: 
author: kuhussai
manager: jwillis
editor: 
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: kuhussai
ms.openlocfilehash: be84f68a044a73673e991f04c7fe36a7787b9c3c
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="azure-blob-storage-hot-cool-and-archive-storage-tiers"></a>Azure Blob Depolama: Sık erişimli, seyrek erişimli ve arşiv depolama katmanları

## <a name="overview"></a>Genel Bakış

Verilerinizi, nasıl kullandığınıza bağlı olarak en uygun maliyetli şekilde depolayabilmeniz için Azure depolama, Blob nesnesi depolama için üç depolama katmanı sunuyor. Azure **sık erişimli depolama katmanı** sık erişimli verileri depolamak için optimize edilmiştir. Azure **seyrek erişimli depolama katmanı** daha az sıklıkta erişilen ve en az 30 gün saklanan verileri depolamak için optimize edilmiştir. Azure **arşiv depolama katmanı**, seyrek erişilen ve en az 180 gün boyunca saklanan, esnek gecikme süresi gereksinimleri olan (saat bazında) verileri depolamak için iyileştirilmiştir. Arşiv depolama katmanı, yalnızca blob düzeyinde kullanılabilir; depolama hesabı düzeyinde kullanılamaz. Seyrek erişimli depolama katmanındaki veriler, biraz daha düşük bir kullanılabilirliği kabul edebilir, ancak yine de sık erişimli veriler kadar erişim süresi ve verimlilik gerektirir. Seyrek erişimli veriler için, sık erişimli verilere kıyasla biraz daha düşük kullanılabilirlik SLA'sı ve yüksek erişim maliyetleri, daha düşük depolama maliyetleri için kabul edilebilir tercihlerdir. Çevrimdışı olan arşiv depolama, en düşük depolama maliyetini sunar ancak aynı zamanda en yüksek erişim maliyetine sahiptir.

Bugün, bulutta depolanan veriler büyük bir hızla artmaktadır. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, maliyetleri optimize etmek amacıyla erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, nasıl oluşturulduğu, işlendiği ve yaşam süresi boyunca nasıl erişildiği açısından farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında sık erişilebilir. Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir.

Bu veri senaryolarının her biri, belirli erişim düzeni için optimize edilmiş olan farklı bir depolama katmanından faydalanır. Sık erişimli, seyrek erişimli ve arşiv depolama katmanlarının kullanılmaya başlanmasıyla, Azure Blob Depolama farklı fiyatlandırma modelleriyle bu ayrılmış depolama katmanları ihtiyacına hitap ediyor.

## <a name="storage-accounts-that-support-tiering"></a>Katman ayarlamayı destekleyen depolama hesapları

Blob Depolama veya Genel Amaçlı v2 (GPv2) hesaplarda nesne depolama verilerinizi yalnızca sık erişimli, seyrek erişimli ve arşiv seçeneklerine katman ayarlayabilirsiniz. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Bununla birlikte, müşteriler Azure portalında basit bir tek tıklama işlemiyle var olan GPv1 veya Blob Depolama hesaplarını GPv2 hesaplarına kolayca dönüştürebilirler. GPv2, bloblar, dosyalar ve kuyruklar için yeni bir fiyatlandırma yapısı ve yeni diğer birçok depolama özelliğine de erişim sağlar. Ayrıca, bazı yeni özelliklere geçmek ve fiyat indirimleri yalnızca GPv2 hesaplarda sunulur. Bu nedenle, müşteriler GPv2 hesaplarını kullanmayı değerlendirmeli ancak bazı iş yükleri GPv2’de GPv1’den daha pahalı olabileceği için bu hesapları yalnızca tüm hizmetlerin fiyatlarını gözden geçirdikten sonra kullanmalıdır. Daha fazla bilgi için bkz. [Azure depolama hesabı seçenekleri](../common/storage-account-options.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Blob Depolama ve GPv2 hesapları, **Erişim Katmanı** özniteliğini nesne düzeyinde kullanıma sunar; bu ise nesne düzeyinde ayarlanmış katmanı olmayan depolama hesabındaki tüm bloblar için varsayılan depolama katmanını sık veya seyrek erişimli olarak belirtmenizi sağlar. Katmanı nesne düzeyinde ayarlanan nesnelerde hesap katmanı geçerli olmaz. Arşiv katmanı yalnızca nesne düzeyinde uygulanabilir. Bu depolama katmanları arasında istediğiniz zaman geçiş yapabilirsiniz.

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

* Uzun süreli yedekleme, arşivleme ve olağanüstü durum kurtarma veri kümeleri.
* Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*Örneğin*, kodlama diğer biçimlere dönüştürüldükten sonra ham medya dosyaları)
* Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*Örneğin* güvenlik kamerası kayıtları, sağlık kuruluşları için eski röntgen/MRI çekimleri, finans hizmetleri için sesli kayıtlar ve müşteri görüşmesi dökümleri)

### <a name="blob-rehydration"></a>Blob yeniden doldurma
Arşiv depolama birimindeki verileri okumak için katmanı önce sık erişimli veya seyrek erişimli blob katmanı olarak değiştirmeniz gerekir. Bu işleme yeniden doldurma denir ve tamamlanması 15 saat kadar sürebilir. En iyi performans için kesinlikle büyük blob boyutları önerilir. Birkaç küçük blobu aynı anda yeniden doldurmak süreyi uzatabilir.

Yeniden doldurma sırasında katmanın değişip değişmediğini onaylamak için **Arşiv Durumu** blob özelliğini kontrol edebilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.  

## <a name="blob-level-tiering"></a>Blob düzeyinde katman ayarlama

Blob düzeyinde katman ayarlama, [Blob Katmanını Ayarlama](/rest/api/storageservices/set-blob-tier) adlı tek bir işlem kullanarak nesne düzeyinde verilerinizin katmanını değiştirmenize olanak verir. Bir blob’un erişim katmanını, kullanım şekli değiştikçe verileri hesapları arasında taşımaya gerek kalmadan sık erişimli, seyrek erişimli veya arşiv katmanları arasında kolayca değiştirebilirsiniz. Birkaç saat sürebilen blobun arşivden yeniden doldurulması dışında, tüm katman değişiklikleri anında gerçekleşir. Son blob katmanı değişikliğinin zamanı, **Erişim Katmanı Değişim Zamanı** blob özelliği aracılığıyla gösterilir. Blob arşiv katmanındaysa üzerine yazılamayabilir ve bu nedenle aynı blob’un karşıya yüklenmesine bu senaryoda izin verilmez. Sık veya seyrek erişimli blob’ların üzerine yazabilirsiniz ve bu durumda yeni blob, üzerine yazılan eski blobun katmanını devralır.

Aynı hesapta üç farklı depolama katmanına sahip bloblar birlikte bulunabilir. Açıkça atanan bir katmanı olmayan tüm bloblar katmanı hesap erişim katmanının ayarını çıkarır. Erişim katmanı hesaptan alınıyorsa **Erişim Katmanı Alındı** blob özelliğinin “true” olarak ayarlandığını ve blob **Erişim Katmanı** blob özelliğinin hesap katmanıyla uyuştuğunu görürsünüz. Azure portalında, erişim katmanı alındı özelliği, blob erişim katmanı ile birlikte gösterilir (örneğin, Sık Erişilen(alındı) veya Seyrek Erişilen (alındı)).

> [!NOTE]
> Arşiv depolama ve blob düzeyinde katman ayarlama, yalnızca blok bloblarını destekler. Anlık görüntüleri olan bir blok blobun katmanını da değiştiremezsiniz.

### <a name="blob-level-tiering-billing"></a>Blob düzeyinde katman ayarlama faturalandırması

Bir blob daha seyrek erişilen bir katmana taşındığında (sık erişilen->seyrek erişilen, sık erişilen->arşiv veya seyrek erişilen->arşiv), bu işlem hedef katmana yazma olarak faturalandırılır ve hedef katmanın yazma işlemi (10.000 işlem başına) ile veri yazma (GB başına) ücretleri uygulanır. Bir blob daha sık erişilen bir katmana taşınırsa (arşiv->seyrek erişilen, arşiv->sık erişilen veya seyrek erişilen->sık erişilen), bu işlem kaynak katmandan okuma olarak faturalandırılır ve kaynak katmanın okuma işlemi (10.000 işlem başına) ile veri alma (GB başına) ücretleri uygulanır.

Hesap katmanını sık erişilenden seyrek erişilene değiştirirseniz yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemleri (10.000 işlem başına) ücretlendirilir. Blob Depolama hesaplarında bunun için bir ücret yoktur. Blob Depolama veya GPv2 hesabınızı seyrek erişilenden sık erişilene değiştirirseniz hem okuma işlemleri (10.000 işlem başına) hem de veri alma (GB başına) için ücretlendirilirsiniz. Seyrek erişim veya arşiv katmanı dışına taşınmış tüm bloblar için erken silme ücretleri de uygulanabilir.

### <a name="cool-and-archive-early-deletion-effective-february-1-2018"></a>Seyrek erişim ve arşiv erken silme (1 Şubat 2018’den geçerli)

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
> Blob Depolama hesapları, genel amaçlı depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

* GPv2 veya Blob Depolama hesabının varsayılan hesap erişim katmanını değiştirme.
* GPv2 veya Blob Depolama hesabında blobun katmanını değiştirme.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob depolama hesabının varsayılan hesap erişim katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.

4. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>GPv2 veya Blob Depolama hesabında blobun katmanını değiştirin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınızdaki blobunuza gitmek için Tüm Kaynaklar’ı seçin, depolama hesabınızı seçin, kapsayıcınızı seçin ve ardından blobunuzu seçin.

3. Blob özellikleri dikey penceresinde, **Erişim katmanı** açılan menüsüne tıklayarak **Sık Erişilen**, **Seyrek Erişilen** veya **Arşiv** depolama katmanını seçin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

## <a name="faq"></a>SSS

**Verilerime katman ayarlamak istediğimde Blob Depolama’yı mı yoksa GPv2 hesaplarını mı kullanmalıyım?**

Katman ayarlama için Blob Depolama hesapları yerine GPv2 kullanmanızı öneririz. GPv2, Blob Depolama hesaplarının desteklediği tüm özelliklerin yanı sıra başka birçoğunu da destekler. Blob Depolama ile GPv2 ücretleri neredeyse aynıdır, ancak bazı yeni özellikler ve fiyat indirimleri yalnızca GPv2 hesaplarında kullanılabilir. GPv1 hesapları katman ayarlamayı desteklemez.

GPv1 ve GPv2 hesapları arasındaki fiyat yapısı farklıdır ve müşteriler GPv2 hesaplarını kullanmaya karar vermeden önce her ikisini de dikkatle değerlendirmelidir. Var olan bir Blob Depolama veya GPv1 hesabını tek tıklamada basit bir işlemle kolayca GPv2’ye dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabı seçenekleri](../common/storage-account-options.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

**Nesneleri aynı hesapta üç depolama katmanının (sık erişimli, seyrek erişimli ve arşiv) üçüne de depolayabilir miyim?**

Evet. Hesap düzeyinde ayarlanan **Erişim Katmanı** özniteliği, o hesapta bulunan ve ayarlanmış açık bir katmanı olmayan tüm nesneler için varsayılan katman olur. Ancak blob düzeyinde katman ayarlama, hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanını açık olarak ayarlamanıza olanak tanır. Aynı hesapta, üç depolama katmanının (sık erişilen, seyrek erişilen veya arşiv) tümüne ait bloblar bulunabilir.

**Blob veya GPv2 depolama hesabımın varsayılan depolama katmanını değiştirebilir miyim?**

Evet, depolama hesabındaki **Erişim katmanı** özniteliğini ayarlayarak varsayılan depolama katmanını değiştirebilirsiniz. Depolama katmanının değiştirilmesi, hesabta depolanmış ve açıkça belirlenmiş bir katmanı olmayan tüm nesneler için geçerli olur. Sık erişimli depolama katmanını seyrek erişimli olarak değiştirmek, yalnızca GPv2 hesaplarında ayarlanmış katmanı olmayan tüm bloblar için yazma işlemi maliyetleri (10.000 işlem başına), seyrek erişimliden sık erişimliye geçmek de Blob Depolama ve GPv2 hesaplarındaki tüm bloblar için hem okuma işlemi (10.000 işlem başına) hem de veri alma (GB başına) maliyetleri doğurur.

**Varsayılan hesap erişim katmanımı arşiv olarak ayarlayabilir miyim?**

Hayır. Yalnızca sık ve seyrek erişimli depolama katmanları varsayılan hesap erişim katmanı olarak ayarlanabilir. Arşiv yalnızca nesne düzeyinde ayarlanabilir.

**Sık erişimli, seyrek erişimli ve arşiv depolama katmanları hangi bölgelerde kullanılabilir ?**

Sık ve seyrek erişimli depolama katmanları blob düzeyinde katman ayarlama ile birlikte tüm bölgelerde kullanılabilir. Arşiv depolama, başlangıçta yalnızca seçilmiş bölgelerde kullanılabilir. Tam liste için bkz. [Bölgelere göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/regions/services/).

**Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanındakilerden farklı mı davranır?**

Sık erişimli depolama katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek erişimli depolama katmanındaki bloblar GPv1, GPv2 ve Blob Depolama hesaplarındaki bloblarla benzer gecikme süresine (milisaniye olarak) sahiptir. Arşiv depolama katmanındaki bloblar, GPv1, GPv2 ve Blob Depolama hesaplarında birkaç saatlik gecikme süresine sahiptir.

Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanında depolanan bloblara göre daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [Depolama için SLA](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**İşlemler sık erişimli, seyrek erişimli ve arşiv katmanları arasında aynı mıdır?**

Evet. Sık erişimli ve seyrek erişimli arasında tüm işlemleri %100 tutarlıdır. Blob özelliklerini/meta verileri silme, listeleme, alma ve blob katmanını ayarlama dahil olmak üzere tüm geçerli arşiv işlemleri sık ve seyrek erişim arasında %100 tutarlıdır. Bir blob arşiv katmanındayken okunamaz veya değiştirilemez.

**Blobu arşiv katmanından sık erişimli veya seyrek erişimli katmana yeniden doldururken işlemin ne zaman tamamlandığını nasıl bilebilirim?**

Yeniden doldurma işlemi sırasında, katman değişikliğinin ne zaman tamamlandığını onaylamak üzere **Arşiv Durumu** özniteliğini yoklamak için blob özelliklerini alma işlemini kullanabilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve **Erişim Katmanı** blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.  

**Blobun katmanını ayarladıktan sonra buna uygun fiyattan fatura almaya ne zaman başlarım?**

Her blob, daima **Erişim Katmanı** blob özelliği ile belirtilen katmana göre faturalandırılır. Blob üzerinde yeni bir katman ayarlarken blobu arşivden sık veya seyrek erişimliye birkaç saat sürebilecek şekilde yeniden doldurma dışındaki tüm geçişlerde **Erişim Katmanı** özelliği bu yeni katmanı anında yansıtır. Bu durumda, yeniden doldurma tamamlanana kadar arşiv fiyatlarından faturalandırılmaya devam edersiniz ve o noktadan itibaren **Erişim Katmanı** yeni katmanı yansıtır. Ancak bundan sonra, yeni sık veya seyrek erişimli fiyattan faturalandırılırsınız.

**Seyrek erişimli veya arşiv katmanındaki bir blobu silerken veya dışarı taşırken erken silme ücreti ödeyip ödemeyeceğimi nasıl anlarım?**

Seyrek erişimli (yalnızca GPv2 hesapları) veya arşiv katmanından silinen veya dışarı taşınan (sırasıyla 30 ve 180 günden önce) tüm bloblar eşit dağıtılmış bir erken silme ücreti ödenmesini gerektirir (1 Şubat 2018’den geçerli). Son katman değişikliğinin bir damgasını sağlayan **Erişim Katmanı Değişim Zamanı** blob özelliğini kontrol ederek bir blobun ne zamandır seyrek erişimli katmanda veya arşiv katmanında olduğunu belirleyebilirsiniz. Daha fazla ayrıntı için bkz. [Seyrek erişim ve arşiv erken silme](#cool-and-archive-early-deletion) bölümü.

**Hangi Azure araçları ve SDK’lar blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekliyor?**

Azure portalı, PowerShell ve CLI araçları ile .NET, Java, Python ve Node.js istemci kitaplıklarının tümü blob düzeyinde katman ayarlamayı ve arşiv depolamayı destekler.  

**Sık, seyrek ve arşiv katmanlarına ne kadar veri depolayabilirim?**

Diğer sınırlarla birlikte veri depolama da depolama katmanına göre değil hesap düzeyinde ayarlanır. Bu nedenle, sınırınızın tamamını tek bir katmanda veya üç katmanın hepsinde kullanmayı seçebilirsiniz. Daha fazla bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>GPv2 Blob Depolama hesaplarında sık erişimli, seyrek erişimli ve arşiv seçeneklerini değerlendirme

[Bölgeye göre sık, seyrek ve arşiv kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Blob Depolama ve GPv2 hesaplarında bölgeye göre sık, seyrek ve arşiv fiyatlarını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)
