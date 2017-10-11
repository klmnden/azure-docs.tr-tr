---
title: "Azure AD B2C'ye bir Azure aboneliğine bağlanma | Microsoft Docs"
description: "Azure AD B2C kiracısı için fatura içine bir Azure aboneliği etkinleştirmek için adım adım kılavuzu."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 5b9955b2af7f20a79981315fa33a0eb5380a5465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="linking-an-azure-subscription-to-an-azure-b2c-tenant-to-pay-for-usage-charges"></a>Azure B2C kiracısı kullanım ücretleri için ödeme yapmak için bir Azure aboneliği bağlama

Devam eden kullanım ücretleri Azure Active Directory B2C (veya Azure AD B2C) için bir Azure aboneliği için faturalandırılır. Açıkça B2C Kiracı oluşturduktan sonra bir Azure aboneliğine Azure AD B2C kiracısı bağlanmak Kiracı Yöneticisi gereklidir.  Bu bağlantı, Azure AD "B2C Kiracı" oluşturarak elde edilir kaynak hedef Azure aboneliği. Diğer Azure kaynaklarının (örneğin, sanal makineleri, veri depolama, LogicApps) yanı sıra tek bir Azure aboneliği için çok sayıda B2C Kiracı bağlanabilir


> [!IMPORTANT]
> Faturalama ve B2C aşağıdaki sayfanın için fiyatlandırma kullanımı hakkında en son bilgileri: [Azure AD B2C fiyatlandırma](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>1. adım - Azure AD B2C Kiracısı oluşturma
B2C Kiracı oluşturma önce tamamlanması gerekir. B2C Kiracı hedef zaten oluşturduysanız bu adımı atlayın. [Azure AD B2C ile çalışmaya başlama](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>2. adım - Azure aboneliğinize gösteren Azure AD Kiracı açık Azure portalında
[Azure portalına](https://portal.azure.com) gidin. Azure AD Kiracı için kullanmak istediğiniz Azure aboneliğini gösterir geçin. Bu Azure AD kiracısı B2C kiracısından farklı. Azure portalını Azure AD Kiracı seçmek için panonun sağ üst hesap adına tıklayın. Devam etmek için bir Azure aboneliği gereklidir. [Bir Azure aboneliği edinme](https://account.windowsazure.com/signup?showCatalog=True)

![Azure AD Kiracınıza değiştirme](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>3. adım - Azure Marketi'nde bir B2C Kiracı kaynak oluşturma
Market Pano sol üst köşesindeki "+" Market simgesine tıklayarak veya yeşil seçerek açın.  Arayın ve Azure Active Directory B2C seçin. Bu seçeneği belirleyin.

![Market seçin](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![AD B2C arama](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

Azure AD B2C kaynağı aşağıdaki parametreleri iletişim kapsar oluşturun:

1. Azure AD B2C Kiracısı – Azure AD B2C Kiracısı aşağı açılır listeden seçin.  Yalnızca uygun Azure AD B2C kiracılar gösterir.  Uygun B2C kiracılar bu koşullara uyan: B2C kiracısının genel Yöneticisi olduğunuz ve B2C Kiracı bir Azure aboneliğine şu anda ilişkili değil

2. B2C Kiracı etki alanı adı ile eşleşmesi için Azure AD B2C kaynak adı - seçilmiş

3. Abonelik - bir yönetici veya bir ortak yönetici olan bir etkin Azure aboneliği.  Birden çok Azure AD B2C Kiracı için bir Azure aboneliği eklenebilir

4. Kaynak grubu ve kaynak grubu konumu - bu yapı birden çok Azure kaynağı düzenlemenize yardımcı olur.  Bu seçenek B2C Kiracı konumu, performans veya faturalama durumu üzerinde bir etkisi yoktur

5. B2C Kiracı fatura bilgilerini ve B2C Kiracı ayarları kolay erişim için panoya Sabitle ![B2C kaynak oluşturma](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>4. adım - (isteğe bağlı), B2C Kiracı kaynaklarını yönetme
Dağıtım tamamlandıktan sonra yeni bir "B2C Kiracı" kaynak hedef kaynak grubunda oluşturduğunuz ve Azure abonelik ilgili.  Yeni bir kaynak türü "B2C Kiracı" eklenen yanı sıra diğer Azure kaynaklarınızı görmeniz gerekir.

![B2C kaynağı oluşturma](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

B2C Kiracı kaynak tıklayarak, olan
- Faturalama bilgileri gözden geçirmek için abonelik adını tıklatın. Faturalama ve kullanım bakın.
- Yeni tarayıcı sekmesinde doğrudan B2C kiracınız ayarları dikey penceresini açmak için Azure AD B2C ayarlar'ı tıklatın
- Destek talebi gönderme
- B2C Kiracı kaynak başka bir Azure aboneliği veya başka bir kaynak grubu taşıyın.  Bu seçim değişiklikleri kullanım ücretleri hangi Azure aboneliği alır.

![B2C kaynak ayarları](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bilinen sorunlar
- B2C Kiracı silme. B2C Kiracı oluşturduysanız, silinecek ve aynı etki alanı adıyla yeniden oluşturulacak Lütfen da silin ve "Bağlama" kaynak aynı etki alanı adıyla yeniden oluşturun.  Azure Portalı aracılığıyla abonelik Kiracı, bu "Bağlama" kaynak "Tüm kaynaklar" altında bulabilirsiniz.
- Bölgesel kaynak konumu Self potansiyel kısıtlamalar.  Nadir durumlarda, bir kullanıcı Azure kaynak oluşturma için bölgesel bir kısıtlama oluşturulmuş.  Bu kısıtlama, Azure aboneliği ve bir B2C Kiracı arasındaki bağlantıyı oluşturulmasını engelleyebilir. Etkisini azaltmak için bu kısıtlama hafifletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu adımları her B2C kiracılarınız için tamamlandıktan sonra Azure aboneliğinizin Azure doğrudan ya da Kurumsal Anlaşma ayrıntılarınızı göre faturalandırılır.
- Kullanım ve fatura seçili Azure aboneliğiniz içinde gözden geçirin
- Ayrıntılı gün gün kullanım raporlarını kullanarak gözden geçir [kullanım raporlama API'si](active-directory-b2c-reference-usage-reporting-api.md)
