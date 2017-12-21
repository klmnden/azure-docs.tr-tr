---
title: "Bulmak, tanımlamak ve Microsoft Azure kişisel verileri sınıflandırmak | Microsoft Docs"
description: "Arama, Sınıflandırma, bulma ve verileri tanımlama hakkında bilgi edinin"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 0d99df534da4575f3c34ec6b3475cdd1bdc3308a
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="discover-identify-and-classify-personal-data-in-microsoft-azure"></a>Bulmak, tanımlamak ve Microsoft Azure kişisel verileri sınıflandırmak

Bu makalede bulmak, tanımlamak ve birkaç Azure Araçları ve Hizmetleri Azure Hdınsight, Azure Information Protection, Azure Search ve SQL sorguları Hadoop kümeleri için Azure veri Kataloğu, Azure Active Directory, SQL veritabanı, Power Query Azure Cosmos DB kullanımı dahil olmak üzere, kişisel verileri sınıflandırmak hakkında yönergeler sağlar.

## <a name="scenario-problem-statement-and-goal"></a>Senaryo, sorun bildirimi ve amacı

ABD tabanlı Spor şirket çok çeşitli kişisel ve diğer verileri toplar. Bu, müşterilerin ve çalışan verilerini içerir. Şirket içinde birden çok veritabanı tutar ve Azure ortamlarında birkaç farklı konumlarda depolar. Spor ekipman satılması yanı sıra da barındırmak ve kayıt AB dahil olmak üzere dünyanın taşıyan seçkin athletic olayları yönetmek ve bazı durumlarda bunlar toplamak müşteri verilerini sağlık bilgileri içerir.

Şirket her yıl birçok uluslararası bicycling tur barındırır ve bağlı personel dünya konumlarda sahip olduğundan, birkaç veri kümeleri oldukça büyük. Şirket, müşteriler ve çalışanlar tarafından kullanılan Geliştirici oluşturulan uygulamalar da sahiptir.

Şirket aşağıdaki sorunlara yanıt vermek istemektedir:

- Müşteri ve çalışanın kişisel verilerini uygun erişim ve güvenliği sağlamak için şirket toplar diğer verilerinden sınıflandırılmış/ayırt olması gerekir.
- Müşteri kişisel verilerin konumu Azure ortamı çeşitli alanlar genelindeki kolayca bulmak veri yönetici gerekir.
- Görünür müşteri ve çalışanın kişisel verilerini paylaşılan belgeler ve e-posta iletişimini güvenli tutulur sağlamaya yardımcı olmak için etiketlenmelidir.
- Şirket uygulama geliştiricilerin kendi web ve mobil uygulamaları müşteri ve çalışan kişisel verileri kolayca aramak için bir yol gerekir.
- Geliştiriciler Ayrıca kendi kişisel verileriniz için belge veritabanını sorgulamak gerekir.

### <a name="company-goals"></a>Şirketin hedeflerine

- Kolayca bulunabilir böylece tüm müşteri ve çalışanın kişisel verilerini Azure veri Kataloğu'nda etiketli/açıklama olması gerekir. İdeal olarak müşteri ve çalışanın kişisel verilerini ayrı olarak etiketlenmiş/açıklama.
- Müşteri ve çalışan kullanıcı profilleri ve iş bilgilerini Azure Active Directory'de bulunan kişisel verileri kolayca yer alması gerekir.
- Birden çok SQL veritabanlarında bulunan kişisel verileri kolayca seçmeleri gerekir. 
- Şirketin büyük veri kümeleri bazıları Azure Hdınsight yönetilir ve Hadoop depolanan. Kişisel veriler için sorgulanabilir şekilde Excel'e aktarılmalıdır.
- Belgeleri ve e-posta iletişimini paylaşılan kişisel veriler sınıflandırılmış etiketli ve Azure Information Protection ile güvenli tutulur.
- Şirket uygulama geliştiriciler oluşturuncaya, uygulamaları Azure Search ile yapabileceklerini müşteri ve çalışan kişisel verilerinizi keşfedin kurabilmesi gerekir.
- Geliştiriciler kendi belge veritabanında kişisel verileri bulmak kurabilmesi gerekir.

