1. [Azure Portal][] oturum açın.

2. Portalın sol gezinti bölmesinde **Yeni**'ye tıklayın, ardından **Enterprise Integration**'a ve **Service Bus**'a tıklayın.

4. **Ad alanı oluştur** iletişim kutusunda bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.

5. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel, Standart veya Premium) seçin.

7. **Abonelik** alanında, ad alanı oluşturmak için kullanmak istediğiniz bir Azure aboneliği seçin.

9. **Kaynak grubu** alanında, ad alanını barındırmak üzere var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturun.      

8. **Konum** alanında, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin.

    ![Ad alanı oluşturma][create-namespace]

6. **Oluştur** düğmesine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.
 
### Yönetim kimlik bilgilerini alma

1. Ad alanları listesinde, yeni oluşturulan ad alanı adına tıklayın.
 
3. **Service Bus ad alanı** dikey penceresinde, **Paylaşılan erişim ilkeleri**'ne tıklayın.

4. **Paylaşılan erişim ilkeleri** dikey penceresinde, **RootManageSharedAccessKey** öğesine tıklayın.

    ![bağlantı bilgisi][connection-info]

5. **İlke: RootManageSharedAccessKey** dikey penceresinde **Bağlantı dizesi–birincil anahtar** seçeneğinin yanındaki Kopyala düğmesine tıklayın ve bağlantı dizesini, daha sonra kullanmak üzere panonuza kopyalayın.

    ![bağlantı dizesi][connection-string]

<!--Image references-->

[ad alanı oluşturma]: ./media/service-bus-create-namespace-portal/create-namespace.png
[bağlantı bilgisi]: ./media/service-bus-create-namespace-portal/connection-info.png
[bağlantı dizesi]: ./media/service-bus-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Azure Portal]: https://portal.azure.com


<!--HONumber=Aug16_HO4-->


