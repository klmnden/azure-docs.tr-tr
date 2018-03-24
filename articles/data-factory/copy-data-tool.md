---
title: Veri Kopyala aracı Azure Data Factory | Microsoft Docs
description: Azure veri fabrikası arabiriminde veri Kopyala aracı hakkında bilgi sağlar
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: b82ee060ff3f25e7a92c85114d457ecb349159b3
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-tool-in-azure-data-factory"></a>Azure Data Factory kopyalama veri aracı
Azure veri fabrikası kopya veri aracı kolaylaştırır ve genellikle bir uçtan uca veri tümleştirme senaryosunun bir ilk adım olan bir veri gölü içine veri alma sürecini en iyi duruma getirir.  Zaman kaydeder özellikle Azure Data Factory ilk kez bir veri kaynağından veri alma için kullandığınızda. Bu aracı kullanarak avantajlarından bazıları şunlardır:

- Azure veri fabrikası kopya veri aracını kullanırken, veri fabrikası tanımlarında bağlı hizmetler, veri kümelerini, ardışık düzen, etkinlikler ve Tetikleyicileri anlaşılmıyor. 
- Veri gölü içinde verileri yüklemek için sezgisel veri Kopyala aracının akışıdır. Aracı verileri seçilen kaynak veri deposundan seçilen hedef/havuz veri deposuna kopyalamak için tüm gerekli Veri Fabrikası Kaynakları otomatik olarak oluşturur. 
- Kopya veri aracı olası hatalar başında kendisini önlemenize yardımcı olan geliştirme, zaman alınan veri doğrulama yardımcı olur.
- Veri gölü veri yüklemek için karmaşık iş mantığı uygulamanız gerekiyorsa, veri fabrikası Arabiriminde Etkinlik başına yazma kullanarak veri Kopyala aracı tarafından oluşturulan Veri Fabrikası Kaynakları hala düzenleyebilirsiniz. 

Aşağıdaki tabloda, veri fabrikası Arabiriminde Etkinlik başına yazma ve veri Kopyala aracı kullanmak ne zaman hakkında yönergeler sağlar: 

| Veri Kopyalama aracı | Etkinlik (kopyalama etkinliği) yazma başına |
| -------------- | -------------------------------------- |
| Azure Data Factory varlıkları (bağlı hizmetler, veri kümelerini, ardışık düzen, vb.) hakkında bilgi olmadan görev yüklenirken bir veri kolayca oluşturmak istediğiniz | Veri gölü yükleme için karmaşık ve esnek mantığı uygulamak istiyorsunuz. |
| Çok sayıda veri yapıtı veri gölü içinde hızlı bir şekilde yüklemek istediğiniz. | Kopyalama etkinliği ile temizleme veya işlem verileri için sonraki etkinliklere zincir istiyorsunuz. |

Kopya veri aracını başlatmak için tıklatın **veri Kopyala** döşeme veri fabrikanızın giriş sayfasında.

![Başlangıç sayfasına - veri Kopyala aracın bağlantısı](./media/copy-data-tool/get-started-page.png)


## <a name="intuitive-flow-for-loading-data-into-a-data-lake"></a>Veri gölü içinde verileri yüklenirken sezgisel akışı
Bu araç kolayca veriler çeşitli kaynaklardan hedeflere sezgisel bir akış ile dakika cinsinden taşımanızı sağlar:  

1. Ayarlarını yapılandırmak **kaynak**.
2. Ayarlarını yapılandırmak **hedef**. 
3. Yapılandırma **Gelişmiş ayarları** sütun eşlemesi, performans ayarlarını ve hataya dayanıklılık ayarları gibi kopyalama işlemi için. 
4. Belirtin bir **zamanlama** görev yüklenirken veriler için. 
5. Gözden geçirme **Özet** oluşturulacak Data Factory varlıkları. 
6. **Düzen** ardışık kopyalama etkinliği için ayarları gerektiği gibi güncelleştirin. 

 Araç desteği çeşitli veri ve nesne türleri ile başlangıç aklınızda büyük veri tasarlanmıştır. Klasörleri, dosyaları ya da tablo yüzlerce taşımak için kullanabilirsiniz. Aracın otomatik veri Önizleme, şema yakalama ve otomatik eşleme ve verileri de filtreleme destekler.

![Veri Kopyalama aracı](./media/copy-data-tool/copy-data-tool.png)

## <a name="automatic-data-preview"></a>Otomatik veri Önizleme
Kopyalanan verileri doğrulamak izin veren seçilen kaynak veri deposu verilerin bir kısmını önizleyebilirsiniz. Ayrıca, veri kaynağını bir metin dosyasında ise, satır ve sütun sınırlayıcıları ve şema otomatik olarak algılamak için metin dosyası veri Kopyala aracı ayrıştırır.

![Dosya ayarları](./media/copy-data-tool/file-format-settings.png)

Algılama sonra:

![Algılanan dosya ayarlarını ve Önizleme](./media/copy-data-tool/after-detection.png)

## <a name="schema-capture-and-automatic-mapping"></a>Şema yakalama ve otomatik eşleme
Veri kaynağının şemasını şeması verileri hedef çoğu durumda, aynı olmayabilir. Bu senaryoda, hedef şemanın sütunlarından kaynak şemasından sütun eşleşmesi gerekir.

