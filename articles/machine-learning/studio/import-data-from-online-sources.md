---
title: "Çevrimiçi veri kaynaklarından Machine Learning Studio uygulamasına veri içeri aktarma | Microsoft Docs"
description: "Eğitim verilerinizi Azure Machine Learning Studio çevrimiçi çeşitli kaynaklardan içeri aktarmak nasıl."
keywords: "veriler, veri biçimi, veri türleri, veri kaynakları, eğitim verilerini içeri aktarma"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: 4c699a8e5a9fafa0fec10bcb731f9ba533e3d283
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Veri Alma modülü ile çeşitli çevrimiçi veri kaynaklarından Azure Machine Learning Studio’ya veri alma
Bu makalede verileri çevrimiçi çeşitli kaynakları ve Azure Machine Learning deneme bu kaynaklardan veri taşımak için gereken bilgileri alma desteği açıklanmaktadır.

> [!NOTE]
> Bu makalede, hakkında genel bilgiler sağlar. [veri içeri aktarma] [ import-data] modülü. Erişebileceğiniz veri türleri hakkında daha ayrıntılı bilgi için biçimleri, parametreleri ve sık sorulan soruların yanıtlarını modülü başvurusu konuya bakın [veri içeri aktarma] [ import-data] modülü.
> 
> 

<!-- -->

