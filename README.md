# Design Patterns with PHP
###Adapter Pattern
**Intención**: Convertir la interfaz de una clase en otra interfaz que el cliente espera.
El adapter permite que clases trabajen juntas, que de otra manera no podrían por las interfaces incompatibles.

**UML**
![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/35/ClassAdapter.png/300px-ClassAdapter.png)

**Example**

```php

<?php 
// Concrete Implementation of PayPal Class
class PayPal {
     
    public function __construct() {
        // Your Code here //
    }
     
    public function sendPayment($amount) {
        // Paying via Paypal //
        echo "Paying via PayPal: ". $amount;
    }
}
 
// Simple Interface for each Adapter we create
interface paymentAdapter {
    public function pay($amount);
}
 
class paypalAdapter implements paymentAdapter {
     
    private $paypal;
 
    public function __construct(PayPal $paypal) {
        $this->paypal = $paypal;
    }
     
    public function pay($amount) {
        $this->paypal->sendPayment($amount);
    }
}

// Concrete Implementation of MoneyBooker Class
class MoneyBooker {
 
    public function __construct() {
        // Your Code here //
    }
 
    public function doPayment($amount) {
        // Paying via MoneyBooker //
        echo "Paying via MoneyBooker: ".  $amount;
    }
}
 
// MoneyBooker Adapter
class moneybookerAdapter implements paymentAdapter {
 
    private $moneybooker;
 
    public function __construct(MoneyBooker $moneybooker) {
        $this->moneybooker = $moneybooker;
    }
 
    public function pay($amount) {
        $this->moneybooker->doPayment($amount);
    }
}
 
// Client Code
$moneybooker = new moneybookerAdapter(new MoneyBooker());
$moneybooker->pay('2629');

```
