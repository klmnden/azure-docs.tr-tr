Azure'da Service Bus mesajlaşma varlıklarını kullanmaya başlamak için öncelikle Azure'da benzersiz olan bir ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol gezinti bölmesinde tıklatın **+ kaynak oluşturma**, ardından **Kurumsal tümleştirme**ve ardından **Service Bus**.
3. **Ad alanı oluştur** iletişim kutusunda bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
4. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel, Standart veya Premium) seçin.
5. **Abonelik** alanında, ad alanı oluşturmak için kullanmak istediğiniz bir Azure aboneliği seçin.
6. **Kaynak grubu** alanında, ad alanını barındırmak üzere var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturun.      
7. **Konum** alanında, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin.
   
    ![ad alanı oluşturma][create-namespace]
8. **Oluştur**’a tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.

### <a name="obtain-the-management-credentials"></a>Yönetim kimlik bilgilerini alma
Yeni bir ad alanı otomatik olarak oluşturma, her ad alanı tüm yönleri üzerinde tam denetim izni verin birincil ve ikincil anahtarları ilişkili bir çifti ile bir ilk paylaşılan erişim imzası (SAS) kural oluşturur. Bkz: [Service Bus kimlik doğrulama ve yetkilendirme](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) ile daha fazla başka kuralları oluşturma hakkında bilgi için kısıtlı haklar normal göndericiler ile alıcılar için. İlk kuralı kopyalamak için aşağıdaki adımları takip edin: 

1.  Tıklatın **tüm kaynakları**, yeni oluşturulan ad alanı adına tıklayın.
2. Ad alanı penceresinde **paylaşılan erişim ilkeleri**.
3. İçinde **paylaşılan erişim ilkeleri** ekranında **RootManageSharedAccessKey**.
   
    ![bağlantı bilgisi][connection-info]
4. İçinde **İlkesi: RootManageSharedAccessKey** penceresinde, kopyalama için İleri düğmesini tıklatın **bağlantı dizesi – birincil anahtarı**, panonuza daha sonra kullanmak için bağlantı dizesini kopyalayın. Bu değeri Not Defteri veya başka bir geçici konuma yapıştırın.
   
    ![bağlantı dizesi][connection-string]

5. **Birincil anahtar** değerini daha sonra kullanmak üzere geçici bir konuma kopyalayarak önceki adımı tamamlayın.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
