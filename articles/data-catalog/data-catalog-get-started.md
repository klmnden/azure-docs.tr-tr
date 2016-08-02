<properties
   pageTitle="Azure Veri Kataloğu - Veri Kataloğu ile çalışmaya başlama | Microsoft Azure"
   description="Azure Veri Kataloğu'nun senaryolarını ve özelliklerini sunan kapsamlı öğretici"
   documentationCenter=""
   services="data-catalog"
   authors="steelanddata"
   manager=""
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="05/06/2016"
   ms.author="maroche"/>

# Azure Veri Kataloğu ile çalışmaya başlama

Bu makalede **Azure Veri Kataloğu**'nun senaryolarına ve özelliklerine yönelik kapsamlı bir genel bakış sunulmaktadır. Hizmete kaydolduktan sonra, bir Veri Kataloğu oluşturmak ve veri kaynaklarını kaydetmek, bunlara açıklama eklemek ve veri kaynaklarını bulmak için bu adımları uygulayın.

## Öğretici önkoşulları

Bu öğreticiye başlamadan önce, aşağıdakilere sahip olmanız gerekir:

- **Azure aboneliği** - Aboneliğiniz yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
- **Azure Active Directory** - Azure Veri Kataloğu, kimlik ve erişim yönetimi için [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)'yi kullanır.
- **Veri kaynakları** - Azure Veri Kataloğu, veri kaynaklarını bulmaya yönelik özellikler içerir. Bu öğretici, Adventure Works örnek veritabanını kullanır ancak rolünüz için alışılmış ve rolünüzle ilgili verilerle çalışmayı tercih ediyorsanız desteklenen herhangi bir veri kaynağını kullanabilirsiniz. Desteklenen veri kaynaklarının listesi için bkz. [Desteklenen veri kaynakları](data-catalog-dsr.md).

> [AZURE.NOTE] Azure abonelikleri ve Azure Active Directory hakkında daha fazla bilgi için lütfen bkz. [Azure Veri Kataloğu önkoşulları](data-catalog-prerequisites.md).

Adventure Works örnek veritabanını yükleyerek başlayalım.

## Alıştırma 1: Adventure Works örnek veritabanını yükleme

Bu alıştırmada, sonraki alıştırmalarda kullanılan SQL Server Veritabanı Altyapısı için Adventure Works örneğini yükleyeceksiniz.

### Adventure Works 2014 OLTP veritabanını yükleme

Adventure Works veritabanı; Ürünler, Satış ve Satın Alma'yı içeren kurgusal bir bisiklet üreticisine (Adventure Works Cycles) yönelik standart çevrimiçi işlem gerçekleştirme senaryolarını destekler. Bu öğreticide, ürünlerle ilgili bilgileri **Azure Veri Kataloğu**'na kaydedersiniz.

Adventure Works örnek veritabanının nasıl yükleneceği aşağıda açıklanmıştır.

Adventure Works örnek veritabanını yüklemek için, CodePlex üzerinde [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) içinde bulunan bir AdventureWorks2014 yedeklemesini geri yükleyebilirsiniz. Veritabanını geri yüklemek için kullanılabilecek yollardan biri, SQL Server Management Studio'da bir T-SQL betiği çalıştırmaktır.

**Adventure Works örnek veritabanını bir T-SQL betiği ile yükleme**

1.  C:\DataCatalog_GetStarted adında bir klasör oluşturun. Başka bir klasör adı kullanırsanız aşağıdaki T-SQL betiğindeki yolu değiştirdiğinizden emin olun.
2.  [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) dosyasını indirin.
3.  Adventure Works 2014 Full Database Backup.zip dosyasını C:\DataCatalog_GetStarted konumuna ayıklayın. Aşağıdaki betikte yedekleme dosyasının şu konumda bulunduğu varsayılır: C:\DataCatalog_GetStarted\Adventure Works 2014 Full Database Backup\AdventureWorks2014.bak.
4.  **SQL Server Management Studio**'dan, **Standart** araç çubuğunda **Yeni Sorgu**'ya tıklayın.
5.  Sorgu penceresinde aşağıdaki T-SQL kodunu yürütün.

