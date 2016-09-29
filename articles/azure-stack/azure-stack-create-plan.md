<properties
    pageTitle="Azure Stack'te plan oluşturma| Microsoft Azure"
    description="Hizmet yöneticisi olarak, abonelerin sanal makine sağlamasına olanak tanıyan bir plan oluşturun."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="erikje"/>


# Azure Stack'te plan oluşturma

[Planlar](azure-stack-key-features.md#services-plans-offers-and-subscriptions), bir veya birden fazla hizmetten oluşan gruplardır. Bir sağlayıcı olarak, kiracılarınıza sunabileceğiniz planlar oluşturabilirsiniz. Böylece kiracılarınız tekliflerinize abone olarak, bu tekliflerin içerdiği planları ve hizmetleri kullanabilir. Bu örnekte işlem, ağ ve depolama kaynağı sağlayıcılarını içeren bir plan oluşturursunuz. Bu işlem, abonelere sanal makine sağlayabilme olanağı tanır.

1.  Bir İnternet tarayıcısında https://portal.azurestack.local adresine gidin.

2.  Azure Stack Portal'da hizmet yöneticisi olarak [Oturum açın](azure-stack-connect-azure-stack.md#log-in-as-a-service-administrator) ve hizmet yöneticisi kimlik bilgilerinizi (bu, [PowerShell betiğini çalıştırma](azure-stack-run-powershell-script.md) bölümünün 5. adımında oluşturulan hesaptır) girdikten sonra **Oturum aç** seçeneğine tıklayın.

    Hizmet yöneticileri teklif ve plan oluşturabilir, kullanıcıları yönetebilir.

3.  Kiracıların abone olabileceği bir plan ve teklif oluşturmak için **New** (Yeni) seçeneğine tıklayın.

    ![](media/azure-stack-create-plan/image1.png)

4.  Oluştur dikey penceresinde **Tenant Offers and Plans** (Kiracı Teklifleri ve Planlar) seçeneğine ve ardından**Plan**'a tıklayın.

    ![](media/azure-stack-create-plan/image2.png)

5.  **Display Name** (Görünen Ad) ve **Resource Name** (Kaynak Adı) alanlarını doldurun. Görünen Ad, planın kolay adıdır. Kaynak Adını yalnızca yönetici görebilir. Bu; yöneticilerin, Azure Resource Manager kaynağı olarak planla birlikte çalışmak için kullandığı addır.

    ![](media/azure-stack-create-plan/image3.png)

6.  Plan için kapsayıcı olarak yeni bir **Kaynak Grubu** seçin veya oluşturun. Varsayılan olarak, tüm planlar ve teklifler OffersAndPlans adlı bir kaynak grubunda yer alır.

7.  **Sunulan Hizmetler**'e tıklayın, üç sağlayıcı için de çoklu seçim yapmak (**İşlem Sağlayıcısı**, **Depolama Sağlayıcısı** ve **Ağ Sağlayıcısı**) üzere Shift tuşunu kullanın ve ardından **Seç**'e tıklayın.

    ![](media/azure-stack-create-plan/image4.png)

8.  **Microsoft.Compute**'a ve ardından **Yapılandırma Gerekiyor**'a tıklayın.

    ![](media/azure-stack-create-plan/image5.png)

9.  **Kota Belirle** dikey penceresinde tüm varsayılanları kabul edin, **Tamam**'a ve ardından tekrar **Tamam**'a tıklayın.

    ![](media/azure-stack-create-plan/image6.png)

10. **Microsoft.Network**'e ve ardından **Yapılandırma Gerekiyor**'a tıklayın.

    ![](media/azure-stack-create-plan/image7.png)

11. **Kotaları Belirleyin** dikey penceresinde tüm onay kutularını seçin, **Tamam**'a ve ardından tekrar **Tamam**'a tıklayın.

    ![](media/azure-stack-create-plan/image8.png)

12. **Microsoft.Storage**'a ve **Yapılandırma Gerekiyor**'a tıklayın, ardından **Kotaları Belirleyin** dikey penceresinde tüm varsayılanları kabul edip **Tamam**'a, sonra tekrar **Tamam**'a ve planı oluşturmak için **Oluştur**'a tıklayın.

    ![](media/azure-stack-create-plan/image9.png)

13. Planınız artık bir teklifte yer alabilir. Sağ üst taraftaki zil simgesine tıklayarak bildirimleri görüntüleyebilirsiniz.

    ![](media/azure-stack-create-plan/image10.png)

## Sonraki adımlar

[Teklif oluşturma](azure-stack-create-offer.md)



<!--HONumber=Sep16_HO3-->


