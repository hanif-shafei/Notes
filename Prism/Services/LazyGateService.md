# LazyGateService

The `LazyGateService` is a Symfony service designed to programmatically log in a user without requiring their direct interaction with the login form. This service is typically used in scenarios where you want to authenticate a user based on some external condition or parameter (e.g., URL query parameters).

# Usage

The `LazyGateService` provides a single method called `lazyGatekeeper`. This method accepts a `Request` object, a user entity, and other optional parameters like the gatekeeper value and firewall name.

### Method Signature

```php
public function lazyGatekeeper(
    Request $request,
    AppUser $user,
    string $gatekeeper = 'lazygate',
    string $firewallName = 'dev'
): bool;

```

### Parameters:

- `Request $request`: The current HTTP request object. It is used to read query parameters and manage the session.
- `AppUser $user`: The user entity you want to log in programmatically. This user must be an instance of `AppUser` or any class that implements `UserInterface`.
- `string $gatekeeper` (optional): A custom gatekeeper parameter used to verify that the login request is authorized. The default is `'lazygate'`.
- `string $firewallName` (optional): The firewall context name. This determines the security context under which the user will be authenticated. The default is `'dev'`.

### Return Value:

The method returns a `bool` value:
- `true`: If the login was successful.
- `false`: If the login was unsuccessful (e.g., if the gatekeeper parameter is missing or invalid).

# Example of Use in a Controller

You can use `LazyGateService` in a controller to log in a user programmatically. Below is an example showing how to do this.

### **Step 1: Inject the Service**  

`LazyGateService` may be used by injecting it as a dependency in a controller's `__construct()` method.
Don't foreget to import it at the top: `use App\Service\LazyGateService`.

```php
namespace App\Controller;

use App\Service\LazyGateService;

class SecurityController extends AbstractController
{
    public function __construct( private LazyGateService $lazyGateService ) {}

```

### **Step 2: Authenticate a User**

Use the `lazyGatekeeper` method to programmatically log in a user.  

```php
namespace App\Controller;

use App\Entity\AppUser;
use App\Service\LazyGateService;
use Doctrine\ORM\EntityManagerInterface;

class SecurityController extends AbstractController
{
    public function __construct(
        private LazyGateService $lazyGateService,
        private EntityManagerInterface $entityManager,
    ) {}

    #[Route('/test-login', name: 'test_login')]
    public function testLogin(Request $request): Response
    {
        if ($this->getUser() instanceof UserInterface) {
            return $this->redirectToRoute('synergy');
        }

        $user = $this->entityManager->getRepository(AppUser::class)->find(1234);

        if ($this->lazyGateService->lazyGatekeeper($request, $user)) {
            // Login successful, redirect as needed
            return $this->redirectToRoute('dashboard');
        }

        // ...

```

### **Step 3: Pass the Gatekeeper Check**

Make a request to the route with the correct `gatekeeper` parameter.  

**Example Request**  

```plaintext
GET /test-login?gatekeeper=testgate
```

# **Customizing LazyGate**

Customizing the behavior of LazyGateService is straightforward. You can adjust key settings like the gatekeeper value, firewall context, and session handling to fit your application’s needs. 

## **Change the Gatekeeper Value**  

Modify the expected `gatekeeper` value for additional security or use case flexibility.  

```php
$this->lazyGatekeeper($request, $user, expectedGate: 'customgate');
```

## **Change the Firewall Context**  

Switch the firewall context for authentication.  

```php
$this->lazyGatekeeper($request, $user, firewallName: 'secure_area');
```

## **Changing Request Session to Session Interface**

When you switch from using the Request session to SessionInterface, you're shifting to a more explicit and flexible way of handling sessions. The SessionInterface allows for easier mocking and testing, and it's a recommended approach when you want more control over session management, especially in non-request contexts (e.g., background jobs or CLI commands).

### 1. Modify `LazyGateService` to use `SessionInterface`

- **Comment the Request Session Code**
    
    In the current `LazyGateService`, the request session is used for setting the token. Comment the whole block:
    
    ```php
       // // UnComment to use request session
       // $session = $request->getSession();
       // $session->set('_security_main', serialize($token));
    ```

- **Uncomment the SessionInterface Code**

    Next, you’ll need to use `SessionInterface` for managing the session. The code for this is already prepared, but commented out.
    - Uncomment `SessionInterface` imports:
    
    ```php
       use Symfony\Component\HttpFoundation\Session\SessionInterface;

    ```
    
    - Uncomment constructor injection for `SessionInterface`:
    
    ```php
        public function __construct(
            private TokenStorageInterface $tokenStorage,
            private SessionInterface $session           // uncomment this line
        ) {}

    ```
    
    - Uncomment the session setting block:
    
    ```php
       // UnComment to use SessionInterface, need to manually inject session service in services.yaml
       $this->session->set('_security_' . $firewallName, serialize($token));

    ```

        **Why Change?**: By switching to `SessionInterface`, you decouple the session handling from the `Request` object, making it more flexible and maintainable. You no longer rely on the request’s session state, but rather use the session as an explicit service injected into your class. This change also allows easier testing and works seamlessly in scenarios where direct access to the request object might not be available (such as in some command-line tasks or background processes).

### 2. Modify `services.yaml` for Dependency Injection

To inject the `SessionInterface` into `LazyGateService`, you need to update the `services.yaml` configuration. Locate your service definition in `config/services.yaml` (or wherever your services are defined) and make the following changes:

- **Uncomment the `session` Injection Block**

    In `services.yaml`, uncomment the section that injects the `session` service into `LazyGateService`:
    
    ```yaml
    # UnComment to inject session service into LazyGateService
    App\Service\LazyGateService:
        arguments:
            $tokenStorage: '@security.token_storage'
            $session: '@session'
    ```
    
    This tells Symfony to pass the `session` service (which implements `SessionInterface`) to `LazyGateService`.

- **Ensure the Correct Service Alias**
    
    Ensure that the `@session` alias points to the correct service. In Symfony, `@session` is a predefined alias for the session service that implements `SessionInterface`. It should work out of the box in most Symfony configurations.

# Troubleshooting

1. **Missing Gatekeeper Parameter**: Ensure that the `gatekeeper` parameter is included in the request query string (e.g., `/login?gatekeeper=lazygate`).
2. **Invalid Gatekeeper Value**: If the gatekeeper value is incorrect, an exception will be thrown. Make sure the `gatekeeper` value matches the one specified in the method call.
3. **User Not Found**: Ensure that the user passed into the method exists and is a valid `AppUser` object.
4. **Session Management**: If you encounter issues with session management, ensure that the Symfony session is properly configured and available.

# Conclusion

The `LazyGateService` offers a simple and effective way to log users in programmatically in Symfony. It is especially useful for scenarios where automatic login is required based on conditions or external parameters. By integrating this service into your controller, you can easily authenticate users without needing a traditional login form.
