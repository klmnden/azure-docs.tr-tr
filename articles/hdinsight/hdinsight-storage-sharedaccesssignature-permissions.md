---
title: "Paylaşılan erişim imzaları - Azure Hdınsight kullanarak erişimi kısıtlama | Microsoft Docs"
description: "Paylaşılan erişim imzaları Azure depolama blob'larda depolanan verilere Hdınsight erişimi kısıtlamak için nasıl kullanılacağını öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2017
ms.author: larryfr
ms.openlocfilehash: 92ad526d034591b8f463ef6b01e115101b74e1ae
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>Hdınsight'ta verilere erişimi kısıtlamak için Azure Storage paylaşılan erişim imzaları kullanın

Hdınsight kümesi ile ilişkili Azure depolama hesaplarındaki veri tam erişimi vardır. Blob kapsayıcısında paylaşılan erişim imzaları verilere erişimi kısıtlamak için kullanabilirsiniz. Örneğin, verileri salt okunur erişim sağlamak için. Paylaşılan erişim imzaları (SAS) veri erişimi sınırlamak izin veren bir Azure depolama hesapları özelliğidir. Örneğin, verilere salt okunur erişim sağlama.

> [!IMPORTANT]
> Apache bırakabilmenizi kullanarak bir çözüm için etki alanına katılmış Hdınsight kullanmayı düşünün. Daha fazla bilgi için bkz: [etki alanına katılmış Hdınsight yapılandırma](./domain-joined/apache-domain-joined-configure.md) belge.

> [!WARNING]
> Hdınsight kümesi için varsayılan depolama tam erişimi olmalıdır.

## <a name="requirements"></a>Gereksinimler

* Bir Azure aboneliği
* C# veya Python. C# kod örneği, bir Visual Studio çözümü olarak sağlanır.

  * Visual Studio 2013, 2015 veya 2017 sürümü olması gerekir
  * Python 2.7 ya da daha yüksek bir sürüm olması gerekir

