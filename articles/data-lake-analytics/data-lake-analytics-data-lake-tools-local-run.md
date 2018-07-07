---
title: Yerel makinenizde çalıştırma Azure Data Lake U-SQL betiği | Microsoft Docs
description: Yerel makinenizde U-SQL işlerini çalıştırmak için Visual Studio için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a7f43c7e17f36d9b4e0767744eee9604c2628ea8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888975"
---
# <a name="run-u-sql-script-on-your-local-machine"></a>U-SQL betiğini yerel makinenizde çalıştırma

U-SQL betiği geliştirirken, maliyeti ve zaman kaydedildikçe U-SQL betiğini yerel olarak çalıştırma yaygındır. U-SQL betiklerini yerel makinenizde çalıştırma için Visual Studio için Azure Data Lake Araçları'nı destekler. 

## <a name="basic-concepts-for-local-run"></a>Yerel çalıştırma için temel kavramlar

Grafiğin altına bileşenler için yerel çalıştırma ve bu bileşenler çalıştırma buluta nasıl eşleştiği gösterilir.

|Bileşen|Yerel çalıştırma|Bulut Çalıştır|
|---------|---------|---------|
|Depolama|Yerel veri kök klasörü|Varsayılan Azure Data Lake Store hesabı|
|İşlem|U-SQL yerel çalıştırma altyapısı|Azure veri Gölü analiz hizmeti|
|Yürütme Ortamı|Yerel makinede çalışma dizini|Azure Data Lake Analytics küme|

Daha fazla açıklama bileşenler yerel olarak çalıştırmak için:

### <a name="local-data-root-folder"></a>Yerel veri kök klasörü

Yerel veri kökü "yerel depolama" yerel işlem hesabı klasördür. Yerel makinenizde yerel dosya sistemi herhangi bir klasörde bir yerel veri kök klasör olabilir. Bir Data Lake Analytics hesabının varsayılan Azure Data Lake Store hesabına eşittir. Farklı bir veri kök klasöre geçiş için farklı varsayılan bir depolama hesabı yalnızca değiştirme gibi olur. 

Veri kök klasör için kullanılır:
- Meta veri, veritabanları, tablolar, tablo değerli işlevler ve derlemeler gibi Store.
- U-SQL betiği göreli yollar olarak tanımlanan giriş ve çıkış yol arayın. Göreli yollar kullanarak U-SQL betiklerinizi Azure'a dağıtmanızı kolaylaştırır.

### <a name="u-sql-local-run-engine"></a>U-SQL yerel çalıştırma altyapısı

