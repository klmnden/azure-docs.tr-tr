---
title: "Azure Active Directory Domain Services: Azure Active Directory Domain Services’ı etkinleştirme | Microsoft Docs"
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
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: e5f1fe51d8931985fa55b2d8c0a3fd25bb93f20f
ms.lasthandoff: 04/12/2017


---
# <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services’ı etkinleştirme
## <a name="task-3-enable-azure-active-directory-domain-services"></a>Görev 3: Azure Active Directory Domain Services’ı etkinleştirme
Bu görevde aşağıdaki işlemleri yaparak dizininiz için Azure Active Directory Domain Services’ı (Azure AD DS) etkinleştirirsiniz:

1. [Klasik Azure portalı](https://manage.windowsazure.com)'na gidin.
2. Sol bölmede **Active Directory** düğmesini seçin.
3. Azure AD DS’yi etkinleştirmek istediğiniz Azure Active Directory (Azure AD) kiracısını (dizin) seçin.

    ![Azure AD Dizini Seçme](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. **Önizleme dizini** sayfasında **Yapılandır** sekmesine tıklayın.

    ![Dizinin yapılandır sekmesi](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. **Etki alanı hizmetleri** altında **Bu dizin için etki alanı hizmetlerini etkinleştir** seçeneğini **Evet** olarak değiştirin.  
    Sayfada diğer Azure Active Directory Domain Services yapılandırma seçenekleri görünür.

    ![Etki Alanı Hizmetlerini Etkinleştirme](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Kiracınız için Azure Active Directory Domain Services'i etkinleştirdiğinizde, Azure AD kullanıcıların kimliklerinin doğrulanması için gerekli olan Kerberos ve NTLM kimlik bilgisi karmalarını oluşturur ve depolar.
   >
   >
6. **Etki alanı hizmetlerinin DNS etki alanı adını** belirtin.

   * Dizinin varsayılan etki alanı adı (**.onmicrosoft.com** son eki ile) varsayılan olarak seçilir.

   * **Etki Alanları** sekmesinde yapılandırdığınız doğrulanmış ve aynı zamanda doğrulanmamış etki alanları dahil olmak üzere, liste Azure AD dizininiz için yapılandırılan tüm etki alanlarını içerir.

   * Özel bir etki alanı adı da girebilirsiniz. Bu örnekte özel etki alanı adı *contoso100.com* şeklindedir.

     > [!WARNING]
     > Belirttiğiniz etki alanı adının ön eki (örneğin, *contoso100.com* etki alanı adında *contoso100*) 15 veya daha az karakter içermelidir. 15 karakterden uzun bir ön ek ile Azure Active Directory Domain Services etki alanı oluşturamazsınız.
     >
     >
7. Yönetilen etki alanı için seçtiğiniz DNS etki alanı adının sanal ağda zaten bulunmadığından emin olun. Özellikle, aşağıdakilerin olup olmadığını denetleyin:

   * Sanal ağda aynı DNS etki alanı adına sahip bir etki alanınız zaten varsa.

   * Seçtiğiniz sanal ağın şirket içi ağınıza bir VPN bağlantısı bulunuyorsa ve şirket içi ağınızda aynı DNS etki alanı adına sahip bir etki alanınız zaten varsa.

   * Sanal ağda bu ada sahip bir bulut hizmetiniz zaten varsa.
8. Azure Active Directory Domain Services’ın kullanılabilir olmasını istediğiniz sanal ağı seçin. **Bu sanal ağa etki alanı hizmeti bağlayın** açılır listesinde, oluşturduğunuz sanal ağı ve ayrılmış alt ağı seçin. Ayrıca aşağıdakileri dikkate alın:

   * Belirttiğiniz sanal ağın Azure Active Directory Domain Services tarafından desteklenen bir Azure bölgesine ait olup olmadığından emin olun. Azure Active Directory Domain Services’ın kullanılabildiği Azure bölgelerini öğrenmek için [Bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.

   * Azure Active Directory Domain Services'in desteklenmediği bir bölgeye ait olan sanal ağlar açılan listede görüntülenmez.

   * Azure Active Directory Domain Services için sanal ağ üzerinde adanmış bir alt ağ kullanın. Ağ geçidi alt ağını *seçmeyin*. Bkz. [ağ ile ilgili dikkat edilmesi gerekenler](active-directory-ds-networking.md).

   * Benzer şekilde, Azure Resource Manager kullanılarak oluşturulan sanal ağlar açılan listede görünmez. Resource Manager tabanlı sanal ağlar şu anda Azure Active Directory Domain Services'de desteklenmemektedir.
9. Azure Active Directory Domain Services'i etkinleştirmek için sayfanın altındaki görev bölmesinde **Kaydet**'e tıklayın. 
    * Azure Active Directory Domain Services dizininiz için etkinleştirilirken, sayfada *Bekliyor* durumu gösterilir.

        ![Domain Services Etkinleştirme penceresi](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Azure Active Directory Domain Services, yönetilen etki alanınız için yüksek kullanılabilirlik sağlar. Azure Active Directory Domain Services etkinleştirildikten sonra, sanal ağ üzerinde etki alanı hizmetlerine yönelik IP adresleri birer birer görüntülenir. İkinci IP adresi, hizmet tarafından etki alanınız için yüksek kullanılabilirlik etkinleştirildikten sonra, birincisinin hemen ardından görüntülenir. Yüksek kullanılabilirlik etki alanınız için yapılandırıldığında ve etkin olduğunda **Yapılandır** sekmesinin **etki alanı hizmetleri** bölümünde iki IP adresi görmeniz gerekir.
        >
        >
    * Yaklaşık 20 ila 30 dakika sonra, etki alanı hizmetlerinin sanal ağınızda kullanılabilir olduğu ilk IP adresini **Yapılandır** sayfasının **IP adresi** alanında görürsünüz.

        ![Sağlanan ilk IP’yi gösteren Domain Services penceresi](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * Etki alanınız için yüksek kullanılabilirlik uygulandığında sayfada iki IP adresi gösterilir. Yönetilen etki alanınız, bu iki IP adresindeki seçilen sanal ağlarınız üzerinde kullanılabilir. 
    
10. Sanal ağınızın DNS ayarlarını güncelleştirebilmek için bu iki IP adresini not edin. Bunun yapılması, etki alanına katılım gibi işlemler için sanal ağdaki sanal makinelerin etki alanına bağlanmasını sağlar.

    ![Sağlanan her iki IP’yi gösteren Domain Services penceresi](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Azure AD kiracınızın boyutuna (örneğin, kullanıcı veya grup sayısı) bağlı olarak, yönetilen etki alanınızla eşitleme işlemi biraz zaman alır. Bu eşitleme işlemi arka planda gerçekleşir. On binlerce nesne içeren büyük kiracılarda tüm kullanıcıların, grup üyeliklerinin ve kimlik bilgilerinin eşitlenmesi bir veya iki gün sürebilir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Görev 4: [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)

