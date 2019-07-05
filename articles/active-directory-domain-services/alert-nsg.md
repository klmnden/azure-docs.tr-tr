---
title: 'Azure Active Directory etki alanı Hizmetleri: Sorun giderme ağ güvenlik grubu yapılandırması | Microsoft Docs'
description: Azure AD Domain Services için NSG yapılandırmasıyla ilgili sorunları giderme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 08875ec23740eab7787c4a919566df521deba9a5
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67473916"
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Geçersiz ağ yapılandırması, yönetilen etki alanınız için sorun giderme
Bu makale ve aşağıdaki uyarı iletisinde neden ağla ilgili yapılandırma hatalarını gidermek yardımcı olur:

## <a name="alert-aadds104-network-error"></a>Alert AADDS104: Ağ hatası
**Uyarı iletisi:** *Microsoft bu yönetilen etki alanı için etki alanı denetleyicilerine ulaşamıyoruz. Bu, sanal ağ yönetilen etki alanı erişimi engeller üzerinde yapılandırılan bir ağ güvenlik grubu (NSG) durum meydana. Başka bir olası neden, kullanıcı tanımlı bir yol varsa, internet'ten gelen trafiği engeller. ' dir.*

Geçersiz NSG ağ hatalarının en yaygın nedeni Azure AD Domain Services için yapılandırmalardır. Sanal ağınızı erişmesine izin vermek için yapılandırılan ağ güvenlik grubu (NSG) [belirli bağlantı noktalarını](network-considerations.md#ports-required-for-azure-ad-domain-services). Bu bağlantı noktaları engellenirse, Microsoft izleme veya yönetilen etki alanınıza güncelleştirin. Ayrıca, Azure AD dizininizi ve yönetilen etki alanınıza eşitlemesi etkilenir. NSG oluşturulurken bu bağlantı noktaları hizmet kesintisini önlemek için açık tutun.

### <a name="checking-your-nsg-for-compliance"></a>NSG uyumluluğu denetleniyor

1. Gidin [ağ güvenlik grupları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında sayfası
2. Tablodan, yönetilen etki alanınıza etkinleştirildiği alt ağ ile ilişkilendirilmiş NSG seçin.
3. Altında **ayarları** sol panelde tıklayın **gelen güvenlik kuralları**
4. Yerinde kuralları gözden geçirin ve erişimi engelleme hangi kuralları tanımlamak [Bu bağlantı noktaları](network-considerations.md#ports-required-for-azure-ad-domain-services)
5. NSG kuralı siliniyor, bir kural ekleme ya da tamamen yeni bir NSG oluşturmayı uyumluluk sağlamak için düzenleyin. Adımları [alınabilecek](#add-a-rule-to-a-network-security-group-using-the-azure-portal) veya yeni oluşturun, uyumlu bir NSG aşağıda verilmiştir

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
4. Tıklayarak kural oluşturma **Ekle** ve bilgiler. **Tamam**'ı tıklatın.
5. Kurallar tablodaki yerleştirerek, kuralı oluşturuldu doğrulayın.


## <a name="need-help"></a>Yardım mı gerekiyor?
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](contact-us.md).
