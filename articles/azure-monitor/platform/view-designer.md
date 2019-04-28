---
title: Azure İzleyici'de günlük verilerini analiz etmek için görünümler oluşturun | Microsoft Docs
description: Azure İzleyici'de görünüm Tasarımcısını kullanarak, Azure portalında görüntülenir ve görselleştirmeler Log Analytics çalışma alanındaki veriler üzerinde çeşitli içeren özel görünümlerinizi oluşturabilirsiniz. Bu makalede, Görünüm Tasarımcısı için genel bir bakış içerir ve özel görünümleri düzenleme ve oluşturma için yordamlar sunar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: f07fc2f03ad72e7ee0fd408782b8fe845c88e780
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342133"
---
# <a name="create-custom-views-by-using-view-designer-in-azure-monitor"></a>Azure İzleyici'de Görünüm Tasarımcısı kullanarak özel görünümlerini oluşturma
Azure İzleyici'de görünüm Tasarımcısını kullanarak, Azure portalında Log Analytics çalışma alanınızdaki veri görselleştirmenize yardımcı olabilecek çeşitli özel görünümler oluşturabilirsiniz. Bu makalede, Görünüm Tasarımcısı ve yordamlar oluşturmak ve özel görünümler düzenlemek için genel bir bakış sunar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Kutucuk başvurusu](view-designer-tiles.md): Kendi özel görünümlerinizi de kullanılabilir kutucukların her biri için ayarları için bir başvuru kılavuzu sağlar.
* [Görselleştirme bölümü başvurusu](view-designer-parts.md): Bir başvuru kılavuzu için özel görünümlerinizi kullanılabilir görselleştirme bölümleri ayarlarına sağlar.


## <a name="concepts"></a>Kavramlar
Görünümler, Azure İzleyici'de görüntülenir **genel bakış** Azure portalında sayfası. Bu sayfadan açın **Azure İzleyici** tıklayarak menü **daha fazla** altında **Insights** bölümü. Her özel görünüm kutucuklarda alfabetik olarak görüntülenir ve izleme çözümleri için kutucuklar yüklü aynı çalışma.

![Genel Bakış sayfası](media/view-designer/overview-page.png)

Görünüm Tasarımcısı ile oluşturduğunuz görünümleri, aşağıdaki tabloda açıklanan öğeleri içerir:

| Bölümü | Açıklama |
|:--- |:--- |
| Kutucuklar | , Azure İzleyici görüntülenen **genel bakış** sayfası. Her kutucuk, temsil ettiği özel görünüm görsel bir özetini görüntüler. Her kutucuk türüne kayıtlarınız için farklı bir görselleştirme sağlar. Özel bir görünüm için bir kutucuk seçin. |
| Özel görünüm | Bir kutucuğu seçtiğinizde görüntülenir. Her görünümü bir veya daha fazla görselleştirme bölümü içerir. |
| Görselleştirme bölümü | Bir veya daha fazla bağlı Log Analytics çalışma alanındaki veri görselleştirme sunmak [oturum sorguları](../log-query/log-query-overview.md). Çoğu bölümleri, üst düzey bir görselleştirme sağlar, bir üst bilgi ve en çok rastlanan sonuçlar görüntüler listesini içerir. Her bölüm türü kayıtlarının Log Analytics çalışma alanındaki farklı bir görselleştirme sağlar. Ayrıntılı kayıtlar sağlayan bir günlük sorgusu gerçekleştirmeye bölümünde öğeleri seçin. |

