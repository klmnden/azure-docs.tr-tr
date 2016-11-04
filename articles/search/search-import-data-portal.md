---
title: Azure Portal'daki dizin oluşturucuları kullanarak Azure Search'e veri aktarma | Microsoft Docs
description: Azure VM’lerde bulunan Azure Blob depolama, tablo depolama SQL Veritabanı ve SQL Server verilerinde gezinmek için Azure Portal’daki Azure Search Veri İçeri Aktarma Sihirbazı’nı kullanın.
services: search
documentationcenter: ''
author: HeidiSteen
manager: jhubbard
editor: ''
tags: Azure Portal

ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist

---
# Portalı kullanarak Azure Search'e veri aktarma
Azure portalı, Azure Search panosunda verileri bir dizine yüklemeye yönelik **İçeri Aktarma Verileri** sihirbazını içerir. 

  ![Komut çubuğunda İçeri Aktarma Verileri][1]

Sihirbaz kendi içinde bir *dizin oluşturucu* yapılandırıp çağırır ve dizin oluşturma işleminin birkaç adımını otomatik hale getirir: 

* Geçerli Azure aboneliğindeki bir dış veri kaynağına bağlanma
* Kaynak veri yapısına dayalı bir dizin şemasını otomatik olarak oluşturma
* Veri kaynağından alınan bir satır kümesine dayalı belgeler oluşturma
* Belgeleri arama hizmetinizdeki dizine yükleme

