1. Klasik Azure portalında oturum açın.

2. Portalın sol gezinti bölmesinde **Service Bus** hizmetine tıklayın.

3. Portalın alt bölmesinde **Oluştur**'a tıklayın.

    ![Oluştur’u seçin][select-create]
   
4. **Yeni bir ad alanı ekle** iletişim kutusunda ad alanı adını girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.

    ![Ad alanı adı][namespace-name]
  
5. Ad alanı adının kullanılabilir olduğundan emin olduktan sonra ad alanınızın barındırılması gereken ülke veya bölgeyi seçin.

6. İletişim kutusundaki diğer alanları varsayılan değerleriyle bırakın (**Mesajlaşma** ve **Standart Katman**), ardından Tamam onay işaretine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.
 
    ![Başarıyla oluşturuldu][created-successfully]

###Kimlik bilgilerini alın
1. Kullanılabilir ad alanlarının listesini görüntülemek için sol gezinti bölmesinde **Service Bus** düğümüne tıklayın:
 
    ![Hizmet veri yolu seçin][select-service-bus]
  
2. Görüntülenen listede az önce oluşturduğunuz ad alanını seçin:
 
    ![Ad alanı seçin][select-namespace]
 
3. **Bağlantı Bilgileri**'ne tıklayın.

    ![Bağlantı bilgileri][connection-information]
  
4. **Erişim bağlantısı bilgileri** bölmesinde, SAS anahtarını ve anahtar adını içeren bağlantı dizesini bulun.

    ![Bağlantı bilgilerine erişin][access-connection-information]
  
5. Anahtarı not edin veya panoya kopyalayın.

<!--Image references-->

[select-create]: ./media/service-bus-create-namespace-portal/select-create.png
[namespace-name]: ./media/service-bus-create-namespace-portal/namespace-name.png
[created-successfully]: ./media/service-bus-create-namespace-portal/created-successfully.png
[select-service-bus]: ./media/service-bus-create-namespace-portal/select-service-bus.png
[select-namespace]: ./media/service-bus-create-namespace-portal/select-namespace.png
[connection-information]: ./media/service-bus-create-namespace-portal/connection-information.png
[access-connection-information]: ./media/service-bus-create-namespace-portal/access-connection-information.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[classic-portal]: https://manage.windowsazure.com


<!--HONumber=Aug16_HO1-->


