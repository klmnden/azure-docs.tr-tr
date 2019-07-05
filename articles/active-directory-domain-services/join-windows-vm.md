---
title: "Azure Active Directory etki alanı Hizmetleri: Yönetilen bir etki alanına Windows Server VM'sine katılma | Microsoft Docs"
description: Bir Windows Server sanal makinesini Azure AD DS'ye katılın
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: 377e253ef595e933f3ccab76bd053e2b416d3a16
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67473178"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Windows Server sanal makinesini yönetilen bir etki alanına ekleme
Bu makalede, Azure portalını kullanarak bir Windows Server sanal makine dağıtma gösterilmektedir. Ardından, sanal makine bir Azure Active Directory etki alanı Hizmetleri (Azure AD DS) yönetilen etki alanına ekleme işlemini gösterir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="step-1-create-a-windows-server-virtual-machine"></a>1\. adım: Windows Server sanal makinesi oluşturma
Azure AD DS etkin sanal ağa katılmış bir Windows sanal makine oluşturmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol bölmenin en üstünde seçin **yeni**.
3. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.

    ![Windows Server 2016 Datacenter bağlantı](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. İçinde **Temelleri** bölmesinde Sihirbazı'nın sanal makine için temel ayarları yapılandırın.

    ![Temel bilgileri bölmesi](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > Sanal makineye oturum açmak için kullanılan bir yerel yönetici hesabının kullanıcı adı ve parola buraya girdiğiniz içindir. Sanal makine parola deneme yanılma saldırılarına karşı korumak için güçlü bir parola seçin. Burada bir etki alanı kullanıcı hesabının kimlik bilgilerini girmeyin.
    >

5. Seçin bir **boyutu** sanal makine için. Daha fazla boyut görmek için seçin **tümünü görüntüle** veya değiştirme **desteklenen disk türü** filtre.

    !["Bir boyut seçin" bölmesi](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. İçinde **ayarları** bölmesinde DS tarafından yönetilen Azure AD etki alanınızı dağıtıldığı sanal ağı seçin. Yönetilen etki alanınıza içine dağıtılır olandan farklı bir alt ağ seçin. Diğer ayarlar için varsayılan değerleri koruyun ve ardından **Tamam**.

    ![Sanal makine için sanal ağ ayarları](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Doğru sanal ağı ve alt ağ seçin.**
    >
    > Yönetilen etki alanınıza dağıtıldığı sanal ağı seçin veya sanal kullanarak bağlı olduğu bir sanal ağ ağ eşlemesi. Bağlantısız bir sanal ağı seçerseniz, sanal makinenin yönetilen etki alanına katılamaz.
    >
    > Yönetilen etki alanınıza ayrılmış bir alt ağa dağıtma öneririz. Bu nedenle, yönetilen etki alanınıza etkin alt seçim değil.

7. Diğer ayarlar için varsayılan değerleri koruyun ve ardından **Tamam**.
8. Üzerinde **satın alma** sayfasında, ayarları gözden geçirin ve ardından **Tamam** sanal makine dağıtma.
9. VM dağıtımı, Azure portalı panosuna sabitlenir.

    ![Bitti](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
10. Dağıtım tamamlandıktan sonra VM hakkında daha fazla bilgi görüntüleyebilirsiniz **genel bakış** sayfası.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-by-using-the-local-administrator-account"></a>2\. adım: Yerel yönetici hesabı kullanarak Windows Server sanal makineye bağlanma
Ardından, yeni oluşturulan Windows Server sanal makine etki alanına bağlanın. Sanal makineyi oluşturduğunuzda belirttiğiniz yerel yönetici kimlik bilgilerini kullanın.

Sanal makineye bağlanmak için aşağıdaki adımları gerçekleştirin:

1. İçinde **genel bakış** bölmesinde **Connect**.  
    Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulup indirilir.

    ![Windows sanal makinesine bağlanın](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.
3. Girin, **yerel yönetici kimlik bilgilerini**, sanal makine oluştururken belirttiğiniz (örneğin, *localhost\mahesh*).
4. Oturum açma işlemi sırasında bir sertifika uyarısı görürseniz seçin **Evet** veya **devam** bağlanmak için.

Bu noktada, yeni oluşturulan Windows sanal makine için yerel yönetici kimlik bilgilerinizle oturum açmanız. Sonraki adım, sanal makine etki alanına sağlamaktır.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-azure-ad-ds-managed-domain"></a>3\. adım: Windows Server sanal makinesini Azure AD DS tarafından yönetilen etki alanına katılın
Windows Server sanal makinesini Azure AD DS tarafından yönetilen bir etki alanına katılmak için aşağıdaki adımları tamamlayın:

1. "2. adımda." gösterildiği gibi Windows Server VM'ye bağlanma Üzerinde **Başlat** ekran açık **Sunucu Yöneticisi**.
2. Sol bölmesinde **Sunucu Yöneticisi'ni** penceresinde **yerel sunucu**.

    ![Sanal makine üzerinde Sunucu Yöneticisi penceresi](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Altında **özellikleri**seçin **çalışma grubu**.
4. İçinde **Sistem Özellikleri** penceresinde **değişiklik** etki alanına katılmak için.

    ![Sistem Özellikleri penceresi](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

5. İçinde **etki alanı** kutusuna, Azure AD DS tarafından yönetilen etki alanı adını belirtin ve ardından **Tamam**.

    ![Birleştirilecek etki alanını belirtin](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

6. Etki alanına katılmak için kimlik bilgilerinizi girmeniz istenir. Kimlik bilgilerini kullanan bir *Azure AD DC administrators grubuna ait kullanıcı*. Yalnızca bu grubun üyeleri, makineleri yönetilen etki alanına ayrıcalıkları.

    ![Kimlik bilgilerini belirtmek için Windows Güvenlik penceresi](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

7. Kimlik bilgileri aşağıdaki yollardan birini belirtebilirsiniz:

   * **UPN biçimini**: (Önerilen) Azure AD'de yapılandırılmış kullanıcı hesabının kullanıcı asıl adı (UPN) sonekini belirtin. Bu örnekte, kullanıcının UPN soneki *bob* olduğu *bob\@domainservicespreview.onmicrosoft.com*.

   * **SAMAccountName biçimi**: Hesap adı SAMAccountName biçiminde belirtebilirsiniz. Bu örnekte, kullanıcı *bob* girmesi *CONTOSO100\bob*.

     > [!TIP]
     > **Kimlik bilgilerini belirtmek için UPN biçimini kullanmanızı öneririz.**
     >
     > Bir kullanıcının UPN önek aşırı uzun olması durumunda (örneğin, *joehasareallylongname*), SAMAccountName otomatik olarak oluşturulmuş olabilir. Birden çok kullanıcı aynı UPN önek varsa (örneğin, *bob*) Azure AD kiracınızda otomatik olarak oluşturulan hizmet tarafından SAMAccountName biçiminde olabilir. Bu durumlarda, UPN biçimini güvenilir etki alanında oturum açmak için kullanılabilir.
     >

8. Aşağıdaki iletiyi başarıyla bir etki alanına katıldıktan sonra etki alanına beklemektedir.

    ![Etki alanına Hoş Geldiniz](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

9. Etki alanına katılma'yı tamamlamak için sanal makineyi yeniden başlatın.

## <a name="troubleshoot-joining-a-domain"></a>Bir etki alanına katılım sorunlarını giderme
### <a name="connectivity-issues"></a>Bağlantı sorunları
Sanal Makine etki alanı bulamıyor ise, aşağıdaki sorun giderme adımlarını deneyin:

* Sanal makine, Azure AD DS etkin aynı sanal ağa bağlı olduğunu doğrulayın. Aksi takdirde, sanal makineye bağlanmak veya etki alanına katılmak silemiyor.

* Buna karşılık Azure AD DS etkin sanal ağa bağlı bir sanal ağ üzerindeki sanal makine olduğunu doğrulayın.

* Yönetilen etki alanı DNS etki alanı adını ping atmayı deneyin (örneğin, *contoso100.com ping*). Bunu yapamıyorsanız, IP adresleri Azure AD DS etkinleştirdiğiniz sayfada görüntülenen etki alanı için ping atmayı deneyin (örneğin, *10.0.0.4 ping*). IP adresi, ancak etki alanı değil ping, DNS hatalı yapılandırılmış olabilir. Etki alanı IP adreslerini sanal ağın DNS sunucuları olarak yapılandırılmış olup olmadığını denetleyin.

* Sanal makinedeki DNS çözümleyicisi önbelleğini temizlemeye deneyin (*ipconfig/flushdns*).

Etki alanına katılmak kimlik bilgilerini ister bir penceresi gösterilirse, bağlantı sorunları yoktur.

### <a name="credentials-related-issues"></a>Kimlik bilgileri ile ilgili sorunlar
Kimlik bilgileri ile ilgili sorun yaşadığınız ve etki alanına katılamıyor, aşağıdaki sorun giderme adımlarını deneyin:

* Kimlik bilgilerini belirtmek için UPN biçimini kullanmayı deneyin. Kiracınızda bulunan aynı UPN öneke sahip çok sayıda kullanıcı varsa veya UPN adı ön eki aşırı uzun olması durumunda, SAMAccountName hesabınız için otomatik olarak oluşturulmuş olabilir. Bu durumlarda, hesabınız için SAMAccountName biçimi ne, beklediğiniz veya şirket içi etki alanınızı kullanmak farklı olabilir.

* Ait bir kullanıcı hesabının kimlik bilgilerini kullanmayı deneyin *AAD DC Administrators* grubu.

* Sahip olduğunuz denetimi [parola eşitlemesini etkinleştirdiğinizden](active-directory-ds-getting-started-password-sync.md) yönetilen etki alanınıza.

* Azure AD içinde yapılandırılan kullanıcı UPN'si kullandınız denetleyin (örneğin, *bob\@domainservicespreview.onmicrosoft.com*) oturum açmak için.

* Tamamlanmak üzere yeteri kadar parola eşitleme için bekleyin başlangıç kılavuzunda belirtilmiş.

## <a name="related-content"></a>İlgili içerik
* [Azure AD DS Başlangıç Kılavuzu](create-instance.md)
* [Bir Azure AD Domain Services etki alanını yönetin](manage-domain.md)
