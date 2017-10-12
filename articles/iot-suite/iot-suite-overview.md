---
title: "Microsoft Azure IoT Paketi’ne genel bakış | Microsoft Docs"
description: "Azure IoT Paketi, verileri toplamak, çözümlemek ve depolamak, görselleştirmeler sağlamak ve diğer sistemlerle tümleştirmek için önceden yapılandırılmış nesnelerin internetini nasıl sağladığına genel bakış."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfa8dbbd0b1d943a9eb7a042df0bac25189d9ac9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-azure-iot-suite"></a>Azure IoT Paketi’ne Genel Bakış

Azure nesnelerin interneti (IoT) hizmetleri çok çeşitli özellikler sunar. Bu kurumsal sınıf hizmetler şunları yapmanızı sağlar:

* Cihazlardan veri toplama
* Hareket halinde veri akışı çözümleme
* Büyük veri gruplarını depolama ve sorgulama
* Gerçek zamanlı ve geçmiş verileri görselleştirme
* Arka ofis sistemleriyle tümleştirme
* Cihazlarınızı yönetme

Bu özellikleri sunmak için, Azure IoT Paketi birçok Azure hizmetini özel uzantılarla birlikte *önceden yapılandırılmış çözümler* olarak paketledi. Bu önceden yapılandırılmış çözümler, IoT çözümlerinizi sunmak için geçirmeniz gereken süreyi azaltmak üzere genel IoT çözümü düzenlerinin temel uygulamalarıdır. [IoT yazılım geliştirme setlerini][lnk-sdks] kullanarak, kendi gereksinimlerinizi karşılamak için bu çözümleri özelleştirebilir ve genişletebilirsiniz. Ayrıca bu çözümleri, yeni IoT çözümleri geliştirirken örnekler ya da şablonlar olarak da kullanabilirsiniz.

Aşağıdaki video Azure IoT paketine giriş sağlar:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT Paketindeki Azure IoT Hizmetleri
Önceden yapılandırılmış çözümler genellikle aşağıdaki hizmetleri kullanır:

* Azure IoT paketinin çekirdeği [Azure IoT Hub][lnk-iot-hub] hizmetidir. Bu hizmet cihaz-bulut arası ve bulut-cihaz arası ileti gönderme özellikleri sağlar ve buluta ve diğer temel IoT Paketi hizmetlerine ağ geçidi görevi görür. Hizmet cihazınızdan ölçekte mesajlar almanızı ve cihazlarınıza komutlar göndermenizi sağlar. Hizmet ayrıca [cihazlarınızı yönetmenizi][lnk-device-management] de sağlar. Örneğin hub'a bağlı bir veya daha fazla cihazı yapılandırabilir, yeniden başlatabilir veya fabrika ayarlarına döndürebilirsiniz.
* [Azure Stream Analytics][lnk-asa] hareket halinde veri çözümleme sağlar. IoT Paketi, gelen telemetri işlemek, toplama gerçekleştirmek ve olayları algılamak için bu hizmeti kullanır. Önceden yapılandırılmış çözümler, meta veriler ya da cihazlardan alınan komut yanıtları gibi verileri içeren bilgi iletilerini işlemek için de akış analizini kullanır. Çözümler cihazlarınızdan gelen iletileri işlemek ve bu iletileri diğer cihazlara göndermek için Stream Analytics kullanır.
* [Azure Depolama][lnk-azure-storage] ve [Azure Cosmos DB][lnk-document-db] veri depolama özellikleri sağlar. Önceden yapılandırılmış çözümler, telemetri depolamak ve çözümleme için kullanılabilir hale getirmek üzere blob depolamayı kullanır. Çözümler cihaz meta verilerini depolamak ve çözümlerin cihaz yönetimi özelliklerini etkinleştirmek için Cosmos DB kullanır.
* [Azure Web Apps][lnk-web-apps] ve [Microsoft Power BI][lnk-power-bi] veri görselleştirme özellikleri sağlar. Power BI esnekliği, IoT Paketi verilerini kullanan kendi etkileşimli panolarınızı hızlı bir şekilde oluşturmanızı sağlar.

Tipik bir IoT çözüm mimarisine genel bakış için bkz. [Microsoft Azure ve Nesnelerin İnterneti (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Önceden yapılandırılmış çözümler

IoT paketi, yaygın IoT senaryolarını hızlı şekilde kullanmaya başlamanızı ve keşfetmenizi sağlayan aşağıdaki gibi önceden yapılandırılmış çözümler sunar:

* Uzaktan izleme
* Tahmine dayalı bakım
* Bağlı fabrika

Bu çözümleri Azure aboneliğinize dağıtabilir ve ardından tam, uçtan uca bir IoT senaryosu çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

IoT Paketi'nin yapabileceklerini ve ana bileşenlerini genel hatlarıyla gördüğünüze göre IoT Paketi'ndeki önceden yapılandırılmış çözümler hakkında daha fazla bilgi alabilirsiniz. Daha fazla bilgi için bkz. [Önceden yapılandırılmış Azure IoT çözümleri nelerdir?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
