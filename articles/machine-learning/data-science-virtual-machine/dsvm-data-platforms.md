---
title: Veri platformları için veri bilimi sanal makinesi - Azure | Microsoft Docs
description: Veri platformları ve veri bilimi sanal makinesi üzerinde desteklenen araçları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 27e0deae9c35ad8fa00659e3e3e505cace6e9014
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516492"
---
# <a name="data-platforms-supported-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi üzerinde desteklenen veri platformları

Veri bilimi sanal makinesi (DSVM), çok çeşitli veri platformları karşı analizlerinizi oluşturmanızı sağlar. Uzak Veri platformları arabirimlerine ek olarak, DSVM prototip oluşturma ve hızlı geliştirme için yerel bir örnek sağlar. 

DSVM'nin desteklenen data platform araçları şunlardır. 

## <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Geliştirici sürümü

| | |
| ------------- | ------------- |
| Nedir?   | Yerel bir ilişkisel veritabanı      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Yerel olarak daha küçük veri kümesi ile hızlı geliştirme <br/> Veritabanında R çalıştırın   |
| Örneklere bağlantılar      |    New York City veri kümesinin küçük bir örnek SQL veritabanı'na yüklenen `nyctaxi`. <br/> Microsoft R ve veritabanı içi analiz gösteren Jupyter örnek yolda bulunabilir:<br/> `~notebooks/SQL_R_Services_End_to_End_Tutorial.ipynb`  |
| DSVM ilgili araçları       | SQL Server Management Studio <br/> ODBC/JDBC sürücüleri<br/> pyodbc, RODBC<br />Apache ayrıntıya      |

> [!NOTE]
> SQL Server 2016 developer sürümü, yalnızca geliştirme ve test amaçları için kullanılabilir. Bir lisans ya da bir üretim ortamında çalıştırmak için SQL Server Vm'leri gerekir. 


### <a name="setup"></a>Kurulum

Veritabanı sunucusu önceden yapılandırılmış ve ilgili SQL Server için Windows Hizmetleri (gibi `SQL Server (MSSQLSERVER)`) otomatik olarak çalışacak şekilde ayarlayın. Çalıştırılacak yalnızca el ile adım Microsoft R. kullanarak veritabanı içi analiz etkinleştirmektir. Bir kez Aç, makineyi yönetici olarak oturum açmayı sonra işlem SQL Server Management Studio (SSMS) gibi "Yeni sorguyu", aşağıdaki komutu çalıştırarak SSMS'de bunu, seçili veritabanı olduğundan emin olun `master` ve çalıştırın: 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)
       
SQL Server Management Studio'yu çalıştırmak için "SQL Server Management Studio için" program listede arama veya Windows Search bulmak ve çalıştırmak için kullanın. Kimlik bilgileri istendiğinde, "Windows kimlik doğrulaması" öğesini seçin ve makine adını kullanın veya ```localhost``` SQL Server adı. 

### <a name="how-to-use--run-it"></a>Kullanma / çalıştırın nasıl?  

Varsayılan veritabanı örneği ile veritabanı sunucusu, varsayılan olarak otomatik olarak çalışıyor. Yerel SQL Server veritabanına erişmek için VM üzerinde SQL Server Management Studio gibi araçlar kullanın. Yerel yönetici hesabına veritabanında ADMIN erişime sahiptir. 

ODBC sürücüleri ve JDBC sürücüleri, Python, r dahil olmak üzere birden çok dilde yazılmış uygulamalardan için SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı'kurmak da DSVM birlikte 

### <a name="how-is-it-configured--installed-on-the-dsvm"></a>Nasıl, yapılandırılmış / DSVM üzerinde yüklü? 

SQL Server standart şekilde yüklenir. Şurada bulunabilir `C:\Program Files\Microsoft SQL Server`. In veritabanı R örneği bulunduğu `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES`. DSVM de ayrı bir tek başına, yüklü bir R Server örneğini sahip `C:\Program Files\Microsoft\R Server\R_SERVER`. Bu iki R örnekler kitaplıkları paylaşmayın.


## <a name="apache-spark-2x-standalone"></a>Apache Spark 2.x (tek başına)

