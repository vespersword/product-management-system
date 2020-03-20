# Vaibhav's Market - A product management system
This is a product managment system API which gives the complete functionality of an ecommerce website. This is a REST API that 
follows OAS 3.0 (OpenAPI Specification). <br>
Documentation: 
- [Swagger UI Doc View](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0/)
- [SwaggerHub Code Editor View](https://app.swaggerhub.com/apis/vespers-apis/product-management-api/1.0.0)

## Table of Contents
- [Documentation](#docs)
- [Key Features](#key-points)
- [Authentication](#auth)
- [Collections](#Collections)
- [API Feature Split](#API-Feature-Split)
- [Implementation Details](#Implementation)

## <a name="docs">Documentation </a>
The API was documented using SwaggerHub and visualised with Swagger UI. <br>
Below are the links to check the Swagger UI Doc and YAML: 
- [Swagger UI Doc View](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0/)
- [SwaggerHub Code Editor View](https://app.swaggerhub.com/apis/vespers-apis/product-management-api/1.0.0)

## <a name="key-points"> Key Features </a>
- There are a total of 87 CRUD operations in the API to provide functionality for many common ecommerce features.
- An ```N-level``` hierarchichal grouping of products based on ```category```. Categories are stored in a tree like structure.
- ```Brand Management``` for products.
- Separate account types for normal ```Users(consumers)``` and ```Merchants```.
-  ```Store Management``` system which allows every Merchant to create and manage stores.
- Numerous ways to filter products.
- ```Product Attribute/Option``` management which lets us create ```variants```.
- ```Inventory Management``` to handle inventory of each product. Uses SKUs(Stock Keeping Units) as unique identifiers for each variant of a product.
- ```Catalog Management``` to create product catalogs(global catalogs and catalogs local to store).
- ```Shopping Cart``` to let Users purchase products. Users can have multiple carts.
- ```Orders and Shipping``` to let users keep track of their order during delivery.
- ```Payment``` system.
- ```Product Review``` system which lets users review products.
- ```Search``` functionality which takes a query and searches through every collection for a match.
- ```Product recommendation``` which recommends products similar to what you are buying.
- ```Customer Service``` feature which lets a ```User``` communicate with the ```Merchant``` who owns the store selling an item they are purchasing.

## <a name="auth"> Authorization </a>
This API uses __Bearer Authentication__ also known as __token authentication__ and in particular uses ```JWT(JSON Web Token)```.
