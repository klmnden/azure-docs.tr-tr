---
title: Depolama Gezgini ile Azure Blob depolama kaynaklarını yönetme | Microsoft Docs
description: Azure Blob Kapsayıcılarınıza ve BLOB Depolama Gezgini ile yönetme
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: f46467871a5ae0147b5dc60881bda4175eabac56
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60458566"
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer"></a>Depolama Gezgini ile Azure Blob depolama kaynaklarını yönetme
## <a name="overview"></a>Genel Bakış
[Azure Blob Depolama](storage/blobs/storage-dotnet-how-to-use-blobs.md) büyük miktarda gelen her yerinden HTTP veya HTTPS aracılığıyla dünyanın erişilebilen metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için bir hizmettir.
Verileri genel olarak herkese açık kullanıma sunmak veya uygulama verilerini özel olarak depolamak için Blob Storage’ı kullanabilirsiniz. Bu makalede, blob kapsayıcıları ve blobları ile çalışmak için Depolama Gezgini'ni kullanma öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* [Depolama Gezgini’ni indirip yükleme](https://www.storageexplorer.com)
* [Bir Azure depolama hesabı veya hizmetine bağlanma](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma
Tüm BLOB'lar, BLOB'ları yalnızca mantıksal bir gruplandırması olan bir blob kapsayıcısında bulunmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir ve her kapsayıcı, BLOB'ları, sınırsız sayıda depolayabilirsiniz.

Aşağıdaki adımlar, depolama Gezgini'ndeki bir blob kapsayıcısı oluşturma işlemini göstermektedir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, blob kapsayıcısını oluşturmak istediğiniz depolama hesabını genişletin.
3. Sağ **Blob kapsayıcıları**ve - seçin bağlam menüsünden - **Blob kapsayıcısı Oluştur**.

   ![BLOB kapsayıcıları bağlam menüsü oluşturma][0]
4. Bir metin kutusu altında görünür **Blob kapsayıcıları** klasör. Blob kapsayıcınızın adını girin. Bkz: [kapsayıcı oluşturma ve izinleri ayarlama](storage/blobs/storage-quickstart-blobs-dotnet.md#create-the-container-and-set-permissions) blob kapsayıcılarını adlandırmayla ilgili kural ve kısıtlamaların hakkında bilgi için.

   ![BLOB kapsayıcıları metin kutusu oluşturma][1]
5. Tuşuna **Enter** blob kapsayıcısı oluşturma işlemi tamamlandığında veya **Esc** iptal etmek için. Blob kapsayıcısı başarıyla oluşturulduktan sonra uygulamanın altında görüntülenecek **Blob kapsayıcıları** seçili depolama hesabı için bir klasör.

   ![Oluşturulan blob kapsayıcısı][2]

## <a name="view-a-blob-containers-contents"></a>Bir blob kapsayıcının içeriğini görüntüleme
BLOB kapsayıcıları, blobları ve (Ayrıca BLOB'ları içerebilir) klasörleri içerir.

Aşağıdaki adımlar, depolama Gezgini'ndeki bir blob kapsayıcı içeriğini görüntüleme işlemini göstermektedir:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, görüntülemek istediğiniz blob kapsayıcısı içeren depolama hesabını genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstediğiniz görüntüleyin ve bağlam menüsünden - seçin - blob kapsayıcısını **açık Blob kapsayıcı Düzenleyicisi**.
   Ayrıca, görüntülemek istediğiniz blob kapsayıcısı çift tıklatabilirsiniz.

   ![Açık bir blob kapsayıcı Düzenleyicisi bağlam menüsü][19]
5. Ana bölmede blob kapsayıcının içeriğini görüntüler.

   ![BLOB kapsayıcı Düzenleyicisi][3]

## <a name="delete-a-blob-container"></a>Bir blob kapsayıcısını Sil
BLOB kapsayıcıları kolayca oluşturulabilir ve gerektiğinde silinebilir. (Tek tek bloblar silin, bölümüne bakın yapılacağını görmek için [blob kapsayıcı içindeki blobları yönetme](#managing-blobs-in-a-blob-container).)

Aşağıdaki adımlar, depolama Gezgini'ndeki bir blob kapsayıcısını silme işlemini göstermektedir:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, görüntülemek istediğiniz blob kapsayıcısı içeren depolama hesabını genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstediğiniz silin ve bağlam menüsünden - seçin - blob kapsayıcısını **Sil**.
   Tuşlarına da basabilirsiniz **Sil** seçili blob kapsayıcısı silinemedi.

   ![BLOB kapsayıcı bağlam menüsü Sil][4]
5. Onay iletişim kutusunda **Evet**’i seçin.

   ![BLOB kapsayıcısını onaylama sil][5]

## <a name="copy-a-blob-container"></a>Bir blob kapsayıcısına kopyalayın
Depolama Gezgini, bir blob kapsayıcısı panoya kopyalayın ve ardından bu blob kapsayıcısında başka bir depolama hesabına yapıştırabilirsiniz sağlar. (Tek tek bloblar kopyalayın, bölümüne bakın yapılacağını görmek için [blob kapsayıcı içindeki blobları yönetme](#managing-blobs-in-a-blob-container).)

Aşağıdaki adımlar, bir blob kapsayıcısına bir depolama hesabından diğerine kopyalama işlemini göstermektedir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, kopyalamak istediğiniz blob kapsayıcısı içeren depolama hesabını genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstediğiniz kopyalayın ve bağlam menüsünden - seçin - blob kapsayıcısını **kopyalama Blob kapsayıcısı**.

   ![Kopyalama blob kapsayıcı bağlam menüsü][6]
5. Blob kapsayıcısını Yapıştır ve bağlam menüsünden - seçmek - istediğiniz istediğiniz "hedef" depolama hesabına sağ tıklayın **Blob kapsayıcısını Yapıştır**.

   ![Yapıştırma blob kapsayıcı bağlam menüsü][7]

## <a name="get-the-sas-for-a-blob-container"></a>Blob kapsayıcısı için SAS alma
[Paylaşılan erişim imzası (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar.
Başka bir deyişle, hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan, depolama hesabınızdaki nesnelere belirli bir süre için ve belirli bir izin kümesiyle sınırlı istemci izinleri verebilirsiniz.

Aşağıdaki adımlar, bir blob kapsayıcısı için SAS oluşturma işlemini göstermektedir:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, SAS almak istediğiniz blob kapsayıcısı içeren depolama hesabını genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstenen bir blob kapsayıcıya sağ tıklayın ve - seçin bağlam menüsünden - **paylaşılan erişim imzası Al**.

   ![Bağlam menüsü SAS alma][8]
5. **Paylaşılan Erişim İmzası** iletişim kutusunda ilkeyi, başlangıç ve sona erme tarihlerini, saat dilimini ve kaynak için istediğiniz erişim düzeylerini belirtin.

   ![SAS seçeneklerini Al][9]
6. SAS seçeneklerini belirtmeyi tamamladığınızda **Oluştur**’u seçin.
7. İkinci **paylaşılan erişim imzası** iletişim ardından görüntüler blob kapsayıcısı URL ve QueryStrings depolama kaynağına erişmek için kullanabileceğiniz ile birlikte listeleyen.
   Panoya kopyalamak istediğiniz URL’nin yanındaki **Kopyala** öğesini seçin.

   ![SAS URL'lerini kopyalayın][10]
8. İşiniz bittiğinde **Kapat**’ı seçin.

## <a name="manage-access-policies-for-a-blob-container"></a>Blob kapsayıcısı için erişim ilkelerini yönetme
Aşağıdaki adımları nasıl yönetileceği gösterilmektedir (ekleme ve kaldırma) bir blob kapsayıcısı için erişim ilkelerini:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, erişim ilkelerini yönetmek istediğiniz blob kapsayıcısı içeren depolama hesabını genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstenen blob kapsayıcısını seçip - bağlam menüsünden - **erişim ilkelerini Yönet**.

   ![Erişim ilkelerini yönet bağlam menüsü][11]
5. **Erişim ilkeleri** iletişim için seçilen blob kapsayıcısı zaten oluşturulmuş erişim ilkeleri listelenir.

   ![Erişim ilkesi seçenekleri][12]        
6. Erişim ilkesi yönetim görevine bağlı olarak aşağıdaki adımları izleyin:

   * **Yeni bir erişim ilkesi ekleme** - **Ekle**’yi seçin. Oluşturulduktan sonra, **Erişim İlkeleri** iletişim kutusunda yeni eklenen erişim ilkesi (varsayılan ayarlarla birlikte) gösterilir.
   * **Erişim ilkesini düzenleme** - istediğiniz düzenlemeleri yapın ve seçin **Kaydet**.
   * **Erişim ilkesini kaldırma** - Kaldırmak istediğiniz erişim ilkesinin yanındaki **Kaldır** öğesini seçin.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Blob kapsayıcısı genel erişim düzeyi
Varsayılan olarak, her bir blob kapsayıcısı "Hiçbir genel erişim" olarak ayarlanır.

Aşağıdaki adımlar, bir blob kapsayıcısı genel erişim düzeyini belirtmek nasıl göstermektedir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, erişim ilkelerini yönetmek istediğiniz blob kapsayıcısı içeren depolama hesabını genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstenen blob kapsayıcısını seçip - bağlam menüsünden - **genel erişim düzeyi ayarlamak**.

   ![Genel erişim düzeyi bağlam menüsü ayarlayın][13]
5. İçinde **kapsayıcısı genel erişim düzeyi ayarlamak** iletişim kutusunda, istediğiniz erişim düzeyini belirtin.

   ![Genel erişim düzeyi seçenekleri][14]
6. **Uygula**’yı seçin.

## <a name="managing-blobs-in-a-blob-container"></a>Bir blob kapsayıcısında BLOB'ları yönetme
Bir blob kapsayıcısını oluşturduktan sonra bu blob kapsayıcısına bir blob yüklemek, blob yerel bilgisayarınıza indirme, yerel bilgisayarınıza ve çok daha fazlasını blob açın.

Aşağıdaki adımlar, bir blob kapsayıcısı içinde BLOB'ları (ve klasörleri) yönetme göstermektedir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, yönetmek istediğiniz blob kapsayıcısı içeren depolama hesabını genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. Blob kapsayıcısı, görüntülemek istediğiniz çift tıklayın.
5. Ana bölmede blob kapsayıcının içeriğini görüntüler.

   ![Görünüm blob kapsayıcısı][3]
6. Ana bölmede blob kapsayıcının içeriğini görüntüler.
7. Gerçekleştirmek istediğiniz göreve bağlı olarak aşağıdaki adımları izleyin:

   * **Dosyalar bir blob kapsayıcısına yükleyin.**

     1. Ana bölmedeki araç çubuğunda **Karşıya Yükle**’yi ve ardından açılır listedeki **Dosyaları Karşıya Yükle**’yi seçin.

        ![Dosyaları karşıya yükleme menüsü][15]
     2. **Dosyaları Karşıya Yükle** iletişim kutusunda, **Dosyalar** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek karşıya yüklemek istediğiniz dosyaları belirleyin.

        ![Karşıya yükleme dosyaları seçenekleri][16]
     3. Türünü belirtin **Blob türü**. Bkz: [kapsayıcı oluşturma ve izinleri ayarlama](storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-the-container) daha fazla bilgi için.
     4. İsteğe bağlı olarak, seçili dosyalar karşıya yükleneceği bir hedef klasör belirtin. Hedef klasör mevcut değilse, oluşturulur.
     5. **Karşıya Yükle**’yi seçin.
   * **Klasör bir blob kapsayıcısına yükleyin.**

     1. Ana bölmedeki araç çubuğunda **Karşıya Yükle**’yi ve ardından açılır listedeki **Klasörü Karşıya Yükle**’yi seçin.

        ![Klasörü karşıya yükle menüsü][17]
     2. **Klasörü karşıya yükle** iletişim kutusunda, **Klasör** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek içeriklerini karşıya yüklemek istediğiniz klasörü belirleyin.

        ![Klasör Seçenekleri'ni yükleyin][18]
     3. Türünü belirtin **Blob türü**. Bkz: [kapsayıcı oluşturma ve izinleri ayarlama](storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-the-container) daha fazla bilgi için.
     4. İsteğe bağlı olarak, seçili klasörün içeriklerinin yükleneceği bir hedef klasör belirtin. Hedef klasör mevcut değilse, oluşturulur.
     5. **Karşıya Yükle**’yi seçin.
   * **Blob yerel bilgisayarınıza indirme**

     1. İndirmek istediğiniz blobu seçin.
     2. Ana bölmedeki araç çubuğunda **İndir**’i seçin.
     3. İçinde **indirilen blobun kaydedileceği yeri belirtmek** iletişim kutusunda, indirilen blobun istediğiniz konumu ve dosyaya vermek istediğiniz adı belirtin.  
     4. **Kaydet**’i seçin.
   * **Bir blobu yerel bilgisayarınızda açma**

     1. Açmak istediğiniz blobu seçin.
     2. Ana bölmedeki araç çubuğunda **Aç**’ı seçin.
     3. Blob indirilir ve blob'un temel alınan dosya türü ile ilişkili uygulama kullanılarak açılır.
   * **Bir blob Panoya Kopyala**

     1. Kopyalamak istediğiniz blobu seçin.
     2. Ana bölmedeki araç çubuğunda **Kopyala**’yı seçin.
     3. Sol bölmede başka bir blob kapsayıcısına gidin ve ana bölmede görüntülemek için çift tıklayın.
     4. Ana bölmedeki araç çubuğunda **Yapıştır** blob kopyası oluşturmasını ister.
   * **Blob silme**

     1. Silmek istediğiniz blobu seçin.
     2. Ana bölmedeki araç çubuğunda **Sil**’i seçin.
     3. Onay iletişim kutusunda **Evet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
* [En son Depolama Gezgini yayın notlarını ve videolarını](https://www.storageexplorer.com) görüntüleyin.
* [Azure bloblarını, tablolarını, kuyruklarını ve dosyalarını kullanarak uygulama oluşturma](https://azure.microsoft.com/documentation/services/storage/) hakkında bilgi edinin.

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
