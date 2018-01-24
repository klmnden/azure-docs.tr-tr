---
title: "Veri Fabrikası Azure Kopyalama Sihirbazı'nı | Microsoft Docs"
description: "İç havuzlar için desteklenen veri kaynaklarından alınan verileri kopyalamak için veri fabrikası Azure Kopyalama Sihirbazı'nı kullanma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: d9f3fea0db5a08fc91d9e4dc525b48575c512634
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory-copy-wizard"></a>Azure Data Factory Kopyalama Sihirbazı
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. 

Azure Data Factory Kopyalama Sihirbazı'nı, genellikle bir uçtan uca veri tümleştirme senaryosunun ilk adımda veri alma işlemini kolaylaştırır. Azure veri fabrikası Kopyalama Sihirbazı'nı geçerken bağlı hizmetler, veri kümeleri ve işlem hatları için herhangi bir JSON tanımları anlayın gerekmez. Sihirbaz otomatik olarak seçilen hedef seçilen veri kaynağından verileri kopyalamak için bir işlem hattı oluşturur. Ayrıca, kopyalama Sihirbazı, geliştirme sırasında alınan verileri doğrulamak için yardımcı olur. Bu süre, tasarruf özellikle zaman, alma veri ilk kez veri kaynağından. Kopyalama Sihirbazı'nı başlatmak için tıklatın **veri kopyalama** döşeme veri fabrikanızın giriş sayfasında.

