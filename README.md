<h1 align="center">
  React hooks for Commerce.js
</h1>
<p align="center">
  A library of React hooks that can be used to integrate Commerce.js and interface with the Chec Dashboard.
</p>

<p align="center">
  <a href="https://npmjs.org/package/@chec/react-commercejs-hooks">
    <img src="https://img.shields.io/npm/v/@chec/react-commercejs-hooks.svg" alt="Version" />
  </a>
  <a href="https://npmjs.org/package/@chec/react-commercejs-hooks">
    <img src="https://img.shields.io/npm/dw/@chec/react-commercejs-hooks.svg" alt="Downloads/week" />
  </a>
    <a href="https://github.com/chec/commerce.js/blob/master/packages/react-commercejs-hooks/package.json">
    <img src="https://img.shields.io/npm/l/@chec/react-commercejs-hooks" alt="License" />
  </a>
  <br>
  <a href="https://commercejs.com">commercejs.com</a> | <a href="https://twitter.com/commercejs">@commercejs</a> | <a href="http://slack.commercejs.com">Slack</a>
</p>

## Installation

Use your favourite package manager:

```shell
yarn add @chec/react-commercejs-hooks
# OR
npm install --save @chec/react-commercejs-hooks
```

## Usage

Access to the Commerce.js SDK is provided with a context provider. This is used by the various hooks in this library to
access data from the Chec API. Ensure that you wrap any component that uses a Commerce.js hook in the CommerceContext
component:

```js
import { CommerceContext } from '@chec/react-commercejs-hooks';

export default function MyComponent() {
  return (
    // Provide CommerceContext with your public key
    <CommerceContext publicKey="pk_123abc">
      <ProductList />
    </CommerceContext>
  );
}
```

The arguments for the Commerce.js sdk are supported as props on the `CommerceContext` component:

| Prop | Purpose |
| ---- | ------- |
| publicKey | Your Chec public key, the first argument when using Commerce.js |
| debug | A boolean indicating whether the Commerce.js SDK should emit debug information to the console. The second argument of Commerce.js |
| options | An object with various options. This is the third argument of the Commerce.js SDK |

You can use the Commerce.js SDK in any component that is a child of the `CommerceContext` component by using the 
`useCommerce` hook:

```js
import { useEffect } from 'react';
import { useCommerce } from '@chec/react-commercejs-hooks';

export default function ProductList() {
  const { commerce } = useCommerce();
  
  useEffect(() => {
    if (!commerce) {
      return;
    }
    
    commerce.products.list().then(products => {
      // ...
    })
  }, [commerce]);
}
```

### Checkout hooks

This library provides many hooks for use with the checkout, but in order to use them, you must put components within a
checkout context by using the `CheckoutProvider` component. In the following example, a checkout is created from a 
Commerce.js cart ID, passed in as a prop:

```js
import { CheckoutProvider } from '@chec/react-commercejs-hooks/checkout';

export default function Checkout({ cartId }) {
  return (
    <CheckoutProvider id={cardId}>
      <LineItemSummary />
      <CustomerFields />
      <PaymentSummary />
      <PaymentForm />
    </CheckoutProvider>
  );
}
```

The following hooks are available for use within a checkout, and are all exported from 
`@chec/react-commercejs-hooks/checkout`:

| Hook | Purpose |
| ---- | ------- |
| `useCheckout()` | Provides the checkout (token) object that was fetched by Commerce.js (when available). The object structure of this is available [here](https://commercejs.com/docs/api/?shell#checkout) |
| `useAllCountries()` | Provide all countries that Commerce.js supports. See [list all countries](https://commercejs.com/docs/sdk/checkout#list-all-countries) in the docs. |
| `useConditionals()` | Provides the "conditionals" attribute from the checkout, indicating what conditional flags apply for the current checkout |
| `useLineItems()` | Provides the line items in the checkout |
| `useShippingCountries()` | Provides the countries that are eligible for shipping based on the product selection in the checkout |
| `useShippingOptions(country, region)` | Provides the shipping options that can be selected for the given country (and region if provided) based on the product selection in the checkout |
| `useShippingSubdivisions(countryCode)` | Provides subdivisions of the given country code |
| `useShippingSummary()` | Provides detail of the chosen shipping method in the checkout |
| `useTotals()` | Provides the total cost of the checkout, and various subtotals within the checkout |
| `useCapture()` | Provides a callback that can be used to capture the checkout, with the given detail if provided |
| `useCaptureWithStripe()` | Extends the functionality of `useCapture` to provide integrated Stripe support, assuming that this hook is used within Elements context (from `react-stripe-js`) |
| `useSetProductVariant()` | Provides a callback that can be used to set a variant on a product within the checkout |
| `useSetShippingOption()` | Provides a callback that can be used to set the chosen shipping option for the checkout |

TODO: Add more detailed docs for each hook

### Cart hooks

TBA
