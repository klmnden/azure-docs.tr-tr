---
title: Azure portalını kullanarak VNet Eşlemesi oluşturma | Microsoft Docs
description: Resource Manager’da Azure portalını kullanarak bir sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
tags: azure-resource-manager

ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai;annahar

---
# Azure portalını kullanarak bir sanal ağ eşlemesi oluşturma
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Azure portalını kullanarak yukarıdaki senaryoya dayanan bir VNet eşlemesi oluşturmak için aşağıdaki adımları uygulayın.

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. VNET eşlemesi oluşturmak için iki VNet arasında her biri bir yönde olan iki bağlantı oluşturmanız gerekir. İlk olarak VNET1’den VNET2’ye VNET eşleme bağlantısı oluşturabilirsiniz. Portal'da **Gözat** > **Sanal ağları seçin** öğesine tıklayın
   
    ![Azure portalında VNet eşlemesi oluşturma](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)
3. Sanal Ağlar dikey penceresinde VNET1’i seçin, Eşlemeler’e ve ardından Ekle’ye tıklayın
   
    ![Eşleme seçme](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)
4. Eşleme Ekle dikey penceresinde LinkToVnet2 gibi bir eşleme bağlantısı adı verin, aboneliği ve eş Sanal Ağ VNET2’yi seçin, Tamam’a tıklayın.
   
    ![VNet bağlantısı](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)
5. Bu VNET eşleme bağlantısı oluşturulduktan sonra. Bağlantı durumunu aşağıdaki gibi görebilirsiniz:
   
    ![Bağlantı Durumu](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)
6. Ardından VNET2’den VNET1’e VNET eşleme bağlantısı oluşturun. Sanal Ağlar dikey penceresinde VNET2’yi seçin, Eşlemeler’e ve ardından Ekle’ye tıklayın
   
    ![Diğer VNet’ten eşleme](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)
7. Eşleme Ekle dikey penceresinde LinkToVnet1 gibi bir eşleme bağlantısı adı verin, aboneliği ve eş Sanal Ağ’ı seçin, Tamam’a tıklayın.
   
    ![Sanal ağ kutucuğu oluşturma](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)
8. Bu VNET eşleme bağlantısı oluşturulduktan sonra. Bağlantı durumunu aşağıdaki gibi görebilirsiniz:
   
    ![Son bağlantı durumu](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)
9. LinkToVnet2’nin durumunu denetleyin ve Bağlı duruma geldiğinden emin olun.  
   
    ![Son bağlantı durumu 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)
   
   > [!NOTE]
   > VNET eşlemesi yalnızca her iki bağlantı da kurulduğunda oluşturulur.
   > 
   > 

Her bağlantı için birkaç özellik yapılandırılabilir:

| Seçenek | Açıklama | Varsayılan |
|:--- |:--- |:--- |
| AllowVirtualNetworkAccess |Eş VNet’in adres alanının Virtual_network Etiketine eklenip eklenmeyeceği |Evet |
| AllowForwardedTraffic |Eşlenen VNet’ten kaynaklanmayan trafiğin kabul edilmesine veya bırakılmasına izin verir |Hayır |
| AllowGatewayTransit |Eş VNet’in VNet ağ geçidinizi kullanmasına izin verir |Hayır |
| UseRemoteGateways |Eşinizin VNet ağ geçidini kullanır. Eş VNet için yapılandırılmış bir ağ geçidi olmalı ve AllowGatewayTransit seçilmelidir. Yapılandırılmış bir ağ geçidiniz varsa bu seçeneği kullanamazsınız |Hayır |

VNet eşlemesindeki her bağlantı yukarıdaki özellikleri içerir. Portaldan VNet Eşleme Bağlantısına tıklayabilir ve kullanılabilir tüm seçenekleri değiştirebilirsiniz; değişikliğin uygulanması için Kaydet’e tıklayın.

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. Bu örnekte A ve B adlı iki abonelik ile aboneliklerdeki ilgili ayrıcalıklara sahip UserA ve UserB adlı iki kullanıcı kullanılacaktır
3. Portal'da Gözat’a tıklayın ve Sanal Ağlar’ı seçin. VNET’e ve Ekle'ye tıklayın.
   
    ![Senaryo 2 Gözat](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)
