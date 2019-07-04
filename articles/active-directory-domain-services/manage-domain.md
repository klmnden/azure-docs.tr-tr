---
title: 'Azure Active Directory etki alanı Hizmetleri: Yönetilen bir etki alanını yönetme | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanlarını yönetme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: iainfou
ms.openlocfilehash: 4095161cd35ff610b357196c91e71256a8bc0263
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67473018"
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Azure Active Directory Etki Alanı Hizmetleri tarafından yönetilen etki alanını yönetme
Bu makalede, Azure Active Directory (AD) etki alanı Hizmetleri yönetilen etki alanını yönetme işlemini göstermektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri tamamlamak için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](create-instance.md).
4. A **sanal makine etki alanına katılmış** Azure AD Domain Services yönetilen etki yönettiğiniz. Böyle bir sanal makineye sahip değilseniz, başlıklı makalede açıklanan tüm görevleri izleyin [bir Windows sanal makine için yönetilen etki alanına Katıl](active-directory-ds-admin-guide-join-windows-vm.md).
5. Kimlik bilgilerini ihtiyacınız bir **kullanıcı hesabının 'AAD DC Administrators' grubuna ait** yönetilen etki alanınızı yönetmek için dizinde.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Yönetilen bir etki alanı üzerinde gerçekleştirdiğiniz yönetim görevleri
'AAD DC Administrators' grubunun üyesi gibi görevleri gerçekleştirmesine olanak sağlayan yönetilen etki alanında ayrıcalıklar verilir:

* Makineleri yönetilen etki alanına katılın.
* Yerleşik GPO’yu yönetilen etki alanındaki 'AADDC Computers' ve 'AADDC Users' kapsayıcıları için yapılandırma.
* Yönetilen etki alanında DNS’yi yönetin.
* Oluşturabilir ve özel kuruluş birimine (OU) yönetilen etki alanındaki yönetebilir.
* Yönetilen etki alanına katılan bilgisayarlara yönetici erişimi edinin.

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Yönetici ayrıcalıkları yönetilen bir etki alanında olmayan
Etki alanı, düzeltme eki uygulama için izleme ve diğer yedekler almak gibi etkinlikler dahil olmak üzere Microsoft tarafından yönetilir. Etki alanı kilitli ve etki alanındaki belirli yönetim görevleri gerçekleştirmek için ayrıcalıklara sahip değilsiniz. Görev yapamayacağınız bazı örnekleri aşağıda verilmiştir.

* Yönetilen etki alanı için etki alanı yöneticisinin veya Kurumsal Yönetici ayrıcalıkları yok.
* Yönetilen etki alanı şemasını olamaz.
* Uzak Masaüstü kullanarak yönetilen etki alanı için etki alanı denetleyicisine bağlanamıyor.
* Yönetilen etki alanına etki alanı denetleyicileri ekleyemezsiniz.

## <a name="task-1---create-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Görev 1 - bir etki alanına katılmış Windows Server sanal yönetilen etki alanı uzaktan makinesi oluşturma
Azure AD Domain Services yönetilen etki alanlarını, Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell gibi tanıdık Active Directory yönetim araçları kullanılarak yönetilebilir. Kiracı yöneticileri Uzak Masaüstü aracılığıyla yönetilen etki alanındaki etki alanı denetleyicisine bağlanmak için gerekli ayrıcalıklara sahip değilsiniz. 'AAD DC Administrators' grubunun üyeleri, uzaktan yönetilen etki alanına katılmış bir Windows Server/istemci bilgisayardan AD Yönetimsel Araçlar'ı kullanarak yönetilen etki alanlarını yönetebilir. AD Yönetim Araçları, Uzak Sunucu Yönetim Araçları (RSAT) isteğe bağlı özellik Windows sunucu ve istemci makineleri yönetilen etki alanına katılmış bir parçası olarak yüklenebilir.

