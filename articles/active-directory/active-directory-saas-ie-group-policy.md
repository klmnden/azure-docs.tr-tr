---
title: Azure erişim paneli uzantısı için bir GPO kullanarak IE dağıtma | Microsoft Docs
description: My uygulamaları portal için Internet Explorer eklentisi dağıtmak için Grup İlkesi kullanma
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a39e454bd0993f07efd1168404df453f3013e0fa
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısı dağıtma
Bu öğretici, Grup İlkesi uzaktan erişim paneli uzantısı Internet Explorer için kullanıcılarınızın makinelere yüklemeniz için nasıl kullanılacağını gösterir. Bu uzantı kullanılarak yapılandırılmış olan uygulamalarda oturum açmak için gereken Internet Explorer kullanıcılar için gerekli [parola tabanlı çoklu oturum açma](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on).

Yöneticiler'in bu uzantı dağıtımını otomatik hale getirmek önerilir. Aksi takdirde, kullanıcılar yükleyip uzantısı kendileri hangi kullanıcı hataya ve yönetici izinlerine sahiptir. Bu öğretici, Grup İlkesi kullanarak yazılım dağıtımlarını otomatikleştirme bir yöntem kapsar. [Grup İlkesi hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Erişim paneli uzantısı da kullanılabilir [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) ve [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), kümelesini yüklemek için yönetici izinleri gerektirir.

