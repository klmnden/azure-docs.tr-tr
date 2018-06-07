---
title: Azure SQL veritabanı verileri bulma & sınıflandırma | Microsoft Docs
description: Azure SQL veritabanı verileri bulma & sınıflandırma
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: giladm
ms.openlocfilehash: 673286c8dc9ec688199fe80cf5a763f249192de5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646788"
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL veritabanı verileri bulma ve sınıflandırma
Veri bulma & sınıflandırma (şu anda önizlemede) Azure SQL veritabanı için yerleşik Gelişmiş özellikleri sağlar **keşfetme**, **sınıflandırma**, **etiketleme**  &  **koruma** veritabanlarınızı hassas verileri.
Keşfetmek ve en hassas verilerinizi sınıflandırmak (iş, finansal, sağlık hizmeti, PII, vb.), Kurumsal bilgilerin koruma stature bir bileşendirler rol oynayabilir. Altyapı olarak hizmet verebilir:
* Veri gizliliği standartlarını ve düzenleyici uyumluluk gereksinimleri karşılayan yardımcı olur.
* (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimin uyarma.
* Erişimi denetlemek ve yüksek oranda gizli verileri içeren veritabanları güvenlik sağlamlaştırma.

Veri bulma & sınıflandırma parçasıdır [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir (ATP) teklifi. Veri bulma & sınıflandırma erişilen ve merkezi SQL ATP portal üzerinden yönetilir.

> [!NOTE]
> Bu belge, Azure SQL veritabanına yalnızca ilişkilendirir. SQL Server (şirket içi) için bkz: [SQL veri bulma ve sınıflandırma](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Veri bulma ve sınıflandırma nedir?
Veri bulma & sınıflandırma yalnızca veritabanı verileri korumaya yönelik yeni bir SQL Information Protection kip oluşturan bir dizi Gelişmiş Hizmetleri ve yeni SQL özellikleri sunar:
* **Bulma & önerileri** – sınıflandırma altyapısı veritabanınızı tarar ve olası hassas verileri içeren sütunlarını tanımlar. Bunun ardından, gözden geçirin ve uygun sınıflandırma önerileri Azure Portalı aracılığıyla uygulamak için kolay bir yol sağlar.
* **Etiketleme** – duyarlılık sınıflandırma etiketlerini kalıcı olarak etiketlenmiş SQL motoruna sunulan yeni sınıflandırma meta veri özniteliklerini kullanarak sütunlarda. Bu meta veriler daha sonra Gelişmiş duyarlılık tabanlı denetim ve koruma senaryoları için kullanılabilir.
* **Sorgu sonuç kümesi duyarlılık** – denetim amacıyla için gerçek zamanlı sorgu sonucu kümesi duyarlılığını hesaplanır.
* **Görünürlük** -veritabanı sınıflandırma durumu ayrıntılı bir portal Panosunda görüntülenebilir. Ayrıca, uyumluluk ve denetleme amaçları, yanı sıra diğer gereksinimlerini kullanılacak bir raporu (Excel biçiminde) yükleyebilirsiniz.

## <a id="subheading-2"></a>Bulmak, sınıflandırmak ve hassas sütunları etiket
Aşağıdaki bölümde bulmak, sınıflandırmak ve veritabanı yanı sıra, veritabanınızı geçerli sınıflandırma durumunu görüntüleme ve raporları verme hassas veri içeren sütunlar etiketleme adımlarını açıklar.

Sınıflandırma iki meta veri özniteliklerini içerir:
* Sütunda depolanan veri hassasiyet düzeyini tanımlamak için etiketleri – ana sınıflandırma öznitelikler kullanılır.  
* Bilgi türleri – sütunda depolanan verilerin türünü içine ek ayrıntı düzeyi sağlar.

## <a name="classify-your-sql-database"></a>SQL veritabanınız sınıflandırın

1. [Azure Portal](https://portal.azure.com) gidin.

2. Gidin **Advanced Threat Protection** , Azure SQL veritabanı bölmesinde güvenlik başlığı altında. Advanced Threat Protection etkinleştirmek için tıklayın ve ardından üzerinde **veri bulma & sınıflandırma (Önizleme)** kart.

   ![Bir veritabanı tarama](./media/sql-data-discovery-and-classification/data_classification.png) 

3. **Genel bakış** sekmesi yalnızca belirli şema bölümleri, bilgi türleri ve etiketleri görüntülemek için filtreleyebilirsiniz sınıflandırılmış sütun ayrıntılı bir listesi dahil olmak üzere bu veritabanının geçerli sınıflandırma durumu özetini içerir. Herhangi bir sütundan sınıflandırılmış henüz yapmadıysanız, [5. adıma atlayın](#step-5).

   ![Geçerli sınıflandırma durumu özeti](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png) 

4. Bir raporu Excel biçiminde indirmek için tıklayın **verme** penceresinin üst menü seçeneği.

   ![Excel'e Aktar](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png) 

5.  <a id="step-5"></a>Verilerinizi sınıflandırmak başlatmak için tıklatın **sınıflandırma sekmesini** pencerenin üstündeki.

    ![Veri sınıflandırma](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png) 

6. Sınıflandırma altyapısı veritabanınız olası hassas verileri içeren sütunlar için tarar ve bir listesini sağlar **sütun sınıflandırmaları önerilen**. Görüntülemek ve sınıflandırma önerileri uygulamak için:

    * Pencerenin altındaki önerileri panelindeki önerilen sütun sınıflandırmaları listesini görüntülemek için tıklatın:
    
      ![Verilerinizi sınıflandırmak](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png) 

    * Belirli bir sütun için bir öneri kabul etmek için ilgili satır sol sütunda onay kutusunu işaretleyin önerileri – listesini gözden geçirin. Ayrıca işaretleyebilirsiniz *tüm önerilere* önerileri tablo üstbilgisinde onay kutusunu işaretleyerek kabul gibi.

       ![Gözden geçirme öneri listesi](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png) 

    * Seçili önerileri uygulamak için üzerinde mavi tıklatın **kabul seçili önerileri** düğmesi.

      ![Önerileri geçerlidir](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png) 

7. Ayrıca **el ile sınıflandırma** sütunları alternatif olarak veya ek olarak, öneri tabanlı sınıflandırma için:

    * Tıklayın **sınıflandırma eklemek** penceresinin üst menüde.
  
      ![Sınıflandırma el ile Ekle](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png) 

    * Şemayı açan bağlam penceresinde seçin > Tablo > sınıflandırmak istediğiniz sütun ve bilgi türü ve duyarlılık etiketi. Üzerinde mavi ardından **sınıflandırma eklemek** düğmesi bağlam pencerenin altındaki.

      ![Sınıflandırmak için sütun seçin](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png) 

8. Sınıflandırma tamamlamak ve kalıcı olarak (etiketi) veritabanı sütunlarla yeni sınıflandırma meta veri etiketi için tıklayın **kaydetmek** penceresinin üst menüde.

   ![Kaydet](./media/sql-data-discovery-and-classification/10_data_classification_save.png) 

## <a id="subheading-3"></a>Hassas bilgilere erişimi denetleme

Bir önemli bilgi koruma kip hassas bilgilere erişimi izleme yeteneği yönüdür. [Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) Denetim günlüğüne adlı yeni bir alan eklemek için Gelişmiş *data_sensitivity_information*, tarafından döndürülen gerçek veri duyarlılık sınıflandırmaları (etiketleri) günlüğe kaydeder Sorgu.

![Denetleme günlüğü](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png) 

## <a id="subheading-4"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [SQL Advanced Threat Protection](sql-advanced-threat-protection.md).
- Yapılandırmayı göz önünde bulundurun [Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) izleme ve, sınıflandırılmış hassas bilgilere erişimi denetleme.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Next Steps]: #subheading-4