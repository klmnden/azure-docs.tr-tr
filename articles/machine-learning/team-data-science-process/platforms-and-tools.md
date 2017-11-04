---
title: "Takım projeleri - Azure platformlar ve veri bilimi için araçları | Microsoft Docs"
description: "Maddeler halinde listelemektedir ve takım veri bilimi işlemi Standartlaştırma işletmelere kullanılabilir verileri ve çözümlemeler kaynaklar açıklanır."
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
ms.date: 09/04/2017
ms.author: bradsev;
ms.openlocfilehash: 3ec2eaaf4e8d54e7b1ea3d272c47eac96451f317
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="platforms-and-tools-for-data-science-team-projects"></a>Platformlar ve veri bilimi takım projeleri için Araçlar

Microsoft Bulut veya şirket içi platformlar için veri ve Analiz Hizmetleri ve kaynakları tam bir yelpazesini sağlar. Veri bilimi projelerinizi yürütülmesi verimli ve ölçeklenebilir hale getirmek için dağıtılabilir. Sürüm yönergeler veri bilimi projeleri bir trackable uygulama ekipleri için denetimli ve işbirliği yolu tarafından sağlanır [takım veri bilimi işlemi](overview.md) (TDSP).  Personel rolleri ve bir veri bilimi takım bu işlemi Standartlaştırma tarafından işlenen ilişkilendirilen görevlerinin ana hattı için bkz: [takım veri bilimi işlemi rolleri ve görevleri](roles-tasks.md).

TDSP kullanarak veri bilimi ekipleri için kullanılabilir veri ve Analiz Hizmetleri içerir:

- Veri bilimi sanal makineleri (hem Windows hem de Linux CentOS)
- Hdınsight Spark kümeleri
- SQL Veri Ambarı
- Azure Data Lake
- Hdınsight Hive kümeleri
- Azure dosya depolama
- SQL Server 2016 R Hizmetleri

Bu belgede, biz kısaca kaynakları tanımlayan ve öğreticiler ve TDSP takımlar yayımladığınız izlenecek yollar için bağlantılar sağlar. Bunlar bunları adım adım kullanın ve bunları akıllı uygulamalarınızı oluşturmak için kullanmaya başlama öğrenmenize yardımcı olabilir. Bu kaynaklar hakkında daha fazla bilgi ürün sayfalarında kullanılabilir. 

## <a name="data-science-virtual-machine-dsvm"></a>Veri bilimi sanal makine (DSVM)

Veri bilimi sanal makine hem Windows hem de Linux Microsoft tarafından sunulan veri bilimi modelleme ve geliştirme etkinlikleri için popüler araçları içerir. Araçları gibi içerir:

- Microsoft R Server Geliştirici sürümü 
- Anaconda Python dağıtımı
- Python ve R Jupyter Not Defterleri 
- Python ve Windows'da R araçları ile Visual Studio Community Edition / Tutulma Linux'ta
- Windows için Power BI desktop
- SQL Server 2016 Geliştirici sürümü Windows / Linux üzerinde Postgres

Ayrıca içerir **ML ve AI Araçları** CNTK (açık kaynak derin öğrenme toolkit Microsoft), xgboost, mxnet ve Vowpal Wabbit gibi.

Şu anda DSVM kullanılabilir **Windows** ve **Linux CentOS** işletim sistemleri. Üzerinde yürütmek için planlama veri bilimi projeleri gereksinimlerine göre DSVM (CPU çekirdekleri sayısı) ve bellek miktarını boyutunu seçin. 

DSVM Windows sürümü ile ilgili daha fazla bilgi için bkz: [Microsoft Veri bilimi sanal makine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) Azure Market'te. DSVM Linux sürümü için bkz: [Linux veri bilimi sanal makine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

Bazı genel veri bilimi görevler üzerinde DSVM verimli bir şekilde çalıştırma hakkında bilgi edinmek için [on nokta veri bilimi sanal makine yapabilirsiniz](../data-science-virtual-machine/vm-do-ten-things.md)


## <a name="azure-hdinsight-spark-clusters"></a>Azure Hdınsight Spark kümeleri

