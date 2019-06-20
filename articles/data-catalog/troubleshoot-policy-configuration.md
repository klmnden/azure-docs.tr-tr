---
title: Azure veri Kataloğu sorunlarını giderme
description: Bu makalede, Azure veri Kataloğu kaynakları ile ilgili yaygın sorun giderme konuları açıklanır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: troubleshooting
ms.date: 06/13/2019
ms.openlocfilehash: ed74e90e5e8ed55b75968f51cb50e6a1b4cdd75d
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203498"
---
# <a name="troubleshooting-azure-data-catalog"></a>Azure veri Kataloğu sorunlarını giderme

Bu makalede, Azure veri Kataloğu kaynakları ile ilgili yaygın sorun giderme konuları açıklanır. 

## <a name="functionality-limitations"></a>İşlevselliği sınırlamaları

Azure veri Kataloğu'nu kullanarak, aşağıdaki işlevleri sınırlıdır:

- Tür hesaplarıyla **Konuk rol** desteklenmez. Konuk hesapları Azure veri Kataloğu'nun bir kullanıcı olarak ekleyemezsiniz ve Konuk kullanıcılar www.azuredatacatalog.com portalı kullanamazsınız.

- Azure Resource Manager şablonları ya da Azure PowerShell komutlarını kullanarak Azure veri Kataloğu kaynakları oluşturma desteklenmiyor.

- Azure veri Kataloğu kaynak Azure kiracılar arasında taşınamaz.

## <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory ilke yapılandırması

Azure Veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmak istediğinizde oturum açmanızı engelleyen bir hata iletisiyle karşılaştığınız bir durum yaşayabilirsiniz. Bu hata şirket ağında olduğunuzda ya da şirket ağının dışından gelen bağlanmaya çalıştığınız sırada ortaya çıkabilir.

Kayıt aracı, kullanıcının Azure Active Directory ile oturum açtığını doğrulamak için *form kimlik doğrulaması* kullanır. Başarılı bir oturum açma için Azure Active Directory yöneticisinin *genel kimlik doğrulama ilkesinde* form kimlik doğrulamasını etkinleştirmesi gerekir.

Genel kimlik doğrulama ilkesi ile aşağıdaki görüntüde gösterildiği gibi kimlik doğrulamasını intranet ve extranet bağlantıları için ayrı ayrı etkinleştirebilirsiniz. Bağlantı kurduğunuz ağ için form kimlik doğrulaması etkin değilse oturum açma hataları oluşabilir.

 ![Azure Active Directory genel kimlik doğrulama ilkesi](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Daha fazla bilgi için bkz. [Kimlik doğrulama ilkelerini yapılandırma](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Azure veri Kataloğu oluşturma](data-catalog-get-started.md)
