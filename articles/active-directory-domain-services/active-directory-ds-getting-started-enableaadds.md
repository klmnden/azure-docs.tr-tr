---
title: "Azure AD Etki Alanı Hizmetleri: Azure AD Etki Alanı Hizmetleri&quot;ni Etkinleştirme | Microsoft Belgeleri"
description: "Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/19/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 64bb5f7e78a6e48faf3487da780597b25891a2fa


---
# <a name="enable-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri'ni etkinleştirme
## <a name="task-3-enable-azure-ad-domain-services"></a>Görev 3: Azure AD Etki Alanı Hizmetleri'ni etkinleştirme
Bu görevde dizininiz için Azure AD Etki Alanı Hizmetleri'ni etkinleştireceksiniz. Dizininizde Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek için aşağıdaki yapılandırma adımlarını gerçekleştirin.

1. **Klasik Azure portalına** ([https://manage.windowsazure.com](https://manage.windowsazure.com)) gidin.
2. Sol bölmede **Active Directory** düğümünü seçin.
3. Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek istediğiniz Azure AD kiracısını (dizin) seçin.
   
    ![Azure AD Dizini Seçme](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. **Configure (Yapılandır)** sekmesine tıklayın.
   
    ![Dizinin yapılandır sekmesi](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Aşağı kaydırarak **etki alanı hizmetleri** adlı bölüme gelin.
   
    ![Etki Alanı Hizmetleri yapılandırma bölümü](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)
6. **Enable domain services for this directory (Bu dizin için etki alanı hizmetlerini etkinleştir)** seçeneğini **EVET** olarak değiştirin. Sayfada Azure AD Etki Alanı hizmetleri için birkaç yapılandırma seçeneğinin daha görüntülendiğini fark edersiniz.
   
    ![Etki Alanı Hizmetlerini Etkinleştirme](./media/active-directory-domain-services-getting-started/enable-domain-services.png)
   
   > [!NOTE]
   > Kiracınız için Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğinizde, Azure AD kullanıcıların kimliklerinin doğrulanması için gerekli olan Kerberos ve NTLM kimlik bilgisi karmalarını oluşturur ve depolar.
   > 
   > 
7. **Etki alanı hizmetlerinin DNS etki alanı adını** belirtin.
   
   * Dizinin varsayılan etki alanı adı (**.onmicrosoft.com** etki alanı son eki ile biten) varsayılan olarak seçilir.
   * "Etki Alanları" sekmesinde yapılandırdığınız doğrulanmış ve aynı zamanda doğrulanmamış etki alanları dahil olmak üzere, liste Azure AD dizininiz için yapılandırılan tüm etki alanlarını içerir.
   * Ayrıca, özel etki alanı adı da yazabilirsiniz. Bu örnekte, özel etki alanı adı olarak 'contoso100.com' yazdık.
     
     > [!WARNING]
     > Belirttiğiniz etki alanı adının etki alanı ön ekinin (örneğin, 'contoso100.com' etki alanı adında 'contoso100') 15 karakterden kısa olduğundan emin olun. Etki alanı ön eki 15 karakterden daha uzun olan bir Azure AD Etki Alanı Hizmetleri etki alanını oluşturamazsınız.
     > 
     > 
8. Yönetilen etki alanı için seçtiğiniz DNS etki alanı adının sanal ağda zaten bulunmadığından emin olun. Özellikle aşağıdakileri kontrol edin:
   
   * sanal ağda aynı DNS etki alanı adına sahip bir etki alanınız zaten varsa.
   * seçtiğiniz sanal ağın şirket içi ağınıza bir VPN bağlantısı bulunuyorsa ve şirket içi ağınızda aynı DNS etki alanı adına sahip bir etki alanınız zaten varsa.
   * sanal ağda bu ada sahip bir bulut hizmetiniz zaten varsa.
9. Bir sonraki adım, Azure AD Etki Alanı Hizmetleri'nin kullanılabilir olmasını istediğiniz bir sanal ağ seçmektir. **Connect domain services to this virtual network** (Bu sanal ağa etki alanı hizmeti bağlayın) açılan menüsünde, oluşturduğunuz sanal ağı ve adanmış alt ağı seçin.
   
   * Belirttiğiniz sanal ağın Azure AD Etki Alanı Hizmetleri tarafından desteklenen bir Azure bölgesine ait olduğundan emin olun. Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.
   * Azure AD Etki Alanı Hizmetleri'nin desteklenmediği bir bölgeye ait olan sanal ağlar açılan listede görüntülenmez.
   * Azure AD Etki Alanı Hizmetleri için sanal ağ üzerinde adanmış bir alt ağ kullanın. Ağ geçidi alt ağını seçmediğinizden emin olun. Bkz. [ağ ile ilgili dikkat edilmesi gerekenler](active-directory-ds-networking.md). 
   * Benzer şekilde, Azure Resource Manager kullanılarak oluşturulan sanal ağlar açılan listede görünmez. Resource Manager tabanlı sanal ağlar şu anda Azure AD Etki Alanı Hizmetleri'nde desteklenmemektedir.
10. Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek için sayfanın altındaki görev bölmesinde **Kaydet**'e tıklayın.
11. Sayfada ‘Bekliyor…’ iletisi görüntülenir durumu görüntülenir.
    
    ![Etki Alanı Hizmetleri'ni Etkinleştirme - beklemede durumu](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)
    
    > [!NOTE]
    > Azure AD Etki Alanı Hizmetleri, yönetilen etki alanınız için yüksek kullanılabilirlik sağlar. Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sonra Etki Alanı Hizmetleri'nin sanal ağda kullanılabilir olduğu IP adreslerinin tek tek görüntülendiğine dikkat edin. İkinci IP adresi kısa bir süre içinde, hizmet etki alanınız için yüksek kullanılabilirliği etkinleştirdiği zaman görüntülenir. Yüksek kullanılabilirlik etki alanınız için yapılandırıldığında ve etkin olduğunda **Yapılandır** sekmesinin **etki alanı hizmetleri** bölümünde iki IP adresi görmeniz gerekir.
    > 
    > 
12. Yaklaşık 20-30 dakika sonra, Etki Alanı Hizmetleri'nin sanal ağınızda kullanılabilir olduğu ilk IP adresini **Yapılandır** sayfasının **IP adresi** alanında görürsünüz.
    
    ![Etki Alanı Hizmetleri etkin - ilk IP sağlanmış](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
13. Etki alanınız için yüksek kullanılabilirlik uygulandığında sayfada iki IP adresinin görüntülendiğini görürsünüz. Yönetilen etki alanınız, bu iki IP adresindeki seçilen sanal ağlarınız üzerinde kullanılabilir. Sanal ağınızın DNS ayarlarını güncelleştirebilmek için bu IP adreslerini not edin. Bu adım, etki alanına katılım gibi işlemler için sanal ağdaki sanal makinelerin etki alanına bağlanmasını sağlar.
    
    ![Etki Alanı Hizmetleri etkin - her iki IP de sağlanmış](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Azure AD kiracınızın boyutuna (kullanıcı, grup vb. sayısı) bağlı olarak, yönetilen etki alanınızla eşitleme işlemi biraz zaman alır. Bu eşitleme işlemi arka planda gerçekleşir. On binlerce nesne içeren büyük kiracılarda tüm kullanıcıların, grup üyeliklerinin ve kimlik bilgilerinin eşitlenmesi bir veya iki gün sürebilir.
> 
> 

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Görev 4 - Azure sanal ağı için DNS ayarlarını güncelleştirme
Sonraki yapılandırma görevi [Azure sanal ağı için DNS ayarlarını güncelleştirmedir](active-directory-ds-getting-started-dns.md).




<!--HONumber=Dec16_HO1-->


