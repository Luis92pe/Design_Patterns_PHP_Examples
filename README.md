# Design Patterns with PHP

**Patterns**

[TOCM]

### Adapter Pattern
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



### Bridge Pattern

es un patrón que se caracteriza por crear dos jerarquías diferentes tipos y un puente entre ellos que los una.

**Intención**: Desacoplar una abstracción de su implementación para que los dos puedan variar independientemente.


**UML**:


**Example**:
``` php
<?php

namespace RefactoringGuru\Bridge\Structural;

/**
 * The Abstraction defines the interface for the "control" part of the two class
 * hierarchies. It maintains a reference to an object of the Implementation
 * hierarchy and delegates all of the real work to this object.
 */
class Abstraction
{
    /**
     * @var Implementation
     */
    protected $implementation;

    public function __construct(Implementation $implementation)
    {
        $this->implementation = $implementation;
    }


    public function operation()
    {
        return "Abstraction: Base operation with:\n".
            $this->implementation->operationImplementation();
    }
}

/**
 * You can extend the Abstraction without changing the Implementation classes.
 */
class ExtendedAbstraction extends Abstraction
{
    public function operation()
    {
        return "ExtendedAbstraction: Extended operation with:\n".
            $this->implementation->operationImplementation();
    }
}

/**
 * The Implementation defines the interface for all implementation classes. It
 * doesn't have to match the Abstraction's interface. In fact, the two
 * interfaces can be entirely different. Typically the Implementation interface
 * provides only primitive operations, while the Abstraction defines higher-
 * level operations based on those primitives.
 */
interface Implementation
{
    public function operationImplementation();
}

/**
 * Each Concrete Implementation corresponds to a specific platform and
 * implements the Implementation interface using that platform's API.
 */
class ConcreteImplementationA implements Implementation
{
    public function operationImplementation()
    {
        return "ConcreteImplementationA: Here's the result on the platform A.\n";
    }
}

class ConcreteImplementationB implements Implementation
{
    public function operationImplementation()
    {
        return "ConcreteImplementationB: Here's the result on the platform B.\n";
    }
}

/**
 * Except for the initialization phase, where an Abstraction object gets linked
 * with a specific Implementation object, the client code should only depend on
 * the Abstraction class. This way the client code can support any abstraction-
 * implementation combination.
 */
function clientCode(Abstraction $abstraction)
{
    // ...

    print($abstraction->operation());

    // ...
}

/**
 * The client code should be able to work with any pre-configured abstraction-
 * implementation combination.
 */
$implementation = new ConcreteImplementationA();
$abstraction = new Abstraction($implementation);
clientCode($abstraction);

print("\n");

$implementation = new ConcreteImplementationB();
$abstraction = new ExtendedAbstraction($implementation);
clientCode($abstraction);
```

