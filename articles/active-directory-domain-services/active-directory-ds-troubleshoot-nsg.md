---
title: 'Azure Active Directory Domain Services: Sorun giderme ağ güvenlik grubu yapılandırması | Microsoft Docs'
description: Azure AD Domain Services için NSG yapılandırmasıyla ilgili sorunları giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: ergreenl
ms.openlocfilehash: 6e7d025e9e83f5511fce25d0c24e4da3b04d7e54
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957547"
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Geçersiz ağ yapılandırması, yönetilen etki alanınız için sorun giderme
Bu makale ve aşağıdaki uyarı iletisinde neden ağla ilgili yapılandırma hatalarını gidermek yardımcı olur:

## <a name="alert-aadds104-network-error"></a>Uyarı AADDS104: Ağ hatası
**Uyarı iletisi:** *Microsoft bu yönetilen etki alanı için etki alanı denetleyicilerine ulaşamıyoruz. Bu, sanal ağ yönetilen etki alanı erişimi engeller üzerinde yapılandırılan bir ağ güvenlik grubu (NSG) durum meydana. Başka bir olası neden, kullanıcı tanımlı bir yol varsa, internet'ten gelen trafiği engeller. ' dir.*

Geçersiz NSG ağ hatalarının en yaygın nedeni Azure AD Domain Services için yapılandırmalardır. Sanal ağınızı erişmesine izin vermek için yapılandırılan ağ güvenlik grubu (NSG) [belirli bağlantı noktalarını](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services). Bu bağlantı noktaları engellenirse, Microsoft izleme veya yönetilen etki alanınıza güncelleştirin. Ayrıca, Azure AD dizininizi ve yönetilen etki alanınıza eşitlemesi etkilenir. NSG oluşturulurken bu bağlantı noktaları hizmet kesintisini önlemek için açık tutun.

### <a name="checking-your-nsg-for-compliance"></a>NSG uyumluluğu denetleniyor

1. Gidin [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında sayfası
2. Tablodan, yönetilen etki alanınıza etkinleştirildiği alt ağ ile ilişkilendirilmiş NSG seçin.
3. Altında **ayarları** sol panelde tıklayın **gelen güvenlik kuralları**
4. Yerinde kuralları gözden geçirin ve erişimi engelleme hangi kuralları tanımlamak [Bu bağlantı noktaları](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services)
5. NSG kuralı siliniyor, bir kural ekleme ya da tamamen yeni bir NSG oluşturmayı uyumluluk sağlamak için düzenleyin. Adımları [alınabilecek](#add-a-rule-to-a-network-security-group-using-the-azure-portal) veya [yeni, uyumlu bir NSG oluşturmak](#create-a-nsg-for-azure-ad-domain-services-using-powershell) olan aşağıdaki

## <a name="sample-nsg"></a>Örnek NSG
Aşağıdaki tabloda, yönetilen etki alanınıza güvenli izleme, yönetme ve güncelleştirme bilgileri Microsoft'a verirken engelleneceği NSG bir örnek gösterilmektedir.

![Örnek NSG](./media/active-directory-domain-services-alerts/default-nsg.png)

>[!NOTE]
> Azure AD Domain Services, sanal ağdan giden sınırsız erişim gerektirir. Sanal ağ için giden erişimi kısıtlayan herhangi ek bir NSG kuralı oluşturmamayı öneririz.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Azure portalını kullanarak bir ağ güvenlik grubu Kuralı Ekle
PowerShell kullanmak istemiyorsanız, Azure portalı kullanarak Nsg'ler için tek kuralları el ile ekleyebilirsiniz. Ağ güvenlik grubunuzu kuralları oluşturmak için aşağıdaki adımları tamamlayın:

1. Gidin [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında sayfası.
2. Tablodan, yönetilen etki alanınıza etkinleştirildiği alt ağ ile ilişkilendirilmiş NSG seçin.
3. Altında **ayarları** sol panelde tıklayın **gelen güvenlik kuralları** veya **giden güvenlik kuralları**.
4. Tıklayarak kural oluşturma **Ekle** ve bilgiler. **Tamam** düğmesine tıklayın.
5. Kurallar tablodaki yerleştirerek, kuralı oluşturuldu doğrulayın.


## <a name="need-help"></a>Yardım mı gerekiyor?
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
