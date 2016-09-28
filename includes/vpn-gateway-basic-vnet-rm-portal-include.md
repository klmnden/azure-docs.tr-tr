Azure portalını kullanarak VNet oluşturmak için aşağıdaki adımları uygulayın. Ekran görüntülerinin örnek olarak verildiğini unutmayın. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Virtual Network’e Genel Bakış](../articles/virtual-network/virtual-networks-overview.md).

1. Tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızla giriş yapın.

2. **Yeni** **>** **Ağ** **>** **Sanal Ağ**’a tıklayın.

    ![VNetBlade](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal650.png)

3. Virtual Network dikey penceresinin altı yakınlarında, **Bir dağıtım modeli seçin** listesinden **Resource Manager**’ı seçip **Oluştur**’a tıklayın.


    ![Resource Manager’ı seçin](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png)

4. **Sanal ağ oluştur** dikey penceresinde VNet ayarlarını yapılandırın. Bu dikey pencerede, ilk adres alanınızı ve tek alt ağ adres aralığınızı eklersiniz. VNet oluşturma işlemini tamamladıktan sonra geri dönüp ek alt ağları ve adres alanlarını ekleyin. Bu, portalın geçerli sınırlamasıdır. Portalda VNet özelliklerini düzenleyerek veya PowerShell kullanarak her zaman bu değerleri güncelleştirmek için geri dönebilirsiniz. Kullandığınız değerler oluşturmak istediğiniz yapılandırmaya bağlıdır. Planlanmış yapılandırma değerlerinize başvurulduğundan emin olun. 

    ![Sanal ağ oluştur dikey penceresi](./media/vpn-gateway-basic-vnet-rm-portal-include/createavnet250.png)

5. **Abonelik** alanında doğru bir giriş olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.

6. **Kaynak grubu**’na tıklayın, ya varolan bir kaynak grubunu seçin ya da yeni kaynak grubunuz için bir ad yazarak yeni bir tane oluşturun. Yeni bir grup oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](resource-group-overview.md#resource-groups)’ı ziyaret edin.

7. Ardından, VNet’iniz için **Konum** ayarlarını seçin. Bu VNet'e dağıttığınız kaynakların nerede olacağını konumun saptayacağını unutmayın. Bunu daha sonra, kaynaklarınızı yeniden dağıtmadan değiştiremezsiniz.

8. VNet’inizi panoda kolay bulmak istiyorsanız **Panoya sabitle**’yi seçin ve ardından **Oluştur**’a tıklayın.
    
    ![Panoya sabitle](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png)


9. Oluştur’a tıkladıktan sonra, panonuzda VNet’inizin ilerleme durumunu yansıtacak bir kutucuk göreceksiniz. VNet oluşturulduğu sürece kutucuk da değişecektir.

    ![Sanal ağ kutucuğu oluşturma](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png)

<!--HONumber=Sep16_HO3-->


