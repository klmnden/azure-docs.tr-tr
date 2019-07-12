---
title: Son kullanıcı deneyimlerini uygulamalar - Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD), kuruluşunuzdaki son kullanıcılar uygulamaları dağıtmak için çeşitli özelleştirilebilir yol sağlar.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/03/2019
ms.author: mimart
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74c6787068cf8ba1e86cbf43955d0ac995aa8de1
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67702108"
---
# <a name="end-user-experiences-for-applications-in-azure-active-directory"></a>Azure Active Directory'de uygulamalar için son kullanıcı deneyimleri

Azure Active Directory (Azure AD), kuruluşunuzdaki son kullanıcılar uygulamaları dağıtmak için çeşitli özelleştirilebilir yol sağlar:

* Azure AD erişim paneli
* Office 365 uygulama Başlatıcısı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Kuruluşunuzda dağıtmayı tercih hangi yöntemleri kümeleri olur.

## <a name="azure-ad-access-panel"></a>Azure AD erişim paneli

Adresinden erişim Paneli'nde https://myapps.microsoft.com Azure görüntülemek için Active Directory'de son kullanıcı bir kurumsal hesap ile sağlayan web tabanlı bir portal ve Azure AD yöneticinizin erişim verilen başlatma bulut tabanlı uygulamalar, olan. Son kullanıcı ile başladıysanız [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), ayrıca erişim paneli ile Self Servis Grup Yönetimi özellikleri kullanabilir.

![Azure AD erişim paneli portal ekran görüntüsü gösterir](media/what-is-single-sign-on/azure-ad-access-panel.png)

Erişim paneli, Azure Portalı'ndan ayrıdır ve kullanıcıların bir Azure aboneliği veya Office 365 aboneliğine sahip olmasını gerektirmez.

Azure AD erişim paneli hakkında daha fazla bilgi için bkz. [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="office-365-application-launcher"></a>Office 365 uygulama Başlatıcısı

Office 365 dağıtmış olan kuruluşlar için Azure AD üzerinden kullanıcılara atanan uygulamalar ayrıca Office 365 portalında görünür [ https://portal.office.com/myapps ](https://portal.office.com/myapps). Bu ikinci bir portal kullanmak zorunda kalmadan uygulamalarını başlatmak için bir kuruluştaki kullanıcılar için kolay getirir ve Office 365 kullanan kuruluşlar için önerilen uygulama başlatılırken çözüm.

![Office 365 portalı ekran görüntüsü gösterir](./media/end-user-experiences/microsoft-365-portal-office-com.png)

Office 365 uygulama başlatıcısında hakkında daha fazla bilgi için bkz: [uygulamanızın Office 365 uygulama başlatıcısında sahip](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

## <a name="direct-sign-on-to-federated-apps"></a>Birleştirilmiş uygulamalarda doğrudan oturum açma

SAML 2.0, WS-Federation ve Openıd destekleyen en Federasyon uygulamaları, kullanıcıların uygulamayı başlatın ve ardından Azure AD kullanarak otomatik yeniden yönlendirme veya bir bağlantıya tıklayarak oturum açmak için oturum de destek bağlanın. Bu hizmet sağlayıcısı olarak bilinir-oturum açma başlatılan ve Azure AD uygulama galerisinde en Federasyon uygulamalarına destek (Ayrıntılar için Azure Portalı'nda uygulamanın çoklu oturum açma Yapılandırması Sihirbazı'ndan belgelerine bağlı bu bakın).

![Bir mobil uygulama oturum açma sayfası örneği](./media/end-user-experiences/workdaymobile.png)

## <a name="direct-sign-on-links"></a>Oturum açma doğrudan bağlantılar

Azure AD çoklu oturum açma uygulamalar için doğrudan bağlantılar parola tabanlı çoklu oturum açma, bağlantılı çoklu oturum açma ve herhangi bir biçimde Federasyon çoklu oturum açmayı destekleyen tek tek de destekler.

Bu bağlantılar bir kullanıcı belirli bir uygulama için Azure AD oturum açma işlemi aracılığıyla bunları Azure ad erişim paneli veya Office 365 kullanıcı başlatma gerek kalmadan gönderme özel olarak hazırlanmış URL'leri verilmiştir. Bunlar **kullanıcı erişim URL'leri** mevcut kurumsal uygulamalar özelliklerini altında bulunabilir. Azure portalında **Azure Active Directory** > **kurumsal uygulamalar**. Uygulamayı seçin ve ardından **özellikleri**.

![Kullanıcı erişim URL'SİNDEN Twitter özelliklerinde örneği](media/end-user-experiences/direct-sign-on-link.png)

Bu bağlantıları kopyalanır ve seçilen uygulamanın oturum açma bağlantı sağlamak için istediğiniz yere yapıştırılan. Bu e-posta ya da kullanıcı uygulama erişimi için ayarladığınız tüm özel web tabanlı portal olabilir. Bir Azure AD doğrudan çoklu oturum açma URL'si için Twitter'ın bir örnek aşağıda verilmiştir:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Kuruluşa özgü URL'lere erişim paneli için benzer, daha fazla bu URL'yi dizininiz için etkin veya doğrulanmış etki alanlarından biri myapps.microsoft.com etki alanından sonra ekleyerek özelleştirebilirsiniz. Bu, tüm kurumsal markayı hemen oturum açma sayfasında kullanıcının ilk kullanıcı kimliği girildikten gerek yüklenir sağlar:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Yetkili bir kullanıcı bu uygulamaya özgü bağlantılardan birini tıkladığında, bunlar ilk olarak (Bunlar zaten oturum varsayılarak), Kurumsal oturum açma sayfasına bakın ve oturum açma işleminden sonra uygulamasının adresinden erişim Paneli'nde durdurmadan yönlendirilirsiniz. Kullanıcı, parola tabanlı çoklu oturum açma tarayıcı uzantısı gibi bir uygulamaya erişmek için ön koşullar eksik ardından bağlantıyı eksik uzantıyı yüklemek için girmesini ister. Bağlantı URL'si de uygulama için tek oturum açma yapılandırması değiştiğinde olursa sabit kalır.

Bu bağlantıları erişim panelinde ve Office 365 olarak aynı erişim denetimi mekanizmaları kullanın ve yalnızca bu kullanıcılar veya gruplar, Azure portalında uygulamanıza atanan kimliğini başarıyla doğrulamak mümkün olacaktır. Ancak, yetkilendirilmemiş herhangi bir kullanıcı, erişim verilmemiş ve erişim sahip oldukları mevcut uygulamaları görüntülemek için erişim paneli yüklemek için bir bağlantı verilen açıklayan bir ileti görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım planları için bkz: [Azure Active Directory dağıtım planları](../fundamentals/active-directory-deployment-plans.md)