**Adventure Works 2014 veritabanını yüklemek için bu betiği çalıştırın**

    USE [master]
    GO
    -- NOTE: This script is for sample purposes only. The default backup file path for this script is C:\DataCatalog_GetStarted. To run this script, create the default file path or change the file path, and copy AdventureWorks2014.bak into the path.

    -- IMPORTANT: In a production application, restore a SQL database to the data folder for your SQL Server instance.

    RESTORE DATABASE AdventureWorks2014
        -- AdventureWorks2014.bak file location
        FROM disk = 'C:\DataCatalog_GetStarted\Adventure Works 2014 Full Database Backup\AdventureWorks2014.bak'

        -- AdventureWorks2014.mdf database location
        WITH MOVE 'AdventureWorks2014_data' TO 'C:\DataCatalog_GetStarted\AdventureWorks2014.mdf',

        -- AdventureWorks2014.ldf log location
        MOVE 'AdventureWorks2014_Log' TO 'C:\DataCatalog_GetStarted\AdventureWorks2014.ldf'
    ,REPLACE
    GO

T-SQL betiğini çalıştırmaya alternatif olarak, SQL Server Management Studio'yu kullanarak da veritabanını geri yükleyebilirsiniz. Bkz. [Veritabanı Yedeklemesini Geri Yükleme (SQL Server Management Studio)](http://msdn.microsoft.com/library/ms177429.aspx).

Bu alıştırmada, geriye kalan alıştırmalarda kullanılan Adventure Works örnek veritabanını yüklediniz. Sonraki alıştırmada, Adventure Works örnek veritabanındaki tablolarda bulunan **Azure Veri Kataloğu** veri kaynaklarını nasıl kaydedeceğinizi öğreneceksiniz.

## Alıştırma 2: Veri kaynakları kaydetme

Bu alıştırmada, Adventure Works veri kaynaklarını kataloğa kaydetmek için **Azure Veri Kataloğu** kayıt aracını kullanacaksınız. Kayıt, veri kaynağı ve içerdiği varlıklara ait adlar, türler ve konumlar gibi önemli yapısal meta verilerin ayıklanması ve meta verilerin kataloğa kopyalanması işlemidir. Veri kaynakları ve verileri olduğu yerde kalır ancak katalog tarafından daha kolay bulunabilir ve anlaşılabilir hale getirilmeleri için meta veriler kullanılır.

### Veri kaynağını kaydetme

1.  https://azure.microsoft.com/services/data-catalog adresine gidin ve **Başlarken** seçeneğine tıklayın.
2.  **Azure Veri Kataloğu** portalında oturum açın ve **Verileri yayımla**'ya tıklayın.

    ![](media/data-catalog-get-started/data-catalog-publish-data.png)

3.  **Uygulamayı Başlat**'a tıklayın.

    ![](media/data-catalog-get-started/data-catalog-launch-application.png)

4. **Hoş Geldiniz** sayfasında, **Oturum aç**'a tıklayın ve kimlik bilgilerinizi girin.
5. **Microsoft Azure Veri Kataloğu** sayfasında, **SQL Server**'a çift tıklayın veya **SQL Server**'a ve ardından **İleri**'ye tıklayın.

    ![](media/data-catalog-get-started/data-catalog-data-sources.png)

