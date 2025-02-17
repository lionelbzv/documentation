---
description: Let's get you up and running on Forest Admin in minutes!
---

# Quick start

### Step 1: Create an account and follow the onboarding

Go to [https://app.forestadmin.com/signup](https://app.forestadmin.com/signup), create an account and install your project.

{% hint style="info" %}
For the purpose of this tutorial, we have used [this database](../how-tos/databases/use-a-demo-database.md). Feel free to use it if you don't have one.
{% endhint %}

At the end of your onboarding, you will **out-of-the-box** be able to:

* Access all your data **(1)**
* Export your data **(2)**
* Add a record **(3)**
* View and edit a record **(4)**
* Edit your UI **(5)**
* Search and filter **(6)**

![](<../.gitbook/assets/image (547).png>)

However, your business logic likely requires more features. What if you need to...

* refund an order
* upload new documents, accept or reject them, or ask customers to update their documents,
* contact a customer or ask a team member to perform an action,
* and much more?

It's possible with **smart actions** :point\_down:

### Step 2: Create a Smart Action

Let's say you want to let your customer support team to easily refund orders, you can quickly create a smart action.

{% tabs %}
{% tab title="SQL" %}
Declare it in your `/forest/orders.js` file:

{% code title="/forest/orders.js" %}
```javascript
const { collection } = require('forest-express-sequelize');

collection('orders', {
  actions: [{ 
    name: 'Refund order'
  }],
});
```
{% endcode %}

Then implement it according to your business logic:

```javascript
...

router.post('/actions/refund-order', permissionMiddlewareCreator.smartAction(), (req, res) => {
  // Add your own logic, like calling a Stripe API for instance
  res.send({ success: 'Order refunded!' });
});

...

module.exports = router;
```
{% endtab %}

{% tab title="MongoDB" %}
Declare it in your `/forest/orders.js` file:

{% code title="/forest/orders.js" %}
```javascript
const { collection } = require('forest-express-mongoose');

collection('orders', {
  actions: [{ 
    name: 'Refund order'
  }],
});
```
{% endcode %}

Then implement it according to your business logic:

```javascript
...

router.post('/actions/refund-order', permissionMiddlewareCreator.smartAction(), (req, res) => {
  // Add your own logic, like calling a Stripe API for instance
  res.send({ success: 'Order refunded!' });
});

...

module.exports = router;
```
{% endtab %}

{% tab title="Rails" %}
Declare it in your `/lib/forest_liana/collections/order.rb` file:

{% code title="/lib/forest_liana/collections/order.rb" %}
```ruby
class Forest::Order
  include ForestLiana::Collection

  collection :Order
  
  action 'Refund order'
end
```
{% endcode %}

Then declare the corresponding route:

{% code title="/app/controllers/forest/orders_controller.rb" %}
```ruby
Rails.application.routes.draw do
  # MUST be declared before the mount ForestLiana::Engine.
  namespace :forest do
    post '/actions/refund-order' => 'orders#refund_order'
  end
  
  mount ForestLiana::Engine => '/forest'
end
```
{% endcode %}

Lastly, implement the action according to your business logic:

```ruby
class Forest::OrdersController < ForestLiana::SmartActionsController
  def refund_order
    # Add your own logic, like calling a Stripe API for instance
    render json: { success: 'Order refunded!' }
  end
end
```

Note that Forest Admin takes care of the authentication thanks to the `ForestLiana::SmartActionsController` parent class controller.

{% hint style="info" %}
You may have to [add CORS headers](../how-tos/setup/configuring-cors-headers.md) to enable the domain `app.forestadmin.com` to trigger API call on your Application URL, which is on a different domain name (e.g. _localhost:8000_).
{% endhint %}
{% endtab %}

{% tab title="Django" %}
Declare it in your `app/forest/orders.py` file:

{% code title="app/forest/orders.py" %}
```python
from django_forest.utils.collection import Collection
from app.models import Order

class OrderForest(Collection):
    def load(self):
        self.actions = [{
            'name': 'Refund order'
        }]

Collection.register(OrderForest, Order)
```
{% endcode %}

Ensure the file app/forest/\_\_init\_\_.py exists and contains the import of the previous defined class :

{% code title="app/forest/__init__.py" %}
```python
from app.forest.orders import OrderForest
```
{% endcode %}

Make sure your **project** `urls.py` file include you app urls with the `forest` prefix.

{% code title="urls.py" %}
```javascript
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('forest', include('app.urls')),
    path('forest', include('django_forest.urls')),
    path('admin/', admin.site.urls),
]
```
{% endcode %}

Then declare the corresponding route:

{% code title="app/urls.py" %}
```javascript
from django.urls import path
from django.views.decorators.csrf import csrf_exempt

from . import views

app_name = 'app'
urlpatterns = [
    path('/actions/refund-order', csrf_exempt(views.RefundOrderView.as_view()), name='refund-order'),
]
```
{% endcode %}

Lastly, implement the action according to your business logic:

{% code title="app/views.py" %}
```python
from django.http import JsonResponse
from django_forest.utils.views.action import ActionView


class RefundOrderView(ActionView):

    def post(self, request, *args, **kwargs):
        # Add your own logic, like calling a Stripe API for instance

        return JsonResponse({'success': 'Order refunded!'})
```
{% endcode %}

Note that Forest Admin takes care of the authentication thanks to the `ActionView` parent class view.

{% hint style="info" %}
You may have to [add CORS headers](../how-tos/setup/configuring-cors-headers.md) to enable the domain `app.forestadmin.com` to trigger API call on your Application URL, which is on a different domain name (e.g. _localhost:8000_).
{% endhint %}
{% endtab %}

{% tab title="Laravel" %}
Declare it in your `app/Models/Order.php` file:

{% code title="app/Models/Order.php" %}
```php
/**
 * @return SmartAction
 */
public function refundOrder(): SmartAction
{
    return $this->smartAction('single', 'refund order');
}
```
{% endcode %}

Then declare the corresponding route:

{% code title="routes/web.php" %}
```php
Route::post('forest/smart-actions/order_refund-order', [OrdersController::class, 'refundOrder']);
```
{% endcode %}

Lastly, implement the action according to your business logic:

{% code title="app/Http/Controllers/OrdersController.php" %}
```php
<?php

namespace App\Http\Controllers;

use ForestAdmin\LaravelForestAdmin\Http\Controllers\ForestController;
use Illuminate\Http\JsonResponse;

/**
 * Class OrdersController
 */
class OrdersController extends ForestController
{
    /**
     * @return JsonResponse
     */
    public function refundOrder(): JsonResponse
    {
        return response()->json(
            ['success' => 'Order refunded!']
        );
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Congrats! Now it's possible to refund an order!
{% endhint %}

![](<../.gitbook/assets/image (531).png>)

### Step 3: Deploy to Production

Now that you have a fully functional admin panel, the next logical step is to **make it live**, with your live (production) data. Click on **Deploy to Production** and follow the flow.

![](<../.gitbook/assets/image (487).png>)

{% hint style="success" %}
That's it! You are now fully operational on Forest Admin.
{% endhint %}

Next, we recommend reading about our recommended [development workflow](development-workflow.md).
