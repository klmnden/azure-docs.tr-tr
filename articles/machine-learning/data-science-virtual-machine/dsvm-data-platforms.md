---
title: "Veri platformlar için veri bilimi sanal makine - Azure | Microsoft Docs"
description: "Veri platformlar için veri bilimi sanal makine."
keywords: "Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: gokuma;
ms.openlocfilehash: 921ccf67e5e0320e742066186b7929643536424f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="data-platforms"></a>Veri platformları

Veri bilimi sanal makine (DSVM), çok çeşitli veri platformları karşı analizlerinizi oluşturmanıza olanak verir. Uzak Veri platformları arabirimlerine ek olarak, DSVM prototipi oluşturulurken ve hızlı geliştirme için yerel bir örnek sağlar. 

DSVM üzerinde desteklenen veri platformu araçları şunlardır: 

## <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer Edition

| | |
| ------------- | ------------- |
| Nedir?   | Yerel bir ilişkisel veritabanı örneği      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanır      | Yerel olarak daha küçük veri kümesi ile hızlı geliştirme <br/> Veritabanı R çalıştırın   |
| Örnekleri bağlantılar      |    New York şehrinde kümesinin küçük bir örnek SQL veritabanına yüklenir `nyctaxi`. <br/> Microsoft R ve veritabanı Analytics'i gösteren Jupyter örnek yolda bulunabilir:<br/> `~notebooks/SQL_R_Services_End_to_End_Tutorial.ipynb`  |
| DSVM ilgili araçları       | SQL Server Management Studio <br/> ODBC/JDBC sürücüleri<br/> pyodbc, RODBC<br />Apache Drill      |

> [!NOTE]
> SQL Server 2016 Geliştirici sürümü yalnızca geliştirme ve test amaçları için kullanılabilir. Bir lisans veya ürün çalıştırmak için SQL Server Vm'lerinin biri gerekir. 


### <a name="setup"></a>Kurulum

Veritabanı sunucusu zaten önceden yapılandırılmış ve Windows Hizmetleri için SQL Server ilgili (gibi `SQL Server (MSSQLSERVER)`) otomatik olarak çalışacak şekilde ayarlanmış. Yalnızca el ile adımın çalıştırılması için veritabanı Analytics'i Microsoft R. kullanarak sağlamaktır. Bu eylem SQL Server Management Studio (SSMS) makine yönetici olarak oturum açmayı sonra bir kez açık olarak "Yeni bir sorgu" aşağıdaki komutu çalıştırarak SSMS yapabilirsiniz, seçili veritabanı olduğundan emin olun `master` ve ardından çalıştırın: 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)
       
SQL Server Management Studio çalıştırmak için "SQL Server Management Studio için" program listede arama veya Windows Search bulmak ve çalıştırmak için kullanın. Kimlik bilgileri istendiğinde, "Windows kimlik doğrulaması" seçin ve makine adı kullanın veya ```localhost``` SQL Server adı. 

### <a name="how-to-use--run-it"></a>Kullanın / çalıştırmak için nasıl?  

Varsayılan veritabanı kopyası veritabanı sunucusuyla otomatik olarak varsayılan olarak çalışıyor. Yerel olarak SQL Server veritabanına erişmek için VM'de SQL Server Management Studio gibi araçlar kullanın. Yerel yönetici hesabı veritabanı üzerinde yönetimsel erişime sahiptir. 

Ayrıca SQL Server, Azure SQL veritabanları ve Azure SQL Data Warehouse, Python, r dahil olmak üzere çeşitli dillerde yazılmış uygulamalardan anlaşmak için ODBC sürücüleri ve JDBC sürücüsü olan DSVM gelir 

### <a name="how-is-it-configured--installed-on-the-dsvm"></a>Nasıl, yapılandırılmış veya DSVM üzerinde yüklü? 

SQL Server standart şekilde yüklenir. Konumunda bulunabilir `C:\Program Files\Microsoft SQL Server`. In-veritabanı R örneği bulunduğu konum `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES`. DSVM ayrıca ayrı tek başına, yüklü bir R Server örneğini sahip `C:\Program Files\Microsoft\R Server\R_SERVER`. Bu iki-R örnekler kitaplıkları paylaşmayın.


## <a name="apache-spark-2x-standalone"></a>Apache Spark 2.x (tek başına)

