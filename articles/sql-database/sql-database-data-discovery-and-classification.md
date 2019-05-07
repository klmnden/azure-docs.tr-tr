---
title: Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırma | Microsoft Docs
description: Azure SQL veritabanı ve veri bulma & sınıflandırma
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: e451b7837a1cff4bbeaecd1573dc860524caf4d3
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65142650"
---
# <a name="azure-sql-database-and-sql-data-warehouse-data-discovery--classification"></a>Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırma

Veri bulma & sınıflandırma (şu anda önizlemede), Azure SQL veritabanı için gelişmiş özellikler sunar **keşfetme**, **sınıflandırma**, **etiketleme**  &  **koruma** veritabanlarınızı hassas verileri.
Bulma ve sınıflandırma en hassas verileriniz (iş, Finans, sağlık hizmeti, kişisel verileri (PII) ve benzeri) rol içinde kuruluş bilgilerini koruma stature oynatabilirsiniz. Altyapı olarak hizmet eder:

- Veri gizliliği standartlarını ve yasal uyumluluk gereksinimlerini karşılamak yardımcı olur.
- (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimi üzerinde uyarı.
- Erişimi denetleme ve son derece hassas veri içeren veritabanlarını güvenliğini artırma.

Veri bulma & sınıflandırma parçası olan [gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (REKLAM) sunumunun Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Veri bulma & sınıflandırma erişilen ve merkezi SQL REKLAM portalı üzerinden yönetilebilir.

> [!NOTE]
> Bu belge, Azure SQL veri ambarı ve Azure SQL veritabanı ile ilişkilendirir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır. (Şirket içi) SQL Server için bkz: [SQL veri bulma ve sınıflandırma](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Veri bulma & sınıflandırma nedir

Veri bulma & sınıflandırma birtakım gelişmiş hizmetler ve yalnızca veritabanı verileri korumaya yönelik yeni bir SQL bilgi koruması paradigma oluşturan yeni SQL işlevleri sunar:

- **Bulma ve öneriler**

  Sınıflandırma altyapısının veritabanınızı tarar ve potansiyel olarak hassas veriler içeren sütunlarını tanımlar. Bunun ardından, gözden geçirin ve uygun sınıflandırma öneriler Azure portal aracılığıyla uygulamak için kolay bir yol sağlar.

- **Etiketleme**

  Duyarlılık sınıflandırma etiketleri, SQL motoruna sunulan yeni sınıflandırma meta veri öznitelikleri kullanarak sütunlarda kalıcı olarak etiketlenebilir. Bu meta veriler, ardından Gelişmiş duyarlılık tabanlı denetim ve koruma senaryoları için kullanılabilir.

- **Sorgu sonucu kümesi duyarlılık**

  Sorgu sonucu kümesini duyarlılığına denetim amacıyla gerçek zamanlı olarak hesaplanır.

- **Görünürlük**

  Veritabanı sınıflandırma durumu ayrıntılı bir panoya portalında görüntülenebilir. Ayrıca, uyumluluk ve denetim amacıyla, yanı sıra diğer gereksinimlerini kullanılacak bir raporu (Excel biçiminde) indirebilirsiniz.

## <a id="subheading-2"></a>Bulma, Sınıflandırma ve etiket hassas sütunları

Aşağıdaki bölümde, bulma, Sınıflandırma ve etiketleme veritabanı hem de veritabanı geçerli sınıflandırma durumunu görüntüleme ve raporları dışarı aktarma hassas veriler içeren sütunların adımlarını açıklar.

Sınıflandırma iki meta veri öznitelikleri içerir:

- Sütunda depolanan verilerin duyarlılık düzeyi tanımlamak için kullanılan etiketler – ana sınıflandırma öznitelikleri.  
- Bilgi türlerini sütunda depolanan verilerin türünü içine ek ayrıntı düzeyi sağlar.

## <a name="define-and-customize-your-classification-taxonomy"></a>Tanımlama ve sınıflandırma taksonominizi özelleştirme

SQL veri bulma & sınıflandırma etiketleri duyarlılık yerleşik bir dizi ve bilgi türleri ve bulma mantığını yerleşik bir dizi ile birlikte gelir. Artık bu sınıflandırma özelleştirme ve kümesi ve ortamınız için özel olarak sınıflandırma yapıları sıralamasını tanımlama özelliği var.

Tüm Azure kiracınız için merkezi bir yerde tanımı ve sınıflandırma taksonominizi özelleştirmesini gerçekleştirilir. Konum olduğunu [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), güvenlik ilkenizin bir parçası olarak. Kiracı kök yönetim grubu üzerinde yönetici haklarına sahip biri yalnızca, bu görevi gerçekleştirebilirsiniz.

Information Protection İlkesi yönetiminin bir parçası özel etiketler tanımlamak, bunları derecelendirmek ve bunları seçilen bilgi türleri kümesi ile ilişkilendirebilirsiniz. Ayrıca, kendi özel bilgi türlerini ekleyebilir ve bunları bu tür verilerin veritabanlarınızda tanımlamak için bulma mantığını eklenen dize desenlerle yapılandırabilirsiniz.
Özelleştirme ve ilkenizde yönetme hakkında daha fazla bilgi [Information Protection İlkesi ile ilgili nasıl yapılır Kılavuzu](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409).

Kiracı genelinde bir ilke oluşturulduktan sonra özelleştirilmiş ilkenizi kullanarak tek tek veritabanlarını sınıflandırmasıyla devam edebilirsiniz.

## <a name="classify-your-sql-database"></a>SQL veritabanınız sınıflandırma

1. [Azure Portal](https://portal.azure.com) gidin.

2. Gidin **gelişmiş veri güvenliği** güvenlik başlığı, Azure SQL veritabanı bölmesinde. Gelişmiş veri güvenliği etkinleştirmek için tıklayın ve ardından **veri bulma & sınıflandırma (Önizleme)** kart.

   ![Bir veritabanı tarama](./media/sql-data-discovery-and-classification/data_classification.png)

3. **Genel bakış** sekmesi yalnızca belirli bir şema bölümleri, bilgi türleri ve etiketleri görüntülemek için filtreleyebilirsiniz sınıflandırılmış sütunlar ayrıntılı bir listesi dahil olmak üzere, veritabanının geçerli sınıflandırma durumu özetini içerir. Hiçbir sütun sınıflandırılmış henüz yapmadıysanız, [5. adıma](#step-5).

   ![Geçerli sınıflandırma durumu özeti](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Excel biçimindeki bir rapor indirmek için tıklayın **dışarı** penceresinin üst menü seçeneği.

   ![Excel'e Aktar](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5. <a id="step-5"></a>Verilerinizi sınıflandırmak başlatmak için tıklayın **sınıflandırma sekmesini** pencerenin üst kısmındaki.

    ![Veri sınıflandırma](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. Sınıflandırma altyapısının olası hassas verileri içeren sütunlar için veritabanınızı tarar ve bir listesini sağlar **sütun sınıflandırmaları önerilen**. Sınıflandırma önerileri uygulayın ve görüntülemek için:

   - Pencerenin alt kısmındaki önerileri panelde önerilen sütun sınıflandırmaları listesini görüntülemek için tıklayın:

      ![Veri sınıflandırma](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

   - Belirli bir sütun için bir öneri kabul edin, ilgili satır sol sütunda onay için öneriler – listesini gözden geçirin. Ayrıca işaretleyebilirsiniz *tüm önerileri* önerileri tablo üstbilgisinde onay kutusunu işaretleyerek kabul edildi olarak.

       ![Gözden geçirme öneri listesi](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Seçili önerileri uygulamak için üzerinde mavi tıklayın **seçili önerileri kabul** düğmesi.

      ![Önerileri uygulama](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. Ayrıca **el ile sınıflandırma** sütunları alternatif olarak veya ek olarak, öneri tabanlı sınıflandırma için:

   - Tıklayarak **sınıflandırma Ekle** penceresinin üst menüdeki.

      ![El ile sınıflandırma Ekle](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

   - Açılan bağlam penceresinde şemasını seçin > Tablo > sınıflandırmak istediğiniz sütunu ve bilgi türü ve duyarlılık etiketi. Mavi üzerinde ardından **sınıflandırma Ekle** bağlam pencerenin alt kısmındaki düğmesi.

      ![Sınıflandırmak için sütun seçin](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Yeni sınıflandırma meta verilerle (etiketi) veritabanı sütunları kalıcı olarak etiket sınıflandırmanızı tamamlamak için tıklayın **Kaydet** penceresinin üst menüdeki.

   ![Kaydet](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Hassas verilere erişimi denetleme

Bilgi koruma paradigma önemli bir yönüdür hassas verilere erişimi izlemek için yeteneğidir. [Azure SQL veritabanı denetimi](sql-database-auditing.md) Denetim günlüğüne adlı yeni bir alan eklemek için Gelişmiş *data_sensitivity_information*, günlükleri tarafından döndürülen gerçek verilerin duyarlılık sınıflandırmaları (etiketler) Sorgu.

![Denetleme günlüğü](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Veri sınıflandırması T-SQL kullanarak yönetme

Sütun sınıflandırmaları Ekle/Kaldır ek olarak, veritabanının tamamı için tüm sınıflandırmaları almak için T-SQL kullanabilirsiniz.

> [!NOTE]
> Etiketleri yönetmek için T-SQL kullanarak, eklenen bir sütuna kuruluş bilgilerini koruma İlkesi (portal önerileri görünen etiket kümesi) mevcut olan doğrulama yoktur. Bu nedenle bu doğrulamak için size bağlıdır.

- Sınıflandırma bir veya daha fazla sütun Ekle/Güncelleştir: [DUYARLIK SINIFLANDIRMASI EKLEYİN](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Sınıflandırma, bir veya daha fazla sütunlarından kaldırın: [DUYARLIK SINIFLANDIRMASI BIRAK](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Tüm sınıflandırmalar veritabanında görüntüleyin: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

Sınıflandırmaları programlı olarak yönetmek için REST API de kullanabilirsiniz. Yayımlanan REST API'leri, aşağıdaki işlemleri destekler:

- [Oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate) - oluşturur veya belirli bir sütunun duyarlılık etiketi güncelleştirir
- [Silme](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete) -belirli bir sütunun duyarlılık etiketi siler
- [Alma](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get) -belirli bir sütunun duyarlılık etiketi alır
- [Liste tarafından geçerli veritabanı](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase) -belirli bir veritabanı geçerli duyarlılık etiketlerini alır
- [Veritabanı tarafından önerilen listesinde](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) -belirli bir veritabanı önerilen duyarlılık etiketlerini alır

## <a name="manage-data-discovery-and-classification-using-azure-powershell"></a>Veri bulma ve sınıflandırma Azure PowerShell kullanarak yönetme

Bir Azure SQL veritabanı yönetilen örneği, önerilen tüm sütunları almak için PowerShell kullanabilirsiniz.

### <a name="powershell-cmdlets-for-azure-sql-database"></a>Azure SQL veritabanı için PowerShell cmdlet'leri

- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)

### <a name="powershell-cmdlets-for-managed-instance"></a>Yönetilen örnek için PowerShell cmdlet'leri

- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)

## <a name="permissions"></a>İzinler

Aşağıdaki yerleşik rolleri bir Azure SQL veritabanına veri sınıflandırmasını okuyabilirsiniz: `Owner`, `Reader`, `Contributor`, `SQL Security Manager` ve `User Access Administrator`.

Azure SQL veritabanına veri sınıflandırmasını aşağıdaki yerleşik rolleri değiştirebilirsiniz: `Owner`, `Contributor`, `SQL Security Manager`.

Daha fazla bilgi edinin [Azure kaynakları için RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

## <a id="subheading-5"></a>Sonraki adımlar

- Daha fazla bilgi edinin [gelişmiş veri güvenliği](sql-database-advanced-data-security.md).
- Yapılandırmayı göz önünde bulundurun [Azure SQL veritabanı denetimi](sql-database-auditing.md) izleme ve, sınıflandırılmış hassas verilere erişimi denetleme.

<!--Anchors-->
[SQL data discovery & classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Automated/Programmatic classification]: #subheading-4
[Next Steps]: #subheading-5
