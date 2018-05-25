---
title: 'Azure Active Directory Domain Services: Azure sanal ağı için DNS ayarlarını güncelleştirme | Microsoft Docs'
description: Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2017
ms.author: maheshu
ms.openlocfilehash: 972b995d3768e765b95c136b3cfbee91ab0a88ab
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services’ı etkinleştirme

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Görev 4: Azure sanal ağı için DNS ayarlarını güncelleştirme
Önceki yapılandırma görevlerinde dizininiz için Azure Active Directory Domain Services başarıyla etkinleştirdiniz. Sonraki göreviniz sanal ağınızdaki bilgisayarların bu hizmetlere bağlanabilmesini ve bu hizmetleri kullanabilmesini sağlamaktır. Bu makalede, sanal ağınızdaki DNS sunucusu ayarlarını, sanal ağda Azure Active Directory Domain Services’in kullanılabilir olduğu iki IP adresini işaret edecek şekilde güncelleştirin.

Azure Active Directory Domain Services'i etkinleştirdiğiniz sanal ağın DNS sunucusu ayarını güncelleştirmek için aşağıdaki adımları uygulayın:

1. **Genel Bakış** sekmesinde, yönetilen etki alanınız tamamen hazır hale getirildikten sonra gerçekleştirilecek **gerekli yapılandırma adımları** kümesi bulunur. İlk yapılandırma adımı, **sanal ağınız için DNS sunucusu ayarlarını güncelleştirmektir**.

    ![Etki Alanı Hizmetleri - Tamamen hazır haldeki Genel Bakış sekmesi](./media/getting-started/domain-services-provisioned-overview.png)

2. Etki alanınız tamamen hazır hale getirildiğinde bu kutucukta iki IP adresi görüntülenir. Bu IP adreslerinden her biri, yönetilen etki alanınıza yönelik bir etki alanı denetleyicisini temsil eder.

3. İlk IP adresini panoya kopyalamak için yanında bulunan kopyalama düğmesine tıklayın. Ardından **DNS sunucularını yapılandır** düğmesine tıklayın.

4. İlk IP adresini **DNS sunucuları** dikey penceresinde bulunan **DNS sunucusu ekle** metin kutusuna yapıştırın. İkinci IP adresini kopyalamak için yatay olarak sola kaydırma yapın ve IP adresini **DNS sunucusu ekle** metin kutusuna yapıştırın.

    ![Etki Alanı Hizmetleri - DNS'yi güncelleştirme](./media/getting-started/domain-services-update-dns.png)

5. İşlemi tamamladığınızda, sanal ağ için DNS sunucularını güncelleştirmek üzere **Kaydet**'e tıklayın.

> [!NOTE]
> Ağ üzerindeki sanal makineler, yeni DNS ayarlarını yalnızca yeniden başlatma işleminin ardından alabilir. Sanal makinelerin güncelleştirilen DNS ayarlarını doğrudan almasını istiyorsanız portal, PowerShell veya CLI aracılığıyla bir yeniden başlatma tetikleyin.
>
>

## <a name="next-step"></a>Sonraki adım
[Görev 5: Azure Active Directory Domain Services ile parola karma eşitlemesini etkinleştirme](active-directory-ds-getting-started-password-sync.md)
