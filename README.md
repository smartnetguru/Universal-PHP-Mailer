# Universal PHP Mailer
Super simple yet very powerful PHP mailer. It can use the PHP `mail()` function or the `SMTP` method (local or remote SMTP server).

##Universal?

This doesn't mean that it works in every configuration and every situation.

It means that this mailer is capable of sending anything. You can use it to send very simple mail and even some very complex content combinations. Just give it whatever you have and fire it off! It will automatically configure itself to compose the correct MIME mail string with whatever parts and multiparts are appropriate.

When using the `SMTP` method, the mailer reuses the same socket connection for sending multiple messages, thus achieving better efficiency than the `mail()` function method.

These are the possible combinations (automatic configurations):

##Non-multipart mail:
- only one `attachment`
- only one `text/plain`
- only one `text/html`

##Multipart mail:
- two or more `attachment`
- `text/plain` + `text/html` + zero or more `attachment`
- `text/html`  + `inline images` + zero or more `attachment`
- `text/plain` + `text/html` + `inline images` + zero or more `attachment`


---

How to send `text/plain`:
```php
require 'universalphpmailer.class.php';

$mailor = new universalPHPmailer;

$mailor->toName    = 'John Smith';
$mailor->toEmail   = 'john.smith@udwiwobg';
$mailor->subject   = 'Lorem Ipsum';
$mailor->fromName  = 'James Jones';
$mailor->fromEmail = 'james.jones@udwiwobg';
$mailor->hostName  = 'zarscwfo';

$mailor->textPlain = 'Hi John,

I am testing this mailer from GitHub. Please let me know if you\'ve received this message.

Best regards,
J.J.';

$msgID = $mailor->sendMessage();

if (!empty($msgID)) {
  echo 'Successfully sent message ID ..... '. $msgID;
}

```

---

How to send `text/html`:
```php
require 'universalphpmailer.class.php';

$mailor = new universalPHPmailer;

$mailor->toName    = 'John Smith';
$mailor->toEmail   = 'john.smith@udwiwobg';
$mailor->subject   = 'Lorem Ipsum';
$mailor->fromName  = 'James Jones';
$mailor->fromEmail = 'james.jones@udwiwobg';
$mailor->hostName  = 'zarscwfo';

$mailor->textHtml  = '<body>
  <p>Hi John,</p>
  <p>I am testing this mailer from GitHub. Please let me know if you\'ve received this message.</p>
  <p>Best regards,<br>J.J.</p>
</body>';

$msgID = $mailor->sendMessage();

if (!empty($msgID)) {
  echo 'Successfully sent message ID ..... '. $msgID;
}

```

---

How to send `text/html` + `inline images`:
```php
require 'universalphpmailer.class.php';
require 'mimetypes.class.php';

$mailor = new universalPHPmailer;

$mailor->toName    = 'John Smith';
$mailor->toEmail   = 'john.smith@udwiwobg';
$mailor->subject   = 'Lorem Ipsum';
$mailor->fromName  = 'James Jones';
$mailor->fromEmail = 'james.jones@udwiwobg';
$mailor->hostName  = 'zarscwfo';

$cidA = $mailor->addInlineImage('/some/path/imageA.jpg'); # This gives us the content ID string
$cidB = $mailor->addInlineImage('/some/path/imageB.png');

$mailor->textHtml  = '<body>
  <p>Hi John,</p>
  <p>I am testing this mailer from GitHub. Please let me know if you\'ve received this message.</p>
  <p>Best regards,<br>J.J.</p>
  <div><img src="cid:'.$cidA.'" width="200" height="100" alt="Image A"></div>
  <div><img src="cid:'.$cidB.'" width="200" height="100" alt="Image B"></div>
</body>';

$msgID = $mailor->sendMessage();

if (!empty($msgID)) {
  echo 'Successfully sent message ID ..... '. $msgID;
}

```

---

How to send `text/plain` + `text/html` + `inline images`:
```php
require 'universalphpmailer.class.php';
require 'mimetypes.class.php';

$mailor = new universalPHPmailer;

$mailor->toName    = 'John Smith';
$mailor->toEmail   = 'john.smith@udwiwobg';
$mailor->subject   = 'Lorem Ipsum';
$mailor->fromName  = 'James Jones';
$mailor->fromEmail = 'james.jones@udwiwobg';
$mailor->hostName  = 'zarscwfo';

$cidA = $mailor->addInlineImage('/some/path/imageA.jpg'); # This gives us the content ID string
$cidB = $mailor->addInlineImage('/some/path/imageB.png');

$mailor->textPlain = 'Hi John,

I am testing this mailer from GitHub. Please let me know if you\'ve received this message.

Best regards,
J.J.';

$mailor->textHtml  = '<body>
  <p>Hi John,</p>
  <p>I am testing this mailer from GitHub. Please let me know if you\'ve received this message.</p>
  <p>Best regards,<br>J.J.</p>
  <div><img src="cid:'.$cidA.'" width="200" height="100" alt="Image A"></div>
  <div><img src="cid:'.$cidB.'" width="200" height="100" alt="Image B"></div>
</body>';

$msgID = $mailor->sendMessage();

if (!empty($msgID)) {
  echo 'Successfully sent message ID ..... '. $msgID;
}

```

