1. Portalda **Tüm kaynaklar** menüsündeki **+Ekle**’ye tıklayın. İçinde **her şeyi** dikey penceresinde arama kutusuna **yerel ağ geçidi**, sonra kaynakların bir listesini döndürmek için tıklatın. **Yerel ağ geçidi**’ne tıklayarak dikey pencereyi açın, ardından **Oluştur**’a tıklayarak **Yerel ağ geçidi oluştur** dikey penceresini açın.
   
    ![yerel ağ geçidi oluşturma](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)

2. **Yerel ağ geçidi oluştur** dikey penceresinde yerel ağ geçidi nesnesi için bir **Ad** belirtin. Mümkünse, sezgisel gibi bir şeyi kullanan **ClassicVNetLocal** veya **TestVNet1Local**. Bu Portalı'nda yerel ağ geçidi belirlemenizi kolaylaştırır.
3. Geçerli bir ortak belirtin **IP adresi** bağlanmak istediğiniz VPN cihazı ya da sanal ağ geçidi için.<br>**Bu yerel ağ bir şirket içi konumunu temsil edip etmediğini:** bağlanmak istediğiniz VPN cihazının genel IP adresi belirtin. NAT’nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir.<br>**Bu yerel ağ başka bir VNet temsil ediyorsa:** sanal ağ geçidi, VNet için atanan ortak IP adresi belirtin.<br>**IP adresi henüz yoksa:** geçerli yer tutucu IP adresi yapmak ve daha sonra geri dönün ve bağlanmadan önce bu ayarı değiştirin.
4. **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıklar, bağlandığınız diğer ağlara aralıklarıyla çakışmadığından emin olun.
5. **Abonelik** için doğru aboneliğin gösterildiğini doğrulayın.
6. **Kaynak Grubu** için kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
7. İçin **konumu**, bu kaynak oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.
8. Yerel ağ geçidi oluşturmak için **Oluştur**’a tıklayın.

