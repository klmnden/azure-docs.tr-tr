---
title: "Genel SQL bağlayıcı | Microsoft Docs"
description: "Bu makalede, Microsoft'un Genel SQL bağlayıcısının nasıl yapılandırılacağı açıklanmaktadır."
services: active-directory
documentationcenter: 
author: AndKjell
manager: mtillman
editor: 
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: billmath
ms.openlocfilehash: 04a6b7290c4a17d60145355ef1374960a8b6c5ca
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="generic-sql-connector-technical-reference"></a>Genel SQL bağlayıcı Teknik Başvurusu
Bu makalede Genel SQL Bağlayıcısı'nı açıklar. Makale aşağıdaki ürünler için geçerlidir:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Düzeltme 4.1.3671.0 kullanmanız gerekir ya da daha sonra [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ve FIM2010R2 için bağlayıcı yükleme yoluyla kullanılabilir [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Bu bağlayıcı uygulamada görmek için bkz: [Genel SQL bağlayıcı adım adım](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) makalesi.

## <a name="overview-of-the-generic-sql-connector"></a>Genel SQL bağlayıcı genel bakış
Genel SQL Bağlayıcısı'nı, eşitleme hizmeti ODBC bağlantısı sağlayan bir veritabanı sistemi ile tümleştirmenize olanak sağlar.  

Üst düzey açısından bakıldığında, aşağıdaki özellikler bağlayıcı bir geçerli sürümü tarafından desteklenir:

| Özellik | Destek |
| --- | --- |
| Bağlı veri kaynağı |Bağlayıcı tüm 64-bit ODBC sürücüleri ile desteklenir. Bunu aşağıdaki ile test edilmiştir: <li>Microsoft SQL Server ve SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 ve 11 g</li><li>MySQL 5.x</li> |
| Senaryolar |<li>Nesne yaşam döngüsü yönetimi</li><li>Parola Yönetimi</li> |
| İşlemler |<li>Tam içeri aktarma ve Delta içeri aktarma, dışarı aktarma</li><li>Dışarı aktarma: Eklemek, güncelleştirme, silme ve değiştirme</li><li>Parola, parola değiştirme</li> |
| Şema |<li>Nesneler ve özniteliklerin dinamik bulma</li> |

### <a name="prerequisites"></a>Ön koşullar
Bağlayıcısı'nı kullanmadan önce aşağıdaki eşitleme sunucusunda sahip emin olun:

* 4.5.2 Microsoft .NET Framework veya daha yenisi
* 64-bit ODBC istemci sürücüleri

### <a name="permissions-in-connected-data-source"></a>Bağlı veri kaynağı izinleri
Oluşturmak veya genel SQL Connector'daki desteklenen görevleri gerçekleştirmek için şunlara sahip olmalısınız:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Bağlantı noktalarını ve protokolleri
Çalışmak ODBC sürücüsü için gereken bağlantı noktaları, veritabanı satıcısının belgelerine başvurun.

## <a name="create-a-new-connector"></a>Yeni bir bağlayıcı oluşturun
Genel SQL Bağlayıcıyı oluşturmak için **eşitleme hizmeti** seçin **yönetim Aracısı** ve **oluşturma**. Seçin **Genel SQL (Microsoft)** bağlayıcı.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Bağlantı
Bağlayıcı bağlantısı için bir ODBC DSN dosyası kullanır. DSN dosyası kullanarak oluşturmak **ODBC veri kaynakları** Başlat menüsünün altında bulunan **Yönetimsel Araçlar**. Yönetim Aracı'nda oluşturma bir **dosya DSN** bağlayıcıya sağlanabilir şekilde.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Yeni Genel SQL Bağlayıcısı oluşturduğunuzda bağlantı ekran ilk olur. Önce aşağıdaki bilgileri sağlamanız gerekir:

* DSN dosyası yolu
* Kimlik Doğrulaması
  * User Name
  * Parola

Veritabanı bu kimlik doğrulama yöntemlerini birini desteklemelidir:

