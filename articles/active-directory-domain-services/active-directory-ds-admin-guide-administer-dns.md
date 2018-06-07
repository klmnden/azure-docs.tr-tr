---
title: 'Azure Active Directory etki alanı Hizmetleri: Yönetilen etki alanları DNS yönetme | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri yönetilen etki alanları DNS yönetme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2017
ms.author: maheshu
ms.openlocfilehash: b2cb351e18cfa8a0d0552c9a2a36e5bb11b2d3f7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34587509"
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD etki alanı Hizmetleri yönetilen etki alanının DNS yönetme
Azure Active Directory etki alanı Hizmetleri yönetilen etki alanı için DNS çözümlemesi sağlayan bir DNS (etki alanı adı çözümlemesine) sunucusu içerir. Bazen, yönetilen etki alanında DNS yapılandırmanız gerekebilir. Etki alanına katılmamış makineler için DNS kayıtlarını oluşturun, yük Dengeleyiciler için sanal IP adreslerini yapılandırın veya dış DNS ileticileri Kurulum gerekebilir. Bu nedenle, 'AAD DC Yöneticiler' gruba ait kullanıcılar yönetilen etki alanı DNS yönetim ayrıcalıkları verilir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. A **sanal makine etki alanına katılmış** Azure AD etki alanı Hizmetleri yönetilen etki yönettiğiniz. Bu tür bir sanal makine yoksa, başlıklı makalede açıklanan tüm görevleri izleyin [Windows sanal makinesini yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini gereken bir **'AAD DC Yöneticiler' grubuna ait olan kullanıcı hesabı** dizininizde yönetilen etki alanınız için DNS yönetmek için.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Görev 1 - yönetilen etki alanı için DNS uzaktan yönetmek için bir etki alanına katılmış sanal makine sağlama
Azure AD etki alanı Hizmetleri yönetilen etki alanları, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi bilinen Active Directory yönetim araçlarını kullanarak uzaktan yönetilebilir. Benzer şekilde, yönetilen etki alanı için DNS DNS sunucu yönetim araçları kullanarak uzaktan yönetilebilir.

Yöneticiler, Azure AD dizini, Uzak Masaüstü aracılığıyla yönetilen etki alanında etki alanı denetleyicisine bağlanmak için ayrıcalıklara sahip değil. 'AAD DC Yöneticiler' grubunun üyeleri, yönetilen etki alanına katılmış bir Windows Server/istemci bilgisayardan DNS sunucu araçları kullanarak uzaktan yönetilen etki alanları için DNS yönetebilirsiniz. DNS sunucusu araçları, Windows Server ve yönetilen etki alanına katılan istemci makineleri Uzak Sunucu Yönetim Araçları (RSAT) isteğe bağlı özellik bir parçası olarak yüklenebilir.

İlk yönetilen etki alanına katılmış bir Windows Server sanal makine sağlamak için bir görevdir. Yönergeler için başlıklı makaleye bakın [bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılmak](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Görev 2 - sanal makineye yükleme DNS Sunucusu Araçları
Etki alanına katılmış sanal makinede DNS Yönetim Araçları'nı yüklemek için aşağıdaki adımları gerçekleştirin. Daha fazla bilgi için [yükleme ve uzak sunucu yönetim araçları kullanarak](https://technet.microsoft.com/library/hh831501.aspx), Technet konusuna bakın.

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
9. Üzerinde **özellikleri** sayfası, genişletmek için tıklatın **Uzak Sunucu Yönetim Araçları** düğümü genişletmek için tıklayın ve sonra **Rol Yönetim Araçları** düğümü. Seçin **DNS Sunucusu Araçları** rol yönetim araçları listesinden özelliği.

    ![Özellikler sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. Üzerinde **onay** sayfasında, **yükleme** DNS Sunucusu Araçları özelliği sanal makineye yüklemek için. Özellik yükleme işlemi başarıyla tamamlandığında tıklatın **Kapat** çıkmak için **rol ve Özellik Ekle** Sihirbazı.

    ![Onay sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Görev 3 - DNS yönetmek için DNS management konsolunu başlatın
DNS Sunucusu Araçları özelliği yüklü göre sanal makine etki alanına katıldı, biz DNS araçları yönetilen etki alanı DNS yönetmek için kullanabilirsiniz.

> [!NOTE]
> Yönetilen etki alanı DNS yönetmek için 'AAD DC Yöneticiler' grubunun bir üyesi olmanız gerekir.
>
>

1. Başlangıç ekranından tıklatın **Yönetimsel Araçlar**. Görmeniz gerekir **DNS** sanal makinede yüklü konsol.

    ![Yönetim Araçları - DNS konsolunu](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Tıklatın **DNS** DNS Yönetimi konsolunu başlatın.
3. İçinde **DNS sunucusuna bağlan** iletişim kutusunda, başlıklı seçeneğini **aşağıdaki bilgisayar**, yönetilen etki alanı (örneğin, ' contoso100.com') DNS etki alanı adını girin.

    ![DNS konsolunu - etki alanına bağlayın](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. DNS konsolunu yönetilen etki alanına bağlanır.

    ![DNS konsolunu - etki alanını yönetme](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. Şimdi, AAD etki alanı Hizmetleri'ni etkinleştirdikten sanal ağ içindeki bilgisayarlar için DNS girişleri eklemek için DNS konsolunu da kullanabilirsiniz.

> [!WARNING]
> DNS Yönetim Araçları'nı kullanarak yönetilen etki alanı için DNS yönetirken, dikkatli olun. Sağlamak sizin **silmeyin veya etki alanındaki etki alanı Hizmetleri tarafından kullanılan yerleşik DNS kayıtlarını değiştirme**. Yerleşik DNS kayıtları etki alanı DNS kayıtları, ad sunucusu kayıtlarını ve DC konumu için kullanılan diğer kayıtlarını içerir. Bu kayıtları değiştirirseniz, etki alanı Hizmetleri'nin sanal ağda herhangi bir kesinti.
>
>

Bkz: [DNS araçları Technet makalesi](https://technet.microsoft.com/library/cc753579.aspx) DNS yönetme hakkında daha fazla bilgi için.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [DNS Yönetim Araçları](https://technet.microsoft.com/library/cc753579.aspx)
