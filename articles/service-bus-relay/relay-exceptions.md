---
title: Azure geçiş özel durumları ve bunların nasıl çözüleceğine | Microsoft Docs
description: Azure geçiş özel durumları ve bunları çözmenize yardımcı olacak önerilen eylemler listesi.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: spelluru
ms.openlocfilehash: fe8f057443b978e70e7cdd2591affd455fefdca8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60749045"
---
# <a name="azure-relay-exceptions"></a>Azure geçiş özel durumları

Bu makalede, Azure geçişi API'leri tarafından oluşturulan bazı özel durumlar listelenir. Bu başvuru değişebilir, bu nedenle geri Güncelleştirmeleri denetle.

## <a name="exception-categories"></a>Özel durum kategorisi

Geçiş API'leri, aşağıdaki kategorilere ayrılır özel durumlar oluşturun. Ayrıca özel durumları çözmek yardımcı olacak önerilen eylemleri listelenmiştir.

*   **Kodlama hatası kullanıcı**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Genel eylem**: Devam etmeden önce bu kodu düzeltmek bu seçeneği deneyin.
*   **Kurulumu/yapılandırma hatası**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Genel eylem**: Yapılandırmanızı gözden geçirin. Gerekirse, yapılandırmasını değiştirin.
*   **Geçici özel durumlar**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Genel eylem**: İşlemi yeniden deneyin veya kullanıcılara bildirin.
*   **Diğer özel durumlar**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Genel eylem**: Belirli özel durum türü. Aşağıdaki bölümde bulunan tabloya bakın. 

## <a name="exception-types"></a>Özel durum türleri

Aşağıdaki tabloda, Mesajlaşma özel durum türlerini ve bunlara ait nedenlerin listeler. Özel durumları çözmek yardımcı olacak önerilen eylemleri Not alır.

| **Özel durum türü** | **Açıklama** | **Önerilen eylem** | **Otomatik veya hemen yeniden deneme sırasında dikkat edin.** |
| --- | --- | --- | --- |
| [zaman aşımı](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu istenen işlemi tarafından denetlenen belirtilen süre içinde yanıt vermedi [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). Sunucu istenen işlemi tamamlanmış olabilir. Bu, ağ veya diğer altyapı gecikmeler nedeniyle oluşabilir. |Tutarlılık için sistem durumunu denetleyin ve ardından, gerekirse, yeniden deneyin. Bkz: [TimeoutException](#timeoutexception). |Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [Geçersiz işlem](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemi, sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi ayrıntıları için bkz. |Kod ve belgelere bakın. İstenen işlem geçerli olduğundan emin olun. |Yeniden deneme yardımcı olmaz. |
| [İşlem iptal edildi](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapalı, durduruldu veya atılmış bir nesne üzerinde bir işlem çağırmak için bir deneme yapılır. Nadiren de olsa, ortam işlem zaten atıldı. |Kodu kontrol edip atılan nesneye işlemleri çağırma kullanılamaz olduğundan emin olun. |Yeniden deneme yardımcı olmaz. |
| [Yetkisiz erişim](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesne bir belirteci alınamadı, belirteci geçersiz veya işlemi gerçekleştirmek için gerekli talep belirteci içermiyor. |Belirteç sağlayıcısı doğru değerlerle oluşturulduğundan emin olun. Erişim denetimi hizmetinin yapılandırmasını denetleyin. |Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [Bağımsız değişken özel durumunu](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Bağımsız değişken Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Bağımsız değişkeni aralık dışında](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Bir veya daha fazlasını oluştu:<br />Yöntemine sağlanan bir veya daha fazla bağımsız değişken geçersiz.<br /> Sağlanan URI [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) bir veya daha fazla yol kesimi içeriyor.<br />Sağlanan URI düzeni [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) geçersiz. <br />Özellik değeri, 32 KB'den daha büyük. |Çağıran kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden deneme yardımcı olmaz. |
| [Server Busy](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Hizmet şu anda isteğinizi mümkün değil. |İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. |İstemci, belirli bir süre sonra yeniden. Yeniden deneme sonuçları farklı bir özel durum, o özel durumu yeniden deneme davranışını kontrol edin. |
| [Kota aşıldı](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Mesajlaşma varlığı, izin verilen boyut üst sınırına. |Alanı varlık içinde varlık veya onun alt iletilerini alma oluşturun. Bkz: [QuotaExceededException](#quotaexceededexception). |İletiler sırada kaldırdıysanız, yeniden deneme yardımcı olabilir. |
| [İleti boyutu aşıldı](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükü 256 KB'lik sınırını aşıyor. İleti boyutu 256 KB'lık sınırı olduğunu unutmayın. İleti boyutu, Sistem özellikleri ve herhangi bir Microsoft .NET yükü içerebilir. |İleti yükü azaltın, ardından işlemi yeniden deneyin. |Yeniden deneme yardımcı olmaz. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

Bu özel durum geçişi için sarmalar [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), dinleyicileri sayısı için bu endpoint aşıldığını gösterir. Bu belirtilen **MaximumListenersPerEndpoint** özel durum iletisi değeri.

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlemi zaman aşımı süresinden daha uzun sürdüğünü gösterir. 

Değerini kontrol edin [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) özelliği. Bu sınıra ulaşılması da neden olabilir bir [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

Geçiş için geçiş gönderen bağlantısı ilk kez açtığınızda, zaman aşımı özel durumları alabilirsiniz. Bu özel durumun sık karşılaşılan iki nedeni vardır:

*   [Opentimeout =](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) değeri çok küçük (hatta tarafından bir saniyenin varsa) olabilir.
* Bir şirket içi geçiş dinleyicisinin yanıt vermiyor olabilir (veya dinleyicileri yeni istemci bağlantılarını kabul etmesini engelleyen bir güvenlik duvarı kuralları sorunların karşılaşabilirsiniz) ve [opentimeout =](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) küçüktür yaklaşık 20 saniye değerdir.

Örnek:

```
'System.TimeoutException’: The operation did not complete within the allotted timeout of 00:00:10.
The time allotted to this operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Olası nedenler
Bu hatanın yaygın nedenleri şunlardır:

*   **Yanlış yapılandırma**
    
    İşlem zaman aşımı işletimsel koşul için çok küçük olabilir. İstemci SDK'sı işlemi zaman aşımı'için varsayılan değer 60 saniyedir. Kodunuzda değeri çok küçük bir şeye ayarlanmış olup olmadığını denetleyin. Bir işlemin tamamlanması için geçen süreyi CPU kullanımı ve ağ durumunu etkileyebilecek unutmayın. İşlem zaman aşımı çok düşük bir değere ayarlı değil iyi bir fikirdir.
*   **Geçici bir hizmet hatası**

    Bazen, geçiş hizmeti isteklerinin işlenmesinde gecikmelere karşılaşabilirsiniz. Bu, örneğin, yüksek trafiği dönemlerinde gerçekleşebilir. Bu meydana gelirse, işlem başarılı olana kadar bir gecikmeden sonra işleminizi yeniden deneyin. Aynı işlem sonra birden fazla girişimde başarısız olmaya devam ederse, kontrol [Azure hizmet durumu sitesine](https://azure.microsoft.com/status/) vardır bilinen hizmet kesintileri olup olmadığını görmek için.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure geçişi ile ilgili SSS](relay-faq.md)
* [Bir geçiş ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET ve Azure geçişi ile çalışmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Azure geçişi ve Node ile çalışmaya başlama](relay-hybrid-connections-node-get-started.md)

