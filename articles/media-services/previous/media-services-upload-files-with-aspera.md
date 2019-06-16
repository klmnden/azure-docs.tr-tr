---
title: Aspera kullanarak Azure Media Services hesabına dosya yükleme | Microsoft Docs
description: Bu öğreticide, Azure üzerinde **Aspera Server On Demand** hizmetini kullanarak bir Media Services hesabıyla ilişkili depolama hesabına dosya yükleme adımları gösterilmektedir.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: ''
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: e522883da7fddad44741599107f2dbc4c99aace6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60827028"
---
# <a name="upload-files-into-a-media-services-account-using-the-aspera-server-on-demand-service-on-azure"></a>Azure üzerinde Aspera Server On Demand hizmetini kullanan bir Media Services hesabına dosya yükleme 

## <a name="overview"></a>Genel Bakış

**Aspera** çok yüksek hızlı bir dosya aktarım yazılımıdır. Azure için **Aspera Server On Demand**, büyük dosyaların doğrudan Azure Blob nesne depolama alanına yüksek hızda yüklenmesini ve indirilmesini sağlar. **İsteğe Bağlı Aspera** hakkında bilgi için [Aspera Bulut](http://cloud.asperasoft.com/) sitesine bakın. 
  
Azure için **Aspera Server On Demand**, [Azure Market](https://azure.microsoft.com/marketplace/)’ten satın alınabilir. **Aspera Server On Demand** satın alma işlemini tamamlamak için lütfen Azure Market’te Windows Live ID’nizle oturum açın.

Bu öğreticide, Azure üzerinde **Aspera Server On-Demand** hizmetini kullanarak bir Media Services hesabıyla ilişkili depolama hesabına dosya yükleme adımları gösterilmektedir. 

Azure işlevlerinin Aspera ve Media Services ile kullanımını gösteren bir örneği [burada](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest) bulabilirsiniz.

>[!NOTE]
>Azure Media Services medya işlemcileri (MP’ler) ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. Dosya boyutu sınırlaması hakkında ayrıntılı bilgi için [bu](media-services-quotas-and-limitations.md) makaleye bakın.
>

## <a name="prerequisites"></a>Önkoşullar 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Windows Live ID
* Bir [Azure hesabı](https://azure.microsoft.com). Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/). 
* Bir [Azure Media Services hesabı](media-services-portal-create-account.md).

## <a name="purchase-aspera-on-demand-for-azure"></a>Azure için İsteğe Bağlı Aspera yazılımını satın alın

Azure için İsteğe Bağlı Azure satın alma işlemini tamamlamak için Azure Market’te oturum açtıktan sonra aşağıdaki basit adımları izleyin.

1. Aspera ifadesini arayın ve 'Server On Demand' öğesini seçin.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. Abonelik planlarını gözden geçirin ve 'Kaydol' öğesine tıklayın

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. Server on-Demand aboneliğinize ilişkin ayrıntıları doldurun.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. **Fiyatlandırma Katmanı**’na tıklayın ve alt panelden istediğiniz aylık hacmi seçin. **Plan ayrıntıları** panelinde **Tamam**’ı seçin. Ardından **Fiyatlandırma Katmanınızı seçin** panelinde **Seç**’e tıklayın.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. Alt panelde yasal koşulları görüntüleyip kabul etmek için **Yasal koşullar** öğesine tıklayın. Yasal koşulları gözden geçirdikten sonra **Satın al**’a tıklayın.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. **Oluştur**’a tıklayarak satın alma işlemini tamamlayın.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. Azure panosu bu hizmeti sağladığını duyurur.  Sağlama ile işlem tamamlandıktan sonra kaynaklarınızda hizmetin adını arayarak yeni aboneliği bulabilirsiniz. Hizmeti bulduktan sonra çift tıklayarak hizmet yönetim portalını başlatın.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. Aspera yönetim portalını başlatın. Yeni Aspera hizmetinizi bulduktan sonra hizmete tıklayarak yönetim portalına erişim elde edebilirsiniz.  Yeni bir panel başlatılır. Bu yeni panelden yeni hizmetinizin **Kaynak Adı** seçeneğine tıklamanız gerekir.  Aşağıdaki ekran görüntüsünde kaynak adı 'AsperaTransferDemo' şeklindedir. Kaynak adına tıkladığınızda başka bir panel başlatılır. Yeni başlatılan bu panelde bir 'Yönet' bağlantısı görürsünüz. Aspera yönetim portalını başlatmak için 'Yönet' bağlantısına tıklayın.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. Yönet bağlantısına tıkladığınızda, hizmete erişmek için gerekli olan kayıt sayfasına ulaşırsınız.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. Bu noktada, erişim anahtarları oluşturabileceğiniz, Aspera istemci ve lisanslarını indirebileceğiniz, kullanımı görüntüleyebileceğiniz ve API’ler hakkında bilgi alabileceğiniz Aspera hizmet yönetim portalına erişebilirsiniz.

    Aşağıdaki ekran görüntüsünde erişim oluşturma işlemi gösterilmektedir. 

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    Aşağıdaki ekran görüntüsünde, portaldaki kullanım raporlama arabirimleri gösterilmektedir. 

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a>Aspera ile dosyaları karşıya yükleme

1. Aspera istemci yazılımını indirip yükleyin:
    
    * [Tarayıcı eklentisi](https://downloads.asperasoft.com/connect2/)
    * [Zengin istemci](https://downloads.asperasoft.com/en/downloads/2)

2. İlk aktarımınızı yapın. Aspera istemcisini kullanarak Aspera aktarım hizmetiyle aktarım yapmak için aşağıdaki işlemleri tamamlamanız gerekir: 

   1. Aspera portalını kullanarak bir erişim anahtarı oluşturun.  
   2. Aspera istemcisini indirin, yükleyin ve lisans ekleyin (yazılım Aspera portalında bulunabilir).  

      >[!NOTE]
      >Yapılandırma bilgileri için lütfen Aspera istemci kılavuzunu okuyun.
    
   3. [Azure portalı](https://portal.azure.com/) kullanarak Azure Medya Hesabınız ile ilişkili depolama hesabınızın bazı bilgilerini alın. Almanız gereken bilgiler şunlardır: ad, anahtar ve içeriğinizi yerleştirmek istediğiniz depolama blobu kapsayıcısının adı. 

       * Portaldan depolama bilgilerini almak için: Depolama hesabınızı bulun, Erişim anahtarları’na tıklayın ve hesabınızın adı ile anahtarını kopyalayın.
       * Kapsayıcı adını almak için: Depolama hesabınızı bulun, **Bloblar**’ı ve sonra da içerik bilgisini yüklemek istediğiniz kapsayıcının adını seçin. 

      Aşağıdaki ekran görüntüsünde, 'Azure' depolama türünü, kimlik bilgilerini ve blob kapsayıcısını belirtmeniz gereken Aspera istemcisi **Bağlantı Yöneticisi** gösterilmektedir.

      ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a>Kaynaklar

Bu makalede aşağıdaki kaynaklardan bahsedilmiştir. 

* [Connect Tarayıcı Eklentisi](https://downloads.asperasoft.com/connect2/)
* [Connect Kılavuzu](https://downloads.asperasoft.com/en/documentation/8)
* [Aspera İstemcisi](https://downloads.asperasoft.com/en/downloads/2)
* [İstemci Kılavuzu](https://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a>Sonraki adımlar

Artık [BLOB kopyalayabilirsiniz bir depolama hesabından AMS hesabına](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

