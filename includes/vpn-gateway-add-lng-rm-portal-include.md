1. Portalda **Tüm kaynaklar** menüsündeki **+Ekle**’ye tıklayın. **Her şey** dikey penceresi arama kutusuna **Yerel ağ geçidi** yazın, ardından aramak için tıklayın. Bunun yapılması bir liste döndürür. **Yerel ağ geçidi**’ne tıklayarak dikey pencereyi açın, ardından **Oluştur**’a tıklayarak **Yerel ağ geçidi oluştur** dikey penceresini açın.
   
    ![yerel ağ geçidi oluşturma](./media/vpn-gateway-add-lng-rm-portal-include/newlng.png)

2. **Yerel ağ geçidi oluştur** dikey penceresinde yerel ağ geçidi nesnesi için bir **Ad** belirtin.
3. Bağlanmak istediğiniz VPN cihazı veya sanal ağ geçidi için geçerli bir genel **IP adresi** belirtin.<br>Söz konusu yerel ağ, şirket içi bir konumu temsil ediyorsa; bu, bağlanmak istediğiniz VPN cihazının genel IP adresidir. NAT’nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir.<br>Bu yerel ağ başka bir VNet'i temsil ediyorsa, o VNet için sanal ağ geçidine atanan genel IP adresini belirtin.<br>
4. **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun.
5. **Abonelik** için doğru aboneliğin gösterildiğini doğrulayın.
6. **Kaynak Grubu** için kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
7. **Konum** için, bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.
8. Yerel ağ geçidi oluşturmak için **Oluştur**’a tıklayın.



<!--HONumber=Jan17_HO3-->


