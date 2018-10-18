---
title: IOT Edge modülü sertifika | Microsoft Docs
description: IOT Edge modülü markette onaylayın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: c37ed908b61ca54957affed3f81526353bc3f53b
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389573"
---
# <a name="the-iot-edge-module-certification-process"></a>IOT Edge modülü sertifika işlemi

Bu makalede, Azure Marketi'nde yayımlama için bir IOT Edge modülü onaylamak için gereksinimler ve adımlar açıklanır. 

>[!Note]
>Bu geçici bir belgedir. IOT Edge modülü markette paralel olarak oluşturulmakta ve bu makalede, public belge tarafından değiştirilecektir.

## <a name="understanding-an-iot-edge-module"></a>IOT Edge modülü anlama

Modül terimler:

-   A **modülü görüntüsü** bir modül tanımlar ve yazılımı içeren bir paket.

-   A **modül örneğinin** belirli bir IOT Edge cihazında modülü görüntüsü çalıştıran hesaplama birimidir. Modül örneğinin IOT Edge çalışma zamanı tarafından başlatılır.

Modüller ayrıca aşağıdaki terimleri SDK'SININ, IOT modülü şunları içerebilir:

-   A **modülü kimlik** her bir modül örneğiyle ilişkili IOT Hub'ında depolanan bilgileri (güvenlik kimlik bilgileri dahil) bir parçasıdır.

-   A **modül ikizi** meta veriler, yapılandırmalar ve koşullar dahil olmak üzere bir modül örneğinin için durum bilgilerini içeren IOT Hub'ında depolanan bir JSON belgesidir.

-   **SDK'ları** birden çok dilde özel modüller gibi geliştirmek için kullanılır: C\#, C, Python, Java ve Node.JS.

## <a name="the-onboarding-process-for-an-iot-edge-module"></a>Ekleme işleminin bir IOT Edge Modülü

Aşağıdaki ekran görüntüsü yakalamayı Azure Marketi'nde genel olacak bir IOT Edge modülü işlemini gösterir.

![Sertifika işlemi](./media/cloud-partner-portal-iot-edge-module-certification-process/onboarding-process.png)

### <a name="onboarding-step-details"></a>Hazırlama adımı ayrıntıları

