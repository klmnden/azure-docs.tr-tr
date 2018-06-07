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
ms.openlocfilehash: 3f1bc925b772265a9f72c34f5ac661088123bb1a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626146"
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Azure veri Kataloğu'nda Data Lake Store verileri kaydetme
Bu makalede, Azure Data Lake Store veri Kataloğu ile tümleştirerek verilerinizin kuruluş içindeki bulunabilmesini sağlamak için Azure veri Kataloğu ile tümleştirmek öğreneceksiniz. Veri katalog daha fazla bilgi için bkz: [Azure veri Kataloğu](../data-catalog/data-catalog-what-is-data-catalog.md). Veri Kataloğu, kullanabileceğiniz senaryoları anlamak için bkz: [Azure veri Kataloğu genel senaryoları](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure aboneliğinizi etkinleştirme** Data Lake Store genel önizlemesi için. Bkz. [yönergeler](data-lake-store-get-started-portal.md).
* **Azure Data Lake Store hesabı**. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın. Bu öğretici için adlı bir Data Lake Store hesabı oluşturmak **datacatalogstore**.

    Hesabı oluşturduktan sonra bir örnek veri kümesi için karşıya yükleme. Bu öğretici için bize altındaki tüm .csv dosyaları karşıya **AmbulanceData** klasöründe [Azure Data Lake Git deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Çeşitli istemciler gibi kullanabilir [Azure Storage Gezgini](http://storageexplorer.com/), verileri bir blob kapsayıcıya yüklemek için.
* **Azure veri Kataloğu**. Kuruluşunuz zaten bir Azure veri Kataloğu, kuruluşunuz için oluşturulmuş olması gerekir. Yalnızca bir katalog her kuruluş için izin verilir.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Veri kataloğu için bir kaynak olarak kayıt Data Lake Store

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Git `https://azure.microsoft.com/services/data-catalog`, tıklatıp **başlama**.
2. Azure veri Kataloğu portalında oturum öğesini tıklatıp **veri yayımlama**.

    ![Veri kaynağını kaydetme](./media/data-lake-store-with-data-catalog/register-data-source.png "veri kaynağını kaydetme")
3. Sonraki sayfada tıklatın **uygulama Başlat**. Bu uygulama bildirim dosyasının, bilgisayarınıza yükleyecek. Uygulamayı başlatmak için bildirim dosyasını çift tıklatın.
4. Hoş Geldiniz sayfasında, tıklatın **oturum**ve kimlik bilgilerinizi girin.

    ![Hoş Geldiniz ekranı](./media/data-lake-store-with-data-catalog/welcome.screen.png "Hoş Geldiniz ekranı")
5. Üzerinde bir veri kaynağı sayfasında, seçin **Azure Data Lake**ve ardından **sonraki**.

    ![Veri kaynağı seçme](./media/data-lake-store-with-data-catalog/select-source.png "veri kaynağını seçin")
6. Sonraki sayfada, veri Kataloğu'nda kaydetmek istediğiniz Data Lake Store hesabı adı girin. Diğer seçenekleri varsayılan olarak bırakın ve ardından **Bağlan**.

    ![Veri kaynağına bağlan](./media/data-lake-store-with-data-catalog/connect-to-source.png "veri kaynağına bağlan")
7. Sonraki sayfaya aşağıdaki kesimler halinde ayrılabilir.

    a. **Sunucusu hiyerarşisi** kutusunu Data Lake Store hesabı klasör yapısını temsil eder. **$Root** Data Lake Store hesabı kök temsil eder ve **AmbulanceData** Data Lake Store hesabı kökünde oluşturulan klasörü temsil eder.

    b. **Kullanılabilir nesneler** kutusu listeler altındaki dosya ve klasörler **AmbulanceData** klasör.

    c. **Kaydedilecek nesneler** kutusu Azure veri Kataloğu'nda kaydetmek istediğiniz klasörleri ve dosyaları listeler.

    ![Veri yapısı görüntülemek](./media/data-lake-store-with-data-catalog/view-data-structure.png "görüntülemek veri yapısı")
8. Bu öğreticide, dizindeki tüm dosyaları kaydetmelisiniz. Bunun için tıklatın (![nesneleri taşıma](./media/data-lake-store-with-data-catalog/move-objects.png "nesneleri taşıma")) tüm dosyaların taşınacağı düğmesi **kaydedilecek nesneler** kutusu.

    Verileri bir kuruluş genelinde veri Kataloğu'nda kayıtlı olması nedeniyle, daha sonra verileri hızla bulmak için kullanabileceğiniz bazı meta verileri eklemek için önerilen yaklaşımdır. Örneğin, veri sahibi (örneğin, kimse karşıya veri yükleme) bir e-posta adresi ekleyebilir veya verileri tanımlamak için bir etiket ekleyebilirsiniz. Aşağıdaki ekran görüntüsünde, verileri eklediğinizde bir etiket gösterir.

    ![Veri yapısı görüntülemek](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "görüntülemek veri yapısı")

    Tıklatın **kaydetmek**.
9. Aşağıdaki ekran görüntüsünde, verileri veri Kataloğu'nda başarıyla kaydedildiğini gösterir.

    ![Kayıt tamamlandı](./media/data-lake-store-with-data-catalog/registration-complete.png "görüntülemek veri yapısı")
10. Tıklatın **portalı görüntüle** veri Kataloğu portalına geri dönün ve Portalı'ndan kayıtlı veri şimdi erişebildiğinizi doğrulayın. Veri aramak için veri kaydedilirken kullanılan etiketi kullanabilirsiniz.

     ![Veri Kataloğu'nda arama](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "veri Kataloğu'nda arama")
11. Şimdi veri ek açıklamaları ve belgelerine ekleme gibi işlemleri gerçekleştirebilir. Daha fazla bilgi için aşağıdaki bağlantılara bakın.

    * [Veri Kataloğu veri kaynaklarına açıklama ekleme](../data-catalog/data-catalog-how-to-annotate.md)
    * [Belge veri kaynakları veri Kataloğu](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Veri Kataloğu veri kaynaklarına açıklama ekleme](../data-catalog/data-catalog-how-to-annotate.md)
* [Belge veri kaynakları veri Kataloğu](../data-catalog/data-catalog-how-to-documentation.md)
* [Data Lake Store diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md)