4. Erişim ekle dikey penceresinde Rol seçin’e tıklayın ve Ağ Katılımcısı’nı seçin, Kullanıcı Ekle’ye tıklayın, UserB oturum açma adını yazın ve Tamam’a tıklayın.
   
    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)
   
    Bu bir gereklilik değildir; istekler eşleştiği sürece kullanıcılar ilgili sanal ağları için eşleme isteklerini ayrı ayrı gönderse bile eşleme kurulabilir. Diğer VNet’in ayrıcalıklı kullanıcısının yerel VNet’e kullanıcı olarak eklenmesi portalda kurulum yapmayı kolaylaştırır.
5. Ardından Azure portalında SubscriptionB’nin ayrıcalıklı kullanıcısı olan UserB ile oturum açın. UserA’yı Ağ Katılımcısı olarak eklemek için yukarıdaki adımları izleyin.
   
    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)
   
   > [!NOTE]
   > Yetkilendirmenin başarıyla etkinleştirildiğinden emin olmak için tarayıcıda her iki kullanıcı oturumunu açıp kapatabilirsiniz.
   > 
   > 
6. Portalda UserA olarak oturum açın, VNET3 dikey penceresine gidin, Eşleme’ye tıklayın, ‘Kaynak kimliğimi biliyorum” onay kutusunu işaretleyin ve VNET5 kaynak kimliğini aşağıdaki biçimde yazın.
   
    /subscriptions/<Subscription- ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/<VNET name>
   
    ![Kaynak kimliği](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)
7. Portalda UserB olarak oturum açın ve VNet5 ile VNet3 arasında bir eşleme bağlantısı oluşturmak için yukarıdaki adımı izleyin.
   
    ![Kaynak kimliği 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)
8. Eşleme kurulur ve VNet3’teki tüm sanal makineler VNet5’teki tüm sanal makinelerle iletişim kurabilir

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. İlk adım olarak VNET eşlemesi HubVnet’i VNET1’e bağlar. Bağlantı için İletilen Trafiğe İzin Ver seçeneği belirlenmez.
   
    ![Temel Eşleme](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)
2. Sonraki adım olarak, VNET1’den HubVnet’e eşleme bağlantıları oluşturulabilir. İletilen trafiğe izin ver seçeneği belirlenir.
   
    ![Temel Eşleme](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)
3. Eşleme kurulduktan sonra bu [makaleye](virtual-network-create-udr-arm-ps.md) bakabilir ve Kullanıcı Tanımlı Yönlendirme’yi (UDR) tanımlayarak, özelliklerini kullanmak üzere bir sanal gereç aracılığıyla VNet1 trafiğini yeniden yönlendirebilirsiniz. Yolda Sonraki Atlama adresini belirttiğinizde eş VNet HubVNet içindeki sanal gerecin IP adresine ayarlayabilirsiniz

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. Bu senaryoda VNET eşlemesi oluşturmak için Azure resource manager ile klasikteki sanal ağ arasında yalnızca bir bağlantı oluşturmanız gerekir. Diğer bir deyişle, **VNET1** ile **VNET2** arasında. Portalda **Gözat**’a tıklayın ve **Sanal Ağlar**’ı seçin
3. Sanal ağlar dikey penceresinde **VNET1**’i seçin. **Eşlemeler**’e ve ardından **Ekle**’ye tıklayın.
4. Eşleme Ekle dikey penceresinde bağlantınızı adlandırın. Burada **LinkToVNet2** olarak adlandırılır. Eşleme ayrıntıları altında **Klasik**’i seçin.
5. Ardından aboneliği ve eş Sanal Ağ **VNET2**’yi seçin. Daha sonra Tamam'a tıklayın.
   
    ![Vnet1’i Vnet 2'ye bağlama](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)
6. Bu VNet eşleme bağlantısı oluşturulduktan sonra iki sanal ağ eşlenir ve aşağıdakileri görebilirsiniz:
   
    ![Eşleme bağlantısını denetleme](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)

## VNet Eşlemesini Kaldırma
1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. Sanal ağ dikey penceresine gidin, Eşlemeler’e tıklayın, kaldırmak istediğiniz bağlantıya ve Sil düğmesine tıklayın.
   
   ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)
3. VNET eşlemesindeki bir bağlantıyı kaldırdıktan sonra eşleme bağlantı durumu bağlantı kesildi olarak değişir.
   
    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)
4. Bu durumda eşleme bağlantı durumu Başlatıldı olana kadar bağlantıyı yeniden oluşturamazsınız. VNET eşlemesini yeniden oluşturmadan önce her iki bağlantıyı da kaldırmanız önerilir.

<!--HONumber=Sep16_HO3-->


