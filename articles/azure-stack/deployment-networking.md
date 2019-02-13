---
title: Azure Stack dağıtım ağ trafiğini | Microsoft Docs
description: Bu makalede, Azure Stack dağıtım ağ işlemleri hakkında beklenmesi gerekenler açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: wamota
ms.lastreviewed: 08/30/2018
ms.openlocfilehash: 5f4f76f87718ddcc81f8fae8b043b73a4dbd6b0a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189285"
---
# <a name="about-deployment-network-traffic"></a>Dağıtım ağ trafiğini hakkında
Azure Stack sırasında ağa trafiğin nasıl akacağını anlama dağıtım başarılı bir dağıtım sağlamak için önemlidir. Bu makalede beklenmesi gerekenler bir anlayış sağlamak için dağıtım işlemi sırasında beklenen ağ trafiğinin gösterilmektedir.

Bu çizim, tüm bileşenleri ve bağlantılar dahil dağıtım işlemindeki gösterir:

![Azure Stack dağıtım ağ topolojisi](media/deployment-networking/figure1.png)

> [!NOTE]
> Bu makalede bağlı dağıtım için gereksinimleri, diğer dağıtım yöntemleri hakkında bilgi edinmek için bkz [Azure Stack dağıtım bağlantı modelleri](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>VM dağıtımı
Azure Stack çözüm, Azure Stack bileşenleri barındırmak için kullanılan sunucuları ve donanım yaşam döngüsü ana bilgisayar (HLH) adlı ek bir sunucu grubu içerir. Bu sunucu, dağıtmak ve çözümünüzü ömrünü yönetmek için kullanılır ve dağıtım sırasında dağıtım VM (DVM) barındırır.

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri
Dağıtım başlamadan önce dağıtım başarıyla tamamlandıktan emin olmak için OEM tarafından doğrulanabilecek bazı en düşük gereksinimi yoktur. Bu gereksinimleri ortamını hazırlayın ve doğrulama başarılı olduğundan emin olun yardımcı olacak anlama, bunlar:

-   [Sertifikalar](azure-stack-pki-certs.md)
-   [Azure Aboneliği](https://azure.microsoft.com/free/?b=17.06)
-   İnternet erişimi
-   DNS
-   NTP

> [!NOTE]
> Bu makale, son üç gereksinimlerine odaklanır. İlk iki hakkında daha fazla bilgi için yukarıdaki bağlantılara bakın.

## <a name="deployment-network-traffic"></a>Dağıtım ağ trafiği
DVM BMC ağdan bir IP ile yapılandırılmış ve internet ağ erişimi gerektirir. BMC ağ bileşenlerinin tüm dış yönlendirme veya Internet erişimi gerekli olsa da bu ağdan IP'leri kullanan bazı OEM özgü bileşenler bunu da gerekebilir.

Dağıtım sırasında Azure aboneliğinizde bir Azure hesabı kullanarak Active Directory (Azure AD) karşı DVM kimliğini doğrular. Bunu yapmak için DVM belirli bağlantı noktası ve URL'ler listesini Internet erişimi gerektirir. Tam listesinde bulabilirsiniz [yayımlama uç noktaları](azure-stack-integrate-endpoints.md) belgeleri. DVM dış URL'lere iç bileşenleri tarafından yapılan DNS istekleri iletmek için bir DNS sunucusu yararlanacaktır. İç DNS, bu istekleri OEM dağıtımından önce sağladığınız DNS ileticisi adresine iletir. Aynı durum NTP sunucusu için geçerlidir, tüm Azure Stack bileşenlerin tutarlılık ve saati eşitleme sağlamak için güvenilir bir zaman sunucusu gereklidir.

Dağıtım sırasında DVM tarafından gerekli internet erişimi yalnızca giden, dağıtım sırasında gelen çağrı yapılır. Kendi IP kaynağı olarak kullanır ve bu Azure Stack proxy yapılandırmaları desteklemez göz önünde bulundurun. Bu nedenle, gerekirse, İnternet'e erişmek için bir saydam proxy veya NAT sağlamanız gerekir. Dağıtım sırasında bazı iç bileşenleri, dış ortak VIP kullanarak ağ üzerinden internet erişimi başlar. Dağıtım tamamlandıktan sonra Azure Stack ile Azure arasındaki tüm iletişimi genel VIP kullanarak dış ağ üzerinden yapılır.

Azure Stack anahtarları ağ yapılandırmalarına erişim denetimi, belirli ağ kaynaklarına ve hedeflere arasında trafiği kısıtlamak listeleri (ACL'ler) içerir. DVM sınırsız erişimi olan tek bir bileşenidir; hatta HLH çok sınırlıdır. Yönetim ve ağlarınızı gelen erişimi kolaylaştırmak için özelleştirme seçenekleri hakkında OEM sorabilirsiniz. Bu ACL'ler nedeniyle dağıtım sırasında DNS ve NTP sunucusu adreslerini değiştirmekten kaçınmak önemlidir. Bunu yaparsanız, tüm çözüm için anahtarları yeniden yapılandırmanız gerekir.

Dağıtım tamamlandıktan sonra sistemin bileşenler tarafından doğrudan kullanılmak üzere sağlanan DNS ve NTP sunucusu adreslerini devam eder. Dağıtım tamamlandıktan sonra DNS istekleri işaretlerseniz, örneğin, kaynak DVM IP adresin ağda aralığından değişir.

Dağıtım tamamlandıktan sonra sistemin bileşenlerini ve dış ağa kullanarak SDN aracılığıyla tarafından kullanılmak üzere sağlanan DNS ve NTP sunucusu adreslerini devam eder. Örneğin, dağıtım tamamlandıktan sonra DNS istekleri kontrol etmeniz durumunda, kaynak için genel bir VIP DVM IP değişecektir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure kaydı doğrula](azure-stack-validate-registration.md)
