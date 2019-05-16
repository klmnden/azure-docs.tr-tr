---
title: "Azure Hızlı Başlangıç: Azure portalı kullanarak olay hub'ı oluşturma | Microsoft Docs"
description: Bu hızlı başlangıçta Azure portalı kullanarak Azure olay hub'ı oluşturmayı ve .NET Standard SDK kullanarak olay gönderip almayı öğreneceksiniz.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/23/2019
ms.author: shvija
ms.openlocfilehash: 83e33ffa2854b92718828ae870b82431993fac24
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65603521"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir olay hub'ı oluşturma
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta [Azure portalı](https://portal.azure.com) kullanarak olay hub'ı oluşturacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- [Visual Studio 2019)](https://www.visualstudio.com/vs) veya üzeri.
- [.NET Standard SDK'sı](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya üzeri.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Kaynak grubu, Azure kaynakları için mantıksal bir koleksiyondur. Tüm kaynaklar bir kaynak grubuna dağıtılır ve buradan yönetilir. Bir kaynak grubu oluşturmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüden **Kaynak grupları**'na tıklayın. Daha sonra **Ekle**'ye tıklayın.

   ![Kaynak grupları - Ekle düğmesi](./media/event-hubs-quickstart-portal/resource-groups1.png)

2. İçin **abonelik**, kaynak grubu oluşturmak istediğiniz Azure aboneliğini seçin.
3. Benzersiz bir türü **kaynak grubu adı**. Sistem, adın seçili Azure aboneliğinde var olup olmadığını kontrol eder.
4. Seçin bir **bölge** kaynak grubu için.
5. Seçin **gözden geçir + Oluştur**.

   ![Kaynak grubu - oluştur](./media/event-hubs-quickstart-portal/resource-groups2.png)
6. Üzerinde **gözden geçir + Oluştur** sayfasında **Oluştur**. 

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Event Hubs ad alanı, tam etki alanı adının başvurduğu, içinde bir veya daha fazla olay hub'ı oluşturduğunuz benzersiz bir kapsam kapsayıcısı sağlar. Portalı kullanarak kaynak grubunuzda bir ad alanı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

1. Azure portalda ekranın sol üst köşesindeki **Kaynak oluştur**'a tıklayın.
2. Seçin **tüm hizmetleri** seçin ve soldaki menüden **yıldız (`*`)** yanındaki **Event Hubs** içinde **Analytics** kategorisi. Onaylayın **Event Hubs** eklenir **Sık Kullanılanlar** sol gezinti menüsünde. 
    
   ![Event Hubs araması yapın](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
3. Seçin **Event Hubs** altında **Sık Kullanılanlar** sol gezinti menüsünde, seçip **Ekle** araç.

   ![Araç çubuğu düğmesi ekleme](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
4. Üzerinde **ad alanı oluşturma** sayfasında, aşağıdaki adımları uygulayın:
    1. Ad alanı için bir ad girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
    2. Fiyatlandırma katmanını (temel veya standart) seçin.
    3. Seçin **abonelik** ad alanı oluşturmak istediğiniz.
    4. Seçin bir **konumu** ad alanı.
    5. **Oluştur**’u seçin. Sistemin kaynakları tam olarak sağlaması için birkaç dakika beklemeniz gerekebilir.

       ![Olay hub'ı ad alanı oluşturma](./media/event-hubs-quickstart-portal/create-event-hub1.png)
5. Yenileme **Event Hubs** olay hub'ı ad alanını görmek için sayfayı. Uyarılar, olay hub'ı oluşturma durumunu kontrol edebilirsiniz. 

    ![Olay hub'ı ad alanı oluşturma](./media/event-hubs-quickstart-portal/event-hubs-refresh.png)
6. Ad alanını seçin. Giriş sayfasını görürsünüz, **Event Hubs ad alanı** portalında. 

   ![Ad alanı için giriş sayfası](./media/event-hubs-quickstart-portal/namespace-home-page.png)
    
## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Ad alanında bir olay hub'ı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

1. Olay hub'ları Namespace sayfasında **Event Hubs** soldaki menüde.
1. Pencerenin en üstündeki **+ Olay Hub’ı** seçeneğine tıklayın.
   
    ![Olay Hub'ı Ekle - düğme](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. Olay hub'ınız için bir ad yazın, ardından **Oluştur**’a tıklayın.
   
    ![Olay hub'ı oluşturma](./media/event-hubs-quickstart-portal/create-event-hub5.png)
4. Uyarılar, olay hub'ı oluşturma durumunu kontrol edebilirsiniz. Olay hub'ı oluşturulduktan sonra bu olay hub'ları listesinde aşağıdaki görüntüde gösterildiği gibi görürsünüz:

    ![Olay hub'ı oluşturuldu](./media/event-hubs-quickstart-portal/event-hub-created.png)

Tebrikler! Portalı kullanarak bir Event Hubs ad alanı ve bu ad alanının içinde bir olay hub'ı oluşturdunuz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir kaynak grubu, bir Event Hubs ad alanı ve bir olay hub'ı oluşturdunuz. Olayları gönderme (veya) bir olay hub'ından olay alma hakkında adım adım yönergeler için bkz **olayları alıp göndermek** öğreticiler: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca reecive)](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png
