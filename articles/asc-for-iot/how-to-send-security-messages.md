---
title: Güvenlik iletilerinizi Azure Güvenlik Merkezi'ne IOT Önizleme için gönderin | Microsoft Docs
description: IOT için Azure Güvenlik Merkezi'ni kullanarak güvenlik iletilerinizi göndereceğinizi öğrenin.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: c611bb5c-b503-487f-bef4-25d8a243803d
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: a91a3538a9c176e3c76e351eb53eb84decc85938
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65200526"
---
# <a name="send-security-messages-sdk"></a>SDK'sı güvenlik ileti gönderme

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu nasıl yapılır kılavuzunda, toplamak ve cihazınız için IOT Aracısı bir ASC kullanmadan güvenlik iletileri göndermek seçtiğinizde Azure Güvenlik Merkezi (ASC) için IOT hizmet özellikleri açıklar ve bunun nasıl yapılacağı açıklanır.  

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * Gönderme güvenlik iletisi API kullanınC#
> * C için gönderilen güvenlik iletiyi API kullanma

## <a name="asc-for-iot-capabilities"></a>ASC IOT özellikleri için

IOT için ASC işleyebilir ve gönderilen verileri uyumlu olduğu sürece her türlü güvenlik ileti veri [ASC IOT şeması](https://aka.ms/iot-security-schemas) ve iletiyi bir güvenlik uyarısı ayarlanır.

## <a name="security-message"></a>Güvenlik Uyarısı

ASC IOT için aşağıdaki ölçütleri kullanarak bir güvenlik uyarısı tanımlar:
- Azure IOT C ile ileti gönderildiyse /C# SDK'sı
- İleti için uyuyorsa [güvenlik ileti şeması](https://aka.ms/iot-security-schemas)
- İleti göndermeden önce bir güvenlik uyarısı olarak ayarlandıysa

Her güvenlik uyarısı gibi gönderenin meta verileri içeren `AgentId`, `AgentVersion`, `MessageSchemaVersion` ve güvenlik olaylarının bir listesi.
Şema olay türleri dahil olmak üzere güvenlik iletinin geçerli ve gerekli özellikleri tanımlar.

[!NOTE]
> Gönderilen iletileri şemasıyla uyumlu değil göz ardı edilir. Yoksayılan ileti şu anda depolanmadığı gibi veri gönderimi başlatmadan önce şemayı doğruladığınızdan emin olun. 
> Gönderilen iletileri, ayarlı değil Azure IOT C kullanarak bir güvenlik uyarısı /C# SDK değil yönlendirilecek ASC IOT ardışık düzen için

## <a name="valid-message-example"></a>Geçerli ileti örneği

Aşağıdaki örnek, geçerli güvenlik ileti nesnesi gösterir. İleti meta verileri ve bir örnek içeren `ProcessCreate` güvenlik olayı.

Bir güvenlik uyarısı ayarlamalı ve IOT için gönderilen bu ileti ASC tarafından işlenir.

```json
"AgentVersion": "0.0.1",
"AgentId": "e89dc5f5-feac-4c3e-87e2-93c16f010c25",
"MessageSchemaVersion": "1.0",
"Events": [
    {
        "EventType": "Security",
        "Category": "Triggered",
        "Name": "ProcessCreate",
        "IsEmpty": false,
        "PayloadSchemaVersion": "1.0",
        "Id": "21a2db0b-44fe-42e9-9cff-bbb2d8fdf874",
        "TimestampLocal": "2019-01-27 15:48:52Z",
        "TimestampUTC": "2019-01-27 13:48:52Z",
        "Payload":
            [
                {
                    "Executable": "/usr/bin/echo",
                    "ProcessId": 11750,
                    "ParentProcessId": 1593,
                    "UserName": "nginx",
                    "CommandLine": "./backup .htaccess"
                }
            ]
    }
]
```

## <a name="send-security-messages"></a>Güvenlik ileti gönderme 

ASC IOT aracısının kullanmadan kullanarak güvenlik ileti gönderme [Azure IOT C# cihaz SDK'sını](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview) veya [Azure IOT C cihaz SDK'sını](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview).

Cihazlarınızı IOT ASC tarafından işlenmek için cihaz verilerini göndermek için doğru ASC'ye IOT işleme işlem hattı ve yönlendirme iletileri işaretlemek için aşağıdaki API'leri birini kullanın. Bu şekilde gönderilen iletileri işlenir ve IOT hem IOT hub'ının içinden olarak ya da Azure Güvenlik Merkezi güvenlik öngörüleri ASC içinde olarak görüntülenen. 

Doğru üstbilgiyle işaretlenmiş olsa bile gönderilen tüm verileri de uymalıdır [ASC IOT ileti şeması için](https://aka.ms/iot-security-schemas). 

### <a name="send-security-message-api"></a>Güvenlik iletisi API Gönder

**Güvenlik ileti gönderme** API'si şu anda C ve C#.  

#### <a name="c-api"></a>C# API’si

```cs

private static async Task SendSecurityMessageAsync(string messageContent)
{
    ModuleClient client = ModuleClient.CreateFromConnectionString("<connection_string>");
    Message  securityMessage = new Message(Encoding.UTF8.GetBytes(messageContent));
    securityMessage.SetAsSecurityMessage();
    await client.SendEventAsync(securityMessage);
}
```

#### <a name="c-api"></a>C API

```c
bool SendMessageAsync(IoTHubAdapter* iotHubAdapter, const void* data, size_t dataSize) {
 
    bool success = true;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;
 
    messageHandle = IoTHubMessage_CreateFromByteArray(data, dataSize);
 
    if (messageHandle == NULL) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubMessage_SetAsSecurityMessage(messageHandle) != IOTHUB_MESSAGE_OK) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubModuleClient_SendEventAsync(iotHubAdapter->moduleHandle, messageHandle, SendConfirmCallback, iotHubAdapter) != IOTHUB_CLIENT_OK) {
        success = false;
        goto cleanup;
    }
 
cleanup:
    if (messageHandle != NULL) {
        IoTHubMessage_Destroy(messageHandle);
    }
 
    return success;
}
 
static void SendConfirmCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback) {
    if (userContextCallback == NULL) {
        //error handling
        return;
    }
 
    if (result != IOTHUB_CLIENT_CONFIRMATION_OK){
        //error handling
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
- ASC IOT hizmeti için okuma [genel bakış](overview.md)
- ASC hakkında daha fazla bilgi edinmek için IOT [mimarisi](architecture.md)
- Etkinleştirme [hizmeti](quickstart-onboard-iot-hub.md)
- Okuma [SSS](resources-frequently-asked-questions.md)
- Nasıl erişeceğinizi öğrenin [ham güvenlik verileri](how-to-security-data-access.md)
- Anlamak [önerileri](concept-recommendations.md)
- Anlamak [uyarıları](concept-security-alerts.md)
