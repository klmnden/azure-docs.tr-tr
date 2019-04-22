---
title: Jupyter Not Defteri, azure'da bir slayt gösterisi olarak sunmak
description: Slayt gösterisi modu hücre bir Jupyter not defteri yapılandırın ve ardından ortaya çıkmasına neden uzantısını kullanarak slayt gösterisi sunmak nasıl.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: c372175b-beb5-4b45-b2f8-34cb06990117
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 405fe71676de311ed7e59ea72798ff4fd2db0f62
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59280477"
---
# <a name="run-a-notebook-slideshow"></a>Bir not defteri slayt gösterisi çalıştırın

Azure not defterleri ile Jupyter/Ipython slayt gösterisi uzantısı (bir slayt gösterisi olarak doğrudan bir not defteri göstermenize olanak sağlayan HACMİNDEKİ) önceden yapılandırılmış. Bir slayt gösterisi hücreleri genellikle görüntülenen teker teker büyük ekranlar ve size sunmak kod hala çalıştırabilirsiniz için uygun bir yazı tipi boyutu yerine ayrı bir tanıtım bilgisayara geçiş ' dir.

Aşağıdaki görüntüde, Markdown ve kod hücreleri tümünü bir araya görebileceğiniz standart not defteri görünümü gösterilmektedir:

![Standart görünümünde bir not defteri](media/slideshow/slideshow-notebook-view.png)

Slayt gösterisi başlatmak, ilk hücrenin tarayıcı dolduracak şekilde genişletilir burada **X** slayt gösterisi, sol üst çıkar **?** alt sol görüntüler slaytlar arasında klavye kısayolları ve sağ alt köşesinde oklar gidin:

![Slayt gösterisi modunda bir not defteri](media/slideshow/slideshow-slide-view.png)

Bir not defteri için slayt gösterisi hazırlama birincil iki etkinlik içerir:

1. Markdown hücreleri büyük yazı tipleriyle işlendiği için bazı içerikleri slayt gösterisi olarak görünmeyebilir. Bu nedenle genellikle belirli bir hücre metinde miktarı sınırı, genellikle dört ile altı çizgili bir üst bilgi en iyi şekilde çalışır. Daha fazla metin varsa, bu bilgileri birden çok hücreye bölün.

2. Her bir hücresinde davranışını slayt gösterisi hücresi araç çubuğunu kullanarak slayt gösterisi olarak yapılandırın. Gezinti düğmelerinin davranışını hücresi türlerini belirleyin.

## <a name="the-anatomy-of-a-slideshow"></a>Slayt gösterisi anatomisi

Rastgele bir not alın ve bunu kullanmak için bir slayt gösterisi, tüm hücreleri birlikte önlenmiş ve içeriğin tarayıcı penceresinin alt kısmındaki gizli genellikle bulun. Etkili bir sunum yapmak, daha sonra bir slayt gösterisi türü slayt gösterisi hücresi araç çubuğunu kullanarak her hücreye atamanız gerekir:

1. Üzerinde **görünümü** menüsünde **hücresi araç** > **slayt gösterisi**:

    ![Hücre slayt gösterisi araç kapatma](media/slideshow/slideshow-view-cell-toolbar.png)

1. A **slayt türü** sağ üst kısmındaki her hücreye not defterindeki açılan bulunur:

    ![Hücre slayt gösterisi araç çubuğu](media/slideshow/slideshow-cell-toolbar.png)

1. Her bir hücresinde beş türlerinden birini seçin:

    ![Hücre slayt gösterisi türleri](media/slideshow/slideshow-cell-slide-types.png)

    | Slayt türü | Davranış |
    | --- | --- |
    | -(ayarlanmamış) | Hücre sık istenen bir slayt gösterisi etkili değildir önceki hücrenin görüntülenir. |
    | Slayt | Hücre Gezinti denetimin sol ve sağ okları kullanarak çıkıldığında birincil bir slayttır. |
    | Alt slayt | "Below" Gezinti denetimi aşağı okunu kullanarak gittiğinizde, birincil bir slayt hücre var. Yukarı Ok birincil slayta döndürür. Alt slaytları bir sunu ana yolunda Atla, ancak gerekirse kullanıma hazır ikincil malzeme için kullanılır. |
    | Parça | Hücre içeriğini önceki slayt veya alt slayt bağlamında (bir parça yukarı ok kullanırken kaldırılır) gezinti oku kullanılırken görünür. Birden çok parçaya (sonraki bölümde örneğe bakın) tek bir metin madde işaretleri görünür yapmak için kullanabilirsiniz veya bir parça, kod içinde bir slayt görünür hale getirmek için bir kod hücresi ile kullanabilirsiniz. Geçerli slaytta parçaları oluşturmak için aşırı parçaları tarayıcı penceresinin alt kısmındaki devre dışı görünür olmayacaktır. |
    | Atla | Hücre slayt gösterisi olarak gösterilmez. |
    | Notlar | Slayt gösterisi olarak gösterilmez Konuşmacı notları, hücre içerir. |

