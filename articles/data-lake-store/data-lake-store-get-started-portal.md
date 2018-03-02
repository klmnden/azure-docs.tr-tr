---
title: "Azure portalı kullanarak Data Lake Store ile çalışmaya başlama| Microsoft Docs"
description: "Bir Data Lake Store hesabı oluşturmak ve Data Lake Store'da temel işlemleri gerçekleştirmek için Azure portalı kullanma"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: fea324d0-ad1a-4150-81f0-8682ddb4591c
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: c5b0f5250a08915e987a1eb5229f2c4648e660fd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Azure portalı kullanarak Azure Data Lake Store ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
>
> 

Azure Data Lake Store hesabı oluşturmak ve klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure Portal'ın nasıl kullanılacağını öğrenin. Daha fazla bilgi için bkz. [Azure Data Lake Store’a Genel Bakış](data-lake-store-overview.md).

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store hesabı oluşturma

1. Yeni [Azure Portal](https://portal.azure.com)'da oturum açın.
2. **Kaynak oluştur**'a, **Veri ve Depolama**'ya ve ardından **Azure Data Lake Store**'a tıklayın. **Azure Data Lake Store** dikey penceresindeki bilgileri okuyun ve ardından dikey pencerenin sol alt köşesindeki **Oluştur** seçeneğine tıklayın.
3. **Yeni Data Lake Store** dikey penceresinde, aşağıdaki ekran görüntüsünde gösterilen değerleri sağlayın:
   
    ![Yeni bir Azure Data Lake Store hesabı oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Create a new Azure Data Lake account")
   
   * **Ad**. Data Lake Store hesabı için benzersiz bir ad girin.
   * **Abonelik**. Altında yeni bir Data Lake Store hesabı oluşturmak istediğiniz aboneliği seçin.
   * **Kaynak Grubu**. Mevcut bir kaynak grubunu seçin veya yeni grup oluşturmak için **Yeni oluştur** seçeneğini belirleyin. Kaynak grubu, bir uygulama için ilgili kaynakları bir arada tutan bir kapsayıcıdır. Daha fazla bilgi için bkz. [Azure'da Kaynak Grupları](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Konum**: Data Lake Store hesabını oluşturmak istediğiniz konumu seçin.
   * **Şifreleme Ayarları**. Üç seçenek vardır:
     
     * **Şifrelemeyi etkinleştirme**.
     * **Azure Data Lake tarafından yönetilen anahtarları kullanma**.  Şifreleme anahtarlarınızı yönetmek için Azure Data Lake Store kullanmak istiyorsanız.
     * **Kendi Anahtar Kasanızdaki anahtarları kullanın**. Mevcut bir Azure Key Vault’u seçin veya yeni bir Key Vault oluşturun. Azure Key Vault’taki anahtarları kullanmak için Azure Data Lake Store hesabının Azure Key Vault’a erişmesini sağlayacak izinleri atamanız gerekir. Yönergeler için bkz. [Azure Key Vault'a izinleri atama](#assign-permissions-to-azure-key-vault).
       
        ![Data Lake Store şifrelemesi](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Data Lake Store encryption")
       
        **Şifreleme Ayarları** dikey penceresinde **Tamam**’a tıklayın.

        Daha fazla bilgi için bkz. [Azure Data Lake Store'da verileri şifreleme](./data-lake-store-encryption.md).

4. **Oluştur**’a tıklayın. Hesabı panoya sabitlemeyi tercih ediyorsanız tekrar panoya yönlendirilirsiniz ve Data Lake Store hesap hazırlama işleminizin ilerleme durumunu görebilirsiniz. Data Lake Store hesabı sağlandıktan sonra, hesap dikey penceresi görünür.

### <a name="assign-permissions-to-azure-key-vault"></a>Azure Key Vault’a izin atama
Data Lake Store hesabında şifreleme yapılandırmak için bir Azure Anahtar Kasası’ndaki anahtarları kullandıysanız Data Lake Store hesabı ile Azure Anahtar Kasası hesabı arasında erişim yapılandırmalısınız. Bunu yapmak için aşağıdaki adımları gerçekleştirin.

1. Azure Anahtar Kasası’ndaki anahtarları kullandıysanız Data Lake Store hesabına ait dikey pencerenin üstünde bir uyarı görüntülenir. Uyarıya tıklayarak **Şifreleme** sayfasını açın.
   
    ![Data Lake Store şifrelemesi](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Data Lake Store encryption")
2. Bu dikey pencere, erişimi yapılandırmak için iki seçenek gösterir.

    ![Data Lake Store şifrelemesi](./media/data-lake-store-get-started-portal/adls-encryption-4.png "Data Lake Store encryption")
   
   * Erişim yapılandırmak için ilk seçenekte **İzin Ver**'e tıklayın. İlk seçenek yalnızca Data Lake Store hesabını oluşturan kullanıcı aynı zamanda Azure Anahtar Kasası’nın bir yöneticisiyse çalışır.
   * Diğer seçenek, dikey pencerede görüntülenen PowerShell cmdlet’ini çalıştırmak içindir. Azure Anahtar Kasası’nın sahibi olmanız ya da Azure Anahtar Kasası’nda izin verme yetkisine sahip olmanız gerekir. Cmdlet’i çalıştırdıktan sonra, erişimi yapılandırmak için dikey pencereye dönün ve **Etkinleştir**’e tıklayın.

> [!NOTE]
> İsterseniz Azure Resource Manager şablonlarını kullanarak bir Data Lake Store hesabı da oluşturabilirsiniz. Bu şablonlara [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/?term=data+lake+store)’ndan erişebilirsiniz:
    - Veri şifreleme olmadan: [Azure Data Lake Store hesabını veri şifrelemesi olmadan dağıtma](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/).
    - Data Lake Store kullanarak veri şifrelemesi ile: [Şifreleme (Data Lake) ile Data Lake Store hesabı dağıtma](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
    - Azure Key Vault kullanarak veri şifrelemesi ile: [Şifreleme (Key Vault) ile Data Lake Store hesabı dağıtma](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).
> 
> 



## <a name="createfolder"></a>Azure Data Lake Store hesabında klasör oluşturma
Veri depolamak ve yönetmek için Data Lake Store hesabınızın altında klasör oluşturabilirsiniz.

1. Oluşturduğunuz Data Lake Store hesabını açın. Sol bölmeden **Gözat**'a tıklayın, **Data Lake Store**'a tıklayın ve Data Lake Store dikey penceresinden, altında klasör oluşturmak istediğiniz hesabın adına tıklayın. Hesabı başlangıç panosuna sabitlediyseniz bu hesap kutucuğuna tıklayın.
2. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Data Lake Store hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Create folders in Data Lake Store account")
3. Veri Gezgini dikey penceresinde, **Yeni Klasör**'e tıklayın, yeni klasör için bir ad girin ve ardından **Tamam**'a tıklayın.
   
    ![Data Lake Store hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Create folders in Data Lake Store account")
   
    Yeni oluşturulan klasör, **Veri Gezgini** dikey penceresinde listelenir. Herhangi bir düzeye kadar iç içe geçmiş klasörler oluşturabilirsiniz.
   
    ![Data Lake hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Create folders in Data Lake account")

## <a name="uploaddata"></a>Azure Data Lake Store hesabına veri yükleme
Verilerinizi Azure Data Lake Store hesabınıza doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir klasöre yüklenecek şekilde yükleyebilirsiniz. 

1. **Veri Gezgini** dikey penceresinde **Yükle**'ye tıklayın. 
2. **Dosyaları karşıya yükleme** dikey penceresinde yüklemek istediğiniz dosyaları bulun ve **Seçili dosyaları ekle**'ye tıklayın. Yüklemek için birden fazla dosya da seçebilirsiniz.

    ![Veri yükleme](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

## <a name="properties"></a>Depolanan verilerde kullanılabilen eylemler
Dosyanın yanındaki üç nokta simgesine tıklayın ve açılan menüden verilerle ilgili gerçekleştirmek istediğiniz eyleme tıklayın.

![Verilere yönelik özellikler](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data") 

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
Azure Active Directory'yi ve erişim denetimini (ACL'ler) kullanarak Azure Data Lake Store hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Bunun nasıl yapılacağına ilişkin yönergeler için bkz. [Azure Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Azure Data Lake Store hesabını silme
Bir Azure Data Lake Store hesabını silmek için, Data Lake Store dikey pencerenizden **Sil**'e tıklayın. Eylemi onaylamak için silmek istediğiniz hesabın adını girmeniz istenir. Hesabın adını girin ve ardından **Sil**'e tıklayın.

![Data Lake hesabını silme](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete Data Lake account")

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store'u büyük veri gereksinimleri için kullanma](data-lake-store-data-scenarios.md) 
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)

