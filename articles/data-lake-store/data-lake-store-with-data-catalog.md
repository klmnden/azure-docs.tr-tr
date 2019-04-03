---
title: Azure Data Lake depolama Gen1 verileri Azure veri Kataloğu'nda kaydetme | Microsoft Docs
description: Azure veri Kataloğu'nda Azure Data Lake depolama Gen1 verileri kaydetme
services: data-lake-store,data-catalog
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: fd887560c0011fb1ec2141e33f02f7e3d8a39c81
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877893"
---
# <a name="register-data-from-azure-data-lake-storage-gen1-in-azure-data-catalog"></a>Azure veri Kataloğu'nda Azure Data Lake depolama Gen1 verileri kaydetme
Bu makalede, Azure Data Lake depolama Gen1 veri Kataloğu ile tümleştirerek verilerinizi bir kuruluş içinde bulunabilir hale getirmek için Azure veri Kataloğu ile tümleştirmeyi öğreneceksiniz. Veri kataloglama daha fazla bilgi için bkz: [Azure veri Kataloğu](../data-catalog/data-catalog-what-is-data-catalog.md). Veri Kataloğu, kullanabileceğiniz senaryoları anlamak için bkz: [Azure veri Kataloğu genel senaryoları](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure aboneliğinizi etkinleştirme** Data Lake depolama Gen1 için. Bkz. [yönergeler](data-lake-store-get-started-portal.md).
* **Bir Data Lake depolama Gen1 hesabı**. Konumundaki yönergeleri [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md). Bu öğreticide, adlı bir Data Lake depolama Gen1 hesabı oluşturabilir **datacatalogstore**.

    Hesap oluşturulduktan sonra, bir örnek veri kümesini karşıya. Bu öğreticide, bize altındaki tüm .csv dosyaları karşıya yükleme **AmbulanceData** klasöründe [Azure Data Lake Git deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Çeşitli istemciler gibi kullanabileceğiniz [Azure Depolama Gezgini](https://storageexplorer.com/), veriler bir blob kapsayıcısına yükleyin.
* **Azure veri Kataloğu**. Kuruluşunuz zaten bir Azure veri Kataloğu, kuruluşunuz için oluşturulmuş olması gerekir. Her kuruluş için yalnızca bir katalog izin verilir.

## <a name="register-data-lake-storage-gen1-as-a-source-for-data-catalog"></a>Data Lake depolama Gen1 veri kataloğu için bir kaynak olarak kaydedin.

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Git `https://azure.microsoft.com/services/data-catalog`, tıklatıp **başlama**.
1. Azure veri Kataloğu portalında oturum açın ve tıklayın **veri yayımlama**.

    ![Veri kaynağını kaydetme](./media/data-lake-store-with-data-catalog/register-data-source.png "veri kaynağını kaydetme")
1. Sonraki sayfada **uygulama Başlat**. Bu uygulama bildirim dosyasını bilgisayarınızdaki indirir. Uygulamayı başlatmak için bildirim dosyasını çift tıklayın.
1. Hoş Geldiniz sayfasında tıklayın **oturum**, kimlik bilgilerinizi girin.

    ![Hoş Geldiniz ekranı](./media/data-lake-store-with-data-catalog/welcome.screen.png "Hoş Geldiniz ekranı")
1. Bir veri kaynağı sayfa seçin üzerinde seçin **Azure Data Lake Store**ve ardından **sonraki**.

    ![Veri kaynağı seçme](./media/data-lake-store-with-data-catalog/select-source.png "veri kaynağını seçin")
1. Sonraki sayfada, veri Kataloğu'nda kaydetmek istediğiniz Data Lake depolama Gen1 hesap adını belirtin. Diğer seçenekleri varsayılan bırakın ve ardından **Connect**.

    ![Veri kaynağına bağlanma](./media/data-lake-store-with-data-catalog/connect-to-source.png "veri kaynağına bağlanma")
1. Sonraki sayfaya aşağıdaki parçalara ayrılabilir.

    a. **Sunucusu hiyerarşisi** kutusunu Data Lake depolama Gen1 hesabı klasör yapısını temsil eder. **$Root** Data Lake depolama Gen1 hesabı kök temsil eder ve **AmbulanceData** Data Lake depolama Gen1 hesabının kök dizininde oluşturduğunuz klasöre temsil eder.

    b. **Kullanılabilir nesneler** kutusu listeler dosyaları ve klasörlerinin **AmbulanceData** klasör.

    c. **Kaydedilecek nesneler** kutusu, Azure veri Kataloğu'nda kaydetmek istediğiniz klasörleri ve dosyaları listeler.

    ![Veri yapısı görüntülemek](./media/data-lake-store-with-data-catalog/view-data-structure.png "görüntülemek veri yapısı")
1. Bu öğreticide, dizindeki tüm dosyaları kaydolmalıdır. Bunun için tıklatın (![nesneleri taşıma](./media/data-lake-store-with-data-catalog/move-objects.png "nesneleri taşıma")) tüm dosyalar taşımak için düğmeyi **kaydedilecek nesneler** kutusu.

    Bir kuruluş genelinde veri Kataloğu'nda veri kaydedilecek çünkü daha sonra verileri hızla bulmak için kullanabileceğiniz bazı meta verileri eklemek için önerilen yaklaşımdır. Örneğin, veri sahibi (örneğin, kimse karşıya veri yükleyerek) için bir e-posta adresi ekleyin veya verileri tanımlamak için bir etiket ekleyin. Aşağıdaki ekran yakalama verileri eklediğinizde bir etiket gösterilmektedir.

    ![Veri yapısı görüntülemek](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "görüntülemek veri yapısı")

    Tıklayın **kaydetme**.
1. Aşağıdaki ekran görüntüsü yakalamayı verileri, veri Kataloğu'nda başarıyla kaydedildiğini gösterir.

    ![Kayıt tamamlandı](./media/data-lake-store-with-data-catalog/registration-complete.png "görüntülemek veri yapısı")
1. Tıklayın **portalı görüntüle** veri Kataloğu portalına geri dönün ve Portalı'ndan artık kayıtlı veri erişebildiğini doğrulayın. Veri aramak için verileri kaydedilirken kullanılan etiketi kullanabilirsiniz.

     ![Veri Kataloğu'nda arama](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "veri Kataloğu'nda arama")
1. Artık veri ek açıklamaları ve belgeler ekleme gibi işlemler gerçekleştirebilirsiniz. Daha fazla bilgi için aşağıdaki bağlantılara bakın.

    * [Veri Kataloğu'nda veri kaynaklarına açıklama](../data-catalog/data-catalog-how-to-annotate.md)
    * [Veri Kataloğu'nda veri kaynaklarını belgeleme](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Veri Kataloğu'nda veri kaynaklarına açıklama](../data-catalog/data-catalog-how-to-annotate.md)
* [Veri Kataloğu'nda veri kaynaklarını belgeleme](../data-catalog/data-catalog-how-to-documentation.md)
* [Data Lake depolama Gen1 diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md)
