# Installation

Cryptography suite can be used as follows:

1. Generic.
2. Selective.

{% hint style="info" %}
The suite is used as a header-only and is currently statically linked. Future versions will allow dynamic linking.
{% endhint %}

## **Generic**

Generic usage of cryptography suite consists of all modules available at [GitHub =nil; Crypto3 Team Repositories](https://github.com/orgs/NilFoundation/teams/nil-crypto3/repositories).

The generic module can be added to your c++ project as follows

```shell
git submodule add https://github.com/NilFoundation/crypto3.git <dir>
```

## **Selective**

Developer can select to include a one or more modules to reduce the sources of resulting project and dependencies tree height. This however does require the developer to manually resolve all required dependencies and stay upto date regarding compatibility across modules.

Selective modules can be added to your project as follows:

```shell
git submodule add https://github.com/NilFoundation/crypto3-<lib>.git <dir>shell
```
