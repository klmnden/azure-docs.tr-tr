---
title: Azure Active Directory'de cihaz yönetimine giriş | Microsoft Docs
description: Cihaz yönetimi, ortamınızdaki kaynakların erişen cihazlar üzerinde denetim altına almak için nasıl yardımcı olabileceğini öğrenin.
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
ms.openlocfilehash: 151aa2f8065c7232284c560ff515afab40ae7f5c
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414983"
---
# <a name="introduction-to-device-management-in-azure-active-directory"></a>Azure Active Directory'de cihaz yönetimine giriş

Bir mobil öncelikli ve bulut öncelikli dünyada, Azure Active Directory (Azure AD), çoklu oturum açma cihazlar, uygulamalar ve hizmetlere her yerden sağlar. Kendi cihazını getir (KCG), dahil olmak üzere cihazları - çoğalan ile BT uzmanları ile iki rakip hedefleri yılının:

- Son kullanıcılara ne zaman ve yerde üretken olmasını sağlayın
- Kurumsal varlıkları herhangi bir zamanda koruma

Cihazlar aracılığıyla, kullanıcılarınızın şirket varlıklarınızı erişim sağlama. BT yöneticisi olarak şirket varlıklarınızı korumak için bu cihazlar üzerinde denetime sahip olmasını istiyor. Bu, kullanıcılarınızın kaynaklarınıza güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan eriştiğiniz emin olmanızı sağlar. 

Cihaz yönetimi, ayrıca temel [cihaz tabanlı koşullu erişim](../active-directory-conditional-access-policy-connected-applications.md). Cihaz tabanlı koşullu erişim ile ortamınızdaki kaynakları erişimi yalnızca yönetilen cihazlarla mümkün olduğundan emin olun.   

Bu makalede, Azure Active Directory'de cihaz yönetimini nasıl çalıştığı açıklanmaktadır.

## <a name="getting-devices-under-the-control-of-azure-ad"></a>Azure ad denetimi altında cihazları alma

Azure ad denetimi altında bir cihaz almak için iki seçeneğiniz vardır:

- Kaydediliyor 
- Birleştirme

**Kaydetme** bir cihazı Azure AD'ye cihaz kimlik yönetmenizi sağlar. Bir cihaz kaydedildiğinde Azure AD cihaz kaydı bir kullanıcı, oturum açtığında Azure AD'ye cihaz kimliğini doğrulamak için kullanılan bir kimliğe sahip cihazı sağlar. Etkinleştirme veya devre dışı bir cihaz kimliği kullanabilirsiniz.

Bir mobil cihaz çözümü ile birleştirildiğinde Intune gibi Azure AD'de cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Bu durum, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar. Intune yönetimi için cihazları kaydetme hesabından cihazların kaydını Microsoft Intune hakkında daha fazla bilgi için bkz.

**Birleştirme** bir cihaz için bir cihazı kaydetme bir uzantısıdır. Diğer bir deyişle, bir cihazı kaydetme avantajları sağlar ve buna ek olarak, bu da bir cihazın yerel durumunu değiştirir. Yerel durumu değiştiriliyor bir cihaza bir kurumsal iş veya Okul hesabı yerine kişisel bir hesap kullanarak oturum olanak sağlar.

## <a name="azure-ad-registered-devices"></a>Azure AD kayıtlı cihazları   

Kayıtlı Azure AD cihazları için destek sağlamak için hedefidir **kendi cihazını getir (KCG)** senaryo. Bu senaryoda, bir kullanıcı kişisel bir cihaz kullanarak, kuruluşunuzun Azure Active Directory denetlenen kaynaklarına erişebilir.  

![Azure AD kayıtlı cihazları](./media/overview/03.png)

Erişim cihazda girilen bir iş veya Okul hesabı temel alır.  
Örneğin, Windows 10, kullanıcıların bir iş veya Okul hesabı kişisel bilgisayar, tablet veya telefon eklemesine olanak sağlar.  
Ne zaman bir kullanıcı eklendi bir iş veya Okul hesabı, cihazı Azure AD'ye kayıtlı ve isteğe bağlı olarak, kuruluşunuz mobil cihaz Yönetimi (MDM) sistemine kayıtlı. Kuruluşunuzdaki kullanıcılar bir iş ekleyebilir veya Okul hesabı için kişisel bir cihazı rahatça:

