# Vaibhav's Market - A product management system
This is a product managment system API which gives the complete functionality of an ecommerce website. This is a REST API that 
follows OAS 3.0 (OpenAPI Specification). <br>
Documentation: 
- [Swagger UI Doc View](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0/)
- [SwaggerHub Code Editor View](https://app.swaggerhub.com/apis/vespers-apis/product-management-api/1.0.0)

## Table of Contents
- [Documentation](#docs)
- [Implementation Details](#Implementation)
- [Key Features](#key-points)
- [Authentication](#auth)
- [Resource Features](#resources)
- [API Feature Split](#API-Feature-Split)


## <a name="docs">Documentation </a>
The API was documented using SwaggerHub and visualised with Swagger UI. <br>
Below are the links to check the Swagger UI Doc and YAML: 
- [Swagger UI Doc View](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0/)
- [SwaggerHub Code Editor View](https://app.swaggerhub.com/apis/vespers-apis/product-management-api/1.0.0)

## <a name="Implementation"> Implementation Details </a>
__Tools Used:__
- Express
- MongoDB Atlas
- JWT (for authentication using Bearer Token method) 
<br>
Every unimplemented method and endpoint currently returns a ```501``` status code. 
<br>
To demonstrate the following features, ```user-auth api``` and ```users``` api have been completely implemented. <br>
You can try them out [here.](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0#/users.

- __Rate Limiting:__
    - The API has rate limiting implemented. Users who are not logged in can only make 10 requests every minute.
    - Logged in users on the other hand can make a 1000 requests every minute.
    
- __Pagination:__
    - The API also shows pagination for a lot of the ```GET``` methods.
    - The following are the parameters used to control pagination in theses methods,
        - ```page_size``` - ```type: number```, used to decide number of results per page(default is 5).
        - ```page_no``` - ```type: number```, used to decide page number(gets first page by default).
        
- __Authentication:__
    - This is implemented using the Bearer Token method specifically with JWT.

## <a name="key-points"> Key Features </a>
- The API has been split into __20 sub categories__ to make the API more modular and group functionality better.
- There are a total of __87 CRUD operations__ in the API to provide functionality for many common ecommerce features.
- SKUs follow the following format: ```BRAND_LABEL-PRODUCT_CODE-PRODUCT_OPTION_VALUES```. Ex: ```BOSE-QC35-RED```
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
This API uses __Bearer Authentication__ also known as __token authentication__ and in particular we are using ```JWT(JSON Web Token)``` to implement this. <br>
The point of this is to give access to the user bearing the token. The big benefit of this method is that nothing has to be stored on the server side and the same authentication can be reused on a different website that has the same user database. <br>
Below is the form in which the Authorization header takes,<br>
```Authorization: Bearer {token}```
<br>
In our API we receive this token on successfully creating an account and then trying to login with valid credentials.<br>
Additionally the API allows us to get a refresh token by making a ```GET``` request to the following end point ```/api/auth/getRefreshToken```. (Note: Tokens expire after 30 minutes which is why we have a refresh token feature) <br>
### Permissions
- There are 3 types of users in this API: ```admin```, ```user(consumer)``` and ```merchant```.
- ```admin``` has access to every single CRUD operation in the API.
- ```user``` has access to almost all User exclusive endpoints particularly involving user management.
- ```merchant``` has access to almost all Merchant endpoints and ```Store``` endpoints.

## <a name="resources"> Resource Features </a>
<b> List of Resources: </b>
- [Users](#users)
- [Merchants](#merchants)
- [Stores](#stores)
- [Categories](#categories) *** Very Important(Please Read)***
- [Products](#products)
- [Inventory](#inventory)
- [Catalogs](#catalogs)
- [Orders](#orders)
- [Shipping](#shipping)
- [Brands](#brands)

Please refer the [Swagger doc](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0) for a more detailed look at the resources schema.

### <a name="users"> Users </a>
__Description:__ This is the ```Users``` resource where User information is stored. <br>
__Features:__
- This is the consumer level account.
- 
__Example Value:__ <br>
```
{
    "username": "ron35",
    "firstName": "Robert",
    "lastName": "Kennedy",
    "email": "email234@email.com",
    "country": "USA",
    "shippingAddress": {
      "address": "72 Cardinal St. Mechanicsburg",
      "state": "Pennsylvania",
      "country": "USA",
      "pin_code": 17050
    }
  }
```

### <a name="merchants"> Merchants </a>

__Description:__ This is the ```Merchants``` resource where Merchant information is stored. <br>
__Example Value:__ <br>
```
{
  "merchant_name": "Avinash Kishore",
  "merchant_username": "merch1avinash",
  "merchant_email": "merchavinash123@merchant.com",
  "details": "string",
  "password": "string",
  "stores": [
    {
      "store_id": "123",
      "store_name": "Store 1"
    },
    {
      "store_id": "1234",
      "store_name": "Store 2"
    }
  ]
}
```

### <a name="stores"> Stores </a>
__Description:__ This is the ```Stores```resource where store information is stored. These stores are created by Merchant accounts and we can see above how the IDs of these stores are stored in the ```merchants``` resource.<br>
__Example Value:__ <br>
```
{
  "store_id": "exampleshop123",
  "store_name": "Example Store 1",
  "details": {
    "description": "This is an example description for Example Store 1.",
    "phone_no": 1234567899,
    "email": "examplemail@store.com"
  },
  "address_info": {
    "address": "72 Cardinal St. Mechanicsburg",
    "state": "Pennsylvania",
    "country": "USA",
    "pin_code": 17050
  }
}
```

### <a name="categories"> Categories </a>
__Description:__  This is the ```Categories```resource where category information is stored. This is one of the most important resources because of how the category hierarchy is implemented. 
#### Features
- __Category Hierarchy:__ This API supports an ```N-level``` hierarchy of categories which are stored in a Tree like data structure where a category keeps track of its parent category and child categories. <br>
 <b> Example: </b>
 ```
  *Books
    - History
      * Roman History
      * Indian History
        - Mughal History
          * ...(and so on)
        - British History
    - Literature
    - Romance
 ```
- __Attribute Generalisation:__ Another very important feature here is how we generalise certain attributes among products of the same category. This is normally a big problem as different products across the website are of different kinds and cannot have similar attributes which makes generalisation harder. <br>
For example, a Novel and a Chair clearly have different attributes as one is a Book and the other is Furniture. <br>
__Solution:__ We define category specific attributes for each category we define. Every category that's lower on the category hierarchy can then inherit these attributes.

__Example Value:__  <br>

```
{
  "parent_category_id": null,
  "category_hierarchy_level": 0,
  "category_name": "Books",
  "category_id": "book",
  "sku_label": "book",
  "category_specific_attributes": {
    "author": "string",
    "no_of_pages": "number",
    "format": "string",
    "language": "string"
  },
  "description": "This category is for books.",
  "sub_category_ids": [
    [
      "history_book",
      "adventure_book",
      "scifi_book"
    ]
  ]
}
```


### <a name="products"> Products </a>
__Description:__ This is the ```Products```resource where product information is stored. This is the one of the most important resources as the API is for product management.<br>
__Features:__
- Product specific attributes/options can be created and are different from category specific attributes. Variants can only be created from product specific attributes.
- After a variant is created it is automatically added into the product's inventory with a unique SKU.
- The only products that are actually sold are the ones created through the endpoint used to create variants. If the base product has no variants then it should still be put through the variant creation endpoint albeit with a blank array for option values.
- The base product can be considered as something like a template containing information for the variants.

__Note:__ Go through the [Swagger Doc](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0#/product%20option%2Fattribute%20management) to get a better understanding of how product info is stored as this is a complicated resource due to the product options and variants. <br>
__Example Values:__ <br>
__base product__ <br>
```
{
  "product_name": "Redmi Note Pro",
  "product_id": "redminotepro",
  "sku": "RED-redminotepro",
  "tags": [
    "phone",
    "redmi",
    "pro",
    "smart"
  ],
  "category_id": "smartphone",
  "details": {
    "description": "This is the latest smartphone from Redmi.",
    "price": 14999
  }
}
```

__product options__ <br>
```
[
  {
    "option_name": "colour",
    "option_id": 1,
    "description": "This option allows you to pick the colour of the product.",
    "option_values": [
      {
        "is_default": "true",
        "option_value_name": "blue",
        "sku_label": "BLU",
        "value_id": "1",
        "additional_details": {
          "description": "This is the blue colour option."
        }
      },
      {
        "is_default": "false",
        "option_value_name": "red",
        "sku_label": "RED",
        "value_id": "2",
        "additional_details": {
          "description": "This is the red colour option."
        }
      }
    ]
  }
]
```

To see more possibilities please check the [Swagger Doc](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0#/product%20option%2Fattribute%20management).

### <a name="inventory"> Inventory </a>
__Description:__ This is the ```Inventory```resource where product inventory info is stored. This resources is nested within the product resource.
__Features:__
- Stores each variant as a separate inventory unit each identified by a unique SKU. 
- As mentioned above this API follows an SKU template of the following type - ```(Brand Label)-(Product ID)-[Product Option Values]```.
- Each inventory unit shows how much stock each store selling a product has.
- We can also filter to find stock in specific stores.
__Example Value:__ <br>
```
[
  {
    "sku": "PUMA-SHIRT-GREEN-SMALL",
    "store_inventory": [
      [
        {
          "store_id": "mystore123",
          "stock": 8
        },
        {
          "store_id": "mystore12",
          "stock": 14
        },
        {
          "store_id": "mystore12345",
          "stock": 0
        }
      ]
    ]
  }
]
```

### <a name="catalogs"> Catalogs </a>
__Description:__ This is the ```Catalog```resource where catalog info is stored. This resources lets us group products of various types together. A catalog is basically a grouping of various types of products.
__Features:__
- There are 2 types of catalogs that can be created, Global ones for the entire website and local ones which are created by a store for the products they sell.
- Catalogs add another way of fitering products and also give another way of arranging products.
__Example Value:__ <br>
```
{
  "catalog_id": "fashion",
  "catalog_name": "Fashion Catalogue",
  "details": {},
  "product_list": [
    "denim_shirt",
    "nike_boots"
  ]
}
```

### <a name="orders"> Orders </a>
__Description:__ This is the ```Orders```resource where order info is stored. This resource is nested in the user resource and shows us a users orders. This resource is closely connected to the shipping resource.
__Features:__
- When checkout process is complete in the cart an order is created and the cart is emptied. Along with the order a shipment item is also created in the shipping collection and both of these store each others' IDs.
- We can see information about our order such as payment method and status of order(fulfilled or not).
- We can also cancel an order which then leads to the shipment to delivery address being cancelled.
__Example Value:__ <br>
```
{
  "username": "user123",
  "order_id": 1,
  "shipment_id": 32,
  "order_items": [
    {
      "product_id": "bata_shoes",
      "product_variant_id": "string",
      "quantity": 0
    }
  ],
  "status": "In Transit",
  "payment_method": "COD",
  "bill_paid": false
}
```


### <a name="shipping"> Shipping </a>
__Description:__ This is the ```Shipping```resource where shipment info is stored. This resource is closely connected to the orders resource. This resource is mostly controlled by the shipping company.
__Features:__
- The main feature of this resource is live tracking of order.
- This live tracking can be implemented by the developer by using the API of whichever company is responsible for shipping to find current location.
__Example Value:__ <br>
```
{
  "shipment_id": 32,
  "order_id": 1,
  "free_shipping": true,
  "delivery_address": {
    "address": "72 Cardinal St. Mechanicsburg",
    "state": "Pennsylvania",
    "country": "USA",
    "pin_code": 17050
  },
  "shipping_company": "Blue Dart",
  "shipping_status": "in transit",
  "current_location": {
    "address": "72 Cardinal St. Mechanicsburg",
    "state": "Pennsylvania",
    "country": "USA",
    "pin_code": 17050
  }
}
```

### <a name="brands"> Brands </a>
__Description:__ This is the ```Brands```resource where brand info is stored.
__Features:__
- The main feature of this resource is create Brands.
- This resource is also important because the brand label is the first part of the SKU of a product.
__Example Value:__ <br>
```
{
  "brand_id": "bose",
  "brand_name": "Bose",
  "sku_label": "BOSE",
  "details": {
    "description": "This is a brand of the Bose company.",
    "company": "Bose"
  }
}
```
## <a name="API-Feature-Split"> API Feature Split </a>
__Please refer the [Swagger Doc](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0) for a more in depth view of all the endpoints available. <br>
In this section we are only going to discuss some of the features and endpoints in some of the API sub categories that haven't beem discussed yet and deserve some attention.__

### Cart Api
__Features:__
- Users can have multiple carts.
- If an item is added to cart but cart does not exist yet(in case of new user) a new cart is automatically created. Otherwise the item is automatically added to first cart.
- Users can also specify which cart to do CRUD operations with.
- On checkout(defined under ```orders api```) the cart is emptied and an order and shipment object is created.

### Store Management API
| Method | Endpoint                                      | Description                 | Query Parameter |
|--------|-----------------------------------------------|-----------------------------|-----------------|
| PUT    | /api/store/{storeId}/addProduct/{productId}   | Adds a product to the store | variant_sku     |
| DELETE | /api/store{storeId}/deleteProduct/{productId} | Deletes product from store  | variant_sku     |

- Here we can see that when adding a product to the store we have to mention which variant of the product it is in the query parameter.
- Similarly when deleting a product from the store we have to mention which variant we are deleting.
- In both cases if there is no variant of that product and only base product exists then we can leave the parameter empty.

### Product Category Management
| Method | Endpoint                                      | Description                 |
|--------|-----------------------------------------------|-----------------------------|
| PUT    | /api/categories/{categoryId}/move/{parentId}  | Make a category a subcategory of another category. |
- This lets us make one category a subcategory of another.
- It also appropriately updates the hierarchy level of all child categories under it.

### User Authentication
| Method | Endpoint                                      | Description                 | 
|--------|-----------------------------------------------|-----------------------------|
| POST   | /api/users/login                              | User login. Returns an auth token.| 
| POST   | /api/merchants/login                          | Merchant login. Returns an auth token|
| GET    | /api/auth/getRefreshToken                     | Returns a refresh token.    |
- These are the 3 endpoints we have for user authentication.
- The first two give a token and the third gives a refresh token.

### Product Variant Api
__Features__:
- This lets us create variants of products.
- Product variants can only be created after creation of product options through the ```product attributes/options``` api.
- After creation of variant an inventory entry is automatically made with a unique SKU to identify the variant and its inventory details.

__Note:__ Please check the remaining features and end points in the [Swagger Doc](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0#/) as every end point has been properly documented in it.
