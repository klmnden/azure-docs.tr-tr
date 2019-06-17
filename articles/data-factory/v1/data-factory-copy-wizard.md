---
title: Veri Kopyalama Sihirbazı - Azure ile kolayca kopyalama | Microsoft Docs
description: Data Factory Kopyalama Sihirbazı'nı havuzlarından desteklenen veri kaynaklarından alınan verileri kopyalamak için nasıl kullanılacağı hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 90f78428601d7b039d00d39c1ca8339ab3ace9ba
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60487983"
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Azure Data Factory Kopyalama Sihirbazı ile bir kolayca veri taşıma veya kopyalama
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz. [kopyalama etkinliği öğreticisi](../quickstart-create-data-factory-dot-net.md). 


Azure Data Factory Kopyalama Sihirbazı, genellikle bir ilk adım bir uçtan uca veri tümleştirme senaryosunu veri almak işlemini kolaylaştırmak sağlamaktır. Azure Data Factory Kopyalama Sihirbazı giderken, bağlı hizmetler, veri kümeleri ve işlem hatları için herhangi bir JSON tanımı anlamak gerekmez. Ancak, sihirbazdaki tüm adımları tamamladıktan sonra sihirbaz otomatik olarak seçilen hedefte seçili veri kaynağından veri kopyalamak için bir işlem hattı oluşturur. Kopyalama Sihirbazı'nı Ayrıca, zamanınızın kaydeder, geliştirme sırasında alınıyor verileri doğrulamak için yardımcı özellikle zaman, başlayan kümeniz verileri ilk kez veri kaynağından. Kopyalama Sihirbazı'nı başlatmak için tıklatın **veri kopyalama** kutucuğuna veri fabrikanızın giriş sayfasında.

