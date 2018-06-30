---
title: Azure Active Directory'de cihaz yönetimine giriş | Microsoft Docs
description: Aygıt Yönetimi, ortamınızdaki kaynaklarına erişen cihazlar üzerinde denetim elde size nasıl yardımcı olabileceğini öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 96de05eb8ebae85b73eaa012efdfb38ac074abf8
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110929"
---
# <a name="introduction-to-device-management-in-azure-active-directory"></a>Azure Active Directory'de cihaz yönetimine giriş

Bir mobil ilk olarak, bulut ilk dünyasında, Azure Active Directory (Azure AD) çoklu oturum açma cihazları, uygulamaları ve Hizmetleri için yerden sağlar. Kendi cihazını getir (KCG), aygıtlar - artışı ile BT uzmanları iki rakip hedefle karşılaştığı:

- Yerde ve zamanda üretken olmak için son kullanıcılara güç kazandırma
- Herhangi bir zamanda Kurumsal varlıklar koruma

Aygıtlar, kullanıcılarınızın şirket varlıklarınızı erişim sağlama. BT yöneticisi olarak şirket varlıklarınızı korumak için bu cihazlar üzerinde denetime sahip olmasını istiyor. Bu, güvenlik ve uyumluluk standartlarına uyması aygıtlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olmanızı sağlar. 

Cihaz yönetimi, ayrıca için temel [cihaz temelli koşullu erişim](active-directory-conditional-access-policy-connected-applications.md). Cihaz temelli koşullu erişim ile ortamınızdaki kaynaklarına erişimi yalnızca yönetilen cihazlarla olası olduğundan emin olun.   

Bu makalede, Azure Active Directory içindeki aygıt yönetimi nasıl çalıştığı açıklanmaktadır.

## <a name="getting-devices-under-the-control-of-azure-ad"></a>Azure ad denetimindeki aygıtları alma

Azure ad denetimindeki bir aygıtı almak için iki seçeneğiniz vardır:

- Kaydediliyor 
- Birleştirme

**Kaydetme** Azure AD için bir aygıt, bir cihazın kimliğini yönetmenize olanak tanır. Bir cihaz kaydedildiğinde Azure AD cihaz kaydı bir kullanıcı, oturum açtığında Azure AD ile cihaz kimliğini doğrulamak için kullanılan bir kimliğe sahip cihazı sağlar. Etkinleştirme veya devre dışı bir cihaz kimliği kullanabilirsiniz.

Microsoft Intune gibi bir mobil cihaz birleştirildiğinde çözüm ile birlikte kullanıldığında, Azure AD cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Bu durum, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar. Microsoft Intune kaydolan cihazlar hakkında daha fazla bilgi için Yönetim için ıntune'a kaydetme aygıtları bakın.

**Birleştirme** bir aygıt bir cihaz kaydetme için bir uzantısıdır. Yani, bir cihaz kaydetme avantajları sağlar ve buna ek olarak, de aygıtın yerel durumunu değiştirir. Yerel durumu değiştirme oturum için bir kuruluş iş veya Okul hesabı yerine kişisel bir hesabı kullanarak bir aygıtı bileşenini olanak sağlar.

## <a name="azure-ad-registered-devices"></a>Azure AD kayıtlı cihazları   

Azure AD kayıtlı cihazlar için destek sağlamak için hedefidir **kendi cihazını getir (KCG)** senaryo. Bu senaryoda, bir kullanıcı kişisel bir cihazı kullanarak, kuruluşunuzun Azure Active Directory denetlenen kaynaklarına erişebilir.  

![Azure AD kayıtlı cihazları](./media/device-management-introduction/03.png)

Erişim cihazda girilen bir iş veya Okul hesabı temel alır.  
Örneğin, Windows 10 kullanıcıların kişisel bilgisayar, tablet veya telefon bir iş veya Okul hesabı eklemesine olanak sağlar.  
Ne zaman bir kullanıcı eklenmiş bir iş veya Okul hesabı, cihazı Azure AD ile kayıtlı ve isteğe bağlı olarak, kuruluşunuz yapılandırmış mobil cihaz Yönetimi (MDM) sisteminde kayıtlı. Kuruluşunuzdaki kullanıcılar, bir iş veya Okul hesabı kişisel bir cihaz için uygun şekilde:

