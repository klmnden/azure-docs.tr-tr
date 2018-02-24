---
title: "Azure Depolama Gezgini’nde Azure Data Lake Store kaynaklarını yönetme"
description: "Kullanıcıların Azure Kaynak Gezgini’nde ADLS verileriniz ve kaynaklarınıza erişmelerine ve bunları yönetmelerine olanak sağlar"
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: 
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 4c69acebceac6719cf3f12ff0a93879f1b5a8943
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="manage-azure-data-lake-store-resources-with-storage-explorer-preview"></a>Depolama Gezgini (önizleme) ile Azure Data Lake Store kaynaklarını yönetme

[Azure Data Lake Store (ADLS)](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview), büyük miktarlarda metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için bir hizmettir. Kullanıcılar HTTP veya HTTPS aracılığıyla verilere herhangi bir yerden erişebilir. Azure Depolama Gezgini’de ADLS, kullanıcıların blob ve kuyruk gibi diğer Azure varlıklarının yanı sıra ADLS verileri ve kaynaklarına erişmelerine ve bunları yönetmelerine olanak sağlar. Artık kullanıcılar farklı Azure varlıklarını aynı aracı kullanarak tek bir yerde yönetebilir.

Başka bir avantajı, kullanıcıların ADLS verilerini yönetmek için abonelik izninie sahip olmasının gerekmemesidir. Depolama Gezgini’nde, kullanıcılar diğer kullanıcılar buna izin verdiği sürece ADLS yolunu “Yerel ve Ekli” düğümüne ekleyebilir.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımları tamamlayabilmeniz için şu önkoşullar gereklidir:

*   Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial).
*   Bir Azure Data Lake Store hesabı. Hesap oluşturmaya ilişkin yönergeler için bkz. [Azure Data Lake Store kullanmaya başlama](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal)

## <a name="installation"></a>Yükleme

Şuradan en yeni Azure Depolama Gezgini bileşenlerini yükleyin: [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/). Artık Windows, Linux ve MAC sürümlerini destekliyoruz.

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

