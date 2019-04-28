---
title: HDFS CLI ile Azure Data Lake depolama Gen2 kullanma
description: HDFS CLI için Data Lake depolama Gen2'ye Giriş
services: storage
author: artemuwka
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: artek
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: d1c9eff08a7b9cc50ccdca4ce798ac4d0f3d35f2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481794"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Data Lake depolama 2. nesil ile HDFS CLI'yı kullanma

Erişim ve sahip olduğu gibi bir komut satırı arabirimi kullanarak depolama hesabınızdaki verileri yönetme bir [Hadoop dağıtılmış dosya sistemi (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Bu makale yardımcı olacak bazı örnekler başlama sağlar.

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. HDFS ve Hadoop destekleyen diğer dosya sistemleri ile doğrudan etkileşim Kabuğu'nu kullanarak bu dosya sistemine erişebilir.

HDFS CLI hakkında daha fazla bilgi için bkz. [resmi belgelerine](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) ve [HDFS izinleri Kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

>[!NOTE]
>HDInsight yerine Azure Databricks kullanarak ve bir komut satırı arabirimi kullanarak verilerinizle etkileşim kurmak istediğiniz, Databricks dosya sistemi ile etkileşim kurmak için Databricks CLI'yı kullanabilirsiniz. Bkz: [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).

## <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>Linux'ta bir HDInsight Hadoop kümesi ile HDFS CLI'yı kullanma

İlk olarak, kurmak [Uzaktan Erişim Hizmetleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Seçerseniz [SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) örnek PowerShell kodu şu şekilde görünür:

```powershell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
Bağlantı dizesi şu yolda bulunabilir: "SSH + küme oturum açma" bölümünde Azure portalında HDInsight küme dikey penceresi. Küme oluşturma sırasında SSH kimlik bilgileri belirtildi.

>[!IMPORTANT]
>HDInsight kümesi faturalandırması küme oluşturulur ve küme silindiğinde sona erer sonra başlar. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Küme silme hakkında bilgi edinmek için müşterilerimize [makale konusunda](../../hdinsight/hdinsight-delete-cluster.md). Ancak, Data Lake depolama etkin Gen2 ile bir depolama hesabına depolanan verilerinizin bile bir HDInsight kümesi silindikten sonra devam ettirir.

## <a name="create-a-file-system"></a>Bir dosya sistemi oluşturun

    hdfs dfs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/

* Değiştirin `<file-system-name>` dosya sisteminize vermek istediğiniz adla yer tutucu.

* Değiştirin `<storage-account-name>` depolama hesabınızın adıyla yer tutucu.

## <a name="get-a-list-of-files-or-directories"></a>Dosya veya dizinlerin bir listesini alın

    hdfs dfs -ls <path>

Değiştirin `<path>` yer tutucusunu dosya sisteminde veya dosya sistem klasöründe URI'si ile.

Örneğin, `hdfs dfs -ls abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name`

## <a name="create-a-directory"></a>Dizin oluşturma

    hdfs dfs -mkdir [-p] <path>

Değiştirin `<path>` kök dosya sistemi adına veya bir klasördeki bir dosya sisteminize ile yer tutucu.

Örneğin, `hdfs dfs -mkdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/`

## <a name="delete-a-file-or-directory"></a>Bir dosya veya dizin Sil

    hdfs dfs -rm <path>

Değiştirin `<path>` dosya ya da silmek istediğiniz klasör URI'si ile yer tutucu.

Örneğin, `hdfs dfs -rmdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name/my-file-name`

## <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>Erişim denetim listeleri (ACL'ler) dosyaları ve dizinleri görüntüle

    hdfs dfs -getfacl [-R] <path>

Örnek:

`hdfs dfs -getfacl -R /dir`

Bkz: [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl)

## <a name="set-acls-of-files-and-directories"></a>Dosya ve dizinleri ACL'leri ayarlayın

    hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]

Örnek:

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

Bkz: [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl)

## <a name="change-the-owner-of-files"></a>Dosya sahibini Değiştir

    hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>

Bkz: [chown](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown)

## <a name="change-group-association-of-files"></a>Dosya grubu ilişkisini değiştirme

    hdfs dfs -chgrp [-R] <group> <URI>

Bkz: [chgrp](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp)

## <a name="change-the-permissions-of-files"></a>Dosyaları izinlerini değiştirme

    hdfs dfs -chmod [-R] <mode> <URI>

Bkz: [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod)

Komutların tam listesi görüntüleyebileceğiniz [2.4.1 Apache Hadoop dosya sistemi Kabuk Kılavuzu](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) Web sitesi.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Databricks'te bir Azure Data Lake depolama Gen2'ye yeteneğine sahip bir hesap kullanın](./data-lake-storage-quickstart-create-databricks-account.md)

* [Erişim denetim listelerini dosyalar ve dizinler hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)
