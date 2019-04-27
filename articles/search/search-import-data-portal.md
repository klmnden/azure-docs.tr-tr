---
title: Azure portal - Azure Search kullanarak bir arama dizinine veri aktarmak
description: Azure Vm'lerde bulunan Azure veri Cosmos DB, Blob Depolama, tablo depolama, SQL veritabanı ve SQL Server gezinmek için Azure portalında veri İçeri Aktar Sihirbazı'nı kullanmayı öğrenin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: fcb1e4f32608a1c83b653984dfa066da38e7c451
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871123"
---
# <a name="import-data-wizard-for-azure-search"></a>Azure Search için Veri Alma Sihirbazı

Azure portalı, Azure Search panosunda verileri bir dizine yüklemeye yönelik **Verileri içeri aktarma** sihirbazını içerir. Planda, sihirbaz yapılandırır ve çağıran bir *veri kaynağı*, *dizin*, ve *dizin oluşturucu* -birkaç adımdan dizin oluşturma işlemini otomatik hale getirme: 

* Aynı Azure aboneliğindeki bir dış veri kaynağına bağlanır.
* İsteğe bağlı olarak, optik karakter tanıma ya da doğal dil işleme, metin yapılandırılmamış verileri ayıklamak için tümleştirir.
* Veri örnekleme ve dış veri kaynağının meta verileri temel alan bir dizin oluşturur.
* Veri kaynağı için seri hale getirme ve JSON belgeleri dizine yükleme aranabilir içeriği gezinir.

Sihirbazın önceden tanımlanmış bir dizine bağlanmak veya varolan bir dizin oluşturucuyu çalıştırma, ancak sihirbaz içinde yeni bir dizin veya yapısı ve gereksinim duyduğunuz davranışları desteklemek için dizin oluşturucuyu yapılandırabilirsiniz.

Azure Search'ü ilk kez mi kullanıyorsunuz? Adım adım [hızlı başlangıç: İçeri aktarma, dizin ve portal araçlarını kullanarak sorgu](search-get-started-portal.md) içeri aktarma ve dizin oluşturma kullanarak test sürüşü yapması **verileri içeri aktarma** ve yerleşik realestate örnek veri kümesi.

## <a name="start-importing-data"></a>Verileri içeri aktarmaya başlayın

Bu bölümde, Sihirbazı başlatmak açıklar ve her bir adımın üst düzey bir genel bakış sağlar.

