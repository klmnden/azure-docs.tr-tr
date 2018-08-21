---
title: Hizmet olarak Azure Stack doğrulama iş akışı ortak parametrelerinin | Microsoft Docs
description: İş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 81a7be973739cfd6eb3f8fb8dc7a0723623c2b8e
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235143"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>İş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Ortak parametreleri içeren değerleri gibi ortam değişkenlerini ve kullanıcı kimlik bilgilerini doğrulama (VaaS) hizmet olarak tüm testler için gerekli. Bu değerler iş akışı düzeyinde tanımlarsınız. Bir iş akışı oluşturduğunuzda veya düzenlediğinizde değerleri kaydedin. Zaman Çizelgesi iş akışı test değerleri yükler. 

## <a name="environment-parameters"></a>Ortam parametreleri

Test edilen Azure Stack ortamınıza ortam parametrelerini açıklar. Bu değerler sağlanmalıdır, oluşturma ve damga yapılandırma dosyanızı karşıya yükleme `&lt;link&gt;. [How to get the stamp info link].`

| Parametre adı | Gerekli | Tür | Açıklama |
|----------------------------------|----------|------|---------------------------------------------------------------------------------------------------------------------------------|
| Azure Stack derleme | Gerekli |  | Derleme numarası (örneğin, 1.0.170330.9) Azure Stack dağıtımı |
| OEM sürümü | Evet |  | Azure Stack dağıtımı sırasında kullanılan OEM paketin sürüm numarası. |
| OEM imzası | Evet |  | Azure Stack dağıtımı sırasında kullanılan OEM paket imzası. |
| AAD Kiracı kimliği | Gerekli |  | Azure Active Directory kiracısı Azure Stack dağıtım sırasında belirtilen GUID.|
| Bölge | Gerekli |  | Azure Stack Dağıtım bölgesi. |
| Kiracı Resource Manager uç noktası | Gerekli |  | Kiracı Azure Resource Manager işlemleri için uç nokta (örneğin, https://management.<ExternalFqdn>) |
| Yönetici Resource Manager uç noktası | Evet |  | Kiracı Azure Resource Manager işlemleri için uç nokta (örneğin, https://adminmanagement.<ExternalFqdn>) |
| Dış FQDN | Evet |  | Dış uç noktaları için sonek olarak kullanılan tam etki alanı adı. (örneğin, local.azurestack.external veya redmond.contoso.com). |
| Düğüm sayısı | Evet |  | Dağıtımda düğüm sayısı. |

## <a name="test-parameters"></a>Test parametreleri

Ortak test parametreleri yapılandırma dosyalarında depolanan olamaz hassas bilgiler içerir ve el ile sağlanmalı.

| Parametre adı | Gerekli | Tür | Açıklama |
|--------------------------------|------------------------------------------------------------------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kiracı Kullanıcı adı | Gerekli |  | Azure ya da zaten sağlanmış Active Directory Kiracı Yöneticisi veya AAD dizinde Hizmet Yöneticisi tarafından sağlanması gerekiyor. Kiracı hesabı sağlama hakkında daha fazla bilgi için bkz [Azure AD'yi kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). Bu değer test çalıştırmasından kaynakları sağlamak için şablonları dağıtmak gibi Kiracı düzeyi işlemleri gerçekleştirmek için kullanılır (VM'ler, depolama hesaplarını vb.) ve iş yüklerini çalıştırın. Bu değer test çalıştırmasından kaynakları sağlamak için şablonları dağıtmak gibi Kiracı düzeyi işlemleri gerçekleştirmek için kullanılır (VM'ler, depolama hesaplarını vb.) ve iş yüklerini çalıştırın. |
| Kiracı parolası | Gerekli |  | Kiracı Kullanıcı parolası. |
| Hizmet Yöneticisi kullanıcı adı | Gerekiyor: Çözüm doğrulama, paket doğrulama<br>Gerekli değildir: Test geçiş |  | Azure Active Directory Yöneticisi, AAD dizin Kiracısında Azure Stack dağıtım sırasında belirtilen. |
| Hizmet Yöneticisi parolası | Gerekiyor: Çözüm doğrulama, paket doğrulama<br>Gerekli değildir: Test geçiş |  | Hizmet Yöneticisi kullanıcı parolası. |
| Bulut yönetici kullanıcı adı | Gerekli |  | Azure Stack etki alanı yöneticisi hesabı (örneğin, contoso\cloudadmin). Kullanıcı rolü için arama yapılandırma dosyasındaki "CloudAdmin" = ve yapılandırma dosyasında kullanıcı adı etiketi değerini seçin. |
| Bulut yönetici parolası | Gerekli |  | Bulut yönetici kullanıcı parolası. |
| Tanılama bağlantı dizesi | Gerekli |  | Bir Azure depolama hesabı için hangi tanılama günlükleri test yürütme sırasında kopyalanacak bir SAS URI'si. SAS URI'sini oluşturmak için yönergeler yer [bir blob depolama hesabı ayarlama](azure-stack-vaas-set-up-account.md). |


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
