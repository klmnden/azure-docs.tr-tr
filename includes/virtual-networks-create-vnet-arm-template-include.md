## <a name="download-and-understand-the-arm-template"></a>ARM şablonunu indirme ve anlama
VNet ve github’a ait iki alt ağı oluşturmak için var olan ARM şablonunu indirebilir, istediğiniz değişiklikleri yapabilir ve yeniden kullanabilirsiniz. Bunun için aşağıdaki adımları uygulayın.

1. [Örnek şablon sayfasına](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets) gidin.
2. **azuredeploy.json** ve **RAW** öğelerine sırayla tıklayın.
3. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
4. ARM şablonları hakkında bilginiz varsa 7. adıma geçin.
5. Henüz kaydetmiş olduğunuz dosyayı açın ve 5. satırdaki **parametreler** altındaki içeriğe bakın. ARM şablonu parametreleri, dağıtım sırasında doldurulabilecek değerler için bir yer tutucu sağlar.
   
   | Parametre | Açıklama |
   | --- | --- |
   | **konum** |VNet’in oluşturulacağı Azure bölgesi |
   | **vnetName** |Yeni VNet'in adı |
   | **addressPrefix** |CIDR biçiminde VNet adres alanı |
   | **subnet1Name** |İlk VNet adı |
   | **subnet1Prefix** |İlk alt ağ için CIDR bloğu |
   | **subnet2Name** |İkinci VNet adı |
   | **subnet2Prefix** |İkinci alt ağ için CIDR bloğu |
   
   > [!IMPORTANT]
   > Github’da tutulan ARM şablonları zamanla değişebilir. Kullanmadan önce şablonu denetlediğinizden emin olun.
   > 
   > 
6. **Kaynaklar** altındaki içeriği denetleyin ve aşağıdakilere dikkat edin:
   
   * **type**. Şablon tarafından oluşturulan kaynak türü. Bu durumda, bir VNet’i temsil eden **Microsoft.Network/virtualNetworks**.
   * **name**. Kaynağın adı. Kullanıcının adı girdi veya dağıtım sırasında bir parametre dosyası olarak vereceği anlamına gelen **[parameters('vnetName')]** öğesinin kullanımına dikkat edin.
   * **properties**. Kaynak özelliklerinin listesi. Bu şablon, VNet oluşturulduğu sırada adres alanını ve alt ağ özelliklerini kullanır.
7. [Örnek şablon sayfasına](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets) geri gidin.
8. **azuredeploy-parameters.json** ve **RAW** öğelerine sırayla tıklayın.
9. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
10. Yeni kaydettiğiniz dosyayı açın ve parametre değerlerini düzenleyin. Senaryomuzda açıklanan VNet’i dağıtmak için aşağıdaki değerleri kullanın.
    
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
11. Dosyayı kaydedin.



<!--HONumber=Nov16_HO2-->


