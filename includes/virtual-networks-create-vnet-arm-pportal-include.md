## Azure portalında VNet oluşturma
Azure Preview portalını kullanarak yukarıdaki senaryoya dayanan bir VNet oluşturmak için aşağıdaki adımları uygulayın.

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. Aşağıdaki şekilde görüldüğü gibi **YENİ** > **Ağ** > **Sanal ağ**’a, sonra da **Bir dağıtım modeli seçin** listesinden **Resource Manager**’a, ardından **Oluştur**’a tıklayın.
   
    ![Azure portalında VNet oluşturma](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)
3. **Sanal ağ oluştur** dikey penceresinde, aşağıdaki şekilde gösterildiği gibi VNet ayarlarını yapılandırın.
   
    ![Sanal ağ oluştur dikey penceresi](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)
4. **Kaynak grubu**’na tıklayıp VNet'e eklenecek kaynak grubunu seçin veya VNet’i yeni bir kaynak grubuna eklemek için **Yeni oluştur**’a tıklayın. Aşağıdaki şekil, **TestRG** adlı yeni bir kaynak grubunun kaynak grubu ayarlarını göstermektedir. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/resource-group-overview.md#resource-groups) sayfasını ziyaret edin.
   
    ![Kaynak grubu](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)
5. Gerekiyorsa, VNet’inizle ilgili **Abonelik** ve **Konum** ayarlarını değiştirin. 
6. VNet’i bir kutucuk olarak görmek istemiyorsanız **Başlangıç panosu**’nda **Başlangıç panosuna sabitlemek** öğesini devre dışı bırakın. 
7. **Oluştur**’a tıklayın ve aşağıdaki şekilde gösterilen **Sanal ağ oluşturuluyor** adlı kutucuğa dikkat edin.
   
    ![Sanal ağ kutucuğu oluşturma](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)
8. Oluşturulacak VNet’i bekleyin, sonra da **Virtual Network** dikey penceresinde **Tüm ayarlar** > **Alt ağlar** > **Ekle**’ye aşağıda görüldüğü gibi tıklayın.
   
    ![Azure portalında alt ağ ekleme](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)
9. *BackEnd* alt ağıyla ilgili alt ağ ayarlarını aşağıda gösterildiği gibi belirtip **Tamam**’a tıklayın. 
   
    ![Alt ağ ayarları](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)
10. Aşağıdaki şekilde örneği gösterilen alt ağlar listesini inceleyin.
    
    ![VNet’te alt ağlar listesi](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)

<!--HONumber=Sep16_HO3-->


