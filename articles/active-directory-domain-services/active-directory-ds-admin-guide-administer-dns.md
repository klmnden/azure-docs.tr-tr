---
title: "Azure Active Directory etki alanı Hizmetleri: Yönetilen etki alanlarını DNS'yi | Microsoft Docs"
description: Azure Active Directory Domain Services yönetilen etki alanlarını DNS'yi yönetme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: d6705f9f7e324c915c38d01c54bdf16826c62380
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60419321"
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanında DNS'yi yönetme
Azure Active Directory Domain Services yönetilen etki alanı için DNS çözümlemesini sağlayan bir DNS (etki alanı adı çözümlemesi) sunucusu içerir. Bazen, yönetilen etki alanında DNS yapılandırmanız gerekebilir. Etki alanına katılmamış olan makineler için DNS kayıtları oluşturma, yük Dengeleyiciler için sanal IP adreslerini yapılandırın veya dış DNS ileticileri Kurulum gerekebilir. Bu nedenle 'AAD DC Administrators' grubuna ait kullanıcılar yönetilen etki alanındaki DNS yönetim ayrıcalıkları verilir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri tamamlamak için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](active-directory-ds-getting-started.md).
4. A **sanal makine etki alanına katılmış** Azure AD Domain Services yönetilen etki yönettiğiniz. Böyle bir sanal makineye sahip değilseniz, başlıklı makalede açıklanan tüm görevleri izleyin [bir Windows sanal makine için yönetilen etki alanına Katıl](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini ihtiyacınız bir **kullanıcı hesabının 'AAD DC Administrators' grubuna ait** yönetilen etki alanınız için DNS yönetmek için dizinde.

<br>

## <a name="task-1---create-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Görev 1 - uzaktan yönetilen etki alanı için DNS yönetmek için bir etki alanına katılmış sanal makine oluşturma
Azure AD Domain Services yönetilen etki alanlarını, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi tanıdık Active Directory yönetim araçlarını kullanarak uzaktan yönetilebilir. Benzer şekilde, yönetilen etki alanı için DNS, DNS sunucu yönetim araçları kullanarak uzaktan yönetilebilir.

Azure AD directory yöneticileri Uzak Masaüstü aracılığıyla yönetilen etki alanındaki etki alanı denetleyicisine bağlanmak için gerekli ayrıcalıklara sahip değilsiniz. 'AAD DC Administrators' grubunun üyeleri, DNS, DNS sunucusu araçları yönetilen etki alanına katılmış bir Windows Server/istemci bilgisayardan uzaktan kullanarak yönetilen etki alanları için yönetebilirsiniz. DNS sunucusu araçları, Uzak Sunucu Yönetim Araçları (RSAT) isteğe bağlı bir özellik bir parçasıdır.

İlk görev bir Windows Server sanal makinesini yönetilen etki alanına katılmış oluşturmaktır. Yönergeler için başlıklı makalesine bakabilirsiniz [bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Görev 2 - sanal makinede yükleme DNS Sunucusu Araçları
Etki alanına katılmış sanal makinedeki DNS Yönetim Araçları'nı yüklemek için aşağıdaki adımları tamamlayın. Daha fazla bilgi için [yükleme ve Uzak Sunucu Yönetim Araçları'nı kullanarak](https://technet.microsoft.com/library/hh831501.aspx), Technet konusuna bakın.

1. Azure portalına gidin. Tıklayın **tüm kaynakları** sol panelde. Bulun ve görev 1'de oluşturduğunuz sanal makineye tıklayın.
2. Tıklayın **Connect** genel bakış sekmesinde düğmesi. Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulup indirilir.

    ![Windows sanal makinesine bağlanın](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. 'AAD DC Administrators' grubuna ait olan bir kullanıcının kimlik bilgilerini kullanın. Örneğin, 'bob@domainservicespreview.onmicrosoft.com'. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Evet'e tıklayın ya da bağlanmaya devam.

4. Başlangıç ekranından açmak **Sunucu Yöneticisi**. Tıklayın **rol ve Özellik Ekle** orta bölmesinde Sunucu Yöneticisi penceresi.

    ![Sanal makine üzerinde Sunucu Yöneticisi'ni başlatın](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Üzerinde **başlamadan önce** sayfasının **Ekle roller ve Özellikler Sihirbazı**, tıklayın **sonraki**.

    ![Başlamadan önce sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Üzerinde **yükleme türünü** sayfasında **rol tabanlı veya özellik tabanlı yükleme** teslim seçeneğini ve tıklayın **sonraki**.

    ![Kurulum türü sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Üzerinde **sunucu seçimi** sayfasında, sunucu havuzundan geçerli sanal makine seçin ve tıklayın **sonraki**.

    ![Sunucu seçimi sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Üzerinde **sunucu rolleri** sayfasında **sonraki**.
9. Üzerinde **özellikleri** sayfası, genişletmek için tıklatın **Uzak Sunucu Yönetim Araçları** düğümü genişletmek için tıklayın ve sonra **Rol Yönetim Araçları** düğümü. Seçin **DNS Sunucusu Araçları** rol yönetim araçları listesinden özelliği.

    ![Özellikler sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. Üzerinde **onay** sayfasında **yükleme** sanal makinedeki DNS Sunucusu Araçları özelliği yüklemek için. Özellik yükleme başarıyla tamamlandığında, tıklayın **Kapat** çıkmak için **rol ve Özellik Ekle** Sihirbazı.

    ![Onay sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Görev 3 - DNS yönetmek için DNS yönetim konsolunu Başlat
Şimdi, yönetilen etki alanında DNS'yi için Windows Server DNS araçlarını kullanabilirsiniz.

> [!NOTE]
> Yönetilen etki alanında DNS'yi için 'AAD DC Administrators' grubunun bir üyesi olmanız gerekir.
>
>

1. Başlangıç ekranından tıklayın **Yönetimsel Araçlar**. Görmelisiniz **DNS** Konsolu Sanal makinede yüklü.

    ![Yönetim Araçları - DNS konsolunu](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Tıklayın **DNS** DNS Yönetimi konsolunu başlatmak için.
3. İçinde **DNS sunucusuna bağlan** iletişim kutusunda, tıklayın **şu bilgisayarda**, yönetilen etki alanı (örneğin, ' contoso100.com') DNS etki alanı adını girin.

    ![DNS konsolunu - etki alanına bağlayın](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. DNS konsolunda yönetilen etki alanına bağlanır.

    ![DNS konsolunu - etki alanını yönetme](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. Şimdi, AAD etki alanı Hizmetleri etkin sanal ağ içindeki bilgisayarların DNS girdileri eklemek üzere DNS konsolunu da kullanabilirsiniz.

> [!WARNING]
> DNS Yönetimi araçlarını kullanarak yönetilen etki alanı için DNS yönetirken dikkat edin. Emin olmak sizin **silmeyin veya etki alanındaki etki alanı Hizmetleri tarafından kullanılan yerleşik DNS kayıtlarını değiştirme**. Yerleşik DNS kayıtları etki alanı DNS kayıtları, ad sunucusu kayıtlarını ve DC konumu için kullanılan diğer kayıtları içerir. Bu kayıtlar değiştirirseniz, etki alanı Hizmetleri sanal ağ üzerinde herhangi bir kesinti.
>
>

DNS yönetme hakkında daha fazla bilgi için bkz. [DNS araçları Technet makalesi](https://technet.microsoft.com/library/cc753579.aspx).

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına ekleyin](active-directory-ds-admin-guide-join-windows-vm.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [DNS Yönetim Araçları](https://technet.microsoft.com/library/cc753579.aspx)
