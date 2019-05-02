---
title: Azure Active Directory koşullu erişim ilkeleri - dışlanan kullanıcıları yönetmek için erişim gözden geçirmeleri kullanın | Microsoft Docs
description: Koşullu erişim ilkeleri'nden dışlanacak kullanıcıları yönetmek için Azure Active Directory (Azure AD) erişim gözden geçirmeleri kullanmayı öğrenin
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/25/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4169b15304afe1ecc4af9c5354798b29ad9dba38
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64571360"
---
# <a name="use-azure-ad-access-reviews-to-manage-users-excluded-from-conditional-access-policies"></a>Koşullu erişim ilkeleri'nden dışlanacak kullanıcıları yönetmek için kullanım Azure AD erişim gözden geçirmeleri

İdeal bir dünyada, tüm kullanıcıların erişimini izlemeniz gerekir ilkeleri, kuruluşunuzun kaynaklarına güvenli erişim için. Ancak, bazı durumlarda özel durumları yapmanızı gerektiren iş durumlar vardır. Bu makalede burada dışlamaları gerekli olabilir bazı örnekler ve nasıl BT yöneticisi olarak, bu görevi yönetebilir, İlkesi özel durumları sözleşmeli önlemek ve denetçiler düzenli olarak Azure'ı kullanarak bu özel durumlar incelenir kavram ile sağlayın Active Directory (Azure AD) erişim gözden geçirmeleri.

> [!NOTE]
> Erişim gözden geçirmeleri geçerli Azure AD Premium P2, Enterprise Mobility + Security E5 Azure AD kullanmak için ücretli veya deneme lisansı gereklidir. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md).

## <a name="why-would-you-exclude-users-from-policies"></a>Neden kullanıcıların ilkelerinden çıkarmanız?

BT yöneticisi, kullanabileceğinize [Azure AD koşullu erişim](../conditional-access/overview.md) multi factor authentication (MFA) veya bir güvenilir ağ veya cihaz oturum açma kullanarak kimlik doğrulaması gerektirmek. Dağıtım planlaması sırasında bazı bu gereksinimlerin tüm kullanıcılar tarafından karşılanamıyor öğrenin. Örneğin, iç ağınızın bir parçası olmayan bir uzak ofis çalışan kullanıcı veya desteklenmeyen bir eski phone kullanan bir yönetici yok. Bu kullanıcılar oturum açmak ve, işini yapması için izin iş gerektirir. Bu nedenle, koşullu erişim ilkeleri ' tutulur.

Başka bir örnek olarak, kullanabileceğinize [adlandırılmış Konumlar](../conditional-access/location-condition.md) ülkeler ve bölgeler yoksa istediğiniz kullanıcıların, kiracılarının erişmesine izin vermek bir dizi yapılandırmak için koşullu erişim.

![Adlandırılmış konumlar](./media/conditional-access-exclusion/named-locations.png)

Ancak, bazı durumlarda, kullanıcılar engellenen bu ülkelerden/bölgelerden oturum açmak için meşru bir neden olabilir. Örneğin, kullanıcıların iş veya kişisel nedenlerle seyahat. Bu örnekte, bu ülkeler/bölgeler engellemek için koşullu erişim ilkesi ilkesinden dışlanır kullanıcılar için bir özel bulut güvenlik grubu olabilir. Seyahat ederken erişmesi gereken kullanıcılar kendilerine grubunu kullanarak konusunda ekleyebilirsiniz [Azure AD Self Servis Grup Yönetimi](../users-groups-roles/groups-self-service-management.md).

Koşullu erişim ilkesi olan başka bir örnek olabilir, [blokları eski kimlik doğrulaması, kullanıcılarınızın büyük çoğunluğu için](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/07/azure-ad-conditional-access-support-for-blocking-legacy-auth-is-in-public-preview/). Microsoft, kiracınıza güvenliğinizi artırmak için eski protokolleri kullanımını engeller kesinlikle önerir. Ancak, kesinlikle eski kimlik doğrulama yöntemleri aracılığıyla Office 2010 kaynaklarınıza erişmek için kullanması gereken bazı kullanıcılar varsa veya IMAP/SMTP/POP tabanlı istemciler, ardından, bu kullanıcılar eski kimlik doğrulama yöntemleri engelleyen ilkeden hariç tutabilirsiniz.

## <a name="why-are-exclusions-challenging"></a>Neden dışlamaları zor?

Azure AD'de bir koşullu erişim ilkesi kullanıcı kapsamını belirleyebilirsiniz. Bu kullanıcılardan bazıları, Azure AD rolleri, bireysel kullanıcılar veya kullanıcıların Konukları seçerek de dışlayabilirsiniz. Bu özel durumlar yapılandırıldığında, ilke hedefi söz konusu kullanıcılar için zorlanamaz unutmamak önemlidir. Bu dışlama ya da bir liste olarak bireysel kullanıcıların ya da eski şirket içi güvenlik grubu ile yapılandırılmış sonra (kullanıcılar bilmiyor onun varlığını) bu çıkarma listesi ve BT yöneticisinin denetime görünürlüğü sınırlar (kullanıcılar katılın ilke devat etmesine güvenlik grubu). Ayrıca, tek seferde dışlama için yetkili kullanıcılar artık ihtiyaç veya için uygundur.

Bir dışlama başında ilkesini atlama kullanıcılar kısa bir listesi yoktur. Zaman içinde giderek daha fazla kullanıcılar hariç tutulan ve listeyi genişler. Belirli bir noktada listesini gözden geçirin ve bu kullanıcıların her birinden hala bırakılmalıdır onaylayın gerek yoktur. Bir teknik bakış açısıyla, listeyi yönetmek nispeten kolay olabilir, ancak kimin iş kararlarını verir ve tüm denetlenebilirdir nasıl emin olabilirim?

Koşullu erişim ilkesi kullanarak bir Azure AD grubu dışlamayı yapılandırırsanız, ancak daha sonra erişim gözden geçirmeleri bir telafi denetimi olarak görünürlüğünü artırın ve bir özel durum olan kullanıcıların sayısını azaltmak için kullanabilirsiniz.

## <a name="how-to-create-an-exclusion-group-in-a-conditional-access-policy"></a>Bir koşullu erişim ilkesini bir hariç tutma grubu oluşturma

Adımları yeni bir Azure AD grubunu ve koşullu erişim ilkesi, grup için geçerli değildir.

### <a name="create-an-exclusion-group"></a>Bir çıkarma grubu oluşturun

1. Azure Portal’da oturum açın.

1. Sol gezinti bölmesinde tıklayın **Azure Active Directory** ve ardından **grupları**.

1. Üst menüsünde **yeni grup** grubu bölmesini açmak için.

1. İçinde **grup türü** listesinden **güvenlik**. Bir ad ve açıklama belirtin.

1. Ayarladığınızdan emin olun **üyelik** için yazın **atanan**.

1. Bu dışlama grubunun parçası olması ve ardından gereken kullanıcıları seçin **Oluştur**.

    ![Yeni Grup bölmesi](./media/conditional-access-exclusion/new-group.png)

### <a name="create-a-conditional-access-policy-that-excludes-the-group"></a>Grup dışlayan bir koşullu erişim ilkesi oluşturma

Artık bu hariç tutma grubu kullanan bir koşullu erişim ilkesi oluşturabilirsiniz.

1. Sol gezinti bölmesinde tıklayın **Azure Active Directory** ve ardından **koşullu erişim** açmak için **ilkeleri** dikey penceresi.

1. Tıklayın **yeni ilke** açmak için **yeni** bölmesi.

1. Bir ad belirtin.

1. Altında ataması **kullanıcılar ve gruplar**.

1. Üzerinde **INCLUDE** sekmesinde **tüm kullanıcılar**.

1. Üzerinde **hariç** sekmesinde, onay işareti ekleyin **kullanıcılar ve gruplar** ve ardından **dışlanan kullanıcılar seçin**.

1. Oluşturduğunuz dışlama grubu seçin.

    > [!NOTE]
    > Bir en iyi uygulama olarak, kiracınızın dışında kilitli değil emin olmak için test ederken en az bir yönetici hesabını ilkeden dışlamak için önerilir.

1. Kuruluş gereksinimlerinize göre koşullu erişim ilkesini ayarlama ile devam edin.

    ![Dışlanacak kullanıcıları seçin](./media/conditional-access-exclusion/select-excluded-users.png)

Şimdi bir özel koşullu erişim ilkelerini yönetmek için erişim gözden geçirmeleri burada kullanabileceğiniz iki örnek kapsar.

## <a name="example-1-access-review-for-users-accessing-from-blocked-countriesregions"></a>Örnek 1: Engellenen ülkeler/bölgeler üzerinden erişen kullanıcılar için erişim gözden geçirmesi

Bazı ülkeler/bölgeler üzerinden erişimi engeller bir koşullu erişim ilkesine sahip varsayalım. Bu, bir grup ilkesinden dışlandı içerir. Grubun üyeleri, burada gözden geçirilmemiş bir önerilen erişim gözden geçirmesi aşağıdadır.

