Bu örnek kullanılmasına [Twilio](https://www.twilio.com/) cep telefonuna SMS iletileri göndermek için hizmet. Azure işlevleri Twilio desteği zaten [Twilio bağlama](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), ve örnek bu özelliği kullanır.

Size gereken ilk şey bir Twilio hesabıdır. Bir anda boş oluşturabilirsiniz https://www.twilio.com/try-twilio. Hesabınızı edindikten sonra aşağıdaki üç ekleme **uygulama ayarları** işlevi uygulamanıza.

| Uygulama ayarı adı | Değer açıklaması |
| - | - |
| **TwilioAccountSid**  | Twilio hesabınız için SID |
| **TwilioAuthToken**   | Twilio hesabınız için kimlik doğrulama belirteci |
| **TwilioPhoneNumber** | Twilio hesabınızla ilişkili telefon numarası. Bu, SMS iletileri göndermek için kullanılır. |