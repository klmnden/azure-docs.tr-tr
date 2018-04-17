---
title: Satır düzeyi güvenlikte Power BI çalışma koleksiyonları
description: Satır düzeyi güvenlik Power BI çalışma koleksiyonları ile ilgili ayrıntıları
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ROBOTS: NOINDEX
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: 7256e2f798fbc32c098f19f60b62e577300868c7
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="row-level-security-with-power-bi-workspace-collections"></a>Satır düzeyi güvenlikte Power BI çalışma koleksiyonları

Satır düzeyi güvenlik (RLS), rapor veya veri kümesi, birden çok farklı kullanıcıların aynı raporun tüm görme farklı veriler kullanmasına izin vermeyi içinde belirli veri kullanıcı erişimi sınırlamak için kullanılabilir. Power BI çalışma koleksiyonları RLS ile yapılandırılmış veri kümelerini destekler.

![Satır düzeyi güvenlik Power BI çalışma koleksiyonlarda akışı](media/row-level-security/flow-1.png)

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

RLS yararlanmak için üç ana kavramı anlamak önemlidir; Kullanıcıları, rolleri ve kuralları. Her daha yakın bir göz atalım:

**Kullanıcıların** – Bu gerçek son kullanıcılar görüntülemekte olduğunuz raporlar. Power BI çalışma koleksiyonları'nda, kullanıcıların bir uygulama belirteci username özelliği tarafından tanımlanır.

**Rolleri** – kullanıcıları rollere ait. Bir rolü kuralları için bir kapsayıcı ve bir şey "Satış Yöneticisi" veya "Satış Temsilcisi" gibi adlandırılabilir Power BI çalışma koleksiyonları'nda, kullanıcıların bir uygulama belirteci roller özelliği tarafından tanımlanır.

**Kuralları** – rollerin kurallar vardır ve verilere uygulanacak giderek gerçek filtreler bu kurallardır. Bu kadar basit olabilir "Ülke = tr" veya daha fazla dinamik bir şey.

### <a name="example"></a>Örnek

