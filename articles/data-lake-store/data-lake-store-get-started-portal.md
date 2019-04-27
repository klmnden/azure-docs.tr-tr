---
title: Azure portalı kullanarak Azure Data Lake depolama Gen1 ile çalışmaya başlama | Microsoft Docs
description: Bir Azure Data Lake depolama Gen1 hesabı oluşturmak ve Data Lake depolama Gen1 hesabında temel işlemleri gerçekleştirmek için Azure portalını kullanın.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: e021d8c056028c03ac71d2a27c9128272f374da6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60878020"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-the-azure-portal"></a>Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI](data-lake-store-get-started-cli-2.0.md)
>
> 

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure portalında bir Azure Data Lake depolama Gen1 hesabı oluşturmak ve klasör oluşturma gibi karşıya yükleyin ve veri dosyalarını indirir temel işlemleri gerçekleştirmek için hesabınızı silme nasıl kullanılacağını öğrenin. Daha fazla bilgi için [genel bakış, Azure Data Lake depolama Gen1](data-lake-store-overview.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabı oluşturun

1. Yeni [Azure Portal](https://portal.azure.com)'da oturum açın.
2. Tıklayın **kaynak Oluştur > depolama > Data Lake depolama Gen1**.
3. İçinde **yeni Data Lake depolama Gen1** dikey penceresinde, aşağıdaki ekran görüntüsünde gösterilen değerleri sağlayın:
   
    ![Yeni bir Data Lake depolama Gen1 hesabı oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "yeni bir Data Lake depolama Gen1 hesabı oluşturma")
   
   * **Ad**. Data Lake depolama Gen1 hesabı için benzersiz bir ad girin.
   * **Abonelik**. Altında yeni bir Data Lake depolama Gen1 hesabını oluşturmak istediğiniz aboneliği seçin.
   * **Kaynak Grubu**. Mevcut bir kaynak grubunu seçin veya yeni grup oluşturmak için **Yeni oluştur** seçeneğini belirleyin. Kaynak grubu, bir uygulama için ilgili kaynakları bir arada tutan bir kapsayıcıdır. Daha fazla bilgi için bkz. [Azure'da Kaynak Grupları](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Konum**: Data Lake depolama Gen1 hesabı oluşturmak istediğiniz bir konum seçin.
   * **Şifreleme Ayarları**. Üç seçenek vardır:
     
     * **Şifrelemeyi etkinleştirme**.
     * **Data Lake depolama Gen1 tarafından yönetilen anahtarlar kullan**şifreleme anahtarlarınızı yönetmek için Data Lake depolama Gen1 istiyorsanız.
     * **Kendi Anahtar Kasanızdaki anahtarları kullanın**. Mevcut bir Azure Key Vault’u seçin veya yeni bir Key Vault oluşturun. Key vault'taki anahtarları kullanmak için Data Lake depolama Gen1 hesabı, Azure Key Vault'a erişmesini sağlayacak izinleri atamanız gerekir. Yönergeler için bkz. [Azure Key Vault'a izinleri atama](#assign-permissions-to-azure-key-vault).
       
        ![Data Lake depolama Gen1 şifreleme](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Data Lake depolama Gen1 şifreleme")
       
        **Şifreleme Ayarları** dikey penceresinde **Tamam**’a tıklayın.

        Daha fazla bilgi için [Azure Data Lake depolama Gen1 veri şifreleme](./data-lake-store-encryption.md).

4. **Oluştur**’a tıklayın. Hesabı panoya sabitlemeyi tercih ediyorsanız tekrar panoya yönlendirilirsiniz ve Data Lake depolama Gen1 hesap hazırlama işleminizin ilerleme durumunu görebilirsiniz. Data Lake depolama Gen1 hesap sağlandıktan sonra hesap dikey penceresi görünür.

## <a name="assign-permissions-to-azure-key-vault"></a>Azure Key Vault’a izin atama
Data Lake depolama Gen1 hesabında şifreleme yapılandırmak için bir Azure anahtar Kasası'ndaki anahtarları kullandıysanız Data Lake depolama Gen1 hesabı ve Azure anahtar kasası hesabı arasında erişim yapılandırmalısınız. Bunu yapmak için aşağıdaki adımları gerçekleştirin.

1. Azure anahtar Kasası'ndaki anahtarları kullandıysanız Data Lake depolama Gen1 hesabı için dikey pencerenin en üstünde bir uyarı görüntüler. Uyarıya tıklayarak **Şifreleme** sayfasını açın.
   
    ![Data Lake depolama Gen1 şifreleme](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Data Lake depolama Gen1 şifreleme")
2. Bu dikey pencere, erişimi yapılandırmak için iki seçenek gösterir.

    ![Data Lake depolama Gen1 şifreleme](./media/data-lake-store-get-started-portal/adls-encryption-4.png "Data Lake depolama Gen1 şifreleme")
   
   * Erişim yapılandırmak için ilk seçenekte **İzin Ver**'e tıklayın. İlk seçenek, yalnızca Data Lake depolama Gen1 hesabı oluşturan kullanıcı Azure Key Vault için bir yönetici ayrıca olduğunda etkinleştirilir.
   * Diğer seçenek, dikey pencerede görüntülenen PowerShell cmdlet’ini çalıştırmak içindir. Azure Anahtar Kasası’nın sahibi olmanız ya da Azure Anahtar Kasası’nda izin verme yetkisine sahip olmanız gerekir. Cmdlet’i çalıştırdıktan sonra, erişimi yapılandırmak için dikey pencereye dönün ve **Etkinleştir**’e tıklayın.

> [!NOTE]
> Ayrıca, Azure Resource Manager şablonlarını kullanarak bir Data Lake depolama Gen1 hesabı oluşturabilirsiniz. Bu şablonlara [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/?term=data+lake+store)’ndan erişebilirsiniz:
> - Veri şifreleme olmadan: [Azure Data Lake depolama Gen1 hesabını veri şifrelemesi olmadan dağıtma](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/).
> - Data Lake depolama Gen1 kullanarak veri şifrelemesi ile: [Şifreleme (Data Lake) ile Data Lake depolama Gen1 hesabı dağıtma](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
> - Azure Key Vault kullanarak veri şifrelemesi ile: [Şifreleme (Key Vault) ile Data Lake depolama Gen1 hesabı dağıtma](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).
> 
> 



## <a name="createfolder"></a>Bir Data Lake depolama Gen1 hesabında klasör oluşturma
Veri depolamak ve yönetmek için Data Lake depolama Gen1 hesabınızın altında klasör oluşturabilirsiniz.

1. Oluşturduğunuz Data Lake depolama Gen1 hesabını açın. Sol bölmeden **Tüm kaynaklar**'a tıklayın ve ardından Tüm kaynaklar dikey penceresinden, altında klasör oluşturmak istediğiniz hesabın adına tıklayın. Hesabı başlangıç panosuna sabitlediyseniz bu hesap kutucuğuna tıklayın.
2. Data Lake depolama Gen1 hesabı dikey penceresinde **Veri Gezgini**.
   
    ![Bir Data Lake depolama Gen1 hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "bir Data Lake depolama Gen1 hesabında klasör oluşturma")
3. Veri Gezgini dikey penceresinde, **Yeni Klasör**'e tıklayın, yeni klasör için bir ad girin ve ardından **Tamam**'a tıklayın.
   
    ![Bir Data Lake depolama Gen1 hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "bir Data Lake depolama Gen1 hesabında klasör oluşturma")
   
    Yeni oluşturulan klasör, **Veri Gezgini** dikey penceresinde listelenir. Herhangi bir düzeye kadar iç içe geçmiş klasörler oluşturabilirsiniz.
   
    ![Bir Data Lake hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "bir Data Lake hesabında klasör oluşturma")

## <a name="uploaddata"></a>Bir Data Lake depolama Gen1 hesabına veri yükleme
Bir Data Lake depolama Gen1 hesabınıza doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir klasöre verilerinizi karşıya yükleyebilirsiniz. 

1. **Veri Gezgini** dikey penceresinde **Yükle**'ye tıklayın. 
2. **Dosyaları karşıya yükleme** dikey penceresinde yüklemek istediğiniz dosyaları bulun ve **Seçili dosyaları ekle**'ye tıklayın. Yüklemek için birden fazla dosya da seçebilirsiniz.

    ![Veri yükleme](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

## <a name="properties"></a>Depolanan verilerde kullanılabilen eylemler
Dosyanın yanındaki üç nokta simgesine tıklayın ve açılan menüden verilerle ilgili gerçekleştirmek istediğiniz eyleme tıklayın.

![Verilere yönelik özellikler](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data") 

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure Active Directory ve erişim denetimi (ACL'ler) kullanarak Data Lake depolama Gen1 hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Bunu yapmak yönergeler için bkz: [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](data-lake-store-secure-data.md).

## <a name="delete-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabını Sil
Bir Data Lake depolama Gen1 hesabı, Data Lake depolama Gen1 dikey penceresinden silmek için tıklayın **Sil**. Eylemi onaylamak için silmek istediğiniz hesabın adını girmeniz istenir. Hesabın adını girin ve ardından **Sil**'e tıklayın.

![Data Lake depolama Gen1 hesabını silme](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Data Lake hesabını silme")

## <a name="next-steps"></a>Sonraki adımlar
* [Büyük veri gereksinimleri için Azure Data Lake depolama Gen1 kullanın](data-lake-store-data-scenarios.md) 
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)