-   **1. adım** -iş ortakları teklif türüyle kendi teklifleri gönderin **IOT Edge Modülü** Azure Market ekibi tarafından bulut iş ortağı portalı aracında. Bulut iş ortağı portalı aracına erişmek için resmi bir MS ortak olmalıdır. Daha fazla bilgi için [yayımcının Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   **2. adım** - Market otomatik olarak çalışan virüsten koruma ve kötü amaçlı yazılımdan koruma denetler. IOT takım bu aşaması sırasında özel otomatik alımı testleri çalıştırabilir.

-   **3. adım** -genel bir modüldür. Market'te listelenir ve kapsayıcı bir URL'den anonim olarak çekilebilir. Örneğin, *marketplace.azurecr.io/module-identifier*.

## <a name="iot-edge-module-certification-criteria"></a>IOT Edge modülü sertifika ölçütlerini

Sertifikalı ve Azure Market'te yayımlanmış bir IOT Edge modülü için karşılanması gereken ana gereksinimleri aşağıdadır.

>[!Note]
>Bu gereksinimleri değişebilir. Önceden bildirilir ve olması için güncelleştirilmiş gereksinimlerini karşıladıkları kapsayıcılarınızı güncellemek için verilen.

### <a name="technical-requirements"></a>Teknik gereksinimler

**IOT Edge modülü olabilir**

-   Yalnızca docker uyumlu kapsayıcılar IOT Edge modülleri desteklenmemektedir. Modül çalıştırmalıdır [Moby](https://mobyproject.org/). 

    >[!Note]
    >Docker için temel alınan açık kaynaklı proje Moby olduğu için docker kapsayıcıları Moby ile çalışma olasılığı yüksektir.

-   İş ortağı aşağıdaki varsayılan ayarlarla sağlamanız gerekir: 

    -   Varsayılan **etiketi** (hangi boş olamaz.

    -   Varsayılan **createOptions** (Bu boş olabilir.)

    -   Varsayılan bir IOT modülü SDK kullanıyorsanız **ikizi** (Bu boş olabilir.)

    -   Varsayılan bir IOT modülü SDK kullanıyorsanız **yollar** (Bu boş olabilir.)

**Platform desteği**

-   Olan platformları **sunuldu** IOT Edge Katman 1'deki (kaydedildiği şekilde [Azure IOT Edge desteği](https://docs.microsoft.com/azure/iot-edge/support)) desteklenmesi gerekir. Örneğin, bu şu anda aşağıdaki işletim sistemi mimarisi birleşimleri destekleyen anlamına gelir:

    -   Ubuntu Server 18.04 x64 için

    -   Ubuntu Server 16.04 x64 için

    -   Raspbian-esnetme için arm32 (armhf)

**Cihaz boyutlandırma**

-   Eşdeğer boyutları ile tüm cihazlar (CPU/RAM/depolama/GPU/ve vb.) Raspberry Pi'yi veya daha büyük bir IOT Edge cihazı olabilir. Bir modülü yalnızca içinde belirli boyut kısıtlamalarını çalışıyorsa, bu kısıtlamaları modül tanımı belirtilmelidir.

**Varsayılan ayarları**

-   Bir modülü, yepyeni desteklenen her platform için (işletim sistemi + mimarisi) için varsayılan parametreleri olan başlamanız gerekir.

-   Yapılandırma ayarları olmalıdır açıkça belgelenen (etiketler, ortam değişkenleri, ikizi, yolları) Listenin bir parçası olarak, iş ortakları, temel HTML biçimlendirmeyi destekler veya bir dış Web sayfasına işaret kendi modül için bir açıklama tanımlayabilirsiniz.

**Sürüm oluşturma**

-   Müşteriler, otomatik güncelleştirme marketten yakında bir modül istedikleri ya da tam bir sürümünü kullanmak istiyorsanız test ettikten seçebilir olmalıdır. Market modülleri, IOT Edge çalışma zamanı aynı sürüm oluşturma düzeni izlemeniz gerekir. Örneğin:

    -   Etiketleri "1.0" gibi sıralı güncelleştirilebilir.

    -   "1.3.24" gibi alt sürüm etiketleri güncelleştirilemez.

**Telemetri**

-   Modülü IOT SDK'sını kullanarak modülleri telemetri amacıyla Market tarafından atanmış benzersiz modülü tanımlayıcısı ayarlamanız gerekir. Bu, çalışan modülü örnek sayısını belirlemek Azure Marketi sağlar. Bu benzersiz tanımlayıcısı alımı Market'ten atayan kapsayıcı adı değil. Bu tanımlayıcı ayarlamak için aşağıdaki Sdk'lardan yöntemlerini kullanın:
    - [C\#](https://hub.docker.com/_/mysql/)
    - [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._product_info?view=azure-java-stable)

-   Modülü IOT SDK'sını kullanma modüller için daha az kesin öngörüleri bulut iş ortağı Portalı aracılığıyla kullanılabilir. Örneğin, indirilme sayısı.

**Güvenlik**

-   Kapsayıcıların konağın mümkün olduğunca az ayrıcalıklı erişimi olmalıdır. "Ayrıcalıklı" kapsayıcılar çok seyrek olmalıdır.

-   İş ortakları kendi modül tutması gerekir sağladıkları desteğin bir parçası olarak güvenli.

**Güncelleştirmeler**

-   İş ortakları kendi modül güncel ve çalışır durumda kalması gerekir. IOT Edge çalışma zamanı için planlanan bozucu değişiklikler varsa önceden bildirilir.

**Modülü IOT SDK'sı**

-   Modülü IOT SDK'sı dahil olmak üzere sertifika için bir önkoşul değildir.
    Ancak, modül IOT SDK'sı dahil olmak üzere daha iyi bir kullanıcı deneyimi sağlayabilir. Örneğin, yönlendirme, iletileri buluta gönderme destekleyebilir. Modülü IOT SDK'sı, çalışan modülü örneklerinin sayısına telemetri almak için gereklidir.

**Öznel gereksinimi**

-   En az bir gerçek dünya IOT Edge kullanım örneği karşılaması gerekir. Örneğin, bir müşteri IOT Edge kullanılacak iradeye sahip değilse bir WordPress kapsayıcı eklenen olmamalıdır.

**Yasal gereksinimlere (ilkesinden AMP)**

-   Modül Microsoft Azure Marketi sözleşmeleri ve ilkeleri ile uyumlu olmalıdır. Daha fazla bilgi için bağlı yayımcı Sözleşmesi'ne bakın ve [katılım ilke](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

-   Azure Marketi sözleşmeleri ve ilkeleri yanı sıra olabilir ek yasal gereksinimler bir IOT Edge modülü teklif türüne özel. Şu anda gözden geçiriliyor budur.

-   Modül olmalıdır bir *kullanım koşullarını* ve *gizlilik ilkesi*.

-   Modül, ayrıca üçüncü modülü herhangi bir üçüncü taraftan kullanıyorsa bildirimi taraf sahip olmalıdır.

**Destek gereksinimlerini (ilkesinden AMP)**

-   İş ortakları, IOT Edge modülleri desteklemek için sorumlu olursunuz. Bunlar IOT Edge modülü açıklamada verilen destek seçenekleri kullanılabilir kalmasını ve en az bir destek seçeneği her zaman kullanılabilir olduğundan emin olun. (AMP yayımcı anlaşması'daha fazla ayrıntı için 4. Bölüm bakın.)

**Kategorilere ayırma**

-   Tüm IOT Edge modülleri kategorisinin altında görünür **nesnelerin interneti \>IOT Edge Modülü**. Ürün için en iyi alt kategori seçmek için iş ortağı aittir.
