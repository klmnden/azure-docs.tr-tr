---
title: Yerel makinenizde çalıştırma Azure Data Lake U-SQL betikleri
description: Yerel makinenizde U-SQL işlerini çalıştırmak için Visual Studio için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 42e58125fcbc3ab411c0d7503c42c14c28178428
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62113944"
---
# <a name="run-u-sql-scripts-on-your-local-machine"></a>U-SQL betiklerini yerel makinenizde çalıştırma

U-SQL betikleri geliştirme, betikleri yerel olarak çalışan tarafından zaman ve para harcamazsınız kaydedebilirsiniz. Visual Studio için Azure Data Lake araçları yerel makinenizde çalışan bir U-SQL betikleri destekler. 

## <a name="basic-concepts-for-local-runs"></a>Yerel çalışmalar için temel kavramlar

Aşağıdaki grafikte bileşenleri yerel çalıştırma ve bu bileşenler çalıştırma buluta nasıl eşleştiği gösterilir.

|Bileşen|Yerel çalıştırma|Bulutta çalıştırın|
|---------|---------|---------|
|Depolama|Yerel veri kök klasörü|Varsayılan Azure Data Lake Store hesabı|
|İşlem|U-SQL yerel çalıştırma altyapısı|Azure veri Gölü analiz hizmeti|
|Çalışma ortamı|Yerel makinede çalışma dizini|Azure Data Lake Analytics küme|

Aşağıdaki bölümlerde, yerel çalışma bileşenleri hakkında daha fazla bilgi sağlar.

### <a name="local-data-root-folders"></a>Yerel veri kök klasörleri

Bir yerel veri kök klasörü bir **yerel depo** hesabı için yerel işlem. Yerel makinenizde yerel dosya sistemi herhangi bir klasörde bir yerel veri kök klasör olabilir. Bu varsayılan Azure Data Lake Store hesabını Data Lake Analytics hesabı ile aynı olur. Farklı veri kök klasörüne geçiş için farklı varsayılan bir depolama hesabı yalnızca değiştirme gibi olur. 

Verileri kök klasörü şu şekilde kullanılır:
- Meta veri Store. Veritabanı, tablolar, tablo değerli işlevler ve derlemeleri verilebilir.
- U-SQL betikleri göreli yollar olarak tanımlanan giriş ve çıkış yol arayın. Göreli yolları'nı kullanarak U-SQL betiklerinizi Azure'a dağıtmak daha kolay olur.

### <a name="u-sql-local-run-engines"></a>U-SQL yerel altyapıları çalıştırın

Bir U-SQL yerel çalıştırma altyapısı bir **yerel işlem hesabı** U-SQL işleri için. Kullanıcılar U-SQL işleri için Visual Studio Azure Data Lake araçları yerel olarak çalıştırabilirsiniz. Yerel çalışmalar da Azure Data Lake U-SQL SDK komut satırı ve programlama arabirimleri desteklenir. Daha fazla bilgi edinin [Azure Data Lake U-SQL SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/).

### <a name="working-directories"></a>Çalışma dizini

U-SQL betiğini çalıştırdığınızda, bir çalışma dizininiz derleme sonuçlarını önbelleğe alma, günlükleri çalıştırmak ve diğer işlevleri gerçekleştirmek için gereklidir. Azure Data Lake araçları Visual Studio için çalışma dizini U-SQL projenin çalışma dizinidir. Altında bulunan `<U-SQL project root path>/bin/debug>`. Çalışma dizini, her seferinde yeni bir çalıştırma tetiklenen temizlendiği.

## <a name="local-runs-in-microsoft-visual-studio"></a>Microsoft Visual Studio yerel çalıştırma

Visual Studio için Azure Data Lake araçları, yerleşik bir yerel çalışma altyapısı vardır. Araçları, yerel işlem hesabı olarak altyapısı yüzey. U-SQL betiğini yerel olarak çalıştırmak için seçin **yerel makine** veya **yerel proje** hesabında betik Düzenleyici kenar boşluğu açılan menüsü. Ardından **Gönder**.

