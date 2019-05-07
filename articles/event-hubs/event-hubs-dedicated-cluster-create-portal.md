---
title: Azure hızlı başlangıç - Azure portalını kullanarak Event Hubs adanmış kümesi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, Azure portalını kullanarak bir Azure Event Hubs kümesi oluşturmayı öğrenin.
services: event-hubs
documentationcenter: ''
author: xurui203
manager: ''
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 05/02/2019
ms.author: xurui
ms.openlocfilehash: ffdcd3e6314e659443630f74f8f17261659f1246
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158514"
---
# <a name="quickstart-create-a-dedicated-event-hubs-cluster-preview-using-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir ayrılmış Event Hubs kümesi (Önizleme) oluşturma 
Event Hubs kümeleri, En zorlu akış ihtiyaçları olan müşteriler için tek kiracılı dağıtımları sunar. Bu tek kiracılı teklifi, garantili % 99,99 SLA'sı var ve yalnızca adanmış bizim fiyatlandırma katmanında kullanılabilir. Bir olay hub'ları kümesi garantili kapasitesi ve subsecond gecikme süresi ile saniyede milyonlarca giriş olabilir. Ayrılmış bir küme içinde oluşturulan ad alanları ve olay hub'ları ve daha fazlasını standart teklifi, ancak herhangi bir giriş sınır tüm özellikleri içerir. Ayrıca popüler içerir [Event Hubs yakalama](event-hubs-capture-overview.md) özelliği otomatik olarak batch ve günlük verilerini sağlayarak, hiçbir ek ücret ödemeden akışları için [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md) veya [Azure Data Lake depolama 1 kuşağı](../data-lake-store/data-lake-store-overview.md). Event Hubs adanmış kümeleri hakkında daha fazla bilgi için bkz. [ayrılmış Event Hubs'a genel bakış](event-hubs-dedicated-overview.md).

