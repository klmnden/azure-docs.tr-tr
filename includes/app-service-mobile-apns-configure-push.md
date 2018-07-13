

1. Mac'inizde başlatma **Anahtarlık erişimi**. Sol gezinti çubuğunda, altında **kategori**açın **My sertifikaları**. Önceki bölümde indirdiğiniz SSL sertifikasını bulmak ve ardından içeriği ifşa. Yalnızca sertifikası seçin (özel anahtarı seçmek için değil). Ardından [dışarı](https://support.apple.com/kb/PH20122?locale=en_US).
2. İçinde [Azure portalında](https://portal.azure.com/)seçin **tümüne Gözat** > **uygulama hizmetleri**. Ardından select, Mobile Apps arka ucu. 
3. Altında **ayarları**seçin **App Service gönderim**. Ardından, bildirim hub'ı adı seçin. 
3. Git **Apple anında iletilen bildirim servisi** > **sertifikayı karşıya yükle**. Doğru seçerek .p12 dosyası karşıya **modu** (istemci SSL sertifikası önceki olup olmadığına bağlı olarak üretim ya da korumalı alan'dir). Tüm değişiklikleri kaydedin.

Hizmetiniz, artık iOS anında iletme bildirimleri ile çalışacak şekilde yapılandırılmıştır.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
