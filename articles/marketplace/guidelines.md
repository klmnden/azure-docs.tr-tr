---
title: Azure Market ve AppSource yayımcı için yönergeler | Azure
description: Uygulama ve hizmet yayımcılar için Azure Marketi ve AppSource için yönergeler
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: 135f934cd6b352dad9e4cea5a14406804f31b66b
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825335"
---
# <a name="guidelines"></a>Yönergeler  

<!--
## Guidelines for AppSource  
-->
---  

## <a name="guidelines-for-azure-marketplace"></a>Azure Market yönergeleri  

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-a-marketplace-account"></a>Market hesabı yönetmek için a Microsoft ID oluşturma yönergeleri  
Birden çok kişi Market hesabınızı oluşturmak için kullanılan aynı Microsoft ID erişimi gerektiriyorsa, bir şirket hesabı oluşturmanıza yardımcı olması için aşağıdaki yönergelere uygun olmalıdır. 

>[!IMPORTANT]
>Microsoft Developer Center'da (Dev Center) hesabınıza erişmek için birden çok kullanıcı yetkilendirmek için Microsoft Azure Active Directory (Azure AD) bireysel kullanıcılara rolleri atamak için kullanmanızı önerir. Her kullanıcı hesabı tek oturum imzalayarak erişmelisiniz Azure AD kimlik. Bir etki alanında bir e-posta adresi kullanarak Microsoft ID oluşturun şirketiniz için kayıtlı Microsoft e-posta için bir kişinin atanmaması önerir. `windowsapps@fabrikam.com` bunun bir örneğidir.  
>*   Daha fazla bilgi için ziyaret [sorun: Microsoft ID Azure AD etki alanı Federasyon](#issue:-microsoft-id-in-an-azure-ad-federated-domain) bölümü.  

*   Geliştiriciler en küçük olası sayıyı Microsoft ID erişimi sınırlayın. 
*   Geliştirici Merkezi hesabınızda erişmelisiniz herkes içeren şirket e-posta dağıtım listesini (DT) olarak ayarlayın. DL e-posta adresi güvenlik bilgilerinizi ekleyin. DL istendiğinde güvenlik kodlarını almak ve Microsoft ID. için güvenlik bilgilerini yönetmek için tamamı listesindeki sağlar Bir dağıtım listesi oluşturarak uygun değilse, tek tek e-posta hesabının sahibi erişmek ve istendiğinde güvenlik kodu paylaşmak kullanılabilir olmalıdır.  
    *   Örneğin, yeni güvenlik bilgileri Microsoft ID veya yeni bir CİHAZDAN Microsoft ID erişildiğinde eklendiğinde sahibi istenir.  
*   Uzantı gerektirmez ve anahtar takım üyeleri için erişilebilir olan bir şirket telefon numarası ekleyin.  
*   Genel olarak, geliştiriciler Geliştirici Merkezi hesabınızda oturum açmak için güvenilen cihazlar kullanmanızı gerektirir. Tüm anahtar ekip üyelerinin güvenilen cihazları erişiminiz olması. Güvenilen cihazlar erişmek için kullandığı birisi Geliştirici Merkezi hesabına erişirken güvenlik kodlarını göndermek için gereksinim azaltır.  
*   Güvenilir olmayan bir bilgisayardan Geliştirici Merkezi hesabına erişim için gerekliyse, beşten fazla geliştiriciler erişimi sınırlamanız gerekir. İdeal olarak, geliştiricilere aynı coğrafi paylaşan ve ağ konumu bilgisayarlardan hesabınıza erişmeniz.  
*   Sık gözden geçirin ve güvenlik bilgilerinizi doğrulayın.  
    *   Güvenlik bilgilerinizi görüntülemek için Ayarlar sayfasında bulunan güvenlik ziyaret [account.live.com/proofs/Manage](https://account.live.com/proofs/Manage).

Geliştirici Merkezi hesabınızda öncelikle Güvenilen bilgisayarlardan erişilmesi. Haftalık Geliştirme Merkezi hesabı başına oluşturulan kodlarını sayısına bir sınır olduğundan güvenilen bilgisayarlardan erişecekseniz önemlidir. Güvenilen bilgisayarlara kullanarak, en güvenli ve tutarlı oturum açma deneyimi sağlar. 
*   Ek Geliştirme Merkezi hesabı kılavuzları ve güvenlik hakkında daha fazla bilgi için bir geliştirici hesabı sayfası bulunan açılış ziyaret [docs.microsoft.com/windows/uwp/publish/opening-a-developer-account](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account). 

---  

#### <a name="issue-microsoft-id-in-an-azure-ad-federated-domain"></a>Sorun: Microsoft Azure AD Federasyon etki alanını ID  
Şirket hesabınızı Azure Active Directory (Azure AD) ile Federasyon. A Microsoft Azure AD ile birleştirildiyse bir şirket e-posta adresini kullanarak ID oluşturmayı denerseniz, bir hata alırsınız. Bir hata alırsanız, daha sonra Azure AD üzerinden hesabınızın Federal onaylamak için BT ekibiniz başvurmanız gerekir. Azure AD federe e-posta bilinen bir sorundur ve çözmek için Microsoft çalışma.  
*   Azure AD hakkında daha fazla bilgi için Azure Active Directory sayfasında bulunan belgeleri ziyaret [docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory).

Microsoft, geçici bir çözüm önerir. İçinde yeni bir e-posta adresi oluşturmak için aşağıdaki adımları izleyin `outlook.com` etki alanı ve iletişimlerin iletmek için bir kural oluşturun.  
1.  Oluşturma hesabı sayfasına gidin ve yeni bir e-posta adresi bağlantı almada'ı tıklatın. 
    *   Microsoft ID için kaydolmak için konumunda bulunan oluşturma hesap sayfasını ziyaret edin [signup.live.com/signup](https://signup.live.com/signup).  
2.  Yeni bir e-posta adresi oluşturun ve bir parola girin. A yeni Microsoft ID ve bir e-posta kutunuzdaki `outlook.com` etki alanı oluşturulur. Hesap oluşturulduktan kadar kayıt işlemine devam edin.  

    >[!IMPORTANT]
    >Geliştirme Merkezi'ne kaydetmek için a Microsoft ID olarak kayıtlı bir e-posta adresi veya dağıtım listesi kullanmanız gerekir. Microsoft, kişilere bağımlılığı kaldırmak için bir dağıtım listesi kullanmanızı önerir. E-posta adresi veya dağıtım listesi kayıtlı değilse, daha sonra artık kaydetmeniz gerekir.    

    >[!Important]
    >Herhangi bir e-posta adresi, bulunan `Microsoft` şirket etki alanı, sonra da geliştirme Merkezi'ndeki kaydı için kullanmak mümkün değildir.  

3.  Outlook e-posta adresiyle Microsoft ID oluşturduktan sonra Outlook posta oturum açın. Bir e-posta iletme kuralı oluşturun. Kural iletme e-posta Market hesabınızı yönetmek için oluşturulan etki alanınızdaki e-posta adresine Outlook posta kutusunda alınan tüm e-postaları taşımanız gerekir.  
    *   Outlook posta oturum için konumunda bulunan Outlook sayfasını ziyaret edin [outlook.live.com/owa](https://outlook.live.com/owa).  
    *   İletme kuralları hakkında daha fazla bilgi için kullanım kuralları otomatik olarak konumunda bulunan başka bir hesap sayfasına iletilerini iletmek için Outlook Web App'te ziyaret [support.office.com/article/ Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed](https://support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed).  

1.  İletme kuralı tüm e-posta ve şirketiniz için kayıtlı bir etki alanındaki e-posta adresine Outlook e-posta hesabı alınan iletişimleri gönderir. `outlook.com` E-posta adresi, Geliştirici Merkezi ve bulut iş ortağı Portalı'kimlik doğrulaması için kullanılmalıdır.  

## <a name="next-steps"></a>Sonraki adımlar
*   Ziyaret [Azure Marketi ve AppSource yayımcı Kılavuzu](./marketplace-publishers-guide.md) sayfası.  
 
---  
