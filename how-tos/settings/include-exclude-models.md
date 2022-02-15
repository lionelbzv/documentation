# Include/exclude models

By default, all models declared in your app are analysed by the Forest Liana in order to display them as collections in your admin panel.

You can exclude some of them from the analysis to never send their metadata to Forest Admin. By doing this, these models will therefore never be available in your admin panel.

To do so, add the following code to **either** define which models are included **or** excluded.&#x20;

{% tabs %}
{% tab title="SQL" %}
### Include models

{% code title="middlewares/forestadmin.js" %}
```javascript
...

app.use(require('forest-express-sequelize').init({
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  objectMapping,
  connections,
  includedModels: ['customers']
}));

...
```
{% endcode %}

### Exclude models

{% code title="middlewares/forestadmin.js" %}
```javascript
...

app.use(require('forest-express-sequelize').init({
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  objectMapping,
  connections,
  excludedModels: ['documents', 'transactions']
}));

...
```
{% endcode %}
{% endtab %}

{% tab title="Mongodb" %}
### Include models

{% code title="middlewares/forestadmin.js" %}
```javascript
...

app.use(require('forest-express-mongoose').init({
  modelsDir: __dirname + '/models',
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  sequelize: require('./models').sequelize,
  includedModels: ['customers']
}));

...
```
{% endcode %}

### Exclude models

{% code title="middlewares/forestadmin.js" %}
```javascript
...

app.use(require('forest-express-mongoose').init({
  modelsDir: __dirname + '/models',
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  sequelize: require('./models').sequelize,
  excludedModels: ['documents', 'transactions']
}));

...
```
{% endcode %}
{% endtab %}

{% tab title="Rails" %}
{% code title="/config/initializers/forest_liana.rb" %}
```ruby
ForestLiana.env_secret = Rails.application.secrets.forest_env_secret
ForestLiana.auth_secret = Rails.application.secrets.forest_auth_secret

# ...

# in the [] you may add the precise list of all models you want to see in Forest
ForestLiana.included_models = ['Customer'];

# or second possibility below :

# in the [] you may add the precise list of all models you do not want to see in Forest
ForestLiana.excluded_models = ['Document', 'Transaction'];
```
{% endcode %}
{% endtab %}
{% endtabs %}