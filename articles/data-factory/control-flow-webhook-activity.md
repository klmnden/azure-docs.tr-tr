---
title: Azure Data factory'de Web kancası etkinliği | Microsoft Docs
description: Web kancası etkinlik ekli veri kümesini kullanıcının belirttiği belirli ölçütlerle doğrulayıncaya kadar işlem hattının yürütme devam etmez.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: shlo
ms.openlocfilehash: 0b8b892f02e54c3b0ddb155af97ce63ff115bb1f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58523127"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Azure Data factory'de Web kancası etkinliği
İşlem hattı yürütme, özel kod aracılığıyla denetlemek için bir web kancası etkinliği kullanabilirsiniz. Müşterilerin, Web kancası etkinliği kullanarak, bir uç noktasını çağırmak ve bir geri çağırma URL'si geçirin. İşlem hattı çalıştırması sonraki etkinliğe geçmeden önce çağrılacak geri arama bekler.

## <a name="syntax"></a>Sözdizimi

```json

{
    "name": "MyWebHookActivity",
    "type": "WebHook",
    "typeProperties": {
        "method": "POST",
        "url": "<URLEndpoint>",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": {
            "key": "value"
        },
        "timeout": "00:03:00",
        "authentication": {
            "type": "ClientCertificate",
            "pfx": "****",
            "password": "****"
        }
    }
}

```


## <a name="type-properties"></a>Tür özellikleri



Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Web kancası etkinliği adı | String | Evet |
type | Ayarlanmalıdır **Web kancası**. | String | Evet |
method | Hedef uç nokta için REST API yöntemi. | dize. Desteklenen türler: 'POSTA' | Evet |
url | Hedef uç nokta ve yolu | Dize (veya dizenin ifadenin resulttype'ı ile). | Evet |
Üst bilgileri | Gönderilen istek için üstbilgiler. Örneğin, türü ve dili, bir istek üzerinde ayarlanan için: "üst": {"Accept-Language": "en-us", "Content-Type": "application/json"}. | Dize (veya dizenin ifadenin resulttype'ı ile) | Evet, Content-type üst bilgisi gereklidir. "headers":{ "Content-Type":"application/json"} |
body | Uç noktaya gönderdi yükünü temsil eder. | Gövde geçirilen geri için geri arama URI'si geçerli bir JSON olmalıdır. İstek yükü şemayı [istek yükü şeması](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23request-payload-schema&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=ljUZv5csQQux2TT3JtTU9ZU8e1uViRzuX5DSNYkL0uE%3D&amp;reserved=0) bölümü. | Evet |
kimlik doğrulaması | Uç noktasını çağırmak için kullanılan kimlik doğrulama yöntemi. "Temel" veya "ClientCertificate." türleri desteklenir Daha fazla bilgi için [kimlik doğrulaması](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23authentication&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=GdA1%2Fh2pAD%2BSyWJHSW%2BSKucqoAXux%2F4L5Jgndd3YziM%3D&amp;reserved=0) bölümü. Kimlik doğrulama gerekli değilse, bu özellik hariç tutun. | Dize (veya dizenin ifadenin resulttype'ı ile) | Hayır |
timeout | Etkinliğin ne kadar süre bekleyeceğini &#39;callbackuri&#39; çağrılacak. Ne kadar süreyle etkinlik 'çağrılacak callbackuri için' bekler. Varsayılan değer: 10mins ("00: 10:00"). Format is Timespan i.e. d.hh:mm:ss | String | Hayır |

## <a name="additional-notes"></a>Ek notlar

Azure Data Factory, ek bir özellik "callbackuri" gövdesinde url uç noktasına geçer ve belirtilen zaman aşımı değeri önce çağrılacak bu URI beklediği. URI çağrılır, etkinlik 'Zaman aşımına uğradı' durumuyla başarısız olur.

Web kancası etkinliği kendisini, yalnızca başarısız olduğunda özel uç noktasına çağrı başarısız olur. Herhangi bir hata iletisi, geri çağırma gövdesine eklenen ve sonraki bir etkinliği kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın:

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