Kopya veri aracı izler ve kaynak ve hedef depoları arasında sütunları eşlerken, davranış öğrenir. Bir veya birkaç sütun kaynak veri deposundan seçin ve hedef şemanın eşleme sonra her iki taraftan çekilen sütun çiftleri için desen çözümlemek veri Kopyala aracını başlatır. Ardından, sütunları geri kalanı için aynı düzeni uygular. Bu nedenle, tüm sütunları yalnızca birkaç tıklama sonra istediğiniz şekilde hedef eşledikten bakın.  Seçimi kopyala veri aracı tarafından sağlanan sütun eşlemesi, memnun değilseniz yok sayın ve sütunları el ile eşleme ile devam edebilirsiniz. Bu sırada, kopya veri aracı sürekli öğrenir ve desen güncelleştirir ve sonuçta elde etmek istediğiniz sütun eşlemesi için doğru deseni ulaştığında. 

> [!NOTE]
> Tablo hedef deposunda mevcut değilse verileri SQL Server veya Azure SQL veritabanından Azure SQL Data Warehouse'a kopyalarken, veri Kopyala aracı oluşturmayı tablonun otomatik olarak kaynak şemasını kullanarak destekler. 

## <a name="filter-data"></a>Verileri filtreleme
Havuz veri deposuna kopyalanması gereken verileri seçmek için kaynak verilerini filtreleyebilirsiniz. Filtreleme havuz veri deposuna kopyalanacak veri hacmini azaltır ve bu nedenle kopyalama işlemi verimliliğini artırır. Kopya veri aracını SQL sorgu dili veya dosyaları bir Azure blob klasöründeki kullanarak ilişkisel bir veritabanındaki verilere filtre esnek bir şekilde sağlar. 

### <a name="filter-data-in-a-database"></a>Veritabanındaki verileri filtreleme
Aşağıdaki ekran görüntüsü verilerini filtrelemek için bir SQL sorgusu gösterir.

![Veritabanındaki verileri filtreleme](./media/copy-data-tool/filter-data-in-database.png)

### <a name="filter-data-in-an-azure-blob-folder"></a>Bir Azure blob klasördeki verileri filtreleme
Bir klasörden verileri kopyalamak için klasör yoluna değişkenleri kullanabilirsiniz. Değişkenleri desteklenir: **{year}**, **{month}**, **{day}**, **{saat}**, ve **{dakika}**. Örneğin: inputfolder / {year} / {month} / {day}. 

Klasörleri aşağıdaki biçimde giriş varsayın: 

```
2016/03/01/01
2016/03/01/02
2016/03/01/03
...
```

Tıklatın **Gözat** için düğmesini **dosya veya klasör**, bu klasörlerden birine göz atın (örneğin, 2016 03 -> -> 01 02 ->), tıklatıp **Seç**. 2016/03/01/02 metin kutusuna görmeniz gerekir. 

Ardından, Değiştir **2016** ile **{year}**, **03** ile **{month}**, **01** ile **{day}** , ve **02** ile **{saat}**ve basın **sekmesini** anahtarı. Bu dört değişkenleri biçimini seçmek için aşağı açılır listeler görmeniz gerekir:

![Filtre dosya veya klasör](./media/copy-data-tool/filter-file-or-folder.png)

Kopya veri aracı ifadeleri, İşlevler ve {year} temsil etmek için kullanılan sistem değişkenleri, {month} parametrelerle oluşturur {day}, {saat} ve {dakika} ardışık düzen oluştururken. Daha fazla bilgi için bkz: [veri okumak veya yazmak nasıl bölümlenmiş](how-to-read-write-partitioned-data.md) makalesi.

## <a name="scheduling-options"></a>Zamanlama Seçenekleri
Kopyalama işlemi bir kez veya bir zamanlamaya göre çalıştırabilirsiniz (saatlik, günlük, vb.). Bu seçenekler, şirket içi, Bulut ve yerel Masaüstü gibi farklı ortamlar genelinde bağlayıcıları için kullanılabilir. 

Bir kerelik kopyalama işlemi yalnızca bir kez bir hedefe bir kaynaktan veri taşıma sağlar. Herhangi bir boyuta ve desteklenen bir biçim verileri için geçerlidir. Zamanlanmış kopyalama, belirttiğiniz bir yinelenme verileri kopyalamanıza olanak sağlar. Zamanlanmış kopyalama yapılandırmak için zengin ayarlarınızı (örneğin, yeniden deneme zaman aşımı ve uyarılar) kullanabilirsiniz.

![Zamanlama Seçenekleri](./media/copy-data-tool/scheduling-options.png)


## <a name="next-steps"></a>Sonraki adımlar
Veri Kopyala aracını bu öğreticileri deneyin:

- [Hızlı Başlangıç: Veri Kopyala aracını kullanarak bir veri fabrikası oluşturun](quickstart-create-data-factory-copy-data-tool.md)
- [Öğretici: verileri Azure'da kopyalama veri aracı kullanarak kopyalayın](tutorial-copy-data-tool.md) 
- [Öğretici: kopyalama veri kopyalama veri aracını kullanarak şirket içi](tutorial-hybrid-copy-data-tool.md)
