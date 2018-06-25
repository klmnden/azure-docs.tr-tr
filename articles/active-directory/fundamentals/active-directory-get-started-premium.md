---
title: Azure AD Premium’a kaydolma | Microsoft Docs
description: Azure Active Directory Premium sürümü için kayıt işleminin nasıl yapıldığını açıklar
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/07/2017
ms.author: lizross
ms.reviewer: piotrci
ms.custom: it-pro;
ms.openlocfilehash: c15cbb632410eb0b6867677d7802960033dfdd44
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268391"
---
# <a name="quickstart-sign-up-for-azure-active-directory-premium"></a>Hızlı Başlangıç: Azure Active Directory Premium'a kaydolma
Azure Active Directory (Azure AD) Premium ile çalışmaya başlamak için lisans satın alabilir ve aldığınız lisansları Azure aboneliğinizle ilişkilendirebilirsiniz. Yeni bir Azure aboneliği oluşturursanız, lisans planınızı ve aşağıdaki bölümlerde açıklandığı gibi Azure AD hizmeti erişimini etkinleştirmeniz gerekir. 

## <a name="sign-up-for-active-directory-premium"></a>Active Directory Premium'a kaydolma
Active Directory Premium'a kaydolmanız için birkaç seçenek sunulmaktadır: 
* Azure veya Office 365 aboneliğinizi kullanın
* Enterprise Mobility + Security lisans planı kullanın
* Microsoft Toplu Lisanslama planı kullanın

### <a name="azure-or-office-365"></a>Azure veya Office 365 
Azure veya Office 365 abonesi olarak, Azure Active Directory Premium'u çevrimiçi satın alabilirsiniz. 

Ayrıntılı adımlar için bkz. [Azure Active Directory Premium'u satın alma - Mevcut Müşteriler](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-Existing-Customer) veya [Azure Active Directory Premium'u satın alma - Yeni Müşteriler](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-New-Customers).  

