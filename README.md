# MailComposer_AttachImage

```swift
import UIKit
import MessageUI

class MailComposeController: UIViewController, MFMailComposeViewControllerDelegate {

    let emailSubject = "My email title"
    let emailBody = "Hello this is my email\n\nIt is multi-line\n123\nFrom\nPaigeSoftware"
    
    let toEmails = ["paigeshin1991@gmail.com"]
    let fromEmail = "paigeshin1991@gmail.com"
    
    let images: [UIImage] = [
        UIImage(named: "1")!,
        UIImage(named: "2")!,
        UIImage(named: "3")!,
    ]
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.view.backgroundColor = .systemBlue
        self.showMailComposer(self.emailSubject,
                              self.emailBody,
                              self.toEmails,
                              self.fromEmail,
                              self.images)
    }

    private func showMailComposer(
        _ subject: String,
        _ body: String,
        _ toEmails: [String],
        _ fromEmail: String,
        _ images: [UIImage]
    ) {
        guard MFMailComposeViewController.canSendMail() else {
            print("canSendMail Failure")
            return
        }
        
        let composer = MFMailComposeViewController()
        composer.mailComposeDelegate = self
        composer.setSubject(subject)
        composer.setMessageBody(body, isHTML: false)
        composer.setToRecipients(toEmails)
        composer.setPreferredSendingEmailAddress(fromEmail)
        
        for image in images {
            let fileName = Int.random(in: 0...50000).description + ".jpeg"
            let imageData = image.jpegData(compressionQuality: 1)!
            composer.addAttachmentData(imageData, mimeType: "image/jpg", fileName: fileName)
        }
        
        present(composer, animated: true)
        
    }
    
    func mailComposeController(_ controller: MFMailComposeViewController, didFinishWith result: MFMailComposeResult, error: Error?) {
        if let error {
            print("DEBUG PRINT:", "didFinishWithError", error)
            controller.dismiss(animated: true)
            return
        }
        controller.dismiss(animated: true)
    }
    
    
}



```
