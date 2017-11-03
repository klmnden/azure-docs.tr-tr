---
title: "Azure geçiş özel durumlar ve bunların nasıl çözüleceği | Microsoft Docs"
description: "Azure geçiş özel durumlar ve bunları çözmenize yardımcı olacak önerilen eylemleri listesini alın."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 83ff97b59e428e7b617a7f5d1011ca5ddf3060b6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-relay-exceptions"></a>Azure geçiş özel durumlar

Bu makalede Azure geçiş API'ları tarafından oluşturulan bazı özel durumlar listelenir. Bu başvuru değiştirilebilir, bu nedenle geri Güncelleştirmeleri denetle.

## <a name="exception-categories"></a>Özel durum kategorileri

Geçiş API'leri aşağıdaki kategorilere ayrılır özel durumları oluşturur. Ayrıca özel durumları çözmek yardımcı olacak önerilen eylemleri listelenmiştir.

*   **Kodlama hatası kullanıcı**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Genel eylem**: devam etmeden önce kodu düzeltmeye çalışır.
*   **Kurulum/yapılandırma hatası**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Genel eylem**: yapılandırmanızı gözden geçirin. Gerekirse, yapılandırmasını değiştirin.
*   **Geçici özel durumları**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Genel eylem**: işlemi yeniden deneyin veya kullanıcılara bildirin.
*   **Diğer özel durumlar**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Genel eylem**: özel durum türü özgüdür. Aşağıdaki bölümde bulunan tabloya bakın. 

## <a name="exception-types"></a>Özel durum türleri

Aşağıdaki tabloda, ileti özel durum türleri ve bunların nedenleri listelenmektedir. Özel durumları çözmek yardımcı olacak önerilen eylemleri Not alır.

| **Özel durum türü** | **Açıklama** | **Önerilen eylem** | **Otomatik veya hemen yeniden deneme sırasında unutmayın** |
| --- | --- | --- | --- |
| [Zaman aşımı](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu istenen işlemi tarafından denetlenen belirtilen süre içinde yanıt vermedi [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). Sunucu istenen işlemi tamamlanmış. Bu, ağ veya diğer altyapı gecikmeler nedeniyle gerçekleşebilir. |Tutarlılık için sistem durumunu denetleyin ve sonra gerekirse, yeniden deneyin. Bkz: [TimeoutException](#timeoutexception). |Yeniden deneme bazı durumlarda yardımcı olabilir; yeniden deneme mantığı kodu ekleyin. |
| [Geçersiz işlem](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemi, sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi Ayrıntılar için bkz. |Kod ve belgelerine bakın. İstenen işlem geçerli olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [İşlem iptal edildi.](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapatılmış, iptal veya atılmış bir nesne üzerinde bir işlemi başlatmak için bir deneme yapılır. Nadir durumlarda ortam işlem zaten atıldı. |Kodu kontrol edip bırakılmış bir nesne üzerinde işlem çağrılmaz emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [Yetkisiz erişim](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesne bir belirteç alınamadı, belirteci geçersiz veya işlemi gerçekleştirmek için gerekli talep belirteci içermiyor. |Belirteç sağlayıcı doğru değerlerle oluşturulduğundan emin olun. Erişim denetimi hizmeti yapılandırmasını denetleyin. |Yeniden deneme bazı durumlarda yardımcı olabilir; yeniden deneme mantığı kodu ekleyin. |
| [Bağımsız değişken özel](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Bağımsız değişkeni Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Bağımsız değişkeni aralık dışında](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Aşağıdakilerden birini veya birkaçını oluştu:<br />Yönteme sağlanan bir veya daha fazla bağımsız değişken geçersiz.<br /> URI tarafından sağlanan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [oluşturma](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) bir veya daha fazla yol kesimi içeriyor.<br />URI şeması tarafından sağlanan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [oluşturma](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) geçersiz. <br />Özellik değeri, 32 KB'den büyük. |Çağrıyı yapan kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden deneme yardımcı olmayacaktır. |
| [Sunucu meşgul](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Hizmet şu anda isteğinizi mümkün değil. |İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. |İstemci, belirli bir zaman aralığından sonra yeniden. Yeniden deneme sonuçları farklı bir özel durum, bu özel durum yeniden deneme davranışını kontrol edin. |
| [Kotası aşıldı](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Mesajlaşma varlığı, izin verilen boyut üst sınırına ulaştı. |Alan, varlık veya onun alt sıralar iletilerini alarak varlıkta oluşturun. Bkz: [QuotaExceededException](#quotaexceededexception). |İletileri zarfında kaldırılmışsa yeniden deneme yardımcı olabilir. |
| [İleti boyutu aşıldı](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükünü 256 KB'lik sınırını aşıyor. Toplam ileti boyutu 256 KB'lik sınırını olduğuna dikkat edin. Toplam ileti boyutu, Sistem özellikleri ve tüm Microsoft .NET ek yükü içerebilir. |İleti yükü azaltın ve işlemi yeniden deneyin. |Yeniden deneme yardımcı olmayacaktır. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) belirli bir varlık için bir kota aşıldı gösterir.

Geçiş için bu özel durumun sarmalar [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), bu uç nokta için dinleyicileri üst sınırını aştı olduğunu gösterir. Bu belirtilen **MaximumListenersPerEndpoint** özel durum iletisi değeri.

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlem işlem zaman aşımından daha uzun sürüyor gösterir. 

Değerini denetleyin [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) özelliği. Bu sınır basarsa ayrıca neden olabilecek bir [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

Geçiş için geçiş gönderen bağlantısı ilk açtığınızda, zaman aşımı özel durumları alabilirsiniz. Bu özel durumun iki yaygın nedenleri şunlardır:

*   [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) değeri (hatta tarafından bir saniyenin,) çok küçük olabilir.
* Bir şirket içi geçiş dinleyicisi yanıt vermiyor olabilir (veya dinleyicileri yeni istemci bağlantılarını kabul etmesini engelleyen güvenlik duvarı kuralları sorunlarla karşılaşabilirsiniz) ve [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) değerinden yaklaşık 20 saniye bir değerdir.

Örnek:

```
'System.TimeoutException’: The operation did not complete within the allotted timeout of 00:00:10.
The time allotted to this operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Olası nedenler
Bu hatanın sık karşılaşılan nedenleri şunlardır:

*   **Yanlış yapılandırma**
    
    İşlem zaman aşımı işletimsel koşul için çok küçük olabilir. İstemci SDK işlemi zaman aşımı için varsayılan değer 60 saniyedir. Kodunuzu değeri çok küçük bir şey için ayarlanmış olup olmadığını denetleyin. CPU kullanımı ve ağ durumunu bir işlemin tamamlanması için geçen süreyi etkileyebilir unutmayın. İşlem zaman aşımı çok düşük bir değere ayarlı değil iyi bir fikirdir.
*   **Geçici hizmet hatası**

    Bazen, geçiş hizmeti isteklerini işleme gecikme karşılaşabilirsiniz. Bu, örneğin, yüksek trafik dönemlerde gerçekleşebilir. Bu gerçekleşirse, işlem başarılı olana kadar bir gecikmeden sonra işlemi yeniden deneyin. Aynı işlem birden çok denemeden sonra başarısız olmaya devam ederse denetleyin [Azure hizmet durumu site](https://azure.microsoft.com/status/) hizmet kesintileri bilinen vardır olmadığını görmek için.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure geçiş SSS](relay-faq.md)
* [Geçiş ad alanı oluşturma](relay-create-namespace-portal.md)
* [Azure geçişi ve .NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Azure geçişi ve düğüm kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