## <a name="azure-active-directory-data-discovery"></a>Azure Active Directory: Veri bulma

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft'un bulut tabanlı, çok kiracılı dizin ve kimlik yönetimi hizmetidir. Müşteri ve çalışan kullanıcı profilleri ve kişisel veriler içeren kullanıcı iş bilgilerini bulun, [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) ortamı kullanarak [Azure portal](https://portal.azure.com/).

Bulma veya belirli bir kullanıcı için kişisel verileri değiştirmek istiyorsanız, bu özellikle yararlıdır. Ayrıca ekleyin ya da kullanıcı profilini değiştirmek ve iş. Dizin için genel yönetici olan bir hesapla oturum gerekir.

### <a name="how-do-i-locate-or-view-user-profile-and-work-information"></a>Nasıl bulun veya kullanıcı profilini görüntülemek ve iş bilgileri?

1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.

2. Seçin **daha fazla hizmet**, girin **kullanıcılar ve gruplar** metin kutusuna ve ardından **Enter**.

   ![nasıl kullanıcı profilini bulun ve iş](media/how-to-discover-classify-personal-data-azure/user-profile.png)

3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **kullanıcılar**.

  ![Açılış kullanıcı ve Grup](media/how-to-discover-classify-personal-data-azure/users-groups.png)

4. Üzerinde **kullanıcılar ve gruplar - kullanıcıların** dikey penceresinde, listeden bir kullanıcı seçin ve ardından dikey penceresinde seçili kullanıcı, seçin **profil** kişisel verileri içerebilir kullanıcı profili bilgilerini görüntülemek için.

  ![Kullanıcı Seç](media/how-to-discover-classify-personal-data-azure/select-user.png)

5. Kullanıcı profili bilgileri ekleme veya değiştirme gerekiyorsa, bunu yapmak ve ardından komut çubuğunda seçin **kaydedin.**
6. Seçilen kullanıcı için dikey seçin **iş bilgisi** kişisel verileri içerebilir kullanıcı iş bilgilerini görüntülemek için.

 ![iş bilgileri görüntüleme](media/how-to-discover-classify-personal-data-azure/work-info.png)

7. Kullanıcı iş bilgileri ekleme veya değiştirme gerekiyorsa, bunu yapmak ve ardından komut çubuğunda seçin **kaydedin.**

## <a name="azure-sql-database-data-discovery"></a>Azure SQL Database: Veri bulma

[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/?v=16.50) yapı ve uygulamalarını geliştiriciler yardımcı olan bir bulut veritabanıdır. Kişisel veriler bulunabilir [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/?v=16.50) standart SQL sorgularını kullanarak. Azure SQL esnek sorgu (Önizleme) veritabanları arası sorguları gerçekleştirmelerini sağlar.

Bir ayrıntılı [SQL veritabanı](../sql-database/sql-database-technical-overview.md) öğretici bir oluşturma ve veri sorguları gerçekleştirme dahil olmak üzere bir SQL veritabanı kullanarak birçok yönleri açıklanmaktadır. Öğreticide belirli bölümlerine bağlantılar ile birlikte kullanılabilir bilgilerinin özeti verilmiştir.

### <a name="how-do-i-build-a-sql-database"></a>Bir SQL veritabanı nasıl oluşturulacağına?

Bunu yapmanın üç yolu vardır:

