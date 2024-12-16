### **LazyGate Trait**  

The `LazyGate` trait allows you to programmatically authenticate a user by their ID based on a "gatekeeper" query parameter in the request, bypassing traditional authentication. This is useful for testing or controlled scenarios.

---

### **Using LazyGate**

#### **Step 1: Add the Trait**  
Include the `LazyGate` trait in the controller where you want to use it.  

```php
use App\Controller\Traits\LazyGate;

class SecurityController extends AbstractController
{
    use LazyGate;
}
```

---

#### **Step 2: Authenticate a User**

Use the `lazyGatekeeper` method to programmatically log in a user.  

```php
#[Route('/test-login', name: 'test_login')]
public function testLogin(Request $request): Response
{
    $user = $this->getDoctrine()->getRepository(AppUser::class)->find(37667);

    if ($this->lazyGatekeeper($request, $user, expectedGate: 'testgate')) {
        // Login successful, redirect as needed
        return $this->redirectToRoute('dashboard');
    }

    // Handle failed authentication
    return $this->redirectToRoute('error_page');
}
```

---

#### **Step 3: Pass the Gatekeeper Check**

Make a request to the route with the correct `gatekeeper` parameter.  

**Example Request**  

```plaintext
GET /test-login?gatekeeper=testgate
```

---

### **Customizing LazyGate**

#### **Change the Gatekeeper Value**  

Modify the expected `gatekeeper` value for additional security or use case flexibility.  

```php
$this->lazyGatekeeper($request, $user, expectedGate: 'customgate');
```

---

#### **Change the Firewall Context**  

Switch the firewall context for authentication.  

```php
$this->lazyGatekeeper($request, $user, firewallName: 'secure_area');
```

---

### **Notes**  

- This trait is ideal for **testing purposes** or restricted environments.  
- Always ensure it’s used in **controlled contexts**. Disable it in production environments if not necessary.  

**Happy coding!**






Sending Notifications
Using the Notifiable Trait
Notifications may be sent in two ways: using the notify method of the Notifiable trait or using the Notification facade. The Notifiable trait is included on your application's App\Models\User model by default:

<?php
 
namespace App\Models;
 
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
 
class User extends Authenticatable
{
    use Notifiable;
}

The notify method that is provided by this trait expects to receive a notification instance:

use App\Notifications\InvoicePaid;
 
$user->notify(new InvoicePaid($invoice));

Remember, you may use the Notifiable trait on any of your models. You are not limited to only including it on your User model.

Using the Notification Facade
Alternatively, you may send notifications via the Notification facade. This approach is useful when you need to send a notification to multiple notifiable