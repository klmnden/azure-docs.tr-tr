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
ms.openlocfilehash: 89802a638944ec220186e943d5fdc33524b2d4e9
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541688"
---
# <a name="quickstart-create-an-asc-for-iot-module-twin"></a>Hızlı Başlangıç: Bir ASC IOT modül ikizi için oluşturma

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu hızlı başlangıçta açıklamaları IOT modül ikizlerini yeni cihazlar için bireysel ASC oluşturun veya toplu olarak nasıl modül ikizlerini tüm cihazlar için IOT hub'ı oluşturun.  

## <a name="understanding-asc-for-iot-module-twins"></a>ASC için IOT modül ikizlerini anlama 

Azure'da yerleşik IOT çözümleri için cihaz ikizlerini hem süreç otomasyonu, hem de cihaz Yönetimi önemli bir rol oynar. 

ASC IOT için cihaz güvenlik durumunuzu yönetmenize olanak sağlayan tam tümleştirmesi var olan IOT cihaz Yönetimi platformunuz ile sunuyor olun yanı sıra mevcut cihaz denetim özelliklerini kullanın. ASC IOT tümleştirme, IOT hub'ını kullanın sağlayarak gerçekleştirilir ikizi mekanizması.  

Bkz: [IOT hub'ı modül ikizlerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) modül ikizlerini Azure IOT hub'ın genel kavram hakkında daha fazla bilgi edinmek için. 
 
ASC modül ikizi mekanizmasını kullanın ve cihazlarınızın her biri için bir güvenlik modül ikizi tutar IOT için. Güvenlik modül ikizi tüm bilgileri cihaz güvenliği ile ilgili cihazlarınızın her biri için tutar. 
 
ASC tam kullanılmasını IOT özellikleri sağlamak için oluşturma, yapılandırma ve hizmet her cihaz için bu güvenlik modül ikizlerini kullanma gerekecektir.  

## <a name="create-asc-for-iot-module-twin"></a>ASC IOT modül ikizi için oluşturma 

ASC IOT modül ikizlerini için varsayılan yapılandırmayı kullanarak toplu iş modunda veya tek tek her cihaz için belirli yapılandırmalar ile oluşturulabilir. Batch'e yeni cihazlar veya modül ikizi olmayan cihazlar için oluşturun, kullanın [modülü batch betiği](https://aka.ms/iot-security-github-create-module). 

>[!NOTE] 
> Batch yöntemi kullanarak mevcut modül ikizlerini üzerine yazmaz. Batch yöntemi kullanarak, yalnızca bir modül ikizi olmayan cihazlar için yeni modül ikizlerini oluşturur. 

Bkz: [güvenlik modül ikizi değiştirme](how-to-modify-security-module-twin.md) değiştirin veya varolan bir modül ikizi yapılandırmasını değiştirme hakkında bilgi edinmek için. 

IOT modül ikizi bir cihaz için yeni bir ASC oluşturmak için aşağıdaki yönergeleri kullanın: 

1. IOT hub'ınızdaki bulun ve bir güvenlik modül ikizi, IOT hub'ına oluşturmak istediğiniz cihazı seçin. 
1. İçinde **Microsoft Kimlik adı** alanına **ascforiotsecurity**.
1. **Kaydet**’e tıklayın. 

## <a name="verify-creation-of-a-module-twin"></a>Modül ikizi oluşturulmasını doğrulayın

Bir güvenlik modül ikizi belirli bir cihaz için olup olmadığını doğrulamak için:

1. Azure IOT hub'ına, seçin **IOT cihazları** gelen **gezginler** menüsü.    
1. Cihaz kimliği girin veya bir seçenek belirleyin **sorgu cihazı alanına** tıklatıp **sorgu cihazları**. 
    ![Sorgu cihazlar](./media/quickstart/verify-security-module-twin.png)
1. Cihazı seçin veya çift cihaz ayrıntıları sayfasına açmak için tıklayın. 
1. Seçin **modülü kimlikleri** menüsünde ve varlığını onaylamak **ascforiotsecurity** modülü ve **bağlantı durumu** , **bağlı**cihazla ilişkilendirilmiş modülü kimlikleri listesinde. 
    ![Bir cihazla ilişkili modüller](./media/quickstart/verify-security-module-twin-2.png)


IOT için modül ikizlerini ASC özelliklerini özelleştirme hakkında daha fazla bilgi için bkz: [aracı Yapılandırması](concept-agent-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

Özel uyarı yapılandırma hakkında bilgi edinmek için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Özel uyarıları yapılandırma](quickstart-create-custom-alerts.md)
