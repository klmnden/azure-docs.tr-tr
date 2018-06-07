---
title: Azure yığın dağıtım ağ trafiğini | Microsoft Docs
description: Bu makalede Azure yığın dağıtım ağ işlemleri hakkında beklenmesi gerekenler açıklanmaktadır.
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
ms.date: 05/21/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: b808875e66e867b84e2971c6a5bd031d108d003b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655427"
---
# <a name="about-deployment-network-traffic"></a>Dağıtım ağ trafiğini hakkında
Azure yığın sırasında ağ trafiğinin nasıl akacağını anlama dağıtım başarılı bir dağıtım olma konusunda yaşamsal önem taşır. Bu makalede beklenmesi gerekenler bir anlayış sağlamak için dağıtım işlemi sırasında beklenen ağ trafiğinin anlatılmaktadır.

Bu çizimde, tüm bileşenler ve bağlantıları dağıtım işlemi katılan gösterir:

![Azure yığın dağıtım ağ topolojisi](media/deployment-networking/figure1.png)

> [!NOTE]
> Bu makalede bağlı bir dağıtım için gereksinimleri, diğer dağıtım yöntemleri hakkında bilgi edinmek için [Azure yığın dağıtım bağlantı modelleri](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>VM dağıtımı
Azure yığın çözüm Azure yığın bileşenlerini barındırmak için kullanılan sunucuları ve donanım yaşam döngüsü ana bilgisayar (HLH) adlı bir ek sunucu grubu içerir. Bu sunucu, dağıtmak ve çözümünüzü ömrünü yönetmek için kullanılır ve dağıtım sırasında (DVM) dağıtım VM barındırır.

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri
Dağıtım başlamadan önce dağıtım başarıyla tamamlandığından emin olmak için OEM tarafından doğrulanabilecek bazı minimum gereksinimleri vardır. Bu gereksinimleri ortamını hazırlayın ve doğrulama başarılı olduğundan emin olun yardımcı olacak anlama, bunlar:

-   [Sertifikalar](azure-stack-pki-certs.md)
-   [Azure aboneliği](https://azure.microsoft.com/free/?b=17.06)
-   İnternet erişimi
-   DNS
-   NTP

> [!NOTE]
> Bu makalede son üç gereksinimlerine odaklanır. İlk iki hakkında daha fazla bilgi için yukarıdaki bağlantılara bakın.

## <a name="deployment-network-traffic"></a>Dağıtım ağ trafiği
DVM BMC ağdan bir IP ile yapılandırılmış ve internet ağ erişimi gerektirir. BMC ağ bileşenlerinin tümü dış yönlendirme veya Internet erişimi gerektirmesine karşın, bu ağ üzerinden IP'leri kullanan bazı OEM özgü bileşenlerin bunu da gerektirebilir.

Dağıtım sırasında Azure aboneliğinizin bir Azure hesabı kullanarak Active Directory'e (Azure AD) karşı DVM kimliğini doğrular. Bunu yapmak için Internet erişimi belirli bir bağlantı noktası ve URL'ler listesine DVM gerektirir. Tam listesinde bulabilirsiniz [yayımlama uç noktaları](azure-stack-integrate-endpoints.md) belgeleri. DVM dış URL'lere iç bileşenleri tarafından yapılan DNS isteklerine iletmek için bir DNS sunucusu kullanan. İç DNS bu istekleri dağıtımdan OEM sağladığınız DNS ileticisi adresine iletir. Aynı NTP sunucusu için geçerlidir, tutarlılık ve saat eşitlemesi tüm Azure yığın bileşenlerin korumak için güvenilir bir saat sunucusu gereklidir.

Dağıtım sırasında DVM tarafından gerekli Internet erişimi yalnızca giden, hiçbir gelen çağrıları dağıtımı sırasında yapılır. Kaynak olarak kendi IP kullanır ve bu Azure yığın proxy yapılandırmaları desteklemez göz önünde bulundurun. Bu nedenle, gerekirse, saydam proxy ya da NAT Internet'e erişmek için sağlamanız gerekir. Dağıtım tamamlandıktan sonra Azure yığını ile Azure arasındaki tüm iletişimi yapılan dış ağ üzerinden ortak VIP'ler kullanma.

Erişim denetimi, belirli ağ kaynaklarının ve hedeflerinin arasındaki trafiği kısıtlamak listeleri (ACL'ler) ağ yapılandırmaları Azure yığın anahtarlarını içerir. DVM sınırsız erişimi olan tek bileşendir; hatta HLH çok sınırlıdır. Yönetim ve ağlarınızı erişimden kolaylaştırmak için özelleştirme seçenekleri hakkında OEM isteyin. Bu ACL'ler nedeniyle dağıtım sırasında DNS ve NTP sunucusu adreslerini değiştirmekten kaçınmak önemlidir. Bunu yaparsanız, tüm çözüm için anahtarlarını yeniden yapılandırmanız gerekir.

Dağıtım tamamlandıktan sonra sağlanan DNS ve NTP sunucusu adreslerini sistemin bileşenleri tarafından doğrudan kullanılmak üzere devam eder. Dağıtım tamamlandıktan sonra DNS isteklerine işaretlerseniz, örneğin, kaynak DVM IP dış ağı aralığından bir adresi değiştirilir.

Azure yığın başarıyla dağıtıldıktan sonra OEM ortağınızla DVM ek dağıtım sonrası görevler için kullanabilir. Ancak, tüm dağıtım görevleri ve dağıtım sonrası yapılandırmaları tamamlandığında, OEM kaldırın ve HLH DVM silin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure kaydı doğrula](azure-stack-validate-registration.md)
