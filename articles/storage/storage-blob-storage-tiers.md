---
title: "Blob’lar için Azure seyrek erişimli depolama | Microsoft Belgeleri"
description: "Azure Blob Depolama için depolama katmanları, nesne verileri için erişim düzenlerini esas alarak uygun maliyetli depolama sunar. Seyrek erişimli depolama katmanı, daha az sıklıkta erişilen veriler için optimize edilmiştir."
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
ms.date: 10/18/2016
ms.author: mihauss
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 998e95611bca6778de601239bcf9c81246dead83


---
# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob Depolama: Sık erişimli ve seyrek erişimli depolama katmanları
## <a name="overview"></a>Genel Bakış
Nasıl kullandığınıza bağlı olarak verilerinizi en uygun maliyetli şekilde depolayabilmeniz için, Azure Storage şimdi Blob Storage (nesne depolama) için iki depolama katmanı sunuyor.  Azure **sık erişimli depolama katmanı** sık erişimli verileri depolamak için optimize edilmiştir. Azure **seyrek erişimli depolama katmanı** daha az sıklıkta erişilen ve uzun süreli verileri depolamak için optimize edilmiştir. Seyrek erişimli depolama katmanındaki veriler, biraz daha düşük bir kullanılabilirliği kabul edebilir, ancak yine de sık erişimli veriler kadar erişim süresi ve verimlilik gerektirir. Seyrek erişimli veriler için, biraz daha düşük kullanılabilirlik SLA’sı ve yüksek erişim maliyetleri, çok daha düşük depolama maliyetleri için kabul edilebilir tercihlerdir.

Bugün, bulutta depolanan veriler büyük bir hızla artmaktadır. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, nasıl oluşturulduğu, işlendiği ve yaşam süresi boyunca nasıl erişildiği açısından oldukça farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında çok sık erişilebilir. Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir.

Yukarıda açıklanan bu veri senaryolarının her biri, belirli erişim düzeni için optimize edilmiş olan farklı hale getirilmiş bir depolama katmanından faydalanır. Sık erişimli ve seyrek erişimli depolama katmanlarının kullanılmaya başlanmasıyla, Azure Blob Depolama şimdi farklı fiyatlandırma modelleriyle bu ayrılmış depolama katmanları ihtiyacına hitap ediyor.

## <a name="blob-storage-accounts"></a>Blob Storage hesapları
**Blob Storage hesapları**, yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage’da depolamanıza yönelik özel depolama hesaplarıdır. Blob depolama hesaplarıyla, artık daha az sıklıkta erişilen seyrek erişimli verilerinizi daha düşük depolama maliyetiyle depolamak ve daha sık erişilen sık erişimli verilerinizi daha düşük erişim maliyetiyle depolamak amacıyla sık erişimli ve seyrek erişimli katmanlar arasında seçim yapabileceksiniz. Blob Storage hesapları, varolan genel amaçlı depolama hesaplarınıza benzer ve blok blobları ve ilave blobları için %100 API tutarlığı dahil günümüzde kullandığınız tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır.

> [!NOTE]
> Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez.
> 
> 

Blob Storage hesapları, hesapta depolanan verilere bağlı olarak depolama katmanını **Sık Erişimli** veya **Seyrek Erişimli** olarak belirlemenize olanak tanıyan **Erişim Katmanı** özniteliğini verir. Verilerinizin kullanım düzeninde bir değişiklik olursa herhangi bir zamanda bu depolama katmanları arasında geçiş yapabilirsiniz.

> [!NOTE]
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve Faturalama](storage-blob-storage-tiers.md#pricing-and-billing) bölümüne bakın.
> 
> 

Sık erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Etkin kullanımda olan veya sık erişilmesi beklenen (okunma ve üzerine yazılma) veriler.
* Seyrek erişimli depolama katmanına işlemeye ya da sonuçta geçişe hazırlanan veriler.

Seyrek erişimli depolama katmanı için örnek kullanım senaryoları şunları içerir:

* Yedekleme, arşivleme ve olağanüstü durum kurtarma veri kümeleri.
* Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
* Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*örn.*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)
* Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*örn.*, Diğer biçimlere kodlandıktan sonra ham medya dosyaları)
* Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*örn.*, Güvenlik kamerası kayıtları, sağlık kuruluşları için eski röntgen/MRI çekimleri, finans hizmetleri için sesli kayıtlar ve müşteri görüşmesi dökümleri)