Apache Spark büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen framework işleme bir açık kaynak paralel ' dir. Spark işleme altyapısı hızı, kullanımı kolay, gelişmiş analizler için yerleşik olarak bulunur. Spark'ın bellek içi hesaplama özellikleri grafik hesaplamalar ve machine learning'de yinelemeli algoritmalar için iyi bir seçim yapın. Azure'da depolanan mevcut verilerinizi kolayca Spark kullanarak işleyebileceğiniz şekilde Spark ayrıca Azure Blob storage (WASB) ile uyumludur.

HDInsight’ta Spark kümesi oluşturduğunuzda, Spark yüklenmiş ve yapılandırılmış olarak Azure işlem kaynakları oluşturursunuz. Hdınsight'ta Spark kümesi oluşturmak için yaklaşık 10 dakika sürer. Azure Blob depolama alanına işlenmesi için verileri depolar. Bir küme ile Azure Blob Storage kullanma hakkında daha fazla bilgi için bkz: [HDFS uyumlu Azure Blob storage kullanma hdınsight'ta Hadoop ile](../../hdinsight/hdinsight-hadoop-use-blob-storage.md).

TDSP takım Microsoft Azure Hdınsight Spark kümeleri veri bilimi çözümleri oluşturmak için nasıl kullanılacağı hakkında iki uçtan uca izlenecek yollar yayımlanan Python ve diğer Scala kullanarak bir tane. Azure Hdınsight hakkında daha fazla bilgi için **Spark kümeleri**, bkz: [genel bakış: Hdınsight Linux üzerinde Apache Spark](../../hdinsight/spark/apache-spark-overview.md). Bir veri bilimi çözümünü kullanarak oluşturmayı öğrenmek için **Python** bir Azure Hdınsight Spark kümesinde bkz [genel bakış, verileri Azure Hdınsight'ta Spark kullanmanın Bilim](spark-overview.md). Bir veri bilimi çözümünü kullanarak oluşturmayı öğrenmek için **Scala** bir Azure Hdınsight Spark kümesinde bkz [Scala ve Spark Azure üzerinde kullanarak veri bilimi](scala-walkthrough.md). 


##  <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL Data Warehouse işlem kaynaklarını kolayca ve saniye cinsinden aşırı sağlama veya aşırı ödeme ölçeklendirmenizi sağlar. Ayrıca, bulut maliyetlerinizi daha iyi yönetebilmek için özgürlüğünü işlem kaynaklarının kullanımını duraklatmak için benzersiz seçeneği de sunar. Ölçeklenebilir işlem kaynaklarını dağıtma yeteneği tüm verileriniz Azure SQL Data Warehouse'a getirmek mümkün kılar. Depolama maliyetleri en az ve işlem, yalnızca bölümlerini çözümlemek istediğiniz veri kümeleri üzerinde çalışabilir. 

Azure SQL Data Warehouse hakkında daha fazla bilgi için bkz: [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse) Web sitesi. SQL veri ambarı ile uçtan uca Gelişmiş analiz çözümleri oluşturmayı öğrenmek için bkz: [eylem takım veri bilimi işlem: SQL Data Warehouse kullanarak](sqldw-walkthrough.md).


## <a name="azure-data-lake"></a>Azure Data Lake

Bir kuruluş çapında deposu herhangi bir resmi gereksinimleri veya uygulanan şema önce tek bir konumda toplanan verileri her tür olarak Azure data lake olur. Bu esneklik, her tür, boyut veya yapısı bağımsız olarak bir veri gölü veya ne kadar hızlı alınan tutulması için veri sağlar. Gelişmiş analizler bulmak için bu verileri Göller düzenleri veya kuruluşlar sonra Hadoop kullanabilirsiniz. Ayrıca, veri Göller verileri rehberlik ve veri ambarına taşımadan önce daha düşük maliyetli veri hazırlığı için depo olarak hizmet verebilir.

Azure Data Lake hakkında daha fazla bilgi için bkz: [Introducing Azure Data Lake](https://azure.microsoft.com/blog/introducing-azure-data-lake/). Azure Data Lake ölçeklenebilir uçtan uca veri bilimi çözümüyle oluşturmayı öğrenmek için bkz: [Azure Data Lake içinde ölçeklenebilir veri bilimi: bir uçtan uca gözden geçirme](data-lake-walkthrough.md)


## <a name="azure-hdinsight-hive-hadoop-clusters"></a>Azure Hdınsight Hive (Hadoop) kümeleri

Apache Hive bir veri ambarı veri özetleme, sorgulama ve HiveQL, SQL için benzer bir sorgu dili kullanarak veri analizini sağlar Hadoop sistemidir. Hive, etkileşimli olarak verilerinizi keşfedin veya yeniden kullanılabilir toplu işleme işleri oluşturmak için kullanılabilir.

Hive, büyük ölçüde yapılandırılmamış veriler üzerinde proje yapısını olanak tanır. Yapısı tanımladıktan sonra Hive kullanın veya hatta biliyorsanız, Java ya da MapReduce gerek kalmadan bu verileri Hadoop kümesindeki sorgulamak için kullanabilirsiniz. HiveQL (Hive sorgusu dili) T-SQL benzer deyimleri sorgularıyla yazmanızı sağlar.

Veri bilimcilerine için Hive Python User-Defined işlevler (UDF'ler) kayıtlarını işlemek için Hive sorguları çalıştırabilirsiniz. Bu özelliği, veri analizi Hive sorguları yeteneğini önemli ölçüde genişletir. Özellikle, bunlar çoğunlukla aşina dillerde ölçeklenebilir özellik Mühendisliği yürütmek veri bilimcilerine sağlar: SQL benzeri HiveQL ve Python. 

Azure Hdınsight Hive kümeleri hakkında daha fazla bilgi için bkz: [Hive ve HiveQL hdınsight'ta Hadoop ile](../../hdinsight/hadoop/hdinsight-use-hive.md). Azure Hdınsight Hive kümeleri ile ölçeklenebilir uçtan uca veri bilimi çözüm oluşturmayı öğrenmek için bkz: [takım veri bilimi işleminde eylemi: Hdınsight Hadoop kümeleri kullanarak](hive-walkthrough.md).


## <a name="azure-file-storage"></a>Azure dosya depolama 

Azure File Storage standart sunucu ileti bloğu (SMB) protokolü kullanılarak bulutta dosya paylaşımları sunan bir hizmettir. SMB 2.1 ve SMB 3.0 desteklenir. Azure File Storage, Azure’a dosya paylaşımı kullanan eski uygulamaları maliyetli yeniden yazdırmaya ihtiyaç duymadan ve hızla taşıyabilmenizi sağlar. Azure Virtual Machines’de, Cloud Services’da veya şirket içi istemcilerde çalışan uygulamalar, bir masaüstü uygulamanın tipik SMB paylaşımı bağladığı gibi buluta bir dosya paylaşımı bağlayabilir. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

Proje verilerini proje ekip üyelerinizin ile paylaşmak için yer olarak bir Azure dosya deposu oluşturma olanağı veri bilimi projeleri için özellikle yararlıdır. Bunların her birini daha sonra verileri aynı kopyasını Azure dosya depolama alanında erişebilir. Proje yürütülmesi sırasında oluşturulan özellik kümeleri paylaşmak için bu dosya depolama de kullanabilirsiniz. Bir istemci katılım projesiyse istemcileriniz proje verilerini ve özellikleri ile paylaşmak için kendi Azure aboneliği altında bir Azure dosya depolama oluşturabilirsiniz. Bu şekilde, istemci proje veri varlıklarının üzerinde tam denetime sahiptir. Azure File Storage hakkında daha fazla bilgi için bkz: [Windows Azure File storage ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files) ve [Azure File Storage'ı Linux ile kullanma konusunda](../../storage/files/storage-how-to-use-files-linux.md).


## <a name="sql-server-2016-r-services"></a>SQL Server 2016 R Hizmetleri

R hizmetler (veritabanı-), geliştirmek ve yeni bilgiler açığa akıllı uygulamaları dağıtmak için bir platform sağlar. R topluluk tarafından sağlanan birçok paketleri de dahil olmak üzere zengin ve güçlü R dil modelleri oluşturun ve SQL Server verilerinizden tahminleri oluşturmak için kullanabilirsiniz. R Hizmetleri (veritabanı-) R dil SQL Server ile tümleştirmek için analiz verileri taşıma ile ilişkili güvenlik riskleri ve maliyetlerini ortadan kaldırır verileri yakın tutulur.

R Hizmetleri (veritabanı-) açık kaynak R dili ile SQL Server Araçlar ve teknolojiler kapsamlı bir kümesini destekler. Bunlar, üstün performans, güvenlik, güvenilirlik ve yönetilebilirlik sağlar. Kolay ve tanıdık araçlar kullanarak R çözümler dağıtabilirsiniz. Üretim uygulamalarınızı R çalışma zamanı çağırın ve Öngörüler ve Transact-SQL kullanan görseller alın. ScaleR kitaplıkları R çözümlerinizi performansı ve ölçeği artırmak için de kullanır. Daha fazla bilgi için bkz: [SQL Server R Hizmetleri](https://msdn.microsoft.com/library/mt604845.aspx)

Microsoft TDSP ekibinden SQL Server 2016 R Hizmetleri'nde veri bilimi çözümleri oluşturmak üzere nasıl gösteren iki uçtan uca izlenecek yollar yayımladıktan: R programcıları, diğeri SQL geliştiriciler için. İçin **R programcıları**, bkz: [veri bilimi uçtan uca kılavuz](https://msdn.microsoft.com/library/mt612857.aspx). İçin **SQL geliştiriciler**, bkz: [SQL geliştiriciler (öğretici) için veritabanı Advanced Analytics](https://msdn.microsoft.com/library/mt683480.aspx).


## <a name="appendix"></a>Ek: veri bilimi projelerini ayarlamak için Araçlar

### <a name="install-git-credential-manager-on-windows"></a>Windows üzerinde Git kimlik bilgisi Yöneticisi'ni yükleyin

TDSP aşağıdaki varsa **Windows**, yüklemenize gerek **Git kimlik bilgisi Yöneticisi (GCM)** Git depoları ile iletişim kurmak için. GCM yüklemek için önce yüklemeniz gerekir **Chocolaty**. Chocolaty ve GCM yüklemek için Windows PowerShell aşağıdaki komutları çalıştırın. bir **yönetici**:  

    iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
    choco install git-credential-manager-for-windows -y
    

### <a name="install-git-on-linux-centos-machines"></a>Linux (CentOS) makinelerde Git'i yükleyin

Linux (CentOS) makinelerde Git yüklemek için aşağıdaki bash komutu çalıştırın:

    sudo yum install git


### <a name="generate-public-ssh-key-on-linux-centos-machines"></a>Linux (CentOS) makinelerde genel SSH anahtarı oluştur

Git komutları çalıştırmak için Linux (CentOS) makineler kullanıyorsanız, böylece bu makine VSTS sunucu tarafından tanınmıyor, genel SSH anahtarı makinenizin VSTS sunucunuza eklemeniz gerekir. İlk olarak, ortak bir SSH anahtarı oluşturmak ve VSTS güvenlik ayarı sayfanıza SSH ortak anahtarları için anahtar eklemeniz gerekir. 

- SSH anahtarı oluşturmak için aşağıdaki iki komutu çalıştırın: 

        ssh-keygen
        cat .ssh/id_rsa.pub

![](./media/platforms-and-tools/resources-1-generate_ssh.png)

- Bütün ssh kopyası anahtar da dahil olmak üzere *ssh-rsa*. 
- VSTS sunucunuza oturum açın. 
- Tıklatın **< adınız\>**  tıklatın ve sayfanın sağ üst köşedeki adresindeki **güvenlik**. 
    
    ![](./media/platforms-and-tools/resources-2-user-setting.png)

- Tıklatın **SSH ortak anahtarları**, tıklatıp **+ Ekle**. 

    ![](./media/platforms-and-tools/resources-3-add-ssh.png)

- Yapıştır ssh anahtarı yalnızca kopyalanan metin kutusuna ve kaydedin.


## <a name="next-steps"></a>Sonraki adımlar

Tam işlem için tüm adımları gösteren uçtan uca talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) konu. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 

Azure Machine Learning Studio'da kullanmak adımları takım veri bilimi işlemde çalışan örnekler için bkz: [ile Azure ML](http://aka.ms/datascienceprocess) öğrenme yolu.