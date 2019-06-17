---
title: 'Azure Active Directory etki alanı Hizmetleri: Grup İlkesi, yönetilen etki alanlarını yönetme | Microsoft Docs'
description: Yönetim Grup İlkesi, Azure Active Directory Domain Services yönetilen etki alanları
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: mstephen
ms.openlocfilehash: db5fd06bc4d9a923279095ab187d867a6624480a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66245857"
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanında Grup İlkesi yönetme
Azure Active Directory Domain Services 'AADDC Users' ve 'AADDC Computers' kapsayıcıları için yerleşik Grup İlkesi nesneleri (GPO'lar) içerir. Yönetilen etki alanında Grup İlkesi yapılandırmak için bu yerleşik GPO'lar özelleştirebilirsiniz. Ayrıca, 'AAD DC Administrators' grubunun üyeleri, kendi özel kuruluş yönetilen etki alanında oluşturabilirsiniz. Bunlar ayrıca özel GPO'ları oluşturmak ve bunları bu özel OU'lara bağlayın. 'AAD DC Administrators' grubuna ait olan kullanıcılar yönetilen etki alanındaki Grup İlkesi yönetim ayrıcalıkları verilir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](create-instance.md).
4. A **sanal makine etki alanına katılmış** Azure AD Domain Services yönetilen etki yönettiğiniz. Böyle bir sanal makineye sahip değilseniz, başlıklı makalede açıklanan tüm görevleri izleyin [bir Windows sanal makine için yönetilen etki alanına Katıl](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini ihtiyacınız bir **kullanıcı hesabının 'AAD DC Administrators' grubuna ait** Grup İlkesi için yönetilen etki alanınızı yönetmek için dizinde.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-group-policy-for-the-managed-domain"></a>Görev 1 - Grup İlkesi yönetilen etki alanı için uzaktan yönetmek için bir etki alanına katılmış sanal makinesi sağlama
Azure AD Domain Services yönetilen etki alanlarını, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi tanıdık Active Directory yönetim araçlarını kullanarak uzaktan yönetilebilir. Benzer şekilde, yönetilen etki alanı için Grup İlkesi, Grup İlkesi yönetim araçları kullanarak uzaktan yönetilebilir.

Azure AD directory yöneticileri Uzak Masaüstü aracılığıyla yönetilen etki alanındaki etki alanı denetleyicisine bağlanmak için gerekli ayrıcalıklara sahip değilsiniz. 'AAD DC Administrators' grubunun üyeleri, Grup İlkesi için yönetilen etki alanları uzaktan yönetebilirsiniz. Yönetilen etki alanına Windows Server/istemci bilgisayarda Grup İlkesi araçlarının kullanabilirler. Grup İlkesi araçları, Windows Server ve istemci makineleri yönetilen etki alanına katılmış isteğe bağlı Grup İlkesi Yönetimi özelliğinin bir parçası olarak yüklenebilir.

Yönetilen etki alanına katılmış bir Windows Server sanal makine sağlamak için ilk görevdir bakın. Yönergeler için başlıklı makalesine bakabilirsiniz [bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-group-policy-tools-on-the-virtual-machine"></a>Görev 2 - sanal makinede yükleme Grup İlkesi Araçları
Etki alanına katılmış sanal makinede Grup İlkesi Yönetim Araçları'nı yüklemek için aşağıdaki adımları gerçekleştirin.

1. Azure portalına gidin. Tıklayın **tüm kaynakları** sol panelde. Bulun ve görev 1'de oluşturduğunuz sanal makineye tıklayın.
2. Tıklayın **Connect** genel bakış sekmesinde düğmesi. Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulup indirilir.

    ![Windows sanal makinesine bağlanın](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Oturum açma isteminde 'AAD DC Administrators' grubuna ait olan bir kullanıcının kimlik bilgilerini kullanın. Örneğin, kullandığımız 'bob@domainservicespreview.onmicrosoft.com' durumda. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Evet'e tıklayın ya da bağlantıya devam etmek devam edin.
4. Başlangıç ekranından açmak **Sunucu Yöneticisi**. Tıklayın **rol ve Özellik Ekle** orta bölmesinde Sunucu Yöneticisi penceresi.

    ![Sanal makine üzerinde Sunucu Yöneticisi'ni başlatın](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Üzerinde **başlamadan önce** sayfasının **Ekle roller ve Özellikler Sihirbazı**, tıklayın **sonraki**.

    ![Başlamadan önce sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Üzerinde **yükleme türünü** sayfasında **rol tabanlı veya özellik tabanlı yükleme** teslim seçeneğini ve tıklayın **sonraki**.

    ![Kurulum türü sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Üzerinde **sunucu seçimi** sayfasında, sunucu havuzundan geçerli sanal makine seçin ve tıklayın **sonraki**.

    ![Sunucu seçimi sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Üzerinde **sunucu rolleri** sayfasında **sonraki**. Biz, size herhangi bir rolü sunucusuna yüklemiyorsanız bu yana bu sayfasını atlayın.
9. Üzerinde **özellikleri** sayfasında **Grup İlkesi Yönetimi** özelliği.

    ![Özellikler sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. Üzerinde **onay** sayfasında **yükleme** sanal makinede Grup İlkesi Yönetimi özelliği yüklemek için. Özellik yükleme başarıyla tamamlandığında, tıklayın **Kapat** çıkmak için **rol ve Özellik Ekle** Sihirbazı.

    ![Onay sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-the-group-policy-management-console-to-administer-group-policy"></a>Görev 3 - Grup İlkesi'ni yönetmek için Grup İlkesi Yönetim konsolunu Başlat
Yönetilen etki alanındaki Grup İlkesi'ni yönetmek için etki alanına katılmış sanal makine üzerinde Grup İlkesi Yönetim Konsolu'nu kullanın.

> [!NOTE]
> Yönetilen etki alanındaki Grup İlkesi'ni yönetmek için 'AAD DC Administrators' grubunun bir üyesi olmanız gerekir.
>
>

1. Başlangıç ekranından tıklayın **Yönetimsel Araçlar**. Görmelisiniz **Grup İlkesi Yönetimi** Konsolu Sanal makinede yüklü.

    ![Başlatma Grup İlkesi Yönetimi](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Tıklayın **Grup İlkesi Yönetimi** Grup İlkesi Yönetim Konsolu'nu başlatın.

    ![Grup İlkesi konsolu](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Görev 4 - yerleşik Grup İlkesi nesneleri özelleştirme
İki yerleşik Grup İlkesi nesneleri (GPO'lar) - her biri, yönetilen etki alanındaki 'AADDC Computers' ve 'AADDC Users' kapsayıcıları için vardır. Bu GPO'ları yönetilen etki alanında Grup İlkesi yapılandırmak için özelleştirebilirsiniz.

1. İçinde **Grup İlkesi Yönetimi** konsolu, genişletmek için tıklatın **orman: contoso100.com** ve **etki alanları** grup ilkeleri için yönetilen etki alanınıza görmek için düğümleri.

    ![Yerleşik GPO'ları](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. Bu yerleşik GPO'lar, yönetilen etki alanında Grup ilkeleri yapılandırmak için özelleştirebilirsiniz. GPO'ya sağ tıklatıp **Düzenle...**  yerleşik GPO'yu özelleştirmek için. Grup İlkesi yapılandırma Düzenleyicisi araç GPO özelleştirmenize olanak sağlar.

    ![Yerleşik GPO'yu düzenleyin](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. Artık **Grup İlkesi Yönetimi Düzenleyicisi** Konsolu yerleşik GPO'yu düzenleyin. Örneğin, aşağıdaki ekran görüntüsünde yerleşik 'AADDC Computers' GPO özelleştirmek nasıl gösterir.

    ![GPO özelleştirme](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Görev 5 - özel bir Grup İlkesi nesnesi (GPO) oluşturma
Oluşturun veya kendi özel Grup İlkesi nesneleri içeri aktarın. Ayrıca, özel GPO'ları, yönetilen etki alanınızda oluşturduğunuz özel bir OU bağlayabilirsiniz. Özel Kuruluş birimi oluşturma hakkında daha fazla bilgi için bkz. [yönetilen bir etki alanında özel bir OU oluşturma](create-ou.md).

> [!NOTE]
> Yönetilen etki alanındaki Grup İlkesi'ni yönetmek için 'AAD DC Administrators' grubunun bir üyesi olmanız gerekir.
>
>

1. İçinde **Grup İlkesi Yönetimi** konsolu, kendi özel kuruluş birimi (OU) seçmek için tıklayın. OU'ya sağ tıklatıp **bu etki alanında GPO oluştur ve buraya bağla...** .

    ![Özel bir GPO oluştur](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Yeni GPO ve tıklatın için bir ad belirtin **Tamam**.

    ![GPO için bir ad belirtin](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Yeni bir GPO oluşturulur ve kendi özel kuruluş birimine bağlı. GPO'ya sağ tıklatıp **Düzenle...**  menüsünde.

    ![Yeni oluşturulmuş GPO](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. Yeni oluşturulan GPO kullanarak özelleştirebileceğiniz **Grup İlkesi Yönetimi Düzenleyicisi**.

    ![Yeni GPO özelleştirme](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


Kullanma hakkında daha fazla bilgi [Grup İlkesi Yönetim Konsolu](https://technet.microsoft.com/library/cc753298.aspx) Technet üzerindeki kullanılabilir.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](create-instance.md)
* [Bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına ekleyin](active-directory-ds-admin-guide-join-windows-vm.md)
* [Bir Azure AD Domain Services etki alanını yönetin](manage-domain.md)
* [Grup İlkesi Yönetim Konsolu](https://technet.microsoft.com/library/cc753298.aspx)