| | |
| ------------- | ------------- |
| Nedir?   | Popüler Apache Spark platform, bir sistem hızlı büyük ölçekli veri işleme ve machine learning için tek başına (tek düğümlü işlemdeki) örneği     |
| Desteklenen DSVM sürümleri      | Linux <br /> Windows (Deneysel)      |
| Tipik kullanır      | * Hızlı geliştirme Spark/PySpark uygulamaların yerel olarak daha küçük veri kümesi ve daha sonra Azure Hdınsight gibi büyük Spark kümeleri üzerinde bunu dağıtma<br/> * Test Microsoft R Server Spark bağlamı <br />* SparkML ya da Microsoft'un açık kaynaklı kullanmak [MMLSpark](https://github.com/Azure/mmlspark) ML uygulamaları geliştirmek için kitaplığı  |
| Örnekleri bağlantılar      |    Jupyter örneği: <br />&nbsp;&nbsp;* ~/notebooks/SparkML/pySpark <br /> &nbsp;&nbsp;* ~/notebooks/MMLSpark <br /> Microsoft R Server (Spark bağlamı): /dsvm/samples/MRS/MRSSparkContextSample.R |
| DSVM ilgili araçları       | PySpark, Scala<br/>Jupyter (Spark/PySpark tekrar)<br/>Microsoft R Server, SparkR, Sparklyr <br />Apache Drill      |

### <a name="how-to-use-it"></a>Nasıl kullanılacağını
Komut satırı ile Spark işlerine göndererek Spark çalıştırabilirsiniz `spark-submit` veya `pyspark` komutları. Yeni bir not defteri ile Spark çekirdek oluşturarak Jupyter Not Defteri de oluşturabilirsiniz. 

R DSVM üzerinde kullanılabilir kitaplıkları SparkR, Sparklyr veya Microsoft R Server gibi kullanarak Spark'tan kullanabilirsiniz. Önceki tabloda örnekleri işaretçiler bakın. 

> [!NOTE]
> Spark DSVM bağlamında Microsoft R Server çalıştıran yalnızca Ubuntu Linux DSVM Edition'da desteklenir. 



### <a name="setup"></a>Kurulum
Microsoft R Server Spark bağlamda Ubuntu Linux DSVM Edition'da çalıştırmadan önce kurulum adım yerel tek bir düğüm Hadoop HDFS ve Yarn örneğini etkinleştirmek için bir kez yapmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklü ancak DSVM üzerinde devre dışı. Bunu etkinleştirmek için aşağıdaki komutları kök olarak ilk kez çalıştırmanız gereken:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Hadoop durdurabilirsiniz, bunları çalıştırarak gerekmediğinde Hizmetleri ilgili ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` sağlanan ve kullanılabilir geliştirmek ve (DSVM tek başına Spark örneğinde olan) uzaktan Spark bağlamında MRS sınamak nasıl gösteren bir örnek `/dsvm/samples/MRS` Dizin. 


### <a name="how-is-it-configured--installed-on-the-dsvm"></a>Nasıl, yapılandırılmış veya DSVM üzerinde yüklü? 
|Platform|Yükleme konumu ($SPARK_HOME)|
|:--------|:--------|
|Windows | c:\dsvm\tools\spark-X.X.X-bin-hadoopX.X|
|Linux   | /dsvm/tools/spark-X.X.X-bin-hadoopX.X|


Verileri Azure Blob veya Azure Data Lake storage (ADLS) ve Microsoft'un MMLSpark machine learning kitaplıkları kullanarak erişmesini kitaplıkları $SPARK_HOME/Kavanoz önceden yüklenmiş. Spark başlatıldığında bu Kavanoz otomatik olarak yüklenir. Varsayılan olarak, yerel diskte veri Spark kullanır. DSVM Azure blob veya ADLS depolanan verilere erişmek için Spark örneğinde sırayla oluşturmak ve yapılandırmak gereken `core-site.xml` $SPARK_HOME/conf/core-site.xml.template içinde bulunan şablonunu temel dosyası (bulunduğu Blob ve ADLS yer tutucular yapılandırmaları) Azure blob ve Azure Data Lake Storage için uygun kimlik bilgilerine sahip. Daha ayrıntılı ADLS hizmet kimlik bilgilerini oluşturma adımları Bul [burada](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory#create-an-active-directory-application). Azure blob veya ADLS için kimlik bilgilerini core-site.xml dosyasında girildikten sonra bu kaynakları wasb URI öneki ile depolanan verileri başvurabilir: / / veya adl: / /. 

