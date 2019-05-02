---
title: Azure Market ve AppSource yayımcı Kılavuzu | Azure
description: Uygulama ve hizmet yayımcılar için Azure Market ve AppSource yönergeleri
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: jm-aditi-ms
manager: pabutler
ms.service: marketplace
ms.topic: article
ms.date: 06/13/2018
ms.author: ellacroi
ms.openlocfilehash: 05bb531a88677125318ddc23563cd08a3901aa4e
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64937938"
---
# <a name="guidelines"></a>Yönergeler  

<!--
## Guidelines for AppSource  
-->
---

## <a name="guidelines-for-azure-marketplace"></a>Azure Marketi için yönergeler  

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-a-marketplace-account"></a>Bir Market hesabını yönetmek için a Microsoft ID oluşturma yönergeleri  
Ardından birden fazla kişi Market hesabınızı oluşturmak için kullanılan aynı Microsoft ID erişim gerektiriyorsa, bir şirket hesabı oluşturmanıza yardımcı olması için bu yönergeleri izlemelidir. 

>[!IMPORTANT]
>Microsoft Developer Center (Dev Merkezi) hesabınıza erişmek için birden çok kullanıcı yetkilendirmek için Microsoft Azure Active Directory (Azure AD) bireysel kullanıcılara rolleri atamak için kullanmanızı önerir. Tek oturum açarak, her kullanıcı hesabı erişmesi gereken Azure AD kimlik. Bir etki alanında bir e-posta adresi kullanarak Microsoft ID oluşturun şirketiniz için kayıtlı bir kişiye e-posta atanamaz Microsoft önerir. `windowsapps@fabrikam.com` bunun bir örneğidir.  
>*   Daha fazla bilgi için ziyaret [sorunu: Microsoft Azure AD'yi ID Federasyon etki alanı](#issue-microsoft-id-in-an-azure-ad-federated-domain) bölümü.  

*   Olası en küçük sayı geliştiricilerin Microsoft ID erişimi sınırlayın. 
*   Geliştirici Merkezi hesabınızda erişmesi gereken herkes içeren bir şirket e-posta dağıtım listesi (DL) ayarlayın. DL e-posta adresi için güvenlik bilgilerinizi ekleyin. Tüm çalışanların listedeki istendiğinde güvenlik kodlarını ve Microsoft ID. için güvenlik bilgilerini yönetmek için dl'nin etkin sağlar Bir dağıtım listesi ayarlama yapmak uygun değilse, tek bir e-posta hesabının sahibi erişim ve istendiğinde güvenlik kodu paylaşmak kullanılabilir olmalıdır.  
    *   Örneğin, yeni güvenlik bilgileri Microsoft ID veya yeni bir CİHAZDAN Microsoft ID erişildiğinde eklendiğinde sahibi istenir.  
*   Bir uzantı gerekli değildir ve anahtar takım üyeleri için erişilebilir olan bir şirket telefon numarası ekleyin.  
*   Genel olarak, geliştiriciler, Geliştirici Merkezi hesabınızda oturum açmak için güvenilen cihazları istemeniz gerekir. Tüm anahtar ekip üyeleri, güvenilen cihazlara erişim izni olmalıdır. Güvenilen cihazlara erişim kullanmayı güvenlik kodları göndermek için biri Geliştirme Merkezi hesabı erişirken gereksinimi azaltır.  
*   Güvenilir olmayan bir bilgisayardan Geliştirme Merkezi hesabı için erişim vermek gerekirse, en fazla beş geliştiriciden erişimi sınırlamanız gerekir. İdeal olarak, geliştiricilerinizin hesabı, aynı coğrafi paylaşın ve ağ konumu bilgisayarlardan erişecekseniz.  
*   Sık gözden geçirin ve güvenlik bilgilerinizi doğrulayın.  
    *   Güvenlik bilgilerinizi görüntülemek için Güvenlik Ayarları sayfasında bulunan ziyaret [account.live.com/proofs/Manage](https://account.live.com/proofs/Manage).

Geliştirici Merkezi hesabınızda öncelikle Güvenilen bilgisayarlardan erişilmelidir. Haftalık Geliştirme Merkezi hesabı başına oluşturulan kodları sayısına bir sınır olduğundan, güvenilen bilgisayarlardan erişim önemlidir. Güvenilen bilgisayarlara kullanarak, en güvenli ve tutarlı oturum açma deneyimi sağlar. 
*   Bir geliştirici hesabı sayfasında bulunan açılış ek Geliştirme Merkezi hesabı kuralları ve güvenlik hakkında daha fazla bilgi için ziyaret [docs.microsoft.com/windows/uwp/publish/opening-a-developer-account](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account). 

---

#### <a name="issue-microsoft-id-in-an-azure-ad-federated-domain"></a>Sorun: Microsoft Azure AD Federasyon etki alanı ID  
Şirket hesabınızı Azure Active Directory (Azure AD) ile Federasyon. Azure AD ile Federasyon bir şirket e-posta adresi kullanarak a Microsoft ID oluşturmayı denerseniz, bir hata alırsınız. Ardından bir hata alırsanız, BT ekibiniz hesabınız Azure AD'ye Federasyon onaylamak için danışmalısınız. Azure AD Federasyon e-posta bilinen bir sorundur ve Microsoft çözümleme için çalışmaktadır.  
*   Azure AD hakkında daha fazla bilgi için Azure Active Directory sayfasında bulunan belgeleri ziyaret [docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory).

Microsoft, geçici bir çözüm önerir. Yeni bir e-posta adresi oluşturmak için bu adımları `outlook.com` etki alanı ve kullanıcılarınızın gönderdiğiniz iletişimleri iletmek için bir kural oluşturun.  
1.  Hesap oluşturma sayfasına gidin ve yeni bir e-posta adresi bağlantı üzerinde Get tıklayın. 
    *   Microsoft ID için kaydolmak üzere konumundaki Oluştur hesap sayfasını ziyaret edin [signup.live.com/signup](https://signup.live.com/signup).  
2.  Yeni e-posta adresi oluşturun ve bir parola girin. Yeni a Microsoft ID ve bir e-posta kutunuzdaki `outlook.com` etki alanı oluşturulur. Hesap oluşturulana kadar kayıt işlemine devam edin.  

    >[!IMPORTANT]
    >A Microsoft geliştirme Merkezi'nde kaydetmek için ID olarak kaydedilmiş bir e-posta adresi veya dağıtım listesinin kullanmanız gerekir. Microsoft, kişilerden gelen bağımlılığı kaldırmak için bir dağıtım listesi kullanmanızı önerir. Ardından, e-posta adresi veya dağıtım listesi kayıtlı değilse, artık kaydetmelisiniz.    

    >[!Important]
    >Tüm e-posta adresi, bulunan `Microsoft` şirket etki alanı, sonra da Dev Center'da kaydı için kullanmak mümkün değildir.  

3.  Outlook posta kutunuza Outlook e-posta adresiyle Microsoft ID oluşturduktan sonra oturum açın. E-posta iletme kuralı oluşturun. E-posta iletme kuralı Outlook posta kutunuza Market hesabınızı yönetmek için oluşturduğunuz etki alanınızdaki e-posta adresi alınıp tüm e-postaları taşımanız gerekir.  
    *   Outlook posta kutunuza imzalamak için konumundaki Outlook sayfasını ziyaret edin [outlook.live.com/owa](https://outlook.live.com/owa).  
    *   Konumunda bulunan başka bir hesap sayfası iletilerini otomatik olarak iletmek için Outlook Web App'te kullanım kuralları iletme kuralları hakkında daha fazla bilgi için ziyaret edin [support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed](https://support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed).  

1.  İletme kuralı tüm e-posta ve iletişim e-posta adresi şirket için kaydedilmiş bir etki alanında Outlook e-posta hesabına alınan gönderir. `outlook.com` E-posta adresi, hem Geliştirme Merkezi hem de bulut iş ortağı portalı kimliğini doğrulamak için kullanılmalıdır.  

## <a name="next-steps"></a>Sonraki adımlar
*   Ziyaret [Azure Market ve AppSource yayımcı Kılavuzu](./marketplace-publishers-guide.md) sayfası.  
 
---
