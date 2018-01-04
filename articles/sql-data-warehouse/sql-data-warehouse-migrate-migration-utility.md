---
title: "Geçir: Veri ambarı geçiş yardımcı programı | Microsoft Docs"
description: "SQL veri ambarı'na geçirin."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 4d7ad981-ef31-4513-b337-50bdc4709c09
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: e8a8a84153a950f2d1bc002b34c83dc5ed8a5eb8
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="data-warehouse-migration-utility-preview"></a>Veri ambarı geçiş yardımcı programı (Önizleme)
> [!div class="op_single_selector"]
> * [Geçiş yardımcı programı indir][Download Migration Utility]
> 
> 

Veri ambarı geçiş yardımcı programı'nı SQL Server ve Azure SQL veritabanı şeması ve verisi için Azure SQL Data Warehouse geçirmek için tasarlanmış bir araçtır. Şema geçiş sırasında aracı hedef karşılık gelen şemaya kaynağından otomatik olarak eşler. Şema geçirildikten sonra araçları otomatik olarak oluşturulan komut ile veri taşımak için bir seçenek sağlar.

Şeması ve verisi geçişe ek olarak, bu aracı kolaylaştırılmış geçiş önleyen hedef ve kaynak örnekleri arasında uyumsuzluk özetlemek uyumluluk raporları oluşturmak için seçeneği sunar.

## <a name="get-started"></a>başlarken
Yükleme için bir önkoşul olarak geçiş betikleri ve uyumluluk raporu görüntülemek üzere Office çalıştırmak için BCP komut satırı yardımcı programı gerekir. İndirilen yürütülebilir başlattıktan sonra aracı yüklenmeden önce standart EULA kabul etmeniz istenir.

Ayrıca, geçiş Utiliy çalıştırmak için bir gerekir geçirmeye bakıyorsanız veritabanında aşağıdaki izinleri: veritabanı oluşturma, ALTER ANY DATABASE veya Görünüm herhangi TANIMI.

### <a name="launching-the-tool-and-connecting"></a>Aracı'nı başlatarak ve bağlanma
Aracı yükleme sonrası görünen masaüstü simgesini tıklatarak başlatın. Aracı açıldığında, kaynak ve hedef geçiş aracı için seçebileceğiniz bir ilk bağlantı sayfasıyla istenir. Bu aşamada, SQL Server ve Azure SQL veritabanı kaynakları ve SQL Data Warehouse hedef olarak olarak destekliyoruz. Bu seçtikten sonra sunucu adını doldurma ve kimlik doğrulaması ve 'Bağlan' a tıklayarak kaynak sunucunuza bağlanmak istenir.

Kimlik doğrulandıktan sonra aracı, bağlandığınız sunucudaki var olan veritabanlarının listesini gösterir. Geçirmek istediğiniz veritabanını seçtikten ve 'seçili geçirme' üzerinde tıklatarak geçişe başlayabilirsiniz.

## <a name="migration-report"></a>Geçiş raporu
'Veritabanı uyumluluğu denetle' aracı seçme geçirmek için istenen veritabanındaki tüm nesne uyumsuzlukları özetleyen bir rapor oluşturur. SQL veri ambarı'nda mevcut değil SQL Server işlevsellik daha geniş bir listesi bulunabilir bizim [geçiş belgeleri][migration documentation]. Raporun oluşturulduğu sonra kaydedin ve rapor Excel'de açın.

Lütfen geçiş şeması, 'Object' bu verilerin hemen geçiş izin vermek üzere ayarlanmış olarak tanımlanan çoğu sorunların oluştururken unutmayın. Lütfen şemayı uygulamadan önce ek ayarlamalar yapmak istiyor musunuz emin olmak için değişiklikleri gözden geçirin.

## <a name="migrate-schema"></a>Geçiş şeması
Bağlandıktan sonra geçirme'Schema ' seçerek seçili tablo için şema geçiş komut dosyası oluşturur. Bu komut dosyası bağlantı noktalarını eşlemeleri uyumsuz veri tablosunun yapısı daha uyumlu formlara türleri ve bu geçiş ayarları kullanıcı tarafından belirtilirse, güvenlik kimlik bilgileri ve şema oluşturur. Bu kod hedeflenen SQL Data Warehouse örneğine karşı çalıştırabilirsiniz, bir dosyaya kaydedilir, panonuza kopyaladığınız veya daha fazla eylemi gerçekleştirmeden önce satır içi bile düzenlenebilir.  

Geçirme şema gözden geçirme, değiştiğinde olarak yukarıda belirtildiği, aracı emin olmak için yaptığı tam olarak bunları anladığınızdan.  

## <a name="migrate-data"></a>Geçiş verileri
'Verileri geçirmek' seçeneğini tıklatarak, verilerinizi ilk sunucunuzda, düz dosyalar taşır BCP komut dosyaları oluşturabilir ve ardından SQL veri ambarı doğrudan. Küçük miktarda veri ve yeniden deneme olarak taşımak için bu işlemi yerleşik olmayan ve hataları ağ bağlantı kaybı oluşabilir öneririz. Bunu çalıştırmak için BCP komut satırı yardımcı programının yüklü olması gerekir ve veriler için şema zaten oluşturulmuş olması gerekir.

Parametreleri yukarıdaki doldurduktan sonra çalışma geçiş tıklamanız yeterlidir ve iki paket kümesini belirtilen konuma oluşturulur. Veri geçiş kaynağınızdan düz dosyasına dışarı aktarma için dışarı aktarma dosyasını çalıştırın ve verilerinizi SQL Data Warehouse'a almak için içeri aktarma dosyasını çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
Nasıl yapılır bazı verileri geçirdikten, kullanıma [geliştirmek][develop].

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://www.microsoft.com/en-us/download/details.aspx?id=49100
