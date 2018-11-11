---
title: Machine Learning Studio'ya çevrimiçi veri kaynaklarından alınan verileri içeri aktarma | Microsoft Docs
description: Azure Machine Learning Studio'da eğitim verilerinizi çeşitli çevrimiçi kaynaklardan içeri aktarma.
keywords: verileri, veri biçimi, veri türleri, veri kaynakları, eğitim verilerini içeri aktarma
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 87a7e968073d8625375ea837f9377145b6dfb45a
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344870"
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Veri Alma modülü ile çeşitli çevrimiçi veri kaynaklarından Azure Machine Learning Studio’ya veri alma
Bu makalede, çevrimiçi verileri çeşitli kaynaklar ve bir Azure Machine Learning denemesine bu kaynaklardan veri taşımak için gereken bilgileri almak için destek açıklanır.

> [!NOTE]
> Bu makalede, hakkında genel bilgiler sağlanmaktadır. [verileri içeri aktarma] [ import-data] modülü. Erişebileceğiniz veri türleri hakkında daha ayrıntılı bilgi için biçimleri, parametreleri ve sık sorulan soruların yanıtlarını modülü başvurusu için konusuna [verileri içeri aktarma] [ import-data] modülü.
> 
> 

## <a name="introduction"></a>Giriş
Kullanarak [verileri içeri aktarma] [ import-data] modülü erişebilirsiniz veri çeşitli çevrimiçi veri kaynaklarından biri denemenizi çalışırken [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* HTTP kullanarak bir Web URL'si
* Hadoop HiveQL kullanma
* Azure blob depolama
* Azure tablosu
* Azure SQL veritabanı ya da Azure vm'lerde SQL Server
* Şirket içi SQL Server veritabanı
* Bir veri sağlayıcısı, şu anda OData akışı
* Azure Cosmos DB

Studio denemenizi, çevrimiçi veri kaynaklarına erişmek için ekleme [verileri içeri aktarma] [ import-data] modülü, seçin **veri kaynağı**ve erişmek için gerekli parametreleri belirtin veriler. Desteklenen çevrimiçi veri kaynakları, aşağıdaki tabloda listelenen. Bu tablo, desteklenen dosya biçimleri ve verilere erişmek için kullanılan parametreleri de özetler.

Bu eğitim verilerini denemenizi çalışırken erişildiği için yalnızca bu deneme kullanılabilir olduğunu unutmayın. Buna karşılık olarak çalışma alanınızdaki tüm denemenize bir veri kümesi modülde depolanan veriler kullanılabilir.

> [!IMPORTANT]
> Şu anda [verileri içeri aktarma] [ import-data] ve [verileri dışarı aktarma] [ export-data] modülleri okuma ve yalnızca klasik kullanılarak oluşturulan Azure Depolama'dan veri yazma dağıtım modeli. Diğer bir deyişle, sık erişimli depolama erişim katmanı veya seyrek erişimli depolama erişim katmanı sağlayan yeni Azure Blob Depolama hesap türü henüz desteklenmiyor. 
> 
> Tüm Azure depolama hesaplarını genel olarak, bu hizmet seçeneği kullanılabilir olmadan önce oluşturmuş olabileceğiniz olduğunu etkilenmez. 
> Yeni bir hesap oluşturmanız gerekiyorsa, seçin **Klasik** dağıtım modeli veya Kaynak Yöneticisi'ni kullanın ve seçin **genel amaçlı** yerine **Blob Depolama** için **Hesap türü**. 
> 
> Daha fazla bilgi için [Azure Blob Depolama: sık erişimli ve seyrek erişimli depolama katmanları](../../storage/blobs/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Desteklenen çevrimiçi veri kaynakları
Azure Machine Learning **verileri içeri aktarma** Modülü aşağıdaki veri kaynaklarını destekler:

| Veri Kaynağı | Açıklama | Parametreler |
| --- | --- | --- |
| HTTP üzerinden Web URL'si |Verileri virgülle ayrılmış değerler (CSV), sekmeyle ayrılmış değerler (TSV), öznitelik-ilişki dosyası biçimi'ne (ARFF) ve Destek vektör makineler (ışık SVM) biçimleri, HTTP kullanan herhangi bir web URL'den okur |<b>URL</b>: site URL'sini ve tüm uzantılı dosya adı dahil olmak üzere dosyanın tam adını belirtir. <br/><br/><b>Veri biçimi</b>: biri, desteklenen veri biçimlerini belirtir: CSV, TSV, ARFF'ye veya SVM açık. Verileri bir üst bilgi satırı varsa, sütun adları atamak için kullanılır. |
| Hadoop/HDFS |Hadoop dağıtılmış depolama alanından verileri okur. HiveQL, bir SQL benzeri sorgu dili kullanarak istediğiniz verileri belirtin. HiveQL veri toplama ve veri Machine Learning Studio'ya veri eklemeden önce filtreleme yapmak için de kullanılabilir. |<b>Hive veritabanı sorgusu</b>: verileri oluşturmak için kullanılan Hive sorgusu belirtir.<br/><br/><b>HCatalog sunucusu URI </b> : belirtilen biçimi kullanarak kümenizin adını  *&lt;küme adınızı&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop kullanıcı hesabı adı</b>: kümesi sağlamak için kullanılan Hadoop kullanıcı hesabının adını belirtir.<br/><br/><b>Hadoop kullanıcı hesabı parolası</b> : kümesi sağlanırken kullanılan kimlik bilgilerini belirtir. Daha fazla bilgi için [Hadoop kümeleri oluşturma HDInsight](../../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Çıktı verilerini konumunu</b>: verileri bir Hadoop dağıtılmış dosya sistemi (HDFS) veya Azure içinde mi depolanacağını belirtir. <br/><ul>HDFS çıktı verilerini depolamak, HDFS sunucusuna URI belirtin. (HTTPS:// ön eki olmadan HDInsight küme adı kullandığınızdan emin olun). <br/><br/>Çıktı verilerinizi Azure'da depolamak, Azure depolama hesabı adı, depolama erişim anahtarı ve depolama kapsayıcısı adı belirtmeniz gerekir.</ul> |
| SQL veritabanı |Bir Azure SQL veritabanı'nda veya bir Azure sanal makinesinde çalışan SQL Server veritabanında depolanan verileri okur. |<b>Veritabanı sunucusu adı</b>: veritabanını çalıştıran sunucunun adını belirtir.<br/><ul>Azure SQL veritabanı durumunda, oluşturulan sunucu adını girin. Genellikle bu biçimde  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Bir Azure sanal makinede barındırılan bir SQL server durumunda girin *tcp:&lt;sanal makine DNS adı&gt;, 1433*</ul><br/><b>Veritabanı adı </b>: sunucudaki veritabanı adını belirtir. <br/><br/><b>Server kullanıcı hesabı adı</b>: bir veritabanı için erişim izinleri olan bir hesabın kullanıcı adını belirtir. <br/><br/><b>Server kullanıcı hesabı parolası</b>: kullanıcı hesabının parolasını belirtir.<br/><br/><b>Veritabanı sorgusu</b>: okumak istediğiniz verileri tanımlayan bir SQL deyimi girin. |
| Şirket içinde SQL veritabanı |Bir şirket içi SQL veritabanı'nda depolanan verileri okur. |<b>Veri ağ geçidi</b>: veri yönetimi ağ geçidi burada da erişebilir, SQL Server veritabanınızın bir bilgisayarda yüklü adını belirtir. Ağ geçidini ayarlama hakkında daha fazla bilgi için bkz. [kullanarak verileri şirket içi SQL Server'dan Azure Machine Learning ile Gelişmiş analiz gerçekleştirme](use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Veritabanı sunucusu adı</b>: veritabanını çalıştıran sunucunun adını belirtir.<br/><br/><b>Veritabanı adı </b>: sunucudaki veritabanı adını belirtir. <br/><br/><b>Server kullanıcı hesabı adı</b>: bir veritabanı için erişim izinleri olan bir hesabın kullanıcı adını belirtir. <br/><br/><b>Kullanıcı adı ve parola</b>: tıklayın <b>değerleri girin</b> veritabanı kimlik bilgilerinizi girmeniz gerekmez. Windows tümleşik kimlik doğrulaması veya SQL Server şirket içi SQL Server'ınızı nasıl yapılandırıldığına bağlı olarak kimlik doğrulaması kullanabilirsiniz.<br/><br/><b>Veritabanı sorgusu</b>: okumak istediğiniz verileri tanımlayan bir SQL deyimi girin. |
| Azure Tablosu |Azure depolama tablo hizmeti veri okur.<br/><br/>Azure tablo hizmeti, büyük miktarlarda verinin seyrek okuma kullanın. Esnek, sağlayan ilişkisel olmayan (NoSQL), yüksek düzeyde ölçeklenebilir, Hesaplı ve yüksek oranda kullanılabilir bir depolama çözümü. |Seçenekler **verileri içeri aktarma** , genel bilgi ya da oturum açma kimlik bilgileri gerektiren özel bir depolama hesabı erişim bağlı olarak değiştirin. Bu belirlenir <b>kimlik doğrulama türü</b> değeri "PublicOrSAS" veya "Account", kendi parametrelerinin her biri sahip olabilir. <br/><br/><b>Genel veya paylaşılan erişim imzası (SAS) URI</b>: parametreler şunlardır:<br/><br/><ul><b>Tablo URI</b>: Tablo için genel veya SAS URL'sini belirtir.<br/><br/><b>Özellik adlarını taramak için satırı belirten</b>: değerler <i>üst n</i> belirtilen sayıda satırı, tarama veya <i>ScanAll</i> tablodaki tüm satırları almak için. <br/><br/>Veriler homojen ve tahmin edilebilir değilse, seçtiğiniz önerilir *üst n* ve N. için bir sayı girin Büyük tablolar için bu daha hızlı okuma kez sonuçlanabilir.<br/><br/>Veri kümeleri özelliklerinin derinliği göre değişir ve tablonun konumu ile yapılandırılırsa seçin *ScanAll* tüm satırları taramak için seçeneği. Bu meta veri dönüştürme ve sonuçta elde edilen özelliği bütünlüğü sağlar.<br/><br/></ul><b>Özel depolama hesabı</b>: parametreler şunlardır: <br/><br/><ul><b>Hesap adı</b>: okumak için tablo içeren bir hesabın adını belirtir.<br/><br/><b>Hesap anahtarı</b>: hesabıyla ilişkili depolama anahtarını belirtir.<br/><br/><b>Tablo adı</b> : okunacak verileri içeren tablonun adını belirtir.<br/><br/><b>Özellik adlarını taramak için satır</b>: değerler <i>üst n</i> belirtilen sayıda satırı, taramak için veya <i>ScanAll</i> tablodaki tüm satırları alınamıyor.<br/><br/>Veriler homojen ve tahmin edilebilir değilse, seçtiğiniz öneririz *üst n* ve N. için bir sayı girin Büyük tablolar için bu daha hızlı okuma kez sonuçlanabilir.<br/><br/>Veri kümeleri özelliklerinin derinliği göre değişir ve tablonun konumu ile yapılandırılırsa seçin *ScanAll* tüm satırları taramak için seçeneği. Bu meta veri dönüştürme ve sonuçta elde edilen özelliği bütünlüğü sağlar.<br/><br/> |
| Azure Blob Depolama |Resimler, yapılandırılmamış metin veya ikili veriler de dahil olmak üzere Azure depolama, Blob hizmetinde depolanan verileri okur.<br/><br/>Blob hizmeti, verileri genel olarak kullanıma sunmak veya uygulama verilerini özel olarak depolamak için kullanabilirsiniz. Her yerden verilerinize erişebilirsiniz HTTP veya HTTPS bağlantıları kullanarak. |Seçenekler **verileri içeri aktarma** genel bilgi ya da oturum açma kimlik bilgileri gerektiren özel bir depolama hesabı erişmeye çalıştığınız bağlı olarak değişiklik modülü. Bu belirlenir <b>kimlik doğrulama türü</b> "PublicOrSAS" veya "Hesap" bir değer olabilir.<br/><br/><b>Genel veya paylaşılan erişim imzası (SAS) URI</b>: parametreler şunlardır:<br/><br/><ul><b>URI</b>: depolama blobu genel veya SAS URL'sini belirtir.<br/><br/><b>Dosya biçimi</b>: Blob hizmetinde verilerin biçimini belirtir. Desteklenen biçimler şunlardır: CSV, TSV ve ARFF'ye.<br/><br/></ul><b>Özel depolama hesabı</b>: parametreler şunlardır: <br/><br/><ul><b>Hesap adı</b>: okumak istediğiniz blob içeren hesabının adını belirtir.<br/><br/><b>Hesap anahtarı</b>: hesabıyla ilişkili depolama anahtarını belirtir.<br/><br/><b>Kapsayıcı, dizin veya blob yolu </b> : okunacak verileri içeren blob adını belirtir.<br/><br/><b>BLOB dosya biçimi</b>: blob hizmetinde verilerin biçimini belirtir. Desteklenen veri biçimlerini belirtilen kodlama ve Excel, CSV, TSV, ARFF'ye, CSV olan. <br/><br/><ul>Biçim, CSV veya TSV ise, dosyanın bir üst bilgi satırı içerip içermediğini belirtmek emin olun.<br/><br/>Excel çalışma kitaplarından veri okumak için Excel seçeneğini kullanabilirsiniz. İçinde <i>Excel veri biçimi</i> seçeneğinde, gösteren veriler bir Excel çalışma sayfası aralıktaki ya da bir Excel tablosunda olup olmadığı. İçinde <i>Excel sayfası veya katıştırılmış tablo </i>seçeneğinde, sayfadaki veya okumak istediğiniz tablo adını belirtin.</ul><br/> |
| Veri akışı sağlayıcısı |Desteklenen bir akış Sağlayıcısı'ndan veri okur. Şu anda yalnızca açık veri Protokolü (OData) biçiminde desteklenir. |<b>Veri içerik türü</b>: OData biçimini belirtir.<br/><br/><b>Kaynak URL</b>: veri akışı tam URL'sini belirtir. <br/>Örneğin, aşağıdaki URL, Northwind örnek veritabanından okur: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Sonraki adımlar

[Veri içeri aktarma ve veri gönderme modüllerini kullanan Azure ML web hizmetleri dağıtma](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
