---
title: İçeri aktarma ve Azure not defterleri ile projeleri ile verileri dışarı aktarma
description: Verileri bir Azure not defterleri projeye dış kaynaklardan hale getirme ve bir proje verileri dışarı aktarma.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 586b423b-6668-4bdd-9592-4c237d7458fb
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: b522b0bd641d0147518843b11be4cd3a1430ae20
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59265347"
---
# <a name="work-with-data-files-in-azure-notebook-projects"></a>Azure not defteri projeleri veri dosyalarıyla çalışma

Birçok Jupyter not defterleri, özellikle veri bilimi için kullanılan not defterleri, lifeblood verilerdir. Azure not defterleri ile kolayca çeşitli kaynaklardan projesine içeri aktarın ve sonra bu verilerden bir not defterlerini kullanabilirsiniz. Daha sonra başka bir yerde kullanmak için indirebilirsiniz projesinde depolanan verileri üretme not defterleri de olabilir.

**Veri** içinde çalışan bir not defteri menü ayrıca sağlar **karşıya** ve **indirme** projeleri içindeki dosyaların yanı sıra geçici dosyalar için geçerli çalışan komutları Not Defteri oturumu.

Kod içinde bir not defteri çeşitli veri kaynaklarından doğrudan erişmek için bir proje içinde dosyaları dahil olmak üzere de kullanabilirsiniz. Rasgele verileri bir kod hücresine komutlarını kullanarak da erişebilirsiniz. Bu tür veriler, Not Defteri oturumunda değişkenlerinde depolandığından, özellikle proje dosyalarını oluşturmak için kod kullanmadığınız sürece projede kaydedilmez.

