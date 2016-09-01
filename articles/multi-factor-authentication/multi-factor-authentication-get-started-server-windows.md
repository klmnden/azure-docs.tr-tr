<properties 
    pageTitle="Windows Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu" 
    description="Bu, Windows Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Windows Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu

Windows Kimlik Doğrulaması bölümü yöneticinin bir ya da daha fazla uygulama için Windows kimlik doğrulamasını etkinleştirmesini ve yapılandırmasını sağlar.  Aşağıda Windows Kimlik Doğrulaması’nı ayarlamadan önce göz önünde bulundurulacakların bir listesi verilmiştir.

-  Terminal Hizmetleri için Azure Multi-Factor Authentication’ın etkin olması için yeniden başlatma gereklidir.
-  “Azure Multi-Factor Authentication kullanıcı eşleşmesi gerektir” seçeneği işaretliyse ve kullanıcı listesinden değilseniz, yeniden başlatma sonrası makinede oturum açamazsınız.
-  Güvenilen IP'ler uygulamanın ile kimlik doğrulaması ile istemci IP’si sağlayıp sağlayamayacağına bağlıdır. Şu anda yalnızca Terminal Hizmetleri desteklenmektedir.  







>[AZURE.NOTE]Bu özellik, Windows Server 2012 R2’de Terminal Hizmetleri’ni güvenli hale getirmek için desteklenmemektedir.
 



## Bir uygulamayı Windows Kimlik Doğrulaması ile güvenli hale getirmek için aşağıdaki yordamı kullanın.

1. Azure Multi-Factor Authentication Sunucusu’nda Windows Kimlik Doğrulaması simgesine tıklayın.
![Windows Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Windows kimlik doğrulamasını etkinleştir onay kutusunu işaretleyin. Varsayılan olarak, bu kutu işaretlenmemiştir.
3. Uygulamalar sekmesi yöneticinin Windows Kimlik Doğrulaması için bir veya daha fazla uygulamayı yapılandırmasını sağlar.
4. Bir sunucu veya uygulama seçin – sunucusu/uygulama etkin olup olmadığını belirtin. Tamam'a tıklayın.
5. Ekle... düğmesine tıklayın.
6. Güvenilen IP'ler sekmesi belirli IP'lerden gelen Windows oturumları için Azure Multi-Factor Authentication’ı atlamanızı sağlar. Örneğin, çalışanlar ofiste ve evden uygulama kullanıyorsa, ofisteyken Azure Multi-Factor Authentication için telefonlarının çalmasını istemediğinize karar verebilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz.
7. Ekle... düğmesine tıklayın.
8. Tek bir IP adresini atlamak istiyorsanız, tek bir IP adresi seçin.
9. Tüm IP aralığını atlamak istiyorsanız, IP aralığını seçin. Örnek: 10.63.193.1-10.63.193.100.
10. Bir alt ağ gösterimini kullanarak IP aralığı belirtmek istiyorsanız, alt ağı seçin. Alt ağın başlangıç IP’sini girin ve açılır listede uygun ağ maskesini seçin. 
11. Tamam düğmesine tıklayın.



<!--HONumber=Aug16_HO4-->