* **Windows kimlik doğrulaması**: kimlik doğrulama veritabanı kullanıcıyı doğrulamak için Windows kimlik bilgilerini kullanır. Belirtilen kullanıcı adı/parola veritabanı ile kimlik doğrulaması için kullanılır. Bu hesap, veritabanı izinleri olmalıdır.
* **SQL kimlik doğrulaması**: kullanıcı adı/parola tanımlı bir veritabanına bağlanmak için bağlantı ekran kimlik doğrulama veritabanı kullanır. Kullanıcı adı/parolanın DSN dosyasında depolarsanız, bağlantı ekranda sağlanan kimlik bilgileri önceliğe sahiptir.
* **Azure SQL veritabanı kimlik doğrulaması**: daha fazla bilgi için bkz: [SQL veritabanı tarafından kullanarak Azure Active Directory kimlik doğrulaması Bağlan](../../sql-database/sql-database-aad-authentication.md).

**DN olan bağlantı**: Bu seçeneği belirlerseniz, DN bağlantı özniteliği olarak da kullanılır. Basit bir uygulama için kullanılabilir ancak aynı zamanda aşağıdaki sınırlamalara sahiptir:

* Bağlayıcı, yalnızca bir nesne türünü destekler. Bu nedenle herhangi bir başvuru özniteliği yalnızca aynı nesne türü başvuruda bulunabilir.

**Dışarı aktarma türü: Nesne Değiştir**: dışa aktarma sırasında yalnızca bazı öznitelikleri değiştiğinde tüm nesne tüm öznitelikleri ile aktarılır ve varolan nesne yerini alır.

### <a name="schema-1-detect-object-types"></a>Şema 1 (Algıla nesne türleri)
Bu sayfada, farklı nesne türleri veritabanında bulmak için bağlayıcı nasıl gittiğini yapılandırmak olacak.

Her nesne türü bir bölümü olarak sunulan ve yapılandırılmış hakkında daha fazla **yapılandırma bölümleri ve hiyerarşileri**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Nesne türü algılama yöntemi**: bağlayıcı, bu nesne türü algılama yöntemleri destekler.

* **Sabit değer**: nesne türlerinin listesini virgülle ayrılmış bir liste ile sağlayın. Örneğin: `User,Group,Department`.  
  ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
* **Tablo/görünüm/saklı yordam**: Tablo/görünüm/saklı yordamın adını ve ardından nesne türlerinin bir listesini sağlar sütun adı sağlayın. Saklı yordam kullanırsanız, sonra da parametreleri biçiminde sağlayan **[Name]: [Yön]: [değer]**. Her bir parametreyi ayrı bir satırda (kullanın yeni bir satır almak için Ctrl + Enter) sağlayın.  
  ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
* **SQL sorgusu**: Bu seçenek, örneğin nesne türleri ile tek bir sütun döndüren bir SQL sorgusu sağlamanıza olanak verir `SELECT [Column Name] FROM TABLENAME`. Döndürülen sütun (varchar) dize türünden olmalıdır.

### <a name="schema-2-detect-attribute-types"></a>Şema 2 (Algıla öznitelik türlerini)
Bu sayfada, öznitelik adları ve türlerini algılanmış nasıl kalacaklarını yapılandırmak olacak. Önceki sayfada algılanan her nesne türü için yapılandırma seçenekleri listelenmektedir.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Öznitelik türü algılama yöntemi**: bağlayıcı Şeması 1 ekranında bu öznitelik türü algılama yöntemleri her algılanan nesne türü ile destekler.

