## Tıklayarak dağıtma kullanarak ARM şablonu dağıtma

Microsoft tarafından korunan ve topluluğa açık bir github deposuna yüklenmiş ön tanımlı ARM şablonlarını yeniden kullanabilirsiniz. Bu şablonlar github’dan sınırsız dağıtılabilir veya indirilip gereksinimlerinizi karşılayacak biçimde değiştirilebilir. İki alt ağa sahip VNet oluşturan bir şablonu dağıtmak için aşağıdaki adımları uygulayın.

1. Bir tarayıcıdan [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) adresine gidin.
2. Şablonları listesini kaydırın ve **101-vnet-two-subnets** öğesine tıklayın. Aşağıda gösterilen gibi **README.md** dosyası gözden geçirin.

    ![Github’da READEME.md dosyası](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. **Azure’a dağıt**’a tıklayın. Gerekiyorsa, Azure oturum açma kimlik bilgilerinizi girin. 
4. **Parametreler** dikey penceresinde, yeni VNet oluşturmak için kullanmak istediğiniz değerleri girip **Tamam**’a tıklayın. Aşağıdaki şekil senaryomuzun değerlerini göstermektedir.

    ![ARM şablonu parametreleri](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. **Kaynak grubu**’na tıklayıp VNet'e eklenecek kaynak grubunu seçin veya VNet’i yeni bir kaynak grubuna eklemek için **Yeni oluştur**’a tıklayın. Aşağıdaki şekil, **TestRG** adlı yeni bir kaynak grubunun kaynak grubu ayarlarını göstermektedir.

    ![Kaynak grubu](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Gerekiyorsa, VNet’inizle ilgili **Abonelik** ve **Konum** ayarlarını değiştirin.
6. VNet’i bir kutucuk olarak görmek istemiyorsanız **Başlangıç panosu**’nda **Başlangıç panosuna sabitlemek** öğesini devre dışı bırakın.
5. **Yasal koşullar**’a tıklayın, koşulları okuyun ve kabul etmek için **Satın Al**’a tıklayın. 
6. VNet oluşturmak için **Oluştur**’a tıklayın.

    ![Önizleme portalında dağıtım kutucuğu gönderiliyor](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Dağıtım tamamlandığında, alt ağ özelliklerini görmek için aşağıda gösterildiği gibi **TestVNet** > **Tüm ayarlar** > **Alt ağlar**’a tıklayın.

    ![Önizleme portalında VNet oluşturma](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)


<!--HONumber=Jun16_HO2-->


