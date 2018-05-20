---
title: Güvenli LDAP (LDAPS) Azure AD Etki Alanı Hizmetleri'nde yapılandırma | Microsoft Docs
description: Güvenli LDAP (LDAPS) bir Azure AD etki alanı Hizmetleri yönetilen etki alanını yapılandırın
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: maheshu
ms.openlocfilehash: 8da03990ace37b527553b0fe3ff0032515e1b812
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Güvenli LDAP (LDAPS) Azure AD etki alanı Hizmetleri yönetilen etki alanı için yapılandırma

## <a name="before-you-begin"></a>Başlamadan önce
Tamamlandı olun [görev 2 - güvenli LDAP sertifika verme bir. PFX dosyası](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal"></a>Görev 3 - Azure portalını kullanarak yönetilen etki alanı için güvenli LDAP etkinleştirin
Güvenli LDAP etkinleştirmek için aşağıdaki yapılandırma adımlarını gerçekleştirin:

1. Gidin  **[Azure portal](https://portal.azure.com)**.

2. Arama 'etki alanı Hizmetleri'nde' **arama kaynakları** arama kutusu. Seçin **Azure AD etki alanı Hizmetleri** arama sonuç. **Azure AD etki alanı Hizmetleri** sayfası, yönetilen etki alanınız listeler.

    ![Yönetilen etki alanı sağlanacak Bul](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Etki alanı hakkında daha fazla ayrıntı görmek için yönetilen etki alanının adını (örneğin, ' contoso100.com')'yi tıklatın.

    ![Etki alanı hizmetleri - sağlama durumu](./media/getting-started/domain-services-provisioning-state.png)

3. Tıklatın **güvenli LDAP** Gezinti Bölmesi.

    ![Etki Alanı Hizmetleri - güvenli LDAP sayfa](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Varsayılan olarak, yönetilen etki alanınız için güvenli LDAP erişim devre dışıdır. İki durumlu **güvenli LDAP** için **etkinleştirmek**.

    ![Güvenli LDAP etkinleştir](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Varsayılan olarak, internet üzerinden yönetilen etki alanınız için güvenli LDAP erişim devre dışıdır. İki durumlu **Internet üzerinden güvenli LDAP erişim izni** için **etkinleştirmek**istenirse. 

    > [!WARNING]
    > Etki alanınızı Internet üzerinden güvenli LDAP erişimi etkinleştirdiğinizde, Internet üzerinden parola deneme yanılma saldırılarına açıktır. Bu nedenle, bir NSG gerekli kaynak IP adresi aralıklarını erişimi kilitleme ayarlama öneririz. Yönergeler için bkz: [LDAPS erişim internet üzerinden yönetilen etki alanınız için kilitleme](#task-5---lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet).
    >

6. Klasör simgesine aşağıdaki **. Güvenli LDAP Sertifika PFX dosyası**. Güvenli LDAP erişim yönetilen etki alanı için sertifikayla PFX dosyasının yolunu belirtin.

7. Belirtin **şifresini çözmek için parola. PFX dosyası**. Sertifika PFX dosyasına dışarı aktarılırken kullanılan aynı parolayı belirtin.

8. İşiniz bittiğinde tıklatın **kaydetmek** düğmesi.

9. Yönetilen etki alanı için yapılandırılan LDAP güvenli bildiren bir bildirim görürsünüz. Bu işlem tamamlanana kadar diğer etki alanı ayarlarını değiştiremezsiniz.

    ![Güvenli LDAP yönetilen etki alanı için yapılandırma](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Yönetilen etki alanınız için güvenli LDAP etkinleştirmek için yaklaşık 10-15 dakika sürer. Sağlanan güvenli LDAP sertifika gereken ölçütleri eşleşmiyorsa, güvenli LDAP dizin için etkin değil ve bir hata görebilirsiniz. Örneğin, etki alanı adı hatalı olduğu, sertifikanın süresi dolmuş veya süresi yakında doluyor. Bu durumda, geçerli bir sertifikayla yeniden deneyin.
>
>

<br>

## <a name="task-4---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Görev 4 - internet'ten yönetilen etki alanına erişmek için DNS yapılandırma
> [!NOTE]
> **İsteğe bağlı görev** -, düşünmüyorsanız LDAPS kullanarak internet yönetilen etki alanına erişmek için bu yapılandırma görevi atlayın.
>
>

Bu görev başlamadan önce özetlenen adımları tamamladığınızdan emin olun [görev 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

Güvenli LDAP erişim internet üzerinden yönetilen etki alanınız için etkinleştirdikten sonra DNS istemci bilgisayarları Bu yönetilen etki alanı bulabilmesi için güncelleştirmeniz gerekir. Dış IP adresini gösterilir görev 3 sonunda **özellikleri** sekmesinde **dış IP adresi için LDAPS erişim**.

(Örneğin, ' ldaps.contoso100.com') yönetilen etki alanının DNS adı bu dış IP adresine işaret eden dış DNS sağlayıcınız yapılandırın. Örneğin, aşağıdaki DNS girişi oluşturun:

    ldaps.contoso100.com  -> 52.165.38.113

İşte bu kadar - güvenli LDAP internet üzerinden kullanarak yönetilen etki alanına bağlanmak artık hazırsınız.

> [!WARNING]
> İstemci bilgisayarları LDAPS kullanarak yönetilen etki alanı başarıyla bağlanabilmesi için LDAPS sertifikayı veren güvenmelidir unutmayın. Genel olarak güvenilir bir sertifika yetkilisi kullanıyorsanız, istemci bilgisayarlar bu sertifikayı verenler güven bu yana bir şey yapmanız gerekmez. Kendinden imzalı bir sertifika kullanıyorsanız, otomatik olarak imzalanan sertifika ortak parçasını istemci bilgisayarındaki güvenilen sertifika deposuna yükleyin.
>
>


## <a name="task-5---lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>Görev 5 - internet üzerinden yönetilen etki alanınız için güvenli LDAP erişim kilitleme
> [!NOTE]
> İnternet üzerinden yönetilen etki alanına LDAPS erişim etkinleştirmediyseniz, bu yapılandırma görevi atlayın.
>
>

Bu görev başlamadan önce özetlenen adımları tamamladığınızdan emin olun [görev 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

Yönetilen etki alanınız LDAPS erişim için internet üzerinden kullanıma sunan bir güvenlik tehdidi temsil eder. Yönetilen etki alanı güvenli LDAP için kullanılan bağlantı noktası Internet'ten erişilebilir olduğundan (diğer bir deyişle, bağlantı noktası 636). Bu nedenle, belirli bilinen IP adreslerini yönetilen etki alanına erişimi kısıtlamak seçebilirsiniz. Gelişmiş güvenlik için bir ağ güvenlik grubu (NSG) oluşturun ve Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz alt ağı ile ilişkilendirin.

Aşağıdaki tabloda bir örnek, internet üzerinden güvenli LDAP erişim kilitlemek için yapılandırabileceğiniz NSG gösterilmektedir. NSG gelen güvenli LDAP erişim TCP bağlantı noktası IP adreslerinin üzerinden 636 belirtilen kümesinden yalnızca izin veren kurallar kümesini içerir. Varsayılan 'DenyAll' kural, internet'ten diğer gelen trafik için geçerlidir. Belirtilen IP adreslerini Internet üzerinden LDAPS erişime izin verecek şekilde NSG kuralı DenyAll NSG kural daha yüksek önceliğe sahip.

![Örnek internet üzerinden LDAPS erişimi güvenli hale getirmek için NSG](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Daha fazla bilgi** - [ağ güvenlik grupları](../virtual-network/security-overview.md).

<br>


## <a name="troubleshooting"></a>Sorun giderme
Güvenli LDAP kullanarak yönetilen etki alanına bağlanma konusunda sorun yaşıyorsanız, aşağıdaki sorun giderme adımları gerçekleştirin:
* Güvenli LDAP sertifika veren zincirine istemcide güvenilir kabul edildiğinden emin olun. Kök sertifika yetkilisi güven sağlamak için istemcide güvenilen kök sertifika deposuna eklemek tercih edebilirsiniz.
* LDAP istemcisi (örneğin, ldp.exe) bir DNS adı, IP adresi kullanarak güvenli LDAP uç noktasına bağlandığını doğrulayın.
* LDAP istemcisi ortak IP adresine çözümlenecek şekilde yönetilen etki alanı güvenli LDAP bağlanır DNS adını doğrulayın.
* Yönetilen etki alanınız için güvenli LDAP sertifikası konu veya konu alternatif adlarını özniteliği DNS adına sahip doğrulayın.
* Güvenli LDAP Internet üzerinden bağlanıyorsanız, sanal ağı için NSG ayarlarını trafiğine bağlantı noktası 636 internet'ten izin emin olun.

Güvenli LDAP kullanarak yönetilen etki alanına bağlanırken sorun yaşamaya devam ediyorsanız [ürün ekibine başvurun](active-directory-ds-contact-us.md) Yardım. Sorunu daha iyi tanılamaya yardımcı olması için aşağıdaki bilgileri içerir:
* Bağlantı yapma ve başarısız olan Ldp.exe'yi ekran görüntüsü.
* Azure AD Kiracı Kimliğinizi ve yönetilen etki alanınızın DNS etki alanı adı.
* Olarak bağlamaya çalıştığınız tam kullanıcı adı.


## <a name="related-content"></a>İlgili içerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [Grup İlkesi, bir Azure AD etki alanı Hizmetleri yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-group-policy.md)
* [Ağ güvenlik grupları](../virtual-network/security-overview.md)
* [Bir ağ güvenlik grubu oluşturun](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
