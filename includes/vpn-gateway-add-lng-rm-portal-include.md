1. Portalda **Tüm kaynaklar** menüsündeki **+Ekle**’ye tıklayın.
2. İçinde **her şeyi** sayfa arama kutusuna **yerel ağ geçidi**, sonra kaynakların bir listesini döndürmek için tıklatın. **Yerel ağ geçidi**’ne tıklayarak sayfayı açın, ardından **Oluştur**’a tıklayarak **Yerel ağ geçidi oluştur** sayfasını açın.

  ![yerel ağ geçidi oluşturma](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)
3. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

  - **Ad:** Yerel ağ geçidi nesneniz için bir ad belirtin. Mümkünse, sezgisel gibi bir şeyi kullanan **ClassicVNetLocal** veya **TestVNet1Local**. Bu Portalı'nda yerel ağ geçidi belirlemenizi kolaylaştırır.
  - **IP adresi:** geçerli ortak bir belirtin **IP adresi** bağlanmak istediğiniz VPN cihazı ya da sanal ağ geçidi için.

    * **Bu yerel ağ bir şirket içi konumunu temsil edip etmediğini:** bağlanmak istediğiniz VPN cihazının genel IP adresi belirtin. NAT’nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir.
    * **Bu yerel ağ başka bir VNet temsil ediyorsa:** sanal ağ geçidi, VNet için atanan ortak IP adresi belirtin.
    * **IP adresi henüz yoksa:** geçerli yer tutucu IP adresi yapmak ve daha sonra geri dönün ve bağlanmadan önce bu ayarı değiştirin.
  - **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıklar, bağlandığınız diğer ağlara aralıklarıyla çakışmadığından emin olun.
  - **BGP ayarları yapılandır:** Yalnızca BGP’yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
  - **Abonelik:** Doğru aboneliğin gösterildiğinden emin olun.
  - **Kaynak Grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
  - **Konum:** Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.
4. Yerel ağ geçidi oluşturmak için **Oluştur**’a tıklayın.