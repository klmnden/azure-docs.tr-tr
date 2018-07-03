---
title: HDFS CLI kullanarak Azure Data Lake depolama Gen2 Önizleme ile
description: Data Lake depolama Gen2 önizlemesi için HDFS CLI giriş
services: storage
documentationcenter: ''
author: artemuwka
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: artek
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 591d8ea7670bf9b29450695ee7cbee5fa39baaac
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344731"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Data Lake depolama 2. nesil ile HDFS CLI'yı kullanma

Azure Data Lake depolama Gen2 önizlemesi yönetmenizi ve sahip olduğu gibi veri erişim sağlayan bir [Hadoop dağıtılmış dosya sistemi (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Bir HDInsight kümesine eklenmiş veya Azure Data Lake depolama Gen2'içinde depolanan veriler üzerinde analiz gerçekleştirmek için Azure Databricks kullanarak bir Apache Spark işi çalıştırma olup olmadığını almak ve yüklenen verileri işlemek için komut satırı arabirimi (CLI) kullanabilirsiniz. Bu makalenin geri kalanında sahip while seçenekler özetlenmektedir [Azure depolama ekibi, Azure Depolama Gezgini ve Azure portalı desteği ekleme çalıştığından](https://azure.microsoft.com/roadmap/).

## <a name="hdfs-cli-with-hdinsight"></a>HDInsight ile HDFS CLI

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemi, HDFS ve Hadoop destekleyen diğer dosya sistemleri ile doğrudan etkileşim Kabuğu'nu kullanarak erişilebilir. Sık kullanılan komutlar ve faydalı kaynaklara bağlantılar aşağıda verilmiştir.

>[!IMPORTANT]
>HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Faturalandırma, dakika başına eşit olarak artık kullanımda olmadığında kümenizi her zaman silmelisiniz (bilgi nasıl [küme silme](../../hdinsight/hdinsight-delete-cluster.md)). Ancak, hatta bir HDInsight kümesi silindikten sonra Azure Data Lake depolama Gen2'içinde depolanan verileri devam ettirir.

Dosya veya dizinlerin bir listesini almak için:

    hdfs dfs -ls <args>
Bir dizin oluşturmak için:

    hdfs dfs -mkdir [-p] <paths>
Bir dosya veya dizin silmek için:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Artık Linux'ta HDInsight Hadoop kümesi örnek olarak alalım. HDFS CLI kullanmak için öncelikle oluşturmak için ihtiyacınız [Uzaktan Erişim Hizmetleri](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Seçerseniz [SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) örnek PowerShell kodu şu şekilde görünür:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

Bağlantı dizesi şu yolda bulunabilir: "SSH + küme oturum açma" bölümünde Azure portalında HDInsight küme dikey penceresi. Küme oluşturma sırasında SSH kimlik bilgileri belirtildi.

HDFS CLI hakkında daha fazla bilgi için bkz. [resmi belgelerine](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) ve [HDFS izinleri Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>HDFS CLI ile Azure Databricks

Databricks Databricks REST API üzerinde derlenmiş bir kolayca kullanıma CLI sağlar. Açık kaynaklı proje üzerinde barındırılan [GitHub](https://github.com/databricks/databricks-cli). Sık kullanılan komutlar aşağıda verilmiştir.

Dosya veya dizinlerin bir listesini almak için:

    dbfs ls [-l]
Bir dizin oluşturmak için:

    dbfs mkdirs
Bir dosyayı silmek için:

    dbfs rm [-r]

Not defterlerini Databricks ile etkileşim kurmanın başka bir yolu var. Bir not defteri birincil dili olsa da dili Sihirli komutu % dil başına bir hücrenin belirterek dilleri karıştırabilirsiniz. Özellikle, % sh defterinizdeki Kabuk kodu yürütmek için çok benzer HDInsight örnekte bu makalenin önceki bölümlerinde sağlar.

Dosya veya dizinlerin bir listesini almak için:

    %sh ls <args>
Bir dizin oluşturmak için:

    %sh mkdir [-p] <paths>
Bir dosya veya dizin silmek için:

    %sh rm [-skipTrash] URI [URI ...]

Azure Databricks'te Spark kümesi başlattıktan sonra yeni bir not defteri oluşturacaksınız. Not Defteri betiğe aşağıdaki gibi görünür:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Databricks CLI hakkında daha fazla bilgi için bkz. [resmi belgelerine](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Not Defterleri ile ilgili daha fazla bilgi için bkz [not defterlerini](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) belgelerin bölümü.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake depolama 2. nesil ile bir HDInsight kümesi oluşturma](./quickstart-create-connect-hdi-cluster.md)
- [Azure Databricks'te bir Azure Data Lake depolama Gen2'ye yeteneğine sahip bir hesap kullanın](./quickstart-create-databricks-account.md) 