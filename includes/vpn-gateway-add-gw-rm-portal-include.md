1. Portalda **Yeni** > **Ağ** > **Sanal ağ geçidi** seçeneğine gidin. Bu işlemin ardından **Sanal ağ geçidi oluştur** dikey penceresi açılır.

    ![Ağ geçidi](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. **Sanal ağ geçidi oluştur** dikey penceresinin **Ad** alanında ağ geçidinize bir ad verin. Bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu, oluşturacağınız ağ geçidi nesnesinin adıdır.

3. **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Bunu yapmazsanız VNet listesinde sanal ağınız görüntülenmez.
 
4. Ardından, bu ağ geçidini eklemek istediğiniz sanal ağı seçin. **Sanal ağ**'a tıklayarak **Sanal ağ seçin** dikey penceresini açın. VNet'i seçin. Listede VNet’in görünmesi için zaten geçerli bir ağ geçidi alt ağı olması gerekir.

5. Genel bir IP adresi seçin. **Genel IP adresi**'ne tıklayarak **Genel IP adresi seçin** dikey penceresini açın. **+Yeni Oluştur**'a tıklayarak **Genel IP adresi oluştur** dikey penceresini açın. Genel IP adresiniz için bir ad girin. Bu işlem sonrasında genel IP adresi nesnesi oluşturulur ve daha sonra bu nesneye dinamik olarak bir genel IP adresi atanır. <br>Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

5. **Ağ geçidi türü** için, yapılandırmanızla ilgili belirtilen Ağ geçidi türünü seçin.

6. **VPN türü** için, yapılandırmanızla ilgili belirtilen VPN türünü seçin.

7. **Abonelik** için doğru aboneliğin seçildiğini doğrulayın.

8. **Kaynak grubu**, seçtiğiniz Sanal Ağ tarafından belirlenir. 

9. Yukarıdaki ayarları belirttikten sonra **Konum**'u ayarlamayın. 

10. Bu noktada dikey pencereniz, 1. adımdaki grafik gibi görünür. Ayarların, yapılandırmanıza ilişkin ayarlarla eşleştiğini doğrulayın. Ağ geçidinizin panoda görünmesini istiyorsanız dikey pencerenin altında yer alan **Panoya sabitle** seçeneğini belirleyebilirsiniz.

11. Ağ geçidi oluşturmaya başlamak için **Oluştur**’a tıklayın. Ayarlar doğrulandıktan sonra, panoda "Sanal ağ geçidi dağıtma" kutucuğunu görürsünüz. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

    ![Ağ geçidi](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Ağ geçidi oluşturulduktan sonra, portalda sanal ağa bakarak bu ağ geçidine atanmış IP adresini görüntüleyebilirsiniz. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Daha fazla bilgi görüntülemek için bağlı cihaza (sanal ağ geçidiniz) tıklayabilirsiniz.





<!--HONumber=Sep16_HO3-->


