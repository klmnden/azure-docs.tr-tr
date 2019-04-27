---
title: Data Factory Azure Kopyalama Sihirbazı | Microsoft Docs
description: Data Factory Azure Kopyalama Sihirbazı havuzlarından desteklenen veri kaynaklarından alınan verileri kopyalamak için nasıl kullanılacağı hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d13e304b0d10e8bd34d306426f1f9164bcc6be94
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60567682"
---
# <a name="azure-data-factory-copy-wizard"></a>Azure Data Factory Kopyalama Sihirbazı
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

Azure Data Factory Kopyalama Sihirbazı, genellikle bir ilk adım bir uçtan uca veri tümleştirme senaryosunu veri almak işlemi kolaylaştırır. Azure Data Factory Kopyalama Sihirbazı giderken, bağlı hizmetler, veri kümeleri ve işlem hatları için herhangi bir JSON tanımı anlamak gerekmez. Sihirbaz seçilen hedefte seçili veri kaynağından veri kopyalamak üzere işlem hattına otomatik olarak oluşturur. Ayrıca, kopyalama Sihirbazı'nı, yazma sırasında alınıyor verileri doğrulamak için yardımcı olur. Bu yöntem, zaman kazandırır özellikle zaman, başlayan kümeniz verileri ilk kez veri kaynağından. Kopyalama Sihirbazı'nı başlatmak için tıklatın **veri kopyalama** kutucuğuna veri fabrikanızın giriş sayfasında.

