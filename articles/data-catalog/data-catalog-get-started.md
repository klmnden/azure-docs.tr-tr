---
title: Azure Veri Kataloğu ile çalışmaya başlama
description: Azure Veri Kataloğu'nun senaryolarını ve özelliklerini sunan kapsamlı öğretici
services: data-catalog
author: steelanddata
ms.author: spelluru
ms.assetid: 03332872-8d84-44a0-8a78-04fd30e14b18
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: c65f5c2ca3f162c17d036198c4285f9c965bbd53
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053501"
---
# <a name="get-started-with-azure-data-catalog"></a>Azure Veri Kataloğu ile çalışmaya başlama
Azure Veri Kataloğu kurumsal veri varlıkları için bir kayıt sistemi ve bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Ayrıntılı bir genel bakış için bkz. [Azure Veri Kataloğu nedir](data-catalog-what-is-data-catalog.md).

Bu öğretici Azure Veri Kataloğu ile çalışmaya başlamanıza yardımcı olur. Bu öğreticide aşağıdaki yordamları gerçekleştirirsiniz:

| Yordam | Açıklama |
|:--- |:--- |
| [Veri kataloğu sağlama](#provision-data-catalog) |Bu yordamda Azure Veri Kataloğu hizmetini hazırlar veya ayarlarsınız. Bu adımı yalnızca katalog daha önce ayarlanmamışsa yapın. Azure hesabınızla ilişkili birden fazla abonelik olsa bile bir kuruluş (Microsoft Azure Active Directory etki alanı) için yalnızca bir veri kataloğunuz olabilir. |
| [Veri varlıklarını kaydetme](#register-data-assets) |Bu yordamda AdventureWorks2014 örnek veritabanından veri varlıklarını veri kataloğuna kaydedersiniz. Kayıt, veri kaynağına ait adlar, türler ve konumlar gibi önemli yapısal meta verilerin ayıklanması ve meta verilerin kataloğa kopyalanması işlemidir. Veri kaynakları ve veri varlıkları olduğu yerde kalır, ancak katalog tarafından daha kolay bulunabilir ve anlaşılabilir hale getirilmeleri için meta veriler kullanılır. |
| [Veri varlıklarını bulma](#discover-data-assets) |Bu yordamda, önceki adımda kaydedilen veri varlıklarını bulmak için Azure Veri Kataloğu portalını kullanırsınız. Bir veri kaynağı Azure Veri Kataloğu’na kaydedildikten sonra kullanıcıların ihtiyaç duydukları verileri kolayca arayabilmesi için hizmet tarafından meta verileri için dizin oluşturulur. |
| [Veri varlıklarına not ekleme](#annotate-data-assets) |Bu yordamda veri varlıkları için ek açıklamalar (açıklamalar, etiketler, belgeler veya uzmanlar gibi bilgiler) sağlarsınız. Bu bilgiler veri kaynağından ayıklanan meta verileri destekler ve veri kaynağını daha fazla kişi için anlaşılır hale getirir. |
| [Veri varlıklarına bağlanma](#connect-to-data-assets) |Bu yordamda veri varlıklarını tümleşik istemci araçlarında (Excel ve SQL Server Veri Araçları gibi) ve tümleşik olmayan bir araçta (SQL Server Management Studio) açarsınız. |
| [Veri varlıklarını yönetme](#manage-data-assets) |Bu yordamda veri varlıklarınız için güvenliği ayarlarsınız. Veri Kataloğu kullanıcılara veri erişimi sağlamaz. Veri erişimini veri kaynağının sahibi denetler. <br/><br/> Veri Kataloğu ile veri kaynaklarını bulabilir ve kataloğa kayıtlı kaynaklarla ilgili **meta verileri** görüntüleyebilirsiniz. Ancak, veri kaynaklarının yalnızca belirli kullanıcılara veya belirli grupların üyelerine görünmesi gereken durumlar olabilir. Bu senaryolarda katalog içindeki kayıtlı veri kaynaklarının sahipliğini almak ve sahip olduğunuz kaynakların güvenilirliğini denetlemek üzere Veri Kataloğu hizmetini kullanabilirsiniz. |
| [Veri varlıklarını kaldırma](#remove-data-assets) |Bu yordamda veri varlıklarını veri kataloğundan kaldırma hakkında bilgi edinirsiniz. |

## <a name="tutorial-prerequisites"></a>Öğretici önkoşulları
### <a name="azure-subscription"></a>Azure aboneliği
Azure Veri Kataloğu’nu ayarlamak için bir Azure aboneliğinin sahibi veya ortak sahibi olmanız gerekir.

Azure abonelikleri Azure Veri Kataloğu gibi bulut hizmeti kaynaklarına erişimi düzenlemenize yardımcı olur. Ayrıca kaynak kullanımının nasıl raporlandığını, faturalandırıldığını ve ödendiği denetlemenize yardımcı olur. Her abonelik farklı bir faturalandırma ve ödeme ayarına sahip olabilir, bu nedenle departmana, projeye, bölgesel ofise vb. göre farklı abonelikleriniz ve farklı planlarınız olabilir. Her bir bulut hizmeti bir aboneliği aittir ve Azure Veri Kataloğu’nu ayarlamadan önce bir aboneliğe sahip olmanız gerekir. Daha fazla bilgi için bkz. [Hesapları, abonelikleri ve yönetici rollerini yönetme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

Bir aboneliğiniz yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="azure-active-directory"></a>Azure Active Directory
Azure Veri Kataloğu’nu ayarlamak için bir Azure Active Directory (Azure AD) kullanıcı hesabıyla oturum açmanız gerekir. Bir Azure aboneliğinin sahibi veya ortak sahibi olmanız gerekir.  

Azure AD işletmenizin kimlik ve erişimi hem bulutta hem de şirket içinde yönetmesi için kolay bir yöntem sağlar. Tek bir iş veya okul hesabı kullanarak herhangi bir bulut ya da şirket içi web uygulamasında oturum açabilirsiniz. Azure Veri Kataloğu, oturum açma kimliğini doğrulamak için Azure AD kullanır. Daha fazla bilgi için bkz. [Azure Active Directory Nedir](../active-directory/fundamentals/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory ilke yapılandırması
Azure Veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmak istediğinizde oturum açmanızı engelleyen bir hata iletisiyle karşılaştığınız bir durum yaşayabilirsiniz. Bu hata şirket ağında olduğunuzda ya da şirket ağının dışından bağlantı kurduğunuzda gerçekleşebilir.

Kayıt aracı, kullanıcının Azure Active Directory ile oturum açtığını doğrulamak için *form kimlik doğrulaması* kullanır. Başarılı bir oturum açma için Azure Active Directory yöneticisinin *genel kimlik doğrulama ilkesinde* form kimlik doğrulamasını etkinleştirmesi gerekir.

Genel kimlik doğrulama ilkesi ile aşağıdaki görüntüde gösterildiği gibi kimlik doğrulamasını intranet ve extranet bağlantıları için ayrı ayrı etkinleştirebilirsiniz. Bağlantı kurduğunuz ağ için form kimlik doğrulaması etkin değilse oturum açma hataları oluşabilir.

 ![Azure Active Directory genel kimlik doğrulama ilkesi](./media/data-catalog-prerequisites/global-auth-policy.png)

Daha fazla bilgi için bkz. [Kimlik doğrulama ilkelerini yapılandırma](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Veri kataloğu hazırlama
Bir kuruluş (Azure Active Directory etki alanı) için yalnızca bir tane veri kataloğu hazırlayabilirsiniz. Bu nedenle, bu Azure Active Directory etki alanına ait olan Azure aboneliğinin sahibi ya da ortak sahibi zaten bir katalog oluşturmuşsa birden fazla Azure aboneliğinizin olması durumunda bile bir katalog oluşturamazsınız. Azure Active Directory etki alanınızda bir kullanıcı tarafından veri kataloğu oluşturulup oluşturulmadığını test etmek için [Azure Veri Kataloğu giriş sayfasına](http://azuredatacatalog.com) gidin ve kataloğu görüp görmediğinizi doğrulayın. Sizin için katalog zaten oluşturulmuşsa aşağıdaki yordamı atlayın ve sonraki bölümüne gidin.    

1. [Veri Kataloğu hizmet sayfasına](https://azure.microsoft.com/services/data-catalog) gidin ve **Kullanmaya başlayın** öğesine tıklayın.
   
    ![Azure Veri Kataloğu--pazarlama giriş sayfası](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Azure aboneliğinin sahibi ya da ortak sahibi olan bir kullanıcı hesabıyla oturum açın. Oturum açtıktan sonra aşağıdaki sayfayı görürsünüz.
   
    ![Azure Veri Kataloğu--veri kataloğu hazırlama](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Veri kataloğunun **adını**, kullanmak istediğiniz **aboneliği** ve kataloğun **konumunu** belirtin.
4. **Fiyatlandırma** seçeneğini genişletin ve bir Azure Veri Kataloğu **sürümü** (Ücretsiz veya Standart) seçin.
    ![Azure Veri Kataloğu--sürüm seçme](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. **Katalog Kullanıcıları** seçeneğini genişletin ve **Ekle**’ye tıklayarak veri kataloğu için kullanıcı ekleyin. Bu gruba otomatik olarak eklenirsiniz.
    ![Azure Veri Kataloğu--kullanıcılar](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. **Katalog Yöneticileri** seçeneğini genişletin ve **Ekle**’ye tıklayarak veri kataloğu için başka yöneticiler ekleyin. Bu gruba otomatik olarak eklenirsiniz.
    ![Azure Veri Kataloğu--yöneticiler](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Kuruluşunuza ait veri kataloğunu oluşturmak için **Katalog Oluştur**’a tıklayın. Oluşturulduktan sonra veri kataloğunun giriş sayfasını görürsünüz.
    ![Azure Veri Kataloğu--oluşturuldu](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Azure portalında veri kataloğu bulma
1. Web tarayıcısının ayrı bir sekmesinde veya ayrı bir web tarayıcısı penceresinde [Azure portalına](https://portal.azure.com) gidin ve önceki adımda veri kataloğunu oluşturmak için kullandığınız hesabın aynısıyla oturum açın.
2. **Gözat**’ı seçin ve ardından **Veri Kataloğu**’na tıklayın.
   
    ![Azure Veri Kataloğu--Azure’a göz atın](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) Oluşturduğunuz veri kataloğunu görürsünüz.
   
    ![Azure Veri Kataloğu--kataloğu listede görüntüleme](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
3. Oluşturduğunuz kataloğa tıklayın. Portalda **Veri Kataloğu** dikey penceresini görürsünüz.
   
   ![Azure Veri Kataloğu--portaldaki dikey pencere ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
4. Veri kataloğunun özelliklerini görüntüleyebilir ve güncelleştirebilirsiniz. Örneğin, **Fiyatlandırma katmanı**’na tıklayın ve sürümü değiştirin.
   
    ![Azure Veri Kataloğu--fiyatlandırma katmanı](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works örnek veritabanı
Bu öğreticide SQL Server Database Engine için AdventureWorks2014 örnek veritabanından veri kaynaklarını (tablolar) kaydedersiniz, ancak bildiğiniz ve rolünüzle ilgili olan verilerle çalışmayı tercih ederseniz desteklenen herhangi bir veri kaynağını kullanabilirsiniz. Desteklenen veri kaynaklarının listesi için bkz. [Desteklenen veri kaynakları](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Adventure Works 2014 OLTP veritabanını yükleme
Adventure Works veritabanı; ürünler, satış ve satın almayı içeren kurgusal bir bisiklet üreticisine (Adventure Works Cycles) yönelik standart çevrimiçi işlem gerçekleştirme senaryolarını destekler. Bu öğreticide ürünlerle ilgili bilgileri Azure Veri Kataloğu'na kaydedersiniz.

Adventure Works örnek veritabanını yüklemek için:

1. CodePlex üzerinde [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) dosyasını indirin.
2. Veritabanını makinenize geri yüklemek için [SQL Server Management Studio Kullanarak Veritabanı Yedeğini Geri Yükleme](http://msdn.microsoft.com/library/ms177429.aspx) içindeki yönergeleri izleyin veya aşağıdaki adımları izleyin:
   1. SQL Server Management Studio’yu açın ve SQL Server Database Engine’e bağlanın.
   2. **Veritabanları**’na sağ tıklayın ve **Veritabanını Geri Yükle**’ye tıklayın.
   3. **Veritabanını Geri Yükle** altında **Kaynak** için **Cihaz** seçeneğine ve **Gözat**’a tıklayın.
   4. **Yedekleme cihazları seç** altında **Ekle**’ye tıklayın.
   5. **AdventureWorks2014.bak** dosyasının olduğu klasöre gidin, dosyayı seçin ve **Tamam**’a tıklayarak **Yedek Dosyayı Bul** iletişim kutusunu kapatın.
   6. **Tamam**’a tıklayarak **Yedekleme cihazları seç** iletişim kutusunu kapatın.    
   7. **Tamam**’a tıklayarak **Veritabanını Geri Yükle** iletişim kutusunu kapatın.

Artık Azure Veri Kataloğu kullanarak Adventure Works örnek veritabanından veri varlıklarını kaydedebilirsiniz.

## <a name="register-data-assets"></a>Veri varlıklarını kaydetme
Bu alıştırmada, Adventure Works veritabanından veri varlıklarını kataloğa kaydetmek için kayıt aracını kullanacaksınız. Kayıt, veri kaynağı ve içerdiği varlıklara ait adlar, türler ve konumlar gibi önemli yapısal meta verilerin ayıklanması ve meta verilerin kataloğa kopyalanması işlemidir. Veri kaynakları ve veri varlıkları olduğu yerde kalır, ancak katalog tarafından daha kolay bulunabilir ve anlaşılabilir hale getirilmeleri için meta veriler kullanılır.

### <a name="register-a-data-source"></a>Veri kaynağını kaydetme
1. [Azure Veri Kataloğu giriş sayfasına](http://azuredatacatalog.com) gidin ve **Verileri Yayımla**’ya tıklayın.
   
   ![Azure Veri Kataloğu--Verileri Yayımla düğmesi](media/data-catalog-get-started/data-catalog-publish-data.png)
2. **Uygulamayı Başlat**’a tıklayarak kayıt aracını bilgisayarınıza indirin, yükleyin ve çalıştırın.
   
   ![Azure Veri Kataloğu--Başlat düğmesi](media/data-catalog-get-started/data-catalog-launch-application.png)
3. **Hoş Geldiniz** sayfasında **Oturum aç**'a tıklayın ve kimlik bilgilerinizi girin.     
   
    ![Azure Veri Kataloğu--Hoş geldiniz sayfası](media/data-catalog-get-started/data-catalog-welcome-dialog.png)
4. **Microsoft Azure Veri Kataloğu** sayfasında **SQL Server** ve **İleri**’ye tıklayın.
   
    ![Azure Veri Kataloğu--veri kaynakları](media/data-catalog-get-started/data-catalog-data-sources.png)
5. **AdventureWorks2014** için SQL Server bağlantı özelliklerini girin (aşağıdaki örneğe bakın) ve **BAĞLAN**'a tıklayın.
   
   ![Azure Veri Kataloğu--SQL Server bağlantı ayarları](media/data-catalog-get-started/data-catalog-sql-server-connection.png)
6. Veri varlığınızın meta verilerini kaydedin. Bu örnekte, AdventureWorks Üretimi ad alanındaki **Üretim/Ürün** nesnelerini kaydedersiniz:
   
   1. **Sunucu Hiyerarşisi** ağacında **AdventureWorks2014**’ü genişletin ve **Üretim**’e tıklayın.
   2. Ctrl tuşuna basıp tıklayarak **Product**, **ProductCategory**, **ProductDescription** ve **ProductPhoto** seçimini yapın.
   3. **Seçili oku taşı** öğesine tıklayın (**>**). Bu eylem seçilen tüm nesneleri **Kaydedilecek nesneler** listesine taşır.
      
      ![Azure Veri Kataloğu öğreticisi--nesnelere göz atma ve seçme](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
   4. Verilerin bir anlık görüntü önizlemesini dahil etmek için **Önizleme Ekle**’yi seçin. Anlık görüntü her tablodan en fazla 20 kayıt içerir ve kataloğa kopyalanır.
   5. Veri profili için nesne istatistiklerinin bir anlık görüntüsünü dahil etmek üzere **Veri Profili Ekle**’yi seçin (örneğin: bir sütun için en küçük, en büyük ve ortalama değerler, satır sayısı).
   6. **Etiket ekle** alanına **adventure works, cycles** yazın. Bu eylem söz konusu veri varlıklarına arama etiketleri ekler. Etiketler, kullanıcıların kayıtlı bir veri kaynağını bulmasına yardımcı olmak için kullanışlı bir yoludur.
   7. Bu veriler için bir **uzman** adı belirtin (isteğe bağlı).
      
      ![Azure Veri Kataloğu öğreticisi--kaydedilecek nesneler](media/data-catalog-get-started/data-catalog-objects-register.png)
   8. **KAYDET**'e tıklayın. Azure Veri Kataloğu seçtiğiniz nesneleri kaydeder. Bu alıştırmada, Adventure Works'ten seçilen nesneler kaydedilir. Kayıt aracı, veri varlığından meta verileri ayıklar ve bu verileri Azure Veri Kataloğu hizmetine kopyalar. Veriler o anda bulunduğu yerde kalır ve geçerli sistemin yöneticilerinin ve ilkelerinin denetiminde olmaya devam eder.
      
      ![Azure Veri Kataloğu--kayıtlı nesneler](media/data-catalog-get-started/data-catalog-registered-objects.png)
   9. Kayıtlı veri kaynağı nesnelerinizi görmek için **Portalı Görüntüle**'ye tıklayın. Azure Veri Kataloğu portalında dört tablonun tamamını ve veritabanını ızgara görünümünde gördüğünüzü onaylayın.
      
      ![Azure Veri Kataloğu portalındaki nesneler ](media/data-catalog-get-started/data-catalog-view-portal.png)

Bu alıştırmada, kuruluşunuz genelindeki kullanıcılar tarafından kolayca bulunabilmesi için, Adventure Works örnek veritabanında bulunan nesneleri kaydettiniz. Sonraki alıştırmada, kayıtlı veri varlıklarını nasıl bulacağınızı öğreneceksiniz.

## <a name="discover-data-assets"></a>Veri varlıklarını bulma
Azure Veri Kataloğu’nda bulma işlemi iki birincil mekanizmayı kullanır: arama ve filtreleme.

Arama hem sezgisel hem de güçlü olacak şekilde tasarlanmıştır. Varsayılan olarak, arama terimleri kullanıcı tarafından ek açıklamalar dahil olmak üzere katalogdaki herhangi bir özellikle eşleştirilir.

Filtreleme, aramayı tamamlamak üzere tasarlanmıştır. Eşleşen veri varlıklarını görüntülemek ve arama sonuçlarını eşleşen varlıklarla kısıtlamak için uzmanlar, veri kaynağı türü, nesne türü ve etiketler gibi belirli özellikleri seçebilirsiniz.

Arama ve filtrelemenin bir birleşimini kullanarak, gerekli veri varlıklarını bulmak üzere Azure Veri Kataloğu’na kaydedilmiş veri kaynaklarına hızlıca gidebilirsiniz.

Bu alıştırmada, önceki alıştırmada kaydettiğiniz veri varlıklarını bulmak için Azure Veri Kataloğu portalını kullanacaksınız. Arama söz dizimiyle ilgili ayrıntılar için bkz. [Veri Kataloğu Arama söz dizimi başvurusu](https://msdn.microsoft.com/library/azure/mt267594.aspx).

Katalogdaki veri varlıklarını bulmaya yönelik birkaç örnek aşağıda verilmiştir.  

### <a name="discover-data-assets-with-basic-search"></a>Basit arama ile veri varlıklarını bulma
Basit arama bir veya daha fazla arama terimi kullanarak bir katalogda arama yapmanıza yardımcı olur. Sonuçlar herhangi bir özellikte belirtilen terimlerin bir veya daha fazlasıyla eşleşen tüm varlıkları içerir.

1. Azure Veri Kataloğu portalında **Giriş**’e tıklayın. Web tarayıcısını kapattıysanız [Azure Veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com) gidin.
2. Arama kutusuna `cycles` yazın ve **ENTER** tuşuna basın.
   
    ![Azure Veri Kataloğu--basit metin araması](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Sonuçlarda dört tablonun tamamını ve veritabanını (AdventureWorks2014) gördüğünüzü onaylayın. Aşağıdaki görüntüde gösterildiği gibi araç çubuğundaki düğmelere tıklayarak **ızgara görünümü** ile **liste görünümü** arasında geçiş yapabilirsiniz. **Vurgula** seçeneği **Açık** olduğu için arama anahtar sözcüğü arama sonuçlarında vurgulanır. Arama sonuçlarında **sayfa başına sonuç** sayısını da belirtebilirsiniz.
   
    ![Azure Veri Kataloğu--basit metin araması sonuçları](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)
   
    **Aramalar** bölmesi sol tarafta, **Özellikleri** bölmesi sağ taraftadır. **Aramalar** bölmesinde arama ölçütlerini değiştirebilir ve sonuçları filtreleyebilirsiniz. **Özellikler** bölmesinde seçili nesnenin özellikleri ızgara veya liste görünümünde gösterilir.
4. Arama sonuçlarında **Ürün**’e tıklayın. **Önizleme**, **Sütunlar**, **Veri Profili** ve **Belgeler** sekmelerine tıklayın ya da oka tıklayarak alt bölmeyi genişletin.  
   
    ![Azure Veri Kataloğu--alt bölme](media/data-catalog-get-started/data-catalog-data-asset-preview.png)
   
    **Önizleme** sekmesinde **Ürün** tablosundaki verilerin bir önizlemesini görürsünüz.  
5. **Sütunlar** sekmesine tıklayarak veri varlığının içinde sütunlara ilişkin ayrıntıları bulun (**ad** ve **veri türü** gibi).
6. Veri varlığının içinde verilerin profilini (örneğin: satır sayısı, veri boyutu veya bir sütundaki en küçük değer) görmek için **Veri Profili** sekmesine tıklayın.
7. Soldaki **Filtreler** öğesini kullanarak sonuçları filtreleyin. Örneğin, **Nesne Türü** için **Tablo**’ya tıkladığınızda veritabanını değil, yalnızca dört tabloyu görürsünüz.
   
    ![Azure Veri Kataloğu--arama sonuçlarını filtreleme](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Özellik kapsamı ile veri varlıklarını bulma
Özellik kapsamı, arama teriminin belirtilen özellikle eşleştirildiği veri varlıklarını bulmanıza yardımcı olur.

1. **Filtreler** içindeki **Nesne Türü** altında **Tablo** filtresini temizleyin.  
2. Arama kutusuna `tags:cycles` yazın ve **ENTER** tuşuna basın. Veri kataloğunu aramak üzere kullanabileceğiniz tüm özellikler için bkz. [Veri Kataloğu Arama söz dizimi başvurusu](https://msdn.microsoft.com/library/azure/mt267594.aspx).
3. Sonuçlarda dört tablonun tamamını ve veritabanını (AdventureWorks2014) gördüğünüzü onaylayın.  
   
    ![Veri Kataloğu--özellik kapsamı arama sonuçları](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Aramayı kaydetme
1. **Geçerli Arama** bölümündeki **Aramalar** bölmesine aramanın adını girin ve **Kaydet**’e tıklayın.
   
    ![Azure Veri Kataloğu--aramayı kaydetme](media/data-catalog-get-started/data-catalog-save-search.png)
2. Kayıtlı aramanın **Kayıtlı Aramalar** altında gösterildiğini onaylayın.
   
    ![Azure Veri Kataloğu--kayıtlı aramalar](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Kayıtlı arama üzerinde gerçekleştirebileceğiniz eylemlerden birini seçin (aramayı **Yeniden Adlandır**, **Sil**, **Varsayılan Olarak Kaydet**).
   
    ![Azure Veri Kataloğu--kayıtlı arama seçenekleri](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Boole işleçleri
Aramanızı Boole işleçleriyle genişletebilir veya daraltabilirsiniz.

1. Arama kutusuna `tags:cycles AND objectType:table` yazın ve **ENTER** tuşuna basın.
2. Sonuçlarda yalnızca tabloları (veritabanı değil) gördüğünüzü onaylayın.  
   
    ![Azure Veri Kataloğu--aramada Boole işleci](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Parantezler ile gruplandırma
Parantezler ile gruplandırma yaparak, özellikle Boole işleçleri ile birlikte mantıksal ayırma sağlamak için sorgunun bölümlerini gruplandırabilirsiniz.

1. Arama kutusuna `name:product AND (tags:cycles AND objectType:table)` yazın ve **ENTER** tuşuna basın.
2. Arama sonuçlarında yalnızca **Ürün** tablosunu gördüğünüzü onaylayın.
   
    ![Azure Veri Kataloğu--gruplandırma araması](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Karşılaştırma işleçleri
Karşılaştırma işleçleri ile sayısal ve tarih veri türlerine sahip özellikler için eşitlik dışındaki karşılaştırmaları kullanabilirsiniz.

1. Arama kutusuna `lastRegisteredTime:>"06/09/2016"` yazın.
2. **Nesne Türü** altındaki **Tablo** filtresini temizleyin.
3. **ENTER**'a basın.
4. Arama sonuçlarında **Product**, **ProductCategory**, **ProductDescription** ve **ProductPhoto** tablolarını ve kaydettiğiniz AdventureWorks2014 veritabanını gördüğünüzü onaylayın.
   
    ![Azure Veri Kataloğu--karşılaştırma arama sonuçları](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Veri varlıklarını bulma hakkında ayrıntılı bilgi için [Veri varlıklarını bulma](data-catalog-how-to-discover.md), arama söz dizimi için [Veri Kataloğu Arama söz dizimi başvurusu](https://msdn.microsoft.com/library/azure/mt267594.aspx) bölümüne bakın.

## <a name="annotate-data-assets"></a>Veri varlıklarına açıklama ekleme
Bu alıştırmada daha önce kataloğa kaydettiğiniz veri kaynaklarına açıklama eklemek (açıklamalar, etiketler veya uzmanlar) için Azure Veri Kataloğu portalını kullanırsınız. Ek açıklamalar, kayıt sırasında veri kaynağından ayıklanan yapısal meta verileri destekleyip geliştirir ve veri varlıklarının bulunmasını ve anlaşılmasını çok daha kolay hale getirir.

Bu alıştırmada tek bir veri varlığına (ProductPhoto) açıklama eklersiniz. ProductPhoto veri varlığına kolay bir ad ve açıklama eklersiniz.  

1. [Azure Veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com) gidin ve `tags:cycles` ile arama yaparak kaydettiğiniz veri varlıklarını bulun.  
2. Arama sonuçlarında **ProductPhoto** öğesine tıklayın.  
3. **Kolay Ad** için **Ürün görüntüleri** ve **Açıklama** için **Pazarlama malzemeleri için ürün fotoğrafları** yazın.
   
    ![Azure Veri Kataloğu--ProductPhoto açıklaması](media/data-catalog-get-started/data-catalog-productphoto-description.png)
   
    **Açıklama** başkalarının seçilen veri varlığının neden ve nasıl kullanılacağını keşfedip anlamasına yardımcı olur. Ayrıca daha fazla etiket ekleyebilir ve sütunları görüntüleyebilirsiniz. Bundan böyle kataloğa eklediğiniz açıklayıcı meta verileri kullanarak veri kaynaklarını bulmak için aramayı ve filtrelemeyi deneyebilirsiniz.

Bu sayfada aşağıdakileri de yapabilirsiniz:

* Veri varlığı için uzmanlar ekleyin. **Uzmanlar** alanında **Ekle** öğesine tıklayın.
* Veri kümesi düzeyinde etiketler ekleyin. **Etiketler** alanında **Ekle** öğesine tıklayın. Etiket bir kullanıcı etiketi veya bir sözlük etiketi olabilir. Veri Kataloğu Standart Sürümü, katalog yöneticilerinin merkezi bir iş sınıflandırması tanımlamasına olanak tanıyan bir iş sözlüğü içerir. Böylece katalog kullanıcıları, sözlük terimlerini veri varlıklarına ek açıklama olarak dahil edebilir. Daha fazla bilgi için bkz. [İş Sözlüğünü Yönetilen Etiketleme için ayarlama](data-catalog-how-to-business-glossary.md)
* Sütun düzeyinde etiketler ekleyin. Açıklama eklemek istediğiniz sütun için **Etiketler** altındaki **Ekle** öğesine tıklayın.
* Sütun düzeyinde açıklama ekleyin. Sütun için **Açıklama** girin. Ayrıca veri kaynağından ayıklanan açıklama meta verilerini görüntüleyebilirsiniz.
* Kullanıcılara veri varlığına nasıl erişeceklerini gösteren **Erişim isteği** bilgilerini ekleyin.
  
    ![Azure Veri Kataloğu--etiket, açıklama ekleme](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)
* **Belgeler** sekmesini seçin ve veri varlığı için belgeleri belirtin. Azure Veri Kataloğu belgeleri ile veri varlıklarınızın tam bir açıklamasını oluşturmak üzere veri kataloğunuzu bir içerik deposu olarak kullanabilirsiniz.
  
    ![Azure Veri Kataloğu--Belgeler sekmesi](media/data-catalog-get-started/data-catalog-documentation.png)

Birden fazla veri varlığına da ek açıklama ekleyebilirsiniz. Örneğin, kaydettiğiniz tüm veri varlıklarını seçebilir ve bunlar için bir uzman belirtebilirsiniz.

![Azure Veri Kataloğu--birden fazla veri varlığına açıklama ekleme](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure Veri Kataloğu, ek açıklamalara yönelik kitle kaynak yaklaşımını destekler. Herhangi bir Veri Kataloğu kullanıcısı etiketler (kullanıcı veya sözlük), açıklamalar ve başka meta veriler ekleyebilir; böylece bir veri varlığına ve kullanımına yönelik bakış açısı olan herhangi bir kullanıcı bu bakış açısını yakalayabilir ve diğer kullanıcıların kullanımına sunabilir.

Veri varlıklarına açıklama ekleme hakkında ayrıntılı bilgi için bkz. [Veri varlıklarına açıklama ekleme](data-catalog-how-to-annotate.md).

## <a name="connect-to-data-assets"></a>Veri varlıklarına bağlanma
Bu alıştırmada bağlantı bilgilerini kullanarak veri varlıklarını tümleşik bir istemci aracında (Excel) ve tümleşik olmayan bir araçta (SQL Server Management Studio) açarsınız.

> [!NOTE]
> Azure Veri Kataloğu gerçek veri kaynağına erişmenizi sağlamak; yalnızca onu bulup anlamanızı kolaylaştırır. Bir veri kaynağına bağlandığınızda seçtiğiniz istemci uygulaması Windows kimlik bilgilerinizi kullanır veya gerektiğinde kimlik bilgilerinizi ister. Daha önce veri kaynağı için size erişim verilmemişse bağlanabilmeniz için ilk olarak erişim verilmesi gerekir.
> 
> 

### <a name="connect-to-a-data-asset-from-excel"></a>Excel'den veri varlığına bağlanma
1. Arama sonuçlarından **Ürün**’ü seçin. Araç çubuğunda **İçinde Aç**’ı seçin ve **Excel**’e tıklayın.
   
    ![Azure Veri Kataloğu--veri varlığına bağlanma](media/data-catalog-get-started/data-catalog-connect1.png)
2. İndirme açılır penceresinde **Aç**’a tıklayın. Bu deneyim tarayıcıya bağlı olarak farklılık gösterir.
   
    ![Azure Veri Kataloğu--indirilen Excel bağlantı dosyası](media/data-catalog-get-started/data-catalog-download-open.png)
3. **Microsoft Excel Güvenlik Bildirimi** penceresinde, **Etkinleştir**'e tıklayın.
   
    ![Azure Veri Kataloğu--Excel güvenlik açılır penceresi](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. **Verileri İçeri Aktar** iletişim kutusunda varsayılan ayarları değiştirmeden **Tamam**’a tıklayın.
   
    ![Azure Veri Kataloğu--Excel ile verileri içeri aktarma](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Excel'de veri kaynağını görüntüleyin.
   
    ![Azure Veri Kataloğu--Excel’deki ürün tablosu](media/data-catalog-get-started/data-catalog-connect2.png)

Bu alıştırmada Azure Veri Kataloğu kullanarak bulunan veri varlıklarına bağlandınız. Azure Veri Kataloğu portalı ile **Şurada Aç** menüsüne tümleştirilmiş istemci uygulamalarını kullanarak doğrudan bağlantı kurabilirsiniz. Ayrıca varlık meta verilerine dahil edilen bağlantı konumu bilgilerini kullanarak seçtiğiniz herhangi bir uygulamayla bağlantı kurabilirsiniz. Örneğin, bu öğreticide kaydedilen veri varlıklarındaki verilere erişmek için SQL Server Management Studio’yu kullanarak AdventureWorks2014 veritabanına erişebilirsiniz.

1. **SQL Server Management Studio**’yu açın.
2. **Sunucuya Bağlan** iletişim kutusunda Azure Veri Kataloğu portalındaki **Özellikler** bölmesinde bulunan sunucu adını girin.
3. Veri varlığına erişmek için uygun kimlik doğrulama ve kimlik bilgilerini kullanın. Erişiminiz yoksa **Erişim İsteği** alanındaki bilgileri kullanarak erişim elde edin.
   
    ![Azure Veri Kataloğu--erişim isteği](media/data-catalog-get-started/data-catalog-request-access.png)

ADF.NET, ODBC ve OLEDB bağlantı ayarlarını görüntüleyip, uygulamanızda kullanmak üzere panoya kopyalamak için **Bağlantı Dizelerini Görüntüle**’ye tıklayın.

## <a name="manage-data-assets"></a>Veri varlıklarını yönetme
Bu adımda veri varlıklarınız için güvenliği ayarlarsınız. Veri Kataloğu kullanıcılara veri erişimi sağlamaz. Veri erişimini veri kaynağının sahibi denetler.

Veri Kataloğu’nu kullanarak veri kaynaklarını bulabilir ve kataloğa kayıtlı kaynaklarla ilgili meta verileri görüntüleyebilirsiniz. Ancak, veri kaynaklarının yalnızca belirli kullanıcılara veya belirli grupların üyelerine görünmesi gereken durumlar olabilir. Bu senaryolarda katalog içindeki kayıtlı veri kaynaklarının sahipliğini almak ve sonra sahip olduğunuz kaynakların güvenilirliğini denetlemek üzere Veri Kataloğu hizmetini kullanabilirsiniz.

> [!NOTE]
> Bu alıştırmada tanımlanan yönetim özellikleri yalnızca Azure Veri Kataloğu Standart Sürümü'nde mevcuttur; Ücretsiz Sürüm içinde mevcut değildir.
> Azure Veri Kataloğu'nda veri varlıklarının sahipliğini alabilir, veri varlıklarına ikincil sahip atayabilir ve veri varlıklarının görünürlüğünü ayarlayabilirsiniz.
> 
> 

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Veri varlıklarının sahipliğini alma ve görünürlüğü kısıtlama
1. [Azure Veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com) gidin. **Arama** metin kutusuna `tags:cycles` yazın ve **ENTER** tuşuna basın.
2. Sonuç listesindeki bir öğeye ve araç çubuğundaki **Sahipliği Al** öğesine tıklayın.
3. **Özellikler** panelinin **Yönetim** bölümünde **Sahipliği Al** öğesine tıklayın.
   
    ![Azure Veri Kataloğu--sahipliği alma](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Görünürlüğü kısıtlamak için **Görünürlük** bölümünde **Sahipler ve Bu Kullanıcılar**’ı seçip **Ekle**’ye tıklayın. Metin kutusuna kullanıcı e-posta adreslerini girin ve **ENTER** tuşuna basın.
   
    ![Azure Veri Kataloğu--erişimi kısıtlama](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Veri varlıklarını kaldırma
Bu alıştırmada, önizleme verilerini kayıtlı veri varlıklarından kaldırmak ve veri varlıklarını katalogdan silmek için Azure Veri Kataloğu portalını kullanırsınız.

Azure Veri Kataloğu'nda tek bir varlığı veya birden çok varlığı silebilirsiniz.

1. [Azure Veri Kataloğu giriş sayfasına](https://www.azuredatacatalog.com) gidin.
2. **Arama** metin kutusuna `tags:cycles` yazın ve **ENTER** seçeneğine tıklayın.
3. Sonuç listesinden bir öğe seçin ve aşağıdaki görüntüde gösterildiği gibi araç çubuğunda **Sil** öğesine tıklayın:
   
    ![Azure Veri Kataloğu--ızgara öğesini silme](media/data-catalog-get-started/data-catalog-delete-grid-item.png)
   
    Liste görünümünü kullanıyorsanız aşağıdaki görüntüde gösterildiği gibi onay kutusu öğenin sol tarafındadır:
   
    ![Azure Veri Kataloğu--liste öğesini silme](media/data-catalog-get-started/data-catalog-delete-list-item.png)
   
    Ayrıca birden fazla veri varlığı seçebilir ve aşağıdaki görüntüde gösterildiği gibi silebilirsiniz:
   
    ![Azure Veri Kataloğu--birden fazla veri varlığını silme](media/data-catalog-get-started/data-catalog-delete-assets.png)

> [!NOTE]
> Kataloğun varsayılan davranışı, herhangi bir kullanıcının herhangi bir veri kaynağını kaydetmesine ve herhangi bir kullanıcının herhangi bir kayıtlı veri varlığını silmesine olanak tanımaktır. Azure Veri Kataloğu Standart Sürümü'ne dahil edilen yönetim özellikleri, varlıkların sahipliğini almaya, varlıkları bulabilecek kişileri kısıtlamaya ve varlıkları silebilecek kişileri kısıtlamaya yönelik ek seçenekler sağlar.
> 
> 

## <a name="summary"></a>Özet
Bu öğreticide, kurumsal veri varlıklarını kaydetme, bulma, yönetme ve bunlara ek açıklama ekleme de dahil olmak üzere Azure Veri Kataloğu'nun temel özelliklerini incelediniz. Öğreticiyi tamamladığınıza göre artık kataloğu kullanmaya başlayabilirsiniz. Sizin ve ekibinizin bağlı olduğunuz veri kaynaklarını kaydederek ve iş arkadaşlarınızı kataloğu kullanmaya davet ederek hemen şimdi başlayabilirsiniz.

## <a name="references"></a>Başvurular
* [Veri varlıklarını kaydetme](data-catalog-how-to-register.md)
* [Veri varlıklarını bulma](data-catalog-how-to-discover.md)
* [Veri varlıklarına not ekleme](data-catalog-how-to-annotate.md)
* [Veri varlıklarını belgeleme](data-catalog-how-to-documentation.md)
* [Veri varlıklarına bağlanma](data-catalog-how-to-connect.md)
* [Veri varlıklarını yönetme](data-catalog-how-to-manage.md)

