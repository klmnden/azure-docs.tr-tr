---
title: "Hatalı oluşturulmuş giriş olaylarında Azure akış analizi için sorun giderme | Microsoft Docs"
description: "My girişinde hangi olay nasıl anlarım veri Stream Analytics işinde sorunu neden"
keywords: 
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: Kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/19/2018
ms.author: sngun
ms.openlocfilehash: 2b4f82bae20c3cb398848a89715bfdc1316e1ba1
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="troubleshoot-for-malformed-input-events"></a>Hatalı oluşturulmuş giriş olayları için sorun giderme

Giriş akışı akış analizi işinin hatalı biçimlendirilmiş iletileri içeriyorsa, serileştirme sorunlarına neden olur. Hatalı biçimlendirilmiş iletileri yanlış serileştirme eksik parantez gibi bir JSON nesnesi ya da hatalı zaman damgası biçimi içerir. Akış analizi işi hatalı bir ileti aldığında, iletiyi bırakır ve bir uyarı ile kullanıcıyı uyarır. Bir uyarı simgesi gösterilir **girişleri** Stream Analytics işiniz parçasına:

![Girişleri döşeme](media/stream-analytics-malformed-events/inputs_tile.png)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

1. Giriş döşemeye gidin ve Uyarıları görüntülemek için tıklatın.
2. Giriş Ayrıntıları kutucuğu sorun hakkında ayrıntılar uyarılarla kümesini görüntüler. Aşağıdaki örnek uyarı iletisi, bölüm, uzaklık ve sıra numaraları uyarı iletisi gösterir hatalı biçimlendirilmiş JSON verilerini olduğu. 

   ![Uzaklık uyarı iletisi](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Hatalı biçimde JSON veri almak için aşağıdaki kod parçacığını çalıştırın. Bu kod bloğu bölüm kimliği, uzaklığı ve verileri yazdırır okur. Tam örnekten alabilirsiniz [GitHub örnekleri depo](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Verileri okuma sonra çözümlemek ve seri hale getirme biçimi düzeltin.

```csharp
static void PrintMessages(string partitionId, long offset, int numberOfEvents)
        {
            EventHubReceiver receiver;
            
            try
            {
                foreach (var e in receiver.Receive(numberOfEvents, TimeSpan.FromMinutes(1)))
                {
                    Console.WriteLine(Encoding.UTF8.GetString(e.GetBytes()));
                    Console.WriteLine("----");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.ToString());
                return;
            }

            receiver.Close();
        }
```
## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
