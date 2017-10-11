---
title: "Azure AD Connect: Eşitleme hizmeti örnekleri | Microsoft Docs"
description: "Bu sayfa, Azure AD örnekleri için özel hususlar belgeler."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e8321c3d16253226a5931cacbce6fa5d50b697bd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Özel konular örnekleri
Azure AD Connect ile Azure AD dünya çapındaki örneğini en yaygın olarak kullanılan ve Office 365. Ancak aynı zamanda diğer örneği vardır ve bu URL'leri ve diğer özel durumlar için farklı gereksinimlere sahip.

## <a name="microsoft-cloud-germany"></a>Microsoft Bulut Almanya
[Microsoft Cloud Almanya](http://www.microsoft.de/cloud-deutschland) Almanca veri güvenliği tarafından işletilen bir sovereign bulut.

| Proxy sunucu açmak için URL'leri |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Sertifika iptal listeleri |

Azure AD kiracınıza oturum açtığınızda onmicrosoft.de etki alanındaki bir hesabı kullanmanız gerekir.

Özellikleri Microsoft Cloud Almanya şu anda yok:

* **Azure AD Connect Health** kullanılabilir değil.
* **Otomatik Güncelleştirmeler** kullanılabilir değil.
* **Parola geri yazma** 1.1.570.0 Azure AD Connect sürümüyle ve sonra Önizleme için kullanılabilir.
* Diğer Azure AD Premium hizmetlerine kullanılabilir değil.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure kamu bulut
[Microsoft Azure kamu bulut](https://azure.microsoft.com/features/gov/) ABD devlet kurumları için bir bulut.

Bu bulut DirSync önceki sürümleri tarafından desteklenen. Yapıdan Azure AD Connect 1.1.180, yeni nesil bulut desteklenir. Bu oluşturma yalnızca ABD tabanlı uç noktalarını kullanarak ve URL'leri proxy sunucunuzun açmak için farklı bir listesi vardır.

| Proxy sunucu açmak için URL'leri |
| --- |
| \*.microsoftonline.com |
| \*. microsoftonline.us |
| \*. gov.us.microsoftonline.com |
| + Sertifika iptal listeleri |

Azure AD Connect otomatik olarak Azure AD kiracınıza kamu bulutta bulunduğundan algılayabilir değil. Bunun yerine, Azure AD Connect yüklediğinizde aşağıdaki eylemleri gerçekleştirmeniz gerekir.

1. Azure AD Connect yüklemesi başlatın.
2. Burada EULA'yı kabul beklenir ilk sayfasını gördüğünüzde devam eder ancak Yükleme Sihirbazı'nı çalıştıran bırakın.
3. Regedit başlatın ve kayıt defteri anahtarını değiştirin `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` değerine `2`.
4. Azure AD Connect Yükleme Sihirbazı'için geri EULA'yı kabul edin ve devam gidin. Yükleme sırasında kullandığınızdan emin olun **özel yapılandırma** yükleme yolu (ve hızlı yükleme). Ardından yükleme işlemine devam edebilirsiniz.

Özellikleri Microsoft Azure kamu bulutta şu anda yok:

* **Azure AD Connect Health** kullanılabilir değil.
* **Otomatik Güncelleştirmeler** kullanılabilir değil.
* **Parola geri yazma** 1.1.570.0 Azure AD Connect sürümüyle ve sonra Önizleme için kullanılabilir.
* Diğer Azure AD Premium hizmetlerine kullanılabilir değil.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
