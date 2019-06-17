---
title: Azure Machine Learning - Team Data Science Process için senaryoları tanımlama
description: Team Data Science Process ile Tahmine dayalı analiz Gelişmiş yapmak için uygun senaryoları seçin.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6838f4db240a0712eece7a97bc2cfe99efb87215
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61429780"
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Azure Machine Learning’de gelişmiş analiz senaryoları
Bu makalede örnek veri kaynakları ve tarafından işlenebilen hedef senaryoları özetlenmektedir [Team Data Science işlem (TDSP)](overview.md). TDSP ekiplerin akıllı uygulamalar oluşturmaya birlikte çalışması sistematik bir yaklaşım sağlar. Burada sunulan senaryoları veri özelliklerine, kaynak konumları ve hedef Azure depolarında bağlıdır veri işleme iş akışı içinde kullanılabilir seçenekler gösterilmektedir.

**Karar ağacı** son bölümde sunulan seçerek verileri ve hedefi için uygun olan örnek senaryolar için.

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

Aşağıdaki bölümlerde, bir örnek senaryo sunar. Her senaryo, olası veri bilimi veya Gelişmiş analiz için akışı ve destekleyici Azure kaynakları listelenir.

> [!NOTE]
> **Tüm senaryolar için şunları yapmanız gerekir:**
> <br/>
> 
> * [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md)
>   <br/>
> * [Bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
> 
> 

## <a name="smalllocal"></a>Senaryo \#1: Küçük ve orta tablosal veri kümesinde bir yerel dosya
![Küçük ve orta yerel dosyaları][1]

#### <a name="additional-azure-resources-none"></a>Ek Azure kaynakları: None
1. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net/).
1. Bir veri kümesi karşıya yükleyin.
1. Karşıya yüklenen veri kümeleri ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="smalllocalprocess"></a>Senaryo \#2: Küçük ve orta boy veri kümesi işleme gerektiren yerel dosya
![Küçük ve orta yerel dosyaları işleme][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynakları: Azure sanal makinesi (Ipython not defteri sunucusu)
1. Ipython Notebook çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına karşıya yükleyin.
1. Önceden işler ve Azure depolama kapsayıcısından verilerine erişme, Ipython Notebook verileri temizleyin.
1. Temizlenen şekilde tablolu formunu dönüştürün.
1. Dönüştürülmüş verileri Azure blob'larda kaydedin.
1. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net/).
1. Kullanarak Azure bloblarından veri okuma [verileri içeri aktarma] [ import-data] modülü.
1. Alınan veri kümelerindeki ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="largelocal"></a>Senaryo \#3: Yerel dosyaları Azure BLOB'ları hedefleme, büyük veri kümesi
![Büyük yerel dosyaları][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynakları: Azure sanal makinesi (Ipython not defteri sunucusu)
1. Ipython Notebook çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına karşıya yükleyin.
1. Önceden işleme ve verileri Azure bloblarından veri erişimi, Ipython Notebook temizleyin.
1. Gerekirse temizlenmesi için tablolu formunu verileri dönüştürün.
1. Verileri araştırmak ve gerektiği gibi özellikler oluşturun.
1. Küçük ve orta veri örneği ayıklayın.
1. Örneklenen verileri Azure blob'larda kaydedin.
1. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net/).
1. Kullanarak Azure bloblarından veri okuma [verileri içeri aktarma] [ import-data] modülü.
1. Alınan veri kümelerindeki ile başlayan bir Azure Machine Learning deneme akış oluşturacaksınız.

