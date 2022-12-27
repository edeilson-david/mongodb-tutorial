# Relationship

Below are some examples of data modeling, as there is more than one way to solve 
a problem with data modeling. Each case must be studied.

## OneToOne relationship
Example 1 - Two documents: Student and Student card
```json
{
  "_id": 1,
  "doc_number": "123456798",
  "name": "Stephan",
  "city": "San Francisco",
  "student_card_id": 8
}
```

```json
{
  "_id": 8,
  "class": "220A",
  "number": 1245
}
```

Example 2 - One document with embedded document
```json
{
    "_id": 1,
    "doc_number": "123456798",
    "name": "Stephan",
    "city": "San Francisco",
    "student_card": {
      "class": "220A",
      "number": 1245
    }
}
```

## OneToMany relationship
Example 1 - Many documents: One departament and many employees

```json
{
    "_id": 3,
    "target": 4000,
    "departament": "Sales"
}

{
  "_id": 8,
  "salary": 1400,
  "name": "John",
  "role": "Seller",
  "departament_id":  3}

{
  "_id": 12,
  "salario": 1600,
  "name": "Jessica",
  "role": "Seller",
  "departament_id":  3}
```

Example 2 - One document with embedded documents
```json
{
  "_id": 3,
  "target": 4000,
  "departament": "Sales",
  "employees": [
    {
      "_id": 8,
      "salary": 1400,
      "name": "John",
      "role": "Seller",
      "departament_id": 3
    },
    {
      "_id": 12,
      "salary": 1600,
      "name": "Jessica",
      "role": "Seller",
      "departament_id": 3
    }
  ]
}
```

## ManyToMany relationship
Example 1 - Many providers and many products

```json
-- Providers
{
  "_id": 1,
  "provider": "Shoes blue",
  "city": "San Francisco"
}

{
  "_id": 2,
  "provider": "Sho shoes",
  "city": "Palo Alto"
}

-- Products
{
  "_id": 536,
  "product": "Nikiis Men Casual",
  "color": "Brown"
}

{
  "_id": 478,
  "provider": "Adidiis Women Running",
  "color": "Black"
}

{
  "_id": 289,
  "provider": "New Balanciis",
  "color": "White"
}

-- ProviderProduct
{
  "_id": 50,
  "provider_id": 1, 
  "product_id": 536,
  "value": 
}
{
  "_id": 51,
  "provider_id": 2, 
  "product_id": 536 
}
{...}
```

Example 2 - Many providers and many products

```json
-- Providers
{
  "_id": 1,
  "provider": 'Shoes blue",
  "city": "San Francisco",
  "products": [
    { "_id": 536, "price": 15.20 }, 
    { "_id": 289, "price": 16.43 }]
}

{
  "_id": 2,
  "provider": 'Sho shoes",
  "city": "Palo Alto",
  "products": [
    { "_id": 478, "price": 17.25 }, 
    { "_id": 289, "price": 14.75 }]
}

-- Products
{
  "_id": 536,
  "product": 'Nikiis Men Casual",
  "color": "Brown",
  "providers": [{ "_id": 1 }]
}

{
  "_id": 478,
  "provider": 'Adidiis Women Running",
  "color": "Black",
  "providers": [{ "_id": 2 }]
}

{
  "_id": 289,
  "provider": 'New Balanciis",
  "color": "White",
   "providers": [{ "_id": 1 }, { "_id": 2 }]
}

```