* Linux tabanlı Hdınsight kümesi veya [Azure PowerShell] [ powershell] -mevcut bir Linux tabanlı kümeniz varsa, paylaşılan erişim imzası kümeye eklemek için Ambari kullanabilirsiniz. Aksi durumda, bir küme oluşturmak ve paylaşılan erişim imzası küme oluşturma sırasında eklemek için Azure PowerShell'i kullanabilirsiniz.

    > [!IMPORTANT]
    > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Örnek dosyaları [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Bu depo, aşağıdaki öğeleri içerir:

  * Hdınsight ile kullanmak için bir depolama kapsayıcısı, depolanan ilke ve SAS oluşturabilirsiniz bir Visual Studio projesi
  * Hdınsight ile kullanmak için bir depolama kapsayıcısı, depolanan ilke ve SAS oluşturabilirsiniz bir Python komut dosyası
  * Bir Hdınsight kümesi oluşturabilir ve SAS kullanacak şekilde yapılandırma PowerShell Betiği.

## <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları

Paylaşılan erişim imzaları iki tür vardır:

* Geçici: başlangıç zamanı, bitiş saati ve SAS izinlerini tüm SAS URI'sini belirtilir.

* Erişim ilkesinde saklanan: depolanmış erişim ilkesi, bir blob kapsayıcısını gibi bir kaynak kapsayıcısı üzerinde tanımlanır. Bir ilke kısıtlamaları bir veya daha fazla paylaşılan erişim imzalarını yönetmek için kullanılabilir. Bir SAS depolanmış erişim ilkesi ile ilişkilendirdiğinizde, SAS kısıtlamaları - başlangıç saati, sona erme saati ve saklı erişim ilkesi için tanımlanan izinleri - devralır.

İki tür arasındaki farkı bir anahtar senaryo için önemlidir: iptal. Bir SAS bir URL olduğundan SAS edinir herkes, kimin başından itibaren istendiğinde bağımsız olarak kullanabilirsiniz. Bir SAS yayımlandığını, herkes tarafından kullanılabilir. Dağıtılan bir SAS dört özelliklerinden biri işlem yapılana kadar geçerlidir:

1. SAS belirtilen sona erme saati ulaşıldı.

2. SAS tarafından başvurulan depolanmış erişim ilkesinde belirtilen sona erme saati ulaşıldı. Aşağıdaki senaryolarda erişilmesi sona erme saati neden:

    * Gereken süre geçti.
    * Saklı erişim ilkesi, bir sona erme saati geçmişte sahip şekilde değiştirilir. Sona erme saati değiştirme SAS iptal etmek için bir yoludur.

3. SAS iptal etmek için başka bir yol olduğu SAS tarafından başvurulan depolanmış erişim ilkesi silinir. Aynı ada sahip depolanmış erişim ilkesi oluşturun, önceki ilke için tüm SAS belirteci (SAS bitiş saati olmayan geçtiyse) geçerli değil. SAS iptal etmek istiyorsanız, gelecekte bir sona erme saati ile erişim ilkesi oluşturun, farklı bir ad kullandığınızdan emin olun.

4. SAS oluşturmak için kullanılan hesap anahtar yeniden oluşturulacak. Anahtar yeniden kimlik doğrulaması başarısız önceki anahtar kullanan tüm uygulamalar neden olur. Tüm bileşenler için yeni anahtarı güncelleştirin.

> [!IMPORTANT]
> Paylaşılan erişim imzası URI imzayı oluşturmak için kullanılan hesap anahtarı ile ilişkilidir, ve ilişkili erişim ilkesi (varsa) depolanır. Hiçbir depolanmış erişim ilkesi belirtilirse, paylaşılan erişim imzası iptal etmek için yalnızca hesap anahtarı değiştirmek için yoludur.

Her zaman depolanmış erişim ilkeleri kullanmanızı öneririz. Depolanan ilkelerde kullanırken, imzalar iptal etmek veya sona erme tarihi gerektiği şekilde genişletir. Bu belge kullanımı adımlarda SAS oluşturmak için erişim ilkeleri depolanır.

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi için bkz: [SAS modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Depolanan ilke ve C kullanarak SAS oluşturma\#

1. Çözümü Visual Studio'da açın.

2. Çözüm Gezgini'nde sağ **SASToken** proje ve seçin **özellikleri**.

3. Seçin **ayarları** ve aşağıdaki girdiler için değerlerini ekleyin:

   * StorageConnectionString: Depolanan ilke ve için SAS oluşturmak istediğiniz depolama hesabı bağlantı dizesi. Biçiminde olmalıdır `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` nerede `myaccount` depolama hesabınızın adı ve `mykey` depolama hesabı için bir anahtardır.

   * Kapsayıcı adı: Erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.

   * SASPolicyName: için depolanan ilke oluşturmak için kullanılacak ad.

   * FileToUpload: Kapsayıcıya karşıya bir dosya yolu.

4. Projeyi çalıştırın. SAS oluşturulduktan sonra aşağıdakine benzer bilgiler görüntülenir:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    SAS İlkesi belirteci, depolama hesabı adı ve kapsayıcı adı kaydedin. Hdınsight kümenizle depolama hesabını ilişkilendirerek bu değerler kullanılır.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Depolanan ilke ve Python kullanarak SAS oluşturma

1. SASToken.py dosyasını açın ve aşağıdaki değerleri değiştirin:

   * İlke\_ad: için depolanan ilke oluşturmak için kullanılacak ad.

   * Depolama\_hesap\_ad: depolama hesabınızın adını.

   * Depolama\_hesap\_anahtarı: depolama hesabı anahtarı.

   * Depolama\_kapsayıcı\_ad: erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.

   * örnek\_dosya\_yol: bir kapsayıcıya karşıya bir dosya yolu.

2. Betiği çalıştırın. Betik tamamlandığında aşağıdaki metni benzer SAS belirteci görüntüler:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    SAS İlkesi belirteci, depolama hesabı adı ve kapsayıcı adı kaydedin. Hdınsight kümenizle depolama hesabını ilişkilendirerek bu değerler kullanılır.

## <a name="use-the-sas-with-hdinsight"></a>Hdınsight ile SAS kullanma

Bir Hdınsight kümesi oluştururken, bir birincil depolama hesabı belirtmeniz gerekir ve isteğe bağlı olarak ek depolama hesapları belirtebilirsiniz. Depolama ekleme bu yöntemlerin her ikisi de depolama hesapları ve kullanılan kapsayıcıları için tam erişim gerektirir.

Paylaşılan erişim imzası bir kapsayıcıya erişimi sınırlandırmak için kullanmak üzere özel bir girişe eklemek **çekirdek site** kümenin yapılandırması.

* İçin **Windows tabanlı** veya **Linux tabanlı** Hdınsight kümeleri, PowerShell kullanarak küme oluşturma sırasında giriş ekleyebilirsiniz.
* İçin **Linux tabanlı** Hdınsight kümeleri, Ambari kullanarak küme oluşturulduktan sonra yapılandırmayı Değiştir.

### <a name="create-a-cluster-that-uses-the-sas"></a>SAS kullanan bir küme oluşturun

SAS kullanan bir Hdınsight kümesi oluşturmanın bir örneği yer aldığı `CreateCluster` depo dizin. Kullanmak için aşağıdaki adımları kullanın:

1. Açık `CreateCluster\HDInsightSAS.ps1` dosyasını bir metin düzenleyicisinde açın ve belgenin başlangıcına aşağıdaki değerleri değiştirin.

    ```powershell
    # Replace 'mycluster' with the name of the cluster to be created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with the name of the group to be created
    $resourceGroupName = 'myresourcegroup'
    # Replace with the Azure data center you want to the cluster to live in
    $location = 'North Europe'
    # Replace with the name of the default storage account to be created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with the name of the SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with the name of the SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with the SAS token generated earlier
    $SASToken = 'sastoken'
    # Set the number of worker nodes in the cluster
    $clusterSizeInNodes = 3
    ```

    Örneğin, değiştirme `'mycluster'` oluşturmak istediğiniz küme adını. SAS değerler, bir depolama hesabı ve SAS belirteci oluştururken önceki adımları gelen değerler eşleşmelidir.

    Değerleri değişmiş sonra dosyayı kaydedin.

2. Yeni bir Azure PowerShell komut istemini açın. Azure PowerShell ile ilgili bilginiz veya yüklemediyseniz, bkz: [yükleyin ve Azure PowerShell yapılandırma][powershell].

1. İsteminden Azure aboneliğinize kimliğini doğrulamak için aşağıdaki komutu kullanın:

    ```powershell
    Login-AzureRmAccount
    ```

    İstendiğinde, Azure aboneliğinizin hesapla oturum açın.

    Hesabınızın birden çok Azure aboneliği ile ilişkili ise, kullanmanız gerekebilir `Select-AzureRmSubscription` kullanmak istediğiniz aboneliği seçin.

4. Dizinleri satırından değiştirmek `CreateCluster` HDInsightSAS.ps1 dosyasını içeren dizini. Komut dosyasını çalıştırmak için aşağıdaki komutu kullanın

    ```powershell
    .\HDInsightSAS.ps1
    ```

    Komut dosyasını çalıştırır gibi kaynak grubu ve depolama hesapları oluştururken, çıktı PowerShell istemi günlüğe kaydeder. Hdınsight kümesi için HTTP kullanıcı girmeniz istenir. Bu hesap, kümeye HTTP/s erişimini güvenli hale getirmek için kullanılır.

    Linux tabanlı bir küme oluşturuyorsanız, bir SSH kullanıcı hesabı adı ve parola istenir. Bu hesap, kümeye uzaktan oturum açmak için kullanılır.

   > [!IMPORTANT]
   > HTTP/s veya SSH kullanıcı adı ve parola istendiğinde, aşağıdaki ölçütlere uyan bir parola sağlamanız gerekir:
   >
   > * En az 10 karakter uzunluğunda olmalıdır
   > * En az bir rakam içermelidir
   > * En az bir alfasayısal olmayan karakter içermelidir
   > * En az bir büyük veya küçük harf içermelidir

Bu, genellikle yaklaşık 15 dakika tamamlamak bu komut dosyası için biraz uzun sürebilir. Komut hatasız tamamlandığında, küme oluşturuldu.

### <a name="use-the-sas-with-an-existing-cluster"></a>SAS ile var olan bir küme kullanın

Varolan bir Linux tabanlı kümeniz varsa, SAS ekleyebilirsiniz **çekirdek site** yapılandırmasını aşağıdaki adımları kullanarak:

1. Kümeniz için Ambari web kullanıcı arabirimini açın. Bu sayfa için https://YOURCLUSTERNAME.azurehdinsight.net adresidir. İstendiğinde, yönetici adı (Yönetici) kullanarak küme kimliğini ve ne zaman kullanılan parola küme oluşturma.

2. Ambari web kullanıcı Arabirimi sol taraftan seçin **HDFS** ve ardından **yapılandırmalar** sayfasının ortasında sekmesi.

3. Seçin **Gelişmiş** sekmesini tıklatın ve ardından bulana kadar kaydırın **özel çekirdek site** bölümü.

4. Genişletme **özel çekirdek site** bölüm sonra seçin ve son kaydırın **Özellik Ekle...**  bağlantı. İçin aşağıdaki değerleri kullanın **anahtar** ve **değeri** alanlar:

   * **Anahtar**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Değer**: SAS önceden çalıştırdığınız C# veya Python uygulama tarafından döndürülen

     Değiştir **CONTAINERNAME** kapsayıcı adıyla C# veya SAS uygulama ile kullanılır. Değiştir **STORAGEACCOUNTNAME** kullandığınız depolama hesabı adı ile.

5. Tıklatın **Ekle** bu anahtar ve değer Kaydet düğmesine ve ardından **kaydetmek** yapılandırma değişikliklerini kaydetmek için düğmesini. İstendiğinde, ("SAS depolama erişim örneğin ekleme") değişiklik açıklamasını ekleyin ve ardından **kaydetmek**.

    Tıklatın **Tamam** zaman değişiklikleri tamamlanmıştır.

   > [!IMPORTANT]
   > Değişikliğin yürürlüğe girmeden önce birkaç hizmeti yeniden başlatmanız gerekir.

6. Ambari web kullanıcı Arabirimi, seçin **HDFS** solda, listeden seçip **yeniden tüm** gelen **hizmet eylemleri** sağdaki listeden aşağı açılır. İstendiğinde, seçin **bakım Modu'nu** ve select yeniden tüm __Conform ".

    MapReduce2 ve YARN için bu işlemi yineleyin.

7. Hizmetleri yeniden başlattıktan sonra her birini seçin ve Bakım modundan devre dışı bırakma **hizmet eylemleri** açılır.

## <a name="test-restricted-access"></a>Kısıtlı erişim test

Sınırlı erişimi olduğunu doğrulamak için aşağıdaki yöntemleri kullanın:

* İçin **Windows tabanlı** Hdınsight kümeleri, kümeye bağlanmak için Uzak Masaüstü'nü kullanın. Daha fazla bilgi için bkz: [RDP kullanarak Hdınsight Bağlan](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

    Bağlantı kurulduktan sonra kullanmak **Hadoop komut satırı** bir komut istemi açmak için masaüstündeki simgesi.

* İçin **Linux tabanlı** Hdınsight kümeleri, kümeye bağlanmak için SSH kullanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Kümeye bağlantı kurulduktan sonra yalnızca okuma ve liste öğeleri SAS depolama hesabındaki yapabilecekleriniz doğrulamak için aşağıdaki adımları kullanın:

1. Kapsayıcının içeriğini listelemek için isteminden şu komutu kullanın: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Değiştir **SASCONTAINER** SAS depolama hesabı için oluşturulan kapsayıcı adı. Değiştir **SASACCOUNTNAME** SAS kullanılan depolama hesabı adı.

    Bu liste kapsayıcı ve SAS oluşturulduğunda karşıya dosya içerir.

2. Dosya içeriğini okuyabilirsiniz doğrulamak için aşağıdaki komutu kullanın. Değiştir **SASCONTAINER** ve **SASACCOUNTNAME** önceki adımda olduğu gibi. Değiştir **FILENAME** önceki komutta gösterilen dosya adı:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Bu komut dosyasının içeriğini listeler.

3. Yerel dosya sistemine dosyasını karşıdan yüklemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Bu komut dosyası adlı bir yerel dosya yüklemeleri **testdosyası.txt**.

4. Yerel dosya adlı yeni bir dosya karşıya yüklemek için aşağıdaki komutu kullanın **testupload.txt** SAS depolama:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Aşağıdakine benzer bir ileti alırsınız:

        put: java.io.IOException

    Depolama konumu okuma + liste yalnızca olduğundan bu hata oluşur. Yazılabilir olduğundan kümenin varsayılan depolama birimindeki verileri yerleştirmek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    Bu süre, işlem başarıyla tamamlanmalıdır.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="a-task-was-canceled"></a>Bir görev iptal edildi

**Belirtiler**: PowerShell Betiği kullanılarak bir küme oluştururken, aşağıdaki hata iletisini alabilirsiniz:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Neden**: bir parola (için Linux tabanlı kümelerde) veya küme için yönetici/HTTP kullanıcı için SSH kullanıcı kullanıyorsanız bu hata oluşabilir.

**Çözümleme**: aşağıdaki ölçütlere uyan bir parola kullanın:

* En az 10 karakter uzunluğunda olmalıdır
* En az bir rakam içermelidir
* En az bir alfasayısal olmayan karakter içermelidir
* En az bir büyük veya küçük harf içermelidir

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight kümenize sınırlı erişim depolama eklemek öğrendiniz, kümenizde verilerle çalışmak için diğer yollarını öğrenmek:

* [HDInsight ile Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hadoop/hdinsight-use-pig.md)
* [Hdınsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
