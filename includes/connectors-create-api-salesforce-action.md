Eklediğiniz bir koşul, bir şey yapmak için kendi zamanı Tetikleyici tarafından oluşturulan verilerle ilginç. Eklemek için bu adımları **Salesforce - Nesne Al** eylem. Bu eylem, her yeni müşteri adayı oluşturulduğunda verileri alırsınız. Ayrıca, Salesforce - Office 365 Bağlayıcısı'nı kullanarak bir e-posta göndermek için bir nesne eylem Get verileri kullanacak olan ikinci bir eylem ekleyeceksiniz.  

Bunu yapılandırmak için eylem, aşağıdaki bilgileri sağlamanız gerekir. Bazı yeni dosya için özellikler için giriş olarak Tetikleyici tarafından oluşturulan kullanımı kolay veri olduğunu fark edeceksiniz:

| Dosya özelliği oluşturma | Açıklama |
| --- | --- |
| Nesne türü |İlgilendiğiniz Salesforce nesnesi türü budur. Örnek müşteri adayı, hesap, vs. verilebilir. |
| Nesne kimliği |Bu nesne için bir tanımlayıcı temsil eder. |

1. Seçin **Eylem Ekle** bağlantı. Bu açılır, herhangi bir işlem arayabileceğiniz arama kutusuna yapmak istiyorsunuz. Bu örnekte, Salesforce ilgi eylemlerdir.      
   ![Salesforce eylem görüntü 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Girin *salesforce* salesforce'a ilgili eylemler için aranacak.
3. Seçin **Salesforce - Nesne Al** gerçekleştirilecek eylemi olarak.   **Not**: mantıksal uygulamanızı, daha önce yapmadıysanız, Salesforce hesabınıza erişmesine yetki istenecektir.    
   ![Salesforce eyleminin resmi 2](./media/connectors-create-api-salesforce/action-2.png)    
4. **Nesne Al** denetim açar.  
5. Seçin *sağlama* nesne türü.
6. Seçin **nesne kimliği** denetimi.
7. Seçin **...**  eylemleri için giriş olarak kullanılabilecek belirteçler listesinde genişletin.       
   ![Salesforce eylem görüntüsü 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Seçin **müşteri adayı kimliği** denetim açar.   
   ![Salesforce eyleminin resmi 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Neden kimlik belirteci artık bu mantıksal uygulama tetiklenir sağlama için müşteri adayı kimliği eşit olan bir Kimliğe sahip bir müşteri adayı Get nesne eylem arar gösteren nesne kimliği denetiminde olduğuna dikkat edin.  
   ![Salesforce eyleminin resmi 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Çalışmanızı kaydedin. İşte bu kadar mantıksal uygulamanızı Get nesne eylemi ekledik. Get-nesne denetiminiz şu şekilde görünmelidir:    
    ![Salesforce eyleminin resmi 6](./media/connectors-create-api-salesforce/action-6.png)  

Bir müşteri adayı almak için bir eylem ekledikten sonra yeni oluşturulan müşteri adayı ile ilgi çekici bir şey yapmak isteyebilirsiniz. Bir kuruluşta yeni bir müşteri adayı oluşturulduğunu belirten bir dağıtım listesi bildiren bir e-posta göndermek isteyebilirsiniz. Salesforce'taki yeni müşteri adayı nesnesinden bazı ilgili bilgileri içeren bir e-posta göndermek için Office 365 Bağlayıcısı kullanalım.  

1. Seçin **Eylem Ekle** enter *e-posta* arama denetimi. Bu, e-posta gönderip için ilgili eylemleri filtreler.  
2. Seçin **Office 365 Outlook - e-posta Gönder** liste öğesi. Henüz oluşturmadıysanız bir *bağlantı* Office 365 hesabınıza, şimdi oluşturmak için Office 365 kimlik bilgilerinizi girmeniz istenir. Bitirdikten sonra **bir e-posta** denetim açar.        
   ![Salesforce eyleminin resmi 7](./media/connectors-create-api-salesforce/action-7.png)  
3. İçin e-posta göndermesini istediğiniz e-posta adresi girin **için** denetimi.
4. İçinde **konu** denetlemek, girin *oluşturulan yeni müşteri adayı* - seçip *şirket* belirteci. Bu görüntüler *şirket* Salesforce'ta oluşturulan yeni müşteri adayı alanını.  
5. İçinde **gövdesi** denetimi, herhangi bir belirteçler yeni müşteri adayı nesneyi seçin ve e-postanın gövdesinde görüntülemek istediğiniz herhangi bir metin de girebilirsiniz. Bir örneği aşağıda verilmiştir:  
   ![Salesforce eyleminin resmi 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Akışınızı kaydedin.  

Bu kadar. Mantıksal uygulamanız artık tamamlanmıştır.  

Şimdi mantıksal uygulamanızı test edebilirsiniz: Salesforce'ta oluşturduğunuz koşulu karşılayan yeni bir müşteri adayı oluşturun.  Bu kılavuz tamamen izlediyseniz, içeren bir e-posta adresiyle yalnızca bir müşteri adayı oluşturma *amazon.com* da. Birkaç saniye sonra mantıksal uygulamanızı tetikleyecek ve sonuçları şuna benzer şekilde görünebilir:  
![Salesforce eyleminin resmi 9](./media/connectors-create-api-salesforce/action-9.png)  

