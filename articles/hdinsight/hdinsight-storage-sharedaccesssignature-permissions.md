---
title: Paylaşılan erişim imzaları - Azure HDInsight'ı kullanarak erişimi kısıtlama
description: Azure storage bloblarında depolanan verilere HDInsight erişimi kısıtlamak için paylaşılan erişim imzaları'nı kullanmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 7fa46e3a5f0ed6504e4bc927caa0378d75fcc4a7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686993"
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>HDInsight ile verilere erişimi kısıtlamak için Azure depolama paylaşılan erişim imzaları kullanma

HDInsight kümesi ile ilişkili Azure depolama hesaplarında veri tam erişimi vardır. Blob kapsayıcı paylaşılan erişim imzaları, verilere erişimi kısıtlamak için kullanabilirsiniz. Paylaşılan erişim imzaları (SAS), verilere erişimi sınırlamanıza olanak sağlayan bir Azure depolama hesapları özelliğidir. Örneğin, verilere yalnızca okuma erişimi sağlama.

> [!IMPORTANT]  
> Apache Ranger'ı kullanarak bir çözüm için etki alanına katılmış HDInsight kullanmayı düşünün. Daha fazla bilgi için [etki alanına katılmış HDInsight yapılandırma](./domain-joined/apache-domain-joined-configure.md) belge.

> [!WARNING]  
> HDInsight, küme için varsayılan depolama alanı için tam erişimi olmalıdır.

## <a name="requirements"></a>Gereksinimler

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Bir Azure aboneliği
* C# veya Python. C# kod örneği, bir Visual Studio çözümü olarak sağlanır.

  * Visual Studio 2013, 2015 veya 2017 sürümü olmalıdır
  * Python sürüm 2.7 veya üstü olmalıdır

