---
title: Azure SQL veritabanı verileri bulma & sınıflandırma | Microsoft Docs
description: Azure SQL veritabanı verileri bulma & sınıflandırma
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 01/29/2018
ms.author: giladm
ms.openlocfilehash: 375142b0e55c741e6ab914e969751833f989d2fb
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL veritabanı verileri bulma ve sınıflandırma
Veri bulma & sınıflandırma (şu anda önizlemede) Azure SQL veritabanı için yerleşik Gelişmiş özellikleri sağlar **keşfetme**, **sınıflandırma**, **etiketleme**  &  **koruma** veritabanlarınızı hassas verileri.
Keşfetmek ve en hassas verilerinizi sınıflandırmak (iş, finansal, sağlık hizmeti, PII, vb.), Kurumsal bilgilerin koruma stature bir bileşendirler rol oynayabilir. Altyapı olarak hizmet verebilir:
* Veri gizliliği standartlarını ve GDPR gibi yasal uyumluluk gereksinimlerini karşılayacak yardımcı olur.
* (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimin uyarma.
* Erişimi denetlemek ve yüksek oranda gizli verileri içeren veritabanları güvenlik sağlamlaştırma.

> [!NOTE]
> Bu belge, Azure SQL veritabanına yalnızca ilişkilendirir. SQL Server (şirket içi) için bkz: [SQL veri bulma ve sınıflandırma](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Genel bakış
Veri bulma & sınıflandırma yalnızca veritabanı verileri korumaya yönelik yeni bir SQL Information Protection kip oluşturan bir dizi Gelişmiş Hizmetleri ve yeni SQL özellikleri sunar:
* **Bulma & önerileri** – sınıflandırma altyapısı veritabanınızı tarar ve olası hassas verileri içeren sütunlarını tanımlar. Bunun ardından, gözden geçirin ve uygun sınıflandırma önerileri Azure Portalı aracılığıyla uygulamak için kolay bir yol sağlar.
* **Etiketleme** – duyarlılık sınıflandırma etiketlerini kalıcı olarak etiketlenmiş SQL motoruna sunulan yeni sınıflandırma meta veri özniteliklerini kullanarak sütunlarda. Bu meta veriler daha sonra Gelişmiş duyarlılık tabanlı denetim ve koruma senaryoları için kullanılabilir.
* **Sorgu sonuç kümesi duyarlılık** – denetim amacıyla için gerçek zamanlı sorgu sonucu kümesi duyarlılığını hesaplanır.
* **Görünürlük** -veritabanı sınıflandırma durumu ayrıntılı bir portal Panosunda görüntülenebilir. Ayrıca, uyumluluk ve denetleme amaçları, yanı sıra diğer gereksinimlerini kullanılacak bir raporu (Excel biçiminde) yükleyebilirsiniz.

## <a id="subheading-2"></a>Bulma, sınıflandırmak ve hassas sütunları etiketleme
Aşağıdaki bölümde bulmak, sınıflandırmak ve veritabanı yanı sıra, veritabanınızı geçerli sınıflandırma durumunu görüntüleme ve raporları verme hassas veri içeren sütunlar etiketleme adımlarını açıklar.

Sınıflandırma iki meta veri özniteliklerini içerir:
* Sütunda depolanan veri hassasiyet düzeyini tanımlamak için etiketleri – ana sınıflandırma öznitelikler kullanılır.  
* Bilgi türleri – sütunda depolanan verilerin türünü içine ek ayrıntı düzeyi sağlar.

<br>
**SQL veritabanınız sınıflandırmak için:**

1. [Azure Portal](https://portal.azure.com) gidin.

2. Gidin **veri bulma & sınıflandırma (Önizleme)** SQL veritabanınızdaki ayarlama.

    ![Gezinti bölmesi][1]

3. **Genel bakış** sekmesi yalnızca belirli şema bölümleri, bilgi türleri ve etiketleri görüntülemek için filtreleyebilirsiniz sınıflandırılmış sütun ayrıntılı bir listesi dahil olmak üzere bu veritabanının geçerli sınıflandırma durumu özetini içerir. Herhangi bir sütundan sınıflandırılmış henüz yapmadıysanız, [5. adıma atlayın](#step-5).

    ![Gezinti bölmesi][2]

4. Bir raporu Excel biçiminde indirmek için tıklayın **verme** penceresinin üst menü seçeneği.

    ![Gezinti bölmesi][3]

5.  <a id="step-5"></a>Verilerinizi sınıflandırmak başlatmak için tıklatın **sınıflandırma sekmesini** pencerenin üstündeki.

    ![Gezinti bölmesi][4]

6. Sınıflandırma altyapısı veritabanınız olası hassas verileri içeren sütunlar için tarar ve bir listesini sağlar **sütun sınıflandırmaları önerilen**. Görüntülemek ve sınıflandırma önerileri uygulamak için:

    * Pencerenin altındaki önerileri panelindeki önerilen sütun sınıflandırmaları listesini görüntülemek için tıklatın:

        ![Gezinti bölmesi][5]

    * Belirli bir sütun için bir öneri kabul etmek için ilgili satır sol sütunda onay kutusunu işaretleyin önerileri – listesini gözden geçirin. Ayrıca işaretleyebilirsiniz *tüm önerilere* önerileri tablo üstbilgisinde onay kutusunu işaretleyerek kabul gibi.

        ![Gezinti bölmesi][6]

    * Seçili önerileri uygulamak için üzerinde mavi tıklatın **kabul seçili önerileri** düğmesi.

        ![Gezinti bölmesi][7]

7. Ayrıca **el ile sınıflandırma** sütunları alternatif olarak veya ek olarak, öneri tabanlı sınıflandırma için:

    * Tıklayın **sınıflandırma eklemek** penceresinin üst menüde.

        ![Gezinti bölmesi][8]

    * Şemayı açan bağlam penceresinde seçin > Tablo > sınıflandırmak istediğiniz sütun ve bilgi türü ve duyarlılık etiketi. Üzerinde mavi ardından **sınıflandırma eklemek** düğmesi bağlam pencerenin altındaki.

        ![Gezinti bölmesi][9]

8. Sınıflandırma tamamlamak ve kalıcı olarak (etiketi) veritabanı sütunlarla yeni sınıflandırma meta veri etiketi için tıklayın **kaydetmek** penceresinin üst menüde.

    ![Gezinti bölmesi][10]

## <a id="subheading-3"></a>Hassas bilgilere erişimi denetleme

Bir önemli bilgi koruma kip hassas bilgilere erişimi izleme yeteneği yönüdür.

[Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) Denetim günlüğüne adlı yeni bir alan eklemek için Gelişmiş *data_sensitivity_information*, tarafından döndürülen gerçek veri duyarlılık sınıflandırmaları (etiketleri) günlüğe kaydeder Sorgu.

![Gezinti bölmesi][11]

## <a id="subheading-4"></a>Sonraki adımlar
Yapılandırmayı göz önünde bulundurun [Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) izleme ve, sınıflandırılmış hassas bilgilere erişimi denetleme.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Next Steps]: #subheading-4

<!--Image references-->
[1]: ./media/sql-data-discovery-and-classification/1_data_classification_settings_menu.png
[2]: ./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png
[3]: ./media/sql-data-discovery-and-classification/3_data_classification_export_report.png
[4]: ./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png
[5]: ./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png
[6]: ./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png
[7]: ./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png
[8]: ./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png
[9]: ./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png
[10]: ./media/sql-data-discovery-and-classification/10_data_classification_save.png
[11]: ./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png
