## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Azure portalında klasik bir VNet oluşturma
Yukarıdaki senaryoya dayanan bir Klasik VNet oluşturmak için aşağıdaki adımları izleyin.

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. Tıklatın **yeni** > **ağ** > **sanal ağ**, dikkat **dağıtım modeli seçin** Liste zaten gösterir **Klasik**ve ardından **oluşturma**, aşağıdaki şekilde görüldüğü gibi.
   
    ![Azure portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. Üzerinde **sanal ağ** dikey penceresinde, türü **adı** VNet ve ardından **adres alanı**. VNet ve kendi ilk alt ağ, adres alanı ayarlarını yapılandırın ve ardından **Tamam**. Aşağıdaki şekilde senaryomuz için CIDR bloğu ayarları gösterilmektedir.
   
    ![Adres alanı dikey penceresini](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Tıklatın **kaynak grubu** ve tıklayın veya Vnet'e eklenecek kaynak grubunu seçin **yeni kaynak grubu oluştur** VNet yeni bir kaynak grubuna eklemek için. Aşağıdaki şekil, **TestRG** adlı yeni bir kaynak grubunun kaynak grubu ayarlarını göstermektedir. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)’ı ziyaret edin.
   
    ![Kaynak grubu dikey penceresi oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. Gerekiyorsa, VNet’inizle ilgili **Abonelik** ve **Konum** ayarlarını değiştirin. 
6. VNet’i bir kutucuk olarak görmek istemiyorsanız **Başlangıç panosu**’nda **Başlangıç panosuna sabitlemek** öğesini devre dışı bırakın. 
7. **Oluştur**’a tıklayın ve aşağıdaki şekilde gösterilen **Sanal ağ oluşturuluyor** adlı kutucuğa dikkat edin.
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Oluşturulacak VNet için bekleyin ve döşeme gördüğünüzde, daha fazla alt ağlar eklemek için tıklatın.
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. Görmeniz gerekir **yapılandırma** aşağıda gösterildiği gibi VNet için. 
   
    ![Portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Tıklatın **alt ağlar** > **Ekle**, yazın bir **adı** ve belirtin bir **adres aralığı (CIDR bloğu)** , alt ağ için ve ardından tıklatın **Tamam**. Aşağıdaki şekilde, geçerli senaryomuz için ayarları gösterilmektedir.
    
    ![Azure portalında VNet oluşturma](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

