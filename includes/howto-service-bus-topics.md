## Service Bus konuları ve abonelikleri nelerdir?

Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Service Bus kuyruklarının tersine, burada her ileti bir tüketici tarafından işlenir; konular ve abonelikler, publish/subscribe modelini kullanarak iletişimin "bir-çok" biçimini sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir.

Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsterseniz konuyla ilgili filtre kurallarını, hangi konu abonelikleriyle, konunun hangi iletileri alacağını filtrelemenizi veya kısıtlamanızı etkinleştiren abonelik başına temelinde kaydedebilirsiniz.

Service Bus konuları ve abonelikleri, çok sayıda kullanıcıdan ve uygulamadan gelen çok yüksek sayıda iletiyi ölçeklendirmenizi ve işlemenizi sağlar.

## Ad alanı oluşturma

Azure'da Service Bus konularını ve aboneliklerini kullanmaya başlamak için öncelikle bir *hizmet ad alanı* oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1.  [Klasik Azure portalı][]’nda oturum açın.

2.  Portalın sol gezinti bölmesinde **Service Bus** hizmetine tıklayın.

3.  Portalın alt bölmesinde **Oluştur**'a tıklayın.   
    ![][0]

4.  **Yeni bir ad alanı ekle** iletişim kutusunda ad alanı adını girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.   
    ![][2]

5.  Ad alanındaki adın kullanılabilirliğinden emin olduktan sonra, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin (işlem kaynaklarınızın dağıtıldığı aynı ülkeyi/bölgeyi kullandığınızdan emin olun).

    > [AZURE.IMPORTANT] Uygulamanızı dağıtmak için seçmeyi planladığınız **aynı bölgeyi** seçin. Bu en iyi performansı verir.

6.  İletişim kutusundaki diğer alanları varsayılan değerleriyle bırakın (**Mesajlaşma** ve **Standart Katman**), ardından Tamam onay işaretine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika bekleyebilirsiniz.

    ![][6]

## Ad alanı için varsayılan yönetim kimlik bilgilerini alma

Yeni ad alanında konu veya abonelik oluşturma gibi yönetim işlemlerini gerçekleştirmek için ad alanına yönelik yönetici kimlik bilgilerini edinmeniz gerekir. Bu kimlik bilgilerini portaldan elde edebilirsiniz.

### Yönetim kimlik bilgilerini portaldan alın

1.  Kullanılabilir ad alanlarının listesini görüntülemek için sol gezinti bölmesinde **Service Bus** düğümüne tıklayın:   
    ![][0]

2.  Görüntülenen listede az önce oluşturduğunuz ad alanını seçin:   
    ![][3]

3.  **Bağlantı Bilgileri**'ne tıklayın.   
    ![][4]

4.  **Erişim bağlantısı bilgileri** iletişim kutusunda, SAS anahtarını ve anahtar adını içeren bağlantı dizesini bulun. Ad alanıyla işlemleri daha sonra gerçekleştirmek için bu bilgileri kullanacağınızdan bu değerleri not edin. 


  [Klasik Azure portalı]: http://manage.windowsazure.com
  [0]: ./media/howto-service-bus-topics/sb-queues-13.png
  [2]: ./media/howto-service-bus-topics/sb-queues-04.png
  [3]: ./media/howto-service-bus-topics/sb-queues-09.png
  [4]: ./media/howto-service-bus-topics/sb-queues-06.png
  
  [6]: ./media/howto-service-bus-topics/getting-started-multi-tier-27.png




<!--HONumber=Jun16_HO2-->


