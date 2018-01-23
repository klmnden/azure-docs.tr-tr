---
title: "HDInsight'ta R Server ile çalışmaya başlama - Azure | Microsoft Docs"
description: "HDInsight kümesi üzerinde R Server içeren bir Apache Spark oluşturma ve küme üzerinde bir R betiği gönderme hakkında bilgi edinin."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: e688068efb41cdccbeb23de3c8ad7a09021e5b3f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="get-started-with-r-server-on-hdinsight"></a>HDInsight üzerinde R Server kullanma

Azure HDInsight, HDInsight kümenizle tümleştirilecek bir R Server seçeneği içerir. Bu seçenek, R betiklerinin dağıtılmış hesaplamaları çalıştırmak için Spark ve MapReduce kullanmasına olanak tanır. Bu makalede, HDInsight kümesinde R Server oluşturmayı öğreneceksiniz. Ardından dağıtılmış R hesaplamalarında Spark kullanımını gösteren R betiğini çalıştırmayı öğreneceksiniz.


## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**: Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Daha fazla bilgi için bkz. [Microsoft Azure ücretsiz denemesini alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Güvenli Kabuk (SSH) istemcisi**: HDInsight kümesine uzaktan bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için bir SSH istemcisi kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).
* **SSH anahtarları (isteğe bağlı)**: Bir parola veya ortak anahtar kullanarak, kümeye bağlanmak için kullanılan SSH hesabını güvenli hale getirebilirsiniz. Parola kullanmak daha kolaydır ve ortak/özel anahtar çifti oluşturmak zorunda kalmadan çalışmaya başlamanızı sağlar. Ancak, bir anahtar kullanılması daha güvenlidir.

  > [!NOTE]
  > Bu makaledeki adımlarda parola kullandığınız varsayılmıştır.


## <a name="automate-cluster-creation"></a>Küme oluşturmayı otomatikleştirme

HDInsight R Server örnekleri oluşturma işlemini Azure Resource Manager şablonları, SDK ve PowerShell kullanarak otomatik hale getirebilirsiniz.