![Kopyalama Sihirbazı](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>Büyük veriler için tasarlanmış
Bu sihirbaz, verileri bir geniş çeşitli kaynaklardan hedeflere dakikalar içinde kolayca taşımak sağlar. Sihirbazda olduktan sonra bir kopyalama etkinlikli bir işlem hattı, bağımlı Data Factory varlıklarını yanı sıra (bağlı hizmetler ve veri kümeleri) oluşturulur. İşlem hattını oluşturmak için ek adımlar gerekir.   

![Veri kaynağı seçme](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Bir Azure SQL veritabanı tablosuna blob bir Azure blobundaki verileri kopyalamak için bir örnek işlem hattı oluşturmak adım adım yönergeler için bkz: [Kopyalama Sihirbazı'nı öğretici](data-factory-copy-data-wizard-tutorial.md).
>
>

Sihirbaz, çeşitli veri ve nesne türleri için destek ile başlangıç aklınızda büyük verilerle tasarlanmıştır. Klasörleri, dosyaları ya da tabloları yüzlerce taşıma Data Factory işlem hatları yazabilirsiniz. Sihirbaz otomatik veri Önizleme, şema yakalama ve eşleme ve verileri filtreleme destekler.

## <a name="automatic-data-preview"></a>Otomatik veri önizlemesi
Kopyalamak istediğiniz verileri olup olmadığını doğrulamak için seçilen veri kaynağındaki verilerin bir kısmını önizleyebilirsiniz. Ayrıca, kaynak verileri bir metin dosyasına ise, şema ve satır ve sütun sınırlayıcıları otomatik olarak bilgi edinmek için metin dosyası kopyalama Sihirbazı'nı ayrıştırır.

![Dosya biçimi ayarları](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Şema yakalama ve eşleme
Giriş verilerinin şemasını, bazı durumlarda çıktı veri şeması eşleşmeyebilir. Bu senaryoda, hedef şemanın sütunları için kaynak şemasından sütunları eşlemeniz gerekir.

> [!TIP]
> Tablo hedef depoda mevcut değilse data SQL Server veya Azure SQL veritabanından Azure SQL veri ambarı'na kopyalama yapılırken, Data Factory kullanarak kaynağı şemasındaki otomatik tablo oluşturulmasını destekler. Daha fazla bilgi [gelen Azure Data Factory kullanarak Azure SQL veri ambarı ve veri taşıma](./data-factory-azure-sql-data-warehouse-connector.md).
>

Kaynak şemayı hedef şemanın bir sütunda eşlemek için bir sütun seçmek için aşağı açılan listesini kullanın. Sütun eşlemesi için desen anlamak Kopyalama Sihirbazı'nı çalışır. Böylece, her biri ayrı ayrı şema eşleme tamamlamak için sütunları seçmek gerekmez sütunların geri kalanı için aynı düzeni uygular. Tercih ederseniz, tek tek sütunlara eşlemek için açılır listeleri kullanarak bu eşlemelerin üzerine yazabilir. Desen, daha fazla sütun eşleme daha doğru olur. Kopyalama Sihirbazı'nı sürekli deseni güncelleştirir ve sonuçta elde etmek istediğiniz sütun eşlemesi için doğru deseni ulaşır.     

![Şema eşleme](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Verileri filtreleme
Havuz veri deposuna kopyalanacak gereken verileri seçmek için kaynak verilerini filtreleyebilirsiniz. Filtreleme, havuz veri deposuna kopyalanacak verileri hacmini azaltır ve bu nedenle kopyalama işleminin aktarım hızını geliştirir. SQL sorgu dili kullanarak esnek bir şekilde ilişkisel bir veritabanındaki verilere filtre uygulamak sağlar veya bir Azure blob klasördeki dosyaları kullanarak [Data Factory işlevleri ve değişkenler](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Bir veritabanında veri filtreleme
Bir SQL sorgusunu kullanarak aşağıdaki ekran görüntüsünde gösterilmektedir `Text.Format` işlevi ve `WindowStart` değişkeni.

![Doğrulama ifadeleri](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Bir Azure blob klasörü veri filtreleme
Temel çalışma zamanında belirlenir bir klasöre veri kopyalamak için klasör yoluna değişkenleri kullanabilirsiniz [sistem değişkenlerini](data-factory-functions-variables.md#data-factory-system-variables). Desteklenen değişkenler: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}** ve **{Özel}**. Örneğin: inputfolder / {year} / {month} / {day}.

Klasörleri aşağıdaki biçimde giriş varsayalım:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Tıklayın **Gözat** için düğme **dosya veya klasör**, bu klasörlerden birine göz atın (örneğin, 2016 03 -> -> 01 -> 02), tıklatıp **Seç**. Görmelisiniz `2016/03/01/02` metin kutusuna. Şimdi değiştir **2016** ile **{year}**, **03** ile **{month}**, **01** ile **{day}** , ve **02** ile **{hour}** basın **sekmesini** anahtarı. Bu dört değişkenler biçimini seçmek için aşağı açılır listeler görmeniz gerekir:

![Sistem değişkenlerini kullanma](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Aşağıdaki ekran görüntüsünde gösterildiği gibi ayrıca kullanabileceğiniz bir **özel** değişkeni ve [biçim dizeleri desteklenen](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Bu yapıya sahip bir klasör seçmek için kullanın **Gözat** ilk düğme. Ardından bir değerle değiştirin **{özel}** basın **sekmesini** biçim dizesi girebileceğiniz metin kutusunu görmek için anahtar.     

![Özel değişken kullanma](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Zamanlama Seçenekleri
Kopyalama işleminden sonra veya bir zamanlamaya göre çalıştırabilirsiniz (saatlik, günlük, vb.). Bu iki seçenek avantajlarına yerel Masaüstü kopya şirket içi ve bulut dahil olmak üzere ortamlarında, bağlayıcı için kullanılabilir.

Bir kerelik kopyalama işlemi, yalnızca bir kez bir kaynaktan bir hedef veri taşınmasını sağlar. Her boyutta ve desteklenen bir biçim verilere uygulanır. Zamanlanmış kopyalama önceden belirlenmiş bir yinelenme verileri kopyalamanızı sağlar. Zamanlanmış kopyalama yapılandırmak için zengin ayarları (örneğin, yeniden deneme zaman aşımı ve uyarılar) kullanabilirsiniz.

![Zamanlama özellikleri](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği ile işlem hattı oluşturmak için Data Factory Kopyalama Sihirbazı'nı kullanarak hızlı kılavuz için bkz. [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).
