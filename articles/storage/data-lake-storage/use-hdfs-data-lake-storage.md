---
title: HDFS CLI Azure Data Lake Storage Gen2 Önizleme ile kullanma
description: Data Lake Storage Gen2 Önizleme için HDFS CLI giriş
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
ms.openlocfilehash: 75fb07120c78c45d422ee5017eac0afcf0e80859
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060827"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Data Lake Storage Gen2 ile HDFS CLI kullanma

Azure Data Lake Storage Gen2 Önizleme yönetmenizi ve ile yaptığınız gibi veri erişim sağlayan bir [Hadoop dağıtılmış dosya sistemi (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Eklenen veya Azure Databricks kullanarak Azure Data Lake Storage Gen2 içinde depolanan veriler üzerinde analiz gerçekleştirmek için bir Apache Spark işi çalıştırmak bir Hdınsight kümesine sahip olup olmadığını almak ve yüklenen verileri değiştirmek için komut satırı arabirimi (CLI) kullanabilirsiniz. Makalenin kalanında sahip while seçenekleri özetlenmektedir [Azure depolama ekibi, Azure Storage Gezgini ve Azure portal desteği ekleme çalıştığını](https://azure.microsoft.com/roadmap/) -keyfini çıkarın!

## <a name="hdfs-cli-with-hdinsight"></a>Hdınsight ile HDFS CLI

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemi, HDFS ve Hadoop destekleyen diğer dosya sistemleri ile doğrudan etkileşim Kabuğu'nu kullanarak erişilebilir. Sık kullanılan komutlar ve yararlı kaynaklara ait bağlantılar aşağıda verilmiştir.

>[!IMPORTANT]
>HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Faturalaması dakika başına Faturalaması artık kullanımda olmadığında, her zaman kümenizi silmeniz gerekir böylece (öğrenin nasıl [bir kümeyi silmek](../../hdinsight/hdinsight-delete-cluster.md)). Ancak, hatta Hdınsight kümesi silindikten sonra Azure Data Lake Storage Gen2 içinde depolanan verileri devam ettirir.

Dosya veya dizinlerin bir listesini almak için:

    hdfs dfs -ls <args>
Bir dizin oluşturmak için:

    hdfs dfs -mkdir [-p] <paths>
Bir dosya veya dizin silmek için:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Şimdi Linux'ta Hdınsight Hadoop kümesi örnek olarak atalım. HDFS CLI kullanmak için önce oluşturmanız gerekir [Uzaktan Erişim Hizmetleri için](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Seçerseniz [SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) örnek PowerShell kod şu şekilde görünür:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

Bağlantı dizesi şu adreste bulunabilir: "SSH + küme oturum açma" Azure portalında Hdınsight küme dikey bölüm. SSH kimlik bilgisi, küme oluşturma sırasında belirtilmedi.

HDFS CLI hakkında daha fazla bilgi için bkz: [resmi belge](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) ve [HDFS izinleri Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>HDFS CLI Azure Databricks ile

Databricks Databricks REST API üstünde oluşturulmuş bir kullanımı kolay CLI sağlar. Açık kaynaklı proje üzerinde barındırılan [GitHub](https://github.com/databricks/databricks-cli). Sık kullanılan komutlar aşağıda verilmiştir.

Dosya veya dizinlerin bir listesini almak için:

    dbfs ls [-l]
Bir dizin oluşturmak için:

    dbfs mkdirs
Bir dosyayı silmek için:

    dbfs rm [-r]

Databricks ile etkileşim başka bir yoldur dizüstü bilgisayarlar. Bir not defteri birincil dili sahipken, bir hücre başındaki dil Sihirli komut % dili belirterek dilleri karıştırabilirsiniz. Özellikle, % Paylaş defterinizde Kabuk kod yürütmek için çok benzer Hdınsight örnekte bu makalenin önceki bölümlerinde sağlar.

Dosya veya dizinlerin bir listesini almak için:

    %sh ls <args>
Bir dizin oluşturmak için:

    %sh mkdir [-p] <paths>
Bir dosya veya dizin silmek için:

    %sh rm [-skipTrash] URI [URI ...]

Spark kümesi Azure Databricks başlattıktan sonra yeni bir not defteri oluşturacaksınız. Örnek Not Defteri betiği aşağıdaki gibi görünür:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Databricks CLI hakkında daha fazla bilgi için bkz: [resmi belge](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Dizüstü bilgisayarlar hakkında daha fazla bilgi için bkz: [not defterlerini](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) belgelere bölümü.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake Storage Gen2 ile Hdınsight kümesi oluşturma](./quickstart-create-connect-hdi-cluster.md)
- [Bir Azure Data Lake Storage Gen2 özellikli hesap Azure Databricks kullanın](./quickstart-create-databricks-account.md) 