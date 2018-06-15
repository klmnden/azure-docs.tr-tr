---
title: U-SQL betiklerini yerel olarak Azure Data Lake U-SQL SDK'sını kullanarak çalıştırma
description: Bu makalede Azure Data Lake araçları Visual Studio için test ve U-SQL işleri, yerel iş istasyonunda hata ayıklamak için nasıl kullanılacağını açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: mumian
ms.author: yanacai
manager: kfile
editor: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.topic: conceptual
ms.date: 11/15/2016
ms.openlocfilehash: 322278f00f49f718b1ba560e9d21d0af0be49b18
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736012"
---
# <a name="runing-u-sql-scripts-locally"></a>U-SQL betiklerini yerel olarak çalıştırma

U-SQL Azure'da çalışan yerine kendi kutusunu U-SQL çalıştırabilirsiniz. Bu, "yerel Çalıştırma" veya "yerel Çalıştırma" adı verilir. 

U-SQL yerel çalıştırması, bu Araçları'nda kullanılabilir olan:
* Visual Studio için Azure Data Lake araçları
* Azure Data Lake U-SQL SDK'sı

## <a name="understand-the-data-root-folder-and-the-file-path"></a>Veri kök klasör ve dosya yolu anlama

Yerel çalıştırma ve U-SQL SDK'sı veri kök klasör gerektirir. Veri kök klasörü "yerel depolama" yerel işlem hesabıdır. Bir Data Lake Analytics hesabı Azure Data Lake Store hesabına eşdeğerdir. Farklı bir veri kök klasörüne değiştirme yalnızca farklı depolama hesabına geçişi gibi olur. Farklı bir veri kök klasörlerle yaygın olarak Paylaşılan verilere erişmek istiyorsanız, komut dosyalarınızı mutlak yollar kullanmanız gerekir. Veya dosya sistemi sembolik bağlantılar oluşturun (örneğin, **mklink** NTFS) için paylaşılan veri noktası için veri kök klasörü altında.

Veri kök klasör için kullanılır:

- Veritabanları, tablolar, tablo değerli işlevler (Tvf) ve derlemeler çeşitli meta verileri depolar.
- U-SQL göreli yolda olarak tanımlanan giriş ve çıkış yol arayın. Göreli yollar kullanarak U-SQL projelerinizi Azure'a dağıtmak kolaylaştırır.

U-SQL betikleri göreli bir yol ve yerel bir mutlak yolu kullanabilirsiniz. Göreli yolu göreli belirtilen veri kök klasör yoludur. Kullanmanızı öneririz "/" komut dosyalarınızı sunucu tarafı ile uyumlu hale getirmek için yol ayırıcısı olarak. Göreli yollar ve eşdeğer mutlak yollarına bazı örnekleri aşağıda verilmiştir. Bu örneklerde, C:\LocalRunDataRoot veri kök klasörüdür.

|Göreli yol|Mutlak yolu|
|-------------|-------------|
|/ABC/DEF/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Kullanım yerel Visual Studio'dan çalıştırma

Visual Studio için Data Lake araçları, Visual Studio'da U-SQL yerel çalıştırma deneyimi sağlar. Bu özelliği kullanarak şunları yapabilirsiniz:

