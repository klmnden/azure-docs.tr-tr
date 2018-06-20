---
title: 'Azure Active Directory etki alanı Hizmetleri: Grup İlkesi, yönetilen etki alanlarını yönetme | Microsoft Docs'
description: Yönetici Grup İlkesi Azure Active Directory etki alanı Hizmetleri, yönetilen etki alanları
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maheshu
ms.openlocfilehash: 3f49e4ac0073c81a6e55e6653acc7c6531989379
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212249"
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Grup İlkesi, bir Azure AD etki alanı Hizmetleri yönetilen etki alanını yönetme
Azure Active Directory etki alanı Hizmetleri için 'AADDC kullanıcılar' ve 'AADDC Bilgisayarları' kapsayıcı yerleşik Grup İlkesi nesneleri (GPO'lar) içerir. Yönetilen etki alanında Grup İlkesi yapılandırmak için bu yerleşik GPO'ları özelleştirebilirsiniz. Ayrıca, 'AAD DC Yöneticiler' grubunun üyeleri yönetilen etki alanında özel kendi OU'ları oluşturabilirsiniz. Bunlar ayrıca özel GPO'ları oluşturmak ve bunları bu özel OU'lara bağlayın. 'AAD DC Yöneticiler' gruba ait kullanıcılar yönetilen etki alanı Grup İlkesi yönetim ayrıcalıkları verilir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. A **sanal makine etki alanına katılmış** Azure AD etki alanı Hizmetleri yönetilen etki yönettiğiniz. Bu tür bir sanal makine yoksa, başlıklı makalede açıklanan tüm görevleri izleyin [Windows sanal makinesini yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini gereken bir **'AAD DC Yöneticiler' grubuna ait olan kullanıcı hesabı** dizininizde yönetilen etki alanınız için Grup İlkesi yönetmek için.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-group-policy-for-the-managed-domain"></a>Görev 1 - Grup İlkesi yönetilen etki alanı için uzaktan yönetmek için bir etki alanına katılmış sanal makine sağlama
Azure AD etki alanı Hizmetleri yönetilen etki alanları, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi bilinen Active Directory yönetim araçlarını kullanarak uzaktan yönetilebilir. Benzer şekilde, Grup İlkesi yönetilen etki alanı için Grup İlkesi yönetim araçları kullanarak uzaktan yönetilebilir.

Yöneticiler, Azure AD dizini, Uzak Masaüstü aracılığıyla yönetilen etki alanında etki alanı denetleyicisine bağlanmak için ayrıcalıklara sahip değil. 'AAD DC Yöneticiler' grubunun üyeleri, Grup İlkesi yönetilen etki alanları için uzaktan yönetebilirsiniz. Bunlar, yönetilen etki alanına katılmış bir Windows Server/istemci bilgisayarda Grup İlkesi araçları kullanabilirsiniz. Grup İlkesi araçları, Windows Server ve yönetilen etki alanına katılan istemci makineleri üzerinde Grup İlkesi Yönetimi isteğe bağlı özellik bir parçası olarak yüklenebilir.

İlk yönetilen etki alanına katılmış bir Windows Server sanal makine sağlamak için bir görevdir. Yönergeler için başlıklı makaleye bakın [bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılmak](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-group-policy-tools-on-the-virtual-machine"></a>Görev 2 - sanal makineye yükleme Grup İlkesi Araçları
Etki alanına katılmış sanal makinede Grup İlkesi Yönetim Araçları'nı yüklemek için aşağıdaki adımları gerçekleştirin.

1. Azure portalına gidin. Tıklatın **tüm kaynakları** Sol paneldeki. Bulun ve görev 1'de oluşturduğunuz sanal makineye tıklayın.
2. Tıklatın **Bağlan** Genel Bakış sekmesindeki düğmesi. Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve indirilir.

    ![Windows sanal makineye bağlanma](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Oturum açma isteminde 'AAD DC Yöneticiler' grubuna ait olan bir kullanıcının kimlik bilgilerini kullanın. Örneğin, kullandığımız 'bob@domainservicespreview.onmicrosoft.com' bizim durumda. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Evet'i tıklatın veya bağlantı ile devam etmek devam edin.
4. Başlangıç ekranından açmak **Sunucu Yöneticisi'ni**. Tıklatın **rol ve Özellik Ekle** merkezi bölmesinde Sunucu Yöneticisi penceresi.

    ![Sanal makine üzerinde Sunucu Yöneticisi'ni başlatın](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Üzerinde **başlamadan önce** sayfasında **Ekle roller ve Özellikler Sihirbazı**, tıklatın **sonraki**.

    ![Başlamadan önce sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Üzerinde **yükleme türü** sayfasında, bırakın **rol tabanlı veya özellik tabanlı yükleme** işaretli seçeneğini ve tıklayın **sonraki**.

    ![Yükleme türü sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Üzerinde **sunucu seçimi** sayfasında, sunucu havuzundan geçerli sanal makine seçin ve tıklayın **sonraki**.

    ![Sunucu seçimi sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Üzerinde **sunucu rolleri** sayfasında, **sonraki**. Biz herhangi bir rol sunucuda yüklemiyorsanız beri Biz bu sayfayı atla.
9. Üzerinde **özellikleri** sayfasında, **Grup İlkesi Yönetimi** özelliği.

    ![Özellikler sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. Üzerinde **onay** sayfasında, **yükleme** sanal makineye Grup İlkesi Yönetimi özelliğini yüklemek için. Özellik yükleme işlemi başarıyla tamamlandığında tıklatın **Kapat** çıkmak için **rol ve Özellik Ekle** Sihirbazı.

    ![Onay sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-the-group-policy-management-console-to-administer-group-policy"></a>Görev 3 - Grup İlkesi yönetmek için Grup İlkesi Yönetim Konsolu'nu başlatın
Grup İlkesi yönetilen etki alanı yönetmek için etki alanına katılmış sanal makinede Grup İlkesi Yönetim Konsolu'nu kullanın.

> [!NOTE]
> Grup İlkesi yönetilen etki alanı yönetmek için 'AAD DC Yöneticiler' grubunun bir üyesi olmanız gerekir.
>
>

1. Başlangıç ekranından tıklatın **Yönetimsel Araçlar**. Görmeniz gerekir **Grup İlkesi Yönetimi** sanal makinede yüklü konsol.

    ![Başlatma Grup İlkesi Yönetimi](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Tıklatın **Grup İlkesi Yönetimi** Grup İlkesi Yönetim Konsolu'nu başlatın.

    ![Grup İlkesi konsolu](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Görev 4 - yerleşik Grup İlkesi nesnelerini özelleştirme
İki yerleşik Grup İlkesi nesneleri (GPO'lar) - her biri için yönetilen etki alanınız 'AADDC bilgisayarlar' ve 'AADDC kullanıcılar' kapsayıcılarındaki vardır. Bu GPO'ları yönetilen etki alanında Grup İlkesi yapılandırmak için özelleştirebilirsiniz.

1. İçinde **Grup İlkesi Yönetimi** konsolu, genişletmek için tıklatın **orman: contoso100.com** ve **etki alanları** yönetilen etki alanınız için Grup İlkeleri görmek için düğümleri.

    ![Yerleşik GPO'ları](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. Yönetilen etki alanınızda grup ilkeleri yapılandırmak için bu yerleşik GPO özelleştirebilirsiniz. GPO'yu sağ tıklatın ve **Düzenle...**  yerleşik GPO özelleştirmek için. Grup İlkesi yapılandırma Düzenleyicisi araç GPO özelleştirmenize olanak sağlar.

    ![Yerleşik bir GPO'yu düzenleyin](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. Artık kullanabilirsiniz **Grup İlkesi Yönetimi Düzenleyicisi** yerleşik bir GPO'yu düzenlemek için konsol. Örneğin, aşağıdaki ekran görüntüsünde yerleşik 'AADDC bilgisayarlar' GPO özelleştirme gösterilmektedir.

    ![GPO özelleştirme](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Görev 5 - bir özel Grup İlkesi nesnesi (GPO) oluşturun.
Oluşturun veya kendi özel Grup İlkesi nesneleri içeri aktarın. Ayrıca, özel GPO'ları, yönetilen etki alanında oluşturulan özel bir OU bağlayabilirsiniz. Özel Kuruluş birimi oluşturma hakkında daha fazla bilgi için bkz: [yönetilen bir etki alanına özel bir OU oluşturmanız](active-directory-ds-admin-guide-create-ou.md).

> [!NOTE]
> Grup İlkesi yönetilen etki alanı yönetmek için 'AAD DC Yöneticiler' grubunun bir üyesi olmanız gerekir.
>
>

1. İçinde **Grup İlkesi Yönetimi** konsolu, özel, kuruluş birimi (OU) seçmek için tıklatın. OU'yu sağ tıklatın ve **bu etki alanında GPO oluştur ve buraya bağla...** .

    ![Özel bir GPO oluşturun](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Yeni GPO ve tıklatın için bir ad belirtin **Tamam**.

    ![GPO için bir ad belirtin](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Yeni bir GPO oluşturulur ve özel OU'ya bağlı. GPO'yu sağ tıklatın ve **Düzenle...**  menüsünde.

    ![Yeni oluşturulmuş GPO](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. Yeni oluşturulan GPO kullanarak özelleştirebileceğiniz **Grup İlkesi Yönetimi Düzenleyicisi**.

    ![Yeni GPO özelleştirme](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


Kullanma hakkında daha fazla bilgi [Grup İlkesi Yönetim Konsolu](https://technet.microsoft.com/library/cc753298.aspx) Technet üzerindeki kullanılabilir.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [Grup İlkesi Yönetim Konsolu](https://technet.microsoft.com/library/cc753298.aspx)