DocumentDB’deki örnek verileri kullanarak bu iş akışını deneyebilirsiniz. Yönergeler için [Azure Portal'da Azure Search ile çalışmaya başlama](search-get-started-portal.md) başlıklı sayfayı ziyaret edin.

## Veri Alma Sihirbazı tarafından desteklenen veri kaynakları
Veri Alma Sihirbazı aşağıdaki veri kaynaklarını destekler: 

* Azure SQL Database
* Azure VM’lerdeki SQL Server ilişkisel verileri
* Azure DocumentDB
* Azure Blob depolama (önizlemede)
* Azure Tablo depolama (önizlemede)

Düzleştirilmiş veri kümesi gerekli bir giriştir. Yalnızca tek bir tablo, veritabanı görünümü veya eşdeğeri veri yapısından aktarabilirsiniz. Sihirbazı çalıştırmadan önce bu veri yapısını oluşturmanız gerekir.

Dizin oluşturuculardan birkaç tanesi hala önizleme aşamasındadır; diğer bir deyişle dizin oluşturucu tanımı API’nin önizleme sürümü ile desteklenmektedir. Daha fazla bilgi ve bağlantılar için bkz. [Dizin oluşturucuya genel bakış](search-indexer-overview.md).

## Verilerinize bağlanma
1. [Azure Portal](https://portal.azure.com)’da oturum açın ve hizmet panosunu açın. Geçerli abonelikte var olan hizmetleri göstermek için atlama çubuğundaki **Hizmetleri ara** öğesine tıklayabilirsiniz. 
2. Veri İçeri Aktar dikey penceresini kaydırarak açmak için komut çubuğundaki **Veri İçeri Aktar**'a tıklayın.  
3. Bir dizin oluşturucu tarafından kullanılan bir veri kaynağı tanımını belirtmek için **Verilerinize bağlanın**'a tıklayın. Abonelik içi veri kaynakları için sihirbaz genellikle bağlantı bilgilerini algılayıp okuyabilir ve genel yapılandırma gereksinimlerini en aza indirebilir.

|  |  |
| --- | --- |
| **Var olan veri kaynağı** |Arama hizmetinizde önceden tanımlanmış dizin oluşturuculara sahipseniz başka bir içeri aktarma için var olan bir veri kaynağı seçebilirsiniz. |
| **Azure SQL Database** |Hizmet adı, okuma iznine sahip bir veritabanı kullanıcısının kimlik bilgileri ve veritabanı adı, sayfa üzerinde ya da ADO.NET bağlantı dizesi aracılığıyla belirtilebilir. Özellikleri görüntülemek veya özelleştirmek için bağlantı dizesini seçin. <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir. |
| **Azure VM’lerde SQL Server** |Bağlantı dizesi olarak tam hizmet adı, kullanıcı kimliği ve parola ile veritabanı belirtin. Bu veri kaynağını kullanmak için bağlantıyı şifreleyen yerel depoya daha önce bir sertifika yüklemiş olmanız gerekir. <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir. |
| **DocumentDB** |Hesap, veritabanı ve bağlantı gereklidir. Koleksiyondaki tüm belgeler dizine dahil edilir. Satır kümesini düzleştirmek veya filtrelemek ya da sonraki veri yenileme işlemleri için değiştirilen belgeleri algılamak üzere bir sorgu tanımlayabilirsiniz. |
| **Azure Blob Depolama** |Depolama hesabı ve bir kapsayıcı gereklidir. İsteğe bağlı olarak, gruplandırma amacıyla blob adlarından önce bir sanal adlandırma kuralı varsa adın sanal dizin kısmını kapsayıcı altındaki bir klasör olarak belirtebilirsiniz. Daha fazla bilgi için bkz. [Blob Depolama Dizini Oluşturma (önizleme)](search-howto-indexing-azure-blob-storage.md). |
| **Azure Table Storage** |Depolama hesabı ve bir tablo adı gereklidir. İsteğe bağlı olarak, tabloların bir alt kümesini almak için sorgu belirtebilirsiniz. Daha fazla bilgi için bkz. [Tablo Depolama Dizini Oluşturma (önizleme)](search-howto-indexing-azure-tables.md). |

## Hedef dizini özelleştirme
Başlangıç dizini genellikle veri kümesinden çıkarılır. Şemayı tamamlamak için alan ekleyin, düzenleyin veya silin. Ayrıca, sonraki arama davranışlarını belirlemek üzere alan düzeyinde öznitelikler ayarlayın.

1. **Hedef dizini özelleştir** menüsünde her bir belgeyi benzersiz bir şekilde tanımlamak için kullanılan adı ve **Anahtarı** belirtin. Anahtar bir dize olmalıdır. Alan değerleri boşluk veya tire içeriyorsa **Verilerinizi içeri aktarın** menüsündeki gelişmiş seçenekleri, bu karakterler üzerinde doğrulama denetimini gizleyecek şekilde ayarlayın.
2. Kalan alanları gözden geçirin ve düzeltin. Alan adı ve türü genellikle sizin için doldurulur. Veri türünü değiştirebilirsiniz.
3. Her bir alan için dizin özniteliklerini ayarlayın:
   
   * Alınabilir, arama sonuçlarındaki alanı döndürür.
   * Filtrelenebilir, alana filtre ifadelerinde başvurulabilmesini sağlar.
   * Sıralanabilir, alanın bir sıralamada kullanılabilmesini sağlar.
   * Modellenebilir, alanın modellenmiş gezinmesine olanak sağlar.
   * Aranabilir, tam metin aramayı etkinleştirir.
4. Alan düzeyinde bir dil çözümleyicisi belirtmek istiyorsanız **Çözümleyici** sekmesine tıklayın. Şu anda yalnızca dil çözümleyicileri belirtilebilir. Özel bir çözümleyici veya Keyword, Pattern gibi dil dışı bir çözümleyici kullanılması kod gerektirir.
   
   * Alan üzerinde tam metin araması belirlemek ve Çözümleyici açılır listesini etkinleştirmek için **Aranabilir** öğesine tıklayın.
   * İstediğiniz çözümleyiciyi seçin. Ayrıntılı bilgi için bkz. [Birden çok dilde belgeler için dizin oluşturma](search-language-support.md).
5. Seçili alanlarda yazarken tamamlanan sorgu önerilerini etkinleştirmek için **Öneri Aracı**’na tıklayın.

## Verilerinizi içeri aktarma
1. **Verilerinizi içeri aktarın** menüsünde dizin oluşturucu için bir ad belirtin. Dizin oluşturucunun, Verileri İçeri Aktarma sihirbazının bir ürünü olduğunu unutmayın. Daha sonra görüntülemek veya düzenlemek isterseniz sihirbazı yeniden çalıştırmak yerine portaldan seçebilirsiniz. 
2. Hizmetin sağlandığı bölgesel saat dilimine dayalı zamanlamayı belirtin.
3. Bir belge bırakılırsa dizin oluşturmanın devam edip etmeyeceğini belirlemeye yönelik eşikleri belirtmek için gelişmiş seçenekleri ayarlayın. Ek olarak, **Anahtar** alanlarının boşluk ve eğik çizgi içermesine izin verilip verilmeyeceğini belirtebilirsiniz.  

## Var olan bir dizin oluşturucuyu düzenleme
Hizmet panosunda, aboneliğiniz için oluşturulan tüm dizin oluşturucuların listesini kaydırarak açmak için Dizin Oluşturucu kutucuğuna çift tıklayın. Bir dizin oluşturucuyu çalıştırmak, düzenlemek veya silmek için bunlardan birine çift tıklayın. Dizini var olan başka bir dizinle değiştirebilir, veri kaynağını değiştirebilir ve dizin oluşturma sırasında hata eşikleri için seçenekleri ayarlayabilirsiniz.

## Var olan bir dizini düzenleme
Azure Search hizmetinde bir dizinde yapısal güncelleştirmeler yapılması bu dizinin yeniden derlenmesini gerektirir; bu işlem dizinin silinmesini, yeniden oluşturulmasını ve verilerin yeniden yüklenmesini içerir. Yapısal güncelleştirmeler bir veri türünün değiştirilmesini ve bir alanın yeniden adlandırılmasını ya da silinmesini içerir.

Yeni bir alan ekleme, puanlama profillerini değiştirme, öneri araçlarını değiştirme veya dil çözümleyicileri değiştirme gibi işlemler yeniden derleme gerektirmeyen düzenlemelerdir. Daha fazla bilgi için bkz. [Dizin Güncelleştirme](https://msdn.microsoft.com/library/azure/dn800964.aspx).

## Sonraki adım
Dizin oluşturucular hakkında daha fazla bilgi için bu bağlantıları gözden geçirin:

* [Azure SQL Veritabanı dizini oluşturma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
* [DocumentDB dizini oluşturma](../documentdb/documentdb-search-indexer.md)
* [Blob Depolama (önizleme) dizini oluşturma](search-howto-indexing-azure-blob-storage.md)
* [Tablo Depolama (önizleme) dizini oluşturma](search-howto-indexing-azure-tables.md)

<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png




<!--HONumber=Sep16_HO4-->


