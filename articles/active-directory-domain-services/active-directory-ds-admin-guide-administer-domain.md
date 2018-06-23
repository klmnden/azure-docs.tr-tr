---
title: 'Azure Active Directory etki alanı Hizmetleri: yönetilen bir etki alanını yönetme | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri yönetilen etki alanlarını yönetme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: 2ee5250147a82199057a3bf6f043627616e7443d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333695"
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Azure Active Directory Etki Alanı Hizmetleri tarafından yönetilen etki alanını yönetme
Bu makalede Azure Active Directory (AD) etki alanı Hizmetleri yönetilen etki alanını yönetme gösterilmiştir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri tamamlamak için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. A **sanal makine etki alanına katılmış** Azure AD etki alanı Hizmetleri yönetilen etki yönettiğiniz. Bu tür bir sanal makine yoksa, başlıklı makalede açıklanan tüm görevleri izleyin [Windows sanal makinesini yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini gereken bir **'AAD DC Yöneticiler' grubuna ait olan kullanıcı hesabı** dizininizde yönetilen etki alanınızı yönetmek için.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Yönetilen bir etki alanı üzerinde gerçekleştirdiğiniz yönetim görevleri
'AAD DC Yöneticiler' grubunun üyeleri gibi görevleri gerçekleştirmesine olanak sağlayan ayrıcalıklar yönetilen etki alanında verilir:

* Makineler için yönetilen etki alanına katılın.
* Yerleşik GPO’yu yönetilen etki alanındaki 'AADDC Computers' ve 'AADDC Users' kapsayıcıları için yapılandırma.
* Yönetilen etki alanında DNS’yi yönetin.
* Oluşturma ve özel kuruluş birimlerini (OU) yönetilen etki alanı yönetme.
* Yönetilen etki alanına katılan bilgisayarlara yönetici erişimi edinin.

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Yönetilen bir etki alanında olmayan yönetici ayrıcalıkları
Etki alanı, düzeltme eki uygulama için izleme ve diğer yedek almak gibi etkinlikler dahil olmak üzere Microsoft tarafından yönetilir. Etki alanı kilitli ve etki alanındaki belirli yönetim görevleri gerçekleştirmek için ayrıcalığınız yok. Görev yapamayacağınız bazı örnekleri aşağıda verilmiştir.

* Yönetilen etki alanı için etki alanı yöneticisi veya kuruluş yöneticisi ayrıcalıklarına sahip değilsiniz.
* Yönetilen etki alanı şemasını olamaz.
* Uzak Masaüstü kullanarak yönetilen etki alanı için etki alanı denetleyicilerine bağlanamıyor.
* Yönetilen bir etki alanına etki alanı denetleyicilerini ekleyemezsiniz.

## <a name="task-1---create-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Görev 1 - bir etki alanına katılmış Windows Server sanal uzaktan yönetilen etki alanını yönetmek için makine oluşturma
Azure AD etki alanı Hizmetleri yönetilen etki alanları, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi bilinen Active Directory yönetim araçları kullanılarak yönetilebilir. Kiracı Yöneticiler Uzak Masaüstü aracılığıyla yönetilen etki alanında etki alanı denetleyicisine bağlanmak için ayrıcalıklara sahip değil. 'AAD DC Yöneticiler' grubunun üyeleri, yönetilen etki alanı AD yönetilen etki alanına katılmış bir Windows Server/istemci bilgisayardan yönetim araçları kullanarak uzaktan yönetebilirsiniz. AD Yönetim Araçları, Windows Server ve yönetilen etki alanına katılan istemci makineleri Uzak Sunucu Yönetim Araçları (RSAT) isteğe bağlı özellik bir parçası olarak yüklenebilir.

Yönetilen etki alanına katılmış bir Windows Server sanal makine ayarlamak için ilk adımdır bakın. Yönergeler için başlıklı makaleye bakın [bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılmak](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Yönetilen etki alanında bir istemci bilgisayardan (örneğin, Windows 10) uzaktan yönetme
AAD DS yönetmek için bu makalede bir Windows Server sanal makine kullan'ndaki yönergeleri etki alanı yönetilen. Ancak, bunu yapmak için bir Windows istemci (örneğin, Windows 10) sanal makine kullanmayı da seçebilirsiniz.

Yapabilecekleriniz [Uzak Sunucu Yönetim Araçları (RSAT) yüklemek](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) TechNet'teki yönergeleri izleyerek bir Windows istemci sanal makine üzerinde.

## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Görev 2 - sanal makineye yükleme Active Directory Yönetim Araçları
Etki alanına katılmış sanal makinede Active Directory yönetim araçlarını yüklemek için aşağıdaki adımları tamamlayın. TechNet daha fazla bilgi için bkz: [yükleme ve uzak sunucu yönetim araçları kullanarak bilgi](https://technet.microsoft.com/library/hh831501.aspx).

1. Azure portalına gidin. Tıklatın **tüm kaynakları** Sol paneldeki. Bulun ve görev 1'de oluşturduğunuz sanal makineye tıklayın.
2. Tıklatın **Bağlan** Genel Bakış sekmesindeki düğmesi. Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve indirilir.

    ![Windows sanal makineye bağlanma](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. 'AAD DC Yöneticiler' grubuna ait olan bir kullanıcının kimlik bilgilerini kullanın. Örneğin, 'bob@domainservicespreview.onmicrosoft.com'. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Evet'i tıklatın veya bağlantı ile devam etmek devam edin.
4. Başlangıç ekranından açmak **Sunucu Yöneticisi'ni**. Tıklatın **rol ve Özellik Ekle** merkezi bölmesinde Sunucu Yöneticisi penceresi.

    ![Sanal makine üzerinde Sunucu Yöneticisi'ni başlatın](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Üzerinde **başlamadan önce** sayfasında **Ekle roller ve Özellikler Sihirbazı**, tıklatın **sonraki**.

    ![Başlamadan önce sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Üzerinde **yükleme türü** sayfasında, bırakın **rol tabanlı veya özellik tabanlı yükleme** işaretli seçeneğini ve tıklayın **sonraki**.

    ![Yükleme türü sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Üzerinde **sunucu seçimi** sayfasında, sunucu havuzundan geçerli sanal makine seçin ve tıklayın **sonraki**.

    ![Sunucu seçimi sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Üzerinde **sunucu rolleri** sayfasında, **sonraki**.
9. Üzerinde **özellikleri** sayfası, genişletmek için tıklatın **Uzak Sunucu Yönetim Araçları** düğümü genişletmek için tıklayın ve sonra **Rol Yönetim Araçları** düğümü. Seçin **AD DS ve AD LDS Araçları** rol yönetim araçları listesinden özelliği.

    ![Özellikler sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. Üzerinde **onay** sayfasında, **yükleme** yükleme AD ve sanal makinede AD LDS Araçları özelliği. Özellik yükleme işlemi başarıyla tamamlandığında tıklatın **Kapat** çıkmak için **rol ve Özellik Ekle** Sihirbazı.

    ![Onay sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Görev 3 - bağlanmak ve yönetilen etki alanı keşfedin
Şimdi, keşfetmek ve yönetilen etki alanını yönetmek için Windows Server AD yönetim araçlarını kullanabilirsiniz.

> [!NOTE]
> Yönetilen etki alanını yönetmek için 'AAD DC Yöneticiler' grubunun bir üyesi olmanız gerekir.
>
>

1. Başlangıç ekranından tıklatın **Yönetimsel Araçlar**. Sanal makinede yüklü AD Yönetimsel Araçlar görmeniz gerekir.

    ![Sunucuda yüklü Yönetim Araçları](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Tıklatın **Active Directory Yönetim Merkezi**.

    ![Active Directory Yönetim Merkezi](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Etki alanı keşfetmek için sol bölmede (örneğin, ' contoso100.com') etki alanı adına tıklayın. 'AADDC bilgisayarlar' ve 'AADDC kullanıcılar' sırasıyla adlı iki kapsayıcı dikkat edin.

    ![ADAC - görünüm etki alanı](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Adlı bir kapsayıcıya tıklatın **AADDC kullanıcılar** tüm kullanıcılar ve gruplar için yönetilen etki alanına ait görmek için. Kullanıcı hesapları görmeniz gerekir ve grupları, Azure AD'den Kiracı bu kapsayıcıda Göster ayarlama. Bu kapsayıcıda bildirimi Bu örnekte, bir kullanıcı hesabı 'Kemal' adlı kullanıcı ve 'AAD DC Yöneticiler' adlı bir grup için kullanılabilir.

    ![ADAC - etki alanı kullanıcıları](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Adlı bir kapsayıcıya tıklatın **AADDC bilgisayarlar** Bu yönetilen etki alanına katılmış bilgisayarları görmek için. Hangi etki alanına katılan geçerli sanal makine, bir giriş görmeniz gerekir. Azure AD etki alanı Hizmetleri yönetilen etki alanına katılmış olan tüm bilgisayarlar için bilgisayar hesaplarının, bu 'AADDC bilgisayarlar' kapsayıcısında depolanır.

    ![ADAC - etki alanına katılmış bilgisayarlar](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Uzak Sunucu Yönetim Araçları dağıtma](https://technet.microsoft.com/library/hh831501.aspx)