- Bir iş uygulaması ilk kez erişirken
- El ile aracılığıyla **ayarları** Windows 10 olması durumunda menüsü 

Windows 10, iOS, Android ve macOS için kayıtlı Azure AD cihazları için yapılandırabilirsiniz.

## <a name="azure-ad-joined-devices"></a>Azure AD'ye katılmış cihazları

Azure AD'ye katılmış cihazların basitleştirmek olmaktır:

- Windows dağıtımları iş şirketinize ait cihazlar 
- Herhangi bir Windows CİHAZDAN Kurumsal uygulamalara ve kaynaklara erişim
- İş şirketinize ait cihazlar, bulut tabanlı yönetim

![Azure AD kayıtlı cihazları](./media/overview/02.png)

Aşağıdaki yöntemlerden birini kullanarak Azure AD'ye katılım dağıtılabilir: 
 - [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot)
 - [Toplu dağıtım](https://docs.microsoft.com/intune/windows-bulk-enroll)
 - [Self Servis deneyimi](azuread-joined-devices-frx.md) 

**Azure AD'ye katılım** bulut öncelikli isteyen kuruluşlar için tasarlanmıştır (diğer bir deyişle, öncelikli olarak bulut Hizmetleri, bir şirket içi altyapı kullanımını azaltmak için bir hedef ile kullanın) veya yalnızca bulutta yer alan (şirket içi altyapı olmadan). Azure AD'ye katılımı dağıtabilirsiniz kuruluşların türünü ve boyutunu hiçbir kısıtlama vardır. Hem bulut hem de şirket içi uygulamaları ve kaynaklarına erişimi etkinleştirme bile karma bir ortamda, Azure AD'ye katılım çalışır.

Azure AD'ye katılmış cihazların uygulama ile aşağıdaki avantajları sağlar:

- **Çoklu oturum açma (SSO)** SaaS uygulamaları ve Hizmetleri için Azure yönetilen. Kullanıcılarınızın, iş kaynaklarına erişim sırasında ek kimlik doğrulama istemleri görmüyorum. Kullanılabilir etki alanı ağına bağlı olunmadığında bile SSO işlevdir.

- **Kurumsal uyumlu Dolaşım** katılan cihazlar arasında kullanıcı ayarlarının. Kullanıcılar, cihazlar arasında ayarları görmek için bir Microsoft hesabı (örneğin, Hotmail) bağlanmak gerekmez.

- **İş için Windows Store erişimi** kullanarak bir Azure AD hesabı. Kullanıcıların, kuruluş tarafından önceden seçilen uygulama envanterini arasından seçim yapabilirsiniz.

- **Windows Hello** iş kaynaklarına güvenli ve kolay erişim için destek.

- **Erişim kısıtlama** uygulamaların uyumluluk ilkesini karşılayan cihazlar için.

- **Şirket içi kaynaklara sorunsuz erişim** cihaz görebilmesi için şirket içi etki alanı denetleyicisi olduğunda. 


Azure AD join, öncelikle bir şirket içi Windows Server Active Directory altyapısına sahip olmayan kuruluşlar için tasarlanmıştır ancak verilerle bu senaryolarda kullanabilirsiniz burada:

- Azure AD kullanarak bulut tabanlı altyapı için geçiş yapmak istediğiniz ve Intune gibi MDM.

- Satış noktası tabletleri ve telefonları denetimi altında gibi mobil cihazları almanız gerekirse, bir şirket içi etki alanına katılım, örneğin, kullanamazsınız.

- Kullanıcılarınız Office 365 veya Azure AD ile tümleştirilmiş diğer SaaS uygulamalarına erişmek öncelikle gerekir.

- Active Directory yerine Azure AD'de bir kullanıcı grubu yönetmek istiyorsunuz. Bu, örneğin, Dönemsel çalışanların, yüklenicilerin veya Öğrenciler uygulayabilirsiniz.

- Uzak şubeler çalışanlar için sınırlı şirket içi altyapı ile Birleşen özellikleri sunmak istiyorsunuz.

Azure AD'ye katılmış cihazlar Windows 10 cihazları için yapılandırabilirsiniz.


## <a name="hybrid-azure-ad-joined-devices"></a>Hibrit Azure AD'ye katılmış cihazlar

Aşkın için birçok kuruluşun kendi şirket içi Active Directory etki alanına katılma etkinleştirmek için yaptınız:

- İş ait cihazları merkezi bir konumdan yönetmek için BT departmanlarının.

- Kullanıcılar cihazlarını kendi Active Directory ile oturum açmak için iş veya Okul hesapları. 

Genellikle, bir şirket içi ayak izini bir kuruluşlarıyla kullanan cihazları sağlamak için yöntemleri Imaging üzerinde ve sık kullandıkları **System Center Configuration Manager (SCCM)** veya **Grup İlkesi (GP)** yönetmek için bunları.

Şirket içi ortamınız varsa, AD Ayak izi ve ayrıca Azure Active Directory tarafından sağlanan özellikler avantajından istiyorsanız, hibrit Azure AD'ye katılmış cihazlara uygulayabilirsiniz. Bunlar her ikisi de, cihazlar, şirket içi Active Directory ve Azure Active Directory'nize katılmış.

![Azure AD kayıtlı cihazları](./media/overview/01.png)


Azure AD'ye katılmış karma cihazları durumunda kullanmanız gerekir:

- Active Directory makine kimlik doğrulamasını kullanan bu cihazlara dağıtılan Win32 uygulamaları var.

- Cihazları yönetmek GP gerektirir.

- Çalışanlarınız için cihazları yapılandırmak için görüntüleme çözümlerini kullanmaya devam etmek istiyor.

Yapılandırabileceğiniz hibrit Azure AD'ye katılmış cihazlar Windows 10 ve Windows 8 ve Windows 7 gibi alt düzey cihazları.

## <a name="summary"></a>Özet

Azure AD'de cihaz yönetimi ile şunları yapabilirsiniz: 

- Azure AD denetimi altında aygıtlarını getiren işlemini basitleştirin

- Kullanıcılarınızın, kuruluşunuzun bulut tabanlı kaynaklar için kullanımı kolay erişim sağlamak

Genel bir kural olarak, kullanmanız gerekir:

- Azure AD kayıtlı cihazları:

    - Kişisel cihazlar için 

    - Cihazları Azure AD'ye el ile kaydetmek için

- Azure AD'ye katılmış cihazlar: 

    - Kuruluşunuza ait cihazlar için

    - Cihazlarda **değil** birleştirilmiş bir şirket içi AD

    - Cihazları Azure AD'ye el ile kaydetmek için

    - Bir cihazın yerel durumunu değiştirmek için

- Hibrit Azure AD'ye katılmış cihazlar için bir şirket içi katılmış cihazları AD     

    - Kuruluşunuza ait cihazlar için

    - Bir şirket içi katılan cihazlar için AD

    - Cihazları Azure AD'ye otomatik olarak kaydetmek için

    - Bir cihazın yerel durumunu değiştirmek için



## <a name="next-steps"></a>Sonraki adımlar

- Azure portalında cihaz yönetme genel bakış için bkz: [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md)

- Cihaz tabanlı koşullu erişim hakkında daha fazla bilgi için bkz: [Azure Active Directory cihaz tabanlı koşullu erişim ilkelerini yapılandırma](../active-directory-conditional-access-policy-connected-applications.md).

- Kurulum için:
    - Azure Active Directory kayıtlı Windows 10 cihazları için bkz: [kayıtlı Windows 10 cihazlarını Azure Active Directory'yi yapılandırma](../user-help/device-management-azuread-registered-devices-windows10-setup.md)
    - Azure Active Directory'ye katılmış cihazlar için bkz: [alanına katılmış cihazlar Azure Active Directory'yi yapılandırma](../user-help/device-management-azuread-joined-devices-setup.md)
    - Hibrit Azure AD'ye katılmış cihazlar için bkz [hibrit Azure Active Directory join sürecinizi planlamak nasıl](hybrid-azuread-join-plan.md).