* **Tablo/görünüm/saklı yordam**: öznitelik adlarını bulmak için kullanılması gereken tablo/görünüm/saklı yordamın adını sağlayın. Saklı yordam kullanırsanız, sonra da parametreleri biçiminde sağlayan **[Name]: [Yön]: [değer]**. Her bir parametreyi ayrı bir satırda (kullanın yeni bir satır almak için Ctrl + Enter) sağlayın. Birden çok değerli bir öznitelikte öznitelik adları algılamak için tabloları veya görünümleri virgülle ayrılmış listesini sağlayın. Birden çok değerli senaryoları desteklenmez üst ve alt tablo aynı sütun adları olduğunda.
* **SQL sorgusu**: Bu seçenek, öznitelik adları ile tek bir sütun örneğin döndüren bir SQL sorgusunu sağlamanıza olanak verir `SELECT [Column Name] FROM TABLENAME`. Döndürülen sütun (varchar) dize türünden olmalıdır.

### <a name="schema-3-define-anchor-and-dn"></a>Şema 3 (tanımla bağlantı ve DN)
Bu sayfa, bağlantı ve DN özniteliği her algılanan nesne türü için yapılandırmanıza olanak sağlar. Bağlantı benzersiz olması için birden fazla öznitelik seçebilirsiniz.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

* Birden çok değerli ve Boolean öznitelikleri listelenmez.
* Aynı öznitelik kullanamaz DN ve bağlayıcı, sürece **DN olan bağlantı** bağlantı sayfasında seçilidir.
* Varsa **DN olan bağlantı** Seçili bağlantı sayfasında bu sayfa yalnızca DN özniteliği gerektiriyor. Bu öznitelik bağlantı özniteliği olarak da kullanılır.

  ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Şema 4 (tanımla öznitelik türü, başvuru ve yön)
Bu sayfa, öznitelik türü, tamsayı, ikili, veya Boolean ve her bir öznitelik için yönü gibi yapılandırmanıza olanak sağlar. Tüm öznitelikleri sayfasından **şema 2** birden çok değerli öznitelikler dahil olmak üzere listelenir.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

* **Veri türü**: öznitelik türü eşitleme altyapısı tarafından bilinen bu türlerine eşlemek için kullanılır. Varsayılan aynı türde SQL şemasında algılanan kullanmaktır, ancak DateTime ve başvuru kolayca algılanamaz. Bunlar için belirtmek zorunda **DateTime** veya **başvuru**.
* **Yön**: öznitelik yönü içeri aktarma, dışarı aktarma veya ImportExport ayarlayabilirsiniz. ImportExport varsayılandır.

![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Notlar:

* Bir öznitelik türü bağlayıcı tarafından algılanamayan değilse, dize veri türü kullanır.
* **İç içe tablolar** tek sütunluk veritabanı tablolarını kabul edilebilir. Oracle iç içe tablonun satırlarını belirli bir sırada depolar. Ancak, iç içe tablo PL/SQL değişkene aldığınızda, satırları 1'den başlayarak ardışık indisleri verilir. Bu, dizi benzeri erişim ayrı satırlara sağlar.
* **VARRYS** bağlayıcıda desteklenmez.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Şema 5 (başvuru öznitelikleri için bölüm tanımlayın)
Bu sayfada, hangi bölümünü (nesne türü) için bir öznitelik başvuran tüm başvuru öznitelikleri için yapılandırın.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Kullanırsanız **DN olan bağlantı**, aynı nesne türü gelen başvurduğunuz birini kullanmanız gerekir. Başka bir nesne türü başvuramaz.

>[!NOTE]
Şimdi bir seçenek olan Mart 2017 güncelleştirme Başlangıç "*" Bu seçenek seçilen sonra tüm olası üye türleri olduğunda alınacak.

![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/any-option.png)

>[!IMPORTANT]
 Mayıs 2017'dan sonra "*" aka **herhangi bir seçenek** akış dışarı ve içeri aktarma desteklemek için değiştirildi. Bu seçeneği kullanmak istiyorsanız, birden çok değerli tablo/görünüm nesne türünü içeren bir öznitelik olmalıdır.

![](./media/active-directory-aadconnectsync-connector-genericsql/any-02.png)

 </br> "*" Nesne türü ile sütunun adı de belirtilmelidir sonra seçilir.</br> ![](./media/active-directory-aadconnectsync-connector-genericsql/any-03.png)

İçeri aktarma işleminden sonra aşağıdaki görüntü benzer bir şey görürsünüz:

  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Genel Parametreler
Genel Parametreler sayfası Delta içeri aktarma, tarih/saat biçimi ve parola yöntemini yapılandırmak için kullanılır.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)



