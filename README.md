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
- [Resources](#resources)
- [API Feature Split](#API-Feature-Split)
- [Implementation Details](#Implementation)

## <a name="docs">Documentation </a>
The API was documented using SwaggerHub and visualised with Swagger UI. <br>
Below are the links to check the Swagger UI Doc and YAML: 
- [Swagger UI Doc View](https://app.swaggerhub.com/apis-docs/vespers-apis/product-management-api/1.0.0/)
- [SwaggerHub Code Editor View](https://app.swaggerhub.com/apis/vespers-apis/product-management-api/1.0.0)

## <a name="key-points"> Key Features </a>
- There are a total of 87 CRUD operations in the API to provide functionality for many common ecommerce features.
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

## <a name="resources"> Resources </a>
<b> List of Resources: </b>
- [Users](#users)
- [Merchants](#merchants)
- [Stores](#stores)
- [Categories](#categories) *** Very Important ***
- [Products](#products)
- [Inventory](#inventory)
- [Catalogs](#catalogs)
- [Orders](#orders)
- [Shipping](#shipping)
- [Brands](#brands)

Please refer the Swagger doc for a more detailed look at the resources schema.

### <a name="users"> Users </a>
__Description:__ This is the ```Users``` resource where User information is stored. <br>
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
__Description:__ 
- This is the ```Categories```resource where category information is stored. This is one of the most important resources because of how the category hierarchy is implemented. 
- This API supports an ```N-level``` hierarchy of categories which are stored in a Tree like data structure where a category keeps track of its parent category and child categories. <br>
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
- Another very important feature here is how we generalise certain attributes among products
<br>
__Example Value:__ <br>

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

### <a name="inventory"> Inventory </a>

### <a name="catalogs"> Catalogs </a>

### <a name="orders"> Orders </a>

### <a name="shipping"> Shipping </a>

### <a name="brands"> Brands </a>