- C# derlemeleri'nin yanı sıra yerel olarak bir U-SQL komut dosyasını çalıştırın.
- Bir C# derleme yerel olarak hata ayıklama.
- Oluşturun, görüntüleyin ve U-SQL katalogları (yerel veritabanlarını, derlemeleri, şemaları ve tabloları) Server Explorer'dan silin. Yerel katalog de ayrıca ve Sunucu Gezgini'nden bulabilirsiniz.

    ![Visual Studio yerel çalıştırma yerel katalog için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

Data Lake araçları yükleyici varsayılan veri kök klasör olarak kullanılacak bir C:\LocalRunRoot klasör oluşturur. Varsayılan yerel çalıştırma paralellik 1'dir.

### <a name="to-configure-local-run-in-visual-studio"></a>Visual Studio'da yerel çalıştırma yapılandırmak için

1. Visual Studio'yu açın.
2. Açık **Sunucu Gezgini**.
3. Genişletme **Azure** > **Data Lake Analytics**.
4. Tıklatın **Data Lake** menüsüne ve ardından **seçenekleri ve ayarları**.
5. Sol ağacında **Azure Data Lake**, genişletin ve ardından **genel**.

    ![Yerel çalıştırma Visual Studio için Data Lake araçları ayarlarını yapılandırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

Visual Studio U-SQL projesi çalıştırma yerel gerçekleştirmek için gerekli değildir. Bu bölümü, U-SQL betikleri Azure'dan çalışmasını farklıdır.

### <a name="to-run-a-u-sql-script-locally"></a>U-SQL komut dosyasını yerel olarak çalıştırmak için
1. Visual Studio'dan U-SQL projenizi açın.   
2. Çözüm Gezgini'nde U-SQL komut dosyasını sağ tıklatın ve ardından **betiği Gönder**.
3. Seçin **(yerel)** , komut yerel olarak çalıştırmak için Analytics hesabı olarak.
Tıklatarak **(yerel)** hesap komut dosyası penceresinin üst kısmında ve ardından **gönderme** (veya Ctrl + F5 kullanın klavye kısayolu).

    ![Visual Studio yerel çalıştırma gönderme işleri için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a>Betikler ve C# derlemeleri üzerinde yerel olarak hata ayıklama

C# derlemeleri göndererek ve Azure Data Lake Analytics Hizmeti'ne kaydetme ayıklayabilirsiniz. Hem dosyanın arkasındaki kodda hem de başvuruda bulunulan bir C# projesinde kesme noktaları ayarlayabilirsiniz.

#### <a name="to-debug-local-code-in-code-behind-file"></a>Dosyanın arkasındaki kodda yerel kod hatalarını ayıklamak için

1. Dosyanın arkasındaki kodda kesme noktaları ayarlayın.
2. Betik üzerinde yerel olarak hata ayıklama için F5 tuşuna basın.

> [!NOTE]
   > Aşağıdaki yordam yalnızca Visual Studio 2015'te çalışır. Daha eski Visual Studio sürümlerinde, pdb dosyalarını kendiniz eklemeniz gerekebilir.  
   >
   >

#### <a name="to-debug-local-code-in-a-referenced-c-project"></a>Başvuruda bulunulan bir C# projesinde yerel kod hatalarını ayıklamak için

1. Bir C# Derleme projesi oluşturun ve projeyi çıktı dll'sini üretmek üzere oluşturun.
2. U-SQL deyimi kullanarak dll'yi kaydetme:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. C# kodunda kesme noktalarını ayarlayın.
4. C# DLL'sine yerel olarak başvuran ile komut dosyası hata ayıklamak için F5 tuşuna basın.

## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a>Data Lake U-SQL SDK'dan çalıştırmak yerel kullan

Visual Studio kullanarak yerel olarak U-SQL betikleri çalıştırmanın yanı sıra, U-SQL betiklerini yerel olarak komut satırı ve programlama arabirimleri ile çalıştırmak için Azure Data Lake U-SQL SDK'sını kullanabilirsiniz. Bunlar arasında U-SQL yerel testinizi ölçeklendirebilirsiniz.

Daha fazla bilgi edinmek [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).


## <a name="next-steps"></a>Sonraki adımlar

* Daha karmaşık bir sorgu görmek için bkz: [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
* İş ayrıntılarını görüntülemek için bkz: [kullanım iş tarayıcı ve Azure Data Lake Analytics işleri için iş görünümünde](data-lake-analytics-data-lake-tools-view-jobs.md).
* Köşe yürütme görünümü kullanmak için bkz: [köşe yürütme görünümü Visual Studio için Data Lake araçları kullanmak](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