Genel SQL bağlayıcı Delta alma için aşağıdaki yöntemleri destekler:

* **Tetikleyici**: bkz [Tetikleyicileri kullanarak Delta görünümleri oluşturma](https://technet.microsoft.com/library/cc708665.aspx).
* **Filigran**: herhangi bir veritabanı ile kullanılan genel bir yaklaşım. Filigran veritabanı satıcıya göre önceden doldurulmuş haldedir sorgudur. Filigran sütun her tablo/kullanılan görünüm mevcut olması gerekir. Bu sütun eklemeleri izlemelidir ve tabloları olarak ve bağımlı güncelleştirmeleri (birden çok değerli veya alt) tabloları. Eşitleme hizmeti ve veritabanı sunucusu arasındaki saatler eşitlenmelidir. Aksi durumda, bazı girdilerin delta içeri aktarma devre dışı bırakılacak.  
  Sınırlama:
  * Filigran stratejisi değil destek silinmiş nesneler eşleşmiyor.
* **Anlık Görüntü**: (yalnızca Microsoft SQL Server ile birlikte çalışır) [anlık görüntülerini kullanarak Delta görünümler oluşturma](https://technet.microsoft.com/library/cc720640.aspx)
* **Değişiklik izleme**: (yalnızca Microsoft SQL Server ile birlikte çalışır) [değişiklik izleme hakkında](https://msdn.microsoft.com/library/bb933875.aspx)  
  Sınırlamaları:
  * Bağlantı & DN özniteliği tablosundaki seçili nesne için birincil anahtarın bir parçası olması gerekir.
  * SQL sorgusu sırasında içeri ve dışarı aktarma değişiklik izleme desteklenmiyor.

**Ek parametreler**: Veritabanı sunucunuz bulunduğu belirten veritabanı sunucusunun saat dilimini belirtin. Bu değer, tarih ve saat özniteliklerine çeşitli biçimlerde desteklemek için kullanılır.

Bağlayıcı her zaman tarih ve tarih-saat UTC biçiminde depolar. Tarih ve saatini, veritabanı sunucusu ve kullanılan biçimi saat dilimini doğru şekilde dönüştürmek için belirtilmelidir. Biçim .net biçiminde ifade edilmelidir.

Dışa aktarma sırasında her bir tarih saat özniteliği bağlayıcısına UTC saat biçiminde sağlanması gerekir.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Parola yapılandırma**: bağlayıcı parola eşitleme özellikleri sağlar ve ayarlamak ve parola değiştirme destekler.

Bağlayıcı parola eşitlemeyi desteklemek için iki yöntem sunar:

* **Saklı yordam**: Bu yöntem kümesi & değişiklik desteklemek için iki saklı yordamlar gerektirir parola. Ekle için tüm parametreleri yazın ve parolayı işlemde değiştirme **ayarlamak parola SP** ve **değiştirmek parola SP** örnek aşağıdaki parametrelerin sırasıyla göre.
  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
* **Parola uzantısı**: Bu yöntem parola uzantı DLL'si gerektirir (uygulama uzantı DLL adı sağlamanız gereken [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) arabirimi). Böylece çalışma zamanında DLL bağlayıcı yükleyebilir parola uzantı derlemesi uzantısı klasörüne yerleştirilmelidir.
  ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Parola yönetimi etkinleştirmek de **yapılandırmanız uzantı** sayfası.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Bölümleri ve hiyerarşileri Yapılandır
Bölümleri ve hiyerarşileri sayfasında tüm nesne türlerini seçin. Her nesne türü kendi bölümdür.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Üzerinde tanımlanan değerlerden birisini geçersiz kılabilirsiniz **bağlantı** veya **genel parametreleri** sayfası.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Yer işaretlerini Yapılandır
Bağlayıcı zaten tanımlı olduğundan bu sayfayı salt okunurdur. Seçili bağlantı özniteliği her zaman nesne türleri arasında benzersiz kaldığından emin olmak için nesne türü eklenir.

![tutturucular](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Çalışma adımı parametre yapılandırma
Bu adımları çalıştırma profillerini bağlayıcı üzerinde yapılandırılır. Bu yapılandırmalar içeri ve dışarı aktarma veri gerçek iş yapın.

### <a name="full-and-delta-import"></a>Tam ve Delta içeri aktarma
Genel SQL bağlayıcı desteği tam ve Delta içeri aktarma bu yöntemleri kullanarak:

* Tablo
* Görünüm
* Saklı Yordam
* SQL sorgusu

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tablo/görünüm**  
Bir nesne için birden çok değerli öznitelikleri almak için virgülle ayrılmış tablo/görünüm adı sağlamanız gerekiyor **adı, birden çok değerli tablo/Görünüm** ve ilgili birleştirme koşulları **katılma koşulu** üst tabloyla.

Örnek: Çalışan nesne ve tüm birden çok değerli öznitelikleri içeri aktarmak istediğiniz. Çalışan (ana tablo) ve departman (birden çok değerli) adlı iki tablo vardır.
Şunları yapın:

* Tür **çalışan** içinde **tablo/görünüm/SP**.
* Türü departmanında **adı, birden çok değerli tablo/Görünüm**.
* Birleşim koşulu çalışan & departmanında arasındaki yazın **birleştirme koşulunun**, örneğin `Employee.DEPTID=Department.DepartmentID`.
  ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Saklı yordamlar**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

* Fazla veriniz varsa, saklı yordamlar ile sayfalandırma uygulamak için önerilir.
* Sayfalandırma desteklemek için saklı yordam için dizin başlangıç ve bitiş dizini sağlamanız gerekir. Bkz: [verimli bir şekilde büyük miktarlarda verinin disk belleği](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndexve @EndIndex yürütme sırasında yapılandırılan ilgili sayfa boyutu değeri ile değiştirilir **yapılandırma adımı** sayfası. Örneğin, ilk sayfa ve sayfa boyutu bağlayıcı zaman alır 500, böyle bir durumda ayarlanır @StartIndex 1 olur ve @EndIndex 500. Bağlayıcı sonraki sayfalar ve değişiklik aldığında bu değerleri artırmak @StartIndex & @EndIndex değeri.
* Parametreli saklı yordamı yürütmek için parametreleri sağlayın `[Name]:[Direction]:[Value]` biçimi. Her bir parametreyi ayrı bir satırda (yeni bir satır almak için kullanım Ctrl + Enter) girin.
* Genel SQL bağlayıcı, Microsoft SQL Server'da bağlı sunuculardan içeri aktarma işlemi de destekler. Bilgi bağlantılı sunucu tabloda nereden alınacağını, tablo biçiminde sağlanması:`[ServerName].[Database].[Schema].[TableName]`
* Genel SQL bağlayıcı bilgileri ve şeması algılama adımlarını çalıştırmak benzer yapıya (hem diğer adı ve veri türü) arasında sahip nesneleri destekler. Şema ve çalışma adımı sırasında sağlanan bilgiler seçilen nesneden farklıysa, ardından SQL Connector bu tür senaryoları desteklemek üzere alamıyor.

**SQL sorgusu**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

* Birden çok sonuç desteklenmiyor sorguları ayarlar.
* SQL sorgusu sayfalandırma destekler ve başlangıç sağlamak sayfalandırma desteklemek için dizin ve değişken olarak bitiş dizini.

### <a name="delta-import"></a>Delta içeri aktarma
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta içeri aktarma yapılandırma tam içeri aktarma ile karşılaştırıldığında daha fazla miktar yapılandırma gerektirir.

* Delta değişiklikleri izlemek için tetikleyici veya anlık görüntü yaklaşımı seçerseniz, geçmiş tablosu veya anlık görüntü veritabanında sağlamak **geçmiş tablosu veya anlık görüntü veritabanı adı** kutusu.
* Geçmiş tablosu üst tablo arasındaki birleşim koşulu örneğin belirtmeniz gerekir`Employee.ID=History.EmployeeID`
* Geçmiş tablosundan üst tablo üzerinde işlem izlemek için işlem bilgileri (ekleme/güncelleştirme/silme) içeren sütun adı sağlamanız gerekir.
* Delta değişiklikleri izlemek için Filigran seçerseniz, işlem bilgileri içeren sütun adını sağlayın **su işareti sütun adı**.
* **Türü özniteliği değiştirmek** sütun değişiklik türü için gereklidir. Bu sütun birincil tablosu veya birden çok değerli tablosu delta görünümünde değişiklik türü için oluşan bir değişiklik eşler. Bu sütun Modify_Attribute değişiklik türü için öznitelik düzeyi değiştirme veya ekleme, değiştirme, içerebilir veya Delete türü bir nesne düzeyinde değişiklik türü için değiştirin. Varsayılan değer Ekle dışında bir şey olması durumunda, değiştirme veya silme olduktan sonra bu seçeneği kullanarak bu değerleri tanımlayabilirsiniz.

### <a name="export"></a>Dışarı Aktarma
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Genel SQL bağlayıcı desteği gibi dört desteklenen yöntemleri kullanarak verme:

* Tablo
* Görünüm
* Saklı Yordam
* SQL sorgusu

**Tablo/görünüm**  
Tablo/görünüm seçeneği belirlerseniz, bağlayıcı verme işlemini gerçekleştirmek için ilgili sorgular oluşturur.

**Saklı yordamlar**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Saklı yordam seçeneği belirlerseniz, dışa aktarma ekleme/güncelleştirme/silme işlemlerini gerçekleştirmek için üç farklı saklı yordamlar gerektirir.

* **SP adı eklemek**: herhangi bir nesne ekleme ilgili tablodaki için bağlayıcıya geliyorsa bu SP çalıştırır.
* **Güncelleştirme SP adı**: herhangi bir nesne ilgili tablodaki güncelleştirmesi bağlayıcıya geliyorsa bu SP çalıştırır.
* **SP adı silmek**: bağlayıcı ilgili tablodaki silinmek için herhangi bir nesne geliyorsa, bu SP çalıştırır.
* Saklı yordam için bir parametre değeri olarak kullanılan şemasından seçili özniteliği. Örneğin, `@EmployeeName: INPUT: EmployeeName` (EmployeeName bağlayıcı şemada seçili ve dışarı aktarma yaparken ilgili değeri bir bağlayıcının yerine geçer)
* Parametreli saklı yordamını çalıştırmak için parametreleri sağlayın `[Name]:[Direction]:[Value]` biçimi. Her bir parametreyi ayrı bir satırda (yeni bir satır almak için kullanım Ctrl + Enter) girin.

**SQL sorgusu**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

SQL sorgu seçeneği seçerseniz, dışa aktarma ekleme/güncelleştirme/silme işlemlerini gerçekleştirmek için üç farklı sorgu gerektirir.

* **Sorguyu eklemek**: herhangi bir nesne ekleme ilgili tablodaki için bağlayıcı geliyorsa, bu sorguyu çalıştırır.
* **Güncelleştirme sorgusu**: bağlayıcı ilgili tablodaki güncelleştirmesi için herhangi bir nesne geliyorsa, bu sorguyu çalıştırır.
* **Sorguyu silmek**: bağlayıcı ilgili tablodaki silinmek için herhangi bir nesne geliyorsa, bu sorguyu çalıştırır.
* Örneğin bir sorgu parametresi değeri olarak kullanılan şemadan seçili özniteliği`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Sorun giderme
* Bağlayıcı gidermek günlüğü etkinleştirme hakkında daha fazla bilgi için bkz: [bağlayıcıların ETW İzleme etkinleştirmek için nasıl](http://go.microsoft.com/fwlink/?LinkId=335731).
