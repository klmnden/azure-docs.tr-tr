---
title: Data Lake Store Stream Analytics veri akışı | Microsoft Docs
description: Azure Stream Analytics akış verileri için Azure Data Lake Store kullanın
services: data-lake-store,stream-analytics
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 01/30/2018
ms.author: nitinme
ms.openlocfilehash: 7ff59957cf7700af79425aa005444a135b7ee098
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Azure Akış Analizi’ni kullanarak Azure Depolama Blobundan Data Lake Store’a veri akışı gerçekleştirme
Bu makalede Azure Data Lake Store için Azure Stream Analytics işi çıkış olarak kullanmak üzere öğreneceksiniz. Bu makale bir Azure Storage blobundan (giriş) veri okuyan ve Data Lake Store (çıktı) verileri yazar basit bir senaryo gösterir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Depolama hesabı**. Bu hesap bir blob kapsayıcısından Stream Analytics işi için veri girişi için kullanır. Bu öğretici için adlı bir depolama hesabı olduğunu varsayın **storageforasa** ve hesap içinde bir kapsayıcı adlı **storageforasacontainer**. Kapsayıcıyı oluşturduktan sonra bir örnek veri dosyası için karşıya yükleyin. 
  
* **Azure Data Lake Store hesabı**. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın. Adlı bir Data Lake Store hesabı sahip varsayalım **asadatalakestore**. 

## <a name="create-a-stream-analytics-job"></a>Akış analizi işi oluşturma
Bir giriş kaynağı ve bir çıkış hedefini içeren bir akış analizi işi oluşturarak başlayın. Bu öğretici için kaynak bir Azure blob kapsayıcısı ve hedef Data Lake Store.

