---
title: Azure Active Directory dinamik grup üyeliği, otomatik olarak kullanıcı ekleme ve kaldırma öğreticisi
description: Bu öğreticide kullanıcıları otomatik olarak eklemek veya kaldırmak için grupları ve kullanıcı üyeliği kurallarını kullanacaksınız
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: tutorial
ms.date: 08/07/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 2119bb60cbdc36f62623ce0db52885e17f3d3006
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "40209190"
---
# <a name="tutorial-add-or-remove-group-members-automatically"></a>Öğretici: Grup üyelerini otomatik olarak ekleme veya kaldırma

Azure Active Directory'de (Azure AD) sürekli el ile müdahale etmenize gerek kalmaması için güvenlik gruplarında veya Office 365 gruplarında kullanıcı ekleme veya kaldırma işlemlerini otomatik olarak gerçekleştirebilirsiniz. Bir kullanıcının veya cihazın özelliklerinden biri değiştiğinde Azure AD kiracınızdaki tüm dinamik grup kurallarını değerlendirerek gerekli üye ekleme veya kaldırma işlemlerini gerçekleştirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Belirli bir iş ortağı şirketinin konuk kullanıcıları için otomatik olarak doldurulan bir grup oluşturma
> * Gruba konuk kullanıcıların erişmesi için iş ortağına özgü özelliklere yönelik lisans atama
> * Bonus: Üye kullanıcılarınızın yalnızca içeriden erişilebilen sitelere erişimini yönetme gibi işlemler gerçekleştirmek için konuk kullanıcıları kaldırarak **Tüm kullanıcılar** grubunun güvenliğini sağlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu özellik için kiracının global yöneticisi olarak bir Azure AD Premium lisansına ihtiyaç duyarsınız. Lisansınız yoksa Azure AD'de **Lisanslar** > **Ürünler** > **Dene/Satın Al**'ı seçin.

Dinamik gruplara üye olarak ekleyeceğiniz kullanıcılara lisans atamanıza gerek yoktur. Yalnızca kiracıda bu kullanıcıları kapsamaya yeterli sayıda Azure AD Premium P1 lisansına sahip olmanız gerekir. 

## <a name="create-a-group-of-guest-users"></a>Konuk kullanıcı grubu oluşturma

Öncelikle tamamı tek bir iş ortağı şirketinize ait olan konuk kullanıcılar için bir grup oluşturmanız gerekir. Bu kullanıcılar özel lisansa ihtiyaç duyduğundan bu amaca yönelik bir grup oluşturmak daha etkili olacaktır.

1. Kiracınızın genel yöneticisi olan bir hesapla Azure portalda (https://portal.azure.com)) oturum açın.
2. **Azure Active Directory** > **Gruplar** > **Yeni grup**'u seçin.
  ![Yeni grup seçme komutu](./media/groups-dynamic-tutorial/new-group.png)
3. **Grup** dikey penceresinde:
  
  * **Güvenlik** grup türünü seçin
  * Grup adı ve açıklaması olarak `Guest users Contoso` girin
  * **Üyelik türü** değerini **Dinamik Kullanıcı** olarak değiştirin
  * **Dinamik sorgu ekle**'yi seçin
  
4. **Gelişmiş kural**'ı seçin ve **Gelişmiş kural** kutusuna şu değeri girin: `(user.userType -eq "Guest") -and (user.companyName -eq "Contoso")`
5. Dikey pencereyi kapatmak için **Sorgu ekle**'yi seçin.
6. **Grup** dikey penceresinde **Oluştur**'u seçerek grubu oluşturun.

## <a name="assign-licenses"></a>Lisans atama

Yeni grubunuzu oluşturduğunuza göre bu iş ortağı kullanıcılarına ihtiyacı olan lisansları atayabilirsiniz.

1. Azure AD'de **Lisanslar**'ı seçin, bir veya daha fazla lisans seçin ve **Ata**'yı belirleyin.
2. **Kullanıcılar ve gruplar**'ı ve **Guest users Contoso** grubunu seçip değişikliklerinizi kaydedin.
3. **Atama seçenekleri**, seçtiğiniz lisanslarda bulunan hizmet planlarını açıp kapatmanızı sağlar. Değişiklik yaptığınızda **Tamam**'a tıklayarak kaydetmeyi unutmayın.
4. Atamayı tamamlamak için **Lisans ata** bölmesinin en altında bulunan **Ata**'ya tıklayın.

## <a name="remove-guests-from-all-users-group"></a>Tüm kullanıcılar grubundan konukları kaldırma

Yönetim planınız, tüm konuk kullanıcılarınızı şirketlerine göre gruplara ayırmak olabilir. Artık **Tüm kullanıcılar** grubunu değiştirerek yalnızca kiracınızdaki üye kullanıcılara özel hale getirebilirsiniz. Ardından bu grubu kullanarak kuruluşunuza özgü uygulama ve lisans atamaları gerçekleştirebilirsiniz.

   ![Tüm kullanıcılar grubunu yalnızca üyeleri kapsayacak şekilde değiştirme](./media/groups-dynamic-tutorial/all-users-edit.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

**Konuk kullanıcılar grubunu kaldırmak için**

1. Kiracınızın Genel Yöneticisi olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. **Azure Active Directory** > **Gruplar**'ı seçin. **Guest users Contoso** grubunu, üç noktayı (...) ve ardından **Sil**'i seçin. Grubu sildiğinizde atanmış olan lisanslar kaldırılır.

**Tüm Kullanıcılar grubunu geri yüklemek için**
1. **Azure Active Directory** > **Gruplar**'ı seçin. Grubu açmak için **Tüm kullanıcılar** grubunun adını seçin.
1. **Dinamik üyelik kuralları**'nı seçin, kural metninin tamamını silin ve **Kaydet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Konuk kullanıcı grubu oluşturma
> * Yeni grubunuza lisans atama
> * Tüm kullanıcılar grubunu yalnızca üyeleri kapsayacak şekilde değiştirme

Grup tabanlı lisanslama ile ilgili temel bilgiler için bir sonraki makaleye geçin
> [!div class="nextstepaction"]
> [Grup tabanlı lisanslama ile ilgili temel bilgiler](../fundamentals/active-directory-licensing-whatis-azure-portal.md)



