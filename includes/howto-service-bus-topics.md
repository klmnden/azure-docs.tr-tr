---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: ef6d5d22f70d5fff38f90b457a7c945ab59fc67c
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52331567"
---
## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Farklı olarak burada her ileti tek bir tüketici tarafından işlenir, Service Bus kuyrukları, konuları ve abonelikleri bir yayımlama/abone olma modelini kullanarak iletişimin "bir-çok" biçiminin sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir.

Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsteğe bağlı olarak, bir konu başına abonelik temelinde filtre kuralları da kaydedebilirsiniz. Filtre kuralları filtreleyin veya hangi konu aboneliklerinin bir konuya hangi mesajları kısıtlayabilirsiniz olanak tanır.

Service Bus konuları ve Abonelikleri, ölçeklendirme ve çok sayıda kullanıcıdan ve uygulamadan çok sayıda iletileri işlemek etkinleştirin.

## <a name="create-a-namespace"></a>Ad alanı oluşturma
Azure'da Service Bus konularını ve aboneliklerini kullanmaya başlamak için öncelikle bir *hizmet ad alanı* oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol gezinti bölmesinde tıklayın **kaynak Oluştur**, ardından **Kurumsal tümleştirme**ve ardından **Service Bus**.
3. **Ad alanı oluştur** iletişim kutusunda bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
4. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel, Standart veya Premium) seçin.
5. **Abonelik** alanında, ad alanı oluşturmak için kullanmak istediğiniz bir Azure aboneliği seçin.
6. İçinde **kaynak grubu** alan, ad alanı içinde yer alan mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun.      
7. **Konum** alanında, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin.
   
    ![Ad alanı oluşturma][create-namespace]
8. **Oluştur** düğmesine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.

### <a name="obtain-the-credentials"></a>Kimlik bilgilerini alın
1. Ad alanları listesinde, yeni oluşturulan ad alanı adına tıklayın.
2. İçinde **Service Bus ad alanı** bölmesinde tıklayın **paylaşılan erişim ilkeleri**.
3. İçinde **paylaşılan erişim ilkeleri** bölmesinde tıklayın **RootManageSharedAccessKey**.
   
    ![bağlantı bilgisi][connection-info]
4. İçinde **ilke: RootManageSharedAccessKey** bölmesinde, yanındaki Kopyala düğmesine tıklatarak **bağlantı dizesi – birincil anahtar**, bağlantı dizesini Panonuza daha sonra kullanmak üzere kopyalayın.
   
    ![bağlantı dizesi][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


