---
title: Azure Active Directory koşullu erişimin bir taban çizgisi koruma nedir | Microsoft Docs
description: Ortamınızda etkin güvenlik temel düzeyde en az sahip temel koruması nasıl sağlar öğrenin.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 25ae4db2cd4f2a2cea74c428a272c6868acaa5c5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249367"
---
# <a name="what-is-baseline-protection"></a>Taban çizgisi koruma nedir?  

Geçen yıl içinde kimlik saldırıları % 300 artırmıştır. Ortamınızı gitgide artan saldırılara karşı korumak için Azure Active Directory (Azure AD) temel koruma adı verilen yeni bir özellik sunmaktadır. Taban çizgisi koruma, önceden tanımlanmış koşullu erişim ilkeleri kümesidir. Bu ilkelerin en az ortamınızda etkin güvenlik temel düzeyde olmasını sağlamak için hedeftir. 

Önizleme sırasında bunları etkinleştirmek istiyorsanız, temel ilkeleri etkinleştirmeniz gerekir. POST genel kullanılabilirlik, bu ilkeler, varsayılan olarak etkin olan. 

İlk temel koruma ilke ayrıcalıklı hesaplar için MFA gerekir. Bu hesapları ilk korumak için kritik olacak şekilde, ayrıcalıklı hesapların denetim elde saldırganlar inanılmaz hasar yapabilirsiniz. Bu ilke kapsamında aşağıdaki ayrıcalıklı rolleri şunlardır: 

- Genel yönetici  

- SharePoint yöneticisi  

- Exchange yöneticisi  

- Koşullu erişim yöneticisi  

- Güvenlik yöneticisi  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım 

Temel ilke etkinleştirmek için:  

1. Oturum [Azure portal](https://portal.azure.com) genel yönetici, güvenlik veya koşullu erişim yönetici olarak.

2. İçinde **Azure portal**, sol gezinti çubuğu üzerinde tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. Üzerinde **Azure Active Directory** sayfasında **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-baseline-protection/03.png)

4. İlkeler listesinde tıklatın **temel ilke: Yönetici (Önizleme) için MFA gerektiren**. 

5. İlkeyi etkinleştirmek için **ilkeyi hemen kullanmak**.

6. **Kaydet**’e tıklayın. 
 

Temel ilke kullanıcılar ve gruplar dışlamak için seçenek sunar. Bir dışarıda bırakmak isteyebilirsiniz *[Acil Durum erişimi yönetici hesabı](active-directory-admin-manage-emergency-access-accounts.md)* , değil kilitli dışında Kiracı emin olmak için.
  
 

## <a name="what-you-should-know"></a>Bilmeniz gerekenler 

Taban çizgisi ilkesine dahil directory rolleri en yüksek ayrıcalıklı bir Azure AD rolleridir. Geri bildirimi doğrultusunda, diğerleri gelecekte dahil olabilir. 

Komut dosyalarınızı yönetici ayrıcalıklarıyla hesabınız yoksa, kullanmanız gereken [yönetilen hizmet kimliği (MSI)](managed-service-identity/overview.md) veya [hizmet sorumluları (sertifikalarla)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) yerine. Geçici bir çözüm olarak, belirli kullanıcı hesaplarını, temel ilkesinden hariç tutabilirsiniz. 

POP, IMAP, eski Office masaüstü istemcisi gibi eski kimlik doğrulama akışı ilke uygulanır. 

## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