### <a name="enterprise-mobility--security"></a>Enterprise Mobility + Security
Microsoft Enterprise Mobility + Security (EMS), kuruluşların şu hizmetleri tek lisans planı kapsamında birlikte kullanmasını sağlayan uygun maliyetli bir yöntemdir: Azure Active Directory Premium, Azure Information Protection ve Microsoft Intune. EMS hakkında daha fazla bilgiyi [Enterprise Mobility + Security web sitesinde](https://www.microsoft.com/cloud-platform/enterprise-mobility-security), satın alabileceğiniz EMS lisans türleriyle ilgili daha fazla bilgiyi [Enterprise Mobility + Security Fiyatlandırma Seçenekleri](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-pricing) sayfasında bulabilirsiniz.  

Aşağıdaki lisans seçeneklerinden birini kullanarak EMS lisanslarını aracılığıyla Azure AD kullanmaya başlayabilirsiniz:

- EMS’yi, ücretsiz bir [Enterprise Mobility + Security E5 deneme aboneliği](https://signup.microsoft.com/Signup?OfferId=87dd2714-d452-48a0-a809-d2f58c4f68b7&ali=1) ile deneyebilirsiniz.
- [Enterprise Mobility + Security E5 lisansı](https://signup.microsoft.com/Signup?OfferId=e6de2192-536a-4dc3-afdc-9e2602b6c790&ali=1) satın alma
- [Enterprise Mobility + Security E3 lisansı](https://signup.microsoft.com/Signup?OfferId=4BBA281F-95E8-4136-8B0F-037D6062F54C&ali=1) satın alma

### <a name="microsoft-volume-licensing"></a>Microsoft toplu lisanslama
Azure Active Directory Premium, bir [Microsoft Kurumsal Anlaşma](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx) (250 veya daha fazla lisans) aracılığıyla veya [Açık Toplu Lisans](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx) (5 - 250 lisans) programı ile sunulmaktadır.

Toplu lisans satın alma seçenekleri hakkında daha fazla bilgiyi [Toplu Lisanslama ile satın alma](https://www.microsoft.com/en-us/Licensing/how-to-buy/how-to-buy.aspx) sayfasında bulabilirsiniz.

> [!NOTE]
> Azure Active Directory Premium ve Basic sürümleri, Azure Active Directory'nin dünya çapındaki örneğini kullanan Çin'deki müşterilerin kullanımına sunulmuştur. Azure Active Directory Premium ve Basic sürümleri, şu anda Çin'de 21Vianet tarafından işletilen Microsoft Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için [Azure Active Directory Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/)'nda bizimle iletişime geçin.

Önceki adımlarda kullandığınız Azure aboneliği için daha önce Azure AD lisansı alıp etkinleştirdiyseniz lisanslar otomatik olarak aynı dizinde etkinleştirilir. Bunu yapmadıysanız bu makalenin geri kalanında açıklanan adımları takip etmeniz gerekmez.

## <a name="activate-your-license-plan"></a>Lisans planınızı etkinleştirme
Bu Microsoft'tan satın aldığınız ilk Azure AD lisans planınız mı? Öyleyse, satın alma işleminiz tamamlandığında bir onay e-postası oluşturulur ve size gönderilir. İlk lisans planınızı etkinleştirmek için bu e-postaya ihtiyacınız vardır.

**Lisans planınızı etkinleştirmek için aşağıdaki adımlardan birini gerçekleştirin:**

1. Etkinleştirmeyi başlatmak için, **Oturum Aç** veya **Kaydol** seçeneğine tıklayın.
   
    ![Oturum aç](media/active-directory-get-started-premium/MOLSEmail.png)

    - Mevcut bir kiracınız varsa mevcut yönetici hesabınızla oturum açmak için **Oturum Aç** seçeneğine tıklayın. Lisansların etkinleştirilmesi için kullanılması gereken kiracıdan genel yönetici kimlik bilgileriyle oturum açın.

    - Lisans planınızla kullanmak üzere yeni bir Azure AD kiracısı oluşturmak istiyorsanız **Hesap Profili Oluştur** iletişim kutusunu açmak için **Kaydol** seçeneğine tıklayın.

        ![Hesap profili oluşturma](media/active-directory-get-started-premium/MOLSAccountProfile.png)

İşleminiz tamamlandığında, kiracınız için lisans planının etkinleştirilmesinin onayı olarak aşağıdaki iletişim kutusu görünür:

![Onay](media/active-directory-get-started-premium/MOLSThankYou.png)

## <a name="activate-your-azure-active-directory-access"></a>Azure Active Directory erişiminizi etkinleştirme
Mevcut bir aboneliğe yeni Azure AD Premium lisansları ekliyorsanız Azure AD erişiminiz zaten etkinleştirilmiştir. Aksi halde **Hoş Geldiniz e-postasını** aldıktan sonra Azure AD erişiminizi etkinleştirmeniz gerekir.  

Satın aldığınız lisanslar, dizininize sağlandığında size bir **Hoş geldiniz e-postası** gönderilir. E-posta, Azure Active Directory Premium veya Enterprise Mobility + Security lisanslarınızı ve özelliklerinizi yönetmeye başlayabileceğinizi onaylar. 

> [!TIP]
> Lisans sağlama işlemi tamamlandıktan sonra otomatik olarak gönderilen Hoş Geldiniz e-postasını kullanarak Azure AD directory erişiminizi etkinleştirme kadar yeni kiracınız için Azure AD’ye erişemezsiniz. 

**Azure AD erişiminizi etkinleştirmek için aşağıdaki adımları uygulayın:**

1. **Hoş geldiniz e-postanızda**, **Oturum Aç**'a tıklayın. 
   
    ![Hoş geldiniz e-postası](media/active-directory-get-started-premium/AADEmail.png)
2. Başarıyla oturum açtıktan sonra bir mobil cihaz kullanarak ikinci bir kimlik doğrulaması daha yapmanız gerekir:
   
    ![Mobil doğrulama](media/active-directory-get-started-premium/SignUppage.png)

Etkinleştirme yalnızca birkaç dakika sürer ve ardından Azure AD’ye yönetim erişiminiz olur. 

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Azure AD Premium için kaydolmayı ve Azure Active Directory erişiminizi etkinleştirmeyi öğrendiniz. 

Bir Azure aboneliğiniz zaten varsa, aşağıdaki bağlantıyı kullanarak bir deneme başlatabilir veya Azure portalından Azure AD Premium lisansları satın alabilirsiniz.

> [!div class="nextstepaction"]
> [Azure AD Premium lisanslarını etkinleştirme](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/TryBuyProductBlade)