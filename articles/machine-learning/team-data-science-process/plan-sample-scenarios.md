---
title: Azure Machine Learning için gelişmiş analizler senaryolarını belirle | Microsoft Docs
description: Tahmine dayalı analiz takım veri bilimi işlemine Gelişmiş yapmak için uygun senaryolar seçin.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: a194061bf443f50bed673a518ab62746f67b4a78
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838168"
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Azure Machine Learning’de gelişmiş analiz senaryoları
Bu makalede birçok örnek veri kaynakları ve tarafından işlenen hedef senaryoları özetlenmektedir [takım veri bilimi işlem (TDSP)](overview.md). TDSP akıllı uygulamaları derleme işbirliği yapmak ekipler için sistematik bir yaklaşım sağlar. Burada sunulan senaryoları veri özellikleri, kaynak konumları ve Azure hedef depoları bağlıdır veri işleme iş akışı içinde kullanılabilir seçenekler gösterilmektedir.

**Karar ağacı** veri ve hedefi için uygun olan örnek senaryolar seçerek son bölümde sunulan için.

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

Aşağıdaki bölümlerde bir örnek senaryo sunar. Her senaryo, olası veri bilimi veya gelişmiş analizler için akışı ve Azure kaynaklarını destekleyen listelenir.

> [!NOTE]
> **Aşağıdaki senaryoların tümünde için şunları yapmanız gerekir:**
> <br/>
> 
> * [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md)
>   <br/>
> * [Bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md)
> 
> 

## <a name="smalllocal"></a>Senaryo \#1: küçük ve orta tablolu bir veri kümesini yerel dosyaları
![Küçük ve orta yerel dosyaları][1]

#### <a name="additional-azure-resources-none"></a>Ek Azure kaynaklarını: yok
1. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/).
2. Bir veri kümesi karşıya yükleyin.
3. Karşıya yüklenen veri kümeleri ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="smalllocalprocess"></a>Senaryo \#2: küçük ve orta dataset işleme gerektiren yerel dosyaları
![Küçük ve orta yerel dosyaları işleme ile][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (IPython not defteri sunucusu)
1. IPython dizüstü çalıştıran bir Azure sanal makine oluşturun.
2. Verileri bir Azure depolama kapsayıcısı karşıya yükleyin.
3. Önceden işleyebilir ve verileri Azure depolama kapsayıcıdan verilere IPython not defterinde Temizle.
4. Temizlenen için tablo form verileri dönüştürün.
5. Dönüştürülen veriler Azure BLOB'ları kaydedin.
6. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/).
7. Verileri kullanarak Azure bloblarından okuma [veri içeri aktarma] [ import-data] modülü.
8. Alınan veri kümeleri ile başlayan bir Azure Machine Learning deneme akışı oluşturun.