![Kopyalama Sihirbazı](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>Büyük veriler için tasarlanmış
Bu sihirbaz kolayca veriler çeşitli kaynaklardan hedeflere dakika cinsinden taşımanızı sağlar. Sihirbazı kullanarak olduktan sonra kopyalama etkinliği ile işlem hattı otomatik olarak sizin için bağımlı Data Factory varlıklarını birlikte (bağlı hizmetler ve veri kümeleri) oluşturulur. Ardışık düzen oluşturmak için hiçbir ek adımlar gereklidir.   

![Veri kaynağı seç](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Bir Azure SQL veritabanı tablosuna blob Azure verileri kopyalamak için örnek bir işlem hattı oluşturmak adım adım yönergeler için bkz: [Kopyalama Sihirbazı'nı Öğreticisi](data-factory-copy-data-wizard-tutorial.md).
>
>

Sihirbazı çeşitli veri ve nesne türleri için destek ile başlangıç aklınızda büyük veri tasarlanmıştır. Klasörleri, dosyaları ya da tablo yüzlerce taşıma Data Factory işlem hatlarını yazabilirsiniz. Sihirbaz, otomatik veri Önizleme, şema yakalama ve eşleme ve verileri filtreleme destekler.

## <a name="automatic-data-preview"></a>Otomatik veri Önizleme
Kopyalamak istediğiniz verileri olup olmadığını doğrulamak için seçilen veri kaynağından alınan verilerin bir kısmını önizleyebilirsiniz. Ayrıca, veri kaynağını bir metin dosyasında ise, kopyalama Sihirbazı satır ve sütun sınırlayıcıları ve şema otomatik olarak bilgi edinmek için metin dosyası ayrıştırır.

![Dosya biçimi ayarları](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Şema yakalama ve eşleme
Giriş veri şeması ve bazı durumlarda çıktı verilerini şeması eşleşmeyebilir. Bu senaryoda, hedef şemanın sütunlarından kaynak şemasından sütun eşleşmesi gerekir.

> [!TIP]
> Tablo hedef deposunda mevcut değilse veri SQL Server veya Azure SQL veritabanından Azure SQL Data Warehouse'a kopyalarken, veri fabrikası kaynağının şemasını kullanarak Otomatik Tablo oluşturma destekler. ' Dan daha fazla bilgi edinin [veri taşıma Azure Data Factory kullanarak Azure SQL veri ambarından](./data-factory-azure-sql-data-warehouse-connector.md).
>

Hedef şemanın bir sütunda eşlemek için kaynak şemasından sütun seçmek için aşağı açılan listesini kullanın. Sütun eşlemesi için desen anlamak Kopyalama Sihirbazı'nı çalışır. Böylece her birini ayrı ayrı şema eşleme tamamlamak için sütunlar seçin gerekmez sütunlara geri kalanı için aynı düzeni uygular. Tercih ederseniz, tek tek sütunları eşlemek için açılan listeleri kullanarak bu eşlemeler geçersiz kılabilirsiniz. Desen daha fazla sütun eşlemesi olarak daha doğru olur. Kopyalama Sihirbazı'nı sürekli düzeni güncelleştirir ve sonuçta elde etmek istediğiniz sütun eşlemesi için doğru deseni ulaştığında.     

![Şema eşleme](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Verileri filtreleme
Havuz veri deposuna kopyalanması gereken verileri seçmek için kaynak verilerini filtreleyebilirsiniz. Filtreleme havuz veri deposuna kopyalanacak veri hacmini azaltır ve bu nedenle kopyalama işlemi verimliliğini artırır. SQL sorgu dili kullanarak ilişkisel bir veritabanındaki verilere filtre esnek bir şekilde sağlar veya kullanarak bir Azure blob klasördeki dosyaları [Data Factory işlevleri ve değişkenler](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Bir veritabanında veri filtreleme
Aşağıdaki ekran görüntüsü, bir SQL sorgusunu kullanarak gösterir `Text.Format` işlevi ve `WindowStart` değişkeni.

![İfadeler doğrula](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Bir Azure blob klasöründe veri filtreleme
Temel çalışma zamanında belirlenir bir klasörden veri kopyalamak için klasör yoluna değişkenleri kullanabilirsiniz [sistem değişkenleri](data-factory-functions-variables.md#data-factory-system-variables). Değişkenleri desteklenir: **{year}**, **{month}**, **{day}**, **{saat}**, **{dakika}**, ve **{özel}**. Örneğin: inputfolder / {year} / {month} / {day}.

Klasörleri aşağıdaki biçimde giriş varsayın:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Tıklatın **Gözat** için düğmesini **dosya veya klasör**, bu klasörlerden birine göz atın (örneğin, 2016 03 -> -> 01 02 ->), tıklatıp **Seç**. Görmeniz gerekir `2016/03/01/02` metin kutusuna. Şimdi değiştir **2016** ile **{year}**, **03** ile **{month}**, **01** ile **{day}** , ve **02** ile **{saat}**ve basın **sekmesini** anahtarı. Bu dört değişkenleri biçimini seçmek için aşağı açılır listeler görmeniz gerekir:

![Sistem değişkenleri kullanma](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Aşağıdaki ekran görüntüsünde gösterildiği gibi ayrıca kullanabileceğiniz bir **özel** değişkeni ve [biçim dizeleri desteklenen](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Bu yapıya sahip bir klasör seçmek için kullanın **Gözat** ilk düğme. Bir değerle değiştirin **{özel}**ve basın **sekmesini** biçim dizesi yazabileceğiniz metin kutusu görmek için anahtar.     

![Özel değişken kullanma](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Zamanlama Seçenekleri
Kopyalama işlemi bir kez veya bir zamanlamaya göre çalıştırabilirsiniz (saatlik, günlük, vb.). Bu seçeneklerin ikisi de şirket içi ve bulut yerel Masaüstü kopya çeşitli ortamlar genelinde bağlayıcılar derecesini için kullanılabilir.

Bir kerelik kopyalama işlemi yalnızca bir kez bir hedefe bir kaynaktan veri taşıma sağlar. Herhangi bir boyuta ve desteklenen bir biçim verileri için geçerlidir. Zamanlanmış kopyalama, önceden belirlenmiş bir yineleme verileri kopyalamanıza olanak sağlar. Zamanlanmış kopyalama yapılandırmak için zengin ayarlarınızı (örneğin, yeniden deneme zaman aşımı ve uyarılar) kullanabilirsiniz.

![Zamanlama özellikleri](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği ile işlem hattı oluşturmak için Data Factory Kopyalama Sihirbazı'nı kullanarak hızlı kılavuz için bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).
