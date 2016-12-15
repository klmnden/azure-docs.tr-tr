---
title: "LDAP Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu"
description: "Bu, LDAP Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/04/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 0cafcd1d21e12a3e8dfd020d1e59ee99d8c4d370


---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu
Varsayılan olarak, Azure Multi-Factor Authentication Sunucusu kullanıcıları Active Directory'den aktarmak ya da eşitlemek üzere yapılandırılmıştır. Ancak, ADAM dizini veya belirli Active Directory etki alanı denetleyicisi gibi farklı LDAP dizinlerini bağlamak için yapılandırılabilir. LDAP aracılığıyla bir dizine bağlanmak üzere yapılandırıldığında, Azure Multi-Factor Authentication Sunucusu kimlik doğrulaması gerçekleştirmek için bir LDAP proxy görevi görecek şekilde yapılandırılabilir. Ayrıca, IIS kimlik doğrulaması kullanırken ön kimlik doğrulaması için ya da Azure Multi-Factor Authentication Kullanıcı Portalı’nda birincil kimlik doğrulaması için LDAP Bağlama’nın RADIUS hedefi olarak kullanılmasına izin verir.

Azure Multi-Factor Authentication’ı LDAP proxy olarak kullanırken, multi-factor authentication eklemek amacıyla Azure Multi-Factor Authentication Sunucusu LDAP istemcisi (örneğin, VPN gereci, uygulaması) ile LDAP dizini arasına eklenir. Azure Multi-Factor Authentication özelliğinin çalışması için, Azure Multi-Factor Authentication doğrulama sunucusunun istemci sunucular ve LDAP dizini ile iletişim kurmak için yapılandırılmış olması gerekir. Bu yapılandırmada, Azure Multi-Factor Authentication Sunucusu istemci sunucularından uygulamalarından gelen LDAP isteklerini kabul eder ve bunları birincil kimlik bilgilerini doğrulamak amacıyla hedef LDAP dizini sunucusuna iletir. LDAP dizininden alınan yanıt birincil kimlik bilgilerinin geçerli olduğunu gösteriyorsa, Azure Multi-Factor Authentication iki öğeli kimlik doğrulaması gerçekleştirir ve LDAP istemcisine geri yanıt gönderir. Yalnızca LDAP sunucusu için kimlik doğrulaması ve multi-factor authentication başarılı olursa, tüm kimlik doğrulama işlemi başarılı olur.