## <a name="largelocal"></a>Senaryo \#3: büyük veri kümesi yerel dosyaların Azure BLOB'ları hedefleme
![Büyük yerel dosyaları][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (IPython not defteri sunucusu)
1. IPython dizüstü çalıştıran bir Azure sanal makine oluşturun.
2. Verileri bir Azure depolama kapsayıcısı karşıya yükleyin.
3. Önceden işleyebilir ve IPython Azure bloblarından verilere dizüstü bilgisayar, veri temizleme.
4. Gerekirse temizlenmesi için Tablo formunda, verileri dönüştürün.
5. Verileri araştırmak ve Özellikler gerektiği şekilde oluşturun.
6. Küçük ve orta veri örneği ayıklayın.
7. Örneklenen verileri Azure BLOB'ları kaydedin.
8. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Verileri kullanarak Azure bloblarından okuma [veri içeri aktarma] [ import-data] modülü.
10. Alınan veri kümeleri ile başlayan Azure Machine Learning deneme akış oluşturun.

## <a name="smalllocaltodb"></a>Senaryo \#4: küçük ve orta dataset yerel dosyaları, SQL Server içinde bir Azure sanal makine hedefleme
![Küçük ve orta yerel SQL Azure DB'de dosyaları][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / IPython dizüstü sunucu)
1. SQL Server + IPython dizüstü çalıştıran bir Azure sanal makine oluşturun.
2. Verileri bir Azure depolama kapsayıcısı karşıya yükleyin.
3. Önceden işleyebilir ve IPython Not Defteri kullanarak Azure depolama kapsayıcısının verileri temizleyin.
4. Gerekirse temizlenmesi için Tablo formunda, verileri dönüştürün.
5. VM yerel dosyalar için verileri kaydetmek (IPython dizüstü bilgisayar, VM üzerinde çalıştığından, yerel sürücülere VM sürücülere bakın).
6. Bir Azure VM'de çalışan SQL Server veritabanına veri yükleyin.
   
   Seçenek \#1: SQL Server Management Studio'yu kullanarak.
   
   * SQL Server'da VM oturum açma
   * SQL Server Management Studio'yu çalıştırın.
   * Veritabanı ve hedef tablolar oluşturun.
   * Toplu birini kullanın VM yerel dosyalarından veri yüklemek için yöntemleri içeri aktarın.
   
   Seçenek \#2: kullanarak IPython not defteri – Orta ve büyük veri kümeleri için değil önerilir
   
   <!-- -->    
   * SQL Server VM üzerinde erişmek için ODBC bağlantı dizesi kullanın.
   * Veritabanı ve hedef tablolar oluşturun.
   * Toplu birini kullanın VM yerel dosyalarından veri yüklemek için yöntemleri içeri aktarın.
7. Özellikler gerektiği şekilde oluşturun, verileri keşfedin. Özellikleri veritabanı tablolarında gerçekleştirilmesi gerekmez unutmayın. Yalnızca bunları oluşturmak için gerekli sorgu unutmayın.
8. Gerekli ve/veya istenen veri örnek boyutu üzerinde karar.
9. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/).
10. SQL Server kullanarak doğrudan gelen verileri okumak [veri içeri aktarma] [ import-data] modülü. Alanları ayıklar, özellikleri oluşturur ve veri doğrudan gerekirse örnekleri gerekli sorgu Yapıştır [veri içeri aktarma] [ import-data] sorgu.
11. Alınan veri kümeleri ile başlayan Azure Machine Learning deneme akış oluşturun.

## <a name="largelocaltodb"></a>Senaryo \#5: yerel dosyaları büyük bir veri kümesini hedef Azure VM'deki SQL Server
![SQL Azure DB'de büyük yerel dosyalar][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / IPython dizüstü sunucu)
1. SQL Server ve IPython dizüstü server çalıştıran bir Azure sanal makine oluşturun.
2. Verileri bir Azure depolama kapsayıcısı karşıya yükleyin.
3. (İsteğe bağlı) Önceden işleyebilir ve veri temizleme.
   
   a.  Önceden işleyebilir ve IPython Azure'dan verilere dizüstü bilgisayar, veri temizleme
   
       blobs.
   
   b.  Gerekirse temizlenmesi için Tablo formunda, verileri dönüştürün.
   
   c.  VM yerel dosyalar için verileri kaydetmek (IPython dizüstü bilgisayar, VM üzerinde çalıştığından, yerel sürücülere VM sürücülere bakın).
4. Bir Azure VM'de çalışan SQL Server veritabanına veri yükleyin.
   
   a.  SQL Server VM oturum açın.
   
   b.  Veri zaten kaydedilmemişse, veri dosyalarını Azure'dan indirir.
   
       storage container to local-VM folder.
   
   c.  SQL Server Management Studio'yu çalıştırın.
   
   d.  Veritabanı ve hedef tablolar oluşturun.
   
   e.  Verileri yüklemek için yöntemleri içe toplu birini kullanın.
   
   f.  Tablo birleştirme gerekirse, birleştirmeler hızlandırmak için dizinler oluşturun.
   
   > [!NOTE]
   > Büyük veri boyutları hızlı yükleme için bölümlenmiş tabloları oluşturma ve toplu içeri aktarma verileri paralel önerilir. Daha fazla bilgi için bkz: [paralel veri içe aktarma SQL bölümlenmiş tablolar](parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Özellikler gerektiği şekilde oluşturun, verileri keşfedin. Özellikleri veritabanı tablolarında gerçekleştirilmesi gerekmez unutmayın. Yalnızca bunları oluşturmak için gerekli sorgu unutmayın.
6. Gerekli ve/veya istenen veri örnek boyutu üzerinde karar.
7. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/).
8. SQL Server kullanarak doğrudan gelen verileri okumak [veri içeri aktarma] [ import-data] modülü. Alanları ayıklar, özellikleri oluşturur ve veri doğrudan gerekirse örnekleri gerekli sorgu Yapıştır [veri içeri aktarma] [ import-data] sorgu.
9. Karşıya yüklenen veri kümesi ile başlayan basit Azure Machine Learning deneme akışı

## <a name="largedbtodb"></a>Senaryo \#6: bir SQL Server veritabanı SQL Server içinde bir Azure sanal makine hedefleme şirket içi, büyük bir veri kümesini
![Büyük SQL DB şirket içi SQL Azure DB'de için][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / IPython dizüstü sunucu)
1. SQL Server ve IPython dizüstü server çalıştıran bir Azure sanal makine oluşturun.
2. Veri birini kullanın veri döküm dosyaları için SQL Server'dan dışarı aktarmak için yöntemleri verin.
   
   > [!NOTE]
   > Şirket içi veritabanından azure'da SQL Server örneği için tam veritabanını taşımak için bir alternatif (hızlı) yöntemi tüm verileri taşımaya karar verirseniz. Verileri dışarı aktarma, veritabanı, oluşturma ve yük/hedef veritabanına veri içeri aktarma ve Alternatif yöntem izleyin adımları atlayın.
   > 
   > 
3. Azure depolama kapsayıcısı için döküm dosyalarını yükleyin.
4. Bir Azure sanal makinede çalışan SQL Server veritabanına veri yükleyin.
   
   a.  SQL Server VM oturum açın.
   
   b.  Veri dosyaları bir Azure depolama kapsayıcıdan VM yerel klasöre yükleyin.
   
   c.  SQL Server Management Studio'yu çalıştırın.
   
   d.  Veritabanı ve hedef tablolar oluşturun.
   
   e.  Verileri yüklemek için yöntemleri içe toplu birini kullanın.
   
   f.  Tablo birleştirme gerekirse, birleştirmeler hızlandırmak için dizinler oluşturun.
   
   > [!NOTE]
   > Büyük veri boyutları, hızlı yükleme oluşturmak için bölümlenmiş tablolar ve toplu paralel veri içeri aktarın. Daha fazla bilgi için bkz: [paralel veri içe aktarma SQL bölümlenmiş tablolar](parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Özellikler gerektiği şekilde oluşturun, verileri keşfedin. Özellikleri veritabanı tablolarında gerçekleştirilmesi gerekmez unutmayın. Yalnızca bunları oluşturmak için gerekli sorgu unutmayın.
6. Gerekli ve/veya istenen veri örnek boyutu üzerinde karar.
7. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/).
8. SQL Server kullanarak doğrudan gelen verileri okumak [veri içeri aktarma] [ import-data] modülü. Alanları ayıklar, özellikleri oluşturur ve veri doğrudan gerekirse örnekleri gerekli sorgu Yapıştır [veri içeri aktarma] [ import-data] sorgu.
9. Basit Azure Machine Learning ile karşıya yüklenen dataset başlangıç akışı deneyin.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Tam bir veritabanı bir şirket içi SQL Server'dan Azure SQL veritabanına kopyalamak için alternatif yöntemi
![Yerel DB detach ve Azure SQL DB ekleme][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure sanal makinesi (SQL Server / IPython dizüstü sunucu)
SQL Server VM'nize tüm SQL Server veritabanında çoğaltmak için bir veritabanı bir konum/sunucudan diğerine veritabanı geçici olarak çevrimdışı duruma olduğunu varsayarak kopyalamanız gerekir. Bu SQL Server Management Studio Object Explorer veya eşdeğer Transact-SQL komutlarını kullanarak yapın.

1. Kaynak konumda veritabanı ayrılmadı. Daha fazla bilgi için bkz: [bir veritabanını ayırma](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
2. Windows Explorer ya da Windows komut istemi penceresinde, ayrılmış veritabanına dosya veya dosyalar ve günlük dosyasının veya dosyalarının azure'da SQL Server VM üzerinde hedef konuma kopyalayın.
3. Kopyalanan dosyalar hedef SQL Server örneğine ekleyin. Daha fazla bilgi için bkz: [bir veritabanını](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Kullanarak bir veritabanı ayırma ve (Transact-SQL) ekleme taşıma](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>Senaryo \#7: yerel dosyalarında büyük veri hedef Azure Hdınsight Hadoop kümeleri Hive veritabanında
![Yerel hedef Hive büyük verileri][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Ek Azure kaynaklarını: Azure Hdınsight Hadoop kümesi ve Azure sanal makine (IPython not defteri sunucusu)
1. IPython dizüstü server çalıştıran bir Azure sanal makine oluşturun.
2. Bir Azure Hdınsight Hadoop kümesi oluşturun.
3. (İsteğe bağlı) Önceden işleyebilir ve veri temizleme.
   
   a.  Önceden işleyebilir ve IPython Azure'dan verilere dizüstü bilgisayar, veri temizleme
   
       blobs.
   
   b.  Gerekirse temizlenmesi için Tablo formunda, verileri dönüştürün.
   
   c.  VM yerel dosyalar için verileri kaydetmek (IPython dizüstü bilgisayar, VM üzerinde çalıştığından, yerel sürücülere VM sürücülere bakın).
4. 2. adımda seçilen Hadoop kümesi varsayılan kapsayıcı için veri yükleyin.
5. Azure Hdınsight Hadoop kümesindeki Hive veritabanına veri yükleyin.
   
   a.  Hadoop küme baş düğümüne oturum açın
   
   b.  Hadoop komut satırı açın.
   
   c.  Komutu tarafından Hive kök dizini girin `cd %hive_home%\bin` , Hadoop komut satırı.
   
   d.  Veritabanı ve tablo oluşturmak için Hive sorguları çalıştırmak ve verileri blob depolama alanından Hive tablolara yüklemek.
   
   > [!NOTE]
   > Verileri büyük ise, kullanıcıların bölümlerle Hive tablosu oluşturabilirsiniz. Kullanıcılar daha sonra kullanabileceğiniz bir `for` döngü içinde Hadoop bölüm bölümlenmiş Hive tablosu verileri yüklemek için komut satırını baş düğüm.
   > 
   > 
6. Verileri araştırmak ve özellikleri Hadoop komut satırında gerektiği şekilde oluşturun. Özellikleri veritabanı tablolarında gerçekleştirilmesi gerekmez unutmayın. Yalnızca bunları oluşturmak için gerekli sorgu unutmayın.
   
   a.  Hadoop küme baş düğümüne oturum açın
   
   b.  Hadoop komut satırı açın.
   
   c.  Komutu tarafından Hive kök dizini girin `cd %hive_home%\bin` , Hadoop komut satırı.
   
   d.  Hive sorguları Hadoop komut satırında verileri araştırmak ve gerektiği gibi özellikleri oluşturmak için Hadoop kümesi baş düğümünde çalıştırın.
7. Gerekli ve/veya istenen, Azure Machine Learning Studio'da sığması için veri örneği.
8. Oturum [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Verileri doğrudan okumak `Hive Queries` kullanarak [veri içeri aktarma] [ import-data] modülü. Alanları ayıklar, özellikleri oluşturur ve veri doğrudan gerekirse örnekleri gerekli sorgu Yapıştır [veri içeri aktarma] [ import-data] sorgu.
10. Basit Azure Machine Learning ile karşıya yüklenen dataset başlangıç akışı deneyin.

## <a name="decisiontree"></a>Senaryo seçimi için karar ağacı
- - -
Aşağıdaki diyagramda, yukarıda açıklanan senaryoları ve Gelişmiş analiz işlem ve teknoloji seçimleri dökümü senaryoları için aldığınız yapılan özetler. Veri işleme, keşfi, özellik Mühendisliği ve örnekleme alabileceğini unutmayın, bir veya daha fazla yöntem/ortam--kaynaktaki, Ara, ve/veya hedef ortamları – içinde yerleştirin ve gerektiğinde tekrarlayarak devam edebilirsiniz. Diyagram yalnızca bazı olası akışlarının çizim görev yapar ve kapsamlı bir numaralandırma sağlamaz.

![DS işlemi izlenecek senaryolar][8]

### <a name="advanced-analytics-in-action-examples"></a>Gelişmiş analizler eylem örnekleri
Gelişmiş Analytics işlemi ve ortak veri kümeleri kullanarak teknolojisi kullanan uçtan uca Azure Machine Learning talimatlar için bkz:

* [Veri bilimi işlemi eylemde ekip: SQL Server'ı kullanarak](sql-walkthrough.md).
* [Veri bilimi işlemi eylemde ekip: Hdınsight Hadoop kümeleri kullanarak](hive-walkthrough.md).

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
