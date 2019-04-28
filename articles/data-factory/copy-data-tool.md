---
title: Azure Data Factory veri kopyalama aracı | Microsoft Docs
description: Azure Data Factory kullanıcı arabiriminde veri kopyalama aracı hakkında bilgi sağlar.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/18/2018
ms.author: yexu
ms.openlocfilehash: 107687c785433f81870449d1445136b5148a4d2c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60787738"
---
# <a name="copy-data-tool-in-azure-data-factory"></a>Azure Data Factory'de veri aracı kopyalayın
Azure Data Factory veri kopyalama aracını kolaylaştırır ve almak veri işlemi, genellikle bir uçtan uca veri tümleştirme senaryosunu ilk adımı bir veri gölü içinde iyileştirir.  Zaman kaydeder özellikle kullandığınızda, Azure Data Factory ilk kez bir veri kaynağından veri alımı için. Bu aracı kullanarak avantajlarından bazıları şunlardır:

- Azure Data Factory veri kopyalama aracını kullanırken, Data Factory bağlı Hizmetleri, veri kümeleri, işlem hatları, etkinlikler ve Tetikleyicileri tanımlarında anlaşılmıyor. 
- Verileri göle verileri yüklemek için sezgisel veri kopyalama aracının akışıdır. Araç, seçilen kaynak veri deposundan seçilen hedef/havuz veri deposuna veri kopyalamak için gereken tüm Data Factory kaynaklarını otomatik olarak oluşturur. 
- Veri kopyalama aracını başında kendisini olası hataları önlemeye yardımcı olan geliştirme, zaman alınan verileri doğrulamanıza yardımcı olur.
- Verileri verileri göle yüklemek için karmaşık iş mantığı uygulamanız gerekiyorsa, Etkinlik başına yazma Data Factory kullanıcı arabirimini kullanarak veri kopyalama aracı tarafından oluşturulan Data Factory kaynaklarını yine de düzenleyebilirsiniz. 

Aşağıdaki tabloda Data Factory kullanıcı arabirimini Etkinlik başına yazma ve veri kopyalama aracını kullanmak ne zaman hakkında rehberlik sağlar: 

| Veri Kopyalama aracı | Etkinlik (kopyalama etkinliği) yazma başına |
| -------------- | -------------------------------------- |
| Azure Data Factory varlıklarını (bağlı hizmetler, veri kümeleri, işlem hatları, vb.) hakkında daha fazla bilgi olmadan görev yüklenirken veri kolayca oluşturmak istiyorsunuz | Verileri göle yüklemek için karmaşık ve esnek mantığını uygulamak istiyorsunuz. |
| Hızlı bir şekilde çok sayıda veri yapıtı verileri göle yüklemek istiyorsunuz. | Kopyalama etkinliği ile verileri temizleme veya işlem için sonraki etkinliklerin zincir istiyorsunuz. |

Veri kopyalama aracını başlatmak için tıklatın **veri kopyalama** kutucuğuna veri fabrikanızın giriş sayfasında.

![Başlarken sayfasında - veri kopyalama aracını Bağla](./media/copy-data-tool/get-started-page.png)


## <a name="intuitive-flow-for-loading-data-into-a-data-lake"></a>Sezgisel flow bir veri gölü veri yükleme
Bu araç veri'nın sezgisel bir flow ile dakikalar içinde bir çok çeşitli kaynaklardan hedeflere kolayca taşımanızı sağlar:  

1. Ayarlarını yapılandırmak **kaynak**.
2. Ayarlarını yapılandırmak **hedef**. 
3. Yapılandırma **Gelişmiş ayarlar** kopyalama işleminin sütun eşleme, performans ayarları ve hataya dayanıklılık ayarları gibi. 
4. Belirtin bir **zamanlama** veri görev yükleme için. 
5. Gözden geçirme **Özet** oluşturulacak Data Factory varlıkları. 
6. **Düzen** işlem hattının kopyalama etkinliği için ayarları gerektiği gibi güncelleştirin. 

   Aracı, çeşitli veri ve nesne türleri için destek ile başlangıç aklınızda büyük verilerle tasarlanmıştır. Klasörleri, dosyaları ya da tabloları yüzlerce taşımak için kullanabilirsiniz. Aracı otomatik veri Önizleme, şema yakalama ve otomatik eşleme ve verileri de filtreleme destekler.

![Veri Kopyalama aracı](./media/copy-data-tool/copy-data-tool.png)

## <a name="automatic-data-preview"></a>Otomatik veri önizlemesi
Kopyalanan verileri doğrulamak izin veren seçilen kaynak veri deposundan veri parçası önizleyebilirsiniz. Ayrıca, kaynak verileri bir metin dosyasına ise, satır ve sütun sınırlayıcıları ve şema otomatik olarak algılamak için metin dosyası veri kopyalama aracını ayrıştırır.

![Dosya ayarları](./media/copy-data-tool/file-format-settings.png)

Algılama sonra:

![Algılanan dosyası ayarlarının ve Önizleme](./media/copy-data-tool/after-detection.png)

## <a name="schema-capture-and-automatic-mapping"></a>Şema yakalama ve otomatik eşleme
Veri kaynağının şemasını veri hedef çoğu durumda, şema olarak aynı olmayabilir. Bu senaryoda, hedef şemanın sütunları için kaynak şemasından sütunları eşlemeniz gerekir.