1. Başlangıçta seçilecek yardımcı **slayt** her hücre için. Ardından, slayt gösterisi çalıştırın ve uygun ayarlamaları yapın.

### <a name="example-fragment-cells-for-bullet-items"></a>Örnek: parça hücreleri maddesi öğeleri için

Tek bir yerde Markdown hücrede slayt üstbilgi görünür madde işaretleri slayttaki yapmak **slayt** yazın, sonra ayrı bir Markdown hücre her madde işareti koyun **parça** türü:

![Birden fazla Markdown hücre maddesi öğeleri için oluşturma örneği](media/slideshow/slideshow-fragments.png)

Slayt gösterisi madde işaretleri aynı hücrede olduğunda, daha fazla dikey daha aralığı ile parçalarını işler çünkü çok madde işaretlerini kullanmanız mümkün olmayabilir.

## <a name="run-the-slideshow"></a>Slayt gösterisi çalıştırın

1. Markdown hücreler düzenlenebilir, aksi durumda görünürler, HTML işlemek için bunları çalıştırmaya karar emin *olarak* slayt gösterisi Markdown'da.

1. Yapılandırdıktan sonra **slayt türü** her hücre için slayt gösterisi başlatmak ve ardından istediğiniz içeren hücreyi seçin **Enter/çıkış YÜKSELMEYE slayt gösterisi** ana araç çubuğunda:

    ![Ana araç çubuğunda girin/çıkış YÜKSELMEYE slayt gösterisi](media/slideshow/slideshow-start.png)

1. Parçaları yanı sıra slaytları arasında gezinmek için sol ve sağ ok Gezinti denetimi olarak kullanın. Denetimdeki metin temsil eden bir sayıyı gösteren *slide.sub slayt*.

    ![Slayt gösterisi gezinme kontrolü](media/slideshow/slideshow-navigation-control.png)

1. Slayt ve alt slaytları yanı arasında parçalar gezinmek için yukarı ve etkinleştirilirse, oklar aşağı:

    ![Slayt gösterisi gezinti denetimlerinin alt dilimleyiciler için](media/slideshow/slideshow-navigation-control-subslide.png)

1. Kodu hücreyi temel kodu çalıştırmak için Yürüt düğmesini kullanın; Çıktı, slaytta görünür:

    ![Kod hücresini çalıştırmak için YÜRÜT düğmesine](media/slideshow/slideshow-run-code-cell.png)

    ![Kodu hücreyi çıkış slayt gösterisi görünür.](media/slideshow/slideshow-run-code-cell-output.png)

    > [!Tip]
    > Hücre çıkış bir slayt gösterisi hücresinde bir parçası olarak kabul edilir. Not Defteri veya Slayt Gösterisi Görünümü'nde bir hücre çalıştırırsanız, çıkış diğer görünümünde de görüntülenir. Çıkışı silmek için kullanın **hücre** > **geçerli çıkışları** > **Temizle** (geçerli hücreyi) komutunu veya **hücre**  >  **Tüm çıkışları** > **Temizle** (için tüm hücreleri).

1. Slayt gösterisi ile işiniz bittiğinde kullanın **X** not defteri görünümüne dönmek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: Yapılandırma ve projeleri yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: İçinde bir not defteri paketleri yükleme](install-packages-jupyter-notebook.md)
- [Nasıl yapılır: Veri dosyaları ile çalışma](work-with-project-data-files.md)
- [Nasıl yapılır: Veri kaynaklarına erişim](access-data-resources-jupyter-notebooks.md)
