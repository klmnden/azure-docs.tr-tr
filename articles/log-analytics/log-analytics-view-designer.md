---
title: Azure günlük analizi verileri çözümlemek üzere görünümler oluşturma | Microsoft Docs
description: Günlük analizi Görünüm Tasarımcısı kullanarak Azure portalında gösterilir ve veri görselleştirmeleri günlük analizi çalışma alanındaki çeşitli içeren özel görünümler oluşturabilirsiniz. Bu makalede oluşturma ve özel görünümler düzenleme için yordamlar sunar ve Görünüm Tasarımcısı genel bir bakış içerir.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 91d4efcd7fabc2f284078d752ea68778a9bd8d86
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752048"
---
# <a name="create-custom-views-by-using-view-designer-in-log-analytics"></a>Günlük analizi Görünüm Tasarımcısı kullanarak özel görünümlerini oluşturma
Görünüm Tasarımcısı'nda kullanarak [Azure günlük analizi](log-analytics-overview.md), Azure portalında veri günlük analizi çalışma alanınızdaki görselleştirmenize yardımcı olabilecek çeşitli özel görünümler oluşturabilirsiniz. Bu makalede Görünüm Tasarımcısı ve oluşturma ve özel görünümler düzenleme yordamları genel bakış sunar.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Döşeme başvuru](log-analytics-view-designer-tiles.md): başvuru kılavuzu için her biri kendi özel görünümlerinizi kullanılabilir döşemeleri ayarlar sağlar.
* [Görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md): başvuru kılavuzu için özel görünümlerde kullanılabilir görselleştirme bölümleri ayarlar sağlar.


## <a name="concepts"></a>Kavramlar
Görünümler görüntülenir **genel bakış** Azure portalında günlük analizi çalışma sayfası. Her özel görünüm döşemeleri alfabetik olarak görüntülenir ve çözümler için kutucukları yüklenen aynı çalışma alanı.

![Genel Bakış sayfası](media/log-analytics-view-designer/overview-page.png)

Görünüm Tasarımcısı ile oluşturduğunuz görünümler aşağıdaki tabloda açıklanan öğeleri içerir:

| Bölümü | Açıklama |
|:--- |:--- |
| Kutucuklar | Günlük analizi çalışma alanınız görüntülenen **genel bakış** sayfası. Her bölme temsil ettiği özel görünüm visual özetini görüntüler. Her döşeme türü farklı bir görsel öğe, kayıtların sağlar. Özel bir görünüm için bir kutucuk seçin. |
| Özel Görünüm | Bir kutucuk seçtiğinizde görüntülenir. Her görünüm bir veya daha fazla görselleştirme bölümleri içerir. |
| Görselleştirme bölümleri | Bir veya daha fazla göre günlük analizi çalışma alanındaki veri görselleştirme sunmak [oturum aramaları](log-analytics-log-searches.md). Çoğu bölümleri üst düzey bir görsel öğe sağlayan bir üstbilgi ve en iyi sonuç görüntüler listesini içerir. Her bölüm türü kayıtlar günlük analizi çalışma alanındaki farklı bir görsel öğe sağlar. Ayrıntılı kayıtları sağlayan günlük arama yapmaya bölümünde öğeleri seçin. |


## <a name="work-with-an-existing-view"></a>Var olan bir görünümü ile çalışma
Görünüm Tasarımcısı ile oluşturulan görünümleri aşağıdaki seçenekler görüntülenir:

![Genel Bakış menüsü](media/log-analytics-view-designer/overview-menu.png)

Seçenekler aşağıdaki tabloda açıklanmıştır:

