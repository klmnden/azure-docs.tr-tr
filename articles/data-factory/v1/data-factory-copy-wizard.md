---
title: "Veri Kopyalama Sihirbazı - Azure ile kolayca kopyalama | Microsoft Docs"
description: "İç havuzlar için desteklenen veri kaynaklarından alınan verileri kopyalamak için Data Factory Kopyalama Sihirbazı'nı kullanma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: c993b1dfb0055da84751c042efccf42d943375d9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Kopyalama veya Azure Data Factory Kopyalama Sihirbazı ile kolayca veri taşıma
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Data Factory hizmetinin önizleme aşamasında olan 2. sürümünü kullanıyorsanız [sürüm 2’de kopyalama etkinliği öğreticisi belgeleri](../quickstart-create-data-factory-dot-net.md) konusunu inceleyin. 


Azure Data Factory Kopyalama Sihirbazı'nı, genellikle bir uçtan uca veri tümleştirme senaryosunun ilk adımda veri alma sürecini kolaylaştırmak için ' dir. Azure veri fabrikası Kopyalama Sihirbazı'nı geçerken bağlı hizmetler, veri kümeleri ve işlem hatları için herhangi bir JSON tanımları anlayın gerekmez. Ancak, Sihirbazı'ndaki tüm adımları tamamladıktan sonra sihirbaz otomatik olarak seçilen hedef seçilen veri kaynağından verileri kopyalamak için bir işlem hattı oluşturur. Ayrıca, kopyalama Sihirbazı'nı zamanınızın çoğunu kaydeder, yazma, aynı anda alınan veri doğrulama yardımcı olacak özellikle zaman, alma veri ilk kez veri kaynağından. Kopyalama Sihirbazı'nı başlatmak için tıklatın **veri kopyalama** döşeme veri fabrikanızın giriş sayfasında.