## <a name="smalllocaltodb"></a>Senaryo \#4: Küçük ve orta veri kümesi bir Azure sanal Makine'de SQL Server'ı hedefleyen yerel dosya
![Küçük ve orta yerel dosyaları Azure SQL DB'ye][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
1. SQL Server + Ipython Notebook çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına karşıya yükleyin.
1. Önceden işleyebilir ve Azure depolama kapsayıcısında Ipython Not Defteri kullanarak verileri temizledik.
1. Gerekirse temizlenmesi için tablolu formunu verileri dönüştürün.
1. VM yerel dosyaya veri kaydetme (Ipython Notebook VM üzerinde çalışan, yerel sürücülere VM sürücülere bakın).
1. Verileri bir Azure sanal makinesinde çalışan SQL Server veritabanına yükleyin.
   
   Seçenek \#1: SQL Server Management Studio'yu kullanarak.
   
   * SQL Server sanal makinesi için oturum açma
   * SQL Server Management Studio'da çalıştırın.
   * Veritabanı ve hedef tablolar oluşturun.
   * Verileri VM yerel dosyadan yüklemek için yöntemleri içe toplu birini kullanın.
   
   Seçenek \#2: Ipython Notebook – Orta ve büyük veri kümeleri için değil tavsiye kullanma
   
   <!-- -->    
   * ODBC bağlantı dizesi, SQL Server VM üzerinde erişmek için kullanın.
   * Veritabanı ve hedef tablolar oluşturun.
   * Verileri VM yerel dosyadan yüklemek için yöntemleri içe toplu birini kullanın.
1. Verileri keşfedin, gerektiği gibi özellikler oluşturun. Özellikler, veritabanı tablolarında gerçekleştirilmiş gerekmez. unutmayın. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
1. Gerekli ve/veya istenen veri örnek boyutuna karar.
1. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net/).
1. Doğrudan SQL Server kullanarak verileri okumak [verileri içeri aktarma] [ import-data] modülü. Alanları ayıklar, Özellikler oluşturup doğrudan gerekirse veri örnekleri gerekli sorguyu yapıştırın [verileri içeri aktarma] [ import-data] sorgu.
1. Alınan veri kümelerindeki ile başlayan bir Azure Machine Learning deneme akış oluşturacaksınız.

## <a name="largelocaltodb"></a>Senaryo \#5: Büyük veri kümesinde bir yerel dosyalar, hedef Azure VM'de SQL Server
![Azure SQL DB'de büyük yerel dosyalar][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
1. SQL Server ve Ipython Notebook sunucusu çalıştıran bir Azure sanal makinesi oluşturun.
1. Verileri bir Azure depolama kapsayıcısına karşıya yükleyin.
1. (İsteğe bağlı) Önceden işleme ve verileri temizledik.
   
   a.  Ipython Notebook Azure'dan verilerine erişme, verileri önceden işleme ve temizleme
   
       blobs.
   
   b.  Gerekirse temizlenmesi için tablolu formunu verileri dönüştürün.
   
   c.  VM yerel dosyaya veri kaydetme (Ipython Notebook VM üzerinde çalışan, yerel sürücülere VM sürücülere bakın).
1. Verileri bir Azure sanal makinesinde çalışan SQL Server veritabanına yükleyin.
   
   a.  SQL Server sanal makinesi oturum açın.
   
   b.  Veri zaten kaydedilmemiş, Azure'dan veri dosyalarını indirir
   
       storage container to local-VM folder.
   
   c.  SQL Server Management Studio'da çalıştırın.
   
   d.  Veritabanı ve hedef tablolar oluşturun.
   
   e.  Verileri yüklemek için yöntemleri içe toplu birini kullanın.
   
   f.  Tablo birleştirmelerde gerekiyorsa, birleştirmeler hızlandırmak için dizin oluşturun.
   
   > [!NOTE]
   > Büyük boyutlu veriler için daha hızlı yüklenmesini bölümlenmiş tablolar oluşturmak ve toplu verileri paralel alma önerilir. Daha fazla bilgi için [SQL bölümlenmiş tablolar için paralel veri içeri aktarma](parallel-load-sql-partitioned-tables.md).
   > 
   > 
1. Verileri keşfedin, gerektiği gibi özellikler oluşturun. Özellikler, veritabanı tablolarında gerçekleştirilmiş gerekmez. unutmayın. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
1. Gerekli ve/veya istenen veri örnek boyutuna karar.
1. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net/).
1. Doğrudan SQL Server kullanarak verileri okumak [verileri içeri aktarma] [ import-data] modülü. Alanları ayıklar, Özellikler oluşturup doğrudan gerekirse veri örnekleri gerekli sorguyu yapıştırın [verileri içeri aktarma] [ import-data] sorgu.
1. Karşıya yüklenen veri kümesi ile başlayan basit Azure Machine Learning deneme akış