Bir çalışan Not Defteri içinde kendi veri kodla çalışma yaşadı en iyi: Bu amaç için başvurmak [verilerinize Azure not defterleri örnek not defterinde alma](https://notebooks.azure.com/Microsoft/projects/samples/html/Getting%20to%20your%20Data%20in%20Azure%20Notebooks.ipynb).

Bu makalenin geri kalanında proje düzeyi dosya işlemleri hakkında ayrıntılar sağlar.

## <a name="import-data"></a>Veri içeri aktarma

Proje panosunu kullanarak çalışan bir not defteri içinde veya bir projeye dosya getirebilirsiniz **veri** menü veya gibi bir komutun `curl`.

### <a name="import-files-from-the-project-dashboard"></a>Proje Panosu dosyalarından içeri aktarma

1. Proje dosyalarını içeri aktarmak istediğiniz klasöre gidin.

1. Seçin **karşıya** komutu, sonra da **URL'den** veya **bilgisayardan** ve gerekli bilgileri içeri aktarmak istediğiniz verilerin proje:

   - **URL'den**: Kaynak adresi girerek **dosya URL'si** alan ve projenizde not defterine atamak için dosya adı **dosya adı** alan. Ardından **+ Dosya Ekle** URL karşıya yükleme listesine eklenecek. Herhangi bir ek URL için işlemi tekrarlayın ve sonra seçin **Bitti**.

     ![URL açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-url-popup.png)

   - **Bilgisayardan**: Sürükleyin ve açılan dosyaları açılır ya da seçin **dosya seçin**, ardından gözatın ve içeri aktarmak istediğiniz veri dosyaları seçin. Bırakın veya kod dosyasını açın ve verileri ayrıştırmak için not defterinde olduğu için herhangi bir sayıda herhangi bir türü ve biçim dosyalarını seçin.

     ![Bilgisayar açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-computer-popup.png)

1. İçeri sonra dosyaları proje Panosu üzerinde görünür ve not defteri kodu içeren klasöre göreli yol adları kullanarak içinde erişilebilir.

### <a name="import-files-from-the-file-menu-in-a-notebook"></a>Bir not defteri Dosya menüsünden dosyalarını içeri aktarın

1. Çalışan bir not defteri içinde seçin **dosya** > **karşıya** komutu:

    ![Karşıya yükleme komutu içinde bir not defteri dosyası](media/file-menu-upload.png)

1. Açılan iletişim kutusuna gidin ve karşıya yüklemek istediğiniz dosyaları seçin. Herhangi bir sayıda tüm dosya türlerini seçebilirsiniz. Seçin **açık** işiniz bittiğinde.

1. İçinde **karşıya yükleme durumu** görüntülenirse, seçin açılan bir **hedef klasör** aşağı açılan listeden:

    - Oturum klasörü (*~/* ): Dosyaları geçerli not defteri oturuma yükler ancak projedeki dosyaları oluşturmaz. Oturum klasör proje klasörüne eşdüzeyde, ancak oturumu sona erdikten sonra kalmıyor. Kod, oturum dosyalara erişmek için göreli yolu içeren dosya adlarını önek *... /*.

        Oturum klasörü kullanılarak deneme için yararlıdır ve proje olabilir veya uzun vadeli temelinde gerekmeyebilir dosyalarla karışıklığı ortadan kaldırır. Çakışmalarına neden olmadan ve dosyaları yeniden adlandırmak gerek kalmadan proje dosyalarında aynı adlara sahip oturum klasöre dosyalar da karşıya yükleyebilirsiniz. Örneğin, bir sürümü deyin sahip *data.csv* projede zaten, ancak istediğiniz farklı bir sürümü ile denemeler *data.csv*. Oturum klasöre dosya yükleyerek karşıya yüklenen dosya verileri kullanarak not defterini çalıştırabilirsiniz (kod kullanarak kendisine başvuran *.. /Data.csv*) yerine projenin dosya verileri.

    - Proje klasörü (*/project*): dosyaları karşıya yükler, burada olabilir projeye kodda göreli yol adları kullanılarak erişilir. Bir dosyayı bu klasöre yüklemek proje panosundaki bir dosyayı karşıya yüklemeyi aynıdır. Dosyanın projeyle kaydedilir ve daha sonraki oturumlarda kullanılabilir.

        Zaten projede var olan bir aynı ada sahip bir dosyayı karşıya yüklemeyi denerseniz, başarısız karşıya yükleniyor. Bir dosyanın üzerine yazmak için üzerine yazma seçeneğini sunar Yeni Proje panosunu dosyasından bunun yerine, yükleyin.

1. Seçin **Başlat karşıya** tıklayarak işlemi tamamlar.

### <a name="create-or-import-files-using-commands"></a>Oluşturma veya içeri aktarma komutlarını kullanarak dosyaları

Hem proje hem de oturum klasörlerdeki dosyaları oluşturmak için bir Python kodu hücreyi veya bir terminal içinde komutlarını kullanabilirsiniz. Örneğin, komutları gibi `curl` ve `wget` dosyaları doğrudan Internet'ten indirin.

Terminalde dosyaları indirmek için seçin **Terminal** proje Panosu üzerinde komutunu ve ardından uygun komutları girin:

```bash
curl https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv

wget https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv
```

Bir Python kodu hücreyi bir not defteri kullanırken, komutlara önek `!`.

Böylece gibi bir hedef dosya adı belirtmek için varsayılan klasör proje klasördür *oil_price.csv* projede dosyası oluşturur. Oturumu dosyası oluşturmak için adıyla önek *... /* olarak *... /oil_price.csv*.

### <a name="create-files-in-code"></a>Kod içinde dosyaları oluşturma

Pandas gibi bir dosyaya oluşturan kodu kullanırken `write_csv` işlevi, yol adları her zaman proje klasörüyle. Kullanarak *... /* not defterini durdurulamaz ve atılır oturumu dosyası oluşturur.

## <a name="export-files"></a>Dosyaları dışarı aktarma

Proje panosunu veya içinden veri aktarabilirsiniz bir not defteri.

## <a name="export-files-from-the-project-dashboard"></a>Proje Panosu dosyalarından dışarı aktarma

Proje Panosu üzerinde bir dosyaya sağ tıklayıp **indirme**:

![Proje öğesi bağlam menüsü komutu indirin](media/download-command.png)

Ayrıca bir dosya seçin ve kullanmak **indirme** komut (klavye kısayolu: d) Panoda:

![Proje Panosu üzerinde araç çubuğu komutuna indirin](media/download-command-toolbar.png)

## <a name="export-files-from-the-data-menu-in-a-notebook"></a>Bir not defteri veri menüsünden dosyalarını Dışarı Aktar

1. Seçin **dosya** > **indirme** menü komutu:

    ![Veri yükleme komutu içinde bir not defteri](media/file-menu-download.png)

1. Açılan oturumda klasörleri gösteren görüntülenir; *proje* klasörü proje dosyalarını içerir:

    ![Veri yükleme komut dosyaları ve klasörleri seçin açılan menüsü](media/file-menu-download-popup.png)

1. İndirin ve ardından istediğiniz klasörleri ve dosyaları sol tarafındaki kutuları işaretleyin **indirme seçili**.

1. Tek bir not defteri hazırlar *.zip* , ardından olarak normal şekilde kaydedin seçili dosyalarını içeren dosya tarayıcınızdan yapın. Not defteri oluşturur bir *.zip* tek bir dosyayı indirdiğinizde dosya.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir not defteri bulut verilerine erişim](access-data-resources-jupyter-notebooks.md)
