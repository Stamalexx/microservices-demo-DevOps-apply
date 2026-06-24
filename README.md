# Online Boutique (GitOps CI/CD Thesis Fork)

**Author:** Alexios Stamelos

> **ACADEMIC PROJECT NOTICE:** This repository is a forked version of the official [Google Cloud Platform microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo). It serves specifically as the target application source code for the MSc Thesis: **Experimental Study of Kubernetes and GitOps CI/CD Architectures**.

## Related Repositories
To understand the full scope of this project, please refer to the following repositories:
* [**Academic Thesis & Empirical Evaluation**](https://github.com/Stamalexx/MSc-Thesis-GitOps-Microservices.git)
* [**GitOps Configuration & Jenkins Pipeline**](https://github.com/Stamalexx/microservices-gitops-config.git)
* [**E2E Playwright Testing Suite**](https://github.com/Stamalexx/microservices-playwright-testing.git)

## Role in the CI/CD Pipeline
This repository acts as the primary codebase for the developers. In the context of this thesis, any code merged into the main branch of this repository triggers the automated Jenkins integration pipeline. 

Instead of pushing directly to production, the continuous integration server clones this repository, utilizes a dynamic `git diff` mechanism to compile Docker images only for the modified microservices, and provisions an ephemeral testing environment. Only if the "Shift-Left" functional tests pass will the resulting artifacts be synchronized to the live Kubernetes cluster via Argo CD.

---

## Original Application Architecture

**Online Boutique** is a cloud-first microservices demo application consisting of an e-commerce web app where users can browse items, add them to the cart, and purchase them. It is composed of 11 microservices written in different languages that communicate over gRPC.

| Service                                              | Language      | Description                                                                                                                       |
| ---------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **frontend** | Go            | Exposes an HTTP server to serve the website. Does not require signup/login and generates session IDs for all users automatically. |
| **cartservice** | C#            | Stores the items in the user's shopping cart in Redis and retrieves it.                                                           |
| **productcatalogservice** | Go            | Provides the list of products from a JSON file and ability to search products and get individual products.                        |
| **currencyservice** | Node.js       | Converts one money amount to another currency. Uses real values fetched from European Central Bank. It's the highest QPS service. |
| **paymentservice** | Node.js       | Charges the given credit card info (mock) with the given amount and returns a transaction ID.                                     |
| **shippingservice** | Go            | Gives shipping cost estimates based on the shopping cart. Ships items to the given address (mock)                                 |
| **emailservice** | Python        | Sends users an order confirmation email (mock).                                                                                   |
| **checkoutservice** | Go            | Retrieves user cart, prepares order and orchestrates the payment, shipping and the email notification.                            |
| **recommendationservice** | Python        | Recommends other products based on what's given in the cart.                                                                      |
| **adservice** | Java          | Provides text ads based on given context words.                                                                                   |
| **loadgenerator** | Python/Locust | Continuously sends requests imitating realistic user shopping flows to the frontend.                                              |

*(For the original Google repository and deployment tutorials, please visit the [upstream repository](https://github.com/GoogleCloudPlatform/microservices-demo)).*