İlk adım Windows Server sanal makinesini yönetilen etki alanına katılmış ayarlamaktır. Yönergeler için başlıklı makalesine bakabilirsiniz [bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Yönetilen etki alanı (örneğin, Windows 10) istemci bilgisayardan uzaktan yönetme
AAD-DS yönetmek için yönergeleri Bu makalede kullanılan bir Windows Server sanal makinesini yönetilen etki. Ancak, bunu yapmak için bir Windows istemci (örneğin, Windows 10) sanal makine kullanmayı da seçebilirsiniz.

Yapabilecekleriniz [Uzak Sunucu Yönetim Araçları (RSAT) yükleme](https://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) TechNet'te yönergeleri izleyerek bir Windows istemci sanal makine üzerinde.

## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Görev 2 - sanal makinede yükleme Active Directory Yönetim Araçları
Etki alanına katılmış sanal makinede Active Directory Yönetim Araçları'nı yüklemek için aşağıdaki adımları tamamlayın. Daha fazla bilgi için TechNet bkz [yükleme ve Uzak Sunucu Yönetim Araçları'nı kullanarak bilgi](https://technet.microsoft.com/library/hh831501.aspx).

1. Azure portalına gidin. Tıklayın **tüm kaynakları** sol panelde. Bulun ve görev 1'de oluşturduğunuz sanal makineye tıklayın.
2. Tıklayın **Connect** genel bakış sekmesinde düğmesi. Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulup indirilir.

    ![Windows sanal makinesine bağlanın](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. 'AAD DC Administrators' grubuna ait olan bir kullanıcının kimlik bilgilerini kullanın. Örneğin, 'bob@domainservicespreview.onmicrosoft.com'. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Evet'e tıklayın ya da bağlantıya devam etmek devam edin.
4. Başlangıç ekranından açmak **Sunucu Yöneticisi**. Tıklayın **rol ve Özellik Ekle** orta bölmesinde Sunucu Yöneticisi penceresi.

    ![Sanal makine üzerinde Sunucu Yöneticisi'ni başlatın](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Üzerinde **başlamadan önce** sayfasının **Ekle roller ve Özellikler Sihirbazı**, tıklayın **sonraki**.

    ![Başlamadan önce sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Üzerinde **yükleme türünü** sayfasında **rol tabanlı veya özellik tabanlı yükleme** teslim seçeneğini ve tıklayın **sonraki**.

    ![Kurulum türü sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Üzerinde **sunucu seçimi** sayfasında, sunucu havuzundan geçerli sanal makine seçin ve tıklayın **sonraki**.

    ![Sunucu seçimi sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Üzerinde **sunucu rolleri** sayfasında **sonraki**.
9. Üzerinde **özellikleri** sayfası, genişletmek için tıklatın **Uzak Sunucu Yönetim Araçları** düğümü genişletmek için tıklayın ve sonra **Rol Yönetim Araçları** düğümü. Seçin **AD DS ve AD LDS Araçları** rol yönetim araçları listesinden özelliği.

    ![Özellikler sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. Üzerinde **onay** sayfasında **yükleme** yükleme AD ve sanal makine üzerinde AD LDS Araçları özelliği. Özellik yükleme başarıyla tamamlandığında, tıklayın **Kapat** çıkmak için **rol ve Özellik Ekle** Sihirbazı.

    ![Onay sayfası](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Görev 3 - bağlanmak ve yönetilen etki alanı keşfedin
Artık, Windows Server AD Yönetimsel Araçlar keşfedin ve yönetilen etki alanını yönetmek için kullanabilirsiniz.

> [!NOTE]
> Yönetilen etki alanını yönetmek için 'AAD DC Administrators' grubunun bir üyesi olmanız gerekir.
>
>

1. Başlangıç ekranından tıklayın **Yönetimsel Araçlar**. Sanal makinede yüklü AD Yönetimsel Araçlar görmeniz gerekir.

    ![Sunucuda yüklü Yönetim Araçları](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Tıklayın **Active Directory Yönetim Merkezi'ni**.

    ![Active Directory Yönetim Merkezi](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Etki alanı keşfetmek için etki alanı adı (örneğin, ' contoso100.com') sol bölmesinde tıklayın. 'AADDC Computers' ve 'AADDC Users' adlı, sırasıyla iki kapsayıcı dikkat edin.

    ![ADAC - görünüm etki alanı](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Adlı bir kapsayıcıya tıklayın **AADDC Users** tüm kullanıcılar ve gruplar yönetilen etki alanına ait görmek için. Kullanıcı hesaplarını görmeniz gerekir ve grupları Azure AD Kiracı bu kapsayıcıda show ayarlama. Bu örnekte bildirimi, 'Kemal' adlı kullanıcı ve 'AAD DC Administrators' adlı bir grup için bir kullanıcı hesabı, bu kapsayıcı içinde kullanılabilir.

    ![ADAC - etki alanı kullanıcıları](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Adlı bir kapsayıcıya tıklayın **AADDC Computers** Bu yönetilen etki alanına katılmış bilgisayarları görmek için. Etki alanına katılmışsa, geçerli sanal makine, bir giriş görmeniz gerekir. Azure AD Domain Services yönetilen etki alanına katılmış olan tüm bilgisayarlar için bilgisayar hesaplarının, bu 'AADDC Computers' kapsayıcısında depolanır.

    ![ADAC - etki alanına katılmış bilgisayarlar](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](create-instance.md)
* [Bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına ekleyin](active-directory-ds-admin-guide-join-windows-vm.md)
* [Uzak Sunucu Yönetim Araçları dağıtma](https://technet.microsoft.com/library/hh831501.aspx)
