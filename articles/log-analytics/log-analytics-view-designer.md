---
title: "Azure günlük analizi verileri çözümlemek üzere görünümler oluşturma | Microsoft Docs"
description: "Görünüm Tasarımcısı'nda günlük analizi, Azure portalında gösterilir ve günlük analizi çalışma alanındaki verilerin farklı görselleştirmeleri içeren özel görünümler oluşturmanıza olanak sağlar. Bu makale, Görünüm Tasarımcısı ve yordamları oluşturmak ve özel görünümler düzenlemek için genel bir bakış içerir."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: a84f40503c1b9778c496461ebbf6864f99bd1c4b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Günlük analizi özel görünümler oluşturmak için Görünüm Tasarımcısı kullanın
Görünüm Tasarımcısı'nda [günlük analizi](log-analytics-overview.md) günlük analizi çalışma alanınızdaki verilerin farklı görsel öğeleri içeren Azure portalında özel görünümler oluşturmanıza olanak sağlar. Bu makale, Görünüm Tasarımcısı ve yordamları oluşturmak ve özel görünümler düzenlemek için genel bir bakış içerir.

Görünüm Tasarımcısı için kullanılabilir diğer makaleler şunlardır:

* [Döşeme başvuru](log-analytics-view-designer-tiles.md) -her özel görünümlerde kullanılabilir döşeme ayarlarını başvuru.
* [Görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md) -her özel görünümlerde kullanılabilir döşeme ayarlarını başvuru.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), tüm görünümleri sorgularda yazılmalıdır sonra [yeni sorgu dili](https://go.microsoft.com/fwlink/?linkid=856078).  Çalışma alanı yükseltilmeden önce oluşturulan görünümleri dönüştürülen automtically olacaktır.

## <a name="concepts"></a>Kavramlar
Görünümler görüntülenir **genel bakış** Azure portalında günlük analizi çalışma sayfası.  Aynı çalışma alanı çözümleri yüklü her özel görünüm için döşeme kutucuk alfabetik olarak görüntülenir.

![Genel Bakış sayfası](media/log-analytics-view-designer/overview-page.png)

Görünüm Tasarımcısı ile oluşturulan görünümleri aşağıdaki tablodaki öğeler içerir.

| Bölümü | Açıklama |
|:--- |:--- |
| Kutucuk |Günlük analizi çalışma alanınız için genel bakış sayfasında görüntülenir.  Bir görsel özel görünümde içerdiği bilgi özetlemeye içerir.  Döşeme farklı kayıtların farklı görselleştirmeleri sağlar.  Özel Görünüm açmak için Kutucuğa tıklayın. |
| Özel Görünüm |Kullanıcı kutucuğu tıklattığında görüntülenir.  Bir veya daha fazla görselleştirme bölümleri içerir. |
| Görselleştirme bölümleri |Günlük analizi çalışma alanındaki veri görselleştirme dayalı bir veya daha fazla [oturum aramaları](log-analytics-log-searches.md).  Çoğu bölümleri üst düzey bir görsel öğe sağlayan üstbilgi ve en iyi sonuç listesini içerir.  Günlük analizi çalışma alanı kayıtlarında farklı görselleştirmesini farklı bölümü türler sağlar.  Ayrıntılı kayıtları sağlayan bir günlük arama yapmak için bölümündeki öğelere tıklayın. |


## <a name="work-with-an-existing-view"></a>Var olan bir görünümü ile çalışma
Görünüm Tasarımcısı ile oluşturulmuş bir görünümü açtığınızda, aşağıdaki tabloda bir menü seçenekleri ile sahip olur.

![Genel Bakış menüsü](media/log-analytics-view-designer/overview-menu.png)


| Seçenek | Açıklama |
|:--|:--|
| Yenile   | En son verilerle görünümü yenileyin. | 
| Analiz | Açık [Advanced Analytics portalı](log-analytics-log-search-portals.md#advanced-analytics-portal) günlük arama ile veri çözümleme için. () log-Analytics-log-Search-portals.MD#Advanced-Analytics-Portal). |
| Filtre    | Görünümde bulunan veriler için zaman filtresi ayarlayın. |
| Düzenle      | Görünümü içeriklerini ve yapılandırmasını düzenlemek için Görünüm Tasarımcısı'nda açın.   |
| Kopyala     | Yeni bir görünüm oluşturun ve Görünüm Tasarımcısı'nda açın.  Yeni görünümü ile orijinal olarak "onu sonuna eklenen Kopyala" aynı ada sahip. |


## <a name="create-a-new-view"></a>Yeni bir görünüm oluşturma
Yeni bir görünüm oluşturma **Görünüm Tasarımcısı** genel bakış sayfasında Azure Portalı'ndaki günlük analizi çalışma alanınızın Görünüm Tasarımcısı kutucuğuna tıklayarak.

![Görünüm Tasarımcısı döşeme](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="working-with-view-designer"></a>Görünüm Tasarımcısı ile çalışma
Yeni bir görünüm oluşturmayı ya da mevcut bir düzenleme görünümü Tasarımcısı ile çalışması.  

Görünüm Tasarımcısı üç bölmesi vardır.  **Tasarım** bölmesi, oluşturma veya düzenleme özel bir görünümü içerir.  Döşeme ve parçalarını ekleme **denetim** bölmesine **tasarım** bölmesi.  **Özellikleri** bölmesinde döşeme veya seçili bölümü özelliklerini görüntüler.

![Görünüm Tasarımcısı](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Görünümü kutucuğu yapılandırın
Özel bir görünüm yalnızca tek bir döşeme olabilir.  Seçin **döşeme** sekmesinde **denetim** bölmesinde geçerli kutucuğu görüntülemek veya alternatif bir tanesini seçin.  **Özellikleri** bölmesinde geçerli döşeme özelliklerini görüntüler.  Ayrıntılı bilgilere göre döşeme özelliklerini yapılandırmak [döşeme başvuru](log-analytics-view-designer-tiles.md) tıklatıp **Uygula** değişiklikleri kaydetmek için.

### <a name="configure-visualization-parts"></a>Görselleştirme bölümleri yapılandırma
Bir görünüm görselleştirme bölümlerinin herhangi bir sayı içerebilir.  Seçin **Görünüm** sekmesini ve ardından görünümüne eklemek için bir görselleştirme bölümü.  **Özellikleri** bölmesinde seçilen bölümü özelliklerini görüntüler.  Görünüm Özellikleri ayrıntılı bilgilere göre yapılandırma [görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md) tıklatıp **Uygula** değişiklikleri kaydetmek için.

Görünümler yalnızca bir satır görselleştirme bölümlerinin gerekir.  Varolan bölümleri görünümünde tıklatarak ve sürükleyerek yeni bir konuma yeniden düzenleyin.

Tıklayarak görselleştirme bölümü görünümden kaldırabilirsiniz **X** bölümü sağ üst köşesinde düğmesini.


### <a name="menu-options"></a>Menü Seçenekleri
Düzenleme modunda bir görünümü ile çalışırken, aşağıdaki tabloda menü seçeneğiniz vardır.

![Düzen menüsü](media/log-analytics-view-designer/edit-menu.png)

| Seçenek | Açıklama |
|:--|:--|
| Kaydet        | Değişikliklerinizi kaydetmek ve görünümü kapatın. |
| İptal      | Değişikliklerinizi atmak ve görünümü kapatın. |
| Görünümü Sil | Görünümü silin. |
| Dışarı Aktarma      | Görünüm verme bir [Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md) başka bir çalışma alanına alabilir.  Dosya adı uzantısına sahip görünümün adı olacaktır *omsview*. |
| İçeri Aktarma      | İçeri aktarma bir *omsview* başka bir çalışma alanından dışarı aktardığınız dosya.  Bu, varolan bir görünümü yapılandırmasını üzerine yazar. |
| Kopyala       | Yeni bir görünüm oluşturun ve Görünüm Tasarımcısı'nda açın.  Yeni görünümü ile orijinal olarak "onu sonuna eklenen Kopyala" aynı ada sahip. |
| Yayımlama     | İçine eklenen bir JSON dosyası görünümü vermek bir [Mangagement çözüm](../operations-management-suite/operations-management-suite-solutions-resources-views.md).  Dosya adı uzantısına sahip görünümün adı olacaktır *json*. İkinci bir dosya uzantısı ile oluşturulan *resjson* JSON dosyasında tanımlanan kaynaklara değerlerini içerir.

## <a name="next-steps"></a>Sonraki adımlar
* Ekleme [kutucukları](log-analytics-view-designer-tiles.md) özel görünüm.
* Ekleme [görselleştirme bölümleri](log-analytics-view-designer-parts.md) özel görünüm.
