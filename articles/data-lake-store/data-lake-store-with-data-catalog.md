---
title: Data Lake Store verileri Azure veri Kataloğu'nda kaydetme | Microsoft Docs
description: Azure veri Kataloğu'nda Data Lake Store verileri kaydetme
services: data-lake-store,data-catalog
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 8da9f0f8aeb36d9ff2f87511c902dd719bc755b9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441613"
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Azure veri Kataloğu'nda Data Lake Store verileri kaydetme
Bu makalede, Azure Data Lake Store, veri Kataloğu ile tümleştirerek verilerinizi bir kuruluş içinde bulunabilir hale getirmek için Azure veri Kataloğu ile tümleştirmeyi öğreneceksiniz. Veri kataloglama daha fazla bilgi için bkz: [Azure veri Kataloğu](../data-catalog/data-catalog-what-is-data-catalog.md). Veri Kataloğu, kullanabileceğiniz senaryoları anlamak için bkz: [Azure veri Kataloğu genel senaryoları](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure aboneliğinizi etkinleştirme** Data Lake Store genel önizlemesi için. Bkz. [yönergeler](data-lake-store-get-started-portal.md).
* **Azure Data Lake Store hesabı**. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın. Bu öğreticide, adlı bir Data Lake Store hesabı oluşturmak **datacatalogstore**.

    Hesap oluşturulduktan sonra, bir örnek veri kümesini karşıya. Bu öğreticide, bize altındaki tüm .csv dosyaları karşıya yükleme **AmbulanceData** klasöründe [Azure Data Lake Git deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Çeşitli istemciler gibi kullanabileceğiniz [Azure Depolama Gezgini](http://storageexplorer.com/), veriler bir blob kapsayıcısına yükleyin.
* **Azure veri Kataloğu**. Kuruluşunuz zaten bir Azure veri Kataloğu, kuruluşunuz için oluşturulmuş olması gerekir. Her kuruluş için yalnızca bir katalog izin verilir.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Veri kataloğu için bir kaynak olarak kayıt Data Lake Store

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Git `https://azure.microsoft.com/services/data-catalog`, tıklatıp **başlama**.
1. Azure veri Kataloğu portalında oturum açın ve tıklayın **veri yayımlama**.

    ![Veri kaynağını kaydetme](./media/data-lake-store-with-data-catalog/register-data-source.png "veri kaynağını kaydetme")
1. Sonraki sayfada **uygulama Başlat**. Bu uygulama bildirim dosyasını bilgisayarınızdaki indirir. Uygulamayı başlatmak için bildirim dosyasını çift tıklayın.
1. Hoş Geldiniz sayfasında tıklayın **oturum**, kimlik bilgilerinizi girin.

    ![Hoş Geldiniz ekranı](./media/data-lake-store-with-data-catalog/welcome.screen.png "Hoş Geldiniz ekranı")
1. Bir veri kaynağı sayfa seçin üzerinde seçin **Azure Data Lake**ve ardından **sonraki**.

    ![Veri kaynağı seçme](./media/data-lake-store-with-data-catalog/select-source.png "veri kaynağını seçin")
1. Sonraki sayfada, veri Kataloğu'nda kaydetmek istediğiniz Data Lake Store hesap adını belirtin. Diğer seçenekleri varsayılan bırakın ve ardından **Connect**.

    ![Veri kaynağına bağlanma](./media/data-lake-store-with-data-catalog/connect-to-source.png "veri kaynağına bağlanma")
1. Sonraki sayfaya aşağıdaki parçalara ayrılabilir.

    a. **Sunucusu hiyerarşisi** kutusunu Data Lake Store hesabı klasör yapısını temsil eder. **$Root** Data Lake Store hesabının kökü temsil eder ve **AmbulanceData** Data Lake Store hesabının kök dizininde oluşturduğunuz klasöre temsil eder.

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
* [Data Lake Store, diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md)