Depolama hesapları hakkıında daha fazla bilgi için bkz. [Azure Storage hesapları hakkında](storage-create-storage-account.md).

Yalnızca blok veya ilave blobu depolaması gerektiren uygulamalar için, katmanlı depolamanın farklı fiyat avantajlarından faydalanmak üzere Blob Storage hesapları kullanılmasını öneriyoruz. Ancak, bunun aşağıdaki gibi, genel amaçlı depolama hesaplarının kullanılması gerektiği belirli koşullar altında mümkün olmayabileceğini de anlıyoruz:

* Tablo, kuyruk veya dosyaları kullanmanız gerekmesi ve bloblarınızın aynı depolama hesabında saklanmasını istemeniz. Bunları aynı hesapta depolamanın, aynı paylaşılan anahtara sahip olma dışında teknik bir avantajı olmadığını unutmayın.
* Hala Klasik dağıtım modeli kullanmanız gerekmesi. Blob Storage hesapları yalnızca Azure Resource Manager dağıtım modeli aracılığıyla kullanılabilir.
* Sayfa blobları kullanmanız gerekmesi. Blob Storage hesapları sayfa bloblarını desteklemez. Sayfa bloblarını kullanmaya özel olarak ihtiyacınız yoksa, genelde blok bloblarını kullanılmasını öneriyoruz.
* 2014-02-14 tarihinden önceki [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) sürümünü veya 4.x’ten düşük bir istemci kitaplığı sürümü ile kullanmanız ve uygulamanızı güncelleştirememeniz.

> [!NOTE]
> Blob Storage hesapları şu anda çoğu Azure bölgesinde desteklenmektedir; bölge sayısı artacaktır. Kullanılabilir bölgelerin güncelleştirilmiş listesini [Bölgeye göre Azure Hizmetleri ](https://azure.microsoft.com/regions/#services) sayfasında bulabilirsiniz.
> 
> 

## <a name="comparison-between-the-storage-tiers"></a>Depolama katmanları karşılaştırması
Aşağıdaki tabloda iki depolama katmanı arasındaki karşılaştırma vurgulanmıştır:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Sık erişimli depolama katmanı</center></strong></td>
    <td><strong><center>Seyrek erişimli depolama katmanı</center></strong></td
</tr>
<tr>
    <td><strong><center>Kullanılabilirlik</center></strong></td>
    <td><center>%99,9</center></td>
    <td><center>%99</center></td>
</tr>
<tr>
    <td><strong><center>Kullanılabilirlik<br>(RA-GRS okumaları)</center></strong></td>
    <td><center>%99,99</center></td>
    <td><center>%99,9</center></td>
</tr>
<tr>
    <td><strong><center>Kullanım ücretleri</center></strong></td>
    <td><center>Daha yüksek depolama maliyetleri<br>Daha düşük erişim ve işlem maliyetleri</center></td>
    <td><center>Daha düşük depolama maliyetleri<br>Daha yüksek erişim ve işlem maliyetleri</center></td>
</tr>
<tr>
    <td><strong><center>En düşük nesne boyutu<center></strong></td>
    <td colspan="2"><center>Yok</center></td>
</tr>
<tr>
    <td><strong><center>En az depolama süresi<center></strong></td>
    <td colspan="2"><center>Yok</center></td>
</tr>
<tr>
    <td><strong><center>Gecikme süresi<br>(İlk bayta kalan süre)<center></strong></td>
    <td colspan="2"><center>milisaniye</center></td>
</tr>
<tr>
    <td><strong><center>Ölçeklenebilirlik ve performans hedefleri<center></strong></td>
    <td colspan="2"><center>Genel amaçlı depolama hesaplarıyla aynı</center></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Blob Storage hesapları, genel amaçlı depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](storage-scalability-targets.md).
