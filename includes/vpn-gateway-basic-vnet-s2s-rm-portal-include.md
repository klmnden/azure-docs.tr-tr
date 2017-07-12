Azure portalını kullanarak Resource Manager dağıtımında bir VNet oluşturmak için aşağıdaki adımları izleyin. Bu adımları öğretici olarak uyguluyorsanız [örnek değerleri](#values) kullanın. Bu adımları öğretici olarak uygulamıyorsanız değerleri kendi değerlerinizle değiştirmeyi unutmayın. Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Virtual Network’e Genel Bakış](../articles/virtual-network/virtual-networks-overview.md).

1. Bir tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve Azure hesabınızla oturum açın.
2. **Yeni**’ye tıklayın. **Markette ara** alanına 'Sanal Ağ' yazın. Döndürülen listeden **Sanal Ağ**’ı bulun ve tıklayarak **Sanal Ağ** dikey penceresini açın.
3. Virtual Network dikey penceresinin altı yakınlarında, **Bir dağıtım modeli seçin** listesinden **Resource Manager**’ı seçip **Oluştur**’a tıklayın. Bu işlem "Sanal ağ geçidi oluştur" dikey penceresini açar.

    ![Sanal ağ oluşturma dikey penceresi](./media/vpn-gateway-basic-vnet-s2s-rm-portal-include/createvnet.png "Create virtual network blade")
4. **Sanal ağ oluştur** dikey penceresinde sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür.

  - **Ad**: Sanal ağınızın adını girin. Bu örnekte TestVNet1 kullandık.
  - **Adres alanı**: Adres alanını girin. Eklenecek birden fazla adres alanınız varsa birinci adres alanınızı ekleyin. Sanal ağı oluşturduktan sonra başka adres alanları ekleyebilirsiniz. Belirttiğiniz adres alanının, şirket içi konumunuzdaki adres alanıyla çakışmadığından emin olun.
  - **Alt ağ adı**: İlk alt ağ adını ve alt ağ adres aralığını ekleyin. Bu VNet'i oluşturduktan sonra ağ geçidi alt ağı ve başka alt ağlar ekleyebilirsiniz. 
  - **Abonelik**: Listelenen aboneliğin doğru olduğunu onaylayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
  - **Kaynak grubu**: Var olan bir kaynak grubunu seçin ya da yeni kaynak grubunuz için bir ad yazarak yeni bir tane oluşturun. Yeni bir grup oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) sayfasını ziyaret edin.
  - **Konum**: Sanal ağınızın konumunu seçin. Konum bu sanal ağa dağıttığınız kaynakların nerede olacağını belirler.

5. Sanal ağınızı panoda kolay bulmak istiyorsanız **Panoya sabitle**’yi seçin ve ardından **Oluştur**’a tıklayın. **Oluştur**’a tıkladıktan sonra panonuzda sanal ağınızın ilerleme durumunu yansıtacak bir kutucuk göreceksiniz. Sanal ağ oluşturulurken kutucuk değişir.