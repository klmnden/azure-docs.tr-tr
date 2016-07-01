<properties
    pageTitle="Bloblar için Azure Seyrek Erişimli Depolama | Microsoft Azure"
    description="Blob Storage için depolama katmanları, nesne verileri için erişim düzenlerini esas alarak uygun maliyetli depolama sunar. Azure seyrek erişimli depolama, daha az sıklıkta erişilen veriler için optimize edilmiştir."
    services="storage"
    documentationCenter=""
    authors="sribhat-msft"
    manager=""
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="sribhat"/>


# Azure Blob Storage: Sık Erişimli ve Seyrek Erişimli Depolama Katmanları

## Genel Bakış

Nasıl kullandığınıza bağlı olarak verilerinizi en uygun maliyetli şekilde depolayabilmeniz için, Azure Storage şimdi Blob Storage (nesne depolama) için iki depolama katmanı sunuyor.  Azure **sık erişimli depolama katmanı** sık erişimli verileri depolamak için optimize edilmiştir. Azure **seyrek erişimli depolama katmanı** daha az sıklıkta erişilen ve uzun süreli verileri depolamak için optimize edilmiştir. Seyrek erişimli depolama katmanındaki veriler, biraz daha düşük bir kullanılabilirliği kabul edebilir, ancak yine de sık erişimli veriler kadar erişim süresi ve verimlilik gerektirir. Seyrek erişimli veriler için, biraz daha düşük kullanılabilirlik SLA’sı ve yüksek erişim maliyetleri, çok daha düşük depolama maliyetleri için kabul edilebilir tercihlerdir.

Bugün, bulutta depolanan veriler büyük bir hızla artmaktadır. Artan depolama ihtiyaçlarınızın maliyetlerini yönetmek için, erişim sıklığı ve planlanan elde tutma dönemi gibi özniteliklere bağlı olarak verilerinizi düzenlemek yararlıdır. Bulutta depolanan veriler, nasıl oluşturulduğu, işlendiği ve yaşam süresi boyunca nasıl erişildiği açısından oldukça farklı olabilir. Bazı veriler ve yaşam süresi boyunca aktif şekilde erişilebilir ve değiştirilebilir. Bazı verilere, veriler eskidikçe önemli ölçüde azalan erişimle, yaşam sürelerinin başlarında çok sık erişilebilir. Bazı veriler bulutta boşta kalır ve depolandıktan sonra, olursa, nadiren erişilir.

Yukarıda açıklanan bu veri senaryolarının her biri, belirli erişim düzeni için optimize edilmiş olan farklı hale getirilmiş bir depolama katmanından faydalanır. Sık erişimli ve seyrek erişimli depolama erişim katmanlarının kullanılmaya başlanmasıyla, Azure Blob Storage şimdi farklı fiyatlandırma modelleriyle bu ayrılmış depolama katmanları ihtiyacına hitap ediyor.

## Blob Storage hesapları

**Blob Storage hesapları**, yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage’da depolamanıza yönelik özel depolama hesaplarıdır. Blob Storage hesaplarıyla, artık daha az sıklıkta erişilen seyrek erişimli verilerinizi daha düşük depolama maliyetiyle depolamak ve daha sık erişilen sık erişimli verilerinizi daha düşük erişim maliyetiyle depolamak amacıyla sık erişimli ve seyrek erişimli erişim katmanları arasında seçim yapabileceksiniz. Blob Storage hesapları, varolan genel amaçlı depolama hesaplarınıza benzer ve blok blobları ve ilave blobları için %100 API tutarlığı dahil günümüzde kullandığınız tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır.

> [AZURE.NOTE] Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez.

Blob Storage hesapları, hesapta depolanan verilere bağlı olarak depolama katmanını **Sık Erişimli** veya **Seyrek Erişimli** olarak belirlemenize olanak tanıyan **Erişim Katmanı** özniteliğini verir. Verilerinizin kullanım düzeninde bir değişiklik olursa,herhangi bir zamanda bu erişim katmanları arasında geçiş yapabilirsiniz.

> [AZURE.NOTE] Erişim katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve Faturalama](storage-blob-storage-tiers.md#pricing-and-billing) bölümüne bakın.

Sık erişimli depolama erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Etkin kullanımda olan veya sık erişilmesi beklenen (okunma ve üzerine yazılma) veriler.
- Seyrek erişimli depolama katmanına işlemeye ya da sonuçta geçişe hazırlanan veriler.

Seyrek erişimli depolama erişim katmanı için örnek kullanım senaryoları şunları içerir:

- Yedekleme, arşivleme ve olağanüstü durum kurtarma veri kümeleri.
- Artık sık görüntülenmeyen ancak erişildiğinde hemen kullanılabilir olması beklenen eski medya içeriği.
- Gelecekte işlenmek üzere daha fazla veri toplanırken uygun maliyetli olarak depolanması gereken büyük veri kümeleri. (*örn.*, bilimsel verilerin uzun süreli depolanması, üretim tesisinden alınan ham telemetri verileri)
- Son kullanılabilir biçime işlendikten sonra bile özgün (ham) veriler korunmalıdır. (*örn.*, Diğer biçimlere kodlandıktan sonra ham medya dosyaları)
- Uzun süre depolanması gereken ve çok ender erişilecek uyumluluk ve arşiv verileri. (*örn.*, Güvenlik kamerası kayıtları, sağlık kuruluşları için eski röntgen/MRI çekimleri, finans hizmetleri için sesli kayıtlar ve müşteri görüşmesi dökümleri)

Depolama hesapları hakkıında daha fazla bilgi için bkz. [Azure Storage hesapları hakkında](storage-create-storage-account.md).

Yalnızca blok veya ilave blobu depolaması gerektiren uygulamalar için, katmanlı depolamanın farklı fiyat avantajlarından faydalanmak üzere Blob Storage hesapları kullanılmasını öneriyoruz. Ancak, bunun aşağıdaki gibi, genel amaçlı depolama hesaplarının kullanılması gerektiği belirli koşullar altında mümkün olmayabileceğini de anlıyoruz:

- Tablo, kuyruk veya dosyaları kullanmanız gerekmesi ve bloblarınızın aynı depolama hesabında saklanmasını istemeniz. Bunları aynı hesapta depolamanın, aynı paylaşılan anahtara sahip olma dışında teknik bir avantajı olmadığını unutmayın.
- Hala Klasik dağıtım modeli kullanmanız gerekmesi. Blob Storage hesapları yalnızca Azure Resource Manager dağıtım modeli aracılığıyla kullanılabilir.
- Sayfa blobları kullanmanız gerekmesi. Blob Storage hesapları sayfa bloblarını desteklemez. Sayfa bloblarını kullanmaya özel olarak ihtiyacınız yoksa, genelde blok bloblarını kullanılmasını öneriyoruz.
- 2014-02-14 tarihinden önceki [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) sürümünü veya 4.x’ten düşük bir istemci kitaplığı sürümü ile kullanmanız ve uygulamanızı güncelleştirememeniz.

> [AZURE.NOTE] Blob Storage hesapları şu anda çoğu Azure bölgesinde desteklenmektedir; bölge sayısı artacaktır. Kullanılabilir bölgelerin güncelleştirilmiş listesini [Bölgeye göre Azure Hizmetleri ](https://azure.microsoft.com/regions/#services) sayfasında bulabilirsiniz.

## Erişim katmanları karşılaştırması

Aşağıdaki tabloda iki erişim katmanı arasındaki karşılaştırma vurgulanmıştır:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Sık erişimli depolama erişim katmanı</center></strong></td>
    <td><strong><center>Seyrek erişimli depolama erişim katmanı</center></strong></td
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

> [AZURE.NOTE] Blob Storage hesapları, genel amaçlı depolama hesaplarıyla aynı performans ve ölçeklenebilirlik hedeflerini destekler. Daha fazla bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](storage-scalability-targets.md).

## Fiyatlandırma ve Faturalama

Blob Storage hesapları, erişim katmanını temel alan yeni bir fiyatlandırma modelini kullanır. Bir Blob Storage hesabı kullanırken, aşağıdaki fatura değerlendirmeleri geçerlidir:

- **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti erişim katmanına bağlı olarak değişir. Gigabayt başına maliyet, seyrek erişimli depolama erişim katmanı için sık erişimli depolama erişim katmanına göre olandan düşüktür.
- **Veri erişim maliyetleri**: Seyrek erişimli depolama erişim katmanındaki veriler için, okuma ve yazma işlemleri için erişilen gigabayt veri başına ücretlendirilirsiniz.
- **İşlem maliyetleri**: Her iki katma için işlem başına ücret alınır. Ancak, işlem başına maliyet, seyrek erişimli depolama erişim katmanı için sık erişimli depolama erişim katmanına göre olandan yüksektir.
- **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu, yalnızca GRS ve RA-GRS dahil, coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.
- **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.
- **Erişim katmanını değiştirme**: Erişim katmanını seyrek erişimliden sık erişimliye değiştirmek, her geçiş için depolama hesabında varolan tüm verilerin okunmasına eşit bir ücret doğurur. Öte yandan, erişim katmanını sık erişimliden seyrek erişimliye değiştirmek ücretsizdir.

> [AZURE.NOTE] Kullanıcıların yeni depolama katmanlarını denemesine ve kullanıma başlama öncesi işlevselliği doğrulamasına izin vermek üzere, 30 Haziran 2016 tarihine kadar, erişim katmanını seyrek erişimliden sık erişimliye değiştirmek ücretsizdir. 1 Temmuz 2016’dan başlayarak, seyrek erişimliden sık erişimliye tüm geçiş işlemleri için ücret uygulanacaktır. Blob Storage hesaplarına ilişkin fiyatlandırma modeli hakkında daha fazla bilgi için bkz. [Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için bkz. [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfası.

## Hızlı Başlangıç

Bu bölümde Azure Portal kullanarak aşağıdaki senaryolar gösterilmektedir:

- Blob Storage hesabı oluşturma.
- Blob Storage hesabı yönetme.

### Azure Portal’ı kullanma

#### Azure Portal'ı kullanarak Blob Storage hesabı oluşturma

1. [Azure Portal](https://portal.azure.com)’da oturum açın.

2. Hub menüsünde, **Yeni** -> **Veri + Depolama** -> **Depolama hesabı**’nı seçin.

3. Depolama hesabınız için bir ad girin.

4. Dağıtım modeli olarak **Kaynak Yöneticisi**’ni seçin.

5. Depolama hesabı türü olarak **Blob Storage**’ı seçin.

6. Erişim katmanını seçin: **Sık Erişimli** veya **Seyrek Erişimli**. Varsayılan seçenek **Sık Erişimli**’dir.

7. Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**, **GRS** veya **RA-GRS**. Varsayılan seçenek **RA-GRS**’dir. Azure Storage çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Storage çoğaltma](storage-redundancy.md).

8. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.

9. Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için Azure Portal’ı kullanma](../azure-portal/resource-group-portal.md)

10. Depolama hesabınız için coğrafi konumu seçin.

11. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

#### Azure Portal'ı kullanarak Blob Storage hesabındaki erişim katmanını değiştirme

1. [Azure Portal](https://portal.azure.com)’da oturum açın ve depolama hesabınıza gidin.

2. **Tüm ayarlar**’a tıklayın ve ardından hesap yapılandırmasını görüntülemek ve/veya yapılandırmak için **Yapılandırma**’ya tıklayın.

3. İstenen erişim katmanını belirtin: **Sık Erişimli** veya **Seyrek Erişimli**.

    > [AZURE.NOTE] Erişim katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve Faturalama](storage-blob-storage-tiers.md#pricing-and-billing) bölümüne bakın.

## Blob Storage hesaplarına geçme

Bu bölümün amacı kullanıcıların Blob Storage hesaplarına sorunsuz bir geçiş yapmasına yardımcı olmaktır. Bir Blob Storage hesabı yalnızca blok ve ilave bloblarının depolanmasına yöneliktir. Blobların yanı sıra tablo, kuyruk, dosya ve diskleri de depolamanızı sağlayan mevcut genel amaçlı depolama hesapları Blob Storage hesaplarına dönüştürülemez. Depolama katmanlarını kullanmak için, yeni Blob Storage hesapları oluşturmanız ve varolan verilerinizi yeni oluşturulan hesaplara taşımanız gerekir.

### Mevcut verilerin geçişini planlama

Verilerinizi bir Blob Storage hesabına taşıyorsanız, büyük olasılıkla daha az sık kullanılan veri depolama maliyetlerinden tasarruf için sık erişimli depolama erişim katmanı avantajından yararlanmak istersiniz. Soğuk erişimli erişim katmanındaki bir Blob Storage hesabında bulunan veri geçişini planlamanın ilk adımı, bir Blob Storage hesabına geçişten faydalanıp faydalanamayacağınızı belirlemek üzere mevcut kullanım düzeninizi değerlendirmektir. Genel olarak, şunları bilmek isteyeceksiniz:

- Depolama tüketim düzeniniz – Ne kadar veri depolanıyor ve bu aylık olarak nasıl değişiyor?
- Depolama erişim düzenleriniz – Hesaptan ne kadar veri okunuyor ve hesaba ne kadar veri yazılıyor (yeni veriler dahil)? Veri erişimi için kaç tane ve hangi işlemler kullanılıyor?

Varolan depolama hesaplarınızı izlemek ve bu verileri toplamak için, lütfen [Azure Storage ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](storage-enable-and-view-metrics.md). Şimdi bu verileri kullanarak maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Storage Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)’yı kullanabilirsiniz.

### Mevcut verileri geçirme

Mevcut verileri şirket içi depolama aygıtlarından, üçüncü taraf bulut depolama sağlayıcılardan ya da Azure’daki mevcut genel amaçlı depolama hesaplarınızdan Blob Storage hesaplarına geçirmek için aşağıdaki yöntemleri kullanabilirsiniz:

#### AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy yardımcı programını, verileri genel amaçlı depolama hesaplarınızdan Blob Storage hesabınıza kopyalamak ya da şirket içi depolama aygıtlarınızdaki verileri Blob Storage hesabınıza yüklemek için kullanabilirsiniz.

Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](storage-use-azcopy.md).

#### Veri Hareketi Kitaplığı

.NET için Azure Storage veri hareketi kitaplığı AzCopy’yi çalıştıran çekirdek veri hareketi altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. Bu, AzCopy’nin dış örneklerini çalıştırmanıza ve izlemenize gerek kalmadan, AzCopy tarafından uygulamanızda yerel olarak sağlanan özelliklerden tam olarak faydalanmanızı sağlar.

Daha fazla ayrıntı için bkz. [.Net için Azure Storage Veri Hareketi Kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

#### REST API’si veya istemci kitaplığı

Azure istemci kitaplıklarından birini ya da Azure Storage hizmetleri REST API’sini kullanarak verilerinizi Blob Storage hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Daha fazla bilgi için,bkz. [Azure Blob Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Bloblar, blobla depolanan istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. Tüm kopyalama mekanizmalarının blob verilerinin ve özellikle şifrelemeyle ilgili meta verilerin korunduğundan emin olması kesinlikle önemlidir. Blobları bu meta veriler olmadan kopyalarsanız, blob içeriği tekrar alınabilir olmaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Storage istemci tarafı şifreleme](storage-client-side-encryption.md).

## SSS

1. **Varolan depolama hesapları hala kullanılabilir mi?**

    Evet, var olan depolama hesapları hala kullanılabilir ve fiyatlandırma veya işlev açısından bir farklılık göstermez.  Bunlar erişim katmanı seçme olanağına sahip değildir ve gelecekte katmanlama özelliğine sahip olmayacaktır.

2. **Neden ve ne zaman Blob Storage hesapları kullanmaya başlamalıyım?**

    Blob Storage hesapları blobları depolamak içindir yeni blob merkezli özellikleri sunmamıza izin verir. Dahası, bu hesap türüne göre hiyerarşik depolama ve katmanlama gibi özellikler gelecekte sunulacağından, Blob Storage hesapları blobları depolamak için önerilen yoldur. Ancak, ne zaman geçiş yapacağınız iş gereksinimleriniz temelinde size bağlıdır.

3. **Varolan depolama hesabımı Blob Storage hesabına dönüştürebilir miyim?**

    Hayır. Blob Storage hesabı farklı türde bir depolama hesabıdır ve yeni bir tane oluşturmanız ve yukarıda açıklandığı şekilde verilerinizi taşımanız gereklidir.

4. **Nesneleri aynı hesaptaki iki erişim katmanında depolayabilir miyim? **

    Erişim katmanı özniteliği hesap düzeyinde ayarlanır ve bu hesaptaki tüm nesnelere uygulanır. Nesne düzeyinde erişim katmanı ayarlayamazsınız.

5. **Blob Storage hesabımdaki erişim katmanını değiştirebilir miyim?**

    Evet. Depolama hesabındaki erişim katmanını değiştirebilirsiniz. Hesap düzeyinde bir erişim katmanını değiştirmek hesapta depolanan tüm nesnelere uygulanır. Erişim katmanını sık erişimliden seyrek erişimliye değiştirmek bir ücret doğurmazken, seyrek erişimliden sık erişimliye değiştirmek hesaptaki tüm verilerin okunması için GB başına maliyet doğurur.

6. **Blob Storage hesabımdaki erişim katmanını hangi sıklıkta değiştirebilirim?**

    Erişim katmanını değiştirme sıklığına ilişkin bir sınırlama koymuyoruz ancak erişim katmanını seyrek erişimliden sık erişimliye değiştirmenin büyük maliyetler doğurduğuna dikkat edin. Erişim katmanını sık değiştirmenizi önermiyoruz.

7. **Seyrek erişimli depolama erişim katmanındaki bloblar, sık erişimli depolama erişim katmanındakilerden farklı mı davranacak?**

    Sık erişimli depolama erişim katmanındaki bloblar genel amaçlı depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek erişimli depolama erişim katmanındaki bloblar genel amaçlı depolama hesaplarındaki bloblarla benzer gecikme süresine (milisaniye olarak) sahiptir.

    Seyrek erişimli depolama erişim katmanındaki bloblar, sık erişimli depolama erişim katmanında depolanan bloblara göre daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [Depolama için SLA](https://azure.microsoft.com/support/legal/sla/storage).

8. **Sayfa bloblarını ve sanal makine disklerini Blob Storage hesaplarında depolayabilir miyim?**

    Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez. Azure Virtual Machine diskleri sayfa blobları tarafından yedeklenir ve bu nedenle sanal makine disklerini depolamak için Blob Storage hesapları kullanılamaz. Ancak, sanal makine disklerinin yedeklerini blok blobları olarak Blob Storage hesabında depolamak mümkündür.

9. **Varolan uygulamalarımı Blob Storage hesaplarını kullanmak için değiştirmem gerekecek mi?**

    Blob Storage hesapları, blok ve ilave blobları için genel amaçlı depolama hesaplarıyla % 100 API tutarlıdır. Uygulamanız blok veya ilave bloblarını kullandığı ve [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx)’sinin 2014-02-14 sürümünü veya üstünü kullandığınız sürece, uygulamanız çalışmaya devam etmelidir. Protokolün daha eski bir sürümünü kullanıyorsanız, her iki tür depolama hesabıyla sorunsuz çalışarak yeni sürümü kullanmak için uygulamanızı güncelleştirmeniz gerekecektir. Genel olarak, hangi depolama hesabını kullandığınızdan bağımsız olarak her zaman en son sürümü kullanmanızı öneriyoruz.

10. **Kullanıcı deneyiminde bir değişiklik olacak mı?**

    Blob Storage hesapları blok ve ilave bloblarını depolamak için genel amaçlı depolama hesaplarına çok benzer ve yüksek dayanıklılık ve kullanılabilirlik, ölçeklenebilirlik, performans ve güvenlik dahil olmak üzere Azure Storage’ın tüm anahtar özelliklerini destekler. Blob Storage hesaplarına özgü özellikler ve kısıtlamalar ve yukarıda bahsedilen erişim katmanları dışındaki her şey aynı kalır.

## Sonraki adımlar

### Blob Storage hesaplarını değerlendirme

[Bölgeye göre Blob Storage hesaplarının kontrol denetleme](https://azure.microsoft.com/regions/#services)

[Azure Storage ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](storage-enable-and-view-metrics.md)

[Bölgeye göre Blob Storage fiyatlandırmayı denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)

### Blob Storage hesaplarını kullanmaya başlama

[Azure Blob Storage’ı kullanmaya başlama](storage-dotnet-how-to-use-blobs.md)

[Azure Storage’a ve Azure Storage’da veri taşıma](storage-moving-data.md)

[AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)



<!--HONumber=Jun16_HO2-->