* Linux tabanlı HDInsight kümesi veya [Azure PowerShell] [ powershell] -var olan bir Linux tabanlı küme varsa, bir paylaşılan erişim imzası kümeye eklemek için Apache Ambari kullanabilirsiniz. Aksi durumda, küme oluşturma ve küme oluşturma sırasında bir paylaşılan erişim imzası eklemek için Azure PowerShell kullanabilirsiniz.

    > [!IMPORTANT]  
    > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Örnek dosyaları [ https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature ](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Bu depo, aşağıdaki öğeleri içerir:

  * Depolama kapsayıcı, depolanan ilke ve SAS, HDInsight ile kullanmak için oluşturabileceğiniz bir Visual Studio projesi
  * Depolama kapsayıcı, depolanan ilke ve SAS, HDInsight ile kullanmak için oluşturabileceğiniz bir Python betiği
  * Bir HDInsight kümesi oluşturma ve SAS'ı kullanacak şekilde yapılandırma PowerShell Betiği.

## <a name="shared-access-signatures"></a>Paylaşılan Erişim İmzaları

Paylaşılan erişim imzaları iki tür vardır:

* Geçici: Başlangıç zamanı, süre sonu ve SAS izinlerini tüm SAS URI öğesinde belirtilir.

* Depolanmış erişim ilkesini: Bir depolanmış erişim ilkesini bir blob kapsayıcısı gibi bir kaynak kapsayıcısı üzerinde tanımlanır. Bir ilke kısıtlamaları bir veya daha fazla paylaşılan erişim imzalarını yönetmek için kullanılabilir. Bir SAS bir depolanmış erişim ilkesini ile ilişkilendirdiğinizde, SAS kısıtlamaları - başlangıç zamanı, süre sonu ve izinleri için depolanmış erişim ilkesini tanımlanan - devralır.

Bir anahtar senaryosu için iki biçim arasındaki fark önemlidir: iptal etme. Bir SAS URL olduğundan herkes SAS alır, kimin başlangıç istendiğinde bağımsız olarak kullanabilirsiniz. Bir SAS yayımlandığını, herkes tarafından kullanılabilir. Dağıtılan bir SAS dört şeylerden biri oluşuncaya kadar geçerlidir:

1. SAS üzerinde belirtilen süre sonu ulaşıldı.

2. SAS'den başvurulan depolanmış erişim ilkesini belirtilen süre sonu ulaşıldı. Aşağıdaki senaryolarda erişilmesi gereken süre sonu neden:

    * Zaman aralığı geçti.
    * Depolanmış erişim ilkesini bir bitiş zamanı geçmişte sahip şekilde değiştirilir. Sona erme saati değiştirme SAS iptal etmek için bir yoludur.

3. SAS iptal etmek için başka bir yolu olan SAS tarafından başvurulan depolanmış erişim ilkesini silinir. Aynı ada sahip depolanmış erişim ilkesini yeniden oluşturma, önceki ilke için tüm SAS belirteçlerini (süre sonu SAS üzerinde değil geçtiyse) geçerli değil. SAS iptal etmek istiyorsanız, farklı bir ad kullanırsanız erişim ilkesi gelecekte bir sona erme saati ile yeniden emin olun.

4. SAS oluşturmak için kullanılan hesap anahtarı yeniden oluşturuldu. Anahtarı yeniden kimlik doğrulaması başarısız önceki anahtar kullanan tüm uygulamalar neden olur. Tüm bileşenler için yeni anahtarı güncelleştirin.

> [!IMPORTANT]  
> Paylaşılan erişim imzası URI'si imza oluşturmak için kullanılan hesap anahtarı ile ilişkilidir, ve ilişkili erişim ilkesi (varsa) depolanır. Hiçbir depolanmış erişim ilkesini belirtilirse, paylaşılan erişim imzası iptal etmek için tek yolu hesap anahtarını değiştirmektir.

Saklı erişim ilkeleri her zaman kullanmanızı öneririz. Saklı ilkeler kullanıldığında imzaları iptal veya sona erme tarihi gerektiği şekilde genişletin. Bu belgeyi kullanmak adımda SAS oluşturmak için erişim ilkeleri depolanır.

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi için bkz [SAS modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Depolanan ilke ve C kullanarak SAS oluşturma\#

1. Visual Studio içinde çözümü açın.

2. Çözüm Gezgini'nde sağ **SASToken** seçin ve proje **özellikleri**.

3. Seçin **ayarları** ve aşağıdaki girdileri için değerleri ekleyin:

   * StorageConnectionString: Depolanan ilke ve için SAS oluşturmak istediğiniz depolama hesabı için bağlantı dizesi. Biçim olmalıdır `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` burada `myaccount` depolama hesabınızın adı ve `mykey` depolama hesabı için anahtar.

   * Kapsayıcı adı: Erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.

   * SASPolicyName: Saklı ilkesi oluşturmak için kullanılacak ad.

   * FileToUpload: Kapsayıcı için karşıya bir dosya yolu.

4. Projeyi çalıştırın. SAS oluşturulduktan sonra aşağıdaki metne benzer bilgiler görüntülenir:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    SAS İlkesi belirteç, depolama hesabı adı ve kapsayıcı adını kaydedin. HDInsight kümenizle depolama hesabını ilişkilendirerek bu değerler kullanılır.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Depolanan ilke ve Python kullanarak SAS oluşturma

1. SASToken.py dosyasını açın ve aşağıdaki değerleri değiştirin:

   * İlke\_adı: Saklı ilkesi oluşturmak için kullanılacak ad.

   * Depolama\_hesabı\_adı: Depolama hesabınızın adı.

   * Depolama\_hesabı\_anahtarı: Depolama hesabı anahtarı.

   * Depolama\_kapsayıcı\_adı: Erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.

   * örnek\_dosya\_yolu: Kapsayıcı için karşıya bir dosya yolu.

2. Betiği çalıştırın. Betik tamamlandığında aşağıdaki metne benzer bir SAS belirteci görüntüler:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    SAS İlkesi belirteç, depolama hesabı adı ve kapsayıcı adını kaydedin. HDInsight kümenizle depolama hesabını ilişkilendirerek bu değerler kullanılır.

## <a name="use-the-sas-with-hdinsight"></a>HDInsight ile SAS kullanın

Bir HDInsight kümesi oluştururken, birincil depolama hesabı belirtin ve isteğe bağlı olarak ek depolama hesapları belirtebilirsiniz. Depolama ekleme bu yöntemlerin ikisi de kullanılan kapsayıcıları ve depolama hesapları için tam erişim gerektirir.

Bir kapsayıcıya erişimi sınırlamak için bir paylaşılan erişim imzası kullanmak için özel bir girişe ekleme **çekirdek site** kümenin yapılandırmasını.

* İçin **Windows tabanlı** veya **Linux tabanlı** HDInsight kümeleri, PowerShell kullanarak küme oluşturma sırasında giriş ekleyebilirsiniz.
* İçin **Linux tabanlı** HDInsight kümeleri Ambari kullanarak küme oluşturulduktan sonra yapılandırmasını değiştirin.

### <a name="create-a-cluster-that-uses-the-sas"></a>SAS'ı kullanan bir kümesi oluşturma

SAS'ı kullanan bir HDInsight kümesi oluşturmanın bir örneği yer aldığı `CreateCluster` deponun dizin. Bunu kullanmak için aşağıdaki adımları kullanın:

1. Açık `CreateCluster\HDInsightSAS.ps1` dosyasını bir metin düzenleyicisinde ve belgenin başlangıcında şu değerleri değiştirin.

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

    Örneğin, değiştirme `'mycluster'` olarak oluşturmak istediğiniz kümenin adıdır. SAS değerler, bir depolama hesabı ve SAS belirteci oluştururken önceki adımdan değerler eşleşmelidir.

    Değerler değiştirildiğinde sonra dosyayı kaydedin.

2. Yeni bir Azure PowerShell istemi açın. Azure PowerShell ile bilginiz veya yüklemediyseniz, bkz. [yüklemek ve Azure PowerShell yapılandırma][powershell].

1. İsteminde, Azure aboneliğinize kimliğini doğrulamak için aşağıdaki komutu kullanın:

    ```powershell
    Connect-AzAccount
    ```

    İstendiğinde, Hesapla Azure aboneliğiniz için oturum açın.

    Hesabınız birden çok Azure aboneliği ile ilişkili ise, kullanmanız gerekebilir `Select-AzSubscription` kullanmak istediğiniz aboneliği seçmek için.

4. İsteminde dizinleri `CreateCluster` HDInsightSAS.ps1 dosyasını içeren dizin. Ardından betiği çalıştırmak için aşağıdaki komutu kullanın

    ```powershell
    .\HDInsightSAS.ps1
    ```

    Komut dosyası çalıştırılırken, kaynak grubu ve depolama hesapları oluşturduğundan PowerShell istemine çıkış kaydeder. HDInsight kümesi için HTTP kullanıcısı girmeniz istenir. Bu hesap, kümeye HTTP/s erişimini güvenli hale getirmek için kullanılır.

    Linux tabanlı bir küme oluştururken bir SSH kullanıcı hesabı adı ve parola istenir. Bu hesap, kümeye uzaktan oturum açmak için kullanılır.

   > [!IMPORTANT]  
   > HTTP/s veya SSH kullanıcı adı ve parola istendiğinde, aşağıdaki ölçütlere uyan bir parola sağlamanız gerekir:
   >
   > * En az 10 karakter uzunluğunda olmalıdır.
   > * En az bir rakam içermelidir.
   > * En az bir alfasayısal olmayan karakter içermelidir.
   > * En az bir büyük veya küçük harf içermelidir.

Bir süredir bu betik, tamamlanması genellikle yaklaşık 15 dakika sürer. Betik herhangi bir hata olmadan tamamlandığında, küme oluşturuldu.

### <a name="use-the-sas-with-an-existing-cluster"></a>SAS ile var olan bir küme kullanın

Var olan bir Linux tabanlı küme varsa SAS'ye ekleyebilirsiniz **çekirdek site** yapılandırmasını aşağıdaki adımları kullanarak:

1. Kümeniz için Ambari web kullanıcı arabirimini açın. Bu sayfa adresi https://YOURCLUSTERNAME.azurehdinsight.net. İstendiğinde, yönetici adı (Yönetici) kullanarak kümeye kimlik doğrulaması ve parola, kullanılan küme oluşturma.

2. Ambari web kullanıcı Arabirimi sol taraftan seçin **HDFS** seçip **yapılandırmaları** sayfanın ortasındaki sekmesi.

3. Seçin **Gelişmiş** sekmesine ve ardından bulana kadar kaydırın **özel çekirdek-site** bölümü.

4. Genişletin **özel çekirdek-site** bölümüne ve ardından seçin ve son gidin **Özellik Ekle...**  bağlantı. İçin aşağıdaki değerleri kullanın **anahtarı** ve **değer** alanlar:

   * **Anahtar**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Değer**: SAS tarafından döndürülen C# ya da daha önce çalıştırdığınız Python uygulaması

     Değiştirin **CONTAINERNAME** kapsayıcı adı ile C# veya SAS uygulamayla birlikte kullanılır. Değiştirin **STORAGEACCOUNTNAME** ile kullanılan depolama hesabı adı.

5. Tıklayın **Ekle** bu anahtar ve değer Kaydet düğmesine ve ardından tıklayın **Kaydet** yapılandırma değişikliklerini kaydetmek için düğme. İstendiğinde, değişikliği ("SAS depolama erişim örneğin ekleme") bir açıklama ekleyin ve ardından **Kaydet**.

    Tıklayın **Tamam** zaman değişiklikleri tamamlanmıştır.

   > [!IMPORTANT]  
   > Değişikliğin etkili olmadan önce birkaç hizmeti yeniden başlatmanız gerekir.

6. Ambari web kullanıcı Arabirimi, seçin **HDFS** sol taraftaki listeden seçip **yeniden tüm etkilenen** gelen **hizmet eylemleri** listesi sağdaki aşağı açılır. Sorulduğunda, __tüm yeniden onaylayın__.

    MapReduce2 ve YARN için bu işlemi yineleyin.

7. Hizmetleri yeniden başlattıktan sonra her birini seçin ve Bakım modundan devre dışı **hizmet eylemleri** açılır.

## <a name="test-restricted-access"></a>Sınırlı erişimi test etme

Sınırlı erişimi olduğunu doğrulamak için kümeye bağlanmak için SSH kullanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Kümeye bağlandıktan sonra SAS depolama hesabına yalnızca okuma ve liste öğeleri için doğrulamak için aşağıdaki adımları kullanın:

1. Kapsayıcı içeriğini listelemek için isteminden şu komutu kullanın: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Değiştirin **SASCONTAINER** SAS depolama hesabı için oluşturulan kapsayıcı adı. Değiştirin **SASACCOUNTNAME** SAS için kullanılan depolama hesabı adı ile.

    Listede, kapsayıcı ve SAS oluşturulduğunda karşıya yüklediğiniz dosyaya bulunur.

2. Dosyanın içeriğini okuyabilir doğrulamak için aşağıdaki komutu kullanın. Değiştirin **SASCONTAINER** ve **SASACCOUNTNAME** önceki adımla. Değiştirin **FILENAME** önceki komutta gösterilen dosya adı:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Bu komut dosyasının içeriğini listeler.

3. Yerel dosya sistemine dosya indirmek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Bu komut dosyası adlı bir yerel dosya yüklemeleri **testfile.txt**.

4. Yerel dosya adlı yeni bir dosya karşıya yüklemek için aşağıdaki komutu kullanın **testupload.txt** SAS depolama:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Aşağıdaki metne benzer bir ileti alırsınız:

        put: java.io.IOException

    Okuma + liste yalnızca depolama konumu olduğu için bu hata oluşur. Yazılabilir olduğundan kümenin varsayılan depolama üzerinde verileri yerleştirmek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    Bu süre, işlem başarıyla tamamlanmalıdır.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="a-task-was-canceled"></a>Bir görev iptal edildi

**Belirtiler**: PowerShell betiğini kullanarak bir kümeyi oluştururken, aşağıdaki hata iletisini alabilirsiniz:

    New-AzHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Neden**: Bir parola (için Linux tabanlı kümeler) veya küme için yönetici/HTTP kullanıcısı için SSH kullanıcısı kullanıyorsanız bu hata oluşabilir.

**Çözüm**: Aşağıdaki ölçütlere uyan bir parola kullanın:

* En az 10 karakter uzunluğunda olmalıdır.
* En az bir rakam içermelidir.
* En az bir alfasayısal olmayan karakter içermelidir.
* En az bir büyük veya küçük harf içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

Sınırlı erişimli depolama, HDInsight kümenize eklemek öğrendiniz, kümenizde verilerle çalışmak için yollar öğrenin:

* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
