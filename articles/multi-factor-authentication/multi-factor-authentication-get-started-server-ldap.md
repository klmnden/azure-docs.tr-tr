---
title: "LDAP Kimlik Doğrulaması ve Azure MFA Sunucusu | Microsoft Belgeleri"
description: "Bu, LDAP Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/03/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 1c6386dda94a3e0ca6eb340f542d04cb336159c3
ms.openlocfilehash: 0de97050e385e3efb9e63bbf934712157ab0d0af


---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu
Varsayılan olarak, Azure Multi-Factor Authentication Sunucusu kullanıcıları Active Directory'den aktarmak ya da eşitlemek üzere yapılandırılmıştır. Ancak, ADAM dizini veya belirli Active Directory etki alanı denetleyicisi gibi farklı LDAP dizinlerini bağlamak için yapılandırılabilir. LDAP aracılığıyla bir dizine bağlandığında, Azure Multi-Factor Authentication Sunucusu kimlik doğrulaması gerçekleştirmek için bir LDAP proxy görevi görecek şekilde yapılandırılabilir. Ayrıca, IIS Kimlik Doğrulaması ile ön kimlik doğrulaması için ya da Azure MFA kullanıcı portalında birincil kimlik doğrulaması için LDAP bağlamanın RADIUS hedefi olarak kullanılmasına izin verir.

Azure Multi-Factor Authentication’ı LDAP proxy olarak kullanmak için Azure Multi-Factor Authentication Sunucusunu LDAP istemcisi (örneğin VPN gereci, uygulaması) ile LDAP dizin sunucusu arasına ekleyin. Azure Multi-Factor Authentication doğrulama sunucusunun istemci sunucular ve LDAP dizini ile iletişim kurmak için yapılandırılmış olması gerekir. Bu yapılandırmada, Azure Multi-Factor Authentication Sunucusu istemci sunucularından uygulamalarından gelen LDAP isteklerini kabul eder ve bunları birincil kimlik bilgilerini doğrulamak amacıyla hedef LDAP dizini sunucusuna iletir. LDAP dizininden alınan yanıt birincil kimlik bilgilerinin geçerli olduğunu gösteriyorsa, Azure Multi-Factor Authentication ikinci bir kimlik doğrulaması gerçekleştirir ve LDAP istemcisine geri yanıt gönderir. Kimlik doğrulama sürecinin tamamının başarılı olması için hem LDAP sunucusu kimlik doğrulamasının hem de ikinci adım kimlik doğrulama işleminin başarılı olması gerekir.

## <a name="ldap-authentication-configuration"></a>LDAP Kimlik Doğrulaması Yapılandırması
LDAP kimlik doğrulamasını yapılandırmak için, bir Windows sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin. Aşağıdaki yordamı kullanın:

### <a name="add-an-ldap-client"></a>LDAP istemcisi ekleme

1. Azure Multi-Factor Authentication Sunucusunda, soldaki menüde LDAP Kimlik Doğrulaması simgesini seçin.
2. **LDAP Kimlik Doğrulamasını etkinleştir** onay kutusunu işaretleyin.

        ![LDAP Authentication](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)

3. Azure Multi-Factor Authentication LDAP hizmetinin LDAP isteklerini dinlemek için standart olmayan bağlantı noktalarına bağlanması gerekliyse, İstemciler sekmesinde TCP bağlantı noktasını ve SSL bağlantı noktasını değiştirin.
4. İstemciden gelen LDAPS’ı Azure Multi-Factor Authentication Sunucusu için kullanmayı planlıyorsanız, Sunucu’nun çalıştığı sunucuya bir SSL sertifikası yüklenmelidir. **Gözat…** düğmesine tıklayın. kutusunun yanındaki Gözat düğmesine tıklayın ve güvenli bağlantı için kullanılacak yüklü sertifikayı seçin.
5. **Ekle...** düğmesine tıklayın.
6. LDAP İstemcisi Ekle iletişim kutusuna, Sunucu için kimlik doğrulaması gerçekleştirecek gereç, sunucu ya da uygulamanın IP adresini ve bir Uygulama adı (isteğe bağlı) girin. Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
7. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın. Bu özellik hakkında ek bilgi için yardım dosyasına bakın.

Bu adımları tekrarlayarak diğer LDAP istemcilerini ekleyin.

### <a name="configure-the-ldap-directory-connection"></a>LDAP dizini bağlantısını yapılandırma

Azure Multi-Factor Authentication LDAP kimlik doğrulamaları almak üzere yapılandırıldığında, bu kimlik doğrulamalarını LDAP dizinine sunmalıdır. Bu nedenle, LDAP hedefi olarak kullanmak üzere, Hedef sekmesi yalnızca tek bir, gri seçenek görüntüler.

