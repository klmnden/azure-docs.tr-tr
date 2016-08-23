<properties
    pageTitle="Azure AD Etki Alanı Hizmetleri: Sanal ağ oluşturma veya seçme | Microsoft Azure"
    description="Azure Active Directory Etki Alanı Hizmetleri ile (Önizleme) çalışmaya başlama"
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
    ms.date="07/06/2016"
    ms.author="maheshu"/>

# Azure AD Etki Alanı Hizmetleri *(Önizleme)* - Sanal ağ oluşturma veya seçme

## Azure sanal ağı seçmeye yönelik yönergeler
Azure AD Etki Alanı Hizmetleri ile kullanmak üzere bir sanal ağ seçerken aşağıdaki yönergeleri göz önünde bulundurun:

- Azure AD Etki Alanı Hizmetleri tarafından desteklenen bir bölgede bulunan bir sanal ağı seçtiğinizden emin olun. Azure AD Etki Alanı Hizmetleri'nin kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına göz atın.

- Var olan bir sanal ağı kullanmayı planlıyorsanız bunun bölgesel sanal ağ olduğundan emin olun. Eski benzeşim grupları mekanizmasını kullanan sanal ağlar, Azure AD Etki Alanı Hizmetleri ile kullanılamaz. [Eski sanal ağları bölgesel sanal ağlara geçirmeniz](../virtual-network/virtual-networks-migrate-to-regional-vnet.md) gerekir.

- Var olan bir sanal ağı kullanmayı planlıyorsanız sanal ağ için yapılandırılmış hiçbir özel DNS sunucusunun bulunmadığından emin olun. Azure AD Etki Alanı Hizmetleri, özel DNS sunucularını/kendi DNS sunucunu getir olanağını desteklemez.

- Var olan bir sanal ağı kullanmayı planlıyorsanız bu sanal ağ üzerinde aynı etki alanı adına sahip başka bir etki alanınızın olmadığından emin olun. Örneğin, seçilen sanal ağ üzerinde zaten "contoso.com" adında bir etki alanınız olduğunu varsayın. Ardından, bu sanal ağ üzerinde Azure AD Etki Alanı Hizmetleri tarafından yönetilen ve aynı etki alanı adına ("contoso.com") sahip olan bir etki alanını etkinleştirmeyi deniyorsunuz. Azure AD Etki Alanı Hizmetleri'ni etkinleştirmeye çalışırken bir hatayla karşılaşırsınız. Bu, söz konusu sanal ağ üzerindeki etki alanı adı çakışmalarından kaynaklanır. Bu durumda, Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanınızı ayarlamak için farklı bir ad kullanmanız gerekir. Alternatif olarak, var olan etki alanının sağlanmasını kaldırıp Azure AD Etki Alanı Hizmetleri'ni etkinleştirme işlemiyle devam edebilirsiniz.

- Azure AD Etki Alanı Hizmetleri'ne erişmesi gereken sanal makineleri şu anda barındıran/barındıracak olan sanal ağı seçin. Hizmeti etkinleştirdikten sonra, Etki Alanı Hizmetleri'ni farklı bir sanal ağa taşıyamazsınız.

- Azure AD Etki Alanı Hizmetleri, Azure Resource Manager kullanılarak oluşturulan sanal ağlar için desteklenmez. Azure AD Etki Alanı Hizmetleri'ni, Azure Resource Manager kullanılarak oluşturulan bir sanal ağda kullanmak için, [bir klasik sanal ağı ARM tabanlı bir sanal ağa bağlayabilirsiniz](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).


## Görev 2: Azure sanal ağı oluşturma
Sonraki yapılandırma görevi, üzerinde Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek istediğiniz bir Azure sanal ağı oluşturmaktır. Kullanmayı tercih ettiğiniz bir sanal ağınız zaten varsa bu adımı atlayabilirsiniz.

> [AZURE.NOTE] Oluşturduğunuz veya Azure AD Etki Alanı Hizmetleri ile kullanmayı seçtiğiniz Azure sanal ağının, Azure AD Etki Alanı Hizmetleri tarafından desteklenen bir Azure bölgesine ait olduğundan emin olun. Azure AD Etki Alanı Hizmetleri'nin kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına göz atın.

Sonraki bir yapılandırma adımında Azure AD Etki Alanı Hizmetleri etkinleştirilirken doğru sanal ağı seçebilmek için sanal ağın adını not etmeniz gerekir.

Üzerinde Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek istediğiniz bir Azure sanal ağı oluşturmak için sonraki yapılandırma adımlarını uygulayın.

1. **Klasik Azure portalına** ([https://manage.windowsazure.com](https://manage.windowsazure.com)) gidin.

2. Sol bölmede **Ağlar** düğümünü seçin.

3. Sayfanın altındaki görev bölmesinde **YENİ** seçeneğine tıklayın.

    ![Sanal ağlar düğümü](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. **Ağ Hizmetleri** düğümünde **Virtual Network** seçeneğini belirleyin.

5. Sanal ağ oluşturmak için **Hızlı Oluştur** seçeneğine tıklayın.

    ![Sanal ağ - hızlı oluştur](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Sanal ağınız için bir **Ad** belirtin. Ayrıca bu ağ için **Adres alanını** veya **Maksimum VM sayısını** yapılandırmayı tercih edebilirsiniz. Şu an için DNS sunucusu ayarını "Yok" olarak ayarlanmış şekilde bırakabilirsiniz. Bu ayar, Azure AD Etki Alanı Hizmetleri'ni etkinleştirmenizin ardından güncelleştirilir.

7. **Konum** açılan menüsünde desteklenen bir Azure bölgesini seçtiğinizden emin olun. Azure AD Etki Alanı Hizmetleri'nin kullanılabildiği Azure bölgelerini öğrenmek için [bölgeye göre Azure hizmetleri](https://azure.microsoft.com/regions/#services/) sayfasına göz atın. Bu önemli bir adımdır. Azure AD Etki Alanı Hizmetleri tarafından desteklenmeyen bir Azure bölgesindeki bir sanal ağı seçerseniz hizmeti bu sanal ağ üzerinde etkinleştiremezsiniz.

8. Sanal ağınızı oluşturmak için **Sanal Ağ Oluştur** düğmesine tıklayın.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/create-vnet.png)

<br>

## Görev 3 - Azure AD Etki Alanı Hizmetleri'ni etkinleştirme
Sonraki yapılandırma görevi, [Azure AD Etki Alanı Hizmetleri'ni etkinleştirme](active-directory-ds-getting-started-enableaadds.md) olacak.



<!--HONumber=Aug16_HO1-->


