---
title: Bir güvenlik modül ikizi ASC IOT Önizleme için oluşturun | Microsoft Docs
description: IOT modül ikizi kullanmak için bir ASC ASC ile IOT oluşturmayı öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: c782692e-1284-4c54-9d76-567bc13787cc
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: cd085333f0ddc8a9d49637a2d7156b34c6e5cc72
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579436"
---
# <a name="quickstart-create-an-azureiotsecurity-module-twin"></a>Hızlı Başlangıç: Bir azureiotsecurity modül ikizi oluşturma

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu hızlı başlangıç açıklamaları bireysel oluşturma _azureiotsecurity_ modül ikizlerini yeni cihazları veya toplu iş için bir IOT Hub'ında tüm cihazlar için modül ikizlerini oluşturun.  

## <a name="understanding-azureiotsecurity-module-twins"></a>Azureiotsecurity modül ikizlerini anlama 

Azure'da yerleşik IOT çözümleri için cihaz ikizlerini hem süreç otomasyonu, hem de cihaz Yönetimi önemli bir rol oynar. 

ASC IOT için cihaz güvenlik durumunuzu yönetmenize olanak sağlayan tam tümleştirmesi var olan IOT cihaz Yönetimi platformunuz ile sunuyor olun yanı sıra mevcut cihaz denetim özelliklerini kullanın.
ASC IOT tümleştirme, IOT hub'ını kullanın sağlayarak gerçekleştirilir ikizi mekanizması.  

Bkz: [IOT hub'ı modül ikizlerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) modül ikizlerini Azure IOT hub'ın genel kavram hakkında daha fazla bilgi edinmek için. 
 
Modül ikizi mekanizmasını kullanın ve adlı bir güvenlik modül ikizi tutar IOT için ASC _azureiotsecurity_ cihazlarınızın her biri için.
Güvenlik modül ikizi tüm bilgileri cihaz güvenliği ile ilgili cihazlarınızın her biri için tutar. 
 
ASC tam kullanılmasını IOT özellikleri sağlamak için oluşturma, yapılandırma ve hizmet her cihaz için bu güvenlik modül ikizlerini kullanma gerekecektir.  

## <a name="create-azureiotsecurity-module-twin"></a>Modül ikizi azureiotsecurity oluşturma 

_azureiotsecurity_ modül ikizlerini iki şekilde oluşturulabilir:
1. [Modül batch betiği](https://aka.ms/iot-security-github-create-module) - otomatik olarak varsayılan yapılandırması'nı kullanarak bir modül ikizi yeni bir cihaz veya cihazları modül ikizi oluşturur.
2. Her cihaz için belirli yapılandırmalar ile tek tek her modül ikizi el ile düzenleme.

>[!NOTE] 
> Batch yöntemi kullanarak mevcut azureiotsecurity modül ikizlerini üzerine yazmaz. Batch yöntemi kullanarak, yalnızca güvenlik modül ikizi olmayan cihazlar için yeni modül ikizlerini oluşturur. 

Bkz: [aracı Yapılandırması](how-to-agent-configuration.md) değiştirin veya varolan bir modül ikizi yapılandırmasını değiştirme hakkında bilgi edinmek için. 

El ile oluşturmak için yeni bir _azureiotsecurity_ modül ikizi bir cihaz için aşağıdaki yönergeleri kullanın: 

1. IOT hub'ınızdaki bulun ve bir güvenlik modül ikizi, IOT hub'ına oluşturmak istediğiniz cihazı seçin.
1. Cihazınızda ve üzerinde tıklatın **modül kimliği eklemek**.
1. İçinde **modülü kimlik adı** alanına **azureiotsecurity**.

1. **Kaydet**’e tıklayın. 

## <a name="verify-creation-of-a-module-twin"></a>Modül ikizi oluşturulmasını doğrulayın

Bir güvenlik modül ikizi belirli bir cihaz için olup olmadığını doğrulamak için:

1. Azure IOT hub'ına, seçin **IOT cihazları** gelen **gezginler** menüsü.    
1. Cihaz kimliği girin veya bir seçenek belirleyin **sorgu cihazı alanına** tıklatıp **sorgu cihazları**. 
    ![Sorgu cihazlar](./media/quickstart/verify-security-module-twin.png)
1. Cihazı seçin veya çift cihaz ayrıntıları sayfasına açmak için tıklayın. 
1. Seçin **modülü kimlikleri** menüsünde ve varlığını onaylamak **azureiotsecurity** cihazla ilişkilendirilmiş modülü kimlikleri listesinde modülü. 
    ![Bir cihazla ilişkili modüller](./media/quickstart/verify-security-module-twin-3.png)


IOT için modül ikizlerini ASC özelliklerini özelleştirme hakkında daha fazla bilgi için bkz: [aracı Yapılandırması](how-to-agent-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

Özel uyarı yapılandırma hakkında bilgi edinmek için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Özel uyarıları yapılandırma](quickstart-create-custom-alerts.md)