| | |
| ------------- | ------------- |
| Nedir?   | Popüler Apache Spark platform, büyük ölçekli hızlı veri işleme ve makine öğrenimi için bir sistem bir tek başına (tek düğüm işlem içi) örneği     |
| Desteklenen DSVM sürümleri      | Linux <br /> Windows (Deneysel)      |
| Tipik kullanımları      | * Hızlı uygulamaları geliştirmeye yönelik Spark/PySpark daha küçük veri kümesi ile yerel olarak ve daha sonra Azure HDInsight gibi büyük Spark kümelerinde bunu dağıtma<br/> * Test Microsoft R Server Spark bağlamı <br />* SparkML ya da Microsoft'un açık kaynak kullanan [MMLSpark](https://github.com/Azure/mmlspark) kitaplığını kullanarak ML uygulamalar oluşturun  |
| Örneklere bağlantılar      |    Jupyter örneği: <br />&nbsp;&nbsp;* ~/notebooks/SparkML/pySpark <br /> &nbsp;&nbsp;* ~/notebooks/MMLSpark <br /> Microsoft R Server (Spark bağlamını): /dsvm/samples/MRS/MRSSparkContextSample.R |
| DSVM ilgili araçları       | PySpark, Scala<br/>Jupyter (Spark/PySpark çekirdekleri)<br/>Microsoft R Server, SparkR, Sparklyr <br />Apache ayrıntıya      |

### <a name="how-to-use-it"></a>Nasıl kullanılacağını
Spark ile komut satırında Spark işleri göndererek çalıştırabileceğiniz `spark-submit` veya `pyspark` komutları. Yeni bir not defteri ile Spark çekirdek oluşturarak, Jupyter Not Defteri de oluşturabilirsiniz. 

DSVM'nin kullanılabilir kitaplıklar SparkR, Sparklyr veya Microsoft R Server gibi kullanarak R Spark'tan kullanabilirsiniz. Önceki tabloda örnekleri işaretçileri bakın. 

> [!NOTE]
> Microsoft R Server DSVM Spark bağlamında çalıştırılması, yalnızca Ubuntu Linux DSVM'sini Edition'da desteklenir. 



### <a name="setup"></a>Kurulum
Microsoft R Server Ubuntu Linux DSVM'sini edition üzerinde Spark bağlamında çalıştırmadan önce kurulum adımı yerel bir tek düğümlü Hadoop HDFS ve Yarn örneği etkinleştirmek için bir kere yapmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklendi ancak DSVM'nin devre dışı. Bunu etkinleştirmek için aşağıdaki komutları kök olarak ilk kez çalıştırma gerekir:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Hadoop durdurabilirsiniz çalıştırarak değil gerektiğinde Hizmetleri ilgili ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn``` geliştirip (Bu tek başına Spark örneğinde DSVM) uzaktan Spark bağlamında MRS test nasıl yazılacağını gösteren bir örnek sağlanan ve kullanılabilir `/dsvm/samples/MRS` Dizin. 


### <a name="how-is-it-configured--installed-on-the-dsvm"></a>Nasıl, yapılandırılmış / DSVM üzerinde yüklü? 
|Platform|Yükleme konumu ($SPARK_HOME)|
|:--------|:--------|
|Windows | c:\dsvm\tools\spark-X.X.X-bin-hadoopX.X|
|Linux   | /dsvm/Tools/Spark-X.X.X-bin-hadoopX.X|


Verileri Azure Blob veya Azure Data Lake storage (ADLS) ve Microsoft'un makine öğrenimi MMLSpark kitaplığı kullanarak erişmek için kitaplıkları $SPARK_HOME/jar dosyaları dışındaki içinde önceden yüklenen. Bu jar dosyaları dışındaki, Spark başlatıldığında otomatik olarak yüklenir. Varsayılan olarak, yerel diskte Spark verileri kullanır. Azure blob veya ADLS depolanan verilere erişmek için DSVM üzerinde bir Spark örneği için sırayla oluşturmak ve yapılandırmak gereken `core-site.xml` $SPARK_HOME/conf/core-site.xml.template içinde bulunan şablon temel dosya (olduğu Blob ve ADLS yer tutucuları yapılandırmaları) Azure blob ve Azure Data Lake depolama için uygun kimlik bilgileri. ADLS hizmeti kimlik bilgileri oluşturma adımları ayrıntılı bulduğunuz [burada](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory). Core-site.xml dosyasında Azure blob veya ADLS kimlik bilgileri girildikten sonra bu kaynakları wasb URI öneki ile depolanan veriler başvurabilir: / / ya da adl: / /. 