1. LDAP dizini bağlantısını yapılandırmak için **Dizin Tümleştirme** simgesine tıklayın.
2. Ayarlar sekmesinde **Özel LDAP yapılandırması kullan** radyo düğmesini seçin.
3. **Düzenle…** seçeneğini belirleyin.
4. LDAP Yapılandırmasını Düzenle iletişim kutusunda, LDAP dizinine bağlanmak için gerekli bilgilerle alanları doldurun. Bu alanların açıklamaları Azure Multi-Factor Authentication Sunucusu yardım dosyasında da bulunmaktadır.

    ![Dizin Tümleştirme](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)

5. **Test** düğmesine tıklayarak LDAP bağlantısını test edin.
6. LDAP bağlantı testi başarılı olursa **Tamam** düğmesine tıklayın.
7. **Filtreler** sekmesine tıklayın. Sunucu, Active Directory’den kapsayıcıları, güvenlik gruplarını ve kullanıcıları yüklemek üzere önceden yapılandırılmıştır. Farklı bir LDAP dizinine bağlanıyorsanız, büyük olasılıkla görüntülenen filtreleri düzenlemeniz gerekir. Filtreler hakkında daha fazla bilgi için **Yardım** bağlantısına tıklayın.
8. **Öznitelikler** sekmesine tıklayın. Sunucu Active Directory’deki öznitelikleri eşlemek üzere önceden yapılandırılmıştır.
9. Farklı bir LDAP dizinine bağlıyorsanız veya önceden yapılandırılmış öznitelik eşlemelerini değiştirmek istiyorsanız için **Düzenle…** düğmesine tıklayın.
10. Öznitelikleri Düzenle iletişim kutusunda, dizininizin LDAP öznitelik eşlemelerini değiştirin. Öznitelik adlarını yazabilir veya **…** simgesine tıklayarak seçebilirsiniz. düğmesine tıklanarak seçilebilir. Öznitelikler hakkında daha fazla bilgi için **Yardım** bağlantısına tıklayın.
11. **Tamam** düğmesine tıklayın.
12. **Şirket Ayarları** simgesine tıklayın ve **Kullanıcı Adı Çözümleme** sekmesini seçin.
13. Active Directory’ye etki alanına katılmış bir sunucudan bağlanıyorsanız, **Kullanıcı adlarını eşlemek için Windows güvenlik tanımlayıcılarını (SID’ler) kullan** radyo düğmesini seçili bırakın. Diğer durumlarda **Kullanıcı adlarını eşlemek için LDAP benzersiz tanımlayıcı özniteliğini kullan** radyo düğmesini seçin. 

**Kullanıcı adlarını eşlemek için LDAP benzersiz tanımlayıcı özniteliğini kullan** radyo düğmesi seçili olduğunda Azure Multi-Factor Authentication Sunucusu her kullanıcı adını LDAP dizinindeki benzersiz bir tanımlayıcıya çözümlemeyi dener. Dizin Tümleştirme -> Öznitelikler sekmesinde tanımlanan Kullanıcı adı özniteliklerinde bir LDAP araması gerçekleştirilir. Bir kullanıcının kimliği doğrulandığında, kullanıcı adı LDAP dizinindeki benzersiz tanımlayıcıya çözümlenir ve benzersiz tanımlayıcı kullanıcıyı Azure Multi-Factor Authentication veri dosyasında eşleştirmek için kullanılır. Bu, büyük küçük harf duyarlı olmayan karşılaştırmaların yanı sıra uzun ve kısa kullanıcı adı biçimlerini mümkün kılar.

Bu adımları tamamladıktan sonra MFA Sunucusu yapılandırılan istemcilerden gelen LDAP erişimi istekleri için yapılandırılan bağlantı noktalarını dinlemeye başlar ve kimlik doğrulaması için bu istekleri LDAP dizinine sunmaya ayarlanır.

## <a name="ldap-client-configuration"></a>LDAP İstemcisi Yapılandırması
LDAP istemcisini yapılandırmak için yönergeleri kullanın:

* LDAP aracılığıyla gerecinizi, sunucunuzu ya da uygulamanızı LDAP dizinindeymiş gibi Azure Multi-Factor Authentication Sunucusu için kimliğini doğrulamak üzere yapılandırın. Azure Multi-Factor Authentication Sunucusu’na ait olacak sunucu adı ya da IP adresi için olanlar dışında, LDAP dizininize normalde doğrudan bağlandığınızla aynı ayarları kullanın.
* Kullanıcının kimlik bilgilerini LDAP diziniyle doğrulama, ikinci adım doğrulamayı gerçekleştirme, bunların yanıtını alma ve sonra LDAP erişim isteğini yanıtlamaya zaman kalacak şekilde, LDAP zaman aşımını 30-60 saniye olarak yapılandırın.
* LDAPS kullanıyorsanız, LDAP sorgularını yapan gereç ya da sunucunun Azure Multi-Factor Authentication Sunucusu’nda yüklü SSL sertifikasına güvenmesi gerekir.




<!--HONumber=Jan17_HO1-->