| Seçenek | Açıklama |
|:--|:--|
| Yenile   | En son verilerle Görünümü yeniler. | 
| Analiz | Açılır [Advanced Analytics portalı](log-analytics-log-search-portals.md#advanced-analytics-portal) günlük arama ile veri çözümleme için. |
| Düzenle       | Görünüm içeriklerini ve yapılandırmasını düzenlemek için Görünüm Tasarımcısı'nda açar.  |
| Kopyala      | Yeni bir görünüm oluşturur ve Görünüm Tasarımcısı'nda açar. Yeni Görünüm adını özgün adına, ancak ile aynıdır *kopyalama* eklenmiş. |
| Tarih aralığı | Görünümde bulunan veriler için tarih ve saat aralığı filtre ayarlayın. |
| +          | Tanımlanmış bir özel filtre görünüm için tanımlayın. |


## <a name="create-a-new-view"></a>Yeni bir görünüm oluşturma
Seçerek Görünüm Tasarımcısı'nda yeni bir görünüm oluşturabilirsiniz **Görünüm Tasarımcısı** günlük analizi çalışma alanı menüsünde.

![Görünüm Tasarımcısı döşeme](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Görünüm Tasarımcısı ile çalışma
Görünüm Tasarımcısı yeni görünümler oluşturabilir veya var olanları düzenlemek için kullanın. 

Görünüm Tasarımcısı üç bölmesi vardır: 
* **Tasarım**: oluşturma veya düzenleme özel bir görünümü içerir. 
* **Denetimleri**: döşeme ve eklediğiniz bölümleri içeren **tasarım** bölmesi. 
* **Özellikleri**: döşeme veya seçilen parçaları özelliklerini görüntüler.

![Görünüm Tasarımcısı](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>Görünümü kutucuğu yapılandırın
Özel bir görünüm yalnızca tek bir döşeme olabilir. Geçerli kutucuğu görüntülemek veya alternatif bir seçmek için Seç **döşeme** sekmesinde **denetim** bölmesi. **Özellikleri** bölmesinde geçerli döşeme özelliklerini görüntüler. 

Bilgileri göre döşeme özelliklerini yapılandırabilirsiniz [döşeme başvuru](log-analytics-view-designer-tiles.md) ve ardından **Uygula** değişiklikleri kaydedin.

### <a name="configure-the-visualization-parts"></a>Görselleştirme bölümleri yapılandırma
Bir görünüm görselleştirme bölümlerinin herhangi bir sayı içerebilir. Bir görünüme bölümleri eklemek için seçin **Görünüm** sekmesini tıklatın ve ardından bir görselleştirme bölümü seçin. **Özellikleri** bölmesinde seçilen bölümü özelliklerini görüntüler. 

Görünüm Özellikleri bilgileri göre yapılandırabilirsiniz [görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md) ve ardından **Uygula** değişiklikleri kaydetmek için.

Görünümleri görselleştirme bölümlerinin yalnızca bir satır var. Yeni bir konuma sürükleyerek mevcut bölümleri düzenleyebilirsiniz.

Seçerek görünümden görselleştirme bölümünü kaldırabilirsiniz **X** üst sağ bölümünün.


### <a name="menu-options"></a>Menü Seçenekleri
Düzenleme modunda görünümleri ile çalışmak için seçenekler aşağıdaki tabloda açıklanmıştır.

![Düzen menüsü](media/log-analytics-view-designer/edit-menu.png)

| Seçenek | Açıklama |
|:--|:--|
| Kaydet        | Yaptığınız değişiklikleri kaydeder ve görünüm kapatır. |
| İptal      | Değişikliklerinizi atar ve görünüm kapatır. |
| Görünümü Sil | Görünümü siler. |
| Dışarı Aktarma      | Görünüm verir bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md) başka bir çalışma alanına alabilir. Dosyanın adını görünümü adıdır ve bunun bir *omsview* uzantısı. |
| İçeri Aktarma      | İçeri aktarmalar *omsview* başka bir çalışma alanından dışarı aktardığınız dosya. Bu eylem, varolan bir görünümü yapılandırmasını üzerine yazar. |
| Kopyala       | Yeni bir görünüm oluşturur ve Görünüm Tasarımcısı'nda açar. Yeni Görünüm adını özgün adına, ancak ile aynıdır *kopyalama* eklenmiş. |

## <a name="next-steps"></a>Sonraki adımlar
* Ekleme [kutucukları](log-analytics-view-designer-tiles.md) özel görünüm.
* Ekleme [görselleştirme bölümleri](log-analytics-view-designer-parts.md) özel görünüm.
