---
title: Güvenli LDAP (LDAPS) Azure AD Etki Alanı Hizmetleri'ni etkinleştirme | Microsoft Docs
description: Bir Azure AD Domain Services yönetilen etki alanı için güvenli LDAP (LDAPS) etkinleştir
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: df189e405dcd5277c1ccbd94e9d5d302660be79b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60359861"
---
# <a name="enable-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Güvenli LDAP (LDAPS) bir Azure AD Domain Services yönetilen etki alanı için etkinleştirme

## <a name="before-you-begin"></a>Başlamadan önce
Tam [görev 2 - için güvenli LDAP sertifikasını dışarı aktarma bir. PFX dosyasını](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).


## <a name="task-3-enable-secure-ldap-for-the-managed-domain-using-the-azure-portal"></a>3. Görev: Azure portalını kullanarak yönetilen etki alanı için güvenli LDAP'yi etkinleştirme
Güvenli LDAP'yi etkinleştirmek için aşağıdaki yapılandırma adımlarını gerçekleştirin:

1. Gidin  **[Azure portalında](https://portal.azure.com)**.

2. Ara 'etki alanı Hizmetleri'nde' **kaynak Ara** arama kutusu. Seçin **Azure AD Domain Services** arama sonuç. **Azure AD Domain Services** sayfası, yönetilen etki alanınıza listeler.

    ![Yönetilen etki alanı hala uygulanmakta Bul](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Etki alanı hakkında daha fazla ayrıntı görmek için yönetilen etki alanı adını (örneğin, ' contoso100.com')'ye tıklayın.

    ![Etki alanı hizmetleri - sağlama durumu](./media/getting-started/domain-services-provisioning-state.png)

3. Tıklayın **güvenli LDAP** Gezinti Bölmesi.

    ![Etki Alanı Hizmetleri - güvenli LDAP sayfa](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Varsayılan olarak, yönetilen etki alanınıza güvenli LDAP erişimini devre dışıdır. İki durumlu **güvenli LDAP** için **etkinleştirme**.

    ![Güvenli LDAP'yi etkinleştirme](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Varsayılan olarak, yönetilen etki alanınıza internet üzerinden güvenli LDAP erişimini devre dışıdır. İki durumlu **internet üzerinden güvenli LDAP erişimine izin** için **etkinleştirme**, gerekirse.

    > [!WARNING]
    > Etki alanınızı internet üzerinden güvenli LDAP erişimi etkinleştirdiğinizde, internet üzerinden parola deneme yanılma saldırılarına açıktır. Bu nedenle, gerekli kaynak IP adresi aralıkları için erişim kilitlemek için bir NSG ayarlama öneririz. Yönergeler için bkz: [yönetilen etki alanınıza internet üzerinden erişim LDAPS kilitleme](active-directory-ds-ldaps-bind-lockdown.md#task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet).
    >

6. Klasör simgesini aşağıdaki **. Güvenli LDAP sertifikalı PFX dosyasını**. Yönetilen etki alanı için güvenli LDAP erişimi için Sertifika PFX dosyasının yolunu belirtin.

7. Belirtin **şifresini çözmek için parola. PFX dosyasını**. Sertifikayı PFX dosyasına aktarırken kullanılan aynı parolayı belirtin.

8. İşiniz bittiğinde tıklayın **Kaydet** düğmesi.

9. Yönetilen etki alanı için yapılandırılmış güvenli LDAP bildiren bir bildirim görürsünüz. Bu işlem tamamlanana kadar etki alanı için diğer ayarlarını değiştiremezsiniz.

    ![Yönetilen etki alanı için güvenli LDAP yapılandırılıyor](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Yönetilen etki alanınız için güvenli LDAP'yi etkinleştirmek için yaklaşık 10-15 dakika sürer. Sağlanan güvenli LDAP sertifikasını gerekli ölçütleri eşleşmiyorsa, güvenli LDAP dizininiz için etkin değil ve bir hata görürsünüz. Örneğin, etki alanı adı hatalı, sertifikanın süresi doldu veya süresi yakında doluyor. Bu durumda, geçerli bir sertifikayla yeniden deneyin.
>
>

## <a name="next-step"></a>Sonraki adım
[4. Görev: İnternet’ten yönetilen etki alanına erişmek için DNS’yi yapılandırma](active-directory-ds-ldaps-configure-dns.md)
