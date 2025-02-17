# How it works

Before you start writing a single line of code, it’s a good idea to get an overview of how Forest Admin works. The magic lies in its architecture.

Forest Admin provides you with:

* An API hosted on your server to retrieve your data. We call it the **Admin Backend**
  * if you chose a database as a datasource (PostgreSQLL, MySQL / MariaDB, MSSQL, MongoDB), your Admin Backend will be generated as a **standalone folder**.
  * if you chose an existing app as a datasource (Rails, Django, Laravel, Express/Sequelize, Express/Mongoose), your Admin Backend will be generated **within your app**.
* A user interface to access and manage your data from your browser. This **Forest Admin User Interface** is built and managed through resources hosted on Forest Admin's servers.

{% tabs %}
{% tab title="SQL/Mongodb" %}
![The Admin Backend is a Node.JS REST API hosted on your servers](../../.gitbook/assets/how\_it\_works\_2.jpg)
{% endtab %}

{% tab title="Rails/Django/Laravel" %}
![The Admin Backend is a Rails Engine mounted on your application](../../.gitbook/assets/how\_it\_works\_3.jpg)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For a more in-depth explanation of Forest Admin's architecture (the Node.JS agent version), please read the [following article](https://medium.com/forest-admin/a-deep-dive-into-forest-admins-architecture-and-its-benefits-for-the-developers-who-trust-it-1d49212fb4b).
{% endhint %}

## The Admin Backend

The Admin Backend is generated upon install and **hosted on your end**.

It includes an API allowing to **translate calls made from the Forest Admin UI into queries** to your database (covering actions such as CRUD, search & filters, pagination, sorting, etc.).

It also provides the Forest Admin servers with the information needed to build the User Interface (the **Forest Admin Schema**). This information includes table names, column names and types, and relationships. It is sent when you run your Admin Backend [within a file called `forestadmin-schema.json`](../models/#the-forestadmin-schema-json-file).

## Data Privacy

When logging into the **Forest Admin UI** in your browser, you will connect to:

1. The **Forest Admin servers** to retrieve the **Forest Admin UI.**
2. The **Admin Backend** to retrieve your **data** and populate the Forest Admin UI with it.

{% hint style="warning" %}
As your data transits directly from the Admin Backend hosted on your end and the user browser, **it never transits through our servers**.
{% endhint %}

{% tabs %}
{% tab title="SQL/Mongodb" %}
![](../../.gitbook/assets/how\_it\_works\_4.jpg)
{% endtab %}

{% tab title="Rails/Django/Laravel" %}
![](../../.gitbook/assets/how\_it\_works\_3.jpg)
{% endtab %}
{% endtabs %}

## Security

The connection to both servers to the **Admin Backend** and the **Forest Admin Servers** are protected using 2 different [**JWT**](https://jwt.io/) signed by 2 different keys:

1. `FOREST_ENV_SECRET` to authenticate all requests made to the **Forest Admin Servers**
2. `FOREST_AUTH_SECRET` to authenticate all requests made to the **Admin Backend**

{% tabs %}
{% tab title="SQL/Mongodb" %}
![](../../.gitbook/assets/how\_it\_works\_5.jpg)
{% endtab %}

{% tab title="Rails/Django/Laravel" %}
![](../../.gitbook/assets/how\_it\_works\_6.jpg)
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
`FOREST_ENV_SECRET` is provided by Forest Admin and ensures your **Admin Backend** interacts with the relevant environment configuration on our end\*\*.\*\*

`FOREST_AUTH_SECRET` is chosen freely by you and is never disclosed to anyone\*\*.\*\*
{% endhint %}

{% hint style="info" %}
The JWT Data Token contains all the details of the requesting user. On any authenticated request to your Admin Backend, you can access them with the variable `req.user`.

{% code title="req.user content example" %}
```javascript
{
  "id": "172",
  "email": "angelicabengtsson@doha2019.com",
  "firstName": "Angelica",
  "lastName": "Bengtsson",
  "team": "Pole Vault",
  "role": "Manager",
  "tags": [{ key: "country", value: "Canada" }],
  "renderingId": "4998",
  "iat": 1569913709,
  "exp": 1571123309
}
```
{% endcode %}
{% endhint %}

### **No 3rd-party Tracking**

The **Forest Admin UI** has an option to completely disable any 3rd-party provider that could track data available from your browser to guarantee the respect of data privacy.

{% tabs %}
{% tab title="SQL/Mongodb" %}
![](../../.gitbook/assets/how\_it\_works\_7.jpg)
{% endtab %}

{% tab title="Rails/Django/Laravel" %}
![](../../.gitbook/assets/how\_it\_works\_8.jpg)
{% endtab %}
{% endtabs %}

### IP Whitelisting

The [IP whitelisting](../../how-tos/setup/forest-admin-ip-white-listing-forest-cloud.md) feature allows you to create a list of trusted IP addresses or IP ranges from which your admin users can both access to the **Forest Admin UI** and interact with your **Admin Backend**.

{% tabs %}
{% tab title="SQL/Mongodb" %}
![](../../.gitbook/assets/how\_it\_works\_9.jpg)
{% endtab %}

{% tab title="Rails/Django/Laravel" %}
![](../../.gitbook/assets/how\_it\_works\_10.jpg)
{% endtab %}
{% endtabs %}

### **DMZ & VPN**

You're free to host your **Admin Backend** in the cloud architecture you want to be compliant with your security infrastructure (DMZ, VPN, etc.).﻿

![](../../.gitbook/assets/how\_it\_works\_11.jpg)

## Credentials

We’re already working with companies compliant with the following Industry Standard Certifications.

![](<../../.gitbook/assets/image (338).png>)
