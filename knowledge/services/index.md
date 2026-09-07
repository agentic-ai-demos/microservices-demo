# Services

* [Ad Service](adservice.md) - Java service returning contextual ads for a set of category keywords.
* [Cart Service](cartservice.md) - C# service holding per-session carts behind a pluggable store: Redis, AlloyDB or Spanner.
* [Checkout Service](checkoutservice.md) - Go service that orchestrates placing an order across cart, catalog, currency, shipping, payment and email.
* [Currency Service](currencyservice.md) - Node service converting money between currencies from a static ECB rate table.
* [Email Service](emailservice.md) - Python service that renders an order confirmation. In the default deployment it only logs it.
* [Frontend](frontend.md) - Go HTTP server that renders the storefront and fans every page out to the backing gRPC services.
* [Load Generator](loadgenerator.md) - Python Locust client that drives synthetic shopper traffic against the frontend.
* [Payment Service](paymentservice.md) - Node service that validates a card and returns a simulated transaction id. It charges nothing.
* [Product Catalog Service](productcatalogservice.md) - Go service serving the product list, single products and search from a JSON catalog or a database.
* [Recommendation Service](recommendationservice.md) - Python service returning related product ids, chosen at random from the catalog.
* [Shipping Service](shippingservice.md) - Go service returning shipping quotes and issuing tracking ids. Both values are simulated.
* [Shopping Assistant Service](shoppingassistantservice.md) - Python Flask service answering shopping questions with an LLM, reached over HTTP rather than gRPC.
