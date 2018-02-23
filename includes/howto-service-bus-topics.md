## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Burada her ileti tek bir tüketici tarafından işlenir, Service Bus kuyruklarının tersine, konular ve abonelikler publish/subscribe modelini kullanarak iletişimin "bir-çok" biçimini sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir.

Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. Abonelik başına temelinde bir konu için filtre kuralları isteğe bağlı olarak kaydedebilirsiniz. Filtre kuralları, filtre ya da bir konunun hangi iletileri hangi konu abonelikleriyle kısıtlamanızı sağlar.

Service Bus konuları ve Abonelikleri, ölçeklendirme ve birçok kullanıcılar ve uygulamalar arasında çok sayıda iletileri işlemek etkinleştirin.

## <a name="create-a-namespace"></a>Ad alanı oluşturma
Azure'da Service Bus konularını ve aboneliklerini kullanmaya başlamak için öncelikle bir *hizmet ad alanı* oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol gezinti bölmesinde tıklatın **kaynak oluşturma**, ardından **Kurumsal tümleştirme**ve ardından **Service Bus**.
3. **Ad alanı oluştur** iletişim kutusunda bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
4. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel, Standart veya Premium) seçin.
5. **Abonelik** alanında, ad alanı oluşturmak için kullanmak istediğiniz bir Azure aboneliği seçin.
6. İçinde **kaynak grubu** alan, ad içinde bulunan varolan bir kaynak grubu seçin veya yeni bir tane oluşturun.      
7. **Konum** alanında, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin.
   
    ![Ad alanı oluşturma][create-namespace]
8. **Oluştur** düğmesine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.

### <a name="obtain-the-credentials"></a>Kimlik bilgilerini alın
1. Ad alanları listesinde, yeni oluşturulan ad alanı adına tıklayın.
2. İçinde **Service Bus ad alanı** bölmesinde tıklatın **paylaşılan erişim ilkeleri**.
3. İçinde **paylaşılan erişim ilkeleri** bölmesinde tıklatın **RootManageSharedAccessKey**.
   
    ![bağlantı bilgisi][connection-info]
4. İçinde **İlkesi: RootManageSharedAccessKey** bölmesi, kopyalama için İleri düğmesini tıklatın **bağlantı dizesi – birincil anahtarı**, panonuza daha sonra kullanmak için bağlantı dizesini kopyalayın.
   
    ![bağlantı dizesi][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


