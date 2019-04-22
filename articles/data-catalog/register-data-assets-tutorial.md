---
title: Azure Veri Kataloğu’ndaki veri varlıklarını kaydetme
description: Azure veri Kataloğu'nda veri varlıklarını kaydetme
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: tutorial
ms.date: 04/08/2019
ms.openlocfilehash: e89bd4806e2bb2369c100e948be51dcf32f2e123
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59362507"
---
# <a name="tutorial-register-data-assets-in-azure-data-catalog"></a>Öğretici: Azure Veri Kataloğu’ndaki veri varlıklarını kaydetme

Bu öğreticide, Azure SQL veritabanı örnekteki veri varlıklarını kataloğa kaydetmek için kayıt aracını kullanın. Kayıt, veri kaynağı ve içerdiği varlıklara ait adlar, türler ve konumlar gibi önemli yapısal meta verilerin ayıklanması ve meta verilerin kataloğa kopyalanması işlemidir. Veri kaynakları ve veri varlıkları olduğu yerde kalır, ancak katalog tarafından daha kolay bulunabilir ve anlaşılabilir hale getirilmeleri için meta veriler kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Veri varlıklarını kaydetme 
> * Veri varlıkları Ara
> * Veri varlıklarına açıklama ekleme
> * Veri varlıklarına bağlanma
> * Veri varlıklarını yönetme
> * Veri varlığını silme

## <a name="prerequisites"></a>Önkoşullar

Başlamak için tamamlamanız gereken [hızlı](register-data-assets-tutorial.md).

