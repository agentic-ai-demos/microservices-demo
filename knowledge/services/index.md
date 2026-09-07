# Services

* [Frontend Service](frontend.md) - The Go frontend serves the web UI and translates browser workflows into backend gRPC calls.
* [Checkout Service](checkoutservice.md) - The Go checkout service orchestrates cart, catalog, currency, payment, shipping, and email calls into an order.
* [Cart Service](cartservice.md) - The C# cart service stores per-user cart items behind the CartService gRPC API.
* [Product Catalog Service](productcatalogservice.md) - The Go product catalog service exposes product listing, lookup, and search over the repository catalog data.
* [Currency Service](currencyservice.md) - The Node.js currency service exposes supported currencies and currency conversion over gRPC.
* [Payment Service](paymentservice.md) - The Node.js payment service mock-validates cards and returns transaction IDs.
* [Shipping Service](shippingservice.md) - The Go shipping service estimates shipping cost and produces mock shipment tracking IDs.
* [Email Service](emailservice.md) - The Python email service implements mock order confirmation delivery.
* [Recommendation Service](recommendationservice.md) - The Python recommendation service returns product IDs related to the current cart context.
* [Ad Service](adservice.md) - The Java ad service returns contextual text ads based on page context keys.
* [Load Generator](loadgenerator.md) - The Locust load generator drives realistic browser-like traffic against the frontend.
* [Shopping Assistant Service](shoppingassistantservice.md) - The Python shopping assistant accepts image input and calls a Gemini-backed recommendation path.
