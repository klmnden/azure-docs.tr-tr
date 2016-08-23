<properties
    pageTitle="Azure Portal'daki dizin oluşturucuları kullanarak Azure Search'e veri aktarma | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Azure Portal'da dizin oluşturucuları kullanma"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="06/08/2016"
    ms.author="heidist"/>

# Portalı kullanarak Azure Search'e veri aktarma

Azure Portal, Azure Search panosunda verileri bir dizine yüklemeye yönelik **Verileri İçeri Aktar** komutunu içerir. Komut, veri kaynağından çekilen satır kümesini temel alan belgeler oluşturarak ve bunları karşıya yükleyerek var olan bir veri kaynağında gezinen yerleşik dizin oluşturucu özelliklerini kullanır.

Sihirbazda verilerin içeri aktarılması 3 parçalı bir yapıdır:

- bir veri kaynağı bağlantısı
- verilerin karşıya yüklendiği bir hedef dizin (sihirbaz bunu genellikle sizin için oluşturur)
- şimdi veya düzenli aralıklarla çalışan bir zamanlama

Bir dizin oluşturucu veya **Veri İçeri Aktar** komutu kullanmak için birincil veri kaynağınızın şu desteklenen veri kaynaklarından biri olması gerekir: Azure SQL Database, Azure VM'deki SQL Server ilişkisel veritabanları veya Azure DocumentDB.

Yalnızca tek bir tablo, görünüm veya eşdeğeri veri yapısından aktarabilirsiniz. Arama dizininize doğru meta verileri ve veri girişlerini almak için öncelikle bu veri yapısını uygulamanızın veri kaynağında oluşturmanız gerekebilir.

Örnek verileri kullanarak bu iş akışını deneyebilirsiniz. Azure Search ile çalışmaya başlamak için [Azure Portal'da Azure Search ile çalışmaya başlama](search-get-started-portal.md) başlıklı sayfayı ziyaret edin.

##Verileri içeri aktarmayı yapılandırma

1. [Azure Portal](https://portal.azure.com)'da oturum açın.

2. Azure Search hizmetinizin hizmet panosunu açın. Panoyu bulmanın birkaç yolu burada verilmiştir.
    - Harf çubuğunda **Giriş**'e tıklayın. Giriş sayfasında, aboneliğinizin tüm hizmetleri için kutucuklar bulunur. Hizmet panosunu açmak için kutucuğa tıklayın.
    - Harf çubuğunda, listedeki Arama hizmetinizi bulmak için **Tümüne Gözat** > **Filtreleme ölçütü:** > **Arama hizmetleri**'ne tıklayın.

3. Hizmet panosunda en üst kısımda **Veri İçeri Aktar** dahil olmak üzere bir komut çubuğu görürsünüz. Veri İçeri Aktar dikey penceresini kaydırarak açmak için **Veri İçeri Aktar**'a tıklayın.

4. Bir dizin oluşturucu tarafından kullanılan bir veri kaynağı tanımını belirtmek için **Verilerinize bağlanın**'a tıklayın. Seçeneklere şunlar dahildir:
    -   Var olan veri kaynakları, önceden bir dizin oluşturucu için oluşturulan bir veri kaynağı tanımına başvurur. Arama hizmetinizde önceden tanımlanmış dizin oluşturuculara sahipseniz başka bir içeri aktarma için bir veri kaynağı tanımını yeniden amaçlandırabilirsiniz.
    -   Azure SQL, Azure'daki bir SQL veritabanına veya Azure VM'deki bir SQL Server veritabanına bir veri kaynağı bağlantısını belirtmek için kullanılır.
    -   DocumentDB, bu veri kaynağı türüne yönelik veri kaynağı bağlantısı belirtmek için kullanılır.

   Veritabanı, hem Azure SQL hem de DocumentDB için aboneliğinizde var olmalıdır. Biliyorsanız bir bağlantı dizesini yapıştırabilir veya aboneliğiniz için yazma ayrıcalıklarına sahip biri tarafından önceden oluşturulmuş bir veri kaynağını seçebilirsiniz.

5. Varsayılan dizini tamamlamak için **Hedef dizini özelleştir**'e tıklayın.
    - **Anahtar** gereklidir. Anahtar için seçtiğiniz alan, benzersiz değerler içeren bir dize alanı olmalıdır.
    - Alan adı ve türü genellikle sizin için doldurulur. Veri türünü değiştirebilirsiniz.
    - Her bir alan için öznitelikler seçin:
        - Alınabilir, arama sonuçlarındaki alanı döndürür.
        - Filtrelenebilir, alana filtre ifadelerinde başvurulabilmesini sağlar.
        - Sıralanabilir, alanın bir sıralamada kullanılabilmesini sağlar.
        - Modellenebilir, alanın modellenmiş gezinmesine olanak sağlar.
        - Aranabilir, tam metin aramayı etkinleştirir.
    - Alan düzeyinde bir dil çözümleyicisi belirtmek istiyorsanız **Çözümleyici** sekmesine tıklayın. Ayrıntılı bilgi için bkz. [Birden çok dilde belgeler için dizin oluşturma](search-language-support.md).
    - Otomatik tamamlamayı veya yazarken tamamlanan sorgu önerilerini etkinleştirmek istiyorsanız **Öneri Aracı**'na tıklayın.

6. Şimdi Çalıştır seçeneğini kullanarak içeri aktarma işlemini yürütmek veya yinelenen bir zamanlama ayarlamak için **Verilerinizi içeri aktarın**'a tıklayın.

Şimdi tamamladığınız verileri içeri aktarma işlemi, arka planda bir dizin oluşturucu oluşturdu. Artık bileşen parçalarından herhangi birini değiştirmek için dizin oluşturucuyu doğrudan düzenleyebilirsiniz.

##Var olan bir dizin oluşturucuyu düzenleme

Hizmet panosunda, aboneliğiniz için oluşturulan tüm dizin oluşturucuların listesini kaydırarak açmak için Dizin Oluşturucu kutucuğuna çift tıklayın. Bir dizin oluşturucuyu çalıştırmak, düzenlemek veya silmek için bunlardan birine çift tıklayın. Dizini var olan başka bir dizinle değiştirebilir, veri kaynağını değiştirebilir ve dizin oluşturma sırasında hata eşikleri için seçenekleri ayarlayabilirsiniz.

##Var olan bir dizini düzenleme

Azure Search hizmetinde bir dizinde yapısal güncelleştirmeler yapılması bu dizinin yeniden derlenmesini gerektirir; bu işlem dizinin silinmesini, yeniden oluşturulmasını ve verilerin yeniden yüklenmesini içerir. Yapısal güncelleştirmeler bir veri türünün değiştirilmesini ve bir alanın yeniden adlandırılmasını ya da silinmesini içerir.

Yeni bir alan ekleme, puanlama profillerini değiştirme, öneri araçlarını değiştirme veya dil çözümleyicileri değiştirme gibi işlemler yeniden derleme gerektirmeyen düzenlemelerdir. Daha fazla bilgi için bkz. [Dizin Güncelleştirme](https://msdn.microsoft.com/library/azure/dn800964.aspx).



<!--HONumber=Aug16_HO1-->