* A [Microsoft Azure](https://azure.microsoft.com/) abonelik.
* Kendi ihtiyacınız [Azure Active Directory kiracısı](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

Veri Kataloğu'nu ayarlamak için sahibi veya bir Azure aboneliğinin ortak sahibi olmalıdır.

## <a name="register-data-assets"></a>Veri varlıklarını kaydetme

### <a name="register-a-data-source"></a>Veri kaynağını kaydetme

Veri kaynaklarını (tablolar), kaydetmek bir [Azure SQL veritabanı örnek](../sql-database/sql-database-single-database-get-started.md), ancak bildiğiniz ve rolünüzle ilgili verilerle çalışmayı tercih ederseniz herhangi bir desteklenen veri kaynağı kullanabilirsiniz. Desteklenen veri kaynaklarının listesi için bkz. [Desteklenen veri kaynakları](data-catalog-dsr.md).

Kullanmakta olduğumuz Bu öğreticide Azure SQL veritabanı adı *RLSTest*.

Artık Azure veri kataloğu kullanarak Azure SQL veritabanı örnek veri varlıklarından kaydedebilirsiniz.

1. Git [Azure veri Kataloğu giriş sayfasına](http://azuredatacatalog.com) seçip **verileri Yayımla**.

   ![Azure Veri Kataloğu--Verileri Yayımla düğmesi](media/register-data-assets-tutorial/data-catalog-publish-data.png)

2. seçin **uygulama Başlat** indirmek, yüklemek ve bilgisayarınızda kayıt aracını çalıştırın.

   ![Azure Veri Kataloğu--Başlat düğmesi](media/register-data-assets-tutorial/data-catalog-launch-application.png)

3. Üzerinde **Hoş Geldiniz** sayfasında **oturum** ve kimlik bilgilerinizi girin.

    ![Azure Veri Kataloğu--Hoş geldiniz sayfası](media/register-data-assets-tutorial/data-catalog-welcome-dialog.png)

4. Üzerinde **Microsoft Azure veri Kataloğu** sayfasında **SQL Server** ve **sonraki**.

    ![Azure Veri Kataloğu--veri kaynakları](media/register-data-assets-tutorial/data-catalog-data-sources.png)

5. Azure SQL veritabanı Örneğiniz için SQL Server bağlantı özelliklerini girin ve seçin **CONNECT**.

   ![Azure Veri Kataloğu--SQL Server bağlantı ayarları](media/register-data-assets-tutorial/data-catalog-sql-server-connection.png)

6. Veri varlığınızın meta verilerini kaydedin. Bu örnekte, kayıt **ürün** Azure SQL veritabanı örnek ad alanından nesneler:

    1. İçinde **sunucusu hiyerarşisi** ağacı, Azure SQL veritabanı örneğiniz genişletin ve seçin **SalesLT**.

    2. Seçin **ürün**, **ProductCategory**, **ProductDescription**, ve **ProductModel** kullanarak Ctrl + seçin.

    3. seçin **seçili Taşı okuna** (**>**). Bu eylem seçilen tüm nesneleri **Kaydedilecek nesneler** listesine taşır.

          ![Azure Veri Kataloğu öğreticisi--nesnelere göz atma ve seçme](media/register-data-assets-tutorial/data-catalog-server-hierarchy.png)

    4. Verilerin bir anlık görüntü önizlemesini dahil etmek için **Önizleme Ekle**’yi seçin. Anlık görüntü her tablodan en fazla 20 kayıt içerir ve kataloğa kopyalanır.

    5. Veri profili için nesne istatistiklerinin bir anlık görüntüsünü dahil etmek üzere **Veri Profili Ekle**’yi seçin (örneğin: bir sütun için en küçük, en büyük ve ortalama değerler, satır sayısı).

    6. İçinde **Etiket Ekle** alanına **satış, ürün, azure sql**. Bu eylem söz konusu veri varlıklarına arama etiketleri ekler. Etiketler, kullanıcıların kayıtlı bir veri kaynağını bulmasına yardımcı olmak için kullanışlı bir yoludur.

    7. Bu veriler için bir **uzman** adı belirtin (isteğe bağlı).

          ![Azure Veri Kataloğu öğreticisi--kaydedilecek nesneler](media/register-data-assets-tutorial/data-catalog-objects-register.png)

    8. Seçin **kaydetme**. Azure Veri Kataloğu seçtiğiniz nesneleri kaydeder. Bu alıştırmada, Azure SQL veritabanı örneğiniz seçilen nesnelerden kaydedilir. Kayıt aracı, veri varlığından meta verileri ayıklar ve bu verileri Azure Veri Kataloğu hizmetine kopyalar. Burada şu anda kalır veri kalır. Veriler, Yöneticiler, Denetim ve ilkeleri kaynak sistemi altında kalır.

          ![Azure Veri Kataloğu--kayıtlı nesneler](media/register-data-assets-tutorial/data-catalog-registered-objects.png)

    9. Kayıtlı veri kaynağı nesnelerinizi görmek için seçin **portalı görüntüle**. Azure veri Kataloğu portalında dört tablonun tamamını ve veritabanını ızgara görünümünde gördüğünüzü onaylayın (Arama çubuğuna açık olduğundan emin olun).

        ![Azure Veri Kataloğu portalındaki nesneler](media/register-data-assets-tutorial/data-catalog-view-portal.png)

Bu alıştırmada, böylece bir kolayca kullanıcılar tarafından kuruluşunuz genelinde bulunabilmeleri Azure SQL veritabanı örnek nesneleri kaydettiniz.

Sonraki alıştırmada, kayıtlı veri varlıklarını nasıl bulacağınızı öğreneceksiniz.

## <a name="discover-data-assets"></a>Veri varlıklarını bulma

Azure Veri Kataloğu’nda bulma işlemi iki birincil mekanizmayı kullanır: arama ve filtreleme.

Arama hem sezgisel hem de güçlü olacak şekilde tasarlanmıştır. Varsayılan olarak, arama terimleri kullanıcı tarafından ek açıklamalar dahil olmak üzere katalogdaki herhangi bir özellikle eşleştirilir.

Filtreleme, aramayı tamamlamak üzere tasarlanmıştır. Eşleşen veri varlıklarını görüntülemek ve arama sonuçlarını eşleşen varlıklarla kısıtlamak için uzmanlar, veri kaynağı türü, nesne türü ve etiketler gibi belirli özellikleri seçebilirsiniz.

Aramayı ve filtrelemeyi, bir birleşimini kullanarak, Azure veri Kataloğu'na kayıtlı veri kaynaklarına hızlıca gidebilirsiniz.

Bu alıştırmada, önceki alıştırmada kaydettiğiniz veri varlıklarını bulmak için Azure Veri Kataloğu portalını kullanacaksınız. Arama söz dizimiyle ilgili ayrıntılar için bkz. [Veri Kataloğu Arama söz dizimi başvurusu](/rest/api/datacatalog/#search-syntax-reference).

Katalogdaki veri varlıklarını bulmaya yönelik birkaç örnek aşağıda verilmiştir.  

### <a name="discover-data-assets-with-basic-search"></a>Basit arama ile veri varlıklarını bulma

Basit arama bir veya daha fazla arama terimi kullanarak bir katalogda arama yapmanıza yardımcı olur. Sonuçlar herhangi bir özellikte belirtilen terimlerin bir veya daha fazlasıyla eşleşen tüm varlıkları içerir.

1. seçin **giriş** Azure veri Kataloğu Portalı'nda. Web tarayıcısını kapattıysanız Git [Azure veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com).

2. Arama kutusuna `product` yazın ve **ENTER** tuşuna basın.

    ![Azure Veri Kataloğu--basit metin araması](media/register-data-assets-tutorial/data-catalog-basic-text-search.png)

3. Dört tablonun tamamını ve veritabanını sonuçlarında gördüğünüzü onaylayın. Arasında geçiş yapabilirsiniz **kılavuz görünümü** ve **liste görünümü** aşağıdaki görüntüde gösterildiği gibi araç çubuğundaki düğmeler seçerek. **Vurgula** seçeneği **Açık** olduğu için arama anahtar sözcüğü arama sonuçlarında vurgulanır. Arama sonuçlarında **sayfa başına sonuç** sayısını da belirtebilirsiniz.

    ![Azure Veri Kataloğu--basit metin araması sonuçları](media/register-data-assets-tutorial/data-catalog-basic-text-search-results.png)

    **Aramalar** bölmesi sol tarafta, **Özellikleri** bölmesi sağ taraftadır. **Aramalar** bölmesinde arama ölçütlerini değiştirebilir ve sonuçları filtreleyebilirsiniz. **Özellikler** bölmesinde seçili nesnenin özellikleri ızgara veya liste görünümünde gösterilir.

4. seçin **ürün** arama sonuçlarında. seçin **Önizleme**, **sütunları**, **veri profili**, ve **belgeleri** sekmeler veya alt Bölmeyi genişletmek için oku seçin.  

    ![Azure Veri Kataloğu--alt bölme](media/register-data-assets-tutorial/data-catalog-data-asset-preview.png)

    **Önizleme** sekmesinde **Ürün** tablosundaki verilerin bir önizlemesini görürsünüz.  
5. seçin **sütunları** sütunlara ilişkin ayrıntıları bulmak için sekmesinde (gibi **adı** ve **veri türü**) veri varlığının içinde.

6. seçin **veri profili** görmek için sekmesinde (örneğin: satır sayısı veri veya bir sütundaki en küçük değer boyutu) veri varlığının içinde.

### <a name="discover-data-assets-with-property-scoping"></a>Özellik kapsamı ile veri varlıklarını bulma

Özellik kapsamı, arama teriminin belirtilen özellikle eşleştirildiği veri varlıklarını bulmanıza yardımcı olur.

1. **Filtreler** içindeki **Nesne Türü** altında **Tablo** filtresini temizleyin.  

2. Arama kutusuna `tags:product` yazın ve **ENTER** tuşuna basın. Veri kataloğunu aramak üzere kullanabileceğiniz tüm özellikler için bkz. [Veri Kataloğu Arama söz dizimi başvurusu](/rest/api/datacatalog/#search-syntax-reference).

3. Tabloları ve sonuçları veritabanında gördüğünüzü onaylayın.  

    ![Veri Kataloğu--özellik kapsamı arama sonuçları](media/register-data-assets-tutorial/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Aramayı kaydetme

1. İçinde **aramaları** bölmesinde **geçerli aramayı** bölümünde arama için bir ad girin ve seçin **Kaydet**.

    ![Azure Veri Kataloğu--aramayı kaydetme](media/register-data-assets-tutorial/data-catalog-save-search.png)

2. Kayıtlı aramanın **Kayıtlı Aramalar** altında gösterildiğini onaylayın.

3. Kayıtlı arama üzerinde gerçekleştirebileceğiniz eylemlerden birini seçin (aramayı **Yeniden Adlandır**, **Sil**, **Varsayılan Olarak Kaydet**).

### <a name="grouping-with-parentheses"></a>Parantezler ile gruplandırma

Parantezler ile gruplandırma yaparak, özellikle Boole işleçleri ile birlikte mantıksal ayırma sağlamak için sorgunun bölümlerini gruplandırabilirsiniz.

1. Arama kutusuna `name:product AND (tags:product AND objectType:table)` yazın ve **ENTER** tuşuna basın.

2. Arama sonuçlarında yalnızca **Ürün** tablosunu gördüğünüzü onaylayın.

    ![Azure Veri Kataloğu--gruplandırma araması](media/register-data-assets-tutorial/data-catalog-grouping-search.png)

### <a name="comparison-operators"></a>Karşılaştırma işleçleri

Karşılaştırma işleçleri ile sayısal ve tarih veri türlerine sahip özellikler için eşitlik dışındaki karşılaştırmaları kullanabilirsiniz.

1. Arama kutusuna `lastRegisteredTime:>"06/09/2016"` yazın.

2. **Nesne Türü** altındaki **Tablo** filtresini temizleyin.

3. **ENTER**'a basın.

4. Gördüğünüzü onaylayın **ürün**, **ProductCategory**, ve **ProductDescription** tablolarını ve kaydettiğiniz Azure SQL veritabanı arama sonuçları.

    ![Azure Veri Kataloğu--karşılaştırma arama sonuçları](media/register-data-assets-tutorial/data-catalog-comparison-operator-results.png)

Bkz: [veri varlıklarını nasıl bulacağınızı](data-catalog-how-to-discover.md) veri varlıklarını bulma hakkında ayrıntılı bilgi için. Arama söz dizimi hakkında daha fazla bilgi için bkz. [veri kataloğu arama söz dizimi başvurusu](/rest/api/datacatalog/#search-syntax-reference).

## <a name="annotate-data-assets"></a>Veri varlıklarına açıklama ekleme

Bu alıştırmada ek açıklama eklemek için Azure veri Kataloğu portalını kullanacaksınız (açıklamalar, etiketler veya uzmanlar gibi bilgiler Ekle) var olan veri varlıklarını kataloğa. Ek açıklamalar, kayıt sırasında veri kaynağından ayıklanan yapısal meta verileri tamamlar. Ek açıklama, veri varlıklarının bulunmasını ve anlaşılmasını çok daha kolay hale getirir.

Bu alıştırmada tek bir veri varlığına (ProductPhoto) açıklama eklersiniz. ProductPhoto veri varlığına kolay bir ad ve açıklama eklersiniz.  

1. Git [Azure veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com) ve ile arama `tags:product` kayıtlı veri varlıklarını bulun.

2. seçin **ProductModel** arama sonuçlarında.  

3. **Kolay Ad** için **Ürün görüntüleri** ve **Açıklama** için **Pazarlama malzemeleri için ürün fotoğrafları** yazın.

    ![Azure Veri Kataloğu--ProductPhoto açıklaması](media/register-data-assets-tutorial/data-catalog-productmodel-description.png)

    **Açıklama** başkalarının seçilen veri varlığının neden ve nasıl kullanılacağını keşfedip anlamasına yardımcı olur. Ayrıca daha fazla etiket ekleyebilir ve sütunları görüntüleyebilirsiniz. Arama yapabilirsiniz ve veri kaynaklarını kataloğa eklediğiniz açıklayıcı meta verileri kullanarak filtreleyin.

Bu sayfada aşağıdakileri de yapabilirsiniz:

* Veri varlığı için uzmanlar ekleyin. seçin **Ekle** içinde **uzmanlar** alan.

* Veri kümesi düzeyinde etiketler ekleyin. seçin **Ekle** içinde **etiketleri** alan. Etiket bir kullanıcı etiketi veya bir sözlük etiketi olabilir. Veri Kataloğu Standart Sürümü, katalog yöneticilerinin merkezi bir iş sınıflandırması tanımlamasına olanak tanıyan bir iş sözlüğü içerir. Böylece katalog kullanıcıları, sözlük terimlerini veri varlıklarına ek açıklama olarak dahil edebilir. Daha fazla bilgi için bkz. [İş Sözlüğünü Yönetilen Etiketleme için ayarlama](data-catalog-how-to-business-glossary.md)

* Sütun düzeyinde etiketler ekleyin. seçin **Ekle** altında **etiketleri** açıklama eklemek istediğiniz sütun.

* Sütun düzeyinde açıklama ekleyin. Sütun için **Açıklama** girin. Ayrıca veri kaynağından ayıklanan açıklama meta verilerini görüntüleyebilirsiniz.

* Kullanıcılara veri varlığına nasıl erişeceklerini gösteren **Erişim isteği** bilgilerini ekleyin.
  
* **Belgeler** sekmesini seçin ve veri varlığı için belgeleri belirtin. Azure Veri Kataloğu belgeleri ile veri varlıklarınızın tam bir açıklamasını oluşturmak üzere veri kataloğunuzu bir içerik deposu olarak kullanabilirsiniz.
  
Birden fazla veri varlığına da ek açıklama ekleyebilirsiniz. Örneğin, kaydettiğiniz tüm veri varlıklarını seçebilir ve bunlar için bir uzman belirtebilirsiniz.

![Azure Veri Kataloğu--birden fazla veri varlığına açıklama ekleme](media/register-data-assets-tutorial/data-catalog-multi-select-annotate.png)

Azure Veri Kataloğu, ek açıklamalara yönelik kitle kaynak yaklaşımını destekler. Herhangi bir veri Kataloğu kullanıcısı etiketler (kullanıcı veya sözlük), açıklamalar ve diğer meta veriler ekleyebilirsiniz. Bunun yapılması, kullanıcılar bir veri varlığına ve kullanımına üzerinde perspektif eklemek ve bu perspektif diğer kullanıcılarla paylaşma.

Veri varlıklarına açıklama ekleme hakkında ayrıntılı bilgi için bkz. [Veri varlıklarına açıklama ekleme](data-catalog-how-to-annotate.md).

## <a name="connect-to-data-assets"></a>Veri varlıklarına bağlanma

Bu alıştırmada bağlantı bilgilerini kullanarak veri varlıklarını tümleşik bir istemci aracında (Excel) ve tümleşik olmayan bir araçta (SQL Server Management Studio) açarsınız.

> [!NOTE]
> Azure Veri Kataloğu gerçek veri kaynağına erişmenizi sağlamak; yalnızca onu bulup anlamanızı kolaylaştırır. Bir veri kaynağına bağlandığınızda seçtiğiniz istemci uygulaması Windows kimlik bilgilerinizi kullanır veya gerektiğinde kimlik bilgilerinizi ister. Daha önce veri kaynağı için size erişim verilmemişse bağlanabilmeniz için ilk olarak erişim verilmesi gerekir.

### <a name="connect-to-a-data-asset-from-excel"></a>Excel'den veri varlığına bağlanma

1. Arama sonuçlarından **Ürün**’ü seçin. seçin **açma** seçin ve araç **Excel**.

    ![Azure Veri Kataloğu--veri varlığına bağlanma](media/register-data-assets-tutorial/data-catalog-connect1.png)

2. Seçin **açık** indirme açılır penceresinde. Bu deneyim tarayıcıya bağlı olarak farklılık gösterir.

3. İçinde **Microsoft Excel Güvenlik Bildirimi** penceresinde **etkinleştirme**.

    ![Azure Veri Kataloğu--Excel güvenlik açılır penceresi](media/register-data-assets-tutorial/data-catalog-excel-security-popup.png)

4. Varsayılan ayarları değiştirmeden **verileri içeri aktarma** seçin **Tamam**.

    ![Azure Veri Kataloğu--Excel ile verileri içeri aktarma](media/register-data-assets-tutorial/data-catalog-excel-import-data.png)

5. Excel'de veri kaynağını görüntüleyin.

    ![Azure Veri Kataloğu--Excel’deki ürün tablosu](media/register-data-assets-tutorial/data-catalog-connect2.png)

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

Bu alıştırmada Azure Veri Kataloğu kullanarak bulunan veri varlıklarına bağlandınız. Azure Veri Kataloğu portalı ile **Şurada Aç** menüsüne tümleştirilmiş istemci uygulamalarını kullanarak doğrudan bağlantı kurabilirsiniz. Ayrıca varlık meta verilerine dahil edilen bağlantı konumu bilgilerini kullanarak seçtiğiniz herhangi bir uygulamayla bağlantı kurabilirsiniz. Örneğin, bu öğreticide kaydedilen veri varlıklarındaki verilere erişmek için Azure SQL veritabanına bağlanmak için SQL Server Management Studio'yu kullanabilirsiniz.

1. **SQL Server Management Studio**’yu açın.

2. **Sunucuya Bağlan** iletişim kutusunda Azure Veri Kataloğu portalındaki **Özellikler** bölmesinde bulunan sunucu adını girin.

3. Veri varlığına erişmek için uygun kimlik doğrulama ve kimlik bilgilerini kullanın. Erişiminiz yoksa **Erişim İsteği** alanındaki bilgileri kullanarak erişim elde edin.

    ![Azure Veri Kataloğu--erişim isteği](media/register-data-assets-tutorial/data-catalog-request-access.png)

Seçin **bağlantı dizelerini görüntüle** görüntüleme ve ADO.NET, ODBC ve OLEDB bağlantı dizeleri, uygulamanızda kullanmak için panoya kopyalayın.

## <a name="manage-data-assets"></a>Veri varlıklarını yönetme

Bu adımda veri varlıklarınız için güvenliği ayarlarsınız. Veri Kataloğu kullanıcılara veri için erişim vermek değil. Veri erişimini veri kaynağının sahibi denetler.

Veri Kataloğu’nu kullanarak veri kaynaklarını bulabilir ve kataloğa kayıtlı kaynaklarla ilgili meta verileri görüntüleyebilirsiniz. Ancak, veri kaynaklarının yalnızca belirli kullanıcılara veya belirli grupların üyelerine görünmesi gereken durumlar olabilir. Bu senaryolar için veri Kataloğu, kayıtlı veri varlıklarının sahipliğini kullanacağınızı ve sahip olduğunuz varlıklarının görünürlüğünü denetleme.

> [!NOTE]
> Bu alıştırmada tanımlanan yönetim özellikleri yalnızca Azure Veri Kataloğu Standart Sürümü'nde mevcuttur; Ücretsiz Sürüm içinde mevcut değildir.
> Azure Veri Kataloğu'nda veri varlıklarının sahipliğini alabilir, veri varlıklarına ikincil sahip atayabilir ve veri varlıklarının görünürlüğünü ayarlayabilirsiniz.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Veri varlıklarının sahipliğini alma ve görünürlüğü kısıtlama

1. [Azure Veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com) gidin. **Arama** metin kutusuna `tags:cycles` yazın ve **ENTER** tuşuna basın.

2. sonuç listesinden bir öğe seçin ve seçin **Sahipliği Al** araç.

3. İçinde **Yönetim** bölümünü **özellikleri** paneli, select **Sahipliği Al**.

    ![Azure Veri Kataloğu--sahipliği alma](media/register-data-assets-tutorial/data-catalog-take-ownership.png)

4. Görünürlüğü kısıtlamak için seçin **sahipler ve bu kullanıcılar** içinde **görünürlük** seçin ve bölüm **Ekle**. Metin kutusuna kullanıcı e-posta adreslerini girin ve **ENTER** tuşuna basın.

    ![Azure Veri Kataloğu--erişimi kısıtlama](media/register-data-assets-tutorial/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Veri varlıklarını kaldırma

Bu alıştırmada, önizleme verilerini kayıtlı veri varlıklarından kaldırmak ve veri varlıklarını katalogdan silmek için Azure Veri Kataloğu portalını kullanırsınız.

Azure Veri Kataloğu'nda tek bir varlığı veya birden çok varlığı silebilirsiniz.

1. [Azure Veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com) gidin.

2. İçinde **arama** metin kutusuna `tags:cycles` seçip **ENTER**.

3. Sonuç listesinden bir öğe seçin ve seçin **Sil** aşağıdaki görüntüde gösterildiği gibi araç çubuğundaki:

    ![Azure Veri Kataloğu--ızgara öğesini silme](media/register-data-assets-tutorial/data-catalog-delete-grid-item.png)

    Liste görünümünü kullanıyorsanız, onay kutusu aşağıdaki görüntüde gösterildiği gibi öğenin sol tarafındadır:

    ![Azure Veri Kataloğu--liste öğesini silme](media/register-data-assets-tutorial/data-catalog-delete-list-item.png)

    Ayrıca birden fazla veri varlığı seçebilir ve aşağıdaki görüntüde gösterildiği gibi silebilirsiniz:

    ![Azure Veri Kataloğu--birden fazla veri varlığını silme](media/register-data-assets-tutorial/data-catalog-delete-assets.png)

> [!NOTE]
> Kataloğun varsayılan davranışı, herhangi bir kullanıcının herhangi bir veri kaynağını kaydetmesine ve herhangi bir kullanıcının herhangi bir kayıtlı veri varlığını silmesine olanak tanımaktır. Azure Veri Kataloğu Standart Sürümü'ne dahil edilen yönetim özellikleri, varlıkların sahipliğini almaya, varlıkları bulabilecek kişileri kısıtlamaya ve varlıkları silebilecek kişileri kısıtlamaya yönelik ek seçenekler sağlar.

## <a name="summary"></a>Özet

Bu öğreticide, kurumsal veri varlıklarını kaydetme, bulma, yönetme ve bunlara ek açıklama ekleme de dahil olmak üzere Azure Veri Kataloğu'nun temel özelliklerini incelediniz. Öğreticiyi tamamladığınıza göre artık kataloğu kullanmaya başlayabilirsiniz. Sizin ve ekibinizin bağlı olduğunuz veri kaynaklarını kaydederek ve iş arkadaşlarınızı kataloğu kullanmaya davet ederek hemen şimdi başlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Desteklenen veri kaynakları](data-catalog-dsr.md)