> 
> 

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Blob depolama hesapları, depolama katmanını temel alan yeni bir fiyatlandırma modelini kullanır. Bir Blob Storage hesabı kullanırken, aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti depolama katmanına bağlı olarak değişir. Gigabayt başına maliyet, seyrek erişimli depolama katmanı için sık erişimli depolama katmanına göre olandan düşüktür.
* **Veri erişim maliyetleri**: Seyrek erişimli depolama katmanındaki veriler için, okuma ve yazma işlemleri için erişilen gigabayt veri başına ücretlendirilirsiniz.
* **İşlem maliyetleri**: Her iki katma için işlem başına ücret alınır. Ancak, işlem başına maliyet, seyrek erişimli depolama katmanı için sık erişimli depolama katmanına göre olandan yüksektir.
* **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu, yalnızca GRS ve RA-GRS dahil, coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.
* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.
* **Depolama katmanını değiştirme**: Depolama katmanını seyrek erişimliden sık erişimliye değiştirmek, her geçiş için depolama hesabında varolan tüm verilerin okunmasına eşit bir ücret doğurur. Öte yandan, depolama katmanını sık erişimliden seyrek erişimliye değiştirmek ücretsizdir.

> [!NOTE]
> Kullanıcıların yeni depolama katmanlarını denemesine ve kullanıma başlama öncesi işlevselliği doğrulamasına izin vermek üzere, 30 Haziran 2016 tarihine kadar, depolama katmanını seyrek erişimliden sık erişimliye değiştirmek ücretsizdir. 1 Temmuz 2016’dan başlayarak, seyrek erişimliden sık erişimliye tüm geçiş işlemleri için ücret uygulanacaktır. Blob Storage hesaplarına ilişkin fiyatlandırma modeli hakkında daha fazla bilgi için bkz. [Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için bkz. [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfası.
> 
> 

## <a name="quick-start"></a>Hızlı Başlangıç
Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

* Blob Storage hesabı oluşturma.
* Blob Storage hesabı yönetme.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma
#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Azure portalı kullanarak Blob depolama hesabı oluşturma
1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Hub menüsünde, **Yeni** > **Veri + Depolama** > **Depolama hesabı**’nı seçin.
3. Depolama hesabınız için bir ad girin.
   
    Bu ad genel olarak benzersiz olmalıdır; depolama hesabındaki nesnelere erişmek için kullanılan URL’nin bir parçası olarak kullanılır.  
4. Dağıtım modeli olarak **Kaynak Yöneticisi**’ni seçin.
   
    Katmanlı depolama yalnızca Resource Manager depolama hesaplarıyla birlikte kullanılabilir; yeni kaynaklar için önerilen dağıtım modeli budur. Daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).  
5. Hesap Türü açılır listesinden **Blob Depolama** seçeneğini belirleyin.
   
    Depolama hesabının türünü buradan seçebilirsiniz. Katmanlı depolama genel amaçlı depolamada kullanılamaz; yalnızca Blob depolama türündeki hesapta kullanılabilir.     
   
    Bunu seçtiğinizde performans katmanı Standart olarak ayarlanır. Katmanlı depolama, Premium performans katmanı ile kullanılamaz.
6. Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**, **GRS** veya **RA-GRS**. Varsayılan seçenek **RA-GRS**’dir.
   
    LRS = yerel olarak yedekli depolama; GRS = coğrafi olarak yedekli depolama (2 bölge); RA-GRS okuma erişimli, coğrafi olarak yedekli depolama (ikincisine okuma erişiminin bulunduğu 2 bölge).
   
    Azure Depolama çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Depolama çoğaltma](storage-redundancy.md).
7. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin. Varsayılan seçenek **Sık Erişimli**’dir.
8. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
9. Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).
10. Depolama hesabınız için bölge seçin.
11. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Azure portalı kullanarak Blob depolama hesabındaki depolama katmanını değiştirme
1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.
3. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.
4. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin.
5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

