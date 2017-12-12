---
title: "Azure AD B2C'ye bir Azure aboneliğine bağlanma | Microsoft Docs"
description: "Azure AD B2C kiracısı için fatura içine bir Azure aboneliği etkinleştirmek için adım adım kılavuzu."
services: active-directory-b2c
documentationcenter: dev-center-name
author: parakhj
manager: mtillman
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2017
ms.author: parja
ms.openlocfilehash: d6b25d6b9a0d5b3bcf613046a82a9c6c99475d6c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="linking-an-azure-subscription-to-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı için bir Azure aboneliği bağlama

> [!IMPORTANT]
> Faturalama ve Azure AD B2C aşağıdaki sayfanın için fiyatlandırma kullanımı hakkında en son bilgileri: [Azure AD B2C fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-b2c/)

Kullanım ücretleri Azure AD B2C için bir Azure aboneliğine faturalandırılır. Azure AD B2C kiracısı oluşturduğunuzda, bir Azure aboneliğine Azure AD B2C kiracısı açıkça bağlanmak Kiracı Yöneticisi gerekir. Bu makale size nasıl gösterir.

> [!NOTE]
> Bir Azure AD B2C kiracısı bağlantılı bir abonelik yalnızca Azure AD B2C kullanımı için fatura kullanılabilir. Diğer Azure hizmetlerine veya Office 365 lisansları eklemek için abonelik kullanılamaz *Azure AD B2C kiracısı içinde*.

 Abonelik bağlantısına hedef Azure aboneliği içindeki bir Azure AD B2C "kaynak" oluşturarak elde edilir. Çok sayıda Azure AD B2C "Kaynaklar" diğer Azure kaynakları (örneğin, sanal makineleri, veri depolama, LogicApps) yanı sıra tek bir Azure aboneliği içindeki oluşturulabilir. Abonelik için ilişkili Azure AD kiracısı giderek abonelik içindeki kaynakların tümünü görebilirsiniz.

Devam etmek için geçerli bir Azure aboneliği gereklidir.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

Öncelikle [bir Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) aboneliği bağlamak istiyor. Azure AD B2C kiracısı zaten oluşturduysanız bu adımı atlayın.

## <a name="open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Açık Azure portalında Azure aboneliğinize gösterir Azure AD kiracısı

Azure aboneliğiniz gösteren Azure AD kiracısı gidin. Açık [Azure portal](https://portal.azure.com)ve anahtar Azure AD Kiracı için kullanmak istediğiniz Azure aboneliği gösterir.

![Azure AD kiracınıza değiştirme](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="find-azure-ad-b2c-in-the-azure-marketplace"></a>Azure AD B2C Azure Marketi'nde bulma

**Yeni** düğmesine tıklayın. **Markette ara** alanına `B2C` yazın.

![Düğmesi vurgulanmış ve Azure AD B2C metin arama Market alan ekleme](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c.png)

Sonuçlar listesinde **Azure AD B2C**.

![Azure AD B2C sonuçlar listesinde seçili](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c-result.png)

Azure AD B2C ilgili ayrıntılar gösterilir. Yeni Azure Active Directory B2C kiracınızı yapılandırmaya başlamak için **Oluştur** düğmesine tıklayın.

Kaynak oluşturma ekranında seçin **Azure Aboneliğim bağlantı var olan bir Azure AD B2C Kiracısına**.

## <a name="create-an-azure-ad-b2c-resource-within-the-azure-subscription"></a>Azure aboneliği içindeki bir Azure AD B2C kaynak oluşturma

Kaynak oluşturma iletişim kutusunda, bir Azure AD B2C Kiracı aşağı açılır listeden seçin. Tüm genel Yöneticisi ve aboneliği zaten bağlı olmayan olanlar olan kiracılar görürsünüz.

Azure AD B2C kaynak adı, Azure AD B2C Kiracı etki alanı adı ile eşleşmesi için seçilmiş.

Abonelik için Yöneticisi olduğunuz bir etkin Azure aboneliği seçin.

Bir kaynak grubu ve kaynak grubu konumu seçin. Seçim burada Azure AD B2C Kiracı konumu, performans veya faturalama durumu üzerinde etkisi yoktur.

![B2C kaynağı oluşturma](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="manage-your-azure-ad-b2c-tenent-resources"></a>Azure AD B2C Kiracı kaynaklarını yönetme

Bir Azure AD B2C kaynak içinde Azure aboneliği başarıyla oluşturulduktan sonra yeni bir kaynak türü "B2C Kiracı yanı sıra diğer Azure kaynaklarınızı eklenen" görmeniz gerekir.

Bu kaynak için kullanabilirsiniz:

- Faturalama bilgileri gözden geçirmek için abonelik gidin.
- Azure AD B2C kiracınızın gidin
- Destek talebi gönderme
- Azure AD B2C Kiracı kaynak başka bir Azure aboneliğine veya başka bir kaynak grubuna taşıyın.

![B2C kaynak ayarları](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="csp-subscriptions"></a>CSP abonelikleri

Şu anda Azure AD B2C kiracısı **olamaz** CSP abonelikleri bağlantı.

### <a name="self-imposed-restrictions"></a>Kendi kendine potansiyel kısıtlamaları

Bir kullanıcı Azure kaynak oluşturma için bölgesel bir kısıtlama oluşturulmuş. Bu kısıtlama, Azure AD B2C kaynak oluşturulmasını engelleyebilir. Etkisini azaltmak için bu kısıtlama hafifletin.

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları her Azure AD B2C kiracılarınız için tamamlandıktan sonra Azure aboneliğinizin Azure doğrudan ya da Kurumsal Anlaşma ayrıntılarınızı göre faturalandırılır.

Seçili Azure aboneliğiniz içinde kullanım ve fatura ayrıntılarına gözden geçirebilirsiniz. Kullanarak ayrıntılı gün gün kullanım raporlarını gözden geçirebilirsiniz [API raporlama kullanım](active-directory-b2c-reference-usage-reporting-api.md).
