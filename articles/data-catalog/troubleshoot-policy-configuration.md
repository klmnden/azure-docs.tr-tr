---
title: Azure veri kataloğu için Azure Active Directory ilkesini yapılandırma
description: Azure veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmaya çalıştığında bir durumla karşılaşabilirsiniz, bir hata iletisiyle karşılaşırsınız.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/06/2019
ms.openlocfilehash: e69a7e3bd104d0fb82b248b6560d4fd082c88426
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62116610"
---
# <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory ilke yapılandırması

Azure Veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmak istediğinizde oturum açmanızı engelleyen bir hata iletisiyle karşılaştığınız bir durum yaşayabilirsiniz. Bu hata şirket ağında olduğunuzda ya da şirket ağının dışından gelen bağlanmaya çalıştığınız sırada ortaya çıkabilir.

## <a name="registration-tool"></a>Kayıt Aracı

Kayıt aracı, kullanıcının Azure Active Directory ile oturum açtığını doğrulamak için *form kimlik doğrulaması* kullanır. Başarılı bir oturum açma için Azure Active Directory yöneticisinin *genel kimlik doğrulama ilkesinde* form kimlik doğrulamasını etkinleştirmesi gerekir.

Genel kimlik doğrulama ilkesi ile aşağıdaki görüntüde gösterildiği gibi kimlik doğrulamasını intranet ve extranet bağlantıları için ayrı ayrı etkinleştirebilirsiniz. Bağlantı kurduğunuz ağ için form kimlik doğrulaması etkin değilse oturum açma hataları oluşabilir.

 ![Azure Active Directory genel kimlik doğrulama ilkesi](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Daha fazla bilgi için bkz. [Kimlik doğrulama ilkelerini yapılandırma](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Azure veri Kataloğu oluşturma](data-catalog-get-started.md)