![Kopyalama Sihirbazı](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Veri kopyalama için sezgisel bir Sihirbazı
Bu sihirbaz kolayca veriler çeşitli kaynaklardan hedeflere dakika cinsinden taşımanızı sağlar. Sihirbazı kullanarak geçtikten sonra kopyalama etkinliği ile işlem hattı otomatik olarak bağımlı Data Factory varlıklarını birlikte (bağlı hizmetler ve veri kümeleri) oluşturulur. Ardışık düzen oluşturmak için hiçbir ek adımlar gereklidir.   

![Veri kaynağı seç](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Bkz: [Kopyalama Sihirbazı'nı öğretici](data-factory-copy-data-wizard-tutorial.md) kopyalamak için örnek bir işlem hattı oluşturmak adım adım yönergeler için makalenin verileri Azure blob için bir Azure SQL veritabanı tablosu. 
> 
> 

Sihirbazın başından aklınızda büyük veri tasarlanmıştır. Bu basit ve klasörleri, dosyaları ya da veri Kopyala Sihirbazı'nı kullanarak tabloları yüzlerce taşıma Data Factory işlem hatlarını yazmak için etkili olur. Sihirbaz aşağıdaki üç özellikleri destekler: otomatik veri Önizleme, şema yakalama ve eşleme ve verileri filtreleme. 

## <a name="automatic-data-preview"></a>Otomatik veri Önizleme
Kopyalama Sihirbazı, isteğe bağlı olarak veri kopyalamak istediğiniz doğru verileri olup olmadığını doğrulamak seçilen veri kaynağından alınan verilerin bir kısmını gözden geçirmenizi sağlar. Ayrıca, veri kaynağını bir metin dosyasında ise, kopyalama Sihirbazı'nı satır ve sütun sınırlayıcıları ve şema otomatik olarak bilgi edinmek için metin dosyası ayrıştırır. 

![Dosya biçimi ayarları](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Şema yakalama ve eşleme
Giriş veri şeması ve bazı durumlarda çıktı verilerini şeması eşleşmeyebilir. Bu senaryoda, hedef şemanın sütunlarından kaynak şemasından sütun eşleşmesi gerekir. 

Kopyalama Sihirbazı'nı kaynağı şemasını sütunlarında hedef şemanın sütunlara otomatik olarak eşlenir. Eşlemeleri açılan listeleri kullanarak geçersiz kılabilir (veya) bir sütun veriler kopyalanırken atlanması gerekip gerekmediğini belirtin.   

![Şema eşleme](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Verileri filtreleme
Sihirbaz, yalnızca hedef/havuz veri deposuna kopyalanması gereken verileri seçmek için kaynak verilerini filtrelemek sağlar. Filtreleme havuz veri deposuna kopyalanacak veri hacmini azaltır ve bu nedenle kopyalama işlemi verimliliğini artırır. SQL sorgu dili (veya) dosyaları bir Azure blob klasörüne kullanarak ilişkisel bir veritabanındaki verilere filtre esnek bir şekilde sağlar [Data Factory işlevleri ve değişkenler](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Bir veritabanında veri filtreleme
Örnekte, SQL sorgusu kullanan `Text.Format` işlevi ve `WindowStart` değişkeni. 

![İfadeler doğrula](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Bir Azure blob klasöründe veri filtreleme
Temel çalışma zamanında belirlenir bir klasörden veri kopyalamak için klasör yoluna değişkenleri kullanabilirsiniz [sistem değişkenleri](data-factory-functions-variables.md#data-factory-system-variables). Değişkenleri desteklenir: **{year}**, **{month}**, **{day}**, **{saat}**, **{dakika}**, ve **{özel}**. Örnek: inputfolder / {year} / {month} / {day}.

Klasörleri aşağıdaki biçimde giriş varsayın:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Tıklatın **Gözat** için düğmesini **dosya veya klasör**, bu klasörlerden birine göz atın (örneğin, 2016 03 -> -> 01 02 ->), tıklatıp **Seç**. Görmeniz gerekir `2016/03/01/02` metin kutusuna. Şimdi değiştir **2016** ile **{year}**, **03** ile **{month}**, **01** ile **{day}**, ve **02** ile **{saat}**, SEKME tuşuna basın. Bu dört değişkenleri biçimini seçmek için aşağı açılır listeler görmeniz gerekir:

![Sistem değişkenleri kullanma](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Aşağıdaki ekran görüntüsünde gösterildiği gibi ayrıca kullanabileceğiniz bir **özel** değişkeni ve [biçim dizeleri desteklenen](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Bu yapıya sahip bir klasör seçmek için kullanın **Gözat** ilk düğme. Bir değerle değiştirin **{özel}**, biçim dizesi yazabileceğiniz metin kutusu görmek için SEKME tuşuna basın.     

![Özel değişken kullanma](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>Çeşitli veri ve nesne türleri için destek
Kopyalama Sihirbazı'nı kullanarak, klasörleri, dosyaları ya da tablo yüzlerce verimli bir şekilde taşıyabilirsiniz.

![İçinden verileri kopyalamak tabloları seçin](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Zamanlama Seçenekleri
Kopyalama işlemi bir kez veya bir zamanlamaya göre çalıştırabilirsiniz (saatlik, günlük, vb.). Bu seçeneklerin ikisi de bağlayıcıları çıkarmak için şirket içi, Bulut ve yerel Masaüstü kopya kullanılabilir.

Bir kerelik kopyalama işlemi yalnızca bir kez bir hedefe bir kaynaktan veri taşıma sağlar. Herhangi bir boyuta ve desteklenen bir biçim verileri için geçerlidir. Zamanlanmış kopyalama, önceden belirlenmiş bir yineleme verileri kopyalamanıza olanak sağlar. Zamanlanmış kopyalama yapılandırmak için zengin ayarlarınızı (örneğin, yeniden deneme zaman aşımı ve uyarılar) kullanabilirsiniz.

![Zamanlama özellikleri](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği ile işlem hattı oluşturmak için Data Factory Kopyalama Sihirbazı'nı kullanarak hızlı kılavuz için bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