![Yerel bir hesap için bir U-SQL betiği Gönder](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-runs-with-a-local-machine-account"></a>Yerel bir yerel makine hesabı ile çalışır

A **yerel makine** hesabı, paylaşılan bir yerel hesap tek bir yerel veri kök klasör yerel depolama hesabı ile işlem. Verileri kök klasörü varsayılan konumunda yer **C:\Users\<kullanıcıadı > \AppData\Local\USQLDataRoot**. Ayrıca aracılığıyla yapılandırılabilir **Araçları** > **Data Lake** > **seçenekler ve ayarlar**.

![Yerel veri kök klasörünü yapılandırmak](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
U-SQL projesi yerel çalıştırma için gereklidir. U-SQL projenin çalışma dizini, U-SQL yerel çalıştırma için çalışma dizini olarak kullanılır. Derleme sonuçları, günlükler ve diğer iş çalıştırma ile ilgili dosyaları çalıştırın oluşturulur ve yerel çalıştırma sırasında çalışma dizini klasörü altında depolanır. Betiği yeniden her zaman, çalışma dizinindeki tüm dosyalar temizlenmesi ve yeniden oluşturuldu.

## <a name="local-runs-with-a-local-project-account"></a>Yerel bir proje yerel hesabı ile çalışır

A **yerel proje** hesabı, proje yalıtılmış bir yerel hesabı bir ayrılmış yerel verileri kök klasörü ile her proje için işlem. Visual Studio'daki Çözüm Gezgini'nde açar her etkin bir U-SQL projesi karşılık gelen sahip `(Local-project: <project name>)` hesabı. Hesaplar, hem Visual Studio sunucu Gezgini'nde hem de U-SQL betiği Düzenleyici kenar listelenir.  

**Yerel proje** hesabı, açık ve yalıtılmış bir geliştirme ortamı sağlar. A **yerel makine** hesabının yerel tüm işler için meta verileri ve girdi ve çıktı verilerini depolayan bir yerel paylaşılan veri kök klasörü vardır. Ancak bir **yerel proje** hesabı geçici yerel veri kök klasör bir U-SQL projesi çalışma dizini altında bir U-SQL betiği her çalıştırıldığında oluşturur. Bu geçici verileri kök klasörü bir yeniden oluşturma, temizlenmesi veya yeniden olur. 

U-SQL projesi bir proje başvurusu ve özelliği aracılığıyla yalıtılmış yerel çalıştırma ortamını yönetir. Hem proje hem de veritabanı ortamlarında, U-SQL betikleri için giriş veri kaynaklarını yapılandırabilirsiniz.

### <a name="manage-the-input-data-source-for-a-local-project-account"></a>Giriş veri kaynağı için bir yerel proje hesabı yönetme 

U-SQL projesi yerel verileri kök klasörü oluşturur ve veriler için ayarlar bir **yerel proje** hesabı. Geçici verileri kök klasörü temizlenmesi ve yeniden oluşturma ve yerel çalıştırma her zaman U-SQL projesi çalışma dizininin altına yeniden. U-SQL projesi tarafından yapılandırılan tüm veri kaynakları, yerel bir işi çalıştırmadan önce bu geçici bir yerel veri kök klasörüne kopyalanır. 

Veri kaynaklarınızı kök klasörüne yapılandırabilirsiniz. Sağ **U-SQL projesi** > **özelliği** > **Test verisi kaynağı**. Bir U-SQL betiği çalıştırıldığında bir **yerel proje** hesabı, tüm dosyaları ve alt klasörlerinde **Test verisi kaynağı** klasörünü, geçici bir yerel veri kök klasörüne kopyalanır. Alt klasör altındaki dosyalar eklenir. Yerel bir işi çalıştıktan sonra çıktı sonuçları da geçici yerel verileri kök klasörü altında proje çalışma dizininde bulunabilir. Bu çıkış silinir ve projeyi yeniden ve Temizlenen temizlendi. 

![Bir projenin test veri kaynağını yapılandırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-a-referenced-database-environment-for-a-local-project-account"></a>Yönetmek için bir veritabanı ortam bir **yerel proje** hesabı 

Bir U-SQL kullanır veya U-SQL veritabanı nesnelerini sorgularla sorgularsanız, yerel olarak U-SQL betiğini yerel olarak çalıştırmadan önce veritabanı ortamları hazır vermeniz gerekir. İçin bir **yerel proje** hesabı, U-SQL veritabanı bağımlılıkları U-SQL projesi başvuruları tarafından yönetilebilir. U-SQL veritabanı projesi başvuruları, U-SQL projesine ekleyebilirsiniz. U-SQL betiklerini çalıştırma önce bir **yerel proje** hesabı, başvurulan tüm veritabanları, geçici bir yerel veri kök klasörüne dağıtılır. Ve her çalıştırma için yeni bir yalıtılmış ortamın geçici verileri kök klasörü temizlendiği.

Bu ilgili makaleye bakın:
* U-SQL veritabanı tanımları ve başvuruları yönetmeyi öğrenin [U-SQL veritabanı projeleri](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="the-difference-between-local-machine-and-local-project-accounts"></a>Arasındaki fark **yerel makine** ve **yerel proje** hesapları

A **yerel makine** hesabı bir Azure Data Lake Analytics hesabı yerel kullanıcıların makinelerindeki benzetimini yapar. Bu, bir Azure Data Lake Analytics hesabı ile aynı deneyimi paylaşır. A **yerel proje** hesabı, bir kullanıcı dostu yerel geliştirme ortamı sağlar. Bu ortamda veritabanı başvurular ve komut dosyaları yerel olarak çalışan önce giriş verileri dağıtmak kullanıcılara yardımcı olur. A **yerel makine** hesabı, tüm projeleri erişilebilen paylaşılan kalıcı bir ortam sağlar. A **yerel proje** hesabı, her proje için bir yalıtılmış bir geliştirme ortamı sağlar. Her çalıştırma için yenilenir. A **yerel proje** hesabı hızla yeni değişiklikleri uygulayarak daha hızlı bir geliştirme deneyimi sunar.

Daha fazla farklılıklardan **yerel makine** ve **yerel proje** hesapları aşağıdaki tabloda gösterilmiştir:

|Fark açısı|Yerel Makine|Yerel proje|
|----------------|---------------|---------------|
|Yerel erişim|Tüm projeleri tarafından erişilebilir.|Bu hesap yalnızca ilgili projeye erişebilirsiniz.|
|Yerel veri kök klasörü|Kalıcı bir yerel klasör. Aracılığıyla yapılandırılan **Araçları** > **veri Gölü** > **seçenekler ve ayarlar**.|Çalışma dizini U-SQL projesi altındaki her yerel çalıştırma için oluşturulan geçici bir klasör. Klasörü bir yeniden oluşturma, temizlenen veya yeniden olur.|
|Giriş verileri için bir U-SQL betiği|Kalıcı yerel veri kök klasörü altında göreli yol.|Aracılığıyla ayarlanan **U-SQL projesi özelliğini** > **Test verisi kaynağı**. Tüm dosya ve alt yerel çalıştırma önce geçici veri kök klasörüne kopyalanır.|
|Çıktı verileri için bir U-SQL betiği|Göreli yol kalıcı yerel veri kök klasörü altında.|Geçici verileri kök klasörü çıkışı. Sonuçları bir yeniden oluşturma, temizlenmesi veya yeniden olur.|
|Başvurulan veritabanı dağıtımı|Başvurulan veritabanlarını olmayan dağıtılan otomatik olarak karşı çalıştırırken bir **yerel makine** hesabı. Bir Azure Data Lake Analytics hesabına göndermek için aynı olacak.|Başvurulan veritabanlarını dağıtıldığı **yerel proje** önce otomatik olarak bir yerel çalıştırma hesabı. Tüm veritabanı ortamları temizlenmesi ve yeniden oluşturma, yeniden veya yeniden olur.|

## <a name="a-local-run-with-the-u-sql-sdk"></a>Bir yerel U-SQL SDK'sı ile çalışma

U-SQL betikleri Visual Studio'da yerel olarak çalıştırın ve ayrıca U-SQL betiklerini komut satırı ve programlama arabirimleri ile yerel olarak çalıştırmak için Azure Data Lake U-SQL SDK'yı kullanın. Bu arabirimler U-SQL yerel çalıştırma ve testleri otomatik hale getirebilirsiniz.

Daha fazla bilgi edinin [Azure Data Lake U-SQL SDK'sı](data-lake-analytics-u-sql-sdk.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake Analytics için bir CI/CD işlem hattı ayarlayın nasıl](data-lake-analytics-cicd-overview.md).
- [Azure Data Lake Analytics kodunuzu test etmek nasıl](data-lake-analytics-cicd-test.md).
