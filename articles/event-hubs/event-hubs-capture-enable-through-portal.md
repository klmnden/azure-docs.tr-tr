---
title: "Portal üzerinden Azure Event Hubs Yakalama özelliğini etkinleştirme | Microsoft Docs"
description: "Azure portalını kullanarak Event Hubs Yakalama özelliğini etkinleştirin."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 5420c1cdefa99ff30320dd757e04aa0cafc792bc
ms.contentlocale: tr-tr
ms.lasthandoff: 08/29/2017

---

# <a name="enable-event-hubs-capture-using-the-azure-portal"></a>Azure portalını kullanarak Event Hubs Yakalama özelliğini etkinleştirme

Azure [Event Hubs Yakalama][capture-overview] özelliği, Event Hubs’dan akış verilerini seçtiğiniz [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/) alanına veya [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) hesabına otomatik olarak iletmenizi sağlar.

[Azure portalını](https://portal.azure.com) kullanarak olay hub'ı oluşturma sırasında Yakalama özelliğini yapılandırabilirsiniz. Verileri bir Azure [Blob depolama](https://azure.microsoft.com/services/storage/blobs/) kapsayıcısına veya bir [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) hesabına alabilirsiniz.

Daha fazla bilgi için bkz. [Event Hubs Capture'a genel bakış][capture-overview].

## <a name="capture-data-to-an-azure-storage-account"></a>Azure Depolama hesabına veri alma  

Bir olay hub’ı oluşturduğunuzda **Olay Hub'ı oluştur** portal ekranında **Aç** düğmesine tıklayarak Yakalama özelliğini etkinleştirebilirsiniz. Ardından **Yakalama Sağlayıcısı** kutusunda **Azure Storage**’a tıklayarak bir Depolama Hesabı ve kapsayıcı belirtirsiniz. Event Hubs Yakalama özelliği, depolama ile hizmetten hizmete kimlik doğrulama kullandığından depolama bağlantı dizesi belirtmenize gerek yoktur. Kaynak seçici, depolama hesabınız için kaynak URI'sini otomatik olarak seçer. Azure Resource Manager kullanıyorsanız bu URI'yi dize olarak açıkça belirtmeniz gerekir.

Zaman penceresi varsayılan olarak 5 dakikadır. En düşük değer 1, en yüksek değer ise 15'tir. **Boyut** penceresi 10-500 MB aralığındadır.

![][1]

## <a name="capture-data-to-an-azure-data-lake-store-account"></a>Azure Data Lake Store hesabına veri alma

Azure Data Lake Store'a veri almak için bir Data Lake Store hesabı ve bir olay hub'ı oluşturun:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Azure Data Lake Store hesabı ve klasörleri oluşturma

1. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](../data-lake-store/data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayarak bir Data Lake Store hesabı oluşturun. 
2. [Azure Data Lake Store hesabında klasör oluşturma](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) bölümündeki yönergeleri izleyerek bu hesap altında bir klasör oluşturun.
3. Data Lake Store hesabı sayfanızda, **Veri Gezgini**'ne tıklayın.
4. **Erişim**’e tıklayın.
5. **Ekle**'ye tıklayın.
6. **Ada veya e-postaya göre ara** kutusuna **Microsoft.EventHubs** yazıp bu seçeneği belirleyin. 
7. **İzinler** sekmesi görünür. İzinleri aşağıdaki resimde gösterildiği gibi ayarlayın:

    ![][6]

8. **Tamam** düğmesine tıklayın.
9. Şimdi, hedef klasöre gidip ve klasör adını tıklatarak kök klasörde bir klasör oluşturun.
10. **Erişim**’e tıklayın.
11. **Ekle**'ye tıklayın.
12. **Ada veya e-postaya göre ara** kutusuna **Microsoft.EventHubs** yazıp bu seçeneği belirleyin.
13. **İzinler** sekmesi tekrar görünür. İzinleri aşağıdaki resimde gösterildiği gibi ayarlayın:

    ![][5]

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. Olay hub'ının oluşturduğunuz Azure Data Lake Store ile aynı Azure aboneliğinde olması gerektiğini unutmayın. **Event Hub'ı oluştur** portal sayfasının **Yakalama** bölümünde **Açık** düğmesine tıklayarak olay hub'ını oluşturun. 
2. **Event Hub'ı oluştur** portal sayfasında, **Yakalama Sağlayıcısı** kutusundan **Azure Data Lake Store**’u seçin.
3. **Data Lake Store’u seçin** kısmında, daha önce oluşturduğunuz Data Lake Store hesabını belirtin ve **Data Lake Yolu** alanına, oluşturduğunuz veri klasörünün yolunu girin.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>Mevcut bir olay hub'ında Yakalama özelliğini yapılandırma

Event Hubs ad alanlarında mevcut olan olay hub'ları üzerinde Yakalama özelliğini yapılandırabilirsiniz. Yakalama özelliğini mevcut bir olay hub'ında etkinleştirmek veya Yakalama ayarlarınızı değiştirmek için ad alanına tıklayarak **Temel Bileşenler** ekranını yükleyin, ardından Yakalama ayarını etkinleştirmek veya değiştirmek istediğiniz olay hub'ına tıklayın. Son olarak, açık sayfanın **Özellikler** bölümüne tıklayın ve Yakalama ayarlarını aşağıdaki şekilde gösterildiği gibi düzenleyin:

### <a name="azure-blob-storage"></a>Azure Blob Depolama

![][2]

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a>Sonraki adımlar

- Olay hub’larını yakalama hakkında daha fazla bilgi için bkz. [Event Hubs Yakalama özelliğine genel bakış][capture-overview].
- Dilerseniz Azure Resource Manager şablonlarını kullanarak da Event Hubs Yakalama özelliğini yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager şablonu kullanarak Yakalama özelliğini etkinleştirme](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).

[capture-overview]: event-hubs-capture-overview.md