## <a name="prerequisites"></a>Önkoşullar
* Ayarlamış olduğunuz [Active Directory etki alanı Hizmetleri](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), ve kullanıcılarınızın makineler, etki alanına.
* Grup İlkesi nesnesi (GPO) düzenlemek için "Ayarları düzenleme" izni olması gerekir. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri bu izne sahip: etki alanı yöneticileri, kuruluş yöneticileri ve Group Policy Creator Owners. [Daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>1. adım: dağıtım noktası oluşturma
İlk olarak, uzantı uzaktan yüklemek istediğiniz makineler tarafından erişilebilir bir ağ konumu üzerinde yükleyici paketi yerleştirmeniz gerekir. Bunu yapmak için şu adımları uygulayın:

1. Sunucuya bir yönetici olarak oturum açın
2. İçinde **Sunucu Yöneticisi'ni** penceresinde, Git **dosya ve depolama hizmetleri**.
   
    ![Açık dosyaları ve depolama hizmetleri](./media/active-directory-saas-ie-group-policy/files-services.png)
3. Git **paylaşımları** sekmesi. Ardından **görevleri** > **yeni paylaşım...**
   
    ![Açık dosyaları ve depolama hizmetleri](./media/active-directory-saas-ie-group-policy/shares.png)
4. Tamamlamak **yeni paylaşım Sihirbazı'nı** ve kullanıcılarınızın makinelerden erişilebildiğinden emin olmak için izinleri ayarlayın. [Paylaşımları hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc753175.aspx)
5. Aşağıdaki Microsoft Windows Installer paketini (.msi dosyası) indirin: [erişim paneli Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)
6. Yükleyici paketi paylaşımdaki istediğiniz bir konuma kopyalayın.
   
    ![.Msi dosya paylaşımına kopyalayın.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. İstemci makineleriniz Yükleyici paketi paylaşımından erişebilir olduğunu doğrulayın. 

## <a name="step-2-create-the-group-policy-object"></a>Adım 2: Grup İlkesi nesnesi oluşturma
1. Active Directory etki alanı Hizmetleri (AD DS) yüklemenizi barındıran sunucu için oturum açın.
2. Sunucu Yöneticisi'nde Git **Araçları** > **Grup İlkesi Yönetimi**.
   
    ![İçin Araçlar > Grup İlkesi Yönetimi](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. Sol bölmesinde **Grup İlkesi Yönetimi** penceresinde, kuruluş birimi (OU) hiyerarşinizi görüntülemek ve hangi kapsamda grup ilkesini uygulamak istediğiniz belirleyin. Örneğin, test etmek için birkaç kullanıcılara dağıtmak için küçük bir OU çekme karar verebilir veya kuruluşunuzun tümüne dağıtmak için en üst düzey bir OU çekme.
   
   > [!NOTE]
   > İsterseniz oluşturma veya kuruluş birimlerini (OU) düzenleme, Sunucu Yöneticisi geçiş ve Git **Araçları** > **Active Directory Kullanıcıları ve Bilgisayarları**.
   > 
   > 
4. Bir OU seçtikten sonra sağ tıklatın ve seçin **bu etki alanında GPO oluştur ve buraya bağla...**
   
    ![Yeni bir GPO oluşturun](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. İçinde **yeni GPO** istemi, yeni Grup İlkesi nesnesi için bir ad yazın.
   
    ![Yeni GPO adı](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. Oluşturulan ve seçin Grup İlkesi nesnesi sağ **Düzenle**.
   
    ![Yeni GPO düzenleme](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-the-installation-package"></a>3. adım: yükleme paketini atama
1. Temel uzantısı dağıtmak isteyip istemediğinizi belirleyin **Bilgisayar Yapılandırması** veya **Kullanıcı Yapılandırması**. Kullanırken [Bilgisayar Yapılandırması](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), uzantı hangi kullanıcıların oturum açın, bağımsız olarak bilgisayara yüklenir. İle [Kullanıcı Yapılandırması](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), kullanıcılar bunları bağımsız olarak, hangi bilgisayarların oturum açmak için yüklü uzantısı.
2. Sol bölmesinde **Grup İlkesi Yönetimi Düzenleyicisi** penceresinde, seçtiğiniz hangi yapılandırma türüne bağlı olarak aşağıdaki klasör yollarını birini gidin:
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. Sağ **yazılım yükleme**seçeneğini belirleyip **yeni** > **paket...**
   
    ![Yeni bir yazılım yükleme paketi oluşturun.](./media/active-directory-saas-ie-group-policy/new-package.png)
4. Yükleyici paketinden içeren paylaşılan klasöre gidin [1. adım: dağıtım noktası oluşturma](#step-1-create-the-distribution-point).msi dosyasını seçin ve tıklatın **açık**.
   
   > [!IMPORTANT]
   > Paylaşım bu aynı sunucuda bulunuyorsa, yerel dosya yolu yerine ağ dosya yolu .msi eriştiğiniz doğrulayın.
   > 
   > 
   
    ![Paylaşılan klasörden yükleme paketini seçin.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. İçinde **Yazılımı Dağıt** komut istemi, select **atanan** dağıtım yönteminize için. Daha sonra, **Tamam**'a tıklayın.
   
    ![Atanan belirleyin ve ardından Tamam'ı tıklatın.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Uzantı, seçili OU'ya şimdi dağıtılır. [Grup İlkesi Yazılım yükleme hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>4. adım: Otomatik etkinleştirmek için Internet Explorer uzantısı
Kullanılabilmesi için önce yükleyicinin çalıştırmanın yanı sıra, her uzantısını Internet Explorer için açıkça etkinleştirilmesi gerekir. Grup İlkesi kullanarak erişim paneli uzantısı etkinleştirmek için aşağıdaki adımları izleyin:

1. İçinde **Grup İlkesi Yönetimi Düzenleyicisi** penceresinde, hangi yapılandırma türüne bağlı olarak, seçtiğiniz içinde aşağıdaki yollardan birini gidin [adım 3: yükleme paketini atamak](#step-3-assign-the-installation-package):
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. Sağ **eklenti listesi**seçip **Düzenle**.
    ![Eklenti Listesi düzenleyin.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)
3. İçinde **eklenti listesi** penceresinde, seçin **etkin**. Ardından, altında **seçenekleri** 'yi tıklatın **göster...** .
   
    ![Etkinleştir'i tıklatın ve ardından göster...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. İçinde **içeriğini göster** penceresinde, aşağıdaki adımları gerçekleştirin:
   
   1. İlk sütun için ( **değer adı** alanı), kopyalama ve yapıştırma aşağıdaki sınıf kimliği: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   2. İkinci sütun için ( **değeri** alanı), aşağıdaki değeri türü: `1`
   3. Tıklatın **Tamam** kapatmak için **içeriğini göster** penceresi.
      
      ![Yukarıda belirtildiği gibi değerlerini doldurun.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. Tıklatın **Tamam** değişikliklerinizi uygulamak ve kapatmak için **eklenti listesi** penceresi.

Seçili OU'da makineler için şimdi Uzantının etkinleştirilmesi gerekir. [Etkinleştirmek veya Internet Explorer eklentileri devre dışı bırakmak için Grup İlkesi'ni kullanma hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>Adım 5 (isteğe bağlı): "Parolayı anımsa" istemi devre dışı bırak
Kullanıcılar oturum erişim paneli uzantısı kullanılarak Web siteleri açma, Internet Explorer "parolanızı saklamak ister misiniz?" sorusunu sorarken aşağıdaki istemi Göster

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Bu istemi görmesini, kullanıcılarınızın engellemek istiyorsanız, parolaları anımsama otomatik tamamlama önlemek için aşağıdaki adımları izleyin:

1. İçinde **Grup İlkesi Yönetimi Düzenleyicisi** penceresi, aşağıda listelenen yoluna gidin. Bu yapılandırma ayarının yalnızca altında kullanılabilir **Kullanıcı Yapılandırması**.
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. Adlı ayar Bul **kullanıcı adları ve parolalar formlarında için Otomatik Tamamlama özelliğini**.
   
   > [!NOTE]
   > Active Directory önceki sürümlerinde bu ayar adı olarak listesinde **parolaları kaydetmek için otomatik tamamlama izin verme**. Bu öğreticide açıklanan ayarı configuration Bu ayar için farklıdır.
   > 
   > 
   
    ![Kullanıcı Ayarları altında şunun unutmayın.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. Yukarıdaki ayar sağ tıklatın ve seçin **Düzenle**.
4. Başlıklı pencerede **kullanıcı adları ve parolalar formlarında için Otomatik Tamamlama özelliğini**seçin **devre dışı**.
   
    ![Devre dışı bırak seçin](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. Tıklatın **Tamam** pencereyi kapatın ve bu değişiklikleri uygulamak için.

Kullanıcıları artık kendi kimlik bilgilerini veya kullanım otomatik tamamlama önceden depolanan kimlik bilgileri erişmek için depolama mümkün olacaktır. Ancak, bu ilke otomatik tamamlama, form alanları, arama alanları gibi diğer türleri için kullanmaya devam etmek kullanıcıların izin vermez.

> [!WARNING]
> Kullanıcıların bazı kimlik bilgileri, bu ilke işlem depolamak seçtikten sonra bu ilke etkinleştirildiğinde *değil* zaten depolanmış kimlik bilgilerini temizleyin.
> 
> 

## <a name="step-6-testing-the-deployment"></a>6. adım: dağıtımı test etme
Uzantı dağıtımı başarılı olup olmadığını doğrulamak için aşağıdaki adımları izleyin:

1. Kullanarak dağıttıysanız **Bilgisayar Yapılandırması**, oturum açın, seçili OU'ya ait bir istemci makine [adım 2: Grup İlkesi nesnesi oluşturmak](#step-2-create-the-group-policy-object). Kullanarak dağıttıysanız **Kullanıcı Yapılandırması**, ilgili OU'ya ait olan bir kullanıcı olarak oturum açmak emin olun.
2. Bileşenler değişiklikler Grup İlkesi için tam olarak için güncelleştirme birkaç oturum sürebilir bu makine ile. Güncelleştirmeyi uygulamak için açık bir **komut istemi** penceresini açın ve aşağıdaki komutu çalıştırın: `gpupdate /force`
3. Yüklemenin gerçekleşmesi makine yeniden başlatmanız gerekir. Önyükleme uzantısı sırasında normal yükler önemli ölçüde daha uzun sürebilir.
4. Yeniden başlattıktan sonra açmak **Internet Explorer**. Pencerenin sağ üst köşesinde tıklatın **Araçları** (dişli simgesi) ve ardından **eklentileri yönetme**.
   
    ![İçin Araçlar > eklentileri yönetme](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. İçinde **Eklentileri Yönet** penceresinde doğrulayın **erişim paneli uzantısı** yüklendi ve kendi **durum** ayarlandığından **etkin**.
   
    ![Erişim paneli uzantısı yüklü ve etkin olduğunu doğrulayın.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](manage-apps/what-is-single-sign-on.md)
* [Erişim paneli uzantısı Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md)