![Kopyalama Sihirbazı](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Veri kopyalama için sezgisel bir Sihirbazı
Bu sihirbaz, verileri bir geniş çeşitli kaynaklardan hedeflere dakikalar içinde kolayca taşımak sağlar. Sihirbazda filtrelemesinden geçtikten sonra bir kopyalama etkinlikli bir işlem hattı otomatik olarak sizin için bağımlı Data Factory varlıklarını yanı sıra (bağlı hizmetler ve veri kümeleri) oluşturulur. İşlem hattını oluşturmak için ek adımlar gerekir.   

![Veri kaynağı seçme](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Bkz: [Kopyalama Sihirbazı'nı öğretici](data-factory-copy-data-wizard-tutorial.md) makale kopyalamak için bir örnek işlem hattı oluşturmak adım adım yönergeler için verileri Azure blob için bir Azure SQL veritabanı tablosu. 
> 
> 

Sihirbaz, başlangıç aklınızda büyük verilerle tasarlanmıştır. Bu basit ve klasörleri, dosyaları ya da veri kopyalama Sihirbazı'nı kullanarak tablolar yüzlerce taşıma Data Factory işlem hatlarını yazmak için etkili olur. Sihirbaz, aşağıdaki üç özellikleri destekler: Otomatik veri Önizleme, şema yakalama ve eşleme ve verileri filtreleme. 

## <a name="automatic-data-preview"></a>Otomatik veri önizlemesi
Kopyalama Sihirbazı'nı, isteğe bağlı olarak verileri kopyalamak istediğiniz doğru veri olup olmadığını doğrulamak seçilen veri kaynağından alınan verilerin bir kısmını gözden geçirmenizi sağlar. Ayrıca, kaynak verileri bir metin dosyasına ise, satır ve sütun sınırlayıcıları ve şema otomatik olarak bilgi edinmek için metin dosyası kopyalama Sihirbazı'nı ayrıştırır. 

![Dosya biçimi ayarları](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Şema yakalama ve eşleme
Giriş verilerinin şemasını, bazı durumlarda çıktı veri şeması eşleşmeyebilir. Bu senaryoda, hedef şemanın sütunları için kaynak şemasından sütunları eşlemeniz gerekir. 

Kopyalama Sihirbazı'nı sütunları hedef şema kaynak şemasında sütunları otomatik olarak eşlenir. Eşlemeleri açılan listeleri kullanarak geçersiz kılabilir (veya) bir sütun verileri kopyalanırken atlanması gerekip gerekmediğini belirtin.   

![Şema eşleme](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Verileri filtreleme
Sihirbazı, hedef/havuz veri deposuna kopyalanacak gereken verileri seçmek için kaynak verileri filtrelemenize izin verir. Filtreleme, havuz veri deposuna kopyalanacak verileri hacmini azaltır ve bu nedenle kopyalama işleminin aktarım hızını geliştirir. SQL sorgu dili (veya) dosyalarını bir Azure blob klasörüne kullanarak esnek bir şekilde ilişkisel bir veritabanındaki verilere filtre uygulamak sağladığı [Data Factory işlevleri ve değişkenler](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Bir veritabanında veri filtreleme
Örnekte, SQL sorgusu kullanır `Text.Format` işlevi ve `WindowStart` değişkeni. 

![Doğrulama ifadeleri](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Bir Azure blob klasörü veri filtreleme
Temel çalışma zamanında belirlenir bir klasöre veri kopyalamak için klasör yoluna değişkenleri kullanabilirsiniz [sistem değişkenlerini](data-factory-functions-variables.md#data-factory-system-variables). Desteklenen değişkenler: **{year}** , **{month}** , **{day}** , **{hour}** , **{minute}** ve **{Özel}** . Örnek: inputfolder / {year} / {month} / {day}.

Klasörleri aşağıdaki biçimde giriş varsayalım:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Tıklayın **Gözat** için düğme **dosya veya klasör**, bu klasörlerden birine göz atın (örneğin, 2016 03 -> -> 01 -> 02), tıklatıp **Seç**. Görmelisiniz `2016/03/01/02` metin kutusuna. Şimdi değiştir **2016** ile **{year}** , **03** ile **{month}** , **01** ile **{day}** , ve **02** ile **{hour}** , SEKME tuşuna basın. Bu dört değişkenler biçimini seçmek için aşağı açılır listeler görmeniz gerekir:

![Sistem değişkenlerini kullanma](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Aşağıdaki ekran görüntüsünde gösterildiği gibi ayrıca kullanabileceğiniz bir **özel** değişkeni ve [biçim dizeleri desteklenen](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Bu yapıya sahip bir klasör seçmek için kullanın **Gözat** ilk düğme. Ardından bir değerle değiştirmek **{özel}** , biçim dizesinin girebileceğiniz metin kutusunu görmek için SEKME tuşuna basın.     

![Özel değişken kullanma](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>Çeşitli veri ve nesne türleri için destek
Kopyalama Sihirbazı'nı kullanarak, klasörleri, dosyaları ya da tabloları yüzlerce verimli bir şekilde taşıyabilirsiniz.

![Verilerin kopyalanacağı tabloları seçin](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Zamanlama Seçenekleri
Kopyalama işleminden sonra veya bir zamanlamaya göre çalıştırabilirsiniz (saatlik, günlük, vb.). Bu iki seçenek bağlayıcıları çıkarmak için şirket içi, Bulut ve yerel Masaüstü kopyalama arasında kullanılabilir.

Bir kerelik kopyalama işlemi, yalnızca bir kez bir kaynaktan bir hedef veri taşınmasını sağlar. Her boyutta ve desteklenen bir biçim verilere uygulanır. Zamanlanmış kopyalama önceden belirlenmiş bir yinelenme verileri kopyalamanızı sağlar. Zamanlanmış kopyalama yapılandırmak için zengin ayarları (örneğin, yeniden deneme zaman aşımı ve uyarılar) kullanabilirsiniz.

![Zamanlama özellikleri](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği ile işlem hattı oluşturmak için Data Factory Kopyalama Sihirbazı'nı kullanarak hızlı kılavuz için bkz. [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

