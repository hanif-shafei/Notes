### **LazyGate Trait**  

The `LazyGate` trait allows you to programmatically log in a user by their ID, bypassing traditional authentication. This is useful for testing or controlled scenarios.  

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