1. [Azure Portal](https://portal.azure.com)'da oturum açın.

2. Sol bölmeden tıklatın **akış analizi işleri**ve ardından **Ekle**.

    ![Stream Analytics işi oluşturmak](./media/data-lake-store-stream-analytics/create.job.png "Stream Analytics işi oluşturma")

    > [!NOTE]
    > Depolama hesabı ile aynı bölgede işi oluşturmak veya bölgeler arasında veri taşıma ek maliyet erişimliye emin olun.
    >

## <a name="create-a-blob-input-for-the-job"></a>İş için bir Blob giriş oluştur

1. Sayfanın sol bölmeden Stream Analytics işi için Aç'ı tıklatın **girişleri** sekmesini ve ardından **Ekle**.

    ![İşiniz için bir giriş eklemek](./media/data-lake-store-stream-analytics/create.input.1.png "işinizi girdi ekleme")

2. Üzerinde **yeni giriş** dikey penceresinde, aşağıdaki değerleri girin.

    ![İşiniz için bir giriş eklemek](./media/data-lake-store-stream-analytics/create.input.2.png "işinizi girdi ekleme")

    * İçin **giriş diğer adı**, giriş işi için benzersiz bir ad girin.
    * İçin **kaynak türünü**seçin **veri akışı**.
    * İçin **kaynak**seçin **Blob storage**.
    * İçin **abonelik**seçin **blob storage'ı geçerli aboneliğe ilişkin kullanma**.
    * İçin **depolama hesabı**, önkoşulları bir parçası olarak oluşturduğunuz depolama hesabını seçin. 
    * İçin **kapsayıcı**, seçilen depolama hesabında oluşturulan kapsayıcısı seçin.
    * İçin **olayı seri hale getirme biçimi**seçin **CSV**.
    * İçin **sınırlayıcı**seçin **sekmesini**.
    * İçin **kodlama**seçin **UTF-8**.

    **Oluştur**’a tıklayın. Şimdi portal giriş ekler ve bağlantıyı sınar.


## <a name="create-a-data-lake-store-output-for-the-job"></a>İş için Data Lake Store çıktı oluşturma

1. Sayfa Stream Analytics işi için Aç'ı tıklatın **çıkışları** sekmesini ve ardından **Ekle**.

    ![Çıkış, işe ekleme](./media/data-lake-store-stream-analytics/create.output.1.png "çıktısı, işe ekleme")

2. Üzerinde **yeni çıktı** dikey penceresinde, aşağıdaki değerleri girin.

    ![Çıkış, işe ekleme](./media/data-lake-store-stream-analytics/create.output.2.png "çıktısı, işe ekleme")

    * İçin **çıkış diğer adları**, girin bir iş çıktısı için benzersiz bir ad. Bu, sorgu çıktısı bu Data Lake Store'a doğrudan sorgularda kullanılan kolay bir addır.
    * İçin **havuzu**seçin **Data Lake Store**.
    * Data Lake Store hesabına erişim yetkisi vermek için istenir. Tıklatın **yetkilendirmek**.

3. Üzerinde **yeni çıktı** dikey penceresinde, aşağıdaki değerleri sağlamaya devam.

    ![Çıkış, işe ekleme](./media/data-lake-store-stream-analytics/create.output.3.png "çıktısı, işe ekleme")

    * İçin **hesap adı**, Data Lake Store hesabı zaten oluşturuldu çıkış gönderilmesi için iş istediğiniz yeri seçin.
    * İçin **yol önek deseni**, belirtilen Data Lake Store hesabındaki dosyalarınızı yazmak için kullanılan bir dosya yolu girin.
    * İçin **tarih biçimi**, önek yolunda bir tarih belirteci kullandıysanız, dosyalarınızı düzenlenir tarih biçimi seçebilirsiniz.
    * İçin **saat biçimi**, önek yolunda bir zaman belirteci kullandıysanız, dosyalarınızı düzenlenir saat biçimini belirtin.
    * İçin **olayı seri hale getirme biçimi**seçin **CSV**.
    * İçin **sınırlayıcı**seçin **sekmesini**.
    * İçin **kodlama**seçin **UTF-8**.
    
    **Oluştur**’a tıklayın. Portal şimdi çıktı ekler ve bağlantıyı sınar.
    
## <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

1. Akış analizi işi çalıştırmak için bir sorgu çalıştırın **sorgu** sekmesi. Bu öğretici için giriş işlemiyle yer tutucuları değiştirerek örnek sorgu çalıştırabilirsiniz ve aşağıdaki ekran görüntüsünde gösterildiği gibi diğer adlar, çıktı.

    ![Sorguyu çalıştırmak](./media/data-lake-store-stream-analytics/run.query.png "sorgusu çalıştırma")

2. Tıklatın **kaydetmek** ekranın, ardından da üstten **genel bakış** sekmesini tıklatın, **Başlat**. İletişim kutusundan **özel zaman**ve geçerli tarih ve saat ayarlayın.

    ![İş saati ayarlayın](./media/data-lake-store-stream-analytics/run.query.2.png "iş saati ayarlayın")

    Tıklatın **Başlat** işini başlatmak için. Bu birkaç dakika işin başlangıç kadar sürebilir.

3. Blob verilerini almak için iş tetiklemek için blob kapsayıcısına örnek veri dosyasını kopyalayın. Bir örnek veri dosyasından alabilirsiniz [Azure Data Lake Git deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Bu öğretici için şimdi dosya kopyalama **vehicle1_09142014.csv**. Çeşitli istemciler gibi kullanabilir [Azure Storage Gezgini](http://storageexplorer.com/), verileri bir blob kapsayıcıya yüklemek için.

4. Gelen **genel bakış** sekmesinde, altında **izleme**, verileri nasıl işlendiği bakın.

    ![İzleyici işi](./media/data-lake-store-stream-analytics/run.query.3.png "İzleyici işi")

5. Son olarak, iş çıkışı verileri Data Lake Store hesabında kullanılabilir olduğunu doğrulayabilirsiniz. 

    ![Doğrulama çıktı](./media/data-lake-store-stream-analytics/run.query.4.png "çıkış doğrulayın")

    Veri Gezgini bölmesinde Data Lake Store'da belirtildiği gibi bir klasör yolu için çıktı yazılır dikkat edin çıktı ayarları (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Store kullanmak için bir Hdınsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)
