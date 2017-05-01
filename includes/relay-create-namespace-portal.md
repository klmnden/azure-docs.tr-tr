1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol gezinti bölmesinde **Yeni**'ye tıklayın, ardından **Enterprise Integration**'a ve **Geçiş**'e tıklayın.
3. **Ad alanı oluştur** iletişim kutusunda bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
4. **Abonelik** alanında, ad alanı oluşturmak için kullanmak istediğiniz bir Azure aboneliği seçin.
5. **[Kaynak grubu](../articles/azure-resource-manager/resource-group-portal.md)** alanında, ad alanını barındırmak üzere var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturun.      
6. **Konum** alanında, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin.
   
    ![ad alanı oluşturma][create-namespace]
7. **Oluştur**’a tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Birkaç dakika sonra sistem, hesabınız için kaynakları sağlar.

### <a name="obtain-the-management-credentials"></a>Yönetim kimlik bilgilerini alma
1. Ad alanları listesinde, yeni oluşturulan ad alanı adına tıklayın.
2. Ad alanı dikey penceresinde, **Paylaşılan erişim ilkeleri**'ne tıklayın.
3. **Paylaşılan erişim ilkeleri** dikey penceresinde, **RootManageSharedAccessKey** öğesine tıklayın.
   
    ![bağlantı bilgisi][connection-info]
4. **İlke: RootManageSharedAccessKey** dikey penceresinde **Bağlantı dizesi–birincil anahtar** seçeneğinin yanındaki Kopyala düğmesine tıklayın ve bağlantı dizesini, daha sonra kullanmak üzere panonuza kopyalayın. Bu değeri Not Defteri veya başka bir geçici konuma yapıştırın.
   
    ![bağlantı dizesi][connection-string]

5. **Birincil anahtar** değerini daha sonra kullanmak üzere geçici bir konuma kopyalayarak önceki adımı tamamlayın.  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
