## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Service Bus kuyruklarının tersine, burada her ileti bir tüketici tarafından işlenir; konular ve abonelikler, publish/subscribe modelini kullanarak iletişimin "bir-çok" biçimini sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir.

Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsterseniz konuyla ilgili filtre kurallarını, hangi konu abonelikleriyle, konunun hangi iletileri alacağını filtrelemenizi veya kısıtlamanızı etkinleştiren abonelik başına temelinde kaydedebilirsiniz.

Service Bus konuları ve abonelikleri, çok sayıda kullanıcıdan ve uygulamadan gelen çok yüksek sayıda iletiyi ölçeklendirmenizi ve işlemenizi sağlar.

## <a name="create-a-namespace"></a>Ad alanı oluşturma
Azure'da Service Bus konularını ve aboneliklerini kullanmaya başlamak için öncelikle bir *hizmet ad alanı* oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol gezinti bölmesinde **Yeni**'ye tıklayın, ardından **Enterprise Integration**'a ve **Service Bus**'a tıklayın.
3. **Ad alanı oluştur** iletişim kutusunda bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
4. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel, Standart veya Premium) seçin.
5. **Abonelik** alanında, ad alanı oluşturmak için kullanmak istediğiniz bir Azure aboneliği seçin.
6. **Kaynak grubu** alanında, ad alanını barındırmak üzere var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturun.      
7. **Konum** alanında, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin.
   
    ![Ad alanı oluşturma][create-namespace]
8. **Oluştur** düğmesine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.

### <a name="obtain-the-credentials"></a>Kimlik bilgilerini alın
1. Ad alanları listesinde, yeni oluşturulan ad alanı adına tıklayın.
2. **Service Bus ad alanı** dikey penceresinde, **Paylaşılan erişim ilkeleri**'ne tıklayın.
3. **Paylaşılan erişim ilkeleri** dikey penceresinde, **RootManageSharedAccessKey** öğesine tıklayın.
   
    ![bağlantı bilgisi][connection-info]
4. **İlke: RootManageSharedAccessKey** dikey penceresinde **Bağlantı dizesi–birincil anahtar** seçeneğinin yanındaki Kopyala düğmesine tıklayın ve bağlantı dizesini, daha sonra kullanmak üzere panonuza kopyalayın.
   
    ![bağlantı dizesi][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png




<!--HONumber=Jan17_HO1-->


