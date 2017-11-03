---
title: "Azure Active Directory etki alanı Hizmetleri: bir Windows Server VM yönetilen bir etki alanına katılın. | Microsoft Docs"
description: "Azure AD etki alanı Hizmetleri için Windows Server sanal makine birleştirme"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 5f661dba2e647ac905e7d84927fdbf6dbc76094f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Windows Server sanal makinesini yönetilen bir etki alanına ekleme
Bu makalede Azure portal kullanarak bir Windows Server sanal makine dağıtma gösterilmektedir. Ardından, sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılmak nasıl gösterir.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>1. adım: Windows Server sanal makine oluşturma
Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sanal ağa katılmış bir Windows sanal makine oluşturmak için aşağıdaki adımları gerçekleştirin.

1. [http://portal.azure.com](http://portal.azure.com) sayfasından Azure portalda oturum açın.
2. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
3. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.

    ![Görüntüyü seçin](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Sanal makine için temel ayarları yapılandırın **Temelleri** Sihirbazı sayfası.

    ![VM için temel ayarlarını yapılandırın](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > Sanal makinede oturum açmak için kullanılan bir yerel yönetici hesabının kullanıcı adı ve parola buraya girdiğiniz içindir. Sanal makineyi parola yanılma saldırılarına karşı korumak için güçlü bir parola seçin. Burada etki alanı kullanıcı hesabının kimlik bilgilerini girmeyin.
    >

5. Seçin bir **boyutu** sanal makine için. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin.

    ![VM boyutunu seçin](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. Üzerinde **ayarları** sayfasında, Azure AD etki alanı Hizmetleri etki alanı yönetilen sanal ağ dağıtılırken Sihirbazı'nı seçin. Yönetilen etki alanınızı içine dağıtılan fazla farklı bir alt ağ seçin. Diğer ayarlar için varsayılanları tutun ve **Tamam**.

    ![Sanal makine için sanal ağ seçin](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Doğru sanal ağ ve alt ağ seçin.**
    > Yönetilen etki alanınızı dağıtıldığı sanal ağı seçin veya sanal kullanarak kendisine bağlı bir sanal ağ ağ eşlemesi. Bağlantısız bir sanal ağ seçerseniz, sanal makineyi yönetilen bir etki alanına katılamaz.
    > Ayrılmış bir alt ağ, yönetilen etki alanı dağıtma öneririz. Bu nedenle, yönetilen etki alanınızı etkinleştirdikten alt çekme değil.

7. Üzerinde **satın alma** sayfasında, ayarları gözden geçirin ve tıklayın **Tamam** sanal makine dağıtılamıyor.
8. VM dağıtımı Azure portal panosuna sabitlendi.

    ![Bitti](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
9. Dağıtım tamamlandıktan sonra VM ile ilgili bilgileri görebilirsiniz **genel bakış** sayfası.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>2. adım: yerel yönetici hesabını kullanarak Windows Server sanal makineye bağlanma
Artık, yeni oluşturulan Windows Server sanal etki alanına katılmak için makine bağlayın. Sanal makine oluşturulurken belirtilen yerel yönetici kimlik bilgilerini kullanın.

Sanal makineye bağlanmak için aşağıdaki adımları gerçekleştirin.

1. Tıklatın **Bağlan** düğmesini **genel bakış** sayfası. Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve indirilir.

    ![Windows sanal makineye bağlanma](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın.
3. Oturum açma isteminde girin, **yerel yönetici kimlik bilgilerini**, sanal makine oluşturulurken belirtilen. Bu örnekte, örneğin, 'localhost\mahesh' kullandığımız.
4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Evet'i tıklatın veya bağlantı ile devam etmek devam edin.

Bu noktada, yerel yönetici kimlik bilgilerini kullanarak yeni oluşturulan Windows sanal makine için oturum açmanız. Sanal Makine etki alanına katılmak için sonraki adım olacaktır.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>3. adım: Katılım AAD DS yönetilen etki alanına Windows Server sanal makine
Windows Server sanal makine AAD DS yönetilen etki alanına eklemek için aşağıdaki adımları gerçekleştirin.

1. Adım 2'de gösterildiği gibi Windows sunucuya bağlanın. Başlangıç ekranından açmak **Sunucu Yöneticisi'ni**.
2. Tıklatın **yerel sunucu** Sunucu Yöneticisi'ni penceresinin sol bölmesinde.

    ![Sanal makine üzerinde Sunucu Yöneticisi'ni başlatın](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Tıklatın **çalışma grubu** altında **özellikleri** bölümü. İçinde **Sistem Özellikleri** özellik sayfası, tıklatın **değişiklik** etki alanına katılamaz.

    ![Sistem Özellikleri Sayfası](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Azure AD etki alanı Hizmetleri yönetilen etki alanında etki alanı adını belirtin **etki alanı** textbox tıklatıp **Tamam**.

    ![Birleştirilecek etki alanını belirtin](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Etki alanına katılmak için kimlik bilgilerinizi girmeniz istenir. Sağlamak sizin **AAD DC yöneticilere ait olan bir kullanıcı kimlik bilgilerini belirtin** grubu. Yalnızca bu grubun üyeleri makinelere yönetilen etki alanına katılmak için izinleri yönetme izinleri var.

    ![Etki alanına katılma kimlik bilgilerini belirtin](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Kimlik bilgileri aşağıdaki yollardan birini belirtebilirsiniz:

   * **UPN biçimi: (önerilen)** Azure AD içinde yapılandırıldığı gibi kullanıcı hesabı için UPN sonekini belirtin. Bu örnekte, kullanıcı 'bob' UPN soneki eklenir 'bob@domainservicespreview.onmicrosoft.com'.
   * **SAMAccountName biçimi:** SAMAccountName biçiminde hesap adını belirtebilirsiniz. Bu örnekte, kullanıcı 'bob' 'CONTOSO100\bob' girmeniz gerekir.

     > [!TIP]
     > **Kimlik bilgileri belirtmek için UPN biçimini kullanmanızı öneririz.**
     > Bir kullanıcının UPN önek aşırı uzun (örneğin, 'joehasareallylongname') ise, otomatik olarak oluşturulan SAMAccountName olabilir. Birden çok kullanıcı aynı UPN önek (örneğin, ' bob'), Azure AD kiracınızda varsa, bunların SAMAccountName biçimi hizmeti tarafından otomatik olarak oluşturulan olabilir. Bu durumlarda, UPN biçimini güvenilir etki alanında oturum açmak için kullanılabilir.
     >

7. Etki alanına katılma başarılı olduktan sonra etki alanına davet eder aşağıdaki iletiyi görürsünüz. Sanal Makine etki alanına katılma işlemi tamamlanması için yeniden başlatın.

    ![Etki alanına Hoş Geldiniz](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Sorun giderme etki alanına katılma
### <a name="connectivity-issues"></a>Bağlantı sorunları
Sanal Makine etki alanı bulamıyor ise, aşağıdaki sorun giderme adımlarını bakın:

* Sanal Makine etki alanı Hizmetleri'nde etkinleştirdikten, aynı sanal ağa bağlı olduğundan emin olun. Aksi durumda, sanal makine etki alanına bağlanamıyor ve bu nedenle etki alanına katılma alamıyor.
* Buna karşılık etki alanı Hizmetleri'ni etkinleştirdikten sanal ağa bağlı bir sanal ağ üzerindeki sanal makine olduğundan emin olun.
* Yönetilen etki alanı (örneğin, ' ping contoso100.com') etki alanı adını kullanarak etki alanına ping işlemi yapmayı deneyin. Bunu yapamıyorsanız, IP adreslerini Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sayfada görüntülenen etki alanı için ping işlemi yapmayı deneyin (örneğin, ' 10.0.0.4 ping'). IP adresi, ancak etki alanı değil ping bağlanabiliyorsanız, DNS hatalı yapılandırılmış olabilir. Etki alanının IP adresleri sanal ağ için DNS sunucuları olarak yapılandırılıp yapılandırılmadığını denetleyin.
* Sanal makinedeki (' ipconfig/flushdns") DNS çözümleyicisi önbelleği temizleme deneyin.

Etki alanına katılmak kimlik bilgilerini ister iletişim kutusuna alırsanız, bağlantı sorunları yok.

### <a name="credentials-related-issues"></a>Kimlik bilgileri ile ilgili sorunlar
Eğer kimlik bilgileriyle konusunda sorun yaşıyoruz ve etki alanına katılamıyor için aşağıdaki adımları bakın.

* Kimlik bilgileri belirtmek için UPN biçimini kullanmayı deneyin. Kiracınızda aynı UPN sonekine sahip birden çok kullanıcı varsa veya UPN önek aşırı uzun olması durumunda, SAMAccountName, hesabınız için otomatik olarak oluşturulan olabilir. Bu nedenle, hesabınız için SAMAccountName biçimi ne, beklediğiniz veya şirket içi etki alanınızda kullanmayı farklı olabilir.
* 'AAD DC Yöneticiler' grubuna ait bir kullanıcı hesabının kimlik bilgilerini kullanmayı deneyin.
* Başlarken kılavuzunda açıklanan adımlara uygun olarak [parola eşitlemesini etkinleştirdiğinizden](active-directory-ds-getting-started-password-sync.md) emin olun.
* Azure AD içinde yapılandırıldığı gibi kullanıcı UPN kullandığınızdan emin olun (örneğin, 'bob@domainservicespreview.onmicrosoft.com') oturum açmak için.
* Başlarken Kılavuzu'nda belirtildiği gibi tamamlamak yeterince uzun parola eşitleme için beklenen olun.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
