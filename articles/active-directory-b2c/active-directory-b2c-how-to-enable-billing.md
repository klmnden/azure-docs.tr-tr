---
title: Azure aboneliğinin Azure Active Directory B2C'ye bağlama | Microsoft Docs
description: Bir Azure aboneliği ile Azure AD B2C kiracısı için faturalama için adım adım kılavuzu.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 12/05/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 80ba42d7eab1726c7add6c4c9426b7dde3b55480
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445974"
---
# <a name="linking-an-azure-subscription-to-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı için bir Azure aboneliğine bağlama

> [!IMPORTANT]
> Azure AD B2C şu sayfa için fiyatlandırma ve faturalama kullanım en son bilgiler: [Azure AD B2C fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-b2c/)

Azure AD B2C için kullanım ücretleri, bir Azure aboneliğine faturalandırılır. Azure AD B2C kiracısı oluşturulduğunda, Kiracı Yöneticisi açıkça Azure AD B2C kiracısı bir Azure aboneliğine bağlamak gerekir. Bu makalede, nasıl gösterir.

> [!NOTE]
> Bir Azure AD B2C kiracısıyla bağlantılı bir abonelik yalnızca Azure AD B2C kullanımı, faturalandırma için kullanılır. Aboneliği diğer Azure hizmetlerine veya Office 365 lisansı eklemek için kullanılamaz *Azure AD B2C kiracısı içinde*.

 Abonelik bağlantısını içinde ' % s'hedef Azure aboneliğinin bir Azure AD B2C "kaynak" oluşturarak elde edilir. Diğer Azure kaynakları (örneğin, VM'ler, veri depolama için LogicApps) ile birlikte tek bir Azure aboneliği içindeki birçok Azure AD B2C "Kaynaklar" oluşturulabilir. Aboneliğin ilişkili olduğu Azure AD kiracısı giderek Abonelikteki kaynakların tümünü görebilirsiniz.

Devam etmek için geçerli bir Azure aboneliği gereklidir.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

Öncelikle [bir Azure AD B2C kiracısı oluşturmayı](active-directory-b2c-get-started.md) aboneliği bağlamak istiyor. Azure AD B2C kiracısı oluşturduysanız bu adımı atlayın.

## <a name="open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Azure aboneliğinizi gösteren Azure AD kiracısında Azure portalını Aç

Azure aboneliğinizi gösteren Azure AD kiracısına gidin. Açık [Azure portalında](https://portal.azure.com), kullanmak istediğiniz Azure aboneliğini gösteren Azure AD kiracısına geçiş.

![Azure AD kiracınıza geçiş yapma](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="find-azure-ad-b2c-in-the-azure-marketplace"></a>Azure Market'te Azure AD B2C'yi bulun

Tıklayın **kaynak Oluştur** düğmesi. **Markette ara** alanına `B2C` yazın.

![Aramaya Market alan düğmesi vurgulanmış ve Azure AD B2C metin Ekle](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c.png)

Sonuç listesinden **Azure AD B2C**.

![Sonuç listesinde Azure AD B2C seçili](../../includes/media/active-directory-b2c-create-tenant/find-azure-ad-b2c-result.png)

Azure AD B2C hakkında ayrıntılar gösterilir. Yeni Azure Active Directory B2C kiracınızı yapılandırmaya başlamak için **Oluştur** düğmesine tıklayın.

Kaynak oluşturma ekranında seçin **Azure Aboneliğimi bağlantı var olan bir Azure AD B2C Kiracısına**.

## <a name="create-an-azure-ad-b2c-resource-within-the-azure-subscription"></a>Azure aboneliğindeki bir Azure AD B2C kaynağı oluşturun

Kaynak oluşturma iletişim kutusunda, açılır listeden bir Azure AD B2C kiracısı seçin. Tüm genel Yöneticisi ve zaten bir aboneliğe bağlı değil o olan kiracılar görürsünüz.

Azure AD B2C kaynak adı, Azure AD B2C kiracısı etki alanı adını eşleştirmek için seçilmiş.

Abonelik için Yöneticisi olduğunuz bir etkin Azure aboneliği seçin.

Bir kaynak grubu ve kaynak grubu konumu seçin. Seçimin Burada, Azure AD B2C Kiracı konumu, performans veya faturalama durumu herhangi bir etkisi yoktur.

![B2C kaynağı oluşturma](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Azure AD B2C kiracısı kaynaklarınızı yönetin

Azure aboneliğinde bir Azure AD B2C kaynağı başarıyla oluşturulduktan sonra yeni bir kaynak türü "B2C kiracısı diğer Azure kaynaklarınızın yanı sıra eklendi" görmeniz gerekir.

Bu kaynak için kullanabilirsiniz:

- Faturalama bilgileri gözden geçirmek için abonelik gidin.
- Azure AD B2C kiracınıza gitmek
- Destek talebi gönderme
- Azure AD B2C Kiracı kaynak başka bir Azure aboneliğine veya başka bir kaynak grubuna taşıyın.

![B2C kaynağı ayarları](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="csp-subscriptions"></a>CSP abonelikleri

Şu anda Azure AD B2C kiracısı **olamaz** CSP abonelikleri bağlantı.

### <a name="self-imposed-restrictions"></a>Kendi kendine kıldığını kısıtlamaları

Bir kullanıcı Azure kaynak oluşturma için bölgesel bir kısıtlama oluşturulmuş. Bu kısıtlama, Azure AD B2C kaynağı oluşturulmasını engelleyebilir. Azaltmak için lütfen bu kısıtlama hafifletin.

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları her Azure AD B2C kiracılarınız için tamamlandıktan sonra Azure aboneliğinizi, Azure Direct veya Kurumsal Anlaşma ayrıntılarını uygun olarak faturalandırılır.

Seçili Azure aboneliğiniz kullanım ve fatura ayrıntılarını gözden geçirebilirsiniz. Ayrıntılı günlük olarak günlük kullanım raporları kullanarak da gözden geçirebilirsiniz [kullanım raporlama API'sini](active-directory-b2c-reference-usage-reporting-api.md).
