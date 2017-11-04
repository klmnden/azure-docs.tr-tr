---
title: "Depolama Gezgini (Önizleme) ile Azure Blob Storage kaynaklarını yönetme | Microsoft Docs"
description: "Azure Blob kapsayıcılar ve Bloblar Depolama Gezgini (Önizleme) ile yönetme"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: c23b87cca66df0834a31494be7d8657ff9f2a865
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Depolama Gezgini (Önizleme) ile Azure Blob Storage kaynaklarını yönetme
## <a name="overview"></a>Genel Bakış
[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) büyük miktarda herhangi bir yere HTTP veya HTTPS aracılığıyla erişilebilen metin veya ikili veriler gibi yapılandırılmamış veriyi depolamak için bir hizmettir.
Verileri genel olarak herkese açık kullanıma sunmak veya uygulama verilerini özel olarak depolamak için Blob Storage’ı kullanabilirsiniz. Bu makalede, blob kapsayıcıları ile çalışmak için Depolama Gezgini (Önizleme) ve BLOB'ları nasıl kullanacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* [Depolama Gezgini (önizleme) indirip yükleme](http://www.storageexplorer.com)
* [Bir Azure depolama hesabı veya hizmetine bağlanma](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Bir blob kapsayıcı oluşturun
Tüm BLOB'lar BLOB'ları yalnızca mantıksal bir gruplandırması olan bir blob kapsayıcısında bulunmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir ve her kapsayıcı sınırsız sayıda BLOB depolayabilirsiniz.

Aşağıdaki adımları Depolama Gezgini (Önizleme) içinde bir blob kapsayıcısını oluşturmak nasıl gösterilmektedir.

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede içinde blob kapsayıcısı oluşturmak istediğiniz depolama hesabı'nı genişletin.
3. Sağ **Blob kapsayıcıları**ve - bağlam menüsünden - seçin **Blob kapsayıcısı oluşturmak**.

   ![BLOB kapsayıcıları bağlam menüsü oluşturma][0]
4. Bir metin kutusu altında görünür **Blob kapsayıcıları** klasör. Blob kapsayıcısı için ad girin. Bkz: [kapsayıcı adlandırma kurallarını](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) bölümünde bir liste için kurallar ve blob kapsayıcı adlandırma kısıtlamaları.

   ![BLOB kapsayıcıları metin kutusu oluşturma][1]
5. Tuşuna **Enter** blob kapsayıcısı oluşturmak için yapıldığında veya **Esc** iptal etmek için. Blob kapsayıcısı başarıyla oluşturulduktan sonra uygulamanın altında görüntülenecek **Blob kapsayıcıları** seçili depolama hesabına için klasör.

   ![Blob kapsayıcısı oluşturuldu][2]

## <a name="view-a-blob-containers-contents"></a>Bir blob kapsayıcının içeriğini görüntüleme
BLOB kapsayıcıları, blobları ve (Ayrıca BLOB içerebilir) klasörleri içerir.

Aşağıdaki adımları Depolama Gezgini (Önizleme) bir blob kapsayıcısına içeriğini görüntülemek nasıl gösterilmektedir:

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, görüntülemek istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstediğiniz görüntülemek ve bağlam menüsünden - seçin - blob kapsayıcısını **açık Blob kapsayıcı Düzenleyicisi**.
   Ayrıca, görüntülemek istediğiniz blob kapsayıcısı çift tıklatabilirsiniz.

   ![Açık blob kapsayıcı Düzenleyicisi bağlam menüsü][19]
5. Ana bölmede blob kapsayıcının içeriğini görüntüler.

   ![BLOB kapsayıcı Düzenleyicisi][3]

## <a name="delete-a-blob-container"></a>Bir blob kapsayıcısından silin
BLOB kapsayıcıları kolayca oluşturulur ve gerektiğinde silinir. (Tek tek bloblar silmek için bölümüne başvurun nasıl görmek için [blob kapsayıcısı içinde BLOB'ları yönetme](#managing-blobs-in-a-blob-container).)

Aşağıdaki adımları Depolama Gezgini (Önizleme) içinde bir blob kapsayıcısını silmek nasıl gösterilmektedir:

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, görüntülemek istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstediğiniz silin ve bağlam menüsünden - seçin - blob kapsayıcısını **silmek**.
   Tuşlarına da basabilirsiniz **silmek** şu anda seçili blob kapsayıcısını silmek için.

   ![BLOB kapsayıcı bağlam menüsü Sil][4]
5. Onay iletişim kutusunda **Evet**’i seçin.

   ![BLOB kapsayıcı onay Sil][5]

## <a name="copy-a-blob-container"></a>Bir blob kapsayıcısını kopyalayın
Depolama Gezgini (Önizleme), bir blob kapsayıcısını panoya kopyalayın ve bu blob kapsayıcısı içinde başka bir depolama hesabı yapıştırın olanak sağlar. (Tek tek bloblar kopyalayın, bölümüne başvurun nasıl görmek için [blob kapsayıcısı içinde BLOB'ları yönetme](#managing-blobs-in-a-blob-container).)

Aşağıdaki adımlar bir depolama hesabından başka bir blob kapsayıcısını kopyalamak nasıl gösterilmektedir.

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, kopyalamak istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstediğiniz kopyalayın ve bağlam menüsünden - seçin - blob kapsayıcısını **kopyalama Blob kapsayıcısı**.

   ![Kopya blob kapsayıcı bağlam menüsü][6]
5. Blob kapsayıcısı yapıştırın ve bağlam menüsünden - seçmek - istediğiniz istenen "hedef" depolama hesabını sağ tıklatın **yapıştırın Blob kapsayıcısı**.

   ![Yapıştır blob kapsayıcı bağlam menüsü][7]

## <a name="get-the-sas-for-a-blob-container"></a>Blob kapsayıcısı için SAS alma
[Paylaşılan erişim imzası (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar.
Başka bir deyişle, hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan, depolama hesabınızdaki nesnelere belirli bir süre için ve belirli bir izin kümesiyle sınırlı istemci izinleri verebilirsiniz.

Aşağıdaki adımlar bir blob kapsayıcı için bir SAS oluşturmak nasıl gösterilmektedir:

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, SAS almak istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstenen blob kapsayıcısına sağ tıklayın ve - bağlam menüsünden - seçin **paylaşılan erişim imzası Al**.

   ![SAS bağlam menüsü Al][8]
5. **Paylaşılan Erişim İmzası** iletişim kutusunda ilkeyi, başlangıç ve sona erme tarihlerini, saat dilimini ve kaynak için istediğiniz erişim düzeylerini belirtin.

   ![SAS seçenekleri Al][9]
6. SAS seçeneklerini belirtmeyi tamamladığınızda **Oluştur**’u seçin.
7. İkinci bir **paylaşılan erişim imzası** iletişim ardından görüntüler, blob kapsayıcısı depolama kaynağa erişmek için kullanabileceğiniz QueryStrings ve URL ile birlikte listeler.
   Panoya kopyalamak istediğiniz URL’nin yanındaki **Kopyala** öğesini seçin.

   ![SAS URL'leri Kopyala][10]
8. İşiniz bittiğinde **Kapat**’ı seçin.

## <a name="manage-access-policies-for-a-blob-container"></a>Blob kapsayıcısı için erişim ilkelerini yönetme
Aşağıdaki adımları yönetmek nasıl çalışılacağını (ekleme ve kaldırma) erişim ilkeleri blob kapsayıcısı için:

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, erişim ilkelerini yönetmek istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstenen blob kapsayıcısı seçip - bağlam menüsünden - **erişim ilkelerini Yönet**.

   ![Erişim ilkelerini yönet bağlam menüsü][11]
5. **Erişim ilkeleri** iletişim zaten seçili blob kapsayıcısı için oluşturulan tüm erişim ilkeleri listeler.

   ![Erişim ilkesi seçenekleri][12]        
6. Erişim ilkesi yönetim görevine bağlı olarak aşağıdaki adımları izleyin:

   * **Yeni bir erişim ilkesi ekleme** - **Ekle**’yi seçin. Oluşturulduktan sonra, **Erişim İlkeleri** iletişim kutusunda yeni eklenen erişim ilkesi (varsayılan ayarlarla birlikte) gösterilir.
   * **Bir erişim ilkesi Düzenle** - istenen tüm düzenlemeleri yapın ve seçin **kaydetmek**.
   * **Erişim ilkesini kaldırma** - Kaldırmak istediğiniz erişim ilkesinin yanındaki **Kaldır** öğesini seçin.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Blob kapsayıcısı için genel erişim düzeyini ayarlayın
Varsayılan olarak, her blob kapsayıcısı "Genel erişim yok" olarak ayarlanır.

Aşağıdaki adımlar bir blob kapsayıcısı için genel erişim düzeyini belirtmek amacıyla nasıl gösterilmektedir.

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, erişim ilkelerini yönetmek istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. İstenen blob kapsayıcısı seçip - bağlam menüsünden - **genel erişim düzeyi ayarlanan**.

   ![Genel erişim düzeyi bağlam menüsü Ayarla][13]
5. İçinde **kapsayıcısı genel erişim düzeyi ayarlanan** iletişim kutusunda, istediğiniz erişim düzeyini belirtin.

   ![Genel erişim düzeyi seçeneklerini ayarlama][14]
6. **Uygula**’yı seçin.

## <a name="managing-blobs-in-a-blob-container"></a>Blob kapsayıcısı içinde BLOB'ları yönetme
Bir blob kapsayıcısını oluşturduktan sonra blob kapsayıcıya bir blob karşıya yükleme, blob yerel bilgisayarınıza indirin, yerel bilgisayarınıza ve çok daha fazlasını bir blob açın.

Aşağıdaki adımları blob kapsayıcısı içinde BLOB'ları (ve klasörler) yönetmek nasıl gösterilmektedir.

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, yönetmek istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin.
3. Depolama hesabının genişletin **Blob kapsayıcıları**.
4. Görüntülemek istediğiniz blob kapsayıcısı çift tıklayın.
5. Ana bölmede blob kapsayıcının içeriğini görüntüler.

   ![Görünüm blob kapsayıcısı][3]
6. Ana bölmede blob kapsayıcının içeriğini görüntüler.
7. Gerçekleştirmek istediğiniz göreve bağlı olarak aşağıdaki adımları izleyin:

   * **Bir blob kapsayıcıya dosyaları karşıya yükleme**

     1. Ana bölmedeki araç çubuğunda **Karşıya Yükle**’yi ve ardından açılır listedeki **Dosyaları Karşıya Yükle**’yi seçin.

        ![Dosya menüsü karşıya yükle][15]
     2. **Dosyaları Karşıya Yükle** iletişim kutusunda, **Dosyalar** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek karşıya yüklemek istediğiniz dosyaları belirleyin.

        ![Karşıya yükleme seçenekleri dosyaları][16]
     3. Türünü belirtin **Blob türü**. Makaleyi [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) çeşitli blob türleri arasındaki farklar açıklanmaktadır.
     4. İsteğe bağlı olarak, seçili dosyaları karşıya yüklenecek bir hedef klasör belirtin. Hedef klasör mevcut değilse, oluşturulur.
     5. **Karşıya Yükle**’yi seçin.
   * **Bir klasör bir blob kapsayıcıya karşıya yükle**

     1. Ana bölmedeki araç çubuğunda **Karşıya Yükle**’yi ve ardından açılır listedeki **Klasörü Karşıya Yükle**’yi seçin.

        ![Klasörü karşıya yükle menüsü][17]
     2. **Klasörü karşıya yükle** iletişim kutusunda, **Klasör** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek içeriklerini karşıya yüklemek istediğiniz klasörü belirleyin.

        ![Klasör Seçenekleri karşıya yükle][18]
     3. Türünü belirtin **Blob türü**. Makaleyi [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) çeşitli blob türleri arasındaki farklar açıklanmaktadır.
     4. İsteğe bağlı olarak, seçili klasörün içeriklerinin yükleneceği bir hedef klasör belirtin. Hedef klasör mevcut değilse, oluşturulur.
     5. **Karşıya Yükle**’yi seçin.
   * **Bir blob yerel bilgisayarınıza indirin**

     1. İndirmek istediğiniz blob seçin.
     2. Ana bölmedeki araç çubuğunda **İndir**’i seçin.
     3. İçinde **indirilen blob kaydedileceği yeri belirtin** iletişim kutusunda, indirilen blob istediğiniz konumu ve onu vermek istediğiniz adı belirtin.  
     4. **Kaydet**’i seçin.
   * **Yerel bilgisayarınızda bir blob açın**

     1. Açmak istediğiniz blob seçin.
     2. Ana bölmedeki araç çubuğunda **Aç**’ı seçin.
     3. Blob indirilir ve blob'un temel alınan dosya türü ile ilişkili uygulama kullanılarak açılır.
   * **Bir blob Panoya Kopyala**

     1. Kopyalamak istediğiniz blob seçin.
     2. Ana bölmedeki araç çubuğunda **Kopyala**’yı seçin.
     3. Sol bölmede, başka bir blob kapsayıcısına gidin ve ana bölmede görüntülemek için çift tıklayın.
     4. Ana bölmede ait araç çubuğunda seçin **Yapıştır** blob bir kopyasını oluşturun.
   * **Bir blob Sil**

     1. Silmek istediğiniz blob seçin.
     2. Ana bölmedeki araç çubuğunda **Sil**’i seçin.
     3. Onay iletişim kutusunda **Evet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
* [En son Depolama Gezgini (Önizleme) sürüm notlarını ve videolarını](http://www.storageexplorer.com) görüntüleyin.
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