Bu makalede kalanı için RLS yazma ve, katıştırılmış bir uygulama içinde kullanan bir örnek sağlar. Bizim örneğimizde kullanan [Retail Analysis örnek](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX dosyası.

![Örnek satış raporu](media/row-level-security/scenario-2.png)

Retail Analysis örneğimizde bir belirli perakende zincirindeki tüm depoları için satış gösterir. Hangi bölge olsun Yöneticisi oturum açtığında ve rapor görünümleri RLS aynı verileri görürler. Üst düzey yönetim her bölge yöneticisi yalnızca satış yönettikleri depolarının görmeniz gerekir ve bunu yapmak için biz RLS kullanabilirsiniz belirledi.

RLS, Power BI Desktop'ta yazılmıştır. Rapor ve veri kümesi açıldığında, biz şema görmek için diyagram görünümüne geçiş yapabilirsiniz:

![Power BI Desktop'ta Model Diyagramı](media/row-level-security/diagram-view-3.png)

Bu şema ile fark birkaç şunlardır:

* Tüm ölçüleri, ister **toplam satış**, depolanmış **satış** Olgu Tablosu.
* Dört ek ilgili boyut tabloları vardır: **öğesi**, **zaman**, **deposu**, ve **bölgesi**.
* İlişki satırlarındaki okları filtreleri başka bir tablodan akış hangi yolu belirtin. Örneğin, bir filtre yerleştirildiği **zaman [Date]**, geçerli şemada, yalnızca değerleri aşağı filtreler **satış** tablo. Tüm okları ilişki satırlarındaki satış tabloya ve değil hemen noktası bu yana başka bir tablo bu filtrenin etkilenecek.
* **Bölge** tablo her bölge için yöneticisi olan gösterir:
  
  ![Bölge tablo satırları](media/row-level-security/district-table-4.png)

Biz bir filtre uygularsanız, bu şemaya göre **bölge yöneticisi** bölge tablodaki sütun ve bu filtre raporu görüntüleme kullanıcı eşleşiyorsa, filtre ayrıca filtreleri aşağı **deposu** ve  **Satış** yalnızca tablolara Yöneticisi bu belirli bölge verilerini gösterir.

İşte nasıl:

1. Modelleme sekmesinde, **Rolleri Yönet**.  
   ![Şerit modelleme rolleri düğmesine yönetme](media/row-level-security/modeling-tab-5.png)
2. Adlı yeni bir rol oluşturmak **Yöneticisi**.  
   ![Power BI Desktop'ta roller oluşturma](media/row-level-security/manager-role-6.png)
3. İçinde **bölge** tablo aşağıdaki DAX ifadesi girin: **[Bölge Yöneticisi] USERNAME() =**  
   ![Roldeki tablosu için DAX filtre ifadesi](media/row-level-security/manager-role-7.png)
4. Kuralları çalıştığınız, üzerinde emin olmak için **modelleme** sekmesini tıklatın, **görünüm rolleri olarak**ve ardından aşağıdakileri girin:  
   ![Rolleri olarak görüntüleme](media/row-level-security/view-as-roles-8.png)

   Olarak oturum açtığınızdan sanki raporları şimdi verileri Göster **Barış Ma**.

Burada, yaptığımız şekilde filtre uygulama filtreler tüm kayıtları aşağı **bölge**, **deposu**, ve **satış** tabloları. Ancak, filtre yönünü arasındaki ilişkileri nedeniyle **satış** ve **zaman**, **satış** ve **öğesi**ve **Öğesi** ve **zaman** tabloları aşağı filtrelenmez.

![Vurgulanan ilişkileri ile diyagram görünümü](media/row-level-security/diagram-view-9.png)

Bu gereksinim için Tamam olabilir, ancak bunlar Satış yok öğeleri görmek için yöneticileri olmasını istemezseniz, biz çift yönlü çapraz filtreleme ilişkisi için açın ve her iki yönde güvenlik filtresi akış. Bu arasındaki ilişkiyi düzenleyerek yapılabilir **satış** ve **öğesi**, şöyle:

![İlişki için filtre yönünü arası](media/row-level-security/edit-relationship-10.png)

Şimdi, filtreleri Ayrıca satış tablosundan akabilir **öğesi** tablosu:

![Diyagram görünümünde ilişkinin filtre yönü simgesi](media/row-level-security/diagram-view-11.png)

> [!NOTE]
> Verileriniz için DirectQuery modu kullanıyorsanız, bu iki seçenek seçerek çift yönlü çapraz filtreleme etkinleştirmeniz gerekir:

1. **Dosya** -> **seçenekleri ve ayarları** -> **Önizleme özellikleri** -> **çapraz DirectQuery için her iki yönde de filtreleme etkinleştir** .
2. **Dosya** -> **seçenekleri ve ayarları** -> **DirectQuery** -> **DirectQuerymodundaKısıtlanmamışölçüizin**.

Çift yönlü çapraz filtreleme hakkında daha fazla bilgi için indirme [çift yönlü çapraz filtreleme SQL Server Analysis Services 2016 ve Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) teknik incelemesi.

Bu Power BI Desktop'ta yapılması gereken tüm iş yukarı sarmalar, ancak iş biz Power BI Embedded içinde tanımlanan bir daha fazla parça of RLS hale getirilmesi için gereken iş kuralları yok. Kullanıcıların kimlik doğrulaması ve uygulamanız tarafından yetkili ve uygulama belirteçleri, belirli bir Power BI Embedded rapor kullanıcı erişimi vermek için kullanılır. Power BI Embedded kullanıcı kim herhangi bir özel bilgi yok. Çalışmak RLS için uygulama belirtecinin bir parçası bazı ek bağlam geçmesi gerekir:

* **Kullanıcı adı** (isteğe bağlı) – RLS ile kullanılan bu RLS kuralları uygularken kullanıcı tanımlamaya yardımcı olmak için kullanılan bir dize değeridir. Bkz. satır düzeyi güvenlikte Power BI Embedded kullanma
* **rolleri** – satır düzeyi güvenlik kuralları uygularken seçmek için rolleri içeren bir dize. Birden fazla rol geçirilirse, bir dize dizisi olarak aktarılmalıdır.

Kullanarak belirteci oluşturma [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#Microsoft_PowerBI_Security_PowerBIToken_CreateReportEmbedToken_System_String_System_String_System_String_System_DateTime_System_String_System_Collections_Generic_IEnumerable_System_String__) yöntemi. Username özelliği varsa, ayrıca en az bir değer rollerinde geçmesi gerekir.

Örneğin, EmbedSample değiştirebilirsiniz. Gelen DashboardController satır 55 güncelleştirilemedi

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

-

    var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id, "Andrew Ma", ["Manager"]);'

Tam uygulama belirteci şuna benzer:

![JSON web belirteci örneği](media/row-level-security/app-token-string-12.png)

Birisi bu raporu görüntülemek için uygulamamız oturum açtığında artık, tüm parçaları ile birlikte, bunlar görmek için bizim satır düzeyi güvenlik tarafından tanımlanan izin verilen verileri görürler.

![Uygulamasında görüntülenen raporu](media/row-level-security/dashboard-13.png)

## <a name="see-also"></a>Ayrıca bkz.

[Satır düzeyi güvenlik (RLS) güç](https://powerbi.microsoft.com/documentation/powerbi-admin-rls/)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)