## <a name="required-permissions"></a>Gerekli izinler
En az ihtiyaç duyduğunuz [katkıda bulunan düzeyinde izinler](manage-access.md#manage-accounts-and-users) oluşturmak veya görünümleri değiştirmek için Log Analytics çalışma alanında. Bu izne sahip değilseniz, Görünüm Tasarımcısı seçeneği menüde görüntülenmez.


## <a name="work-with-an-existing-view"></a>Var olan bir görünümü ile çalışma
Görünüm Tasarımcısı ile oluşturulan görünümleri, aşağıdaki seçenekleri görüntüler:

![Genel Bakış menüsü](media/view-designer/overview-menu.png)

Seçenekler aşağıdaki tabloda açıklanmıştır:

| Seçenek | Açıklama |
|:--|:--|
| Yenile   | En son verileri Görünümü yeniler. | 
| Günlükler      | Açılır [Log Analytics](../log-query/portals.md) günlük sorguları ile verileri analiz etmek için. |
| Düzenle       | Görünümün içeriğini ve yapılandırmasını düzenlemek için görünümü Tasarımcısı'nda açılır.  |
| Kopyala      | Yeni bir görünüm oluşturur ve Görünüm Tasarımcısı'nda açılır. Yeni Görünüm adını özgün adıyla, ancak ile aynıdır *kopyalama* eklenmiş. |
| Tarih aralığı | Tarih ve saat aralığı filtresi Görünümü'nde bulunan veriler için ayarlayın. Bu tarih aralığı görünümü sorgularda kümesindeki herhangi bir tarih aralıkları önce uygulanır.  |
| +          | Tanımlanan özel bir filtre görünüm için tanımlar. |


## <a name="create-a-new-view"></a>Yeni bir görünüm oluşturma
Görünüm Tasarımcısı'nda seçerek yeni bir görünüm oluşturabilirsiniz **Görünüm Tasarımcısı** Log Analytics çalışma alanınızın menüsünde.

![Görünüm Tasarımcısı kutucuğu](media/view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Görünüm Tasarımcısı ile çalışma
Görünüm Tasarımcısı yeni görünümler oluşturmak veya varolanları düzenlemek için kullanın. 

Görünüm Tasarımcısı üç bölme vardır: 
* **Tasarım**: Oluşturma veya düzenleme kullandığınız özel görünümü içerir. 
* **Denetimleri**: Kutucukları ve eklediğiniz bölümleri içeren **tasarım** bölmesi. 
* **Özellikler**: Seçilen parçaları ve kutucuklar özelliklerini görüntüler.

![Görünüm Tasarımcısı](media/view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>Görünümü kutucuğu yapılandırın
Özel görünüm yalnızca tek bir kutucuk olabilir. Geçerli kutucuğu görüntüleme veya alternatif bir seçmek için Seç **döşeme** sekmesinde **denetimi** bölmesi. **Özellikleri** döşeme özellikleri bölmesinde görüntülenir. 

Bilgileri göre kutucuk özelliklerini yapılandırabileceğiniz [döşeme başvuru](view-designer-tiles.md) ve ardından **Uygula** değişiklikleri kaydetmek için.

### <a name="configure-the-visualization-parts"></a>Görselleştirme bölümlerini yapılandırma
Bir görünüm herhangi bir sayıda görselleştirme bölümleri içerebilir. Bölümleri görünümüne eklemek için seçin **görünümü** sekmesine ve ardından bir görselleştirme bölümü seçin. **Özellikleri** bölmesi seçili bölümü özelliklerini görüntüler. 

Bilgileri göre görüntüle özelliklerini yapılandırabileceğiniz [görselleştirme bölümü başvurusu](view-designer-parts.md) ve ardından **Uygula** değişiklikleri kaydedin.

Görünümlerde görselleştirme bölümlerinin yalnızca bir satır bulunur. Yeni bir konuma sürükleyerek varolan parçalarını yeniden düzenleyebilirsiniz.

Seçerek bir görselleştirme bölümü görünümden kaldırabilirsiniz **X** üst bölümü'nün sağ.


### <a name="menu-options"></a>Menü Seçenekleri
Görünümleri düzenleme modunda çalışmaya yönelik seçenekleri aşağıdaki tabloda açıklanmıştır.

![Düzenle menüsü](media/view-designer/edit-menu.png)

| Seçenek | Açıklama |
|:--|:--|
| Kaydet        | Yaptığınız değişiklikler kaydedilir ve görünüm kapatır. |
| İptal      | Yaptığınız değişiklikleri atar ve görünüm kapatır. |
| Görünümü Sil | Görünüm siler. |
| Dışarı Aktarma      | Görünümü verir bir [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-authoring-templates.md) , başka bir çalışma alanına içeri aktarabilirsiniz. Görünümün adını dosya adıdır ve bunun bir *omsview* uzantısı. |
| İçeri Aktarma      | İçeri aktarmalar *omsview* başka bir çalışma alanından dışarı aktardığınız dosya. Bu eylem, mevcut görünüm yapılandırmasını üzerine yazar. |
| Kopyala       | Yeni bir görünüm oluşturur ve Görünüm Tasarımcısı'nda açılır. Yeni Görünüm adını özgün adıyla, ancak ile aynıdır *kopyalama* eklenmiş. |

## <a name="next-steps"></a>Sonraki adımlar
* Ekleme [kutucukları](view-designer-tiles.md) , özel bir görünüm.
* Ekleme [görselleştirme bölümleri](view-designer-parts.md) , özel bir görünüm.