Adanmış kümeleri sağlanan ve tarafından faturalandırılır **kapasite birimleri (cu)**, CPU ve bellek kaynakları önceden ayrılmış bir miktarı. Her küme için 1, 2, 4, 8, 12, 16 veya 20 cu satın alabilirsiniz. Bir kümenin ölçeğini için önce bir CU ile küme oluşturma, ardından gönderme bir [destek bileti](#submit-a-support-request-for-your-dedicated-cluster). 

Bu hızlı başlangıçta, Azure portalını kullanarak Event Hubs adanmış kümesi oluşturacaksınız.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir Azure hesabı. Biri yoksa [bir hesabı satın](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) başlamadan önce. Bu özellik, ücretsiz bir Azure hesabı ile desteklenmez. 
- [Visual Studio](https://visualstudio.microsoft.com/vs/) 2017 Güncelleştirmesi (sürüm 15.3, 26730.01) 3 veya üzeri.
- [.NET Standard SDK'sı](https://dotnet.microsoft.com/download), sürüm 2.0 veya üzeri.
- [Bir kaynak grubu oluşturduk](../event-hubs/event-hubs-create.md#create-a-resource-group).

## <a name="create-an-event-hubs-dedicated-cluster"></a>Event Hubs adanmış kümesi oluşturma
Event Hubs adanmış küme bir veya daha fazla ad alanı oluşturabileceğiniz, tam etki alanı adı tarafından başvurulan bir benzersiz bir kapsam kapsayıcı sağlar. Self Servis portalı Önizleme aşamasında deneyimi, desteklenen bölgelerde CU bir küme oluşturabilirsiniz. Bir küme bir CU büyük gerekiyorsa, bir küme ile bir CU ilk oluşturun ve sonra kümenizi ölçeklendirme isteği gönderin. 

Azure portalını kullanarak kaynak grubunuzda bir küme oluşturmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com)seçin **+ kaynak Oluştur** sol gezinti menüsünde.
2. Tür **Event Hubs kümeleri** arama çubuğunda, ENTER tuşuna basın.
3. Üzerinde **küme oluşturma** sayfasında, aşağıdaki adımları uygulayın:
    1. Girin bir **küme adı**. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
    2. Seçin **abonelik** kümeyi oluşturmak istediğiniz.
    3. Seçin **kaynak grubu** kümeyi oluşturmak istediğiniz.
    4. Seçin bir **konumu** küme için. Tercih edilen bir bölgeye griyse gönderme bir [destek isteği](#submit-a-support-request-for-your-dedicated-cluster).
    5. Seçin **sonraki: Etiketleri** sayfanın alt kısmındaki düğmesi. Sistemin kaynakları tam olarak sağlaması için birkaç dakika beklemeniz gerekebilir.

        ![Event Hubs kümesi - temel sayfa oluşturma](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-basics-page.png)
4. Üzerinde **etiketleri** sayfasında, aşağıdaki adımları uygulayın:
    1. Girin bir **adı** ve **değer** eklemek istediğiniz etiketi. Bu adım **isteğe bağlıdır**.  
    2. Seçin **gözden geçir + Oluştur** düğmesi.

        ![Event Hubs kümesi sayfası - etiketler sayfası oluşturma](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-tags-page.png)
5. Üzerinde **gözden geçir + Oluştur** sayfasında, ayrıntıları gözden geçirin ve seçin **Oluştur**. 

    ![Event Hubs kümesi sayfası - gözden geçirmesi oluşturma + sayfası oluşturma](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-review-create-page.png)

## <a name="create-a-namespace-and-event-hub-within-a-cluster"></a>Bir küme içinde bir ad alanı ve olay hub'ı oluşturma

1. Bir küme içinde bir ad alanı oluşturmak için **Event Hubs kümesi** sayfa seçin, kümeniz **+ Namespace** üstteki menüden.

    ![Küme Yönetimi sayfası - ad alanı düğmesi ekleme](./media/event-hubs-dedicated-cluster-create-portal/cluster-management-page-add-namespace-button.png)
2. Ad alanı sayfası oluşturma üzerinde aşağıdaki adımları uygulayın:
    1. Girin bir **ad alanı adı**.  Sistem adı olup olmadığını denetler.
    2. Ad alanına aşağıdaki özellikleri devralır:
        1. Abonelik Kimliği
        2. Kaynak Grubu
        3. Location
        4. Küme Adı
    3. Seçin **Oluştur** ad alanı oluşturmak için. Artık kümenize yönetebilirsiniz.  

        ![Küme sayfasında ad alanı oluşturma](./media/event-hubs-dedicated-cluster-create-portal/create-namespace-cluster-page.png)
3. Ad alanınız oluşturulduktan sonra [bir olay hub'ı oluşturma](event-hubs-create.md#create-an-event-hub) gibi bir ad alanı içinde normalde oluşturursunuz. 


## <a name="submit-a-support-request-for-your-dedicated-cluster"></a>Adanmış kümeniz için bir destek isteği gönderin

Bir destek talebi göndermek için şu adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com)seçin **Yardım + Destek** sol menüden.
2. Seçin **+ yeni destek isteği** destek menüsünde.
3. Destek sayfasında, aşağıdaki adımları izleyin:
    1. İçin **sorun türü**seçin **teknik** aşağı açılan listeden.
    2. **Abonelik** bölümünde aboneliğinizi seçin.
    3. İçin **hizmet**seçin **Hizmetlerim**ve ardından **Event Hubs**.
    4. İçin **kaynak**, zaten varsa, kümenizi seçin, aksi takdirde seçin **genel soru/kaynak kullanılamıyor**.
    5. İçin **sorun türü**seçin **kota**.
    6. İçin **sorun alt**, aşağı açılan listeden değerleri aşağıdakilerden birini seçin:
        1. Seçin **ayrılmış SKU için istek** bölgenizde desteklenmesi özelliğin istemek için.
        2. Seçin **ölçeği Artır veya ayrılmış küme aşağı ölçeklendirme isteği** adanmış kümenizin ölçeğini artırabilir veya istiyorsanız. 
    7. İçin **konu**, sorunu açıklayın.

        ![Destek bileti sayfası](./media/event-hubs-dedicated-cluster-create-portal/support-ticket.png)

 ## <a name="delete-a-dedicated-cluster"></a>Ayrılmış küme silme
1. Kümeyi silmek için işaretleyin **Sil** üstteki menüden.
2. Kümeyi silmek istiyorsanız onaylayan bir ileti görüntülenir.
3. Tür **küme adını** seçip **Sil** kümeyi silmek için.

    ![Küme sayfayı Sil](./media/event-hubs-dedicated-cluster-create-portal/delete-cluster-page.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir olay hub'ları kümesi oluşturuldu. Gönderin ve bir olay hub'ından olay alma ve bir Azure depolama veya Azure Data Lake Store olaylarını yakalamak adım adım yönergeler için aşağıdaki öğreticilere bakın:

- [.NET Core üzerinde olayları alıp göndermek](event-hubs-dotnet-standard-getstarted-send.md)
- [Event Hubs yakalama özelliğini etkinleştirmek için Azure portalını kullanma](event-hubs-capture-enable-through-portal.md)
- [Azure olay hub'ları için Apache Kafka kullanın](event-hubs-for-kafka-ecosystem-overview.md)