- Bir iş uygulaması ilk kez erişirken
- El ile aracılığıyla **ayarları** Windows 10 durumunda menüsü 

Azure AD kayıtlı cihazlara Windows 10, iOS, Android ve macOS için yapılandırabilirsiniz.

## <a name="azure-ad-joined-devices"></a>Azure AD alanına katılmış aygıtlar

Azure AD alanına katılmış aygıtlar basitleştirmek için belirtilir:

- Windows dağıtımları iş ait cihazlar 
- Kuruluş uygulamaları ve kaynaklara erişim herhangi bir Windows CİHAZDAN
- Bulut tabanlı yönetim iş ait cihazlar

![Azure AD kayıtlı cihazları](./media/device-management-introduction/02.png)

Azure AD birleştirme, aşağıdaki yöntemlerden birini kullanarak dağıtılabilir: 
 - [Windows Autopilot](https://docs.microsoft.com/en-us/windows/deployment/windows-autopilot/windows-10-autopilot)
 - [Toplu dağıtım](https://docs.microsoft.com/en-us/intune/windows-bulk-enroll)
 - [Self Servis deneyimi](device-management-azuread-joined-devices-frx.md) 

**Azure AD birleştirme** olmasını istediğiniz kuruluşlar bulut-ilk için tasarlanmıştır (diğer bir deyişle, öncelikle bulut Hizmetleri, bir şirket içi altyapı kullanımını azaltmak için bir hedefi kullanın) veya yalnızca bulut (şirket içi altyapı). Boyutu veya Azure AD katılım dağıtabilirsiniz kuruluşların türünde bir kısıtlama yoktur. Bulut ve şirket uygulamalarına ve kaynaklarına erişimi etkinleştirme bile iyi karma bir ortamda, Azure AD birleştirme çalışır.

Azure AD alanına katılmış aygıtlar uygulama ile aşağıdaki avantajları sağlar:

- **Çoklu oturum açma (SSO)** SaaS uygulamaları ve Hizmetleri için Azure yönetilir. Kullanıcılarınızın, iş kaynaklarına erişirken ek kimlik doğrulama istekleri görmüyorum. Kullanılabilir etki alanı ağınıza bağlı olmasalar bile SSO işlevdir.

- **Kurumsal uyumlu gezici** alanına katılmış aygıtlar arasındaki kullanıcı ayarlarının. Kullanıcıların aygıtları arasında ayarları görmek için bir Microsoft hesabı (örneğin, Hotmail) bağlanmak gerekmez.

- **İş için Windows Mağazası'na erişimi** bir Azure AD hesabı kullanarak. Kullanıcılarınızın, kuruluş tarafından önceden seçilmiş uygulamalarının stoğunu arasından seçim yapabilirsiniz.

- **Windows Hello** iş kaynaklarına güvenli ve kolay erişim için destek.

- **Erişim kısıtlama** uyumluluk ilkesine uygun aygıtlardan uygulamalar için.

- **Şirket içi kaynaklara sorunsuz erişim** cihaz görüş şirket içi etki alanı denetleyicisine sahip olduğunda. 


Azure AD birleştirme öncelikle bir şirket içi Windows Server Active Directory altyapısına sahip olmayan kuruluşlar için tasarlanmıştır ancak kesinlikle senaryolarda kullanabilmeniz için burada:

- Azure AD kullanarak bulut tabanlı altyapı geçiş yapmak istediğiniz ve Intune gibi MDM.

- Tabletler ve telefonlardan denetimindeki gibi mobil cihazları almanız gerekirse, örneğin, bir şirket içi etki alanına katılma kullanamazsınız.

- Kullanıcılarınız Office 365 veya Azure AD ile tümleşik diğer SaaS uygulamaları erişmek için öncelikle gerekir.

- Active Directory yerine Azure AD'de bir kullanıcı grubuna yönetmek istiyorsunuz. Bu, örneğin Mevsimlik çalışanları, Yükleniciler veya Öğrenciler uygulayabilirsiniz.

- Sınırlı şirket içi altyapı ile uzak şube ofislerinde çalışanları katılma yetenekleri sağlamak istiyorsunuz.

Windows 10 cihazlar için Azure AD alanına katılmış cihazları yapılandırabilirsiniz.


## <a name="hybrid-azure-ad-joined-devices"></a>Karma Azure AD alanına katılmış aygıtlar

Birden fazla bir süredir birçok kuruluş, etkinleştirmek için kendi şirket içi Active Directory etki alanına katılma kullanılan:

- İş şirkete ait cihazları merkezi bir konumdan yönetmek için BT departmanları.

- Kullanıcılar cihazlarını kendi Active Directory ile oturum açmak için iş veya Okul hesapları. 

Genellikle, bir şirket içi ayak izini kuruluşlarla kullanan cihazları sağlamak için yöntemleri Imaging ve sık kullandıkları **System Center Configuration Manager (SCCM)** veya **Grup İlkesi (GP)** yönetmek için bunları.

Şirket içi ortamınız varsa, AD alanını ve ayrıca Azure Active Directory tarafından sağlanan özelliklerden avantajı istiyorsanız, karma Azure AD alanına katılmış aygıtlar uygulayabilirsiniz. Her ikisi de, cihazları bunlar şirket için Active Directory ve Azure Active Directory'yi katılmış.

![Azure AD kayıtlı cihazları](./media/device-management-introduction/01.png)


Varsa Azure AD karma alanına katılmış aygıtlar kullanmanız gerekir:

- Active Directory makine kimlik doğrulamasını kullanan bu cihazlara dağıtılan Win32 uygulamalar vardır.

- Cihazları yönetmek GP gerektirir.

- Çalışanlarınız için cihazları yapılandırmak için görüntüleme çözümlerini kullanmaya devam etmek istiyor.

Karma Azure yapılandırabilirsiniz AD alanına katılmış cihazlar Windows 10 ve Windows 8 ve Windows 7 gibi alt düzey cihazlar.

## <a name="summary"></a>Özet

Azure AD'de cihaz yönetimi ile şunları yapabilirsiniz: 

- Azure AD denetiminde aygıtlarını getiren işlemini basitleştirmek

- Kullanıcılarınızın kuruluşunuzun bulut tabanlı kaynaklar kullanımı kolay bir erişim sağlamak

Bir Flash bir kural olarak kullanmanız gerekir:

- Azure AD kaydedilen aygıtlar:

    - Kişisel cihazlar için 

    - El ile Azure AD ile cihazları kaydetmek için

- Azure AD alanına katılmış aygıtlar: 

    - Kuruluşunuz tarafından ait cihazlar için

    - Cihazlar için **değil** bir şirket içi birleşik AD

    - El ile Azure AD ile cihazları kaydetmek için

    - Bir aygıt yerel durumunu değiştirmek için

- Karma Azure AD alanına katılmış cihazlar için bir şirket içi katılmış cihazları AD     

    - Kuruluşunuz tarafından ait cihazlar için

    - Bir şirket içi katılan cihazlar için AD

    - Otomatik olarak Azure AD ile cihazları kaydetmek için

    - Bir aygıt yerel durumunu değiştirmek için



## <a name="next-steps"></a>Sonraki adımlar

- Azure portalında cihaz yönetme özetini almak için bkz: [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md)

- Cihaz temelli koşullu erişim hakkında daha fazla bilgi için bkz: [Azure Active Directory cihaz temelli koşullu erişim ilkelerini yapılandırma](active-directory-conditional-access-policy-connected-applications.md).

- Kurulum için:
    - Azure Active Directory kayıtlı Windows 10 cihazları için bkz: [Azure Active Directory yapılandırma kayıtlı Windows 10 cihazları](device-management-azuread-registered-devices-windows10-setup.md)
    - Azure Active Directory'ye katılmış cihazlar için bkz: [Azure Active Directory yapılandırma alanına katılmış aygıtlar](device-management-azuread-joined-devices-setup.md)
    - Karma Azure AD alanına katılmış cihazlar için bkz [karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl](device-management-hybrid-azuread-joined-devices-setup.md).


