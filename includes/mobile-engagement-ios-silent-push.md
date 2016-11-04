> [!IMPORTANT]
> Mobile Engagement’tan Anında İletme Bildirimi almak için uygulamanızda `Silent Remote Notifications` etkinleştirmeniz gerekir. Info.plist dosyanızdaki UIBackgroundModes dizisine uzaktan bildirim değeri eklemeniz gerekir.
> 
> 

1. Projede `info.plist` dosyasını açın
2. Listedeki ilk öğeye (`Information Property List`) sağ tıklayın ve yeni bir satır ekleyin
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. nYeni satıra şunu girin: `Required background modes`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. Satırı genişletmek için sol oka tıklayın
5. 0 öğesine şu değeri ekleyin: `App downloads content in response to push notifications`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. Değişikliği yaptıktan sonra XML info.plist dosyasında şu anahtar ve değer olmalıdır:
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. **Xcode 7 +** ve **iOS 9 +** kullanıyorsanız:
   
   * **Anında İletme Bildirimleri**’ni Hedefler > Hedef Adınız > Özellikler’de etkinleştirin.

<!--HONumber=Jun16_HO2-->


