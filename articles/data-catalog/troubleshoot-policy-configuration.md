---
title: Azure veri kataloğu için Azure Active Directory ilkesini yapılandırma
description: Azure veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmaya çalıştığında bir durumla karşılaşabilirsiniz, bir hata iletisiyle karşılaşırsınız.
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/06/2019
ms.openlocfilehash: 558f8845f5469bf157188e20f1ec65a07ff8355f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59363139"
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