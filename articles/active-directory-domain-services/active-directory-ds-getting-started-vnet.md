<properties
    pageTitle="Azure AD Etki Alanı Hizmetleri: Sanal ağ oluşturma veya seçme | Microsoft Azure"
    description="Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>


# Azure AD Domain Services için bir sanal ağ oluşturma veya seçme

## Azure sanal ağı seçmeye yönelik yönergeler
> [AZURE.NOTE] **Başlamadan önce**: [Azure AD Domain Services için ağ ile ilgili dikkat edilmesi gerekenler](active-directory-ds-networking.md) sayfasına bakın.


## Görev 2: Azure sanal ağı oluşturma
Sonraki yapılandırma görevi, üzerinde Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek istediğiniz bir Azure sanal ağı oluşturmaktır. Kullanmayı tercih ettiğiniz bir sanal ağınız zaten varsa bu adımı atlayabilirsiniz.

> [AZURE.NOTE] Oluşturduğunuz veya Azure AD Etki Alanı Hizmetleri ile kullanmayı seçtiğiniz Azure sanal ağının, Azure AD Etki Alanı Hizmetleri tarafından desteklenen bir Azure bölgesine ait olduğundan emin olun. Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.

Sanal ağın adını not edin, böylece daha sonraki bir yapılandırma adımında Azure AD Domain Services'i etkinleştirirken doğru sanal ağı seçersiniz.

Azure AD Domain Services'i etkinleştirmek istediğiniz bir Azure sanal ağı oluşturmak için aşağıdaki yapılandırma adımlarını gerçekleştirin.

1. **Klasik Azure portalına** ([https://manage.windowsazure.com](https://manage.windowsazure.com)) gidin.

2. Sol bölmede **Ağlar** düğümünü seçin.

3. Sayfanın altındaki görev bölmesinde **YENİ** seçeneğine tıklayın.

    ![Sanal ağlar düğümü](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. **Ağ Hizmetleri** düğümünde **Virtual Network** seçeneğini belirleyin.

5. Sanal ağ oluşturmak için **Hızlı Oluştur** seçeneğine tıklayın.

    ![Sanal ağ - hızlı oluştur](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Sanal ağınız için bir **Ad** belirtin. Ayrıca bu ağ için **Adres alanını** veya **Maksimum VM sayısını** yapılandırmayı tercih edebilirsiniz. Şu an için DNS sunucusu ayarını "Yok" olarak ayarlanmış şekilde bırakabilirsiniz. Bu ayar, Azure AD Etki Alanı Hizmetleri'ni etkinleştirmenizin ardından güncelleştirilir.

7. **Konum** açılan menüsünde desteklenen bir Azure bölgesini seçtiğinizden emin olun. Azure AD Domain Services'in kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına bakın.

8. Sanal ağınızı oluşturmak için **Sanal Ağ Oluştur** düğmesine tıklayın.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/create-vnet.png)

<br>

## Görev 3 - Azure AD Etki Alanı Hizmetleri'ni etkinleştirme
Sonraki yapılandırma görevi, [Azure AD Etki Alanı Hizmetleri'ni etkinleştirme](active-directory-ds-getting-started-enableaadds.md) olacak.



<!--HONumber=Sep16_HO3-->