> [!NOTE]
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve Faturalama](storage-blob-storage-tiers.md#pricing-and-billing) bölümüne bakın.
> 
> 

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Blob depolama hesaplarını değerlendirme ve geçiş yapma
Bu bölümün amacı kullanıcıların Blob depolama hesapları kullanmaya sorunsuz bir geçiş yapmasına yardımcı olmaktır. İki kullanıcı senaryosu vardır:

* Genel amaçlı bir depolama hesabınız var ve doğru depolama katmanı ile bir Blob depolama hesabına geçişi değerlendirmek istiyorsunuz.
* Bir Blob depolama hesabı kullanmaya karar verdiğiniz veya zaten bir tane var ve sık erişimli ya da seyrek erişimli depolama katmanı kullanıp kullanmayacağınızı değerlendirmek istiyorsunuz.

Her iki durumda da ilk iş sırası bir Blob depolama hesabında depolanan verileriniz depolama ve erişim maliyetinin tahmin edilmesi ve bu maliyetin mevcut maliyetlerinizle karşılaştırılmasıdır.

### <a name="evaluating-blob-storage-account-tiers"></a>Blob depolama hesabı katmanlarını değerlendirme
Bir Blob depolama hesabına depolanan verilerinizin depolama ve erişim maliyetini tahmin etmek için var olan kullanım modelinizi ve beklediğiniz kullanım modelini yaklaşık olarak değerlendirmeniz gerekir. Genel olarak, şunları bilmek isteyeceksiniz:

* Depolama tüketiminiz – Ne kadar veri depolanıyor ve bu aylık olarak nasıl değişiyor?
* Depolama erişim modelleriniz - Hesaptan ne kadar veri okunuyor ve hesaba ne kadar veri yazılıyor (yeni veriler dahil)? Veri erişimi için kaç tane işlem kullanılıyor ve bunlar ne tür işlemler?

#### <a name="monitoring-existing-storage-accounts"></a>Var olan depolama hesaplarını izleme
Var olan depolama hesaplarınızı izlemek ve bu verileri toplamak için, bir depolama hesabına yönelik günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler sağlayan Azure Depolama Analitiği hizmetinden yararlanabilirsiniz.
Storage Analytics hem genel amaçlı depolama hesapları hem de Blob depolama hesapları için toplu işlem istatistiklerini içerebilen ölçümleri ve Blob depolama hizmetine yapılan isteklere ilişkin kapasite verilerini depolayabilir.
Bu veriler aynı depolama hesabındaki iyi bilinen tablolara depolanır.

Daha fazla bilgi için lütfen bkz. [Storage Analytics Ölçümleri hakkında](https://msdn.microsoft.com/library/azure/hh343258.aspx) ve [Storage Analytics Ölçüm Tablosu Şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob depolama hesapları, tablo hizmeti uç noktasını yalnızca ilgili hesabın ölçüm verilerini depolamak ve bunlara erişmek için ortaya çıkarır.
> 
> 

Blob depolama hizmetinin depolama tüketimini izlemek için kapasite ölçümlerini etkinleştirmeniz gerekir.
Bu özellik etkinleştirildiğinde bir depolama hesabının Blob hizmeti için kapasite verileri günlük olarak kaydedilir ve aynı depolama hesabı içindeki *$MetricsCapacityBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir.

Blob depolama hizmetinin veri erişim modelini izlemek için saatlik işlem ölçümlerini bir API düzeyinde etkinleştirmeniz gerekir.
Bu özellik etkinleştirildiğinde API başına işlemler saatte bir toplanır ve aynı depolama hesabındaki *$MetricsHourPrimaryTransactionsBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir. *$MetricsHourSecondaryTransactionsBlob* tablosu, RA-GRS depolama hesapları için işlemleri ikincil uç noktaya kaydeder.

> [!NOTE]
> Blok genelindeki sayfa blob’larını ve sanal makine disklerini engelleme ve ekleme blob verileriyle birlikte depoladığınız genel amaçlı bir depolama hesabınız varsa bu tahmin işlemi geçerli değildir. Bunun nedeni, yalnızca bir Blob depolama hesabına geçirilebilecek engelleme ve ekleme blob’ları için blob türüne göre kapasite ve işlem ölçümlerini ayırt etmenin bir yolunun olmamasıdır.
> 
> 

Veri tüketim ve erişim modelinizi yaklaşık olarak tahmin etmek için, ölçümler için düzenli kullanımınızı temsil eden bir elde tutma süresi seçmeniz ve tahmin etmeniz önerilir.
Seçeneklerden biri son 7 güne ait ölçüm verilerinin tutulması ve verilerin ay sonunda analiz için haftada bir toplanmasıdır.
Diğer bir seçenek ise son 30 güne ait ölçüm verilerinin tutulması ve verilerin 30 günlük süre sonunda toplanıp çözümlenmesidir.

Ölçüm verilerini etkinleştirme, toplama ve görüntüleme hakkında bilgi için bkz. [Azure Depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](storage-enable-and-view-metrics.md).

> [!NOTE]
> Analiz verilerinin depolanması, erişimi ve indirilmesi de normal kullanıcı verileri gibi ücretlendirilir.
> 
> 

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Kullanım ölçümlerinden yararlanarak maliyetleri tahmin etme
##### <a name="storage-costs"></a>Depolama maliyetleri
*$MetricsCapacityBlob* kapasite ölçüm tablosunda *'data'* satır anahtarını içeren en son giriş, kullanıcı verilerinin harcadığı kapasiteyi gösterir.
*$MetricsCapacityBlob* kapasite ölçüm tablosunda *'analytics'* satır anahtarını içeren en son giriş, analiz günlüklerinin harcadığı kapasiteyi gösterir.

Hem kullanıcı verileri hem de analiz günlükleri (etkinse) tarafından kullanılan bu toplam kapasite, verileri depolama hesabına depolama maliyetini tahmin etmek için kullanılabilir.
Aynı yöntem ayrıca engelleme ve ekleme blob’larının genel amaçları depolama hesaplarına depolanma maliyetlerini tahmin etmek için kullanılabilir.

##### <a name="transaction-costs"></a>İşlem maliyetleri
İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalBillableRequests'* toplamı, ilgili API’nin toplam işlem sayısını belirtir. *Örneğin*, belirli bir süre içindeki *'GetBlob'* işlemlerinin toplam sayısı *'user;GetBlob'* satır anahtarını içeren tüm girişlere yönelik toplam faturalandırılabilir isteklerin toplamına göre hesaplanabilir.

Blob depolama hesaplarına ilişkin işlem maliyetlerini tahmin etmek için, farklı şekilde fiyatlandırıldıkları için işlemleri üç gruba ayırmanız gerekir.

* *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* ve *'CopyBlob'* gibi yazma işlemleri.
* *'DeleteBlob'* ve *'DeleteContainer'* gibi silme işlemleri.
* Diğer tüm işlemler.

Genel amaçlı depolama hesaplarının işlem maliyetlerini tahmin etmek için işlemden/API’den bağımsız olarak tüm işlemleri toplamanız gerekir.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Veri erişimi ve coğrafi çoğaltma veri aktarımı maliyetleri
Storage Analytics bir depolama hesabından okunan ve depolama hesabına yazılan veri miktarını belirtmese de, işlem ölçümleri tablosuna bakılarak bu değer kabaca tahmin edilebilir.
İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalIngress'* toplamı, ilgili API’nin toplam giriş verileri miktarını bayt cinsinden belirtir.
Benzer şekilde, *'TotalEgress'* toplamı toplam çıkış verileri miktarını bayt cinsinden belirtir.

Blob depolama hesaplarına ilişkin veri erişimi maliyetlerini hesaplamak için işlemleri iki gruba ayırmanız gerekir.

* Depolama hesabından alınan veri miktarı birincil olarak *'GetBlob'* ve *'CopyBlob'* işlemleri için *'TotalEgress'* toplamına bakılarak tahmin edilebilir.
* Depolama hesabına yazılan veri miktarı birincil olarak *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* ve *'AppendBlock'* işlemleri için *'TotalIngress'* toplamına bakılarak tahmin edilebilir.

Blob depolama hesaplarında coğrafi çoğaltma veri aktarımı maliyeti de bir GRS veya RA-GRS depolama hesabı söz konusu olduğunda yazılan veri miktarı tahmini kullanılarak hesaplanabilir.

> [!NOTE]
> Seyrek veya sık erişimli bir depolama katmanını kullanma maliyetlerini hesaplama hakkında daha ayrıntılı bir örnek için lütfen *'Sık ve Seyrek Erişimli erişim katmanları nelerdir ve hangisinin kullanılacağını nasıl belirlemeliyim?'* başlıklı SSS bölümüne bakın bkz. [Azure Depolama Fiyatlandırma Sayfası](https://azure.microsoft.com/pricing/details/storage/).
> 
> 

### <a name="migrating-existing-data"></a>Mevcut verileri geçirme
Bir Blob Storage hesabı yalnızca blok ve ilave bloblarının depolanmasına yöneliktir. Blobların yanı sıra tablo, kuyruk, dosya ve diskleri de depolamanızı sağlayan mevcut genel amaçlı depolama hesapları Blob depolama hesaplarına dönüştürülemez. Depolama katmanlarını kullanmak için, yeni Blob Storage hesapları oluşturmanız ve varolan verilerinizi yeni oluşturulan hesaplara taşımanız gerekir.
Mevcut verileri şirket içi depolama aygıtlarından, üçüncü taraf bulut depolama sağlayıcılardan ya da Azure’daki mevcut genel amaçlı depolama hesaplarınızdan Blob Storage hesaplarına geçirmek için aşağıdaki yöntemleri kullanabilirsiniz:

#### <a name="azcopy"></a>AzCopy
AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy yardımcı programını, verileri genel amaçlı depolama hesaplarınızdan Blob Storage hesabınıza kopyalamak ya da şirket içi depolama aygıtlarınızdaki verileri Blob Storage hesabınıza yüklemek için kullanabilirsiniz.

Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Veri Hareketi Kitaplığı
.NET için Azure Storage veri hareketi kitaplığı AzCopy’yi çalıştıran çekirdek veri hareketi altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. Bu, AzCopy’nin dış örneklerini çalıştırmanıza ve izlemenize gerek kalmadan, AzCopy tarafından uygulamanızda yerel olarak sağlanan özelliklerden tam olarak faydalanmanızı sağlar.

Daha fazla ayrıntı için bkz. [.Net için Azure Storage Veri Hareketi Kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı
Azure istemci kitaplıklarından birini ya da Azure Storage hizmetleri REST API’sini kullanarak verilerinizi Blob Storage hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Daha fazla bilgi için,bkz. [Azure Blob Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Bloblar, blobla depolanan istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. Tüm kopyalama mekanizmalarının blob verilerinin ve özellikle şifrelemeyle ilgili meta verilerin korunduğundan emin olması kesinlikle önemlidir. Blobları bu meta veriler olmadan kopyalarsanız, blob içeriği tekrar alınabilir olmaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Storage istemci tarafı şifreleme](storage-client-side-encryption.md).
> 
> 

## <a name="faqs"></a>SSS
1. **Mevcut depolama hesapları hâlâ kullanılabilir mi?**
   
    Evet, var olan depolama hesapları hala kullanılabilir ve fiyatlandırma veya işlev açısından bir farklılık göstermez.  Bunlar depolama katmanı seçme olanağına sahip değildir ve gelecekte katmanlama özelliğine sahip olmayacaktır.
2. **Neden ve ne zaman Blob depolama hesapları kullanmaya başlamalıyım?**
   
    Blob Storage hesapları blobları depolamak içindir yeni blob merkezli özellikleri sunmamıza izin verir. Dahası, bu hesap türüne göre hiyerarşik depolama ve katmanlama gibi özellikler gelecekte sunulacağından, Blob Storage hesapları blobları depolamak için önerilen yoldur. Ancak, ne zaman geçiş yapacağınız iş gereksinimleriniz temelinde size bağlıdır.
3. **Mevcut depolama hesabımı Blob depolama hesabına dönüştürebilir miyim?**
   
    Hayır. Blob Storage hesabı farklı türde bir depolama hesabıdır ve yeni bir tane oluşturmanız ve yukarıda açıklandığı şekilde verilerinizi taşımanız gereklidir.
4. **Nesneleri aynı hesaptaki iki depolama katmanında depolayabilir miyim?**
   
    Depolama katmanını belirten *‘Erişim Katmanı’* özniteliği hesap düzeyinde ayarlanır ve bu hesaptaki tüm nesnelere uygulanır. Erişim katmanı özniteliğini nesne düzeyinde ayarlayamazsınız.
5. **Blob depolama hesabımdaki depolama katmanını değiştirebilir miyim?**
   
    Evet. Depolama hesabındaki *'Erişim Katmanı'* özniteliğini ayarlayarak depolama katmanını değiştirebilirsiniz. Depolama katmanının değiştirilmesi hesaba depolanmış tüm nesneler için geçerli olur. Depolama katmanını sık erişimliden seyrek erişimliye değiştirmek bir ücret doğurmazken, seyrek erişimliden sık erişimliye değiştirmek hesaptaki tüm verilerin okunması için GB başına maliyet doğurur.
6. **Blob depolama hesabımdaki depolama katmanını hangi sıklıkta değiştirebilirim?**
   
    Depolama katmanını değiştirme sıklığına ilişkin bir sınırlama koymuyoruz, ancak depolama katmanını seyrek erişimliden sık erişimliye değiştirmenin büyük maliyetler doğurduğuna dikkat edin. Depolama katmanını sık değiştirmenizi önermiyoruz.
7. **Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanındakilerden farklı mı davranacak?**
   
    Sık erişimli depolama katmanındaki bloblar genel amaçlı depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek erişimli depolama katmanındaki bloblar genel amaçlı depolama hesaplarındaki bloblarla benzer gecikme süresine (milisaniye olarak) sahiptir.
   
    Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanında depolanan bloblara göre daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [Depolama için SLA](https://azure.microsoft.com/support/legal/sla/storage).
8. **Sayfa bloblarını ve sanal makine disklerini Blob depolama hesaplarında depolayabilir miyim?**
   
    Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez. Azure Virtual Machine diskleri sayfa blobları tarafından yedeklenir ve bu nedenle sanal makine disklerini depolamak için Blob Storage hesapları kullanılamaz. Ancak, sanal makine disklerinin yedeklerini blok blobları olarak Blob Storage hesabında depolamak mümkündür.
9. **Blob depolama hesaplarını kullanmak için mevcut uygulamalarımı değiştirmem gerekecek mi?**
   
    Blob Storage hesapları, blok ve ilave blobları için genel amaçlı depolama hesaplarıyla % 100 API tutarlıdır. Uygulamanız blok veya ilave bloblarını kullandığı ve [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx)’sinin 2014-02-14 sürümünü veya üstünü kullandığınız sürece, uygulamanız çalışmaya devam etmelidir. Protokolün daha eski bir sürümünü kullanıyorsanız, her iki tür depolama hesabıyla sorunsuz çalışarak yeni sürümü kullanmak için uygulamanızı güncelleştirmeniz gerekecektir. Genel olarak, hangi depolama hesabını kullandığınızdan bağımsız olarak her zaman en son sürümü kullanmanızı öneriyoruz.
10. **Kullanıcı deneyiminde bir değişiklik olacak mı?**
    
    Blob Storage hesapları blok ve ilave bloblarını depolamak için genel amaçlı depolama hesaplarına çok benzer ve yüksek dayanıklılık ve kullanılabilirlik, ölçeklenebilirlik, performans ve güvenlik dahil olmak üzere Azure Storage’ın tüm anahtar özelliklerini destekler. Blob depolama hesaplarına özgü özellikler ve kısıtlamalar ve yukarıda bahsedilen depolama katmanları dışındaki her şey aynı kalır.

## <a name="next-steps"></a>Sonraki adımlar
### <a name="evaluate-blob-storage-accounts"></a>Blob Storage hesaplarını değerlendirme
[Bölgeye göre Blob depolama hesaplarının kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](storage-enable-and-view-metrics.md)

[Bölgeye göre Blob depolama fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Blob Storage hesaplarını kullanmaya başlama
[Azure Blob depolamayı kullanmaya başlama](storage-dotnet-how-to-use-blobs.md)

[Azure Depolama’ya ve Azure Depolama’da veri taşıma](storage-moving-data.md)

[AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)

[Depolama hesaplarınıza göz atma ve bu hesapları keşfetme](http://storageexplorer.com/)




<!--HONumber=Dec16_HO1-->


