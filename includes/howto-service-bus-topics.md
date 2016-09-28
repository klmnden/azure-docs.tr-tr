## Service Bus konuları ve abonelikleri nelerdir?

Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Service Bus kuyruklarının tersine, burada her ileti bir tüketici tarafından işlenir; konular ve abonelikler, publish/subscribe modelini kullanarak iletişimin "bir-çok" biçimini sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir.

Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsterseniz konuyla ilgili filtre kurallarını, hangi konu abonelikleriyle, konunun hangi iletileri alacağını filtrelemenizi veya kısıtlamanızı etkinleştiren abonelik başına temelinde kaydedebilirsiniz.

Service Bus konuları ve abonelikleri, çok sayıda kullanıcıdan ve uygulamadan gelen çok yüksek sayıda iletiyi ölçeklendirmenizi ve işlemenizi sağlar.

## Ad alanı oluşturma

Azure'da Service Bus konularını ve aboneliklerini kullanmaya başlamak için öncelikle bir *hizmet ad alanı* oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1. [Azure Portal][] oturum açın.

2. Portalın sol gezinti bölmesinde **Yeni**'ye tıklayın, ardından **Enterprise Integration**'a ve **Service Bus**'a tıklayın.

4. **Ad alanı oluştur** iletişim kutusunda bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.

5. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel, Standart veya Premium) seçin.

7. **Abonelik** alanında, ad alanı oluşturmak için kullanmak istediğiniz bir Azure aboneliği seçin.

9. **Kaynak grubu** alanında, ad alanını barındırmak üzere var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturun.      

8. **Konum** alanında, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin.

    ![Ad alanı oluşturma][create-namespace]

6. **Oluştur** düğmesine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.
 
### Kimlik bilgilerini alın

1. Ad alanları listesinde, yeni oluşturulan ad alanı adına tıklayın.
 
3. **Service Bus ad alanı** dikey penceresinde, **Paylaşılan erişim ilkeleri**'ne tıklayın.

4. **Paylaşılan erişim ilkeleri** dikey penceresinde, **RootManageSharedAccessKey** öğesine tıklayın.

    ![bağlantı bilgisi][connection-info]

5. **İlke: RootManageSharedAccessKey** dikey penceresinde **Bağlantı dizesi–birincil anahtar** seçeneğinin yanındaki Kopyala düğmesine tıklayın ve bağlantı dizesini, daha sonra kullanmak üzere panonuza kopyalayın.

    ![bağlantı dizesi][connection-string]

[Azure Portal]: https://portal.azure.com
[ad alanı oluşturma]: ./media/howto-service-bus-topics/create-namespace.png
[bağlantı bilgisi]: ./media/howto-service-bus-topics/connection-info.png
[bağlantı dizesi]: ./media/howto-service-bus-topics/connection-string.png




<!--HONumber=Sep16_HO3-->


