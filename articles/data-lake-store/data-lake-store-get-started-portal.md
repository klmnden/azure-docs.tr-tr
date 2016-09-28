<properties 
   pageTitle="Data Lake Store ile çalışmaya başlama | Azure" 
   description="Bir Data Lake Store hesabı oluşturmak ve Data Lake Store'da temel işlemleri gerçekleştirmek için portalı kullanma" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>


# Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Azure Data Lake Store hesabı oluşturmak ve klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure Portal'ın nasıl kullanılacağını öğrenin. Data Lake Store hakkında daha fazla bilgi için bkz. [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md).

## Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## Videolarla daha hızlı mı öğreniyorsunuz?

Data Lake Store ile çalışmaya başlamak için aşağıdaki videoları izleyin.

* [Data Lake Store hesabı oluşturma](https://mix.office.com/watch/1k1cycy4l4gen)
* [Veri Gezgini'ni kullanarak Data Lake Store'da verileri yönetme](https://mix.office.com/watch/icletrxrh6pc)

## Azure Data Lake Store hesabı oluşturma

1. Yeni [Azure Portal](https://portal.azure.com)'da oturum açın.

2. **YENİ**'ye, **Veri + Depolama**'ya ve ardından **Azure Data Lake Store**'a tıklayın. **Azure Data Lake Store** dikey penceresindeki bilgileri okuyun ve ardından dikey pencerenin sol alt köşesindeki **Oluştur** seçeneğine tıklayın.

3. **Yeni Data Lake Store** dikey penceresinde, aşağıdaki ekran görüntüsünde gösterilen değerleri sağlayın:

    ![Yeni bir Azure Data Lake Store hesabı oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Create a new Azure Data Lake account")

    - **Abonelik**. Altında yeni bir Data Lake Store hesabı oluşturmak istediğiniz aboneliği seçin.
    - **Kaynak Grubu**. Var olan bir kaynak grubunu seçin veya bir tane oluşturmak için **Kaynak grubu oluştur**'a tıklayın. Kaynak grubu, bir uygulama için ilgili kaynakları bir arada tutan bir kapsayıcıdır. Daha fazla bilgi için bkz. [Azure'da Kaynak Grupları](resource-group-overview.md#resource-groups).
    - **Konum**: Data Lake Store hesabını oluşturmak istediğiniz konumu seçin.

4. Data Lake Store hesabına Başlangıç Panosu'ndan erişilebilmesini istiyorsanız **Başlangıç Panosuna Sabitlemek** öğesini seçin.

5. **Oluştur**'a tıklayın. Hesabı başlangıç panosuna sabitlemeyi tercih ediyorsanız tekrar başlangıç panosuna yönlendirilir ve Data Lake Store hesap hazırlama işleminizin ilerleme durumunu görebilirsiniz. Data Lake Store hesabı sağlandıktan sonra, hesap dikey penceresi görünür.

6. Data Lake Store hesabınız hakkında, parçası olduğu kaynak grubu, konum vb. bilgileri görmek için **Temel Bileşenler** açılan menüsünü genişletin. Data Lake Store ile ilgili diğer kaynakların bağlantılarını görmek için **Hızlı Başlangıç** seçeneğine tıklayın.

    ![Azure Data Lake Store hesabınız](./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Your Azure Data Lake account")

## <a name="createfolder"></a>Azure Data Lake Store hesabında klasör oluşturma

Veri depolamak ve yönetmek için Data Lake Store hesabınızın altında klasör oluşturabilirsiniz.

1. Yeni oluşturduğunuz Data Lake Store hesabını açın. Sol bölmeden **Gözat**'a tıklayın, **Data Lake Store**'a tıklayın ve Data Lake Store dikey penceresinden, altında klasör oluşturmak istediğiniz hesabın adına tıklayın. Hesabı başlangıç panosuna sabitlediyseniz bu hesap kutucuğuna tıklayın.

2. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.

    ![Data Lake Store hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Create folders in Data Lake Store account")

3. Data Lake Store hesabı dikey pencerenizde, **Yeni Klasör**'e tıklayın, yeni klasör için bir ad girin ve ardından **Tamam**'a tıklayın.
    
    ![Data Lake Store hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Create folders in Data Lake Store account")
    
    Yeni oluşturulan klasör, **Veri Gezgini** dikey penceresinde listelenir. Herhangi bir düzeye kadar iç içe geçmiş klasörler oluşturabilirsiniz.

    ![Data Lake hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Create folders in Data Lake account")


## <a name="uploaddata"></a>Azure Data Lake Store hesabına veri yükleme

Verilerinizi Azure Data Lake Store hesabınıza doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir klasöre yüklenecek şekilde yükleyebilirsiniz. Aşağıdaki ekran görüntüsünü kullanarak, **Veri Gezgini** dikey penceresinden bir dosyayı bir alt klasöre yüklemek için aşağıdaki adımları izleyin. Bu ekran yakalama görüntüsünde, dosya içerik haritalarında gösterilen bir alt klasöre yüklenmektedir (kırmızı kutu içinde işaretlenmiştir).

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

![Karşıya veri yükleme](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")


## <a name="properties"></a>Depolanan verilere yönelik kullanılabilir özellikler ve eylemler

**Özellikler** dikey penceresini açmak için yeni eklenen dosyaya tıklayın. Dosyayla ilişkili özellikler ve dosya üzerinde gerçekleştirebileceğiniz eylemler bu dikey pencerede sunulur. Ayrıca tam yolu, aşağıdaki ekran görüntüsünde vurgulanmış olan Azure Data Lake Store hesabınızdaki dosyaya kopyalayabilirsiniz.

![Verilere yönelik özellikler](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data")

* Dosyanın önizlemesini görmek için, doğrudan tarayıcıdan **Önizleme**'ye tıklayın. Önizleme biçimini de belirtebilirsiniz. **Önizleme**'ye tıklayın,**Dosya Önizleme** dikey penceresinde **Biçim**'e tıklayın ve **Dosya Önizleme Biçimi** dikey penceresinde, görüntülenecek satırların sayısı, kullanılacak kodlama, kullanılacak sınırlayıcı gibi seçenekleri belirtin.

  ![Dosya önizleme biçimi](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "File preview format")

* Dosyayı bilgisayarınıza indirmek için **İndir**'e tıklayın.

* Dosyayı yeniden adlandırmak için **Dosyayı yeniden adlandır**'a tıklayın.

* Dosyayı silmek için **Dosyayı sil**'e tıklayın.


## Verilerinizin güvenliğini sağlama

Azure Active Directory'yi ve erişim denetimini (ACL'ler) kullanarak Azure Data Lake Store hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Bunun nasıl yapılacağına ilişkin yönergeler için bkz. [Azure Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md).


## Azure Data Lake Store hesabını silme

Bir Azure Data Lake Store hesabını silmek için, Data Lake Store dikey pencerenizden **Sil**'e tıklayın. Eylemi onaylamak için silmek istediğiniz hesabın adını girmeniz istenir. Hesabın adını girin ve ardından **Sil**'e tıklayın.

![Data Lake hesabını silme](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete Data Lake account")


## Sonraki adımlar

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Data Lake Store tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md)



<!--HONumber=Sep16_HO3-->