---

How to send `text/plain` + `text/html` + `inline images` + `attachment`:
```php
require 'universalphpmailer.class.php';
require 'mimetypes.class.php';

$mailor = new universalPHPmailer;

$mailor->toName    = 'John Smith';
$mailor->toEmail   = 'john.smith@udwiwobg';
$mailor->subject   = 'Lorem Ipsum';
$mailor->fromName  = 'James Jones';
$mailor->fromEmail = 'james.jones@udwiwobg';
$mailor->hostName  = 'zarscwfo';

$cidA = $mailor->addInlineImage('/some/path/imageA.jpg'); # This gives us the content ID string
$cidB = $mailor->addInlineImage('/some/path/imageB.png');

$mailor->addAttachment('/some/path/contract.pdf');

$mailor->textPlain = 'Hi John,

I have attached the contract PDF document.

Best regards,
J.J.';

$mailor->textHtml  = '<body>
  <p>Hi John,</p>
  <p>I have attached the contract PDF document.</p>
  <p>Best regards,<br>J.J.</p>
  <div><img src="cid:'.$cidA.'" width="200" height="100" alt="Image A"></div>
  <div><img src="cid:'.$cidB.'" width="200" height="100" alt="Image B"></div>
</body>';

$msgID = $mailor->sendMessage();

if (!empty($msgID)) {
  echo 'Successfully sent message ID ..... '. $msgID;
}

```

---

How to send `text/html` + `inline images` in a loop (multiple recipients):
```php
require 'universalphpmailer.class.php';
require 'mimetypes.class.php';

$mailor = new universalPHPmailer;

$mailor->subject   = 'Weekly best deal newsletter';
$mailor->fromName  = '"Mail Robot (don\'t reply)"'; # Display name per RFC5322
$mailor->fromEmail = 'james.jones@udwiwobg';
$mailor->hostName  = 'zarscwfo';

$recipientArr = array(
  0 => array(
            'name'  => 'John Doe', # Display name per RFC5322
            'email' => 'j.doe@udwiwobg',
  ),
  1 => array(
            'name'  => '"Jane V. Wise" (smart cookie)', # Display name and comment per RFC5322
            'email' => 'j.wise@udwiwobg',
  ),
  2 => array(
            'name'  => '"Robert W. Simth"', # Display name per RFC5322
            'email' => 'robert.smith@udwiwobg',
  ),
);

# These 2 images are same for all recipients
$cidA = $mailor->addInlineImage('/some/path/imageA.jpg'); # This gives us the content ID string
$cidB = $mailor->addInlineImage('/some/path/imageB.png');

# The loop
foreach ($recipientArr as $recipient) {

  $mailor->toName    = $recipient['name'];
  $mailor->toEmail   = $recipient['email'];

  $mailor->textHtml  = '<body>
    <p>Hi '.$recipient['name'].',</p>
    <p>You are receiving this newsletter because you have subscribed.</p>
    <p>Best regards,<br>'.$mailor->fromName.'</p>
    <div><img src="cid:'.$cidA.'" width="200" height="100" alt="Image A"></div>
    <div><img src="cid:'.$cidB.'" width="200" height="100" alt="Image B"></div>
  </body>';

  $msgID = $mailor->sendMessage();

  if (!empty($msgID)) {
    echo 'Successfully sent message ID ..... '. $msgID .' .... To: '. $recipient['email'] . PHP_EOL;
  }
}

```

---

If you want to be sure that display name in headers is per RFC5322, use the method `formatDisplayName`. This will avoid some undesired behaviour.

(Here I am using an example of `text/plain` mail):
```php
require 'universalphpmailer.class.php';

$mailor = new universalPHPmailer;

$mailor->toName    = $mailor->formatDisplayName('Mao "Chairman" 毛泽东');
$mailor->toEmail   = 'mao@backintime.sample';
$mailor->subject   = '請問';
$mailor->fromName  = $mailor->formatDisplayName('James J. Jones');
$mailor->fromEmail = 'james.jones@udwiwobg';
$mailor->hostName  = 'host.name.sample';

$mailor->textPlain = 'Hi 泽东,

Is there life in the afterworld?

Best regards,
J.J.J.';

$msgID = $mailor->sendMessage();

if (!empty($msgID)) {
  echo 'Successfully sent message ID ..... '. $msgID;
}

```

---

#Acknowledgements

Peter Kahl had written much of the SMTP-related methods of this package as a result of inspiration from the following class and extends his thanks to the authors thereof:

> PHPMailer RFC821 SMTP email transport class.
>
> Implements RFC 821 SMTP commands and provides some utility methods for sending mail to an SMTP server.
>
> @package PHPMailer
>
> @author Chris Ryan
>
> @author Marcus Bointon <phpmailer@synchromedia.co.uk>
>
> <https://github.com/PHPMailer/PHPMailer/blob/master/class.smtp.php>