1. İçinde [Azure portalında](https://portal.azure.com), panodan arama hizmeti sayfasını açın veya [hizmetinizi bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Hizmet listesinde.

2. Hizmet genel bakış sayfasında üst tıklatın **verileri içeri aktarma**.

   ![İçeri aktarma Portalı'nda veri komut](./media/search-import-data-portal/import-data-cmd2.png "veri içeri aktarma Sihirbazını Başlat")

   > [!NOTE]
   > Başlatabilirsiniz **verileri içeri aktarma** diğer Azure Hizmetleri, Azure Cosmos DB, Azure SQL veritabanı ve Azure Blob Depolama dahil olmak üzere. Aranacak **Azure Search Ekle** hizmet genel bakış sayfasındaki sol gezinti bölmesinde.

3. Sihirbaz açılır **verilerinize bağlanın**burada bu içeri aktarma için kullanılacak bir dış veri kaynağı seçebilirsiniz. Bu nedenle okuduğunuzdan emin olun, bu adım hakkında bilmeniz gereken birkaç şey vardır [veri kaynağı girişleri](#data-source-inputs) ayrıntılı bilgi için.

   ![Veri Alma Sihirbazı portalında](./media/search-import-data-portal/import-data-wizard-startup.png "Azure Search için Veri Alma Sihirbazı")

4. Sonraki **Ekle bilişsel arama**, optik karakter tanıma (OCR), metin, görüntü dosyaları veya metin analizi yapılandırılmamış veriler üzerinde dahil etmek istediğiniz durumunda. Bilişsel Hizmetler'in sunduğu yapay ZEKA algoritmaları bu görev için alınır. Bu adımda iki bölümü vardır:
  
   İlk olarak, [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md) için bir Azure Search beceri kümesi.
  
   İkinci olarak, hangi AI zenginleştirmelerinin beceri kümesi eklemek için seçin. İzlenecek yol gösterimi için bkz. Bu [hızlı](cognitive-search-quickstart-blob.md).

   Verileri almak istiyorsanız bu adımı atlayıp doğrudan dizin Tanıma Git.

5. Sonraki **hedef dizini Özelleştir**, burada kabul edebilir veya değiştirme Sihirbazı'nda sunulan dizin şeması. Sihirbaz, veri örnekleme ve dış veri kaynağından meta veriler okunurken veri türleri ve alanlarını çıkarır.

   Her bir alan için [dizin özniteliklerini](#index-definition) belirli davranışları etkinleştirmek için. Herhangi bir özniteliği seçmezseniz dizininizi kullanılamaz. 

6. Sonraki **dizin oluşturucu oluşturma**, bu sihirbazın bir ürün olan. Bir dizin oluşturucu, bir Azure dış veri kaynağından aranabilir verileri ve meta verileri ayıklar bir Gezgin olur. Her sihirbazın geçerken veri kaynağını seçerek ve olanakları (isteğe bağlı) ve bir dizin eklemek, bir dizin oluşturucu yapılandırmakta.

   Dizin Oluşturucu bir ad verin ve tıklayın **gönderme** içeri aktarma işlemini başlatmak için. 

Oluşturucuda tıklayarak dizin oluşturma işlemini portalda izleyebilirsiniz **dizin oluşturucular** listesi. Belgeler yüklendikçe, tanımladığınız dizine ait belge sayısı artacaktır. Bazı durumlarda portal sayfasının en son güncelleştirmeleri seçmesi birkaç dakika sürer.

Dizin ilk belgenin yüklendikten hemen sonra sorgulamaya hazırdır. Kullanabileceğiniz [arama Gezgini](search-explorer.md) bu görev için.

<a name="data-source-inputs"></a>

## <a name="data-source-inputs"></a>Veri kaynağı girişleri

**Verileri içeri aktarma** Sihirbazı, bir dış veri kaynağı için bağlantı bilgilerini belirtme kalıcı veri kaynağı nesnesi oluşturur. Veri kaynağı nesnesinin yalnızca ile kullanılan [dizin oluşturucular](search-indexer-overview.md) ve aşağıdaki veri kaynakları için oluşturulabilir: 

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure tablo depolama](search-howto-indexing-azure-tables.md) (için desteklenmeyen [bilişsel arama](cognitive-search-concept-intro.md) işlem hatları)

Düzleştirilmiş veri kümesi gerekli bir giriştir. Yalnızca tek bir tablo, veritabanı görünümü veya eşdeğeri veri yapısından aktarabilirsiniz. 

Sihirbazı çalıştırmadan önce bu veri yapısını oluşturmanız gerekir ve içerik içermesi gerekir. Çalıştırma **verileri içeri aktarma** boş veri kaynağı Sihirbazı.

|  Seçim | Açıklama |
| ---------- | ----------- |
| **Mevcut veri kaynağı** |Arama hizmetinizde önceden tanımlanmış dizin oluşturuculara sahipseniz başka bir içeri aktarma için var olan bir veri kaynağı seçebilirsiniz. Azure Search'te veri kaynağı nesneleri yalnızca dizin oluşturucu tarafından kullanılır. Bir veri kaynağı nesnesi programlı olarak oluşturabilir veya **verileri içeri aktarma** Sihirbazı.|
| **Örnekler**| Azure arama, alma ve sorgu istekleri Azure Search hakkında bilgi edinmek için kullanabileceğiniz ücretsiz bir genel Azure SQL veritabanı barındırır. Bkz: [hızlı başlangıç: İçeri aktarma, dizin ve portal araçlarını kullanarak sorgu](search-get-started-portal.md) kılavuz. |
| **Azure SQL Veritabanı** |Hizmet adı, okuma iznine sahip bir veritabanı kullanıcısının kimlik bilgileri ve veritabanı adı, sayfa üzerinde ya da ADO.NET bağlantı dizesi aracılığıyla belirtilebilir. Özellikleri görüntülemek veya özelleştirmek için bağlantı dizesini seçin. <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir. |
| **Azure VM’lerde SQL Server** |Tam hizmet adı, kullanıcı kimliği ve parola ve veritabanı bağlantı dizesi olarak belirtin. Bu veri kaynağını kullanmak için bağlantıyı şifreleyen yerel depoya daha önce bir sertifika yüklemiş olmanız gerekir. Yönergeler için bkz. [Azure Search ile SQL VM bağlantısı](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir. |
| **Cosmos DB** |Hesap, veritabanı ve bağlantı gereklidir. Koleksiyondaki tüm belgeler dizine dahil edilir. Düzleştirmek veya filtrelemek satır kümesi için sorgu tanımlama veya sorgu boş bırakın. Bir sorgu, bu sihirbazda gerekli değildir.|
| **Azure Blob Depolama** |Depolama hesabı ve bir kapsayıcı gereklidir. İsteğe bağlı olarak, gruplandırma amacıyla blob adlarından önce bir sanal adlandırma kuralı varsa adın sanal dizin kısmını kapsayıcı altındaki bir klasör olarak belirtebilirsiniz. Daha fazla bilgi için bkz. [Blob Depolama Dizini Oluşturma](search-howto-indexing-azure-blob-storage.md). |
| **Azure Tablo Depolama** |Depolama hesabı ve bir tablo adı gereklidir. İsteğe bağlı olarak, tabloların bir alt kümesini almak için sorgu belirtebilirsiniz. Daha fazla bilgi için bkz. [Tablo Depolama Dizini Oluşturma](search-howto-indexing-azure-tables.md). |


<a name="index-definition"></a>

## <a name="index-attributes"></a>Dizin öznitelikleri

**Verileri içeri aktarma** Sihirbazı giriş veri kaynağından alınan belgelerle doldurulmuş bir dizin oluşturur. 

İşlevsel bir dizin için tanımlanan aşağıdaki öğelere sahip olduğunuzdan emin olun.

1. Bir alan olarak işaretlenmelidir bir **anahtarı**, her belgenin benzersiz olarak tanımlanabilmesi için kullanılır. **Anahtarı** olmalıdır *Edm.string*. 

   Alan değerleri boşluk veya tire içeriyorsa ayarlamalısınız **Base-64 kodlama anahtar** seçeneğini **dizin oluşturucu oluşturma** altında adım **Gelişmiş Seçenekler**bastırmak için Bu karakterler doğrulama denetimi.

1. Her alan için dizin özniteliklerini ayarlayın. Özniteliklere seçerseniz, gerekli anahtar alanı dışında temelde boş bir dizindir. En az bir veya daha fazla bu öznitelikler için her bir alan seçin.
   
   + **Alınabilir** arama sonuçlarındaki alanı döndürür. Arama sonuçları içerik sağlayan her alan, bu öznitelik olmalıdır. Bu alan ayarlama, dizin boyutu appreciably etkilemez.
   + **Filtrelenebilir** filtre ifadelerinde başvurulabilmesini sağlar. Kullanılan her alanın bir **$filter** ifadesi, bu öznitelik olmalıdır. Filtre ifadeleri için tam eşleşme var. Metin dizelerinin büyük/küçük harfe zamanlamanızın olduğundan ek depolama alanı verbatim İçeriği sığdırmak için gereklidir.
   + **Aranabilir** tam metin aramayı etkinleştirir. Serbest biçimli sorguları veya sorgu ifadelerinde kullanılan her bir alan, bu öznitelik olmalıdır. Her bir alan olarak işaretlemek için ters dizinleri oluşturulur **aranabilir**.

1. İsteğe bağlı olarak, bu öznitelik, gerektiği gibi ayarlayın:

   + **Sıralanabilir** alanın bir sıralamada kullanılabilmesini sağlar. Kullanılan her alanın bir **$Orderby** ifadesi, bu öznitelik olmalıdır.
   + **Modellenebilir** alanın modellenmiş gezinmesine olanak tanır. Yalnızca alanlar olarak da işaretlenmiş **filtrelenebilir** olarak işaretlenebilir **modellenebilir**.

1. Ayarlanmış bir **Çözümleyicisi** dizin oluşturma ve sorgulama dili Gelişmiş istiyorsanız. Varsayılan değer *standart Lucene* seçebilirsiniz, ancak *Microsoft English* düzensiz isim ve fiili forms giderme gibi gelişmiş sözcük işleme için Microsoft'un Çözümleyicisi kullanmaya yönelik istiyordu.

   + Seçin **aranabilir** etkinleştirmek için **Çözümleyicisi** listesi.
   + Sağlanan listeden bir Çözümleyicisi'ni seçin. 
   
   Şu anda yalnızca dil çözümleyicileri belirtilebilir. Özel bir çözümleyici veya Keyword, Pattern gibi dil dışı bir çözümleyici kullanılması kod gerektirir. Çözümleyicileri hakkında daha fazla bilgi için bkz: [birden çok dilde belgeler için dizin oluşturma](search-language-support.md).

1. Seçin **öneri aracı** yazarken tamamlanan sorgu önerilerini etkinleştirmek için onay kutusunu seçili alanlar.


## <a name="next-steps"></a>Sonraki adımlar
Dizin oluşturucular hakkında daha fazla bilgi için bu bağlantıları gözden geçirin:

* [Azure SQL Veritabanı dizini oluşturma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB’yi dizine ekleme](search-howto-index-cosmosdb.md)
* [Blob Depolama dizini oluşturma](search-howto-indexing-azure-blob-storage.md)
* [Tablo Depolama dizini oluşturma](search-howto-indexing-azure-tables.md)