## <a name="largedbtodb"></a>Senaryo \#6: Bir Azure sanal Makine'de SQL Server'ı hedefleyen, şirket içinde bir SQL Server veritabanında büyük veri kümesi
![Büyük SQL DB şirket içinden Azure SQL DB'de][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
1. SQL Server ve Ipython Notebook sunucusu çalıştıran bir Azure sanal makinesi oluşturun.
1. Veri birini verileri SQL Server'dan döküm dosyalarını dışarı aktarmak için yöntemleri dışarı aktarın.
   
   > [!NOTE]
   > Tüm verileri şirket içi veritabanı, azure'da SQL Server örneği için tam veritabanını taşımak için bir alternatif (hızlı) yöntemi taşıma karar verirseniz. Verileri dışarı aktarma, veritabanı oluşturun ve hedef veritabanına veri yükleme/içeri aktarma ve Alternatif yöntem izleyin adımı atlayın.
   > 
   > 
1. Döküm dosyaları Azure depolama kapsayıcısına karşıya yükleyin.
1. Verileri bir Azure sanal makinesinde çalışan SQL Server veritabanına yükleyin.
   
   a.  SQL Server sanal makinesi oturum açın.
   
   b.  Veri dosyalarını bir Azure depolama kapsayıcısından VM yerel klasöre indirir.
   
   c.  SQL Server Management Studio'da çalıştırın.
   
   d.  Veritabanı ve hedef tablolar oluşturun.
   
   e.  Verileri yüklemek için yöntemleri içe toplu birini kullanın.
   
   f.  Tablo birleştirmelerde gerekiyorsa, birleştirmeler hızlandırmak için dizin oluşturun.
   
   > [!NOTE]
   > Büyük veri boyutları, daha hızlı yükleme oluşturmak için bölümlenmiş tabloları ve toplu verileri paralel içeri aktarın. Daha fazla bilgi için [SQL bölümlenmiş tablolar için paralel veri içeri aktarma](parallel-load-sql-partitioned-tables.md).
   > 
   > 
1. Verileri keşfedin, gerektiği gibi özellikler oluşturun. Özellikler, veritabanı tablolarında gerçekleştirilmiş gerekmez. unutmayın. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
1. Gerekli ve/veya istenen veri örnek boyutuna karar.
1. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net/).
1. Doğrudan SQL Server kullanarak verileri okumak [verileri içeri aktarma] [ import-data] modülü. Alanları ayıklar, Özellikler oluşturup doğrudan gerekirse veri örnekleri gerekli sorguyu yapıştırın [verileri içeri aktarma] [ import-data] sorgu.
1. Basit Azure Machine Learning deneme akış karşıya yüklenen veri kümesi ile başlatılıyor.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Tam bir veritabanı bir şirket içi SQL Server'dan Azure SQL veritabanı'na kopyalamak için alternatif yöntem
![Yerel DB ayırabilir ve Azure SQL DB'de ekleme][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynakları: Azure sanal makinesi (SQL Server / Ipython not defteri sunucusu)
Tüm SQL Server VM'nize SQL Server veritabanında çoğaltmak için bir veritabanı bir konum/sunucudan diğerine, veritabanı geçici olarak çevrimdışı duruma getirilmesini varsayılarak kopyalamanız gerekir. Bu SQL Server Management Studio nesne Gezgini'nde veya eşdeğer Transact-SQL komutlarını kullanarak yapın.

1. Kaynak konumundaki veritabanı ayrılmadı. Daha fazla bilgi için [bir veritabanını ayırma](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
1. Windows Gezgini veya Windows komut istemi penceresinde, ayrılan veritabanı dosya veya dosyalar ve günlük dosya veya dosyalar azure'da SQL Server sanal makinesi üzerinde hedef konuma kopyalayın.
1. Kopyalanan dosyalar, hedef SQL Server örneğinde ekleyin. Daha fazla bilgi için [veritabanı ekleme](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Ayırma ve ekleme (Transact-SQL) kullanarak bir veritabanı taşıma](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>Senaryo \#7: Büyük verilerle ilgili yerel dosyalar, hedef Azure HDInsight Hadoop kümeleri Hive veritabanı
![Yerel hedef Hive büyük veri][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynakları: Azure HDInsight Hadoop kümesi ve Azure sanal makine (Ipython not defteri sunucusu)
1. Ipython Notebook sunucusu çalıştıran bir Azure sanal makinesi oluşturun.
1. Azure HDInsight Hadoop kümesi oluşturun.
1. (İsteğe bağlı) Önceden işleme ve verileri temizledik.
   
   a.  Ipython Notebook Azure'dan verilerine erişme, verileri önceden işleme ve temizleme
   
       blobs.
   
   b.  Gerekirse temizlenmesi için tablolu formunu verileri dönüştürün.
   
   c.  VM yerel dosyaya veri kaydetme (Ipython Notebook VM üzerinde çalışan, yerel sürücülere VM sürücülere bakın).
1. 2 adımda seçtiğiniz Hadoop kümesi varsayılan kapsayıcı veri yükleyin.
1. Azure HDInsight Hadoop kümesindeki Hive veritabanı verileri yükleyin.
   
   a.  Hadoop kümesinin baş düğümünü için oturum açın
   
   b.  Hadoop komut satırı açın.
   
   c.  Komutu tarafından Hive kök dizini girin `cd %hive_home%\bin` , Hadoop komut satırı.
   
   d.  Veritabanı ve tablo oluşturma için Hive sorguları çalıştırmak ve verileri blob depolama alanından Hive tablolarına yükleme.
   
   > [!NOTE]
   > Veriler büyük ise, kullanıcıların bölümlerle Hive tablosu oluşturabilirsiniz. Kullanıcılar daha sonra kullanabileceğiniz bir `for` döngü içinde Hadoop bölümü bölümlenmiş Hive tablosuna veri yüklemek için komut satırını baş düğüm üzerinde.
   > 
   > 
1. Verileri araştırmak ve Hadoop komut satırında gerektiği gibi özellikler oluşturun. Özellikler, veritabanı tablolarında gerçekleştirilmiş gerekmez. unutmayın. Yalnızca bunları oluşturmak için gerekli sorguyu unutmayın.
   
   a.  Hadoop kümesinin baş düğümünü için oturum açın
   
   b.  Hadoop komut satırı açın.
   
   c.  Komutu tarafından Hive kök dizini girin `cd %hive_home%\bin` , Hadoop komut satırı.
   
   d.  Hive sorguları, verileri araştırabilir ve gerektiği gibi özellikler oluşturmak için Hadoop kümesi baş düğümünde Hadoop komut satırında çalıştırın.
1. Gerekli ve/veya istenen, verileri Azure Machine Learning Studio'da uyacak şekilde örnek.
1. Oturum [Azure Machine Learning Studio'yu](https://studio.azureml.net/).
1. Verileri doğrudan okumak `Hive Queries` kullanarak [verileri içeri aktarma] [ import-data] modülü. Alanları ayıklar, Özellikler oluşturup doğrudan gerekirse veri örnekleri gerekli sorguyu yapıştırın [verileri içeri aktarma] [ import-data] sorgu.
1. Basit Azure Machine Learning deneme akış karşıya yüklenen veri kümesi ile başlatılıyor.

## <a name="decisiontree"></a>Senaryo seçimi için karar ağacı
- - -
Aşağıdaki diyagramda, yukarıda açıklanan senaryoları ve Gelişmiş analitik işlemi ve listelenen senaryolar için aldığınız yapılan teknoloji seçimleri özetler. Veri işleme, keşfetme, özellik Mühendisliği ve örnekleme sürebilir unutmayın, bir veya daha fazla kaynakta, Ara, / ortam--yöntemi ve/veya hedef ortamlara – içinde yerleştirin ve gerektiğinde çalıştırmalarınızı devam edebilirsiniz. Diyagram yalnızca bazı olası akışlar çizim görev yapar ve kapsamlı bir numaralandırma sağlamaz.

![DS işlem kılavuzu senaryolar][8]

### <a name="advanced-analytics-in-action-examples"></a>Örnek uygulamada Gelişmiş analiz
Gelişmiş analitik işlemi ve teknolojisi genel veri kümeleri kullanılarak kullanan uçtan uca Azure Machine Learning izlenecek yollar için bkz:

* [Team Data Science Process eylemi: SQL Server'ı kullanarak](sql-walkthrough.md).
* [Team Data Science Process eylemi: HDInsight Hadoop kümeleri kullanarak](hive-walkthrough.md).

[1]: ./media/plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