[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Giriş
Kullanarak [veri içeri aktarma] [ import-data] modül erişebileceğiniz veri birkaç çevrimiçi veri kaynaklarından biri denemenizi çalışırken [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* HTTP kullanarak bir Web URL'si
* Hadoop HiveQL kullanma
* Azure blob depolama
* Azure tablosu
* Azure SQL veritabanına veya Azure VM'de SQL Server
* Şirket içi SQL Server veritabanı
* Sağlayıcı, şu anda OData veri akışı
* Azure CosmosDB (daha önce DocumentDB denir)

Studio denemenizi çevrimiçi veri kaynaklarına erişmek için eklemeniz [veri içeri aktarma] [ import-data] modülüne, seçin **veri kaynağı**ve verilere erişmek için gerekli parametreleri belirtin. Desteklenen çevrimiçi veri kaynakları aşağıdaki tabloda listelenmektedir. Bu tablo, desteklenen dosya biçimleri ve verilere erişmek için kullanılan parametreleri de özetler.

Bu eğitim verileri denemenizi çalışırken erişildiği için yalnızca bu deneme kullanılabilir olduğunu unutmayın. Karşılaştırma, bir veri kümesi modülünde depolanan verileri herhangi deneme çalışma alanınızdaki kullanılabilir.

> [!IMPORTANT]
> Şu anda [veri içeri aktarma] [ import-data] ve [verileri dışa aktar] [ export-data] modülleri okuma ve yalnızca klasik dağıtım modeli kullanılarak oluşturulan Azure Storage'dan veri yazma. Diğer bir deyişle, sık erişimli depolama erişim katmanı veya seyrek erişimli depolama erişim katmanı sunan yeni Azure Blob Depolama hesabı türü henüz desteklenmiyor. 
> 
> Genellikle tüm Azure depolama hesapları bu hizmeti seçeneği kullanılabilir olmadan önce oluşturmuş olabileceğiniz olduğunu etkilenmez. 
> Yeni bir hesap oluşturmanız gerekiyorsa, seçin **Klasik** dağıtım modeli veya Kaynak Yöneticisi'ni kullanın ve seçin **genel amaçlı** yerine **Blob storage** için **tür hesap**. 
> 
> Daha fazla bilgi için bkz: [Azure Blob Storage: sık erişimli ve seyrek erişimli depolama katmanları](../../storage/blobs/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Çevrimiçi veri kaynaklarında desteklenmemektedir
Azure Machine Learning **veri içeri aktarma** Modülü aşağıdaki veri kaynaklarını destekler:

| Veri Kaynağı | Açıklama | Parametreler |
| --- | --- | --- |
| HTTP üzerinden Web URL'si |Verileri virgülle ayrılmış değerler (CSV), sekmeyle ayrılmış değerler (TSV), öznitelik-ilişki dosyası biçimi (ARFF) ve Destek vektör makineler (SVM-light) biçimleri, HTTP kullanan tüm web URL'den okur |<b>URL</b>: site URL'sini ve tüm uzantılı dosya adı dahil olmak üzere dosyanın tam adını belirtir. <br/><br/><b>Veri biçimi</b>: desteklenen veri birini biçimleri belirtir: CSV, TSV, ARFF veya SVM açık. Verileri bir başlık satırı varsa, sütun adları atamak için kullanılır. |
| Hadoop/HDFS |Hadoop dağıtılmış depolama alanından verileri okur. HiveQL, bir SQL benzeri sorgu dili kullanarak istediğiniz verileri belirtin. HiveQL, veri toplama ve Machine Learning Studio verileri eklemeden önce filtreleme verilerini gerçekleştirmek için de kullanılabilir. |<b>Veritabanı sorgusu hive</b>: verileri oluşturmak için kullanılan Hive sorgusu belirtir.<br/><br/><b>HCatalog sunucusu URI </b> : belirtilen biçimi kullanarak, küme adı  *&lt;küme adınızı&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop kullanıcı hesabı adı</b>: kümesi sağlamak için kullanılan Hadoop kullanıcı hesabının adını belirtir.<br/><br/><b>Hadoop kullanıcı hesabı parolasını</b> : küme hazırlama sırasında kullanılan kimlik bilgilerini belirtir. Daha fazla bilgi için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri](../../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Çıktı verilerini konumunu</b>: verileri Hadoop dağıtılmış dosya sistemi (HDFS) veya Azure depolanan belirtir. <br/><ul>Çıktı verilerini HDFS'de depolamak, HDFS sunucusu URI belirtin. (HTTPS:// öneki olmadan Hdınsight küme adı kullandığınızdan emin olun). <br/><br/>Azure'da, çıktı verilerini depolarsanız, Azure depolama hesabı adı ve depolama erişim tuşu depolama kapsayıcısı adı belirtmeniz gerekir.</ul> |
| SQL veritabanı |Bir Azure SQL veritabanında veya bir Azure sanal makine üzerinde çalışan bir SQL Server veritabanında depolanan verileri okur. |<b>Veritabanı sunucusu adı</b>: veritabanını çalıştıran sunucunun adını belirtir.<br/><ul>Azure SQL veritabanı durumunda oluşturulan sunucu adı girin. Genellikle form sahip  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Azure sanal bir makinede barındırılan bir SQL server durumunda girin *tcp:&lt;sanal makinenin DNS adı&gt;, 1433*</ul><br/><b>Veritabanı adı </b>: sunucudaki veritabanı adını belirtir. <br/><br/><b>Sunucu kullanıcı hesabı adı</b>: veritabanı için erişim izinlerine sahip bir hesap için kullanıcı adını belirtir. <br/><br/><b>Sunucu kullanıcı hesabı parolasını</b>: kullanıcı hesabının parolasını belirtir.<br/><br/><b>Veritabanı sorgusu</b>: okumak istediğiniz verileri tanımlayan bir SQL deyimi girin. |
| Şirket içi SQL veritabanı |Bir şirket içi SQL veritabanında depolanan verileri okur. |<b>Veri ağ geçidi</b>: veri yönetimi ağ geçidi burada da erişebilir, SQL Server veritabanınızın bir bilgisayarda yüklü adını belirtir. Ağ geçidi ayarlama hakkında daha fazla bilgi için bkz: [bir şirket içi SQL server verilerini kullanarak Azure Machine Learning ile gelişmiş analizler gerçekleştirme](use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Veritabanı sunucusu adı</b>: veritabanını çalıştıran sunucunun adını belirtir.<br/><br/><b>Veritabanı adı </b>: sunucudaki veritabanı adını belirtir. <br/><br/><b>Sunucu kullanıcı hesabı adı</b>: veritabanı için erişim izinlerine sahip bir hesap için kullanıcı adını belirtir. <br/><br/><b>Kullanıcı adı ve parola</b>: tıklatın <b>değerleri girin</b> veritabanı kimlik bilgilerinizi girmeniz için. Windows tümleşik kimlik doğrulaması veya SQL Server şirket içi SQL Server'ınızdaki nasıl yapılandırıldığına bağlı olarak kimlik doğrulaması kullanabilirsiniz.<br/><br/><b>Veritabanı sorgusu</b>: okumak istediğiniz verileri tanımlayan bir SQL deyimi girin. |
| Azure Tablosu |Azure storage'da tablo hizmetinden verileri okur.<br/><br/>Büyük miktarlarda verinin seyrek okuyun, Azure tablo hizmeti kullanın. Esnek, sağlar ilişkisel olmayan (NoSQL), yüksek düzeyde ölçeklenebilir, ucuz ve yüksek oranda kullanılabilir bir depolama çözümü. |Seçenekler, **veri içeri aktarma** , ortak bilgi veya oturum açma kimlik bilgileri gerektiren bir özel depolama hesabına erişme bağlı olarak değiştirin. Bu belirlenir <b>kimlik doğrulama türü</b> "PublicOrSAS" veya "Hesap" değeri her biri kendi parametrelerinin sahip olabilir. <br/><br/><b>Ortak veya paylaşılan erişim imzası (SAS) URI</b>: Parametreler:<br/><br/><ul><b>Tablo URI</b>: Tablo için genel ya da SAS URL'yi belirtir.<br/><br/><b>Özellik adlarını taranacak satır belirtir</b>: değerler <i>TopN</i> belirtilen satır sayısı, taranacak veya <i>ScanAll</i> tablodaki tüm satırları almak için. <br/><br/>Veri homojen ve tahmin edilebilir ise, seçtiğiniz önerilir *TopN* ve N. için bir sayı girin Büyük tablolar için bu daha hızlı okuma kez neden olabilir.<br/><br/>Veri kümeleri derinliği bağlı olarak farklılık özelliklerinin ve tablonun konumunu yapılandırıldıysa seçin *ScanAll* tüm satırları taranacak seçeneği. Bu, sonuçta elde edilen özelliği ve meta veri dönüştürme bütünlüğünü sağlar.<br/><br/></ul><b>Özel depolama hesabı</b>: Parametreler: <br/><br/><ul><b>Hesap adı</b>: okumak için tabloyu içeren hesabının adını belirtir.<br/><br/><b>Hesap anahtarı</b>: hesapla ilişkili depolama anahtarını belirtir.<br/><br/><b>Tablo adı</b> : okunacak veriler içeren tablo adını belirtir.<br/><br/><b>Özellik adlarını taranacak satır</b>: değerler <i>TopN</i> belirtilen satır sayısı, taranacak veya <i>ScanAll</i> tablodaki tüm satırları almak için.<br/><br/>Veri homojen ve tahmin edilebilir ise, seçtiğiniz öneririz *TopN* ve N. için bir sayı girin Büyük tablolar için bu daha hızlı okuma kez neden olabilir.<br/><br/>Veri kümeleri derinliği bağlı olarak farklılık özelliklerinin ve tablonun konumunu yapılandırıldıysa seçin *ScanAll* tüm satırları taranacak seçeneği. Bu, sonuçta elde edilen özelliği ve meta veri dönüştürme bütünlüğünü sağlar.<br/><br/> |
| Azure Blob Depolama |Resimler, yapılandırılmamış metin veya ikili veriler dahil olmak üzere Azure Storage blobu hizmetinde depolanan verileri okur.<br/><br/>Blob hizmeti verileri genel olarak kullanıma sunmak veya özel olarak uygulama verilerini depolamak için kullanabilirsiniz. Verilerinizi yerden erişebilirsiniz HTTP veya HTTPS bağlantıları kullanarak. |Seçenekler, **veri içeri aktarma** ortak bilgi ya da oturum açma kimlik bilgileri gerektiren özel depolama hesabı erişmeye çalıştığınız bağlı olarak modülü Değiştir. Bu belirlenir <b>kimlik doğrulama türü</b> "PublicOrSAS" veya "Hesap" bir değer olabilir.<br/><br/><b>Ortak veya paylaşılan erişim imzası (SAS) URI</b>: Parametreler:<br/><br/><ul><b>URI</b>: depolama blobu genel ya da SAS URL'yi belirtir.<br/><br/><b>Dosya biçimi</b>: Blob hizmetinde veri biçimini belirtir. Desteklenen biçimler şunlardır: CSV, TSV ve ARFF.<br/><br/></ul><b>Özel depolama hesabı</b>: Parametreler: <br/><br/><ul><b>Hesap adı</b>: okumak istediğiniz blob'un bulunduğu hesabının adını belirtir.<br/><br/><b>Hesap anahtarı</b>: hesapla ilişkili depolama anahtarını belirtir.<br/><br/><b>Kapsayıcı, dizin veya blob yolu </b> : okumak için gerekli verileri içeren blob adını belirtir.<br/><br/><b>BLOB dosya biçimi</b>: blob hizmetinde veri biçimini belirtir. Desteklenen veri biçimleri, belirtilen kodlama ve Excel CSV, TSV, ARFF, CSV olacaktır. <br/><br/><ul>Biçim, CSV veya TSV ise, dosya üstbilgisi satır içerip içermediğini belirtmek emin olun.<br/><br/>Excel çalışma kitaplarından veri okumak için Excel seçeneği kullanabilirsiniz. İçinde <i>Excel veri biçimi</i> seçeneği, belirtmek verileri bir Excel çalışma sayfası aralığı veya bir Excel tablosu olup. İçinde <i>Excel sayfası veya katıştırılmış tablo </i>seçeneği, sayfa veya okuma istediğiniz tablo adını belirtin.</ul><br/> |
| Veri akış sağlayıcısı |Desteklenen bir akış Sağlayıcısı'ndan verileri okur. Şu anda yalnızca açık veri Protokolü (OData) biçimi destekleniyor. |<b>Veri içerik türü</b>: OData biçimi belirtir.<br/><br/><b>Kaynak URL</b>: veri akışı tam URL'sini belirtir. <br/>Örneğin, aşağıdaki URL'yi Northwind örnek veritabanından okur: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Sonraki adımlar

[Veri alma ve verileri dışarı aktarma modülleri kullanan Azure ML web hizmetleri dağıtma](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
