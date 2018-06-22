### <a name="prerequisites"></a>Önkoşullar
* Twilio hesabı
* SMS alabileceği bir doğrulanmış Twilio telefon numarası
* SMS gönderebilen bir doğrulanmış Twilio telefon numarası

> [!NOTE]
> Twilio deneme hesabı kullanıyorsanız, SMS için yalnızca gönderebilirsiniz **doğrulandı** telefon numaraları.  
> 
> 

Bir mantıksal uygulama Twilio hesabınızı kullanabilmeniz için önce Twilio hesabınıza bağlanmak için mantığı uygulamasını yetkilendirmeniz gerekir. Neyse ki, Azure Portal'da mantıksal uygulama içinde bu kolayca yapabilirsiniz. 

Mantıksal uygulamanızı Twilio hesabınıza bağlanmak için yetki vermek için adımlar şunlardır:

1. Mantıksal Uygulama Tasarımcısı'nda Twilio, bir bağlantı oluşturmak için seçin **Göster Microsoft yönetilen API'ler** açılır listesinde enter *Twilio* arama kutusuna. Tetikleyici seçin veya eylem kullanmak ister:  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Twilio önce bağlantılarına oluşturmadıysanız, Twilio kimlik bilgilerinizi girmeniz istendiğinde şunu. Bu kimlik bilgileri bağlanmak için mantıksal uygulamanızı yetkilendirmek için kullanılır ve Twilio hesabınızın veri erişim:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. İhtiyacınız vardır **Twilio hesap kimliği** ve **Twilio erişim belirteci** Twilio içinde panodan şekilde Twilio hesabınıza şimdi bu iki parça bilgi edinmek oturum açın:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio ve Logic apps farklı adlar iki bu bilgilerin tanımlamak için kullanın. İşte nasıl bunları Logic apps iletişim kutusuna eşlemeniz gerekir: ![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Seçin **bağlantı oluşturmak** düğmesi:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Bağlantı oluşturuldu ve artık bir mantıksal uygulamanızı adımlarda yüklemeye devam etmek ücretsiz dikkat edin:  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

