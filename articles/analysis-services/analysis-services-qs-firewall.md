---
title: Hızlı başlangıç - Azure'da Analysis Services sunucusu için güvenlik duvarını yapılandırma | Microsoft Docs
description: Azure'da Analysis Services sunucu örneği için güvenlik duvarı yapılandırmayı öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 96141ddeeb73c7483935d5da49b9341436c10c18
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429733"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Hızlı başlangıç: Sunucu güvenlik duvarını yapılandırma - Portal

Bu hızlı başlangıç Azure Analysis Services sunucunuz için güvenlik duvarını yapılandırmanıza yardımcı olur. Güvenlik duvarını etkinleştirmek ve yalnızca sunucunuza erişen bilgisayarlar için IP adresi aralıkları yapılandırmak, sunucunuzu ve verilerinizi güvenlik altına almanın önemli bir parçasıdır.

## <a name="prerequisites"></a>Ön koşullar

- Aboneliğinizde bir Analysis Services sunucusu. Daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: Sunucu oluşturma - Portal](analysis-services-create-server.md) veya [Hızlı başlangıç: Sunucu oluşturma - PowerShell](analysis-services-create-powershell.md)
- İstemci bilgisayarlar için bir veya birden çok IP adresi aralığı (gerekiyorsa).

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma 

[Portalda oturum açma](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Güvenlik duvarını yapılandırma

1. Genel Bakış sayfasını açmak için sunucunuza tıklayın. 
2. **AYARLAR** > **Güvenlik Duvarı** > **Güvenlik duvarını etkinleştir** altında **Açık** seçeneğine tıklayın.
3. Power BI hizmetinden DirectQuery erişimine izin vermek için, **Power BI'dan erişime izin ver** alanında **Açık** seçeneğine tıklayın.  
4. (İsteğe bağlı) Bir veya birden çok IP adresi aralığı belirtin. Her aralık için bir ad, başlangıç ve bitiş IP adresi girin. 
5. **Kaydet**’e tıklayın.

     ![Güvenlik duvarı ayarları](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında, IP adresi aralıklarını silin veya güvenlik duvarını devre dışı bırakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, sunucunuz için güvenlik duvarı yapılandırmayı öğrendiniz. Artık sunucunuz olduğuna ve bir güvenlik duvarıyla sunucunun güvenliğini sağladığınıza göre, ona portaldan temel bir örnek veri modeli ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenmeye yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza örnek model ekleme](analysis-services-create-sample-model.md)