6.  AdventureWorks2014 için SQL Server bağlantı özelliklerini girin (aşağıdaki örneğe bakın) ve **BAĞLAN**'a tıklayın.

    ![](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

7.  Sonraki sayfada, veri kaynağınızın meta verilerini kaydedeceksiniz. Bu örnekte, AdventureWorks Üretimi ad alanındaki **Üretim/ürün** nesnelerini kaydedersiniz. Bu işlemi gerçekleştirmek için aşağıdakileri yapın:

    a. **Sunucusu Hiyerarşisi** ağacında, **Üretim**'e tıklayın.

    ![](media/data-catalog-get-started/data-catalog-server-hierarchy.png)

    b. Product, ProductCategory, ProductDescription ve ProductPhoto öğelerine Ctrl tuşuna basarak tıklayın.

    c. Seçileni taşı okuna tıklayın (**>**). Bu, seçilen tüm Ürün nesnelerini **Kaydedilecek nesneler** listesine taşır.

    ![](media/data-catalog-get-started/data-catalog-available-objects.png)

    d. **Etiket ekle** içinde, açıklama ve fotoğraf girin. Bu, söz konusu veri varlıklarına arama etiketleri ekler. Etiketler, kullanıcıların kayıtlı bir veri kaynağını bulmasına yardımcı olmak için kullanışlı bir yoludur.

    ![](media/data-catalog-get-started/data-catalog-objects-register.png)

    e.  **İsteğe bağlı**: **Önizleme Ekleme** ve **Veri kaynağı uzmanı ekleme** işlemlerini yapabilirsiniz.

    f.  **KAYDET**'e tıklayın. **Azure Veri Kataloğu**, seçilen nesnelerinizi kaydeder. Bu alıştırmada, Adventure Works'ten seçilen nesneler kaydedilir.

    g.  Kayıtlı veri kaynağı nesnelerinizi görmek için **Portalı Görüntüle**'ye tıklayın. **Azure Veri Kataloğu** portalında, veri kaynağı nesnelerini **Kutucuk** veya bir **Liste** içinde görüntüleyebilirsiniz.

    ![](media/data-catalog-get-started/data-catalog-view-portal.png)

Bu alıştırmada, kuruluşunuz genelindeki kullanıcılar tarafından kolayca bulunabilmesi için, Adventure Works örnek veritabanında bulunan nesneleri kaydettiniz.
Sonraki alıştırmada, kayıtlı veri varlıklarını nasıl bulacağınızı öğreneceksiniz.

## Alıştırma 3: Kayıtlı Veri Varlıklarını Bulma

Bu alıştırmada, kayıtlı veri varlıklarını bulmak ve bunların meta verilerini görüntülemek için **Azure Veri Kataloğu** portalını kullanacaksınız. **Azure Veri Kataloğu**, basit anahtar sözcük araması, etkileşimli filtreler ve "ileri" kullanıcılar için gelişmiş arama söz dizimi de dahil olmak üzere, veri varlıklarını bulmaya yönelik birden çok araç sağlar.

### Kayıtlı veri varlıklarını bulma

**Azure Veri Kataloğu**, dönüş verisi kullanıcılarının ihtiyaç duyduğu sorguları kolayca oluşturmanıza olanak sağlayan etkili bir arama söz dizimine sahiptir. **Azure Veri Kataloğu** aramasıyla ilgili ayrıntılar için bkz. [Veri Kataloğu Araması söz dizimi başvurusu](https://msdn.microsoft.com/library/azure/mt267594.aspx).

**Azure Veri Kataloğu** şu arama seçeneklerini içerir:

- Anahtar sözcük araması
- Filtre
- Gelişmiş arama

Ayrıca, hangi veri varlıklarının görüntüleneceğini belirtebilirsiniz. **Azure Veri Kataloğu** şu görüntüleme seçeneklerini içerir:

- Özellikleri görüntüle
- Sütunları görüntüle
- Önizlemeyi görüntüle

Bu örnekte, anahtar sözcük aramasını kullanacaksınız. **Azure Veri Kataloğu** araması birkaç sorgu tekniği içerir. Bu örnekte **Gruplandırma** arama sorgusu kullanılmıştır.

**Sorgu Teknikleri**

|Teknik|Kullanım|Örnek
|---|---|---
|Özellik Kapsamı Belirleme|Yalnızca belirtilen özellikte arama teriminin eşleştiği veri kaynaklarını döndürme|name:ürün
|Mantıksal İşleçler|Bu sayfadaki Boole İşleçleri bölümünde açıklandığı gibi, bir aramayı Boole işleçleri kullanarak genişletme veya daraltma|finans NOT kurumsal
|Parantez ile gruplandırma|Mantıksal ayırma sağlamak için, özellikle Boole işleçleri ile bağlantılı olarak, sorgunun bölümlerini gruplandırmak üzere parantez kullanma|name:product AND (tags:çizim OR tags:fotoğraf)
|Karşılaştırma İşleçleri|Sayısal ve tarih veri türlerine sahip özellikler için eşitlik dışındaki karşılaştırmaları kullanma|creationTime:>11/05/14

Bu örnekte, adın ürüne ve etiketlerin çizime veya etiketlerin fotoğrafa eşit olduğu veri varlıkları için **Gruplandırma** araması gerçekleştirirsiniz.

1. https://azure.microsoft.com/services/data-catalog adresine gidin, **Başlarken** seçeneğine tıklayın ve **Azure Veri Kataloğu** portalında oturum açın.
2. **Veri Kataloğu Arama** kutusunda bir **Gruplandırma** sorgusu girin: (**tags:description OR tags:photo**).
3. Arama simgesine tıklayın veya Enter tuşuna basın. **Azure Veri Kataloğu**, bu arama sorgusuna yönelik veri varlıklarını görüntüler.

    ![](media/data-catalog-get-started/data-catalog-search-box.png)

Bu alıştırmada, kataloğa kaydedilmiş Adventure Works veri varlıklarını bulmak ve görüntülemek için **Azure Veri Kataloğu** portalını kullandınız.

<a name="annotating"/>
## Alıştırma 4: Kayıtlı veri kaynaklarına açıklama ekleme

Bu alıştırmada, daha önce kataloğa kaydedilmiş olan veri varlıklarına açıklama eklemek için **Azure Veri Kataloğu**'nu kullanacaksınız. Sağladığınız ek açıklamalar, kayıt sırasında veri kaynağından ayıklanan yapısal meta verileri destekleyip geliştirecek ve veri varlıklarının bulunmasını ve anlaşılmasını çok daha kolay hale getirecektir. Her **Veri Kataloğu** kullanıcısı kendi ek açıklamalarını sağlayabildiği için, verilere yönelik bir perspektif sahibi olan her kullanıcı bunu kolayca paylaşabilir.

### Veri varlıklarına açıklama ekleme

1. https://azure.microsoft.com/services/data-catalog adresine gidin, **Başlarken** seçeneğine tıklayın ve **Azure Veri Kataloğu** portalında oturum açın.
2. **Bul**'a tıklayın.
3. Bir veya birden fazla **Veri Varlığı** seçin. Bu örnekte, **ProductPhoto**'yu seçin ve "Pazarlama materyalleri için ürün fotoğrafları" ifadesini girin.
4. Başkalarının seçilen veri varlığını neden ve nasıl kullanacağını bulup anlamasına yardımcı olacak bir **Açıklama** girin. Örneğin, "Ürün görüntüleri" ifadesini girin. Ayrıca daha fazla etiket ekleyebilir ve sütunları görüntüleyebilirsiniz.
5. Artık, kataloğa eklediğiniz açıklayıcı meta verileri kullanarak veri kaynaklarını bulmak için aramayı ve filtrelemeyi deneyebilirsiniz.

    ![](media/data-catalog-get-started/data-catalog-annotate.png)

Bu alıştırmada, katalog kullanıcılarının anladıkları terimleri kullanarak veri kaynaklarını keşfedebilmesi için kayıtlı veri varlıklarına açıklayıcı bilgiler eklediniz.

> [AZURE.NOTE] Veri Kataloğu'nun Standart Sürümü, katalog yöneticilerinin merkezi bir iş sınıflandırması tanımlamasına olanak tanıyan bir iş sözlüğü içerir. Böylece katalog kullanıcıları, sözlük terimlerini veri varlıklarına ek açıklama olarak dahil edebilir. Daha fazla bilgi için bkz. [İş Sözlüğünü Yönetilen Etiketleme için ayarlama](data-catalog-how-to-business-glossary.md)  

## Alıştırma 5: Meta veriler için kitle kaynak kullanımını sağlama

Bu alıştırmada, meta verileri katalogdaki veri varlıklarına eklemek için başka bir kullanıcı ile çalışacaksınız. **Azure Veri Kataloğu**'nun ek açıklamalara yönelik kitle kaynak yaklaşımı, herhangi bir kullanıcının etiket, açıklama ve diğer meta verileri eklemesine olanak tanır; böylece bir veri varlığına ve kullanımına yönelik perspektif sahibi olan herhangi bir kullanıcın bu perspektifin yakalanmasını ve başka kullanıcıların kullanımına sunulmasını sağlar.

> [AZURE.NOTE] Bu öğretici için birlikte çalışabileceğiniz başka bir kullanıcı yoksa endişelenmeyin! Veri kataloğuna erişebilen herhangi bir kullanıcı, istediği zaman kendi perspektifini ekleyebilir. Meta verilere yönelik bu kitle kaynak kullanımı yaklaşımı, katalog içeriğinin ve kataloğun meta veri zenginliğinin zamanla artmasına olanak tanır.

### Veri varlıkları ile ilgili meta veriler için kitle kaynak kullanımını sağlama

Bir iş arkadaşınızdan yukarıdaki [Kayıtlı Veri Kaynaklarına Açıklama Ekleme](#annotating) alıştırmasını tekrarlamasını isteyin. İş arkadaşlarınız ProductPhoto gibi bir veri varlığına açıklama ekledikten sonra birden çok ek açıklama göreceksiniz.

![](media/data-catalog-get-started/data-catalog-crowdsource.png)

Bu alıştırmada, **Azure Veri Kataloğu**'nun, herhangi bir kullanıcının bulduğu veri varlıklarına ek açıklama eklemesine olanak tanıyan kitle kaynak kullanımlı meta verilere yönelik özelliklerini incelediniz.

## Alıştırma 6: Veri kaynaklarına bağlanma

Bu alıştırmada, Microsoft Excel kullanarak bir veri kaynağına bağlanmak için **Azure Veri Kataloğu** portalını kullanacaksınız.

> [AZURE.NOTE] **Azure Veri Kataloğu**'nun kullanıcılara asıl veri kaynağı için erişim vermediğinin unutulmaması önemlidir; katalog yalnızca kullanıcıların bu veri kaynaklarını bulup anlamasını kolaylaştırır. Kullanıcılar bir veri kaynağına bağlandığında, seçtikleri istemci uygulaması, gerektiği şekilde bu kullanıcıların Windows kimlik bilgilerini kullanır veya kimlik bilgilerini ister. Kullanıcıya daha önce veri kaynağı için erişim verilmemişse bağlanabilmesi için ilk olarak kendisine erişim verilmesi gerekir.

### Excel'den veri kaynağına bağlanma

1. https://azure.microsoft.com/services/data-catalog adresine gidin, **Başlarken** seçeneğine tıklayın ve **Azure Veri Kataloğu** portalında oturum açın.
2. **Bul**'a tıklayın.
3. Bir veri varlığı seçin. Bu örnekte, ProductCategory'yi seçin.
4. **Şurada Aç:** > **Excel**'i seçin.

    ![](media/data-catalog-get-started/data-catalog-connect1.png)

5. **Microsoft Excel Güvenlik Bildirimi** penceresinde, **Etkinleştir**'e tıklayın.
6. **ProductCategory.odc** dosyasını açın.
7. Veri kaynağı Excel'de açılır.

    ![](media/data-catalog-get-started/data-catalog-connect2.png)

Bu alıştırmada, **Azure Veri Kataloğu** kullanılarak bulunan veri kaynaklarına bağlandınız. **Azure Veri Kataloğu** portalı, kullanıcıların **Şurada aç...** menüsüne tümleştirilmiş istemci uygulamalarını kullanarak doğrudan bağlanmasına ve varlık meta verilerinde bulunan bağlantı konumu bilgilerini kullanarak seçtikleri herhangi bir uygulamayı kullanarak bağlanmasına olanak sağlar.

## Alıştırma 7: Veri kaynağı meta verilerini kaldırma

Bu alıştırmada, önizleme verilerini kayıtlı veri varlıklarından kaldırmak ve veri varlıklarını katalogdan silmek için **Azure Veri Kataloğu** portalını kullanacaksınız.

> [AZURE.NOTE] Kataloğun varsayılan davranışı, herhangi bir kullanıcının herhangi bir veri kaynağını kaydetmesine ve herhangi bir kullanıcının herhangi bir kayıtlı veri varlığını silmesine olanak tanımaktır. **Azure Veri Kataloğu'nun Standart Sürümü**'ne dahil edilen yönetim özellikleri, varlıkların sahipliğini almaya, varlıkları bulabilecek kişileri kısıtlamaya ve varlıkları silebilecek kişileri kısıtlamaya yönelik ek seçenekler sağlar.

**Azure Veri Kataloğu**'nda tek bir varlığı veya birden çok varlığı silebilirsiniz.

### Birden çok veri varlığını silme

1. https://azure.microsoft.com/services/data-catalog adresine gidin, **Başlarken** seçeneğine tıklayın ve **Azure Veri Kataloğu** portalında oturum açın.
2. **Bul**'a tıklayın.
3. Bir veya daha fazla veri varlığı seçin.
4. **Sil**'e tıklayın.

Bu alıştırmada, kayıtlı veri varlıklarını katalogdan kaldırdınız.

## Alıştırma 8: Kayıtlı veri kaynaklarını yönetme

Bu alıştırmada, veri varlıklarının sahipliğini almak ve hangi kullanıcıların bu varlıkları nasıl bulabileceğini ve ne şekilde yöneteceğini denetlemek üzere **Azure Veri Kataloğu**'nun yönetim özelliklerini kullanacaksınız.

> [AZURE.NOTE] Bu alıştırmada tanımlanan yönetim özellikleri yalnızca **Azure Veri Kataloğu'nun Standart Sürümü**'nde mevcuttur; **Ücretsiz Sürüm** içinde mevcut değildir.
**Azure Veri Kataloğu**'nda veri varlıklarının sahipliğini alabilir, veri varlıklarına ikincil sahip atayabilir ve veri varlıklarının görünürlüğünü ayarlayabilirsiniz.

### Veri varlıklarının sahipliğini alma ve görünürlüğü kısıtlama

1. https://azure.microsoft.com/services/data-catalog adresine gidin, **Başlarken** seçeneğine tıklayın ve **Azure Veri Kataloğu** portalında oturum açın.
2. **Bul**'a tıklayın.
3. Bir veya daha fazla veri varlığı seçin.
4. **Özellikler** panelinde, **Yönetim** bölümünde, **Sahipliği Al**'a tıklayın.
5. Görünürlüğü kısıtlamak için **Sahipler ve Bu Kullanıcılar**'a tıklayın.

    ![](media/data-catalog-get-started/data-catalog-ownership.png)

Bu alıştırmada, **Azure Veri Kataloğu**'nun yönetim özelliklerini incelediniz ve seçilen veri varlıklarının görünürlüğünü kısıtladınız.

## Özet

Bu öğreticide, kurumsal veri kaynaklarını kaydetme, bulma, yönetme ve bunlara ek açıklama ekleme de dahil olmak üzere, **Azure Veri Kataloğu**'nun temel özelliklerini incelediniz. Öğreticiyi tamamladığınıza göre artık kataloğu kullanmaya başlayabilirsiniz. Sizin ve ekibinizin bağlı olduğunuz veri kaynaklarını kaydederek ve iş arkadaşlarınızı kataloğu kullanmaya davet ederek hemen şimdi başlayabilirsiniz.



<!---HONumber=Jun16_HO2-->