Veri kopyalama aracını izler ve kaynak ve hedef depolama alanları arasında sütun eşlerken davranışınızı öğrenir. Kaynak veri deposundan bir veya birkaç sütunu seçin ve bunları hedef şemaya eşleme sonra her iki taraftan çekilen sütun çiftleri desenini çözümlenecek veri kopyalama aracını başlatır. Ardından, sütunların geri kalanı için aynı düzeni uygular. Bu nedenle, tüm sütunları, yalnızca birkaç tıklamayla sonra istediğiniz şekilde hedef için eşleştirilmiş iş bakın.  Veri kopyalama aracı tarafından sağlanan sütun eşlemesi seçimi memnun değilseniz yoksayabilir ve sütun eşleme ile el ile devam edin. Bu arada, veri kopyalama aracını sürekli olarak öğrenir ve desen güncelleştirir ve sonuçta elde etmek istediğiniz sütun eşlemesi için doğru deseni ulaşır. 

> [!NOTE]
> Tablo hedef depoda mevcut değilse data SQL Server veya Azure SQL veritabanından Azure SQL veri ambarı'na kopyalama yapılırken, veri kopyalama aracını oluşturulmasını tablo otomatik olarak kaynak şemayı kullanarak destekler. 

## <a name="filter-data"></a>Verileri filtreleme
Havuz veri deposuna kopyalanacak gereken verileri seçmek için kaynak verilerini filtreleyebilirsiniz. Filtreleme, havuz veri deposuna kopyalanacak verileri hacmini azaltır ve bu nedenle kopyalama işleminin aktarım hızını geliştirir. Kopyalama veri aracı, bir Azure blob klasörüne SQL sorgu dili veya dosyalar'ı kullanarak esnek bir şekilde ilişkisel bir veritabanındaki verilere filtre uygulamak sağlar. 

### <a name="filter-data-in-a-database"></a>Bir veritabanındaki verileri filtreleme
Aşağıdaki ekran görüntüsünde, verilere filtre uygulamak için bir SQL sorgusu gösterir.

![Bir veritabanındaki verileri filtreleme](./media/copy-data-tool/filter-data-in-database.png)

### <a name="filter-data-in-an-azure-blob-folder"></a>Bir Azure blob klasördeki verileri filtreleme
Bir klasöre veri kopyalamak için klasör yoluna değişkenleri kullanabilirsiniz. Desteklenen değişkenler: **{year}**, **{month}**, **{day}**, **{hour}**, ve **{minute}**. Örneğin: inputfolder / {year} / {month} / {day}. 

Klasörleri aşağıdaki biçimde giriş varsayalım: 

```
2016/03/01/01
2016/03/01/02
2016/03/01/03
...
```

Tıklayın **Gözat** için düğme **dosya veya klasör**, bu klasörlerden birine göz atın (örneğin, 2016 03 -> -> 01 -> 02), tıklatıp **Seç**. 2016/03/01/02 metin kutusundaki görmeniz gerekir. 

Ardından, **2016** ile **{year}**, **03** ile **{month}**, **01** ile **{day}** , ve **02** ile **{hour}** basın **sekmesini** anahtarı. Bu dört değişkenler biçimini seçmek için aşağı açılır listeler görmeniz gerekir:

![Filtre dosyası veya klasörü](./media/copy-data-tool/filter-file-or-folder.png)

Veri kopyalama aracını ifadeleri, İşlevler ve {year} temsil etmek için kullanılan sistem değişkenleri, {month} parametrelerle oluşturur {day} {hour} ve {minute} işlem hattını oluştururken. Daha fazla bilgi için [okumak veya yazmak nasıl veri bölümlenmiş](how-to-read-write-partitioned-data.md) makalesi.

## <a name="scheduling-options"></a>Zamanlama Seçenekleri
Kopyalama işleminden sonra veya bir zamanlamaya göre çalıştırabilirsiniz (saatlik, günlük, vb.). Bu seçenekler, yerel Masaüstü şirket içi ve bulut dahil olmak üzere farklı ortamlar genelinde bağlayıcılar için kullanılabilir. 

Bir kerelik kopyalama işlemi, yalnızca bir kez bir kaynaktan bir hedef veri taşınmasını sağlar. Her boyutta ve desteklenen bir biçim verilere uygulanır. Zamanlanmış kopyalama belirttiğiniz yineleme verileri kopyalamanızı sağlar. Zamanlanmış kopyalama yapılandırmak için zengin ayarları (örneğin, yeniden deneme zaman aşımı ve uyarılar) kullanabilirsiniz.

![Zamanlama Seçenekleri](./media/copy-data-tool/scheduling-options.png)


## <a name="next-steps"></a>Sonraki adımlar
Veri kopyalama aracını bu öğreticileri deneyin:

- [Hızlı Başlangıç: veri kopyalama aracını kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-copy-data-tool.md)
- [Öğretici: Azure'da veri kopyalama aracını kullanarak veri kopyalama](tutorial-copy-data-tool.md) 
- [Öğretici: şirket içi verileri kopyalama veri kopyalama aracını kullanarak azure'a](tutorial-hybrid-copy-data-tool.md)
