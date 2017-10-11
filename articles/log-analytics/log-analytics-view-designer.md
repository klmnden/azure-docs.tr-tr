---
title: "OMS günlük analizi verileri çözümlemek üzere görünümler oluşturma | Microsoft Docs"
description: "Görünüm Tasarımcısı'nda günlük analizi OMS ve Azure portalında gösterilen özel görünümler oluşturmanıza olanak sağlar ve farklı görsel OMS depo veri içerir. Bu makale, Görünüm Tasarımcısı ve yordamları oluşturmak ve özel görünümler düzenlemek için genel bir bakış içerir."
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
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: e3c463d749dc4179df58286b9bb75584880a6bc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Günlük analizi özel görünümler oluşturmak için Görünüm Tasarımcısı kullanın
Görünüm Tasarımcısı'nda [günlük analizi](log-analytics-overview.md) OMS deposundaki verileri farklı görselleştirmesini içeren OMS konsolunda özel görünümler oluşturmanıza olanak sağlar. Bu makale, Görünüm Tasarımcısı ve yordamları oluşturmak ve özel görünümler düzenlemek için genel bir bakış içerir.

Görünüm Tasarımcısı için kullanılabilir diğer makaleler şunlardır:

* [Döşeme başvuru](log-analytics-view-designer-tiles.md) -her özel görünümlerde kullanılabilir döşeme ayarlarını başvuru.
* [Görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md) -her özel görünümlerde kullanılabilir döşeme ayarlarını başvuru.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), tüm görünümleri sorgularda yazılmalıdır sonra [yeni sorgu dili](https://go.microsoft.com/fwlink/?linkid=856078).  Çalışma alanı yükseltilmeden önce oluşturulan görünümleri dönüştürülen automtically olacaktır.

## <a name="concepts"></a>Kavramlar
Görünüm Tasarımcısı ile oluşturulan görünümleri aşağıdaki tablodaki öğeler içerir.

| Bölümü | Açıklama |
|:--- |:--- |
| Döşeme |Ana günlük analizi genel bakış panosunda görüntülenir.  Bir görsel özel görünümde içerdiği bilgi özetlemeye içerir.  Döşeme farklı OMS depo kayıtlarının farklı görselleştirmeleri sağlar.  Özel Görünüm açmak için Kutucuğa tıklayın. |
| Özel Görünüm |Kullanıcı kutucuğu tıklattığında görüntülenir.  Bir veya daha fazla görselleştirme bölümleri içerir. |
| Görselleştirme bölümleri |OMS depo veri görselleştirme dayalı bir veya daha fazla [oturum aramaları](log-analytics-log-searches.md).  Çoğu bölümleri üst düzey bir görsel öğe sağlayan üstbilgi ve en iyi sonuç listesini içerir.  Farklı bölümü türleri OMS depo kayıtlarının farklı görselleştirmeleri sağlar.  Ayrıntılı kayıtları sağlayan bir günlük arama yapmak için bölümündeki öğelere tıklayın. |

![Görünüm Tasarımcısı genel bakış](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Sunucunuzdan çalışma alanınıza Görünüm Tasarımcısı Ekle
Görünüm Tasarımcısı Önizleme'de olsa da, çalışma alanınızı seçerek eklemelisiniz **Önizleme özellikleri** içinde **ayarları** OMS portalı bölümü.

![Önizlemeyi Etkinleştir](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Oluşturma ve görünümler düzenleme
### <a name="create-a-new-view"></a>Yeni bir görünüm oluşturma
Yeni bir görünüm açmak **Görünüm Tasarımcısı** ana OMS panosunda Görünüm Tasarımcısı kutucuğa tıklayarak.

![Görünüm Tasarımcısı döşeme](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Var olan bir görünümü düzenleme
Görünüm Tasarımcısı'nda var olan bir görünümü düzenlemek için görünümü döşemesinin ana OMS panosunda tıklayarak açın.  Ardından **Düzenle** görünümü Görünüm Tasarımcısı'nda açmak için düğmeye.

![Bir görünümü düzenleme](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Var olan bir görünümü kopyalama
Bir görünüm kopyaladığınızda yeni bir görünüm oluşturur ve Görünüm Tasarımcısı'nda açar.  Yeni görünümü ile orijinal olarak "onu sonuna eklenen Kopyala" aynı ada sahip olacaktır.  Bir görünüm kopyalamak için varolan bir görünümü döşemesinin ana OMS panosunda tıklayarak açın.  Ardından **kopya** görünümü Görünüm Tasarımcısı'nda açmak için düğmeye.

![Bir görünüm kopyalama](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Var olan bir görünümü Sil
Var olan bir görünümü silmek için görünümü döşemesinin ana OMS panosunda tıklayarak açın.  Ardından **Düzenle** görünümü Görünüm Tasarımcısı'nda açmak için düğmesine tıklayın ve **Görünümü Sil'i**.

![Bir görünümü Sil](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Var olan bir görünümü dışarı aktarma
Başka bir çalışma alanına alma veya kullanmak bir JSON dosyası için bir görünüm verebilirsiniz bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md).  Var olan bir görünümü vermek için ana OMS panosunda döşemesinin tıklayarak görünümünü açın.  Ardından **verme** tarayıcının indirme klasöründe bir dosya oluşturmak için düğmesi.  Dosya adı uzantısına sahip görünümün adı olacaktır *omsview*.

![Görünüm verme](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Var olan bir görünümü alma
Alabileceğiniz bir *omsview* başka bir yönetim grubundan dışarı aktardığınız dosya.  Var olan bir görünümü almak için önce yeni bir görünüm oluşturun.  Ardından **alma** düğmesine tıklayın ve ardından *omsview* dosya.  Yapılandırma dosyasındaki var olan görünüme kopyalanacak.

![Görünüm verme](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Görünüm Tasarımcısı ile çalışma
Görünüm Tasarımcısı üç bölmesi vardır.  **Tasarım** bölmesinde özel görünümünü temsil eder.  Kutucuklar ve parçalarını eklediğinizde **denetim** bölmesine **tasarım** bölmesi görünümüne eklenirler.  **Özellikleri** bölmesinde döşeme veya seçili bölümü özelliklerini görüntüler.

![Görünüm Tasarımcısı](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Görünümü kutucuğu yapılandırın
Özel bir görünüm yalnızca tek bir döşeme olabilir.  Seçin **döşeme** sekmesinde **denetim** bölmesinde geçerli kutucuğu görüntülemek veya alternatif bir tanesini seçin.  **Özellikleri** bölmesinde geçerli döşeme özelliklerini görüntüler.  Ayrıntılı bilgilere göre döşeme özelliklerini yapılandırmak [döşeme başvuru](log-analytics-view-designer-tiles.md) tıklatıp **Uygula** değişiklikleri kaydetmek için.

### <a name="configure-visualization-parts"></a>Görselleştirme bölümleri yapılandırma
Bir görünüm görselleştirme bölümlerinin herhangi bir sayı içerebilir.  Seçin **Görünüm** sekmesini ve ardından görünümüne eklemek için bir görselleştirme bölümü.  **Özellikleri** bölmesinde seçilen bölümü özelliklerini görüntüler.  Görünüm Özellikleri ayrıntılı bilgilere göre yapılandırma [görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md) tıklatıp **Uygula** değişiklikleri kaydetmek için.

### <a name="delete-a-visualization-part"></a>Görselleştirme bölümü silin
Tıklayarak görselleştirme bölümü görünümden kaldırabilirsiniz **X** bölümü sağ üst köşesinde düğmesini.

### <a name="rearrange-visualization-parts"></a>Görselleştirme bölümleri yeniden düzenleme
Görünümler yalnızca bir satır görselleştirme bölümlerinin gerekir.  Varolan bölümleri görünümünde tıklatarak ve sürükleyerek yeni bir konuma yeniden düzenleyin.

## <a name="next-steps"></a>Sonraki adımlar
* Ekleme [kutucukları](log-analytics-view-designer-tiles.md) özel görünüm.
* Ekleme [görselleştirme bölümleri](log-analytics-view-designer-parts.md) özel görünüm.
