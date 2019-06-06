---
title: Erişim ve yeni Geliştirici Portalı - Azure API Management özelleştirme | Microsoft Docs
description: API Management'ta Geliştirici portalını yeni kullanmayı öğrenin.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: deb3fdf3069aaad4982d71806c209fe516e3c3d6
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743583"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Erişim ve Azure API Yönetimi'nde yeni Geliştirici portalını özelleştirme

Bu makalenin yeni Azure API Management Geliştirici portalına erişmek nasıl gösterir. Ekleme ve içeriği - düzenleme yanı sıra Web sitesinin görünüşünü özelleştirme visual Düzenleyicisi deneyimi - gösterilmektedir.

![Yeni API Management Geliştirici Portalı](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a>Önkoşullar

- Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- İçeri aktarma ve Azure API Management örneği yayımlama. Daha fazla bilgi için [içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> Yeni Geliştirici Portalı şu anda Önizleme aşamasındadır.

## <a name="managed-and-self-hosted-versions"></a>Yönetilen ve şirket içinde barındırılan sürümleri

Geliştirici portalınızın iki şekilde oluşturabilirsiniz:

- **Yönetilen sürümün** - düzenleyerek ve Portalı'nda yerleşik olarak bulunan API Management örneğinizin özelleştirme ve URL ile erişilebilir `<your-api-management-instance-name>.developer.azure-api.net`.
- **Şirket içinde barındırılan sürümünün** - dağıtarak ve API Management örneği dışında portalınız kendi kendine barındırma. Bu yaklaşım portal'ın düzenlemenize olanak sağlayacak kod temeli ve sağlanan çekirdek işlevselliğini genişleten. Ayrıntılı bilgi ve yönergeler için başvurmak [portal'ın kaynak kodunu içeren GitHub depomuza][1].

## <a name="access-the-managed-version-of-the-portal"></a>Erişim Portalı'nın yönetilen sürümü

Portalda yönetilen sürümüne erişmek için aşağıdaki adımları izleyin.

1. API Management hizmet örneğinizin Azure portalındaki gidin.
1. Tıklayarak **yeni Geliştirici Portalı'nı (Önizleme)** üst gezinti çubuğunda düğme. Yönetim Portalı sürümüne sahip yeni bir tarayıcı sekmesi açılır. Portala ilk kez erişiyorsanız, varsayılan içerik otomatik olarak sağlanır.

## <a name="edit-and-customize-the-managed-version-of-the-portal"></a>Düzenleme ve Portalı'nın yönetilen sürümü özelleştirme

Videoda biz portalı içeriği Düzenle, Web sitesinin görünümünü özelleştirme ve değişiklikleri yayımlamak nasıl ekleyebileceğiniz gösterilmektedir.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="next-steps"></a>Sonraki adımlar

Yeni Geliştirici Portalı hakkında daha fazla bilgi edinin:

- [Kaynak kodunu içeren GitHub depomuza][1]
- [Portal kendi kendine barındırma yönergeleri][2]
- [Projenin genel yol haritası][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects