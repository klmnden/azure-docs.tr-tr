---
title: Azure AD'de yerel Yöneticiler grubuna yönetme alanına katılmış cihazları | Microsoft Docs
description: Azure rolleri Windows cihaz için yerel Yöneticiler grubuna atama konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: joflore
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: da55370df55bcd9122bf87c561b00f3106cc6c58
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60296784"
---
# <a name="how-to-manage-the-local-administrators-group-on-azure-ad-joined-devices"></a>Azure AD'de yerel Yöneticiler grubuna yönetme alanına katılmış cihazları

Bir Windows cihazı yönetmek için yerel Yöneticiler grubunun bir üyesi olmanız gerekir. Azure Active Directory (Azure AD) birleştirme işleminin bir parçası olarak, Azure AD, bir cihazda, bu grubun üyeliğini güncelleştirir. Üyelik Güncelleştirme iş gereksinimlerinizi karşılamak için özelleştirebilirsiniz. Üyelik Güncelleştirme, örneğin, bir cihaz üzerinde yönetici haklarına gerek duyulmadan görevleri yapmak Yardım Masası personelinizin etkinleştirmek istiyorsanız yararlıdır.

Bu makalede, üyeliği güncelleştirilmesinin nasıl çalıştığını ve nasıl bir Azure AD katılımı sırasında özelleştirebilirsiniz açıklanmaktadır. Bu makalenin içeriğini uygulanmaz bir **karma** Azure AD'ye katılım.


## <a name="how-it-works"></a>Nasıl çalışır?

Bir Azure AD'ye katılım'ı kullanarak Azure AD ile bir Windows cihazı bağladığınızda, Azure AD aşağıdaki güvenlik ilkelerinin cihazda yerel Yöneticiler grubuna ekler:

- Azure AD genel yönetici rolü
- Azure AD cihaz yöneticisi rolü 
- Azure AD'ye katılım gerçekleştiren kullanıcı   

Azure AD rolleri yerel administrators grubuna ekleyerek, bir cihaz, cihaz başına hiçbir şey değiştirmeden dilediğiniz zaman Azure AD'de yönetebileceği kullanıcılar güncelleştirebilirsiniz. Şu anda gruplar bir yönetici rolü atama yapılamıyor.
Azure AD, Azure AD cihaz yöneticisi rolü de (PoLP) en az ayrıcalık ilkesini desteklemek için yerel Yöneticiler grubuna ekler. Ek olarak genel Yöneticiler, olan kullanıcılar da etkinleştirebilirsiniz *yalnızca* bir cihazı yönetmek için cihaz yöneticisi rolü atanır. 


## <a name="manage-the-global-administrators-role"></a>Genel Yöneticiler rolünü yönetme

Görüntüleme ve genel Yönetici rolüne üyelik güncelleştirmek için bkz:

- [Azure Active Directory'de Yönetici rolü tüm üyeleri görüntüleyin](../users-groups-roles/directory-manage-roles-portal.md)

- [Bir kullanıcı Azure Active Directory'de yönetici rolleri atama](../fundamentals/active-directory-users-assign-role-azure-portal.md)


## <a name="manage-the-device-administrator-role"></a>Cihaz yöneticisi rolü yönetme 

Azure portalında, üzerinde cihaz Yönetici rolü yönetebilirsiniz **cihazları** sayfası. Açmak için **cihazları** sayfası:

1. Oturum açın, [Azure portalında](https://portal.azure.com) bir genel yönetici veya cihaz Yöneticisi olarak.
2. Sol gezinti tıklatın **Azure Active Directory**. 
3. İçinde **Yönet** bölümünde **cihazları**.
4. Üzerinde **cihazları** sayfasında **cihaz ayarları**.

Cihaz yöneticisi rolü değiştirilecek yapılandırma **ek yerel Yöneticiler Azure AD alanına katılmış cihazlar**.  

![Ek yerel Yöneticiler](./media/assign-local-admin/10.png)

>[!NOTE]
> Bu seçenek, bir Azure AD Premium kiracınız gerektirir. 


Cihaz yöneticileri, tüm Azure AD'ye katılmış cihazlara atanır. Belirli bir dizi cihazda cihaz yöneticilerine kapsamı oluşturulamıyor. Cihaz yöneticisi rolü güncelleştirme mutlaka anında etkili etkilenen kullanıcılar üzerinde yok. Cihazlar için bir kullanıcı zaten oturum açmış, ayrıcalık güncelleştirme gerçekleşir:
     

- Ne zaman bir kullanıcı oturumu kapatır.
- 4 saat sonra yeni bir yenileme birincil belirteç verilir. 




## <a name="manage-regular-users"></a>Normal kullanıcıları yönetme

Varsayılan olarak, Azure AD Yönetici grubu Azure AD'ye katılmasını sağlamaya cihazda gerçekleştiren kullanıcı ekler. Normal kullanıcıların yerel Yöneticiler eskimesini engellemek istiyorsanız, aşağıdaki seçenekleriniz:

- [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) -Windows Autopilot birincil kullanıcı bir yerel yönetici olmaktan birleşim gerçekleştirme önlemek için bir seçenek ile sağlar. Bunu başarmak [Autopilot profili oluşturuluyor](https://docs.microsoft.com/intune/enrollment-autopilot#create-an-autopilot-deployment-profile).
 
- [Toplu kayıt](https://docs.microsoft.com/intune/windows-bulk-enroll) -otomatik olarak oluşturulmuş bir kullanıcı bağlamında bir toplu kayıt bağlamında gerçekleştirilen bir Azure AD katılma olur. Bir cihazı alanına sonra oturum açan kullanıcılar, Yöneticiler grubuna eklenmez.   



## <a name="manually-elevate-a-user-on-a-device"></a>Bir kullanıcı bir cihazda el ile Yükselt 

Azure AD katılma işlemi kullanmaya ek olarak, normal bir kullanıcı belirli bir cihaz üzerinde yerel yönetici olmasını el ile yükseltebilirsiniz. Bu adım zaten yerel Yöneticiler grubunun bir üyesi olmasını gerektirir. 

İle başlayarak **Windows 10 1709** sürüm, gerçekleştirebileceğiniz bu görevden **ayarlar -> hesapları -> diğer kullanıcıların**. Seçin **iş veya Okul kullanıcı ekleme**, altında kullanıcının UPN'sini girin **kullanıcı hesabı** seçip *yönetici* altında **hesap türü**  
 
Ayrıca, komut istemini kullanarak kullanıcıları ekleyebilirsiniz:

- Kiracı kullanıcılarınız şirket içi Active Directory'den eşitlenen kullanırsanız `net localgroup administrators /add "Contoso\username"`.

- Kiracı kullanıcıların Azure AD'de oluşturduysanız kullanın `net localgroup administrators /add "AzureAD\UserUpn"`


## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Grupları için cihaz yöneticisi rolü atayamazsınız, yalnızca bireysel kullanıcılar izin verilir.

Cihaz yöneticileri, tüm Azure AD katıldı cihazlara atanır. Bunlar, belirli bir cihaz kümesi için kapsama alınamaz.

Kullanıcıların cihaz yönetici rolünden kaldırdığınızda, bunlar için oturum açmış olduğu sürece hala bir cihaz üzerinde yerel yönetici ayrıcalığı sahiptirler. Ayrıcalığı, sonraki oturum açma sırasında veya yeni bir birincil yenileme belirteci zaman verilen 4 saat sonra iptal edilir.



## <a name="next-steps"></a>Sonraki adımlar

- Azure portal'da cihaz yönetimine ilişkin genel bir bakış edinmek için bkz. [Azure portal'ı kullanarak cihazları yönetme](device-management-azure-portal.md)

- Cihaz tabanlı koşullu erişim hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory cihaz tabanlı koşullu erişim ilkelerini yapılandır](../conditional-access/require-managed-devices.md).