> [!NOTE]
> Bir genel yönetici veya Kullanıcı Yöneticisi rolü, erişim gözden geçirmeleri oluşturmak için gereklidir.

1. Gözden geçirme, her hafta tekrar oluşmasını.

2. Hiçbir zaman bu hariç tutma grubu en güncel tutarak emin olmak için sona erecek.

3. Bu grubun tüm üyelerinin gözden geçirme kapsamındaki olacaktır.

4. Her bir kullanıcı kendi kendine onaylamasını bunlar yine de bu engellenen ülkeler/bölgelerden gelen erişiminin olması gerekir, bu nedenle bunlar yine de grubunun bir üyesi olmanız gerekir gerekecektir.

5. Kullanıcı için gözden geçirme isteğine yanıt vermezse, bunlar otomatik olarak gruptan kaldırılacak ve bu nedenle, kiracının Bu ülkeler/bölgeler için seyahat ederken erişemez.

6. Kullanıcıların erişim gözden geçirmesi tamamlanmasından ve başlangıç hakkında bildirim almak, e-posta bildirimlerini etkinleştirin.

    ![Erişim gözden geçirmesi oluşturma](./media/conditional-access-exclusion/create-access-review-1.png)

## <a name="example-2-access-review-for-users-accessing-with-legacy-authentication"></a>Örnek 2: Eski bir kimlik doğrulama ile erişen kullanıcılar için erişim gözden geçirmesi

Eski bir kimlik doğrulama ve eski istemci sürümleri kullanan kullanıcılar için erişimi engeller bir koşullu erişim ilkesine sahip varsayalım. Bu, bir grup ilkesinden dışlandı içerir. Grubun üyeleri, burada gözden geçirilmemiş bir önerilen erişim gözden geçirmesi aşağıdadır.

1. Bu gözden geçirme, yinelenen bir gözden geçirme olması gerekir.

2. Gruptaki herkesin, gözden geçirilmesi gerekir.

3. Seçili gözden geçirenler olarak iş birimi sahiplerinin listelemek için yapılandırılabilir.

4. Sonuçları otomatik olarak Uygula ve eski kimlik doğrulama yöntemlerini kullanmaya devam etmek için onaylanmamış kullanıcıları kaldırın.

5. Gözden geçirenler büyük grupları kolayca kendi kararları önerilerini etkinleştirmek yararlı olabilir.

6. Kullanıcıların erişim gözden geçirmesi tamamlanmasından ve başlangıç hakkında bildirim almak, e-posta bildirimlerini etkinleştirin.

    ![Erişim gözden geçirmesi oluşturma](./media/conditional-access-exclusion/create-access-review-2.png)

**Pro İpucu**: Artık, birçok dışlama gruplarınız ve bu nedenle birden çok erişim gözden geçirmeleri oluşturmanız gerekir, bir API oluşturup programlı olarak yönetmek izin veren Microsoft Graph beta uç sahip. Başlamak için bkz: [Azure AD erişim gözden geçirmeleri, API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/accessreviews_root) ve [örnek alma Azure AD erişim gözden geçirmeleri, Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/td-p/236096).

## <a name="access-review-results-and-audit-logs"></a>Erişim gözden geçirmesi sonuçlarını ve Denetim günlükleri

Artık her şeyi bir yerde, grubu, koşullu erişim ilkesi ve erişim gözden geçirmeleri sahip olduğunuza göre bunu bu gözden geçirmeler sonuçlarını izleyin zamanı geldi.

1. Azure portalında açın **erişim gözden geçirmeleriyle** dikey penceresi.

1. Denetim ve dışlama Grup Yönetimi için oluşturduğunuz programı'nı açın.

1. Tıklayın **sonuçları** kimin listede kalmak için Onaylandı ve kimin kaldırıldı.

    ![Erişim gözden geçirmeleri sonuçları](./media/conditional-access-exclusion/access-reviews-results.png)

1. Ardından **denetim günlükleri** bu İnceleme sırasında gerçekleştirilen eylemleri görmek için.

    ![Erişim denetim günlüklerini gözden geçirme](./media/conditional-access-exclusion/access-reviews-audit-logs.png)

Bir BT yöneticisi olarak, ilkelerinizi için dışlama grupları yönetme bazen kaçınılmaz olduğunu biliyorsunuz. Ancak, bu grupları koruyun, kendileri ve bu değişiklikler için Azure AD erişim ile daha kolay denetim iş sahibi veya kullanıcıları tarafından düzenli olarak gözden inceler.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)
- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)
