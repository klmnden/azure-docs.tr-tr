---
title: "Depolama Gezgini’ni (Önizleme) Azure Dosya Depolama ile kullanma | Microsoft Belgeleri"
description: "Depolama Gezgini’ni (Önizleme) dosya paylaşımları ve dosyalarla çalışmak üzere kullanma hakkında bilgi alın."
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 964691758254531cb92a5b1cbe055ef61d25dba8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a>Depolama Gezgini’ni (Önizleme) Azure Dosya Depolama ile kullanma

Azure Dosya Depolama, standart Sunucu İleti Blogu (SMB) Protokolü kullanarak bulutta dosya paylaşımı sunan bir hizmettir. SMB 2.1 ve SMB 3.0 desteklenir. Azure File Storage, Azure’a dosya paylaşımı kullanan eski uygulamaları maliyetli yeniden yazdırmaya ihtiyaç duymadan ve hızla taşıyabilmenizi sağlar. Verileri genel olarak herkese açık kullanıma sunmak veya uygulama verilerini özel olarak depolamak için Dosya Depolama’yı kullanabilirsiniz. Bu makalede, dosya paylaşımları ve dosyalarla çalışmak üzere Depolama Gezgini’ni (Önizleme) nasıl kullanacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

- [Depolama Gezgini (önizleme) indirip yükleme](http://www.storageexplorer.com/)

- [Bir Azure depolama hesabı veya hizmetine bağlanma](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a>Dosya Paylaşımı oluşturma

Tüm dosyalar, basitçe dosyaların mantıksal bir gruplandırması olan dosya paylaşımında bulunmalıdır. Bir hesapta sınırsız sayıda dosya paylaşımı olabilir ve her paylaşım sınırsız sayıda dosya depolayabilir.

Aşağıdaki adımlar, Depolama Gezgini (Önizleme) içinde bir dosya paylaşımı oluşturma işlemini göstermektedir.

1. Depolama Gezgini’ni (Önizleme) açın.

2. Sol bölmede, Dosya Paylaşımını oluşturmak istediğiniz depolama hesabını genişletin

3. **Dosya Paylaşımları**’na sağ tıklayın ve bağlam menüsünden **Dosya Paylaşımı Oluştur**’u seçin.

    ![Dosya Paylaşımı Oluşturma](media/vs-azure-tools-storage-explorer-files/image1.png)

4. **Dosya Paylaşımları** klasörünün altında bir metin kutusu görünür. Dosya paylaşımınızın adını girin. Dosya paylaşımlarını adlandırmayla ilgili kural ve kısıtlamaların listesi için [Paylaşım adlandırma kuralları](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) bölümüne bakın.

    ![Paylaşımı adlandırma](media/vs-azure-tools-storage-explorer-files/image2.png)

5. Dosya paylaşımını oluşturma işlemi tamamlandığında **Enter** tuşuna veya iptal etmek için **Esc** tuşuna basın. Dosya paylaşımı başarıyla oluşturulduktan sonra, seçili depolama hesabının **Dosya Paylaşımları** klasörü altında gösterilir.

    ![Yeni paylaşım](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a>Dosya paylaşımının içeriğini görüntüleme

Dosya paylaşımları, dosya ve klasörler içerir (klasörler de dosya içerebilir).

Aşağıdaki adımlar, Depolama Gezgini’ndeki (Önizleme) bir dosya paylaşımının içeriğini görüntüleme işlemini göstermektedir:+

1. Depolama Gezgini’ni (Önizleme) açın.

2. Sol bölmede, görüntülemek istediğiniz dosya paylaşımını içeren depolama hesabını genişletin.

3. Depolama hesabının **Dosya Paylaşımları**’nı genişletin.

4. Görüntülemek istediğiniz dosya paylaşımına sağ tıklayın ve bağlam menüsünden **Aç**’ı seçin. Ayrıca, görüntülemek istediğiniz dosya paylaşımına çift tıklayabilirsiniz.

    ![Paylaşımı açma](media/vs-azure-tools-storage-explorer-files/image4.png)

5. Ana bölmede dosya paylaşımının içeriğini gösterilir.
    
    ![Paylaşımın içeriği](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a>Dosya paylaşımını silme

Dosya paylaşımları kolayca oluşturulabilir ve gerektiğinde silinebilir. (Dosyaları tek tek silmek için [Bir dosya paylaşımındaki dosyaları yönetme](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container) bölümüne bakın.)

Aşağıdaki adımlar, Depolama Gezgini (Önizleme) içinde bir dosya paylaşımını silme işlemini göstermektedir:

1. Depolama Gezgini’ni (Önizleme) açın.

2. Sol bölmede, görüntülemek istediğiniz dosya paylaşımını içeren depolama hesabını genişletin.

3. Depolama hesabının **Dosya Paylaşımları**’nı genişletin.

4. Silmek istediğiniz dosya paylaşımına sağ tıklayın ve bağlam menüsünden **Sil**’i seçin. Ayrıca **Sil** tuşuna basarak o anda seçili dosya paylaşımını silebilirsiniz.

    ![Sil](media/vs-azure-tools-storage-explorer-files/image6.png)

5. Onay iletişim kutusunda **Evet**’i seçin.
    
    ![Onay iletişim kutusu](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a>Dosya paylaşımını kopyalama

Depolama Gezgini’ni (Önizleme) kullanarak bir dosya paylaşımını panoya kopyalayabilir, ardından bu dosya paylaşımını başka bir depolama hesabına yapıştırabilirsiniz. (Dosyaları tek tek kopyalamak için [Bir dosya paylaşımındaki dosyaları yönetme](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container) bölümüne bakın.)

Aşağıdaki adımlar, dosya paylaşımını bir depolama hesabından diğerine kopyalama işlemini göstermektedir.

1. Depolama Gezgini’ni (Önizleme) açın.

2. Sol bölmede, kopyalamak istediğiniz dosya paylaşımını içeren depolama hesabını genişletin.

3. Depolama hesabının **Dosya Paylaşımları**’nı genişletin.

4. Kopyalamak istediğiniz dosya paylaşımına sağ tıklayın ve bağlam menüsünden **Dosya Paylaşımını Kopyala**’yı seçin.

    ![Dosya Paylaşımını Kopyala](media/vs-azure-tools-storage-explorer-files/image8.png)

5. Dosya paylaşımını yapıştırmak istediğiniz "hedef" depolama hesabına sağ tıklayın ve bağlam menüsünden **Dosya Paylaşımını Yapıştır**’ı seçin.

    ![Dosya Paylaşımını Yapıştır](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-the-sas-for-a-file-share"></a>Dosya paylaşımı için SAS alma

[Paylaşılan erişim imzası (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Başka bir deyişle, hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan, depolama hesabınızdaki nesnelere belirli bir süre için ve belirli bir izin kümesiyle sınırlı istemci izinleri verebilirsiniz.

Aşağıdaki adımlar, bir dosya paylaşımı için SAS oluşturma işlemini göstermektedir:+

1. Depolama Gezgini’ni (Önizleme) açın.

2. Sol bölmede, SAS almak istediğiniz dosya paylaşımını içeren depolama hesabını genişletin.

3. Depolama hesabının **Dosya Paylaşımları**’nı genişletin.

4. İstediğiniz dosya paylaşımına sağ tıklayın ve bağlam menüsünden **Paylaşılan Erişim İmzası Al**’ı seçin.

    ![Paylaşılan Erişim İmzası Al](media/vs-azure-tools-storage-explorer-files/image10.png)

5. **Paylaşılan Erişim İmzası** iletişim kutusunda ilkeyi, başlangıç ve sona erme tarihlerini, saat dilimini ve kaynak için istediğiniz erişim düzeylerini belirtin.

    ![SAS iletişim kutusu](media/vs-azure-tools-storage-explorer-files/image11.png)

6. SAS seçeneklerini belirtmeyi tamamladığınızda **Oluştur**’u seçin.

7. Bu durumda, depolama kaynağına erişmek için kullanabileceğiniz URL ve QueryStrings ile birlikte dosya paylaşımını listeleyen ikinci bir **Paylaşılan Erişim İmzası** iletişim kutusu görüntülenir. Panoya kopyalamak istediğiniz URL’nin yanındaki **Kopyala** öğesini seçin.
    
    ![İkinci SAS iletişim kutusu](media/vs-azure-tools-storage-explorer-files/image12.png)

8. İşiniz bittiğinde **Kapat**’ı seçin.

## <a name="manage-access-policies-for-a-file-share"></a>Bir dosya paylaşımı için Erişim İlkelerini yönetme

Aşağıdaki adımlar, bir dosya paylaşımı için erişim ilkelerini yönetme (ekleme ve kaldırma) işlemlerini gösterir:+. Erişim İlkeleri, kullanıcıların tanımlı bir süre boyunca Depolama Dosya kaynaklarına erişmek için kullanabileceği SAS URL’lerini oluşturmak için kullanılır.

1. Depolama Gezgini’ni (Önizleme) açın.

2. Sol bölmede, erişim ilkelerini yönetmek istediğiniz dosya paylaşımını içeren depolama hesabını genişletin.

3. Depolama hesabının **Dosya Paylaşımları**’nı genişletin.

4. İstediğiniz dosya paylaşımını ve bağlam menüsünden **Erişim İlkelerini Yönet**’i seçin.

    ![Erişim ilkelerini yönet bağlam menüsü](media/vs-azure-tools-storage-explorer-files/image13.png)

5. **Erişim İlkeleri** iletişim kutusunda, seçili dosya paylaşımı için daha önce oluşturulmuş erişim ilkeleri listelenir.
    
    ![Erişim İlkeleri](media/vs-azure-tools-storage-explorer-files/image14.png)

6. Erişim ilkesi yönetim görevine bağlı olarak aşağıdaki adımları izleyin:
    
    - **Yeni bir erişim ilkesi ekleme** - **Ekle**’yi seçin. Oluşturulduktan sonra, **Erişim İlkeleri** iletişim kutusunda yeni eklenen erişim ilkesi (varsayılan ayarlarla birlikte) gösterilir.

    - **Erişim ilkesini düzenleme** - İstediğiniz düzenlemeleri yapıp **Kaydet**’i seçin.

    - **Erişim ilkesini kaldırma** - Kaldırmak istediğiniz erişim ilkesinin yanındaki **Kaldır** öğesini seçin.

7. Daha önce oluşturduğunuz Erişim İlkesi'ni kullanarak yeni bir SAS URL'si oluşturun:
    
    ![SAS alma](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![SAS adı ve özellikleri](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a>Bir dosya paylaşımındaki dosyaları yönetme

Bir dosya paylaşımı oluşturduktan sonra, bu dosya paylaşımına dosya yükleyebilir, bir dosyayı yerel bilgisayarınıza indirebilir, yerel bilgisayarınızda bir dosyayı açabilir ve daha fazlasını yapabilirsiniz.

Aşağıdaki adımlar bir dosya paylaşımındaki dosyaları (ve klasörleri) yönetme işlemini göstermektedir.

1.  Depolama Gezgini’ni (Önizleme) açın.

2.  Sol bölmede, yönetmek istediğiniz dosya paylaşımını içeren depolama hesabını genişletin.

3.  Depolama hesabının **Dosya Paylaşımları**’nı genişletin.

4.  Görüntülemek istediğiniz dosya paylaşımına çift tıklayın.

5.  Ana bölmede dosya paylaşımının içeriğini gösterilir.

    ![Paylaşımın içeriği](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  Ana bölmede dosya paylaşımının içeriğini gösterilir.

7.  Gerçekleştirmek istediğiniz göreve bağlı olarak aşağıdaki adımları izleyin:

    - **Bir dosya paylaşımına dosya yükleme**

        a.  Ana bölmedeki araç çubuğunda **Karşıya Yükle**’yi ve ardından açılır listedeki **Dosyaları Karşıya Yükle**’yi seçin.

        ![Dosyaları karşıya yükleme](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        b. **Dosyaları Karşıya Yükle** iletişim kutusunda, **Dosyalar** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek karşıya yüklemek istediğiniz dosyaları belirleyin.

        ![Dosya ekleme](media/vs-azure-tools-storage-explorer-files/image19.png)

        c. **Karşıya Yükle**’yi seçin.

    - **Bir dosya paylaşımına klasör yükleme**
        
        a. Ana bölmedeki araç çubuğunda **Karşıya Yükle**’yi ve ardından açılır listedeki **Klasörü Karşıya Yükle**’yi seçin.

        ![Klasörü karşıya yükle menüsü](media/vs-azure-tools-storage-explorer-files/image20.png)

        b. **Klasörü karşıya yükle** iletişim kutusunda, **Klasör** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek içeriklerini karşıya yüklemek istediğiniz klasörü belirleyin.

        c. İsteğe bağlı olarak, seçili klasörün içeriklerinin yükleneceği bir hedef klasör belirtin. Hedef klasör mevcut değilse, oluşturulur.

        d. **Karşıya Yükle**’yi seçin.

    - **Bir dosyayı yerel bilgisayarınıza indirme**
        
        a. İndirmek istediğiniz dosyayı seçin.
        
        b. Ana bölmedeki araç çubuğunda **İndir**’i seçin.
        
        c. **İndirilen dosyanın kaydedileceği konumu seçin** iletişim kutusunda, dosyanın indirilmesini istediğiniz konumu ve dosyaya vermek istediğiniz adı belirtin.

        d. **Kaydet**’i seçin.

    - **Bir dosyayı yerel bilgisayarınızda açma**
        
        a.  Açmak istediğiniz dosyayı seçin.
        
        b.  Ana bölmedeki araç çubuğunda **Aç**’ı seçin.
        
        c.  Dosya indirilir ve dosyanın temel alınan dosya türü ile ilişkili uygulama kullanılarak açılır.

    - **Bir dosyayı panoya kopyalama**

        a. Kopyalamak istediğiniz dosyayı seçin.

        b. Ana bölmedeki araç çubuğunda **Kopyala**’yı seçin.

        c. Sol bölmede başka bir dosya paylaşımına gidin ve ana bölmede görüntülemek için çift tıklayın.

        d. Ana bölmedeki araç çubuğunda **Yapıştır**’ı seçerek dosyanın bir kopyasını oluşturun.

    - **Dosyayı silme**

        a. Silmek istediğiniz dosyayı seçin.

        b. Ana bölmedeki araç çubuğunda **Sil**’i seçin.

        c. Onay iletişim kutusunda **Evet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [En son Depolama Gezgini (Önizleme) sürüm notlarını ve videolarını](http://www.storageexplorer.com/) görüntüleyin.

- [Azure bloblarını, tablolarını, kuyruklarını ve dosyalarını kullanarak uygulama oluşturma](https://azure.microsoft.com/documentation/services/storage/) hakkında bilgi edinin.
