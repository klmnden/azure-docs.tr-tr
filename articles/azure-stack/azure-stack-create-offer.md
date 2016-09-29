<properties
    pageTitle="Azure Stack'te teklif oluşturma | Microsoft Azure"
    description="Hizmet yöneticisi olarak Azure Stack'te kiracılarınız için nasıl teklif oluşturacağınızı öğrenin."
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
    ms.date="05/25/2016"
    ms.author="erikje"/>


# Azure Stack'te teklif oluşturma

[Teklifler](azure-stack-key-features.md#services-plans-offers-and-subscriptions), sağlayıcılar tarafından kiracılara satın almaları (abone olmaları) için sunulan bir veya daha fazla plandan oluşan gruplardır. Bu örnekte, son adımda [oluşturduğunuz planı](azure-stack-create-plan.md) içeren bir teklif oluşturacaksınız. Bu teklifin aboneleri sanal makineler sağlayabilir.

1.  Portalda hizmet yöneticisi olarak [oturum açın](azure-stack-connect-azure-stack.md#log-in-as-a-service-administrator).
    ![](media/azure-stack-create-offer/image1.png)

2.  **Yeni**’ye tıklayın.

3.  **Tenant Offers and Plans** (Kiracı Teklifleri ve Planları) seçeneğine ve ardından **Offer** (Teklif) seçeneğine tıklayın.
    ![](media/azure-stack-create-offer/image2.png)

4.  **New Offer** (Yeni Teklif) dikey penceresinde şunları gerçekleştirin:

    1.  **Display Name** (Görünen Ad) ve **Resource Name** (Kaynak Adı) alanlarını doldurun. Görünen Ad, teklifin kolay adıdır. Kaynak Adını yalnızca yönetici görebilir. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

    2.  Yeni veya var olan bir **Kaynak Grubu** seçin.

        ![](media/azure-stack-create-offer/image3.png)

5.  **Base plans** (Temel planlar) seçeneğine tıklayın ve **Plan** dikey penceresinde teklife eklemek istediğiniz planları seçip **Select** (Seç) öğesine tıklayın. Teklifi oluşturmak için **Create** (Oluştur) seçeneğine tıklayın.

    ![](media/azure-stack-create-offer/image4.png)

6.  **Change State** (Durumu Değiştir) öğesine ve **Public** (Genel) seçeneğine tıklayın.
Kiracıların, abone olurken planları ve teklifleri tam olarak görüntüleyebilmeleri için söz konusu plan ve tekliflerin kiracılar için genel durumda olması gerekir. Bir plan durumunun özel, teklif durumunun ise genel olması halinde; kiracılar teklifi alabilir ancak planın ayrıntılarını görüntüleyemez. Plan ve teklif durumları:

    -   **Genel**: Kiracılar görebilir.

    -   **Özel**: Yalnızca hizmet yöneticileri görebilir. Plan veya teklif taslağı oluşturulurken veya hizmet yöneticisi, her aboneliği onaylamak istediğinde faydalıdır.

    -   **Yetkisi Alınmış**: Yeni abonelere kapalıdır. Hizmet yöneticisi, geçerli abonelere müdahale etmeden sonraki abonelikleri engellemek için yetkisi alınmış seçeneğini kullanabilir.

    ![](media/azure-stack-create-offer/image6.png)

Plan veya tekliflerde yapılan değişiklikler, kiracılar tarafından anında görülemez. Değişiklikleri görmek için abonelik durumunun InSync olması ve kiracının portalı yenilemesi veya oturumu kapatıp açması gerekir.

InSync durumunda ek bir abonelik oluşturduktan sonra bile yeni kaynak/kaynak grubu oluştururken "Abonelik seçici" kısmında yeni aboneliği görmek için oturumunuzu kapatıp açmanız gerekebilir.

## Sonraki adımlar

[Bir teklife abone olma ve VM sağlama](azure-stack-subscribe-plan-provision-vm.md)



<!--HONumber=Sep16_HO3-->