* Azure Resource Manager şablonuyla R Server örneği oluşturmak için bkz. [R Server HDInsight kümesi dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
* .NET SDK kullanarak R Server örneği oluşturmak için bkz. [HDInsight’ta .NET SDK kullanarak Linux tabanlı kümeler oluşturma](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md).
* PowerShell kullanarak R Server dağıtımı yapmak için bkz. [HDInsight'ta Azure PowerShell kullanarak Linux tabanlı kümeler oluşturma](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-a-cluster-by-using-the-azure-portal"></a>Azure portalını kullanarak küme oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Yeni** > **Zeka + analiz** > **HDInsight**'ı seçin.

    ![Yeni küme oluşturma görüntüsü](./media/r-server-get-started/newcluster.png)

3. **Hızlı oluşturma** deneyiminde **Küme adı** kutusuna küme için bir ad girin. Birden fazla Azure aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçmek için **Abonelik** kutusunu kullanın.

    ![Küme adı ve abonelik seçimleri](./media/r-server-get-started/clustername.png)

4. **Küme yapılandırması** bölmesini açmak için **Küme türü**’nü seçin. **Küme Yapılandırması** bölmesinde aşağıdaki seçenekleri belirtin:

    * **Küme türü**: **R Server**'ı seçin.
    * **Sürüm**: Kümeye yüklenecek R Server sürümünü seçin. Şu anda kullanılabilir sürüm: **R Server 9.1 (HDI 3.6)**. Kullanılabilir R Server sürümlerinin sürüm notları [MSDN](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) sitesinde bulunabilir.
    * **R Server için R Studio topluluk sürümü**: Tarayıcı tabanlı bu IDE, kenar düğümüne varsayılan olarak yüklenir. Yüklememeyi tercih ederseniz, onay kutusunu temizleyin. Yüklemeyi seçerseniz, RStudio Server oturum açma sayfasına erişim URL'si, kümeniz oluşturulduktan sonra kümenizin portal uygulaması bölmesinde yer alır.
    * Diğer seçenekleri varsayılan değerlerinde bırakın ve küme türünü kaydetmek için **Seç** düğmesini kullanın.

        !["Küme türü" bölmesinin ekran görüntüsü](./media/r-server-get-started/clustertypeconfig.png)

5. **Temel Bilgiler** bölmesindeki **Küme oturum açma kullanıcı adı** ve **Küme oturum açma parolası** kutularında, kümenin kullanıcı adını ve parolasını (sırasıyla) girin.

6. **Secure Shell (SSH) kullanıcı adı** kutusunda SSH kullanıcı adını belirtin. SSH, bir SSH istemcisi kullanarak kümeye uzaktan bağlanmak için kullanılır. SSH kullanıcısını bu kutuda veya küme oluşturulduktan sonra (kümenin **Yapılandırma** sekmesinde) belirtebilirsiniz.
   
   > [!NOTE] 
   > R Server, “remoteuser” SSH kullanıcı adını bekleyecek şekilde yapılandırılmıştır. Farklı bir kullanıcı adı kullanırsanız, küme oluşturulduktan sonra ek bir adım gerçekleştirmeniz gerekir.

7. Bir ortak anahtar kullanmayı tercih etmediğiniz sürece, kimlik doğrulama türü olarak **PAROLA** kullanmak için **Kümede oturum açmak için kullanılan parolayı kullan** onay kutusunu seçili durumda bırakın. Küme üzerindeki R Server'a Visual Studio için R Araçları, RStudio veya başka bir masaüstü IDE gibi bir uzak istemci yoluyla erişmek için ortak/özel anahtar çifti gerekir. RStudio Server topluluk sürümünü yüklerseniz bir SSH parolası seçmeniz gerekir.     

   Ortak/özel anahtar çiftini oluşturmak ve kullanmak için, **Küme oturum açmak için kullanılan parolayı kullan** onay kutusunu temizleyin. Ardından **ORTAK ANAHTAR**'ı seçin ve aşağıda gösterildiği gibi ilerleyin. Bu yönergelerde, ssh-keygen veya eşdeğeri ile birlikte Cygwin'in yüklü olduğu varsayılır.

   a. Dizüstü bilgisayarınızda komut isteminden bir genel/özel anahtar çifti oluşturun:

        ssh-keygen -t rsa -b 2048

   b. Anahtar dosyasını adlandırmak için istemleri izleyin ve daha fazla güvenlik için bir şifre girin. Ekranınız aşağıdaki görüntü gibi görünmelidir:

      ![Windows'da SSH komut satırı](./media/r-server-get-started/sshcmdline.png)

      Bu komut <özel-anahtar-dosya-adı>.pub adıyla bir özel anahtar dosyası ve ortak anahtar dosyası oluşturur. Bu örnekte dosyalar furiosa ve furiosa.pub'dır:

      ![Dir komutunun örnek sonucu](./media/r-server-get-started/dir.png)

   c. HDI küme kimlik bilgilerini atarken ortak anahtar dosyasını (&#42;.pub) belirtin. Kaynak grubunuzu ve bölgenizi onaylayın, sonra da **İleri**'yi seçin.

      ![Kimlik Bilgileri bölmesi](./media/r-server-get-started/publickeyfile.png)  

   d. Dizüstü bilgisayarınızda özel anahtar dosyası üzerindeki izinleri değiştirin:

        chmod 600 <private-key-filename>

   e. Özel anahtar dosyasını uzaktan oturum açma için SSH ile birlikte kullanın:

        ssh –i <private-key-filename> remoteuser@<hostname public ip>

      İsterseniz, istemcide R Server için Hadoop Spark işlem bağlamınızın tanımının bir parçası olarak özel anahtar dosyası kullanabilirsiniz. Daha fazla bilgi için bkz. [Spark için İşlem Bağlamı Oluşturma](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started).

8. Hızlı oluşturma işlemi sizi **Depolama** bölmesine geçirir. Orada, kümenin kullandığı HDFS dosya sisteminin birincil konumu olarak kullanılacak depolama hesabı ayarlarını seçersiniz. Yeni veya mevcut bir Azure depolama hesabı seçin veya mevcut Azure Data Lake Store hesabını seçin.

    - Bir Azure depolama hesabı seçerseniz, **Depolama hesabı seçin** öğesini ve ardından uygun hesabı seçerek mevcut bir hesap belirtebilirsiniz. **Depolama hesabı seçin** bölümündeki **Yeni oluştur** bağlantısını kullanarak yeni bir hesap oluşturun.

      > [!NOTE]
      > **Yeni**'yi seçerseniz, yeni depolama hesabı için bir ad girmeniz gerekir. Ad kabul edilirse yeşil bir onay işareti görünür.

      **Varsayılan kapsayıcı**, varsayılan olarak kümenin adını alır. Varsayılan değeri bırakın.

      Yeni bir depolama hesabı seçtiyseniz, bunun bölgesini belirtmek için bilgi isteminde **Konum**'u kullanın.  

         ![Veri kaynağı bölmesi](./media/r-server-get-started/datastore.png)  

      > [!IMPORTANT]
      > Varsayılan veri kaynağı için konum seçildiğinde, HDInsight kümesinin konumu da ayarlanır. Küme ve varsayılan veri kaynağı aynı bölgede olmalıdır.

    - Mevcut Data Lake Store hesabını kullanmak istiyorsanız, kullanılacak hesabı seçin. Ardından depoya erişime olanak tanımak için küme *EKLEME* kimliğini kümenize ekleyin. Bu işlemle ilgili daha fazla bilgi için bkz. [Azure Portal'ı kullanarak Data Lake Store ile HDInsight kümeleri oluşturma](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

    Veri kaynağı yapılandırmasını kaydetmek için **Seç** düğmesini kullanın.


9. Bundan sonra **Özet** bölmesi görüntülenir ve tüm ayarlarınızı doğrulayabilirsiniz. Burada, kümenizdeki sunucu sayısını değiştirmek için kümenizin boyutunu değiştirebilirsiniz. Ayrıca çalıştırmak istediğiniz tüm betik eylemlerini de belirtebilirsiniz. Daha büyük bir kümeye ihtiyaç duymadıkça, çalışan düğümleri sayısını varsayılan **4** değerinde bırakın. Bölmede kümenin tahmini maliyeti de gösterilir.

    ![Küme özeti](./media/r-server-get-started/clustersummary.png)

   > [!NOTE]
   > Gerekirse, çalışan düğümlerinin sayısını artırmak veya azaltmak üzere Portal üzerinden (**Küme** > **Ayarlar** > **Kümeyi ölçeklendir**) kümenizi yeniden boyutlandırabilirsiniz. Bu yeniden boyutlandırma işlemi, kullanımda olmadığında kümeyi boşa almak veya daha büyük görev gereksinimlerini karşılamak üzere kapasite artırmak için faydalı olabilir.
   >
   >

   Kümenizi, veri düğümlerini ve kenar düğümünü boyutlandırırken göz önünde bulundurulması gereken faktörler aşağıda verilmiştir:  

   * Spark üzerinde dağıtılmış R Server analizlerinin performansı, veriler büyük olduğunda çalışan düğümlerinin sayısıyla doğru orantılıdır.  

   * R Server analizlerinin performansı, analiz edilmekte olan verilerin boyutu ile doğrusal yönde ilerler. Örnek:  

     * Küçük ila orta büyüklükteki veriler için performans, kenar düğümündeki yerel bir işlem bağlamında veriler analiz edildiğinde en üst düzeydedir. Yerel ve Spark işlem bağlamlarının en iyi şekilde çalıştığı senaryolar hakkında daha fazla bilgi için bkz. [HDInsight üzerinde R Server için işlem bağlamı seçenekleri](r-server-compute-contexts.md).<br>
     * Kenar düğümünde oturum açıp R betiğinizi çalıştırırsanız kenar düğümde ScaleR rx işlevleri hariç tüm işlevler *yerel olarak* yürütülür. Kenar düğüm üzerindeki bellek ve çekirdek sayısı ayarlanırken bu durum dikkate alınmalıdır. Aynı durum, HDI üzerinde R Server’ı dizüstü bilgisayarınızdan uzak bir işlem bağlamı olarak çalıştırdığınızda geçerli olur.

   Düğüm fiyatlandırma yapılandırmasını kaydetmek için **Seç** düğmesini kullanın.

   ![Düğüm fiyatlandırma katmanları bölmesi](./media/r-server-get-started/pricingtier.png)

   **Şablon ve parametreleri indirme** bağlantısı da yer almaktadır. Bu bağlantıyı seçtiğinizde, seçili yapılandırma ile küme oluşturma işlemini otomatik hale getirmek için kullanılabilecek betikler gösterilir. Bu betikler, kümeniz oluşturulduktan sonra Azure Portal girişinde de bulunabilir.

   > [!NOTE]
   > Kümenin oluşturulması genellikle yaklaşık 20 dakika sürer. Oluşturma işlemini denetlemek için, Başlangıç Panosu’ndaki kutucuğu veya sayfanın solundaki **Bildirimler** girişini kullanın.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>RStudio Server’a bağlanma

Yüklemenize RStudio Server topluluk sürümünü eklemeyi seçtiyseniz, RStudio oturum açma sayfasına iki yöntemle erişebilirsiniz:

- Aşağıdaki URL'ye gidin (burada *CLUSTERNAME*, oluşturduğunuz kümenin adıdır):

    https://*CLUSTERNAME*.azurehdinsight.net/rstudio/

- Azure portalında kümenizin girişini açın, **R Server panoları** hızlı bağlantısını seçin ve ardından **R Studio panosu**'nu seçin:

  ![RStudio panosuna erişim](./media/r-server-get-started/rstudiodashboard1.png)

  ![RStudio panosuna erişim](./media/r-server-get-started/rstudiodashboard2.png)

> [!IMPORTANT]
> Kullanılan yöntem ne olursa olsun, ilk kez oturum açtığınızda iki kez kimlik doğrulaması yapmanız gerekir. İlk kimlik doğrulamasında kümenin yönetici kullanıcı kimliğini ve parolasını belirtin. İkinci istemde SSH kullanıcı kimliğini ve parolasını belirtin. Bunu izleyen oturum açma işlemlerinde yalnızca SSH kullanıcı kimliği ve parolası gerekir.

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a>R Server kenar düğümüne bağlanma

Şu komut yoluyla SSH kullanarak HDInsight kümesinin R Server uç düğümüne bağlanabilirsiniz:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` IP adresini Azure Portal'da bulabilirsiniz. Kümenizi seçin ve sonra da **Tüm Ayarlar** > **Uygulamalar** > **RServer**'ı seçin. Bu seçim, uç düğümünün SSH uç noktası bilgilerini gösterir.
>
> ![Uç düğümünün SSH uç noktası resmi](./media/r-server-get-started/sshendpoint.png)
>
>

SSH kullanıcı hesabınızın güvenliğini sağlamaya yardımcı olması için parola kullandıysanız parolayı girmeniz istenir. Ortak anahtar kullandıysanız eşleşen özel anahtarı belirtmek için `-i` parametresini kullanmanız gerekebilir. Örnek:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Daha fazla bilgi için bkz. [SSH kullanarak HDInsight'a (Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

Bağlandıktan sonra aşağıdakine benzer bir isteme ulaşırsınız:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Birden çok eş zamanlı kullanıcı etkinleştirme

RStudio topluluk sürümünün çalıştığı kenar düğümüne daha fazla kullanıcı ekleyerek aynı anda birden fazla eşzamanlı kullanıcıyı etkinleştirebilirsiniz.

HDInsight kümesi oluşturduğunuzda bir HTTP kullanıcısı ve bir SSH kullanıcısı olmak üzere iki kullanıcı sağlamanız gerekir.

![Küme kullanıcısı ve SSH kullanıcısının kimlik bilgilerini girme](./media/r-server-get-started/concurrent-users-1.png)

- **Küme oturum açma kullanıcı adı**: Oluşturduğunuz HDInsight kümelerini korumak için kullanılan HDInsight ağ geçidinden kimlik doğrulaması yapmak için kullanılan HTTP kullanıcısı. Bu HTTP kullanıcısı Ambari UI, YARN UI ve diğer UI bileşenlerine erişmek için kullanılır.
- **Secure Shell (SSH) kullanıcı adı**: Kümeye Secure Shell üzerinden erişmek için kullanılan SSH kullanıcısı. Bu kullanıcı Linux sisteminde tüm baş düğümler, çalışan düğümleri ve kenar düğümler için kullanılan kullanıcıdır. Bu sayede uzak kümedeki düğümlere erişmek için SSH kullanabilirsiniz.

HDInsight türü küme üzerindeki Microsoft R Server'da kullanılan RStudio Server topluluk sürümü, oturum açma sistemi olarak yalnızca Linux kullanıcı adı ve parolasını kabul eder. Belirteç iletmeyi desteklemez. Bu nedenle yeni bir küme oluşturduysanız ve buna erişmek için RStudio kullanmak istiyorsanız iki kez oturum açmanız gerekir.

1. HTTP kullanıcısı kimlik bilgilerini kullanarak HDInsight ağ geçidi üzerinden oturum açın: 

    ![HTTP kullanıcısı kimlik bilgilerini girme](./media/r-server-get-started/concurrent-users-2a.png)

2. SSH kullanıcısı kimlik bilgilerini kullanarak RStudio oturumu açın:
  
    ![SSH kullanıcısı kimlik bilgilerini girme](./media/r-server-get-started/concurrent-users-2b.png)

Şu anda bir HDInsight kümesi sağlanırken tek bir SSH kullanıcı hesabı oluşturabilirsiniz. Birden fazla kullanıcının HDInsight kümeleri üzerindeki Microsoft R Server'a erişmesini sağlamak için Linux sisteminde ek kullanıcılar oluşturmanız gerekir.

Kümenin uç düğümünde RStudio Server topluluk sürümü çalıştığı için burada üç adım vardır:

1. Uç düğümde oturum açmak için oluşturulan SSH kullanıcısını kullanma.
2. Uç düğüme daha fazla Linux kullanıcısı ekleme.
3. RStudio topluluk sürümünü oluşturulan kullanıcıyla kullanma.

### <a name="use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Kenar düğümde oturum açmak için oluşturulan SSH kullanıcısını kullanma

Herhangi bir SSH aracını (PuTTY gibi) indirin ve var olan SSH kullanıcısıyla oturum açın. Ardından [SSH kullanarak HDInsight'a (Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md) bölümündeki yönergeleri izleyerek kenar düğümüne erişin. HDInsight kümesi üzerindeki Microsoft R Server için uç düğüm adresi: **clustername-ed-ssh.azurehdinsight.net**


### <a name="add-more-linux-users-to-the-edge-node"></a>Uç düğüme daha fazla Linux kullanıcısı ekleme

Uç düğüme kullanıcı eklemek için şu komutları çalıştırın:

    sudo useradd yournewusername -m
    sudo passwd yourusername

Aşağıdaki öğelerin döndürülmesi gerekir: 

![Sudo komutlarının çıkışı](./media/r-server-get-started/concurrent-users-3.png)

Geçerli Kerberos parolanızı girmeniz istendiğinde, bunu yoksaymak için yalnızca Enter tuşuna basın. `useradd` komutundaki `-m` seçeneği, sistemin kullanıcı için bir ana klasör oluşturacağını belirtir. Bu klasör RStudio topluluk sürümü için gereklidir.


### <a name="use-the-rstudio-community-version-with-the-created-user"></a>RStudio topluluk sürümünü oluşturulan kullanıcıyla kullanma

Oluşturulan kullanıcıyı kullanarak RStudio'da oturum açın:

![RStudio oturum açma sayfası](./media/r-server-get-started/concurrent-users-4.png)

RStudio, kümede oturum açmak için yeni kullanıcıyı (örneğin burada **sshuser6**) kullanmakta olduğunuzu belirtir: 

![RStudio komut çubuğunda yeni kullanıcının konumu](./media/r-server-get-started/concurrent-users-5.png)

Dilerseniz eşzamanlı olarak başka bir tarayıcı penceresinden özgün kimlik bilgilerini (varsayılan olarak **sshuser**) kullanarak da oturum açabilirsiniz.

ScaleR işlevlerini kullanarak bir iş gönderebilirsiniz. Burada, işi çalıştırmaya yönelik örnek komutlar verilmiştir:

    # Set the HDFS (Azure Blob storage) location of example data
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storing data temporarily
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set the directory in bigDataDirRoot to load the data
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (Blob storage) file system
    hdfsFS <- RxHdfsFileSystem()

    # Create an info list for the airline data
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in the local system
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context
    mySparkCluster <- RxSpark()

    # Set the compute context
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary
    summary(modelSpark)


Gönderilen işlerin YARN kullanıcı arabiriminde (UI) farklı kullanıcı adları altında olduğuna dikkat edin:

![YARN UI içinde kullanıcıların listesi](./media/r-server-get-started/concurrent-users-6.png)

Ayrıca, yeni eklenen kullanıcıların Linux sisteminde kök ayrıcalıklara sahip olmadığına da dikkat edin. Ama uzak HDFS dosya sistemi ve Blob depolama alanındaki tüm dosyalar üzerinde ayrı erişime sahiptirler.


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a>R konsolunu kullanma

1. R konsolunu başlatmak için SSH oturumunda aşağıdaki komutu kullanın:  

        R

2. Aşağıdakine benzer bir çıktı görmeniz gerekir:
    
        R version 3.2.2 (2015-08-14) -- "Fire Safety"
        Copyright (C) 2015 The R Foundation for Statistical Computing
        Platform: x86_64-pc-linux-gnu (64-bit)

        R is free software and comes with ABSOLUTELY NO WARRANTY.
        You are welcome to redistribute it under certain conditions.
        Type 'license()' or 'licence()' for distribution details.

        Natural language support but running in an English locale

        R is a collaborative project with many contributors.
        Type 'contributors()' for more information and
        'citation()' on how to cite R or R packages in publications.

        Type 'demo()' for some demos, 'help()' for on-line help, or
        'help.start()' for an HTML browser interface to help.
        Type 'q()' to quit R.

        Microsoft R Server version 8.0: an enhanced distribution of R
        Microsoft packages Copyright (C) 2016 Microsoft Corporation

        Type 'readme()' for release notes.
        >

3. `>` isteminde R kodunu girebilirsiniz. R Server, Hadoop ile kolayca etkileşim kurup dağıtılmış hesaplamaları çalıştırmak için kullanabileceğiniz paketleri içerir. Örneğin, HDInsight kümesinin varsayılan dosya sisteminin kökünü görüntülemek için aşağıdaki komutu kullanın:

        rxHadoopListFiles("/")

4. Ayrıca Blob depolaması stili adresleme de kullanabilirsiniz:

        rxHadoopListFiles("wasb:///")


## <a name="use-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Microsoft R Server veya Microsoft R Client uzak örneğinden HDI üzerinde R Server kullanma

Masaüstü veya dizüstü bilgisayarda çalışan uzak Microsoft R Server veya Microsoft R Client örneğinden HDI Hadoop Spark işlem bağlamına erişim sağlamak mümkündür. Daha fazla bilgi için, [Spark için İşlem Bağlamı Oluşturma](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md) başlığı altındaki "Microsoft R Server'ı Hadoop İstemcisi Olarak Kullanma" bölümüne bakın. Bunu yapmak için dizüstü bilgisayarınızda RxSpark işlem bağlamını tanımlarken şu seçenekleri belirtin: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches ve sshProfileScript. Bir örneği aşağıda verilmiştir:


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a>İşlem bağlamı kullanma

İşlem bağlamını, hesaplamanın uç düğümde yerel olarak yapılmasını veya HDInsight kümesindeki düğümlere dağıtılmasını denetlemek için kullanabilirsiniz.

1. RStudio Server veya R konsolundan (bir SSH oturumunda), varsayılan HDInsight depolama alanına örnek verileri yüklemek için aşağıdaki kodu kullanın:

        # Set the HDFS (Blob storage) location of example data
        bigDataDirRoot <- "/example/data"

        # Create a local folder for storing data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
        remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set the directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Verilerle çalışabilmek için, bazı veri bilgileri oluşturun ve iki veri kaynağı tanımlayın:

        # Define the HDFS (Blob storage) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create an info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # Get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in HDFS
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in the local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # Formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Yerel işlem bağlamını kullanarak, veriler üzerinde lojistik regresyon çalıştırın:

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Aşağıdakilere benzer satırlarla sona eren bir çıktı görmeniz gerekir:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. Spark bağlamını kullanarak aynı lojistik regresyonu çalıştırın. Spark bağlamı, işlemi HDInsight kümesindeki tüm çalışan düğümlerine dağıtır.

        # Define the Spark compute context
        mySparkCluster <- RxSpark()

        # Set the compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > Ayrıca, MapReduce kullanarak hesaplamayı küme düğümleri arasında dağıtabilirsiniz. İşlem bağlamı hakkında daha fazla bilgi için bkz. [HDInsight üzerinde R Server için işlem bağlamı seçenekleri](r-server-compute-contexts.md).


## <a name="distribute-r-code-to-multiple-nodes"></a>R kodunu birden fazla düğüme dağıtma

R Server ile mevcut R kodunu kolayca alabilir ve `rxExec` kullanarak kümedeki birden fazla düğümde çalıştırabilirsiniz. Bu işlev bir parametre taraması veya benzetimleri yaparken yararlıdır. `rxExec` kullanımını gösteren bir kod örneği aşağıda verilmiştir:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Hala Spark veya MapReduce bağlamını kullanıyorsanız bu komut, üzerinde `(Sys.info()["nodename"])` kodunun çalıştırıldığı çalışan düğümleri için `nodename` değerini döndürür. Örneğin, dört düğümlü bir kümede aşağıdakine benzer bir çıkış beklenir:

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="access-data-in-hive-and-parquet"></a>Hive ve Parquet verilerine erişim

R Server 9.1 sürümünde sunulan bir özellik, Spark işlem bağlamındaki ScaleR işlevleri tarafından kullanım için Hive ve Parquet içindeki verilere doğrudan erişime olanak tanır. Bu özellikler RxHiveData ve RxParquetData adlı yeni ScaleR veri kaynağı işlevleri aracılığıyla kullanılabilir. Bu işlevler, verileri ScaleR tarafından analiz edilmek üzere doğrudan Spark DataFrame'e yüklemek amacıyla Spark SQL'in kullanılması yoluyla çalışır.  

Aşağıdaki kod, yeni işlevlerin kullanımına ilişkin bazı örnekler sağlar:

    #Create a Spark compute context
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, clean up, and close the Spark session
    lsObj <- rxSparkListData() #Two data objects are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() #It should show an empty list
    rxSparkDisconnect(myHadoopCluster)


Bu yeni işlevler hakkında daha fazla bilgi için, R Server'da `?RxHivedata` ve `?RxParquetData` komutlarını kullanarak çevrimiçi yardıma bakın.  


## <a name="install-additional-r-packages-on-the-edge-node"></a>Kenar düğümüne ek R paketleri yükleme

Uç düğüme ek R paketleri yüklemek isterseniz, SSH ile uç düğüme bağlı olduğunda doğrudan R konsolu içinden `install.packages()` kullanabilirsiniz. Ancak, kümenin çalışan düğümlerine R paketleri yüklemeniz gerekiyorsa bir betik eylemi kullanmanız gerekir.

Betik eylemleri, HDInsight kümesinde yapılandırma değişiklikleri yapmak veya ek R paketleri gibi ek yazılımlar yüklemek için kullanılan Bash betikleridir. Betik eylemi kullanarak ek paketler yüklemek için aşağıdaki adımları izleyin.

> [!IMPORTANT]
> Betik eylemlerini kullanarak ek R paketleri yükleyebilmeniz için, kümenin oluşturulmuş olması gerekir. Betik R Server'ın tamamen yüklü ve yapılandırılmış olmasına bağlı olduğundan, bu yordamı küme oluşturma sırasında kullanmayın.
>
>

1. [Azure portalı](https://portal.azure.com)’ndan, HDInsight kümesindeki R Server’ınızı seçin.

2. **Ayarlar** bölmesinde **Betik Eylemleri** > **Yenisini Gönder**'i seçin.

   ![Betik eylemleri bölmesinin resmi](./media/r-server-get-started/scriptaction.png)

3. **Betik eylemini gönder** bölmesinde aşağıdaki bilgileri sağlayın:

   * **Ad**: Betiği tanımlayan kolay ad.

   * **Bash betiği URI’si**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Başlık**: Bu öğenin temizlenmiş olması gerekir.

   * **Çalışan**: Bu öğenin temizlenmiş olması gerekir.

   * **Zookeeper**: Bu öğenin temizlenmiş olması gerekir.

   * **Uç düğümler**: Bu öğenin seçilmiş olması gerekir.

   * **Parametreler**: Yüklenecek R paketleri, örneğin `bitops stringr arules`.

   * **Bu betiği kalıcı yap**: Bu öğenin seçilmiş olması gerekir.  

   > [!NOTE]
   > Varsayılan olarak, tüm R paketleri Microsoft R Uygulama Ağı deposunun yüklü R Server sürümüyle tutarlı bir anlık görüntüsünden yüklenir. Paketlerin daha yeni sürümlerini yüklemek istiyorsanız uyumsuzluk riskiyle karşı karşıya kalabilirsiniz. Öte yandan, paket listesinin ilk öğesi olarak `useCRAN` belirtirseniz bu tür bir yükleme mümkündür; örneğin `useCRAN bitops, stringr, arules`.  
   > 
   > Bazı R paketleri için ek Linux sistem kitaplıkları gerekir. Kolaylık olması için, en popüler 100 R paketine gereken bağımlılıklar önceden yüklenmiştir. Yüklediğiniz R paketleri bunların dışında kitaplıklar gerektirirse, burada kullanılan temel betiği indirmeniz ve adımları ekleyerek sistem kitaplıklarını yüklemeniz gerekir. Ardından, değiştirilmiş betiği Azure Depolama hizmetindeki ortak bir blob kapsayıcıya yüklemeniz ve değiştirilmiş betiği kullanarak paketleri yüklemeniz gerekir.
   >
   > Betik eylemleri geliştirme hakkında bilgi için bkz. [Betik eylemi geliştirme](../hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Betik eylemi ekleme](./media/r-server-get-started/submitscriptaction.png)

4. Betiği çalıştırmak için **Oluştur**’u seçin. Betik bittikten sonra R paketleri tüm çalışan düğümlerinde kullanılabilir.


## <a name="configure-microsoft-r-server-operationalization"></a>Microsoft R Server kullanıma hazır hale getirme özelliğini yapılandırma

Veri modellemesi tamamlandığında, tahminlerde bulunmak üzere modelinizi kullanıma hazır hale getirebilirsiniz. Microsoft R Server ile kullanıma hazır hale getirme özelliğini yapılandırmak için aşağıdaki adımları uygulayın:

1. Uç düğüm için `ssh` komutunu kullanın; örneğin: 

       ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

2. Dizini değiştirip ilgili sürüme geçin ve .dll dosyası için `sudo dotnet` komutunu kullanın. 

   Microsoft R Server 9.1 için:

       cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
       sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

   Microsoft R Server 9.0 için:

       cd /usr/lib64/microsoft-deployr/9.0.1
       sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Microsoft R Server ile kullanıma hazır hale getirme özelliğini one-box yapılandırması ile yapılandırmak için aşağıdaki işlemleri yapın:

   a. `Configure R Server for Operationalization` öğesini seçin.

   b. `A. One-box (web + compute nodes)` öğesini seçin.

   c. `admin` kullanıcısının parolasını girin.

   ![One-box kullanıma hazır hale getirme](./media/r-server-get-started/admin-util-one-box-.png)

4. İsteğe bağlı bir adım olarak aşağıdaki şekilde bir tanılama testi gerçekleştirebilirsiniz:

   a. `6. Run diagnostic tests` öğesini seçin.

   b. `A. Test configuration` öğesini seçin.

   c. Kullanıcı adı olarak `admin` girin ve önceki yapılandırma adımındaki parolayı girin.

   d. `Overall Health = pass` öğesini onaylayın.

   e. Yönetici yardımcı programından çıkın.

   f. SSH’den çıkın.

   ![Kullanıma hazır hale getirme için tanılama](./media/r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>Bir Spark işlem bağlamında mrsdeploy ile oluşturulmuş bir web hizmetini kullanmaya çalışırken uzun gecikmeler yaşıyorsanız bazı eksik klasörleri eklemeniz gerekebilir. Spark uygulaması mrsdeoploy işlevleri aracılığıyla bir web hizmetinden çağrıldığında *rserve2* adlı bir kullanıcıya ait oluyor. Bu soruna geçici bir çözüm olarak:

    #Create these required folders for user rserve2 in local and HDFS
    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    #Create a new Spark compute context 
    rxSparkConnect(reset = TRUE)


Bu aşamada kullanıma hazır hale getirme yapılandırması tamamlanmıştır. Şimdi, uç düğümdeki kullanıma hazır hale getirme özelliğine bağlanmak için R Client üzerinde mrsdeploy paketini kullanabilirsiniz. Ardından, onun [uzaktan yürütme](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) ve [web hizmetleri](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette) gibi özelliklerini kullanmaya başlayabilirsiniz. Kümenizin bir sanal ağda ayarlanıp ayarlanmamasına bağlı olarak, SSH oturumu aracılığıyla bağlantı noktası iletme tüneli ayarlamanız gerekebilir.

### <a name="r-server-cluster-on-a-virtual-network"></a>Sanal ağda R Server kümesi

12800 numaralı bağlantı noktası üzerinden uç düğümdeki trafik akışına izin verdiğinizden emin olun. Bu şekilde, kullanıma hazır hale getirme özelliğine bağlanmak için uç düğümü kullanabilirsiniz.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


`remoteLogin()` uç düğüme bağlanamadığı halde uç düğüme bağlanmak için SSH kullanabiliyorsanız, 12800 numaralı bağlantı noktası üzerinde trafiğe izin veren kuralın düzgün ayarlanıp ayarlanmadığını denetlemeniz gerekir. Sorunla karşılaşmaya devam ederseniz, SSH üzerinden bağlantı noktası iletme tüneli oluşturarak bir geçici çözüm uygulayabilirsiniz. Yönergeler için aşağıdaki bölüme bakın.

### <a name="r-server-cluster-not-set-up-on-a-virtual-network"></a>Sanal ağda R Server kümesi ayarlanmamış

Kümeniz sanal üzerinde ayarlanmamışsa veya sanal ağ üzerinden bağlantı kurma sorunları yaşıyorsanız, SSH bağlantı noktası iletme tünelini kullanabilirsiniz:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Ayrıca bunu PuTTY üzerinde de ayarlayabilirsiniz:

![PuTTY SSH bağlantısı](./media/r-server-get-started/putty.png)

SSH oturumunuz etkin duruma geldikten sonra, makinenizin 12800 numaralı bağlantı noktasından giden trafik, SSH aracılığıyla kenar düğümünün 12800 numaralı bağlantı noktasına iletilir. `remoteLogin()` yönteminizde `127.0.0.1:12800` kullandığınızdan emin olun. Bu yöntem, bağlantı noktası iletme yoluyla uç düğümün kullanıma hazır hale getirme özelliğinde oturum açar.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>HDInsight çalışan düğümlerinde Microsoft R Server kullanıma hazır hale getirme işlem düğümlerini ölçeklendirme

### <a name="decommission-the-worker-nodes"></a>Çalışan düğümlerinin yetkisini alma

Microsoft R Server şu anda Yarn üzerinden yönetilmemektedir. Çalışan düğümlerinin yetkisi alınmazsa, Yarn Kaynak Yöneticisi sunucu tarafından kullanılan kaynakları fark edemeyeceği için beklendiği gibi çalışmaz. Bu durumu önlemek için, işlem düğümlerini ölçeklendirmeden önce çalışan düğümlerinin yetkisinin alınması önerilir.

Çalışan düğümlerinin yetkisini almak için:

1. HDI kümesinin Ambari konsolunda oturum açın ve **Konaklar** sekmesini seçin.
2. Yetkisi alınacak çalışan düğümlerini seçin ve sonra da **Eylemler** > **Seçili Konaklar** > **Konaklar** > **Bakım Modunu Aç**'ı seçin. Örneğin, aşağıdaki resimde yetkisini almak üzere wn3 ve wn4 seçilmiştir.  

   ![Bakım modunu açma komutlarının ekran görüntüsü](./media/r-server-get-started/get-started-operationalization.png)  

3. **Eylemler** > **Seçili Konaklar** > **DataNode** > **Yetkisini Al**'ı seçin.
4. **Eylemler** > **Seçili Konaklar** > **NodeManager** > **Yetkisini Al**'ı seçin.
5. **Eylemler** > **Seçili Konaklar** > **DataNode** > **Durdur**'u seçin.
6. **Eylemler** > **Seçili Konaklar** > **NodeManager** > **Durdur**'u seçin.
7. **Eylemler** > **Seçili Konaklar** > **Konaklar** > **Tüm Bileşenleri Durdur**'u seçin.
8. Çalışan düğümlerinin seçimini kaldırın ve baş düğümleri seçin.
9. **Eylemler** > **Seçili Konaklar** > **Konaklar** > **Tüm Bileşenleri Yeniden Başlat**'ı seçin.

### <a name="configure-compute-nodes-on-each-decommissioned-worker-node"></a>Yetkisi alınan her çalışan düğümü üzerinde İşlem düğümünü yapılandırma

1. Yetkisi alınan her çalışan düğümüne bağlanmak için SSH kullanın.
2. `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll` kullanarak bir yönetici yardımcı programı çalıştırın.
3. `Configure R Server for Operationalization` seçeneğini belirtmek için `1` girin.
4. `C. Compute node` seçeneğini belirtmek için `c` girin. Bu adım, çalışan düğümündeki işlem düğümünü yapılandırır.
5. Yönetici yardımcı programından çıkın.

### <a name="add-compute-nodes-details-on-the-web-node"></a>Web düğümüne işlem düğümlerinin ayrıntılarını ekleme

Yetkisi alınan tüm çalışan düğümleri işlem düğümü üzerinde çalışacak şekilde yapılandırıldıktan sonra, kenar düğümüne geri gidin ve yetkisi alınmış çalışan düğümlerinin IP adreslerini Microsoft R Server web düğümünün yapılandırmasına ekleyin:

1. Kenar düğümüne bağlanmak için SSH kullanın.
2. `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` öğesini çalıştırın.
3. `URIs` bölümüne bakın ve çalışan düğümlerinin IP ve bağlantı noktası ayrıntılarını ekleyin.

    ![Uç düğümünün komut satırı](./media/r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [Erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).


## <a name="next-steps"></a>Sonraki adımlar

Şimdi, R Server içeren bir HDInsight kümesinin nasıl oluşturulduğunu anlamanız gerekir. Ayrıca, SSH oturumunda R konsolunu kullanmanın temellerini de anlamalısınız. Aşağıdaki konularda HDInsight üzerinde R Server yönetimi ve çalışması için diğer yöntemler açıklanmaktadır:

* [HDInsight üzerinde R Server için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde R Server için Azure Depolama seçenekleri](r-server-storage.md)
