---
title: 'Azure AD Etki Alanı Hizmetleri: Azure AD Etki Alanı Hizmetleri''ni Etkinleştirme | Microsoft Docs'
description: Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand

ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/21/2016
ms.author: maheshu

---
# Azure AD Etki Alanı Hizmetleri'ni etkinleştirme
## Görev 3: Azure AD Etki Alanı Hizmetleri'ni etkinleştirme
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
   * Aynı zamanda özel bir etki alanı adını bu listeye yazarak ekleyebilirsiniz. Bu örnekte, özel bir etki alanı adı olarak 'contoso100.com' yazdık
     
     > [!WARNING]
     > Belirttiğiniz etki alanı adının etki alanı ön ekinin (örneğin, 'contoso100.com' etki alanı adında 'contoso100') 15 karakterden kısa olduğundan emin olun. Etki alanı ön eki 15 karakterden daha uzun olan bir Azure AD Etki Alanı Hizmetleri etki alanını oluşturamazsınız.
     > 
     > 
8. Bir sonraki adım, Azure AD Etki Alanı Hizmetleri'nin kullanılabilir olmasını istediğiniz bir sanal ağ seçmektir. **Etki alanı hizmetlerini bu sanal ağa bağlayın** açılan menüsünde oluşturduğunuz sanal ağı seçin.
   
   * Belirttiğiniz sanal ağın Azure AD Etki Alanı Hizmetleri tarafından desteklenen bir Azure bölgesine ait olduğundan emin olun.
   * Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.
   * Azure AD Etki Alanı Hizmetleri'nin desteklenmediği bir bölgeye ait olan sanal ağlar açılan listede görüntülenmez.
   * Benzer şekilde, Azure Resource Manager kullanılarak oluşturulan sanal ağlar açılan listede görünmez. Resource Manager tabanlı sanal ağlar şu anda Azure AD Etki Alanı Hizmetleri'nde desteklenmemektedir.
9. Yönetilen etki alanı için seçtiğiniz DNS etki alanı adının sanal ağda zaten bulunmadığından emin olun. Özellikle aşağıdakileri kontrol edin:
   
   * sanal ağda aynı DNS etki alanı adına sahip bir etki alanınız zaten varsa.
   * seçtiğiniz sanal ağın şirket içi ağınıza bir VPN bağlantısı bulunuyorsa ve şirket içi ağınızda aynı DNS etki alanı adına sahip bir etki alanınız zaten varsa.
   * sanal ağda bu ada sahip bir bulut hizmetiniz zaten varsa.
10. Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek için sayfanın altındaki görev bölmesinde **Kaydet**'e tıklayın.
11. Sayfada ‘Bekliyor…’ iletisi görüntülenir durumu görüntülenir.
    
    ![Etki Alanı Hizmetleri'ni Etkinleştirme - beklemede durumu](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)
    
    > [!NOTE]
    > Azure AD Etki Alanı Hizmetleri, yönetilen etki alanınız için yüksek kullanılabilirlik sağlar. Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sonra Etki Alanı Hizmetleri'nin sanal ağda kullanılabilir olduğu IP adreslerinin tek tek görüntülendiğine dikkat edin. İkinci IP adresi kısa bir süre içinde, hizmet etki alanınız için yüksek kullanılabilirliği etkinleştirdiği zaman görüntülenir. Yüksek kullanılabilirlik etki alanınız için yapılandırıldığında ve etkin olduğunda **Yapılandır** sekmesinin **etki alanı hizmetleri** bölümünde iki IP adresi görmeniz gerekir.
    > 
    > 
12. Yaklaşık 20-30 dakika sonra, Etki Alanı Hizmetleri'nin sanal ağınızda kullanılabilir olduğu ilk IP adresini **Yapılandır** sayfasının **IP adresi** alanında görürsünüz.
    
    ![Etki Alanı Hizmetleri etkin - ilk IP sağlanmış](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
13. Etki alanınız için yüksek kullanılabilirlik uygulandığında sayfada iki IP adresinin görüntülendiğini görürsünüz. Bunlar, Azure AD Etki Alanı Hizmetleri'nin seçtiğiniz sanal ağınızda kullanılabilir olduğu IP adresleridir. Sanal ağınızın DNS ayarlarını güncelleştirebilmek için bu IP adreslerini not edin. Bu adım, etki alanına katılım gibi işlemler için sanal ağdaki sanal makinelerin etki alanına bağlanmasını sağlar.
    
    ![Etki Alanı Hizmetleri etkin - her iki IP de sağlanmış](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Azure AD kiracınızın boyutuna bağlı olarak (kullanıcıların, grupların vb. sayısı) kiracı içeriklerinin Azure AD Etki Alanı Hizmetleri'nde kullanılabilir olması biraz zaman alabilir. Bu eşitleme işlemi arka planda gerçekleşir. On binlerce nesne içeren büyük kiracılar için tüm kullanıcıların, grup üyeliklerinin ve kimlik bilgilerinin Azure AD Etki Alanı Hizmetleri'nde kullanılabilir olması bir veya iki gün sürebilir.
> 
> 

<br>

## Görev 4 - Azure sanal ağı için DNS ayarlarını güncelleştirme
Sonraki yapılandırma görevi [Azure sanal ağı için DNS ayarlarını güncelleştirmedir](active-directory-ds-getting-started-dns.md).

<!--HONumber=Sep16_HO4-->