U-SQL yerel Çalıştırma "yerel işlem hesabı" U-SQL işleri altyapısıdır. Kullanıcılar U-SQL işleri için Visual Studio Azure Data Lake araçları yerel olarak çalıştırabilirsiniz. Yerel yürütme, Azure Data Lake U-SQL SDK komut satırı ve programlama arabirimleri da desteklenir. [Azure Data Lake U-SQL SDK'sı hakkında daha fazla bilgi edinin](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/).

### <a name="working-directory"></a>Çalışma dizini

Bir U-SQL komut dosyası çalıştırılırken, derleme sonuçları, yürütme günlükleri ve önbelleğe almak için bir çalışma dizininiz gereklidir. Azure Data Lake araçları Visual Studio için çalışma dizini U-SQL projenin çalışma dizini olup (genellikle altında bulunan `<U-SQL project root path>/bin/debug>`). Çalışma dizini, yeni bir her çalıştırdığınızda tetiklenen silinecektir.

## <a name="local-run-in-visual-studio"></a>Yerel Visual Studio'da çalıştırma

Visual Studio için Azure Data Lake araçları bir yerleşik yerel altyapısı çalıştırma sahip ve onu yerel işlem hesabı ortaya çıkarır. Bir U-SQL betiğini yerel olarak seçin (yerel makine) veya (yerel proje) hesabı betiğin Düzenleyici kenar boşluğunda açılan çalıştırıp tıklayın **Gönder**.

![Yerel hesap gönderme betiğine Visual Studio için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-run-with-local-machine-account"></a>Yerel (yerel makine) hesabıyla çalıştırma

(Yerel makine) tek bir yerel veri kök klasöre paylaşılan yerel işlem hesabıyla yerel depolama hesabı olarak hesabıdır. Varsayılan konumunda bulunan veri kök klasör olarak "C:\Users\<kullanıcıadı > \AppData\Local\USQLDataRoot", ayrıca aracılığıyla yapılandırılabilir **Araçlar > Data Lake > Seçenekler ve ayarlar**.

![Visual Studio için Data Lake araçları yerel veri kökü yapılandırın](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
U-SQL projesi yerel çalıştırma için gereklidir. U-SQL projenin çalışma dizini, U-SQL yerel çalıştırma çalışma dizini için kullanılır. Derleme sonuçları, yürütme günlüklerini ve diğer iş yürütme ile ilgili dosyalar oluşturulur ve yerel çalıştırma sırasında çalışma dizini klasörü altında depolanır. Betiği yeniden her zaman, çalışma dizininde tüm bu dosyalar yeniden ve temizlenir olduğunu unutmayın.

## <a name="local-run-with-local-project-account"></a>Yerel (yerel proje) hesabıyla çalıştırma

(Yerel proje), yalıtılmış yerel verileri kök klasörü her proje için bir proje yalıtılmış yerel işlem hesabı hesabıdır. Çözüm Gezgini'nde açma her etkin U-SQL projesi karşılık gelen sahip `(Local-project: <project name>)` hesabı hem de Sunucu Gezgini ve U-SQL betiği Düzenleyici kenar listelenir. 

(Yerel proje) hesabı, geliştiriciler için bir temiz ve yalıtılmış bir geliştirme ortamı sağlar. Meta verileri ve tüm yerel işleri için giriş/çıkış veri saklama paylaşılan yerel verileri kök klasörü (yerel makine) hesabı (yerel proje) hesabı, geçici bir yerel veri kök klasörü altında U-SQL projesi çalışma dizini her zaman bir U-SQL betiği oluşturur çalıştırma. Bu geçici verileri kök klasörü yeniden oluşturma veya yeniden gerçekleştiğinde temizlendi. 

U-SQL projesi bu yalıtılmış yerel proje başvurusu ve özelliği aracılığıyla ortam çalıştırma yönetmek için iyi bir deneyim sağlar. Her ikisi de U-SQL betikleri için giriş veri kaynaklarını proje, aynı zamanda başvurulan veritabanı ortamları yapılandırabilirsiniz.

### <a name="manage-input-data-source-for-local-project-account"></a>Giriş veri kaynağı için (yerel proje) hesabı yönetme

U-SQL projesi yerel verileri kök klasörü oluşturma ve verileri (yerel proje) hesabı için ayar üstlenir. Geçici bir veri kök klasöre temizlenir ve U-SQL projesi çalışma dizini altında yeniden oluşturun ve yerel çalıştırma'olmuyor her zaman yeniden. U-SQL projesi tarafından yapılandırılmış tüm veri kaynaklarının yerel işi yürütmeden önce bu geçici yerel veri kök klasörüne kopyalanır. 

Veri kaynaklarınızı kök klasörüne yapılandırabileceğiniz **U-SQL projesi sağ tıklayın > özellik > Test verisi kaynağı**. Çalışırken U-SQL betiği (yerel proje) hesabı, tüm dosyaları ve alt klasörleri (alt klasörleri altındaki dosyaları da dahil olmak üzere) **Test verisi kaynağı** klasörünü, geçici yerel veri kök klasörüne kopyalanır. Yerel iş yürütme tamamlandıktan sonra çıktı sonuçları da geçici yerel verileri kök klasörü altında proje çalışma dizininde bulunabilir. Tüm bu çıkışları silinecek ve projeyi yeniden ve Temizlenen, temizlenen olduğunu unutmayın. 

![Visual Studio için Data Lake araçları proje test veri kaynağını yapılandırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-referenced-database-environment-for-local-project-account"></a>Başvurulan veritabanı ortam için (yerel proje) hesabı yönetme 

Kullanır veya sorguları ile U-SQL veritabanı nesnelerini bir U-SQL sorgu, veritabanı ortamları hazır yerel olarak bu U-SQL betiğini yerel olarak çalıştırmadan önce yapmanız gerekir. (Yerel proje) hesap için U-SQL veritabanı bağımlılıklarını U-SQL projesi başvuruları tarafından yönetilebilir. U-SQL veritabanı projesi başvuruları, U-SQL projesine ekleyebilirsiniz. U-SQL betikleri (yerel proje) hesabı çalıştırmadan önce başvurulan tüm veritabanlarını geçici yerel veri kök klasörüne dağıtılır. Ve her çalıştırma için geçici veri kök klasöründe yeni bir yalıtılmış ortamın temizlendiği.

İlgili makaleler:
* [U-SQL veritabanı projesi U-SQL veritabanı tanımıyla yönetmeyi öğrenin](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)
* [U-SQL projesi U-SQL veritabanı başvuru yönetmeyi öğrenin](data-lake-analytics-data-lake-tools-develop-usql-database.md)

## <a name="difference-between-local-machine-and-local-project-account"></a>(Yerel makine) arasındaki farkı ve hesap (yerel proje)

Bir Azure Data Lake Analytics hesabı kullanıcıların yerel makinede benzetimini yapmak için (yerel makine) hesabı amaçlar. Bu, Azure Data Lake Analytics hesabı ile aynı deneyimi paylaşır. Kullanıcıların yardımcı olan bir kullanıcı dostu yerel geliştirme ortamı sağlamak için (yerel proje) aıms veritabanları başvurular ve girdi verilerini betiği çalıştırmadan önce yerel olarak dağıtın. (Yerel makine) hesabı, tüm projeleri erişilebilen paylaşılan bir kalıcı ortamı sağlar. Her proje için bir yalıtılmış bir geliştirme ortamı (yerel proje) hesabı sağlar ve her çalıştırma için yenilenir. Yukarıda bağlı olarak hızla yeni değişiklikleri uygulayarak hesap (yerel proje) daha hızlı bir geliştirme deneyimi sunar.

Daha fazla fark (yerel makine) arasında bulabilirsiniz ve aşağıdaki gibi hesap (yerel proje) hesabı:

|Fark açısı|(Yerel makine)|(Yerel proje)|
|----------------|---------------|---------------|
|Yerel erişim|Tüm projeleri tarafından erişilebilir|Bu hesap yalnızca ilgili projeye erişebilirsiniz|
|Yerel veri kök klasörü|Kalıcı bir yerel klasör. Aracılığıyla yapılandırılan **Araçlar > Data Lake > Seçenekler ve ayarlar**|U-SQL projesi çalışma dizini altında yerel her çalıştırma için oluşturulan geçici bir klasör. Klasör yeniden oluşturma veya yeniden gerçekleştiğinde temizlendi|
|Giriş verileri için U-SQL betiği|Kalıcı yerel verileri kök klasörü altında göreli yolu|Aracılığıyla ayarlanan **U-SQL proje özelliği > Test verisi kaynağı**. Tüm dosyalar, alt klasörler yerel yürütmeden önce geçici veri kök klasöre kopyalanır|
|Çıktı verileri için U-SQL betiği|Kalıcı yerel verileri kök klasörü altında göreli yolu|Geçici veri kök klasörüne yüzdelik. Yeniden oluşturma veya yeniden gerçekleştiğinde sonuçları temizlenir.|
|Başvurulan veritabanı dağıtımı|Başvurulan veritabanlarını (yerel makine) hesaplarında çalışırken otomatik olarak dağıtılmaz. Aynı Azure Data Lake Analytics hesabına gönderiliyor.|Başvurulan veritabanlarını (yerel proje) hesabı yerel yürütme önce otomatik olarak dağıtılır. Tüm veritabanı ortamları temizlenmesi ve yeniden oluşturma veya yeniden olduğunda yeniden dağıtıldı.|

## <a name="local-run-with-u-sql-sdk"></a>Yerel U-SQL SDK'sı ile çalıştırma

Yanında U-SQL betiklerini yerel olarak Visual Studio içinde çalışmakta olan, aynı zamanda Azure Data Lake U-SQL SDK'sı U-SQL betiklerini komut satırı ve programlama arabirimleri ile yerel olarak çalıştırmak için kullanabilirsiniz. Bu arabirimleri otomatikleştirmek U-SQL yerel çalıştırma ve test edin.

[Azure Data Lake U-SQL SDK'sı hakkında daha fazla bilgi edinin](data-lake-analytics-u-sql-sdk.md).

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Data Lake U-SQL SDK'sı](data-lake-analytics-u-sql-sdk.md)
- [Azure Data Lake Analytics için CI/CD işlem hattı ayarlama](data-lake-analytics-cicd-overview.md)
- [U-SQL veritabanı geliştirme için U-SQL veritabanı projesi kullanın](data-lake-analytics-data-lake-tools-develop-usql-database.md)
- [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md)