## <a name="ldap-authentication-configuration"></a>LDAP Kimlik Doğrulaması Yapılandırması
LDAP kimlik doğrulamasını yapılandırmak için, bir Windows sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin. Aşağıdaki yordamı kullanın:

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde LDAP Kimlik Doğrulaması simgesine tıklayın.
2. LDAP Kimlik Doğrulamasını etkinleştir onay kutusunu işaretleyin.![LDAP Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Azure Multi-Factor Authentication LDAP hizmetinin yapılandırılacak istemcilerden gelen LDAP isteklerini dinlemek için standart olmayan bağlantı noktalarına bağlanması gerekliyse, İstemciler sekmesinde TCP bağlantı noktasını ve SSL bağlantı noktasını değiştirin.
4. İstemciden gelen LDAPS’ı Azure Multi-Factor Authentication Sunucusu için kullanmayı planlıyorsanız, Sunucu’nun çalıştığı sunucuya bir SSL sertifikası yüklenmelidir. SSL sertifikası... kutusunun yanındaki Gözat düğmesine tıklayın ve güvenli bağlantı için kullanılacak yüklü sertifikayı seçin.
5. Ekle... düğmesine tıklayın.
6. LDAP İstemcisi Ekle iletişim kutusuna, Sunucu için kimlik doğrulaması gerçekleştirecek gereç, sunucu ya da uygulamanın IP adresini ve bir Uygulama adı (isteğe bağlı) girin. Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
7. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Azure Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın. Bu özellik hakkında ek bilgi için yardım dosyasına bakın.
8. Ek LDAP istemcileri eklemek için 5. adım-7. adıma kadarı tekrar edebilirsiniz.
9. Azure Multi-Factor Authentication LDAP kimlik doğrulamaları almak üzere yapılandırıldığında, bu kimlik doğrulamalarını LDAP dizinine sunmalıdır. Bu nedenle, LDAP hedefi olarak kullanmak üzere, Hedef sekmesi yalnızca tek bir, gri seçenek görüntüler. LDAP dizini bağlantısını yapılandırmak için, Dizin Tümleştirme simgesine tıklayın.
10. Ayarlar sekmesinde, Özel LDAP yapılandırması kullan radyo düğmesini seçin.
11. Düzenle düğmesine... tıklayın.
12. LDAP Yapılandırmasını Düzenle iletişim kutusunda, LDAP dizinine bağlanmak için gerekli bilgilerle alanları doldurun. Alanların açıklamaları aşağıdaki tabloda yer almaktadır: Not: Bu bilgiler ayrıca Azure Multi-Factor Authentication Yardım dosyasında da bulunmaktadır.![Dizin Tümleştirme](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Test Et düğmesine tıklayarak LDAP bağlantısını test edin.
14. LDAP bağlantısı testi başarılı olursa, Tamam düğmesine tıklayın.
15. Filtreler sekmesine tıklayın. Sunucu, Active Directory’den kapsayıcıları, güvenlik gruplarını ve kullanıcıları yüklemek üzere önceden yapılandırılmıştır. Farklı bir LDAP dizinine bağlanıyorsanız, büyük olasılıkla görüntülenen filtreleri düzenlemeniz gerekir. Filtreler hakkında daha fazla bilgi için Yardım bağlantısına tıklayın.
16. Öznitelikler sekmesine tıklayın. Sunucu Active Directory’deki öznitelikleri eşlemek üzere önceden yapılandırılmıştır.
17. Farklı bir LDAP dizinine bağlanıyorsa veya önceden yapılandırılmış öznitelik eşlemelerini değiştirmek için Düzenle düğmesine... tıklayın.
18. Öznitelikleri Düzenle iletişim kutusunda, dizininizin LDAP öznitelik eşlemelerini değiştirin. Öznitelik adları yazılabilir veya her alanın yanındaki ... düğmesine tıklanarak seçilebilir.
19. Öznitelikler hakkında daha fazla bilgi için Yardım bağlantısına tıklayın.
20. Tamam düğmesine tıklayın.
21. Şirket Ayarları simgesine tıklayın ve Kullanıcı Adı Çözümleme sekmesini seçin.
    22. Active Directory’ye etki alanına katılmış bir sunucudan bağlanılıyorsa, seçilen kullanıcı adlarını eşleştirme radyo düğmesi için Windows güvenlik tanımlayıcılarını (SID'ler) bırakabilmelisiniz. Aksi halde, kullanıcı adlarını eşleştirme radyo düğmesi için LDAP benzersiz tanımlayıcı özniteliğini seçin. Seçildiğinde, Azure Multi-Factor Authentication Sunucusu her kullanıcı adını LDAP dizinindeki benzersiz bir tanımlayıcıya çözümlemeye çalışır. Dizin Tümleştirme -> Öznitelikler sekmesinde tanımlanan Kullanıcı adı özniteliklerinde bir LDAP araması gerçekleştirilir. Bir kullanıcının kimliği doğrulandığında, kullanıcı adı LDAP dizinindeki benzersiz tanımlayıcıya çözümlenir ve benzersiz tanımlayıcı kullanıcıyı Azure Multi-Factor Authentication veri dosyasında eşleştirmek için kullanılır. Bu, büyük-küçük harf duyarlı olmayan karşılaştırmaların yanı sıra kısa ve uzun kullanıcı adımlarına olanak tanır. Bu Azure Multi-Factor Authentication Sunucusu yapılandırmasını tamamlar. Sunucu artık yapılandırılan istemcilerden gelen LDAP erişimi istekleri için yapılandırılan bağlantı noktalarını dinler ve kimlik doğrulaması için bu istekleri LDAP dizinine sunmaya ayarlanır.

## <a name="ldap-client-configuration"></a>LDAP İstemcisi Yapılandırması
LDAP istemcisini yapılandırmak için yönergeleri kullanın:

* LDAP aracılığıyla gerecinizi, sunucunuzu ya da uygulamanızı LDAP dizinindeymiş gibi Azure Multi-Factor Authentication Sunucusu için kimliğini doğrulamak üzere yapılandırın. Azure Multi-Factor Authentication Sunucusu’na ait olacak sunucu adı ya da IP adresi için olanlar dışında, LDAP dizininize normalde doğrudan bağlandığınızla aynı ayarları kullanmalısınız.
* Kullanıcının kimlik bilgilerini LDAP diziniyle doğrulama, iki öğeli kimlik doğrulaması gerçekleştirme, bunların yanıtını alma ve sonra LDAP erişim isteğini yanıtlamaya zaman kalacak şekilde, LDAP zaman aşımını 30-60 saniye olarak yapılandırın.
* LDAPS kullanıyorsanız, LDAP sorgularını yapan gereç ya da sunucunun Azure Multi-Factor Authentication Sunucusu’nda yüklü SSL sertifikasına güvenmesi gerekir.




<!--HONumber=Dec16_HO1-->