- Bir Azure SQL veritabanı oluşturulabilir [Azure portal](https://portal.azure.com/). Öğreticide, işlem ve depolama kaynakları bir kaynak grubu ve mantıksal sunucu içinde belirli bir dizi kullanacaksınız. AdventureWorks adlı kurgusal bir şirkette örnek verileri kullanacaksınız. Ayrıca, bir sunucu düzeyinde güvenlik duvarı kuralı oluşturacaksınız. Bunun nasıl yapılacağını öğrenmek için ziyaret edin [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) Öğreticisi.

  ![Azure SQL veritabanı oluşturma](media/how-to-discover-classify-personal-data-azure/create-database.png)
- Bir SQL veritabanı da oluşturulabilir [Azure bulut Kabuk](https://azure.microsoft.com/features/cloud-shell/) CLI, tarayıcı tabanlı bir komut satırı aracıdır. Araç, Azure portalında kullanılabilir ve doğrudan buradan çalıştırabilirsiniz. Bu öğreticide Aracı'nı başlatın, komut dosyası değişkenleri tanımlayın, bir kaynak grubu ve mantıksal sunucu oluşturmak ve bir sunucu güvenlik duvarı kuralı yapılandırın. Ardından örnek verilerle bir veritabanı oluşturun. Bu şekilde, veritabanınızı oluşturmayı öğrenmek için ziyaret edin [Azure CLI kullanarak tek bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-cli.md) Öğreticisi.

  ![CLIT Öğreticisi](media/how-to-discover-classify-personal-data-azure/cli-tutorial.png)

>[!NOTE]
Azure CLI Linux Yöneticiler ve geliştiriciler tarafından yaygın olarak kullanılır. Bazı kullanıcılar, daha kolay ve PowerShell, daha sezgisel, üçüncü seçenek olan bulun.

- Son olarak, Azure ve diğer kaynakları oluşturmak ve yönetmek için kullanılan bir komut satırı/komut dosyası aracı PowerShell kullanarak bir SQL veritabanı oluşturabilirsiniz. Bu öğreticide Aracı'nı başlatın, komut dosyası değişkenleri tanımlayın, bir kaynak grubu ve mantıksal sunucu oluşturmak ve bir sunucu güvenlik duvarı kuralı yapılandırın. Ardından örnek verilerle bir veritabanı oluşturacaksınız.

Öğretici, Azure PowerShell modülü sürüm 4.0 veya üstünü gerektirir. Get-Module - listavailable birlikte sürümünüzü bulmak için AzureRM çalıştırın. Yüklemek veya yükseltmek gereksinim duyarsanız, yükleme Azure PowerShell modülü bakın.

```PowerShell
New-AzureRmSQLDatabase -ResourceGroupName $resourcegroupname `
-ServerName $servername `
-DatabaseName $databasename `
-RequestedServiceObjectiveName "s0"
```

Bu şekilde, veritabanınızı oluşturmayı öğrenmek için ziyaret edin [Powershell kullanarak tek bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-powershell.md) Öğreticisi.

>[!Note]
PowerShell kullanmak için Windows yöneticileri eğilimi gösterir, ancak bazı Azure CLI tercih.

### <a name="how-do-i-search-for-personal-data-in-sql-database-in-the-azure-portal"></a>Ne arama Azure portalında SQL veritabanında kişisel veriler için? **

Kişisel verileri için arama için Azure portal içinde yerleşik sorgu Düzenleyicisi aracını kullanabilirsiniz. SQL server yönetici oturum açma ve parola kullanarak aracı için oturum açın ve bir sorgu girin.

  ![portal kullanarak sql arama](media/how-to-discover-classify-personal-data-azure/search-sql-portal.png)

Adım 5 öğreticinin örnek bir sorgu sorgu Düzenleyicisi'ni bölmesinde gösterir, ancak (Ayrıca iki tablolardaki verileri birleştiren ve döndürülen veri kümesinde kaynak sütunu için diğer adlar oluşturur) kişisel ya da hassas bilgi odaklanmak değil. Aşağıdaki ekran görüntüsü, döndürülen sonuçlar bölmesinde yanı sıra adım 5 ' sorguyu gösterir:

  ![sorgu düzenleyicisi](media/how-to-discover-classify-personal-data-azure/query-editor.png)

Veritabanınızı MyTable çağırdıysanız, kişisel bilgi için bir örnek sorgu adı, sosyal güvenlik numarası ve kimlik numarasını içerebilir ve şuna benzer:

"Adı, SSN, kimlik numarası gelen MyTable Seç"

Sorguyu çalıştırmak ve sonuçları görmek **sonuçları** bölmesi.

Azure portalında bir SQL veritabanını sorgulamasını konusunda daha fazla bilgi için ziyaret [SQL veritabanını sorgulamasını](../sql-database/sql-database-get-started-portal.md) öğreticinin bölümünde.

### <a name="how-do-i-search-for-data-across-multiple-databases"></a>Birden çok veritabanı arasında nasıl verileri için arama?

SQL esnek sorgu (Önizleme) veritabanları arası ve birden çok veritabanı sorguları gerçekleştirmek ve tek bir sonuç sağlar. [Öğreticiye genel bakış](../sql-database/sql-database-elastic-query-overview.md) senaryoları için ayrıntılı bir açıklama içerir ve veritabanı dikey ve yatay bölümleme arasındaki farkı açıklar. Yatay bölümleme "parçalama." olarak adlandırılır

  ![Dikey bölümleme](media/how-to-discover-classify-personal-data-azure/vertical-partition.png)

  ![Yatay bölümleme](media/how-to-discover-classify-personal-data-azure/horizontal.png)

Başlamak için ziyaret [Azure SQL Database esnek sorgu genel bakış (Önizleme)](../sql-database/sql-database-elastic-query-overview.md) sayfası.

#### <a name="power-query-for-importing-azure-hdinsight-hadoop-clusters-data-discovery-for-large-data-sets"></a>Power Query (Azure Hdınsight Hadoop kümeleri almak için): büyük veri kümeleri için veri bulma

Hadoop, Apache depolama ve işleme hizmeti için Hadoop kümeleri içinde depolanan ve analiz büyük veri kümeleri, açık bir kaynaktır. [Azure Hdınsight](https://azure.microsoft.com/services/hdinsight/) kullanıcının Azure Hadoop kümeleri ile çalışmasına olanak verir. Power Query bir Excel eklemek, başka şeylerin Bul farklı kaynaklardan veri kullanıcılarına yardımcı olur bileşenidir.

Azure hdınsight'ta Hadoop kümelerinin ilişkilendirilmiş kişisel verileri Excel'e Power Query ile içeri aktarılabilir. Excel'de veri olduktan sonra bir sorgu tanımlamak için kullanabilirsiniz.

#### <a name="how-do-i-use-excel-power-query-to-import-hadoop-clusters-in-azure-hdinsight-into-excel"></a>Azure hdınsight'ta Hadoop kümeleri Excel'e aktarmak için Excel'in Power Query nasıl kullanırım?

Bir Hdınsight Öğreticisi tüm bu işlemde size yol gösterir. Önkoşulları açıklar ve bir bağlantı içeren bir [Azure Hdınsight kullanmaya başlama](../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md) Öğreticisi. Yönergeleri kapsar Excel 2016 yanı sıra 2013 ve 2010 (adımları Excel daha eski sürümleri için biraz farklı). Öğretici Excel Power Query eklentisini yoksa, nasıl gösterir. Öğretici Excel'de başlayacaksınız ve kümenizle ilişkilendirilmiş Azure Blob Depolama hesabınız olması gerekir.

  ![Excel sorgusu](media/how-to-discover-classify-personal-data-azure/excel.png)

Bunun nasıl yapılacağını öğrenmek için ziyaret edin [bağlanmak Excel için Power Query kullanarak Hadoop](../hdinsight/hadoop/apache-hadoop-connect-excel-power-query.md) Öğreticisi.

Kaynak: [Bağlan Excel için Power Query kullanarak Hadoop](../hdinsight/hadoop/apache-hadoop-connect-excel-power-query.md)

## <a name="azure-information-protection-personal-data-classification-for-documents-and-email"></a>Azure Information Protection: belgeler ve e-posta kişisel veri sınıflandırması

[Azure Information Protection](https://www.microsoft.com/cloud-platform/azure-information-protection) Yardım Azure müşterilerine sınıflandırmak ve dahili olarak veya harici olarak paylaşılan belgeleri korumak ve iletişim e-posta etiketleri uygulayabilirsiniz. Bu öğelerin bazıları, müşteri veya çalışanın kişisel bilgiler içerebilir. Kural ve koşulları Yöneticiler veya kullanıcılar tarafından otomatik olarak veya el ile tanımlanabilir. Örneğin, bir kullanıcı, kredi kartı bilgilerini içeren bir belge kaydetme, isterse yönetici tarafından yapılandırılan etiket öneride görür.

### <a name="how-do-i-try-it"></a>Bunu nasıl deneyin?

Azure Information Protection, kuruluşunuz için uygun olabilir, görmek için bir try vermek istiyorsanız, ziyaret [hızlı başlangıç Öğreticisi](https://docs.microsoft.com/information-protection/get-started/infoprotect-quick-start-tutorial). Beş temel adımlarda size yol gösterir — görmesini Sınıflandırma, etiketleme ve eylemde paylaşımı ilkesi yapılandırma yüklemesinden — ve az yarım saat sürer.

### <a name="how-do-i-deploy-it"></a>Nasıl dağıtırım?

Kuruluşunuz için Azure Information Protection dağıtmak istiyorsanız, ziyaret [Sınıflandırma, etiketleme ve koruma için dağıtım yol haritasını](https://docs.microsoft.com/information-protection/plan-design/deployment-roadmap).

### <a name="is-there-anything-else-i-should-know"></a>Bilmeniz gereken başka bir şey var mı?

Nasıl kurulacağını düşündüğünüz yardımcı olacak tamamlayıcı için bilgi [hazır, ayarlayın, koruma!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
blog postası. Ve daha fazla bağlantılar Azure Information Protection hakkında daha fazla bilgi için aşağıda öğrenin denetleyin.

## <a name="azure-search-data-discovery-for-developer-apps"></a>Azure arama: Geliştirici uygulamaları veri bulma

[Azure arama](https://azure.microsoft.com/services/search/) uygulamalarınız için zengin veri arama deneyimi sağlar ve geliştiriciler için bulut arama çözümüdür. Azure arama Azure Cosmo DB, Azure SQL Database, Azure Blob Storage, Azure Table depolama veya özel müşteri JSON verilerini kaynaklanan, kullanıcı tanımlı dizinler arasında veri bulmanızı sağlar. Ayrıca, kişisel veri türleri veya belirli kişilerin kişisel veri aramak için Azure Search REST API'sini kullanarak Lucene sorgu yapısı. Tam metin araması, Basit Sorgu söz dizimi ve Lucene sorgu söz dizimi özellikleri içerir. 

## <a name="how-do-i-use-sql-to-query-data"></a>SQL sorgu verileri nasıl kullanabilirim?

Başından itibaren temelleri ziyaret [Azure CosmosD DB: SQL kullanarak nasıl](../cosmos-db/tutorial-query-documentdb.md) Öğreticisi. Öğretici, örnek bir belge ve iki örnek SQL sorguları ve sonuçları sağlar.

SQL sorguları oluşturma konusunda daha fazla ayrıntılı yönergeler için ziyaret [SQL sorgularını Azure Cosmos DB belge DB API için.](../cosmos-db/sql-api-sql-query.md)

Azure Cosmos DB yeni ve bir veritabanı oluşturmayı öğrenmek istiyorsanız, bir koleksiyonu ekleyin ve veri ekleme, ziyaret [Azure Cosmos DB: SQL API web uygulaması oluşturma](../cosmos-db/create-sql-api-dotnet.md) hızlı başlangıç Öğreticisi. Siteye aldıktan sonra tercih ettiğiniz dili yalnızca .NET, Java veya Python gibi dışındaki bir dilde bunu istiyorsanız seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/?v=16.50)

[SQL Veritabanı nedir?](../sql-database/sql-database-technical-overview.md)

[SQL veritabanı sorgu düzenleyici Azure portalında kullanılabilir] (https://azure.microsoft.com/blog/t-sql-query-editor-in-browser-azure-portal/)

[Azure Information Protection nedir?](https://docs.microsoft.com/information-protection/understand-explore/what-is-information-protection)

[Azure Rights Management nedir?](https://docs.microsoft.com/information-protection/understand-explore/what-is-azure-rms)

[Azure Information Protection: Hazır, ayarlayın, koruyun!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
