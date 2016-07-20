1. Azure Portal'da **Yeni** **>** **Ağ** **>** **Yerel ağ geçidi**’ne gidin.

    ![yerel ağ geçidi oluşturma](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. **Yerel ağ geçidi oluştur** dikey penceresinde yerel ağ geçidi nesnesi için bir **Ad** belirtin.
 
3. Ağ geçidiniz için bir **IP adresi** belirtin. Bu, bağlanmak istediğiniz dış VPN cihazının IP adresidir. NAT’nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir.

4. **Adres Alanı** yerel (genellikle şirket içi) ağınızdaki adres aralığına başvurur. Birden fazla adres alanı aralığı ekleyebilirsiniz. Buraya girdiğiniz aralıklar, ağ geçidiyle iletişim kuracak sanal ağlar için kullandığınız adres alanı aralıklarıyla çakışamaz.  Azure Virtual Network adres alanları kadar şirket içi yapılandırmanızla da koordine olmanız gerekecek.
 
5. **Abonelik** için doğru aboneliğin gösterildiğini doğrulayın.

6. **Kaynak Grubu** için kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz. Yeni bir kaynak grubu oluşturmak için kutuya adı yazın. Önceden oluşturduğunuz bir kaynak grubunu seçmek için **Kaynak grubu** dikey penceresini açacak **Kaynak Grubu**’na tıklayın ve kullanmak istediğiniz kaynak grubunu seçin.

7. **Konum** için, yeni bir yerel ağ geçidi oluşturuyorsanız sanal ağ geçidiyle aynı konumu kullanabilirsiniz. Ancak bu zorunlu değildir. Yerel ağ geçidi farklı bir konumda olabilir. 

8. Bu yerel ağ geçidini panodan kolayca bulmak istiyorsanız “Panoya sabitle” seçimini seçili bırakın.

9. Yerel ağ geçidi oluşturmak için **Oluştur**’a tıklayın. Panonuzda "Yerel ağ geçidi dağıtılıyor" öğesini görürsünüz.

10. Yerel ağ geçidi oluşturulduğunda görmeniz için portalda açılır.

    



<!--HONumber=Jun16_HO2-->


