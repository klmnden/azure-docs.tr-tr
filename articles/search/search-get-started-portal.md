<properties 
    pageTitle="Azure Search ile çalışmaya başlama | Microsoft Azure | Azure Search ile çalışmaya başlama | DocumentDB | Bulut arama hizmeti" 
    description="Bu öğretici kılavuzu kullanarak ilk Azure Search çözümünüzü oluşturun. DocumentDB verilerini kullanarak bir Azure Search dizininin nasıl oluşturulacağını öğrenin. Bu, Veri İçeri Aktarma sihirbazının kullanıldığı portal tabanlı ve kodsuz bir alıştırmadır." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="paulettm" 
    editor=""
    tags="azure-portal"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="hero-article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/17/2016" 
    ms.author="heidist"/>

# Portalda Azure Search ile çalışmaya başlama

Bu kodsuz giriş, doğrudan portal içinde yerleşik olarak sunulan işlevleri kullanarak Microsoft Azure Search ile çalışmaya başlamanızı sağlar. 

Öğretici, verilerimizi ve yönergelerimizi kullanarak bir [örnek Azure DocumentDB veritabanı](#apdx-sampledata) oluşturmanın kolay olduğunu varsayar ancak bu adımları DocumentDB veya SQL Database'de var olan verilerinize de uyarlayabilirsiniz.

> [AZURE.NOTE] Bu Başlarken öğreticisi için bir [Azure aboneliği](../../includes/free-trial-note.md) ve [Azure Search hizmeti](search-create-service-portal.md) gerekir. 
 
## Hizmetinizi bulma

1. [Azure Portal](https://portal.azure.com)'da oturum açın.

2. Azure Search hizmetinizin hizmet panosunu açın. Panoyu bulmanın birkaç yolu burada verilmiştir.
    - Harf çubuğunda **Arama hizmetleri**'ne tıklayın. Harf çubuğu, aboneliğinizde sağlanan tüm hizmeti listeler. Bir arama hizmeti tanımlanmışsa listede **Arama hizmetleri**'ni görürsünüz.
    - Harf çubuğunda **Gözat**'a tıklayın ve ardından aboneliklerinizde oluşturulan tüm arama hizmetlerinin bir listesini oluşturmak için arama kutusuna "ara" yazın.

## Alan denetleme

Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç dizin, üç veri kaynağı ve üç dizin oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu kılavuz, nesnelerin her birinden birer tane oluşturur.

## Bir dizin ve yük verileri oluşturma

Arama sorguları, belirli arama davranışlarını iyileştirmek için kullanılan aranabilir verileri, meta verileri ve yapıları içeren bir *dizinde* yinelenir. İlk adım olarak, bir dizin tanımlayıp dolduracaksınız.

Bir dizin oluşturmanın birkaç yolu vardır. Verileriniz Azure SQL Database, Azure VM'deki SQL Server veya DocumentDB gibi Azure Search'ün gezinebileceği bir depodaysa *dizin oluşturucu* kullanarak dizini çok kolay bir şekilde oluşturup doldurabilirsiniz.

Bu görevi portal tabanlı tutmak için **Veri içeri aktarma** sihirbazı aracılığıyla bir dizin oluşturucu kullanarak gezinilebilen DocumentDB'den veri varsayacağız. 

Devam etmeden önce bu öğreticiyle kullanmak üzere bir [örnek DocumentDB veritabanı](#apdx-sampledata) oluşturun ve ardından aşağıdaki adımları tamamlamak için bu bölüme geri dönün.

<a id="defineDS"></a>
#### 1. Adım: Veri kaynağını tanımlama

1. Azure Search hizmeti panonuzda, bir dizini hem oluşturan hem de dolduran bir sihirbazı başlatmak için komut çubuğundaki **Veri içeri aktar** seçeneğine tıklayın.

  ![][7]

2. Sihirbazda **Veri Kaynağı** > **DocumentDB** > **Ad**'a tıklayın ve veri kaynağı için bir ad yazın. Veri kaynağı, diğer dizin oluşturucularla kullanılabilen bir Azure Search bağlantı nesnesidir. Oluşturulduktan sonra, hizmetinizde "var olan bir veri kaynağı" olarak kullanılabilir hale gelir.

3. Var olan DocumentDB hesabınızı, veritabanını ve koleksiyonu seçin. Bizim sağladığımız örnek verileri kullanıyorsanız veri kaynağı tanımınız şöyle görünür:

  ![][2]

Sorguyu atladığımıza dikkat edin. Bunun nedeni, veri kümemize bu sefer değişiklik takibi uygulamıyor olmamızdır. Veri kümenizde bir kaydın güncelleştirilmesini takip eden bir alan varsa dizininizde tercihe bağlı güncelleştirmeler için değişiklik takibi kullanma amacıyla bir Azure Search dizin oluşturucusunu yapılandırabilirsiniz.

Sihirbazın bu adımını tamamlamak için **Tamam**'a tıklayın.

#### 2. Adım: Dizini tanımlama

Hâlâ sihirbazdayken **Dizin**'e tıklayın ve bir Azure Search dizininin oluşturulması için kullanılan tasarım yüzeyine göz atın. Bir dizin için en az olarak bir ad ve bir alan belge anahtarı olarak işaretlenmiş şekilde bir alanlar koleksiyonu gerekir. Bir DocumentDB veri kümesi kullandığımızdan, alanlar sihirbaz tarafından otomatik olarak algılanır, dizin alanlar ve veri türü atamalarıyla birlikte önceden yüklenir. 

  ![][3]

Alanlar ve veri türleri yapılandırılmış olsa da öznitelikleri yine de atamanız gerekir. Alan listesinin en üst kısmı boyunca bulunan onay kutuları, bir alanın nasıl kullanıldığını denetleyen *dizin öznitelikleridir*. 

- **Alınabilir**, arama sonuçları listesinde çıktığı anlamına gelir. Örneğin, alanlar yalnızca filtre ifadelerinde kullanıldığında bu onay kutusunun işaretini kaldırarak alanları arama sonuçları için kapsam dışı olarak işaretleyebilirsiniz. 
- **Filtrelenebilir**, **Sıralanabilir** ve **Modellenebilir** seçeneği bir alanın filtre, sıralama veya model gezinti yapısında kullanılıp kullanılamayacağını belirler. 
- **Aranabilir**, bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizeler genellikle aranabilir. Sayısal alanlar ve Boolean alanları genellikle aranamaz olarak işaretlenir. 

Bu sayfadan ayrılmadan önce, aşağıdaki seçenekleri (Alınabilir, Aranabilir, vb.) kullanmak için dizininizdeki alanları işaretleyin. Çoğu alan Alınabilirdir. Çoğu dize alanı Aranabilirdir (Anahtarı aranabilir yapmanıza gerek yoktur). genre, orderableOnline, rating ve tags gibi birkaç alan Filtrelenebilir, Sıralanabilir ve Modellenebilirdir. 
    
Alan | Tür | Seçenekler |
------|------|---------|
id | Edm.String | |
albumTitle | Edm.String | Alınabilir, Aranabilir |
albumUrl | Edm.String | Alınabilir, Aranabilir |
genre | Edm.String | Alınabilir, Aranabilir, Filtrelenebilir, Sıralanabilir, Modellenebilir |
genreDescription | Edm.String | Alınabilir, Aranabilir |
artistName | Edm.String | Alınabilir, Aranabilir |
orderableOnline | Edm.Boolean | Alınabilir, Filtrelenebilir, Sıralanabilir, Modellenebilir |
etiketler | Collection(Edm.String) | Alınabilir, Filtrelenebilir, Modellenebilir |
price | Edm.Double | Alınabilir, Filtrelenebilir, Modellenebilir |
margin | Edm.Int32 | |
rating | Edm.Int32 | Alınabilir, Filtrelenebilir, Sıralanabilir, Modellenebilir |
inventory | Edm.Int32 | Alınabilir |
lastUpdated | Edm.DateTimeOffset | |

Bir karşılaştırma noktası olarak, aşağıdaki ekran görüntüsü önceki tabloda belirtime göre oluşturulan bir dizin çizimidir.

 ![][4]

Sihirbazın bu adımını tamamlamak için **Tamam**'a tıklayın.

#### 3. Adım: Dizin oluşturucuyu tanımlama

**Veri içeri aktarma** sihirbazından çıkmadan, **Dizin Oluşturucu** > **Ad**'a tıklayın, dizin oluşturucu için bir ad yazın ve diğer tüm değerler için varsayılanları kullanın. Bu nesne, yürütülebilir bir işlemi tanımlar. Oluşturulduktan sonra, bunu yinelenen zamanlamaya koyabilirsiniz ancak şimdilik dizin oluşturucuyu **Tamam**'a tıkladığınızda bir kere ve derhal çalışmak üzere varsayılan seçeneği kullanın. 

Veri içeri aktarma girişlerinizin tümünün doldurulmuş ve kullanıma hazır olması gerekir.

  ![][5]

Sihirbazı çalıştırmak için içeri aktarmayı başlatmak ve sihirbazı kapatmak amacıyla **Tamam**'a tıklayın.

## İlerleme durumunu denetleme

İlerleme durumunu denetlemek için hizmet panosuna geri dönün, sayfayı aşağı kaydırın ve dizin oluşturucu listesini açmak için **Dizin Oluşturucular** kutucuğuna çift tıklayın. Listede yeni oluşturduğunuz dizin oluşturucuyu ve Azure Search'te dizine alınan birkaç belgeyle birlikte "devam ediyor" veya başarılı şeklinde gösterilen durumu görmeniz gerekir.

  ![][6]

## Dizini sorgulama

Artık sorgulamaya hazır bir arama dizininiz var. 

**Arama gezgini**, portalda yerleşik bir sorgu aracıdır. Bir arama girişinin, beklediğiniz veriyi döndürebilmesini sağlamak amacıyla bir arama kutusu sağlar. 

1. Komut çubuğunda **Arama gezgini**'ne tıklayın.
2. Hangi dizinin etkin olduğuna dikkat edin. Yeni oluşturduğunuz değilse istediğinizi seçmek için komut çubuğunda **Dizini değiştir**'e tıklayın.
2. Arama kutusunu boş bırakın ve ardından tüm belgeleri döndüren bir joker karakter araması yürütmek için **Ara** düğmesine tıklayın.
3. Birkaç tam metin arama sorgusu girin. Sorgulanacak sanatçılar, albümler ve türler hakkında bilgi edinmek için joker karakter aramanızın sonuçlarını inceleyebilirsiniz.
4. Fikir edinmek için [bu makalenin sonunda sağlanan örnekleri](https://msdn.microsoft.com/library/azure/dn798927.aspx) kullanıp dizininizde bulunma olasılığı olan arama dizelerini kullanmak üzere sorgunuzu değiştirerek diğer sorgu söz dizimlerini deneyin.

## Sonraki adımlar

Sihirbazı bir kez çalıştırdıktan sonra, geri dönüp bileşenleri tek tek görüntüleyebilir veya değiştirebilirsiniz: dizin, dizin oluşturucu veya veri kaynağı. Alanın veri türünü değiştirme gibi bazı düzenlemelere dizinde izin verilmez ancak çoğu özellik ve ayar değiştirilebilir. Bileşenlerin tek tek görüntülemek için panonuzda **Dizin**, **Dizin Oluşturucu** veya **Veri Kaynakları** kutucuğuna tıklayarak var olan nesnelerin bir listesini görüntüleyin.

Bu makalede değinilen diğer özellikler hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:

- [Dizin Oluşturucular](search-indexer-overview.md)
- [Dizin Oluşturma (dizin özniteliklerine yönelik ayrıntılı bir açıklama içerir)](https://msdn.microsoft.com/library/azure/dn798941.aspx)
- [Arama Gezgini](search-explorer.md)
- [Search Belgeleri (sorgu söz dizimi örneklerini içerir)](https://msdn.microsoft.com/library/azure/dn798927.aspx)

Azure sanal makinelerinde Azure SQL Database veya SQL Server gibi diğer veri kaynakları için Veri içeri aktarma sihirbazını kullanarak aynı iş akışını deneyebilirsiniz.

> [AZURE.NOTE] Azure Blob Storage'da gezinmeye yönelik dizin oluşturucu desteği yeni duyurulmuştur ancak bu özellik önizlemededir ve henüz bir portal seçeneği değildir. Bu dizin oluşturucuyu denemek için kod yazmanız gerekir. Daha fazla bilgi için bkz. [Azure Search'te Azure Blob depolama alanı dizini oluşturma](search-howto-indexing-azure-blob-storage.md).
<a id="apdx-sampledata"></a>


## Ek: DocumentDB'de örnek veri oluşturma

Bu bölümde, DocumentDB'de bu öğreticideki görevleri tamamlamak için kullanılabilecek küçük bir veritabanı oluşturulur.

Aşağıdaki yönergeler genel rehberlik sağlar ancak eksiksiz değildir. DocumentDB portalında gezinme veya görevler hakkında daha fazla yardıma ihtiyacınız varsa DocumentDB belgelerine başvurabilirsiniz ancak ihtiyacınız olacak komutların çoğu panonun en üst kısmındaki hizmet komut çubuğunda veya veri tabanı dikey penceresindedir. 

  ![][1]

### Bu öğretici için musicstoredb oluşturma

1. Müzik deposu JSON veri dosyalarını içeren bir ZIP dosyasını indirmek için [buraya tıklayın](https://github.com/HeidiSteen/azure-search-get-started-sample-data). Bu veri kümesi için 246 JSON belgesi sağlıyoruz.
2. DocumentDB'yi aboneliğinize ekleyin ve ardından hizmet panosunu açın.
2. `musicstoredb` kimliğiyle yeni bir veritabanı oluşturmak için **Veritabanı Ekle**'ye tıklayın. Oluşturulduktan sonra sayfanın daha aşağı kısmındaki veritabanı kutucuğunda görünür.
2. Veri tabanı dikey penceresini açmak için veritabanının adına tıklayın.
3. `musicstorecoll` kimliğiyle koleksiyon oluşturmak için **Koleksiyon Ekle**'ye tıklayın.
3. **Belge Gezgini**'ne tıklayın.
4. **Karşıya Yükle**'ye tıklayın.
5. **Belgeyi Karşıya Yükleme** alanında daha önce indirdiğiniz JSON dosyalarını içeren yerel klasöre gidin. JSON dosyalarını en fazla 100 toplu işlem olarak seçin.
    - 386.json
    - 387.json
    - . . .
    - 486.json
6. Sonuncu 669.json dosyasını karşıya yükleyene kadar bir sonraki toplu dosyaları almak için işlemi yineleyin.
7. Belge Gezgini'nin karşıya yükleme gereksinimlerini karşılamak amacıyla verilerin karşıya yüklendiğini doğrulamak için **Sorgu Gezgini**'ne tıklayın.

Bunu yapmanın kolay bir yolu varsayılan sorguyu kullanmaktır ancak varsayılan sorguyu ilk 300'ü (bu veri kümesinde 300'den az öğe bulunur) seçecek şekilde de değiştirebilirsiniz.

386 numaralı belgeyle başlayıp 669 numaralı belgeyle biten JSON çıkışını almanız gerekir. Veriler yüklendikten sonra, **Veri içeri aktarma sihirbazını** kullanarak bir dizin oluşturmak için [bu kılavuzdaki adımlara geri dönebilirsiniz](#defineDS).


<!--Image references-->
[1]: ./media/search-get-started-portal/AzureSearch-GetStart-Docdbmenu1.png
[2]: ./media/search-get-started-portal/AzureSearch-GetStart-DataSource.png
[3]: ./media/search-get-started-portal/AzureSearch-GetStart-DefaultIndex.png
[4]: ./media/search-get-started-portal/AzureSearch-GetStart-FinishedIndex.png
[5]: ./media/search-get-started-portal/AzureSearch-GetStart-ImportReady.png
[6]: ./media/search-get-started-portal/AzureSearch-GetStart-IndexerList.png
[7]: ./media/search-get-started-portal/search-data-import-wiz-btn.png



<!----HONumber=Jun16_HO2-->