1. **Azure Depolama Gezgini**’ni yükledikten sonra, aşağıdaki resimde gösterildiği gibi soldaki **eklenti** simgesine tıklayın.
       
   ![Eklenti simgesi](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. **Azure Hesabı Ekle**'yi seçip **Oturum açın**'a tıklayın.

   ![Azure aboneliğine bağlanma](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. **Azure Oturum Açma** iletişim kutusunda **Oturum aç**’ı seçip Azure kimlik bilgilerinizi girin.

    ![Oturum aç](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Listeden aboneliğinizi seçip **Uygula**’ya tıklayın.

    ![Uygula](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    Gezgin bölmesi güncelleştirilir ve seçili abonelikteki hesapları gösterir.

    ![Hesap listesi](./media/data-lake-store-in-storage-explorer/account-list.png)

    **Azure Data Lake Store** hesabınızı Azure aboneliğinize başarıyla bağladınız.

## <a name="connect-to-data-lake-store"></a>Data Lake Store’a bağlanma
Aboneliğinizde olmayan kaynaklara erişmek istiyorsanız. Ancak diğer kullanıcılar kaynaklar için URI’yi alma izni veriyorsa. Bu durumda, oturum açtıktan sonra URI’yi kullanarak Data Lake Store’a bağlanabilirsiniz. Aşağıdaki adımlara bakın.
1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, **Yerel ve Ekli** öğesini genişletin.
3. **Data Lake Store**’a sağ tıklayıp bağlam menüsünden **Data Lake Store’a bağlan...** seçeneğine tıklayın.

      ![Data Lake Store’a bağlanma bağlam menüsü](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Siz URI’yi girdikten sonra araç girdiğiniz URL’nin konumuna gider.

      ![Data Lake Store’a bağlanma bağlam iletişimi](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![Data Lake Store’a bağlanma sonucu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>Azure Data Lake Store hesabının içeriklerini görüntüleme
Bir Azure Data Lake Store hesabının kaynakları klasörler ve dosyaları içerir.

Aşağıdaki adımlar, Depolama Gezgini’ndeki (Önizleme) bir ADLS hesabının içeriğini görüntüleme işlemini göstermektedir:

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, görüntülemek istediğiniz Azure Data Lake Store hesabını içeren aboneliği genişletin.
3. **Data Lake Store**’u genişletin.
4. Görüntülemek istediğiniz Azure Data Lake Store hesabı düğümüne sağ tıklayın ve bağlam menüsünden **Aç**’ı seçin. Ayrıca ADLS hesabını açmak için çift tıklayabilirsiniz. 
5. Ana bölme ADLS hesabının içeriklerini gösterir.

     ![ana bölme](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="azure-data-lake-store-resources-management"></a>Azure Data Lake Store kaynak yönetimi

Aşağıdaki işlemleri gerçekleştirerek Azure Data Lake Store kaynaklarını yönetebilirsiniz:
*   Birden çok ADL hesabındaki ADLS kaynaklarında gezinme.  
*   Bağlantı dizesini kullanarak ADLS’ye doğrudan bağlanma ve yönetme. 
*   Yerel ve Ekli altında ACL üzerinden diğer kullanıcılar tarafından paylaşılan ADLS kaynaklarını görüntüleme.
*   Dosya/Klasör CRUD İşlemleri gerçekleştirme: Özyinelemeli klasör ve çoklu seçimli dosya desteği. 
*   Masaüstü dosya gezgini deneyiminde olduğu gibi, klasörleri sürükleyip bırakarak hızlı erişim ve son kullanılan konumlara ekleme. 
*   Depolama Gezgini ile bir tıklamayla ADL köprülerini kopyalama ve açma. 
*   Etkinlik durumunu görüntülemek için sağ altta etkinlik günlüğünü görüntüleme.
*   Klasör istatistiklerini ve dosya özelliklerini görüntüleme.

## <a name="manage-resources-in-azure-storage-explorer"></a>Azure Depolama Gezgini’nde kaynakları yönetme
Bir Azure Data Lake Store hesabı oluşturduktan sonra, klasör ve dosyaları karşıya yükleyebilir, indirebilir ve kaynakları yerel bilgisayarınızda açabilirsiniz. Ayrıca öğeleri hızlı erişime ekleyebilir ve yeni klasör, URL kopyalama ve tümünü seçme işlemlerini gerçekleştirebilirsiniz. Buna ek olarak, kopyalama, yapıştırma, yeniden adlandırma, silme, klasör istatistikleri ve yenileme işlemlerini gerçekleştirebilirsiniz.

Aşağıdaki öğelerde bir Azure Data Lake Store hesabındaki kaynakların nasıl yönetileceği gösterilmektedir. Gerçekleştirmek istediğiniz göreve bağlı olarak aşağıdaki adımları izleyin:
   * **Dosyaları karşıya yükleme**

     1. Ana bölmedeki araç çubuğunda **Karşıya Yükle** öğesini ve ardından açılır listedeki **Dosyaları Karşıya Yükle...** öğesini seçin.

        ![Dosyaları karşıya yükleme menüsü](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

     2. **Karşıya yüklenecek dosyaları seçin** iletişim kutusunda, karşıya yüklemek istediğiniz dosyaları seçin.

        ![Klasörü karşıya yükle iletişimi](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

     3. Karşıya yüklemeyi başlatmak için **Aç**’ı seçin.
   * **Bir klasörü karşıya yükleme**

     1. Ana bölmedeki araç çubuğunda **Karşıya Yükle** öğesini ve ardından açılır listedeki **Klasörü Karşıya Yükle...** öğesini seçin.

        ![Klasörü karşıya yükle menüsü](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     2. **Karşıya yüklenecek klasörü seçin** iletişim kutusunda, karşıya yüklemek istediğiniz klasörü seçin.

        ![Klasörü karşıya yükle iletişimi](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

     3. Klasörleri karşıya yüklemeye başlamak için **Klasör Seçin** öğesini seçin.

        ![Klasörü karşıya yükle iletişimi](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

      > [!NOTE] 
          > Karşıya yüklemek için doğrudan yerel bilgisayarda klasör ve dosyaları sürükleyebilirsiniz. 
       
   * **Klasörleri veya dosyaları yerel bilgisayarınıza indirme**

     1. İndirmek istediğiniz klasörler veya dosyaları seçin.
     2. Ana bölmedeki araç çubuğunda **İndir**’i seçin.
     3. **İndirilen dosyaların kaydedileceği klasörü seçin** iletişim kutusunda, klasör veya dosyaların indirilmesini istediğiniz konumu belirtin. Vermek istediğiniz adı belirtin.
     4. **Kaydet**’i seçin.
   * **Yerel bilgisayarınızdan klasör veya dosyaları açma**

     1. Açmak istediğiniz klasör veya dosyayı seçin.
     2. Ana bölme araç çubuğunda, **Aç**’ı seçin veya seçili klasör veya dosyaya sağ tıklayın ve bağlam menüsünde **Aç**’a tıklayın.
     3. Dosya indirilir ve dosyanın temel alınan dosya türü ile ilişkili uygulama kullanılarak açılır. Alternatif olarak, klasör ana bölmede açılır.

        ![dosya aç](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

   * **Dosya veya klasörleri panoya kopyalama**

     1. Kopyalamak istediğiniz klasörler veya dosyaları seçin.
     2. Ana bölme araç çubuğunda, **Kopyala**’yı seçin veya seçili klasör veya dosyaya sağ tıklayın ve bağlam menüsünde **Kopyala**’ya tıklayın.
     3. Sol bölmede başka bir ADLS hesabına gidin ve ana bölmede görüntülemek için çift tıklayın.
     4. Ana bölmedeki araç çubuğunda **Yapıştır**’ı seçerek bir kopyasını oluşturun. Alternatif olarak, hedefte bağlam menüsüne tıklayıp **Yapıştır**’a tıklayın.

        ![klasör veya dosyayı kopyalama ve yapıştırma](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

      > [!NOTE] 
          > + Depolama türleri arasında kopyalama-yapıştırma **desteklenmez**. ADLS klasörleri veya dosyalarını kopyalayabilir ve başka bir ADLS hesabına yapıştırabilirsiniz. Ancak ADLS klasör ve dosyalarını blob depolamaya **kopyalayıp yapıştıramaz** veya bunun tersini gerçekleştiremezsiniz. 
          > + Kopyala-Yapıştır işlemi klasör ve dosyaları yerel bilgisayara indirip ardından hedef konuma yükleyerek çalışır. Araç eylemi arka uçta **gerçekleştirmez**. Büyük dosyalarda kopyala-yapıştır işlemi yavaştır. Yüksek performanslı dosya kopyalama-taşıma iyileştirmeleri devam ediyor.
   * **Klasör veya dosyaları silme**

     1. Silmek istediğiniz klasörler veya dosyaları seçin.
     2. Ana bölme araç çubuğunda, **Sil**’i seçin veya seçili klasör veya dosyaya sağ tıklayın ve bağlam menüsünde **Sil**’e tıklayın.
     3. Onay iletişim kutusunda **Evet**’i seçin.

        ![klasör veya dosyayı kopyalama ve yapıştırma](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

   * **Hızlı erişime sabitleme**

     1. Sabitlemek istediğiniz klasörü seçin.
     2. Ana bölmedeki araç çubuğunda **Hızlı erişime sabitle**’yi seçin.
     3. Sol bölmede, seçili klasörün **Hızlı Erişim** düğümüne eklendiğini görürsünüz.

        ![hızlı erişime sabitleme](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)
     4. Hızlı erişimi oluşturduktan sonra, kaynaklara kolayca ulaşabilirsiniz.
   * **Ayrıntılı Bağlantılar**
     1. Bir URL’niz varsa, URL’yi **Dosya Gezgini** veya tarayıcıda adres yoluna girebilirsiniz.
     2. Ardından **Storage Explorer.exe** otomatik olarak başlatılarak girdiğiniz URL’nin konumuna gider.

        ![dosya gezgininde ayrıntılı bağlantı](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Sonraki adımlar
* [En son Depolama Gezgini (Önizleme) sürüm notlarını ve videolarını](http://www.storageexplorer.com) görüntüleyin.
* [Azure Depolama Gezgini’nde Azure Cosmos DB’yi yönetmeyi](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer) öğrenin
* [Depolama Gezgini (Önizleme) ile çalışmaya başlama](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer) konusunda Depolama Gezgini hakkında daha fazla bilgi edinin
* Azure Data Lake Store (ADLS) kullanmaya başlama[Azure Data Lake Store’a genel bakış](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview)
* [Azure Depolama Gezgini’nde Azure Cosmos DB kullanmayı öğrenmek için](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be) şu videoyu